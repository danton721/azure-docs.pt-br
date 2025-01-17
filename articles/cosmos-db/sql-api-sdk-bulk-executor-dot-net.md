---
title: 'O Azure Cosmos DB: Recursos, SDK e API do executor em massa do .NET'
description: Saiba tudo sobre a API .NET e o SDK do Bulk Executor, incluindo datas de lançamento, datas de aposentadoria e alterações feitas entre cada versão do SDK do .NET do Azure Cosmos DB Bulk Executor.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: 74eddadd7fd967daa1eebb9d7cb223fdc708025f
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471430"
---
# <a name="net-bulk-executor-library-download-information"></a>Biblioteca do executor em massa do .NET: Fazer o download das informações 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Feed de alterações do .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Provedor de recursos REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Executor em massa - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Executor em massa - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **Descrição**| A biblioteca do Executor em massa permite que os aplicativos clientes executem operações em massa nas contas do Azure Cosmos DB. Biblioteca de Executor em massa fornece namespaces BulkImport, BulkUpdate e BulkDelete. O módulo BulkImport pode importar em massa documentos de forma otimizada, de modo que a taxa de transferência provisionada para uma coleção seja consumida até seu limite máximo. O módulo BulkUpdate pode atualizar em massa dados existentes nos contêineres do Azure Cosmos DB como patches. O módulo BulkDelete pode excluir documentos em massa de forma otimizada, de modo que o rendimento provisionado para uma coleção seja consumido em sua extensão máxima.|
|**Baixe o SDK**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **Biblioteca BulkExecutor no GitHub**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**Documentação da API**|[Documentação de referência de API .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Introdução**|[Introdução ao SDK do .NET da biblioteca do Executor em massa](bulk-executor-dot-net.md)|
| **Framework atualmente com suporte**| Microsoft .NET Framework 4.5.2, 4.6.1 e o .NET Standard 2.0 |

## <a name="release-notes"></a>Notas de versão

### <a name="a-name230-preview2230-preview2"></a><a name="2.3.0-preview2"/>2.3.0-preview2

* Adicionado suporte para o executor do graph em massa aceitar a ttl de vértices e bordas

### <a name="a-name220-preview2220-preview2"></a><a name="2.2.0-preview2"/>2.2.0-preview2

* Corrigido um problema que causou exceções durante o dimensionamento Elástico do Azure Cosmos DB durante a execução no modo de Gateway. Essa correção torna funcionalmente equivalente à 1.4.1 de versão.

### <a name="a-name210-preview2210-preview2"></a><a name="2.1.0-preview2"/>2.1.0-preview2

* BulkDelete adicionado suporte para contas de API do SQL aceitar a chave de partição, as tuplas de id do documento para excluir. Essa alteração torna funcionalmente equivalente à 1.4.0 de versão.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Incluindo MongoBulkExecutor para suporte ao .NET Standard 2.0. Esse recurso torna funcionalmente equivalente à versão 1.3.0, com o acréscimo de suporte a .NET Standard 2.0 como a estrutura de destino.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Adicionado .NET Standard 2.0 como uma das estruturas de destino com suporte para fazer com que a biblioteca do BulkExecutor funcione com aplicativos .NET Core.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Atualizado o Executor em massa para agora usar a versão mais recente do SDK do Azure Cosmos DB .NET (2.4.0)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0

* Adicionado suporte para o executor do graph em massa aceitar a ttl de vértices e bordas

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

* Corrigido um problema que causou exceções durante o dimensionamento Elástico do Azure Cosmos DB durante a execução no modo de Gateway.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

* BulkDelete adicionado suporte para contas de API do SQL aceitar a chave de partição, as tuplas de id do documento para excluir.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Corrigido um problema que causou um problema de formatação no agente de usuário usado pelo BulkExecutor.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Realizou um aprimoramento para importar do BulkExecutor e atualizar as APIs de forma transparente para adaptar a escala elástica de contêiner do Cosmos DB ao armazenamento excede a capacidade atual sem gerar exceções.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* E aumentado a dependência do SDK .NET do DocumentDB para a versão 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Corrigido um problema que fez com que o BulkExecutor gerasse o erro JSRT enquanto a importação para coleções corrigidas.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Adicionado suporte para a operação de BulkDelete para contas de API de SQL do Azure Cosmos DB.
* Adicionado suporte para a operação de BulkImport para API do Azure Cosmos DB para MongoDB.
* E aumentado a dependência do SDK .NET do DocumentDB para a versão 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Adicionado suporte para a operação de BulkImport para contas de API do Gremlin do Azure Cosmos DB.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Pequena correção de bug para a operação BulkImport das contas da API SQL do Azure Cosmos DB.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Adicionado suporte para operações BulkImport e BulkUpdate para contas da API SQL do Azure Cosmos DB.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a biblioteca Java de Executor em massa, consulte o artigo a seguir:

[SDK de biblioteca do Bulk Executor Java e informações da versão](sql-api-sdk-bulk-executor-java.md)
