---
title: 'Estender sua rede local para o Azure usando uma conexão privada – Visão geral do ExpressRoute: Azure | Microsoft Docs'
description: A visão geral técnica do ExpressRoute explica como uma conexão do ExpressRoute funciona para estender sua rede local para o Azure por meio de uma conexão privada.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: overview
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 9b13c57728c43e02f7ebf9d7730d82d681f5fc1f
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65965870"
---
# <a name="expressroute-overview"></a>Visão geral do ExpressRoute
O ExpressRoute permite que você estenda suas redes locais até a nuvem da Microsoft por meio de conexão privada facilitada por um provedor de conectividade. Com o ExpressRoute, você pode estabelecer conexões com os serviços de nuvem da Microsoft, como o Microsoft Azure, o Office 365 e o Dynamic 365.

A conectividade pode ocorrer de uma rede “qualquer para qualquer” (VPN IP), uma rede Ethernet ponto a ponto ou uma conexão cruzada virtual por meio de um provedor de conectividade em uma colocalização. As conexões de ExpressRoute não passam pela Internet pública. Isso permite que as conexões de ExpressRoute ofereçam mais confiabilidade, mais velocidade, latências menores e muito mais segurança do que as conexões típicas pela Internet. Para saber mais sobre como conectar sua rede à Microsoft usando ExpressRoute, veja [Modelos de conectividade do ExpressRoute](expressroute-connectivity-models.md).

