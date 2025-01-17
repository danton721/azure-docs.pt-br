---
title: Usar a biblioteca .NET do executor em massa para executar operações de importação e atualização em massa no Azure Cosmos DB
description: Importação em massa e atualizar documentos do Azure Cosmos DB usando a biblioteca do .NET de executor em massa.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: a81b22d8ca538c7dc25a9c6631c2b455d5a6c90e
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257214"
---
# <a name="use-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Use a biblioteca .NET bulk executor para executar operações em massa no Azure Cosmos DB

Este tutorial fornece instruções de como usar a biblioteca .NET do executor em massa do Azure Cosmos DB para importar e atualizar documentos no contêiner do Azure Cosmos DB. Para saber mais sobre a biblioteca bulk executor e como ela ajuda a aproveitar armazenamento e taxa de transferência em massa, consulte o artigo [visão geral da biblioteca bulk executor](bulk-executor-overview.md). Neste tutorial, você verá um exemplo de aplicativo .NET que importa em massa documentos gerados aleatoriamente para um contêiner de banco de dados do Azure Cosmos. Após a importação, ele mostra como você pode atualizar em massa os dados importados, especificando os patches como operações a serem executadas em campos de documento específicos. 

Atualmente, a biblioteca do executor em massa é suportada apenas pelas contas da API do Azure Cosmos DB SQL e da API Gremlin. Este artigo descreve como usar a biblioteca .NET do executor em massa com contas da API do SQL. Para saber mais sobre como usar a biblioteca do .NET de executor em massa com a API do Gremlin, consulte [executar operações em massa na API do Gremlin do Azure Cosmos DB](bulk-executor-graph-dotnet.md). 

## <a name="prerequisites"></a>Pré-requisitos

