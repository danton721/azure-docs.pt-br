---
title: Visão geral da biblioteca BulkExecutor do Azure Cosmos DB | Microsoft Docs
description: Saiba mais sobre a biblioteca BulkExecutor do Azure Cosmos DB, benefícios de usar a biblioteca e sua arquitetura.
keywords: Java bulk executor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: d395376ad6cf191f8f355f6308f27e525da2911f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-bulkexecutor-library-overview"></a>Visão geral da biblioteca BulkExecutor do Azure Cosmos DB
 
O Azure Cosmos DB é um serviço de banco de dados rápido, flexível e distribuído globalmente que foi projetado para elasticidade de expansão para dar suporte a: 

* Grandes taxas de transferência de leitura e gravação (em milhões de operações por segundo).  
* Armazenamento de grandes volumes de transações e dados operacionais (centenas de terabytes ou mais) com latência previsível de milissegundo.  

A biblioteca BulkExecutor ajuda você a aproveitar essa grande taxa de transferência e o armazenamento, a biblioteca BulkExecutor permite que você execute operações em massa no Azure Cosmos DB por meio de APIs de importação em massa e atualização em massa. Você pode ler mais sobre os recursos da biblioteca BulkExecutor nas seções a seguir. 

> [!NOTE] 
> Atualmente, a biblioteca BulkExecutor oferece suporte a operações de importação e atualização e essa biblioteca tem suporte apenas para contas de API de SQL do Azure Cosmos DB. Consulte as notas das versões [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md) para todas as atualizações para a biblioteca.
 
## <a name="key-features-of-the-bulkexecutor-library"></a>Os principais recursos da biblioteca BulkExecutor  
 
* Reduz significativamente os recursos de computação do lado do cliente necessários para saturar a taxa de transferência alocada para um contêiner. Um aplicativo de thread único que grava dados usando a API de importação em massa atinge 10 vezes maior taxa de transferência de gravação quando comparado a um aplicativo multithread que grava dados em paralelo ao saturar a CPU do computador do cliente.  

* Abstrai tarefas entediantes de escrever uma lógica de aplicativo para lidar com a limitação de solicitação, tempos limite de solicitação e outras exceções transitórias tratando-os com eficiência na biblioteca.  

* Fornece um mecanismo simplificado para aplicativos que executam operações em massa de expansão. Uma única instância de BulkExecutor em execução em uma VM do Azure pode consumir mais que 500 K RU/s e você pode obter uma maior taxa de transferência adicionando instâncias adicionais em VMs do cliente individual.  
 
* Pode importar em massa mais de um terabyte de dados em uma hora usando uma arquitetura de expansão.  

* Pode atualizar em massa dados existentes nos contêineres do Azure Cosmos DB como patches. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Como funciona o BulkExecutor? 

Quando uma operação em massa para importar ou atualizar documentos é disparada com um lote de entidades, eles são inicialmente embaralhados em segmentos correspondentes aos seus intervalos de chaves de partição do Azure Cosmos DB. Dentro de cada partição que corresponde a um intervalo de chave de partição, eles são divididos em minilotes e cada minilote atua como uma carga que é confirmada no lado do servidor. A biblioteca BulkExecutor criou otimizações para execução simultânea desses minilotes dentro e entre os intervalos de chave de partição. A imagem a seguir ilustra como BulkExecutory divide os dados de lotes em diferentes chaves de partição:  

![Arquitetura do BulkExecutor](./media/bulk-executor-overview/bulk-executor-architecture.png)

A biblioteca BulkExecutor certifica-se de utilizar ao máximo a taxa de transferência alocada para uma coleção. Ela usa um  [mecanismo de controle de congestionamento do estilo AIMD](https://tools.ietf.org/html/rfc5681) para cada intervalo de chave de partição do Azure Cosmos DB para tratar com eficiência a limitação e os tempos limite. 

## <a name="next-steps"></a>Próximas etapas 
  
* Saiba mais experimentando os aplicativos de exemplo que consumem a biblioteca BulkExecutor em [.NET](bulk-executor-dot-net.md) e [Java](bulk-executor-java.md).  
* Confira as informações e notas de versões do SDK de BulkExecutor no [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md).
* A biblioteca BulkExecutor é integrada ao conector Cosmos DB Spark. Para obter mais informações, consulte o artigo [conector Spark do Azure Cosmos DB](spark-connector.md).  
* A biblioteca BulkExecutor também é integrada a uma nova versão do [conector do Azure Cosmos DB](https://aka.ms/bulkexecutor-adf-v2) para o Azure Data Factory copiar dados.