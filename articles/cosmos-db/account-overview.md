---
title: Trabalhando com contas do Azure Cosmos DB
description: Este artigo descreve como criar e usar contas do banco de dados do Azure Cosmo
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: c0e97eed0a4028fdd9d2254fd9472dc3141d607a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239844"
---
# <a name="work-with-azure-cosmos-account"></a>Como trabalhar com a conta do Azure Cosmos

O Azure Cosmos DB é uma plataforma como serviço (PaaS) totalmente gerenciada. Para começar a usar o Azure Cosmos DB, você deve inicialmente criar uma conta do Azure Cosmos na sua assinatura do Azure. Sua conta do Azure Cosmos contém um nome DNS exclusivo e você pode gerenciar uma conta usando o portal do Microsoft Azure, a CLI do Azure ou usando diferentes SDKs específicos de idioma. Para mais informações, consulte [como gerenciar sua conta do Azure Cosmos](how-to-manage-database-account.md).

A conta do Azure Cosmos é a unidade fundamental de distribuição global e alta disponibilidade. Para distribuir globalmente seus dados e taxa de transferência em várias regiões do Azure, você pode adicionar e remover regiões do Azure à sua conta do Azure Cosmos a qualquer momento. Você pode configurar sua conta do Azure Cosmos para ter uma única ou várias regiões de gravação. Para obter mais informações, consulte [como adicionar e remover regiões do Azure à sua conta do Azure Cosmos](how-to-manage-database-account.md). Você pode configurar o [consistência padrão](consistency-levels.md) nível na conta do Cosmos do Azure. O Azure Cosmos DB fornece SLAs abrangentes, abrangendo throughput, latência no percentil 99, consistência e alta disponibilidade. Para obter mais informações, consulte [Microsoft Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Para gerenciar o acesso a todos os dados dentro de sua conta do Cosmos do Azure, você pode usar o [chaves mestras](secure-access-to-data.md) associada à sua conta. Para proteger ainda mais o acesso aos seus dados, você pode configurar uma [ponto de extremidade de serviço de rede virtual](vnet-service-endpoint.md) e [firewall de IP](firewall-support.md) em sua conta do Cosmos do Azure. 

## <a name="elements-in-an-azure-cosmos-account"></a>Elementos em uma conta do Azure Cosmos

O contêiner do Azure Cosmos DB é a unidade fundamental de escalabilidade. Você pode virtualmente ter uma taxa de transferência provisionada ilimitada (RU/s) e armazenamento em um contêiner. O Azure Cosmos DB particiona de forma transparente seu contêiner usando a chave de partição lógica que você especifica para dimensionar de forma elástica sua taxa de transferência e armazenamento provisionados. Para obter mais informações, consulte [trabalhando com contêineres do Azure Cosmos e itens](databases-containers-items.md).

Atualmente, você pode criar no máximo 100 contas do Azure Cosmos sob uma assinatura do Azure. Uma única conta do Azure Cosmos pode gerenciar virtualmente uma quantidade ilimitada de dados e uma taxa de transferência provisionada. Para gerenciar seus dados e a taxa de transferência provisionada, você pode criar um ou mais bancos de dados do Azure Cosmos em sua conta e, nesse banco de dados, pode criar um ou mais contêineres. A imagem a seguir mostra a hierarquia de elementos em uma conta do Azure Cosmos:

![Hierarquia de uma conta do Azure Cosmos](./media/account-overview/hierarchy.png)

## <a name="next-steps"></a>Próximas etapas

Saiba como gerenciar sua conta do Azure Cosmos e outros conceitos:

* [Como gerenciar sua conta do Azure Cosmos](how-to-manage-database-account.md)
* [Distribuição global](distribute-data-globally.md)
* [Níveis de consistência](consistency-levels.md)
* [Trabalhando com itens e contêineres do Azure Cosmos](databases-containers-items.md)
* [Ponto de extremidade de serviço de rede virtual para sua conta do Azure Cosmos](vnet-service-endpoint.md)
* [Firewall de IP para sua conta do Azure Cosmos](firewall-support.md)
* [Como adicionar e remover regiões do Azure para sua conta do Azure Cosmos](how-to-manage-database-account.md)
* [SLAs do Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
