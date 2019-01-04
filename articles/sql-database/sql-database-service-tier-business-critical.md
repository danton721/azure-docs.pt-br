---
title: Camada Comercialmente Crítico - Serviço do Banco de Dados SQL do Azure | Microsoft Docs
description: Saiba mais sobre a camada Comercialmente Crítico do Banco de Dados SQL do Azure
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: aecbc9415fc7fe93b5df355c97a1a51a74f01e97
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52884418"
---
# <a name="business-critical-tier---azure-sql-database"></a>Camada Comercialmente Crítico - Banco de Dados SQL do Azure

> [!NOTE]
> A Camada Comercialmente Crítico é chamada de Premium no modelo de compra DTU. Para obter uma comparação do modelo de compra baseado no vCore com o modelo de compra baseado em DTU, consulte [Modelos e recursos de compra do Banco de Dados SQL do Azure](sql-database-service-tiers.md).

Banco de dados SQL do Azure baseia-se na arquitetura de mecanismo de banco de dados do SQL Server que é ajustada para o ambiente de nuvem para garantir a disponibilidade de 99,99%, até mesmo no caso de falhas de infraestrutura. Há três modelos de arquitetura que são usados no Banco de Dados SQL do Azure:
- Uso Geral/Padrão 
- Comercialmente Crítico/Premium
- Hiperescala

O modelo da camada de serviço Premium/Comercialmente Crítico que se baseia em um cluster de processos do mecanismo de banco de dados. Esse modelo de arquitetura se baseia no fato de que há sempre um quorum de nós de mecanismo de banco de dados disponíveis e tem impacto mínimo no desempenho em sua carga de trabalho, mesmo durante as atividades de manutenção.

O Azure atualiza e aplica patches subjacentes ao sistema operacional, drivers e Mecanismo de BD do SQL Server de forma transparente com mínimo tempo de inatividade para usuários finais. 

A disponibilidade Premium está habilitada nas camadas de serviço Premium e Business Critical do Banco de Dados SQL do Azure e foi projetada para cargas de trabalho intensivas que não toleram qualquer impacto no desempenho devido às operações de manutenção em andamento.

No modelo Premium, o Banco de Dados SQL do Azure integra computação e armazenamento em um único nó. A alta disponibilidade neste modelo de arquitetura é obtida pela replicação de computação (processo do Mecanismo de Banco de Dados do SQL Server) e armazenamento (SSD conectado localmente) implantada em cluster de quatro nós, usando tecnologia semelhante ao SQL Server [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Cluster de nós de mecanismo de banco de dados](media/sql-database-managed-instance/business-critical-service-tier.png)

O processo do mecanismo de banco de dados SQL e os arquivos mdf / ldf subjacentes são colocados no mesmo nó com armazenamento SSD conectado localmente, fornecendo baixa latência à sua carga de trabalho. A alta disponibilidade é implementada usando tecnologia semelhante ao SQL Server [Always On Availability Groups](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Cada banco de dados é um cluster de nós de banco de dados com um banco de dados primário acessível para carga de trabalho do cliente e três processos secundários que contêm cópias dos dados. O nó primário efetua constantemente push das alterações para nos secundários para garantir que os dados estejam disponíveis em réplicas secundárias se o nó primário falhar por qualquer motivo. O failover é tratado pelo Mecanismo de Banco de Dados do SQL Server – uma réplica secundária se torna o nó primário e uma nova réplica secundária é criada para garantir nós suficientes no cluster. A carga de trabalho é automaticamente redirecionada para o novo nó primário.

Além disso, o cluster Business Critical possui o recurso interno [Expansão Scale-Out](sql-database-read-scale-out.md) que fornece um nó somente leitura interno gratuito que pode ser usado para executar consultas somente leitura (por exemplo, relatórios) não deve afetar o desempenho de sua carga de trabalho principal.

## <a name="when-to-choose-this-service-tier"></a>Quando escolher essa camada de serviço?

A camada de serviço Comercialmente Crítico foi projetada para os aplicativos que requerem respostas de baixa latência do armazenamento SSD subjacente (1 a 2 ms em média), recuperação rápida a infraestrutura subjacente falhar ou precisar carregar relatórios, análise e consultas somente leitura para a réplica secundária do banco de dados primário legível.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre as camadas [Uso Geral](sql-database-service-tier-general-purpose.md) e [Hiperescala](sql-database-service-tier-hyperscale.md).
- Saiba mais sobre o [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para obter mais opções de alta disponibilidade e recuperação de desastres, consulte [Continuidade de Negócios](sql-database-business-continuity.md).