* Se você ainda não tiver o Visual Studio de 2019 instalado, você pode baixar e usar o [Community Edition do Visual Studio 2019](https://www.visualstudio.com/downloads/). Verifique se você habilitou o desenvolvimento do Azure durante a instalação do Visual Studio.

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar. 

* Você pode [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/) sem uma assinatura do Azure, gratuitamente e sem compromisso. Ou você pode usar o [Emulador do Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/local-emulator) com o ponto de extremidade de `https://localhost:8081`. A Chave Primária é fornecida nas [Solicitações de autenticação](local-emulator.md#authenticating-requests).

* Crie uma conta de API SQL do Azure Cosmos DB usando as etapas descritas na seção [criar conta de banco de dados](create-sql-api-dotnet.md#create-account) do artigo de início rápido do .NET. 

## <a name="clone-the-sample-application"></a>Clonar o aplicativo de exemplo

Agora, vamos mudar para o trabalho com código baixando alguns aplicativos .NET de exemplo do GitHub. Esses aplicativos executam operações em massa em dados do Azure Cosmos DB. Para clonar os aplicativos, abra um prompt de comando, navegue até o diretório onde você deseja copiá-los e execute o seguinte comando:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

O repositório clonado contém dois exemplos "BulkImportSample" e "BulkUpdateSample". Você pode abrir qualquer um dos aplicativos de exemplo, atualizar as cadeias de conexão no arquivo App.config com as suas cadeias de conexão da conta do Azure Cosmos DB, compilar a solução e executá-la. 

O aplicativo "BulkImportSample" gera documentos aleatórios e importa-os em massa no Azure Cosmos DB. O aplicativo "BulkUpdateSample" atualiza em massa os documentos importados, especificando os patches como operações a serem executadas em campos de documento específicos. Nas próximas seções, você examinará o código em cada um desses aplicativos de exemplo.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Importar dados em massa no Azure Cosmos DB

1. Navegue até a pasta "BulkImportSample" e abra o arquivo "BulkImportSample.sln".  

2. As cadeias de conexão do Azure Cosmos DB são recuperadas do arquivo App.config, conforme mostrado no código a seguir:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   O importador em massa cria um novo banco de dados e uma coleção com o nome do banco de dados, nome da coleção e os valores de taxa de transferência especificados no arquivo App.config. 

3. Em seguida, o objeto DocumentClient é inicializado com o modo de conexão Direct TCP:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. O objeto BulkExecutor é inicializado com um valor de repetição alta para tempo de espera e solicitações limitadas. E, em seguida, eles são definidos como 0 para passar o controle de congestionamento para BulkExecutor por toda a sua vida útil.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. O aplicativo chama a API BulkImportAsync. A biblioteca .NET fornece duas sobrecargas da API de importação em massa - uma que aceita uma lista de documentos JSON serializados e outra que aceita uma lista de documentos POCO desserializados. Para saber mais sobre as definições de cada um desses métodos sobrecarregados, consulte a [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **O método BulkImportAsync aceita os seguintes parâmetros:**
   
   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |enableUpsert    |   Um sinalizador para permitir upserts de documentos. Se um documento com a ID fornecida já existir, ele será atualizado. Por padrão, está definido como false.      |
   |disableAutomaticIdGeneration    |    Um sinalizador para desabilitar a geração automática de ID. Por padrão, está definido como true.     |
   |maxConcurrencyPerPartitionKeyRange    | O grau máximo de simultaneidade por intervalo de chave de partição, a configuração como null fará com que a biblioteca use um valor padrão de 20. |
   |maxInMemorySortingBatchSize     |  O número máximo de documentos extraídos do enumerador de documentos que é passado para a chamada de API em cada estágio.  Para a fase de classificação de pré-processamento na memória antes da importação em massa, a configuração como null fará com que a biblioteca use o valor padrão de min(documents.count, 1000000).       |
   |cancellationToken    |    O token de cancelamento para sair normalmente da importação em massa.     |

   **Definição de objeto de resposta de importação em massa** O resultado da chamada de API de importação em massa contém os seguintes atributos:

   |**Parâmetro**  |**Descrição**  |
   |---------|---------|
   |NumberOfDocumentsImported (long)   |  O número total de documentos que foram importados com êxito sem os documentos fornecidos para a chamada de API de importação em massa.       |
   |TotalRequestUnitsConsumed (double)   |   O total de unidades de solicitação (RU) consumidas pela chamada de API de importação em massa.      |
   |TotalTimeTaken (TimeSpan)    |   O tempo total gasto pela chamada de API de importação em massa para concluir a execução.      |
   |BadInputDocuments (lista\<objeto >)   |     A lista de documentos com formato inválido que não foram importados com êxito na chamada de API de importação em massa. O usuário deve corrigir os documentos retornados e repetir a importação. Os documentos com formato inválido incluem documentos cujo valor de ID não é uma cadeia de caracteres (null ou qualquer outro datatype é considerado inválido).    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Atualizar dados em massa no Azure Cosmos DB

Você pode atualizar os documentos existentes usando a API BulkUpdateAsync. Neste exemplo, você definirá o campo Nome para um novo valor e removerá o campo Descrição dos documentos existentes. Para o conjunto completo de operações suportadas de atualização de campo, consulte a [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. Navegue até a pasta "BulkUpdateSample" e abra o arquivo "BulkUpdateSample.sln".  

2. Defina os itens de atualização juntamente com as operações de atualização de campo correspondentes. Neste exemplo, você usará SetUpdateOperation para atualizar o campo Nome e UnsetUpdateOperation para remover o campo Descrição de todos os documentos. Você também pode executar outras operações, como incremento de um campo de documento por um valor específico, efetuar push de valores específicos para um campo de matriz ou remover um valor específico de um campo de matriz. Para saber mais sobre os diferentes métodos fornecidos pela API de atualização em massa, consulte a [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. O aplicativo chama a API BulkUpdateAsync. Para saber mais sobre a definição do método BulkUpdateAsync, consulte a [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **O método BulkUpdateAsync aceita os seguintes parâmetros:**

   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   O grau máximo de simultaneidade por intervalo de chave de partição, a configuração como null fará com que a biblioteca use um valor padrão de 20.   |
   |maxInMemorySortingBatchSize    |    O número máximo de itens de atualização extraídos do enumerador de itens de atualização transferidos para a chamada de API em cada estágio da fase de classificação de pré-processamento na memória antes da atualização em massa. A configuração como null fará com que a biblioteca use o valor padrão de min(updateItems.count, 1000000).     |
   | cancellationToken|O token de cancelamento para sair normalmente da atualização em massa. |

   **Definição de objeto de resposta de atualização em massa** O resultado da chamada de API desatualização em massa contém os seguintes atributos:

   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |NumberOfDocumentsUpdated (long)    |   O número total de documentos que foram atualizados com êxito sem os documentos fornecidos para a chamada de API de atualização em massa.      |
   |TotalRequestUnitsConsumed (double)   |    O total de unidades de solicitação (RU) consumidas pela chamada de API de atualização em massa.    |
   |TotalTimeTaken (TimeSpan)   | O tempo total gasto pela chamada de API de atualização em massa para concluir a execução. |
    
## <a name="performance-tips"></a>Dicas de desempenho 

Considere os seguintes pontos para melhor desempenho ao usar a biblioteca bulk executor:

* Para ter o melhor desempenho, execute o aplicativo de uma máquina virtual do Azure que está na mesma região da sua região de gravação da conta do Cosmos DB.  

* É recomendado criar uma instância de um único objeto BulkExecutor para todo o aplicativo dentro de uma única máquina virtual correspondente a um contêiner do Cosmos DB específico.  

* Como uma execução de API de operação em massa única consome uma grande parte de ES de CPU e de rede do computador cliente. Isso acontece por geração de várias tarefas internamente, evite a geração de várias tarefas simultâneas no processo de aplicativo executando chamadas de API de operação em massa. Se uma chamada de API de operação em massa único que está em execução em uma única máquina virtual não consegue consumir a taxa de transferência do contêiner inteira (Se taxa de transferência > 1 da seu contêiner milhão RU/s), é preferível para criar máquinas virtuais separadas a serem executados simultaneamente chamadas de API de operação em massa.  

* Verifique se InitializeAsync() é invocado depois de instanciar um objeto BulkExecutor para buscar o mapa de partições de contêiner do Cosmos DB de destino.  

* No App.Config do aplicativo, certifique-se de **gcServer** esteja habilitado para melhor desempenho
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* A biblioteca emite rastreamentos que podem ser coletados em um arquivo de log ou no console. Para habilitar ambos, adicione o seguinte ao App.Config do seu aplicativo.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
  ```

## <a name="next-steps"></a>Próximas etapas
* Para saber mais sobre os detalhes do pacote Nuget e notas em massa executor da biblioteca do .NET de versão, consulte[em massa de detalhes do SDK de executor](sql-api-sdk-bulk-executor-dot-net.md). 