![Visão geral da conexão do ExpressRoute](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Principais benefícios

* Conectividade de Camada 3 entre sua rede local e a Microsoft Cloud por meio de um provedor de conectividade. A conectividade pode ocorrer de uma rede “qualquer para qualquer” (IPVPN), de uma conexão Ethernet ponto a ponto ou por meio de uma conexão cruzada virtual via troca Ethernet.
* Conectividade com os serviços de nuvem da Microsoft em todas as regiões da região geopolítica.
* Conectividade global com os serviços da Microsoft em todas as regiões com o complemento premium do ExpressRoute.
* Roteamento dinâmico entre sua rede e a Microsoft por meio do BGP.
* Redundância interna em cada local de emparelhamento para proporcionar maior confiabilidade.
* [SLA](https://azure.microsoft.com/support/legal/sla/)do tempo de atividade da conexão.
* Suporte a QoS para Skype for Business.

Para saber mais, veja as [perguntas frequentes sobre o ExpressRoute](expressroute-faqs.md).

## <a name="features"></a>Recursos

### <a name="layer-3-connectivity"></a>Conectividade de Camada 3
A Microsoft usa BGP, um protocolo padrão do setor para roteamento dinâmico, a fim de trocar rotas entre sua rede local, suas instâncias no Azure e os endereços públicos da Microsoft. Estabelecemos várias sessões BGP com sua rede para perfis de tráfego diferentes. Encontre mais detalhes no artigo [Circuito e domínios de roteamento do ExpressRoute](expressroute-circuit-peerings.md) .

### <a name="redundancy"></a>Redundância
Cada circuito do ExpressRoute é formado por duas conexões com dois roteadores de borda do Microsoft Enterprise (MSEEs) do provedor de conectividade/borda de sua rede. A Microsoft exige uma conexão BGP dupla do provedor de conectividade/da borda de sua rede, uma para cada MSEE. Você pode optar por não implantar dispositivos redundantes/circuitos de Ethernet em seu lado. No entanto, os provedores de conectividade usam dispositivos redundantes para garantir que as conexões sejam entregues à Microsoft de forma redundante. É obrigatório ter uma configuração de conectividade de Camada 3 redundante para que nosso [SLA](https://azure.microsoft.com/support/legal/sla/) seja válido.

### <a name="connectivity-to-microsoft-cloud-services"></a>Conectividade com serviços de nuvem da Microsoft
As conexões do ExpressRoute permitem o acesso aos seguintes serviços:
* Serviços do Microsoft Azure
* Serviços do Microsoft Office 365
* Microsoft Dynamics 365

> [!NOTE]
> [!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]
> 

Para obter uma lista detalhada dos serviços com suporte no ExpressRoute, visite a página de [Perguntas frequentes sobre o ExpressRoute](expressroute-faqs.md).

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Conectividade com todas as regiões em uma região geopolítica
Você pode se conectar à Microsoft em um dos nossos [locais de emparelhamento](expressroute-locations.md) e acessar as regiões dentro da região geopolítica.

Por exemplo, se você conectou a Microsoft em Amsterdã por meio do ExpressRoute, terá acesso a todos os serviços de nuvem da Microsoft hospedados na Europa Setentrional e Ocidental. Para obter uma visão geral das regiões geopolíticas, das regiões associadas da nuvem da Microsoft e de locais de emparelhamento correspondentes do ExpressRoute, confira o artigo [Parceiros e locais de emparelhamento do ExpressRoute](expressroute-locations.md).

### <a name="global-connectivity-with-expressroute-premium"></a>Conectividade global com o ExpressRoute Premium
Você pode habilitar o [ExpressRoute Premium](expressroute-faqs.md) a fim de estender a conectividade pelas fronteiras geopolíticas. Por exemplo, se você se conectar à Microsoft em Amsterdã por meio do ExpressRoute, terá acesso a todos os serviços de nuvem da Microsoft hospedados em todas as regiões do mundo (nuvens nacionais são excluídas). Você pode acessar serviços implantados na América do Sul ou na Austrália da mesma maneira que acessa as regiões do Norte da Europa e da Europa Ocidental.

### <a name="local-connectivity-with-expressroute-local"></a>Conectividade local com o ExpressRoute Local
Você pode transferir dados de maneira econômica ao habilitar o [SKU Local](expressroute-faqs.md) se for possível colocar seus dados em um local do ExpressRoute próximo à sua região desejada do Azure. Com o Local, a transferência de dados está incluída no encargo de porta do ExpressRoute. 

### <a name="across-on-premises-connectivity-with-expressroute-global-reach"></a>Em conectividade local com Alcance Global do ExpressRoute
Você pode habilitar o Alcance Global do ExpressRoute para trocar dados entre sites locais conectando seus circuitos do ExpressRoute. Por exemplo, se você tiver um data center privado na Califórnia conectado ao ExpressRoute no Vale do Silício e outro data center privado no Texas conectado ao ExpressRoute em Dallas, com o Alcance Global do ExpressRoute, você poderá conectar seus data centers privados em conjunto por meio de dois circuitos do ExpressRoute. O tráfego entre data centers vai percorrer a rede da Microsoft.

Para obter mais informações, veja [Alcance Global do ExpressRoute](expressroute-global-reach.md).
### <a name="rich-connectivity-partner-ecosystem"></a>Ecossistema abundante de parceiros de conectividade
O ExpressRoute tem um ecossistema de provedores de conectividade e parceiros integradores de sistemas em constante crescimento. Para obter as informações mais recentes, consulte [Parceiros do ExpressRoute e locais de emparelhamento](expressroute-locations.md).

### <a name="connectivity-to-national-clouds"></a>Conectividade com nuvens nacionais
A Microsoft opera ambientes de nuvem isolados para regiões geopolíticas e segmentos de clientes especiais. Consulte a página [Parceiros do ExpressRoute e locais de emparelhamento](expressroute-locations.md) para obter uma lista de nuvens e provedores nacionais.

### <a name="expressroute-direct"></a>ExpressRoute Direct
O ExpressRoute Direct fornece aos clientes a oportunidade de conectar-se diretamente à rede global da Microsoft em locais de emparelhamento distribuídos estrategicamente no mundo todo. O ExpressRoute Direct fornece a conectividade dupla de 100 Gbps, que dá suporte à conectividade ativa/ativa em escala.

Os principais recursos que o ExpressRoute Direct fornece incluem, mas não estão limitados a:

* Ingestão de dados em massa em serviços como Armazenamento e o Cosmos DB
* Isolamento físico para setores regulamentados e que requerem conectividade dedicada e isolada, como: bancos, governo e varejo
* Controle granular de distribuição de circuito com base em unidade de negócios

Para obter mais informações, veja [Sobre o ExpressRoute Direct](https://go.microsoft.com/fwlink/?linkid=2022973).

### <a name="bandwidth-options"></a>Opções de largura de banda
É possível comprar circuitos do ExpressRoute para várias larguras de banda. As larguras de banda com suporte estão listadas abaixo. Não deixe de entrar em contato com seu provedor de conectividade para determinar as larguras de banda com suporte.

* 50 Mbps
* 100 Mbps
* 200 Mbps
* 500 Mbps
* 1 Gbps
* 2 Gbps
* 5 Gbps
* 10 Gbps

### <a name="dynamic-scaling-of-bandwidth"></a>Dimensionamento dinâmico de largura de banda
Você pode aumentar a largura de banda do circuito do ExpressRoute (em uma base de melhor esforço) sem precisar subdividir suas conexões. Para obter mais informações, confira [Modificando um circuito do ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md#modify).

### <a name="flexible-billing-models"></a>Modelos flexíveis de cobrança
Escolha o modelo de cobrança que funcione melhor para você. Escolha um dos modelos de cobrança listados abaixo. Para obter mais informações, confira [Perguntas frequentes sobre o ExpressRoute](expressroute-faqs.md).

* **Dados ilimitados**. A cobrança é baseada em um valor mensal; todas as transferências de dados de entrada e saída estão incluídas gratuitamente.
* **Dados limitados**. A cobrança é baseada em um valor mensal; todas as transferências de dados de entrada são gratuitas. A cobrança pelas transferências de dados de saída ocorrem de acordo com a quantidade de GB da transferência de dados. As taxas de transferência de dados variam de acordo com a região.
* **Complemento ExpressRoute premium**. O ExpressRoute premium é um complemento do circuito do ExpressRoute. O complemento premium do ExpressRoute fornece os seguintes recursos: 
  * Aumento dos limites de rota para emparelhamento público e privado do Azure de 4.000 para 10.000 rotas.
  * Conectividade global para serviços. Um circuito do ExpressRoute criado em qualquer região (excluindo nuvens nacionais) terá acesso a recursos em qualquer região do mundo. Por exemplo, uma rede virtual criada na Europa Ocidental pode ser acessada por meio de um circuito do ExpressRoute provisionado no Vale do Silício.
  * Aumento do número de links de Rede Virtual por circuito do ExpressRoute de 10 para um limite maior, dependendo da largura de banda do circuito.

## <a name="faq"></a>Perguntas frequentes
Para ver perguntas frequentes sobre o ExpressRoute, consulte as [Perguntas frequentes sobre o ExpressRoute](expressroute-faqs.md).

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [Modelos de conectividade do ExpressRoute](expressroute-connectivity-models.md).
* Saiba mais sobre conexões e domínios de roteamento do ExpressRoute. Consulte [Circuitos e domínios de roteamento do ExpressRoute](expressroute-circuit-peerings.md).
* Encontrar um provedor de serviços. Consulte [Parceiros e locais de emparelhamento do ExpressRoute](expressroute-locations.md).
* Verifique se todos os pré-requisitos foram atendidos. Consulte [Pré-requisitos do ExpressRoute](expressroute-prerequisites.md).
* Consulte os requisitos para [Roteamento](expressroute-routing.md), [NAT](expressroute-nat.md) e [QoS](expressroute-qos.md).
* Configurar sua conexão do ExpressRoute.
  * [Criar e modificar um circuito do ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Criar e modificar o emparelhamento de um circuito do ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
  * [Conectar uma rede virtual a um circuito do ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Saiba mais sobre alguns dos outros principais [recursos de rede](../networking/networking-overview.md) do Azure.
