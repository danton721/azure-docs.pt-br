---
title: Redefinir peerings do Azure ExpressRoute | Microsoft Docs
description: Como desabilitar e habilitar peeerings de um circuito do ExpressRoute.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180386"
---
# <a name="reset-expressroute-peerings"></a>Redefinir os emparelhamentos do ExpressRoute

Este artigo descreve como desabilitar e habilitar peerings de um circuito da Rota Expressa usando o PowerShell. Quando você desativa um emparelhamento, a sessão do BGP na conexão principal e na conexão secundária do circuito da ExpressRoute será encerrada. Você perderá conectividade por meio desse peering para a Microsoft. Quando você ativa um emparelhamento, a sessão do BGP na conexão principal e na conexão secundária do circuito da ExpressRoute será ativada. Você vai recuperar a conectividade através deste peering para a Microsoft. Você pode habilitar e desabilitar o Microsoft Peering e o Azure Private Peering em um circuito do ExpressRoute de forma independente. Quando você configura primeiro os peerings em seu circuito da Rota Expressa, os peerings são ativados por padrão. 

Há alguns cenários em que você pode achar útil redefinir seus peerings da ExpressRoute.
* Teste seu design de recuperação de desastres e implementação. Por exemplo, você tem dois circuitos ExpressRoute. Você pode desabilitar os peerings de um circuito e forçar o tráfego de sua rede a falhar no outro circuito.
* Habilite bidirecional encaminhamento de detecção (BFD) no emparelhamento privado do Azure do seu circuito do ExpressRoute. BFD é habilitado por padrão, se o circuito de ExpressRoute for criado após 1 de agosto de 2018. Se o circuito foi criado antes dessa, BFD não foi habilitado. Você pode habilitar BFD desabilitando o emparelhamento e reabilitando a ele. Deve-se observar BFD tem suporte apenas no emparelhamento privado do Azure.


## <a name="reset-a-peering"></a>Redefinir um emparelhamento

1. Instale a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Para obter mais informações, consulte [Instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

2. Abra o console do PowerShell com privilégios elevados e conecte-se à sua conta. Use o exemplo a seguir para ajudar a se conectar:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Se você tiver várias assinaturas do Azure, verifique as assinaturas para a conta.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Especifique a assinatura que você deseja usar.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Execute os seguintes comandos para recuperar seu circuito da Rota Expressa.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. Identifique o emparelhamento que você deseja desativar ou ativar. *Emparelhamentos* é uma matriz. No exemplo a seguir, Peerings [0] é Peering e Peerings privados do Azure [1] Microsoft Peering.

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. Execute os seguintes comandos para alterar o estado do peering.

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
O emparelhamento deve estar em um estado que você definir. 

## <a name="next-steps"></a>Próximas etapas
Se você precisar de ajuda para solucionar um problema do ExpressRoute, confira os seguintes artigos:
* [Verificando a conectividade do ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Solucionando problemas de desempenho de rede](expressroute-troubleshooting-network-performance.md)