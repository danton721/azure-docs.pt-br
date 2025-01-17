---
title: Usar as rotas personalizadas do Azure para habilitar a ativação KMS com túnel forçado | Microsoft Docs
description: Mostra como usar as rotas personalizadas do Azure para habilitar a ativação KMS ao usar o túnel forçado no Azure.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: 6557649eb1b97ad4d88876906737f8249e18b958
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399800"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>Falha de ativação do Windows no cenário de túnel forçado

Este artigo descreve como resolver o problema de ativação do KMS que pode ocorrer ao habilitar o túnel forçado em cenários do ExpressRoute ou de conexão VPN site a site.

## <a name="symptom"></a>Sintoma

Habilite o [túnel forçado](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) nas sub-redes das redes virtuais do Azure para direcionar todo o tráfego da Internet de volta para a sua rede local. Nesse cenário, as máquinas virtuais (VM) do Azure que executam o Windows Server 2012 R2 (ou versões posteriores do Windows) podem ativar o Windows com êxito. No entanto, VMs que executam versões anteriores do Windows não conseguem ativar o Windows.

## <a name="cause"></a>Causa

As VMs do Windows Azure precisam se conectar ao servidor KMS do Azure para ativar o Windows. A ativação requer que a solicitação de ativação venha de um endereço IP público do Azure. No cenário de túnel forçado, a ativação falha porque a solicitação de ativação vem de sua rede local, em vez de um endereço IP público do Azure.

## <a name="solution"></a>Solução

Para resolver esse problema, use a rota personalizada do Azure para direcionar o tráfego de ativação para o servidor KMS do Azure.

O endereço IP do servidor KMS para a nuvem global do Azure é 23.102.135.246. Seu nome DNS é kms.core.windows.net. Se você usa outras plataformas do Azure, como o Azure Alemanha, deve usar o endereço IP do servidor KMS correspondente. Para obter mais informações, confira a tabela a seguir:

|Plataforma| KMS DNS|KMS IP|
|------|-------|-------|
|Azure Global|kms.core.windows.net|23.102.135.246|
|Azure Alemanha|kms.core.cloudapi.de|51.4.143.248|
|Governo dos EUA para Azure|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


Para adicionar a rota personalizada, siga essas etapas:

### <a name="for-resource-manager-vms"></a>VMs do Resource Manager

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

1. Abra o Azure PowerShell e [conecte-se à sua conta do Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Execute os seguintes comandos:

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable

    # Next, attach the route table to the subnet that hosts the VMs

    Set-AzVirtualNetworkSubnetConfig -Name "Subnet01" -VirtualNetwork $vnet -AddressPrefix "10.0.0.0/24" -RouteTable $RouteTable

    Set-AzVirtualNetwork -VirtualNetwork $vnet
    ```
3. Acesse a VM que apresenta problemas de ativação. Use [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para testar se é possível acessar o servidor KMS:

        psping kms.core.windows.net:1688

4. Tente ativar o Windows e veja se o problema foi resolvido.

### <a name="for-classic-vms"></a>Para VMs Clássicas

1. Abra o Azure PowerShell e [conecte-se à sua conta do Azure](https://docs.microsoft.com/powershell/azure/authenticate-azureps).
2. Execute os seguintes comandos:

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. Acesse a VM que apresenta problemas de ativação. Use [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) para testar se é possível acessar o servidor KMS:

        psping kms.core.windows.net:1688

4. Tente ativar o Windows e veja se o problema foi resolvido.

## <a name="next-steps"></a>Próximas etapas

- [Chaves de instalação de cliente KMS](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [Revisar e Selecionar Métodos de Ativação](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
