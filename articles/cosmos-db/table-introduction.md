---
title: Introdução à API de Tabela do Azure Cosmos DB
description: Saiba como você pode usar o Azure Cosmos DB para armazenar e consultar grandes volumes de dados de chave-valor com baixa latência usando a API de Tabela do Azure.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 05/20/2019
ms.author: sngun
ms.openlocfilehash: bd86b230d801f5fff8a9fb0de85f9f3025527382
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65953444"
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Introdução ao Azure Cosmos DB: API de Tabela

O [Azure Cosmos DB](introduction.md) fornece a API de Tabela para aplicativos que são escritos para o Armazenamento de Tabelas do Azure e que precisam de recursos premium como:

* [Distribuição global turnkey](distribute-data-globally.md).
* [Taxa de transferência dedicada](partition-data.md) em todo o mundo.
* Latências de dígito único em milissegundos no percentil 99.
* Alta disponibilidade garantia.
* [Indexação automática secundária](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Esses aplicativos escritos para o armazenamento de Tabelas do Azure podem migrar para o Azure Cosmos DB usando a API de Tabelas, sem alterações de código, e tirar proveito dos recursos premium. A API de Tabela tem SDKs de cliente disponíveis para .NET, Java, Python e Node.js.

> [!IMPORTANT]
> O SDK [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) do .NET Framework está no modo de manutenção e será preterido em breve. Atualize para [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) da nova biblioteca do .NET Standard para continuar a obter os recursos mais recentes com suporte na API de Tabela.

## <a name="table-offerings"></a>Ofertas de Tabela
Caso utilize o Armazenamento de Tabelas do Azure neste momento, você receberá os seguintes benefícios ao mudar para a API de Tabela do Azure Cosmos DB:

| | Armazenamento da tabela do Azure | API de Tabela do Azure Cosmos DB |
| --- | --- | --- |
| Latency | Rápido, mas não há limites superiores de latência. | Latência de milissegundo de dígito único para leituras e gravações, com suporte de leituras de latência de <10 ms e gravações de latência de <15 ms no 99º percentil, em qualquer escala, em qualquer lugar do mundo. |
| Produtividade | Modelo de taxa de transferência variável. As tabelas têm um limite de escalabilidade de 20.000 operações/s. | Altamente escalonável com [taxa de transferência reservada dedicada por tabela](request-units.md), que é respaldada por SLAs. As contas não têm nenhum limite superior na taxa de transferência e dão suporte para >10 milhões de operações/s por tabela. |
| Distribuição global | Região única com uma região secundária legível opcional para alta disponibilidade. Você não pode iniciar o failover. | [Distribuição global turnkey](distribute-data-globally.md) de 1 a 30 ou mais regiões. Suporte para [failovers automáticos e manuais](high-availability.md) a qualquer momento, em qualquer lugar no mundo. |
| Indexação | Somente índice primário em PartitionKey e RowKey. Nenhum índice secundário. | Indexação automática e completa em todas as propriedades, sem gerenciamento de índice. |
| Consultar | A execução de consulta usa o índice para chave primária. Caso contrário, realiza a verificação. | As consultas podem aproveitar a indexação automática em propriedades para tempos rápidos de consulta. |
| Consistência | Forte na região primária. Eventual na região secundária. | [Cinco níveis de consistência bem definidos](consistency-levels.md) para compensar a disponibilidade, latência, taxa de transferência e consistência com base nas necessidades do seu aplicativo. |
| Preços | Otimização de armazenamento. | Otimização de taxa de transferência. |
| SLAs | disponibilidade de 99,99%. | 99,99% para todas as contas de região única e todas as contas de várias regiões com consistência amena e 99,999% de disponibilidade de leitura em todos os [SLAs abrangentes líderes do setor](https://azure.microsoft.com/support/legal/sla/cosmos-db/) de contas de banco de dados de várias regiões em disponibilidade geral. |

## <a name="get-started"></a>Introdução

Crie uma conta do Azure Cosmos DB no [portal do Azure](https://portal.azure.com). Em seguida, veja uma introdução ao nosso [Início rápido para a API de Tabela usando o .NET](create-table-dotnet.md). 

> [!IMPORTANT]
> Se você criou uma conta da API de Tabela durante a versão prévia, crie uma [nova conta da API de Tabela](create-table-dotnet.md#create-a-database-account) para trabalhar com os SDKs da API de Tabela disponíveis para o público em geral.
>

## <a name="next-steps"></a>Próximas etapas

Aqui estão algumas dicas para começar:
* [Compilar um aplicativo .NET usando a API de Tabela](create-table-dotnet.md)
* [Desenvolver com a API de Tabela no .NET](tutorial-develop-table-dotnet.md)
* [Consultar dados de tabela usando a API de Tabela](tutorial-query-table.md)
* [Saiba como configurar a distribuição global do Azure Cosmos DB usando a API de Tabela](tutorial-global-distribution-table.md)
* [API .NET de Tabela do Azure Cosmos DB](table-sdk-dotnet.md)
* [API Java de Tabela do Azure Cosmos DB](table-sdk-java.md)
* [API do Node.js de Tabela do Azure Cosmos DB](table-sdk-nodejs.md)
* [SDK de Tabela do Azure Cosmos DB para Python](table-sdk-python.md)

