---
title: 'Conecte sua rede local a uma rede virtual do Azure: VPN site a site: PowerShell | Microsoft Docs'
description: Etapas para criar uma conexão IPsec de sua rede local para uma rede virtual do Azure pela Internet pública. Essas etapas o ajudarão a criar uma conexão de Gateway de VPN Site a Site entre locais usando o PowerShell.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: cherylmc
ms.openlocfilehash: e4530cd34097bb25dc7100df5852a72f4daae84f
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66727285"
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>Criar uma Rede Virtual com uma conexão VPN site a site usando o PowerShell

Este artigo mostra como usar o PowerShell para criar uma conexão de gateway de VPN Site a Site de sua rede local para a Vnet. As etapas neste artigo se aplicam ao modelo de implantação do Resource Manager. Você também pode criar essa configuração usando uma ferramenta de implantação ou um modelo de implantação diferente, selecionando uma opção diferente na lista a seguir:

> [!div class="op_single_selector"]
> * [Portal do Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Portal do Azure (clássico)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Uma conexão de gateway de VPN Site a Site é usada para conectar a rede local a uma rede virtual do Azure por um túnel VPN IPsec/IKE (IKEv1 ou IKEv2). Esse tipo de conexão exige um dispositivo VPN localizado no local que tenha um endereço IP público voltado para o exterior atribuído a ele. Para saber mais sobre os gateways de VPN, veja [Sobre o gateway de VPN](vpn-gateway-about-vpngateways.md).

![Diagrama de conexão Site a Site de Gateway de VPN entre locais](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <a name="before"></a>Antes de começar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verifique se você atende aos seguintes critérios antes de iniciar a configuração:

* Verifique se você possui um dispositivo VPN compatível e alguém que possa configurá-lo. Para obter mais informações sobre dispositivos VPN compatíveis e a configuração de dispositivo, confira [Sobre dispositivos VPN](vpn-gateway-about-vpn-devices.md).
* Verifique se você possui um endereço IPv4 público voltado para o exterior para seu dispositivo VPN.
* Se não estiver familiarizado com os intervalos de endereços IP localizados na configuração de rede local, você precisará trabalhar em conjunto com alguém que possa lhe fornecer os detalhes. Ao criar essa configuração, você deve especificar os prefixos de intervalo de endereços IP que o Azure roteará para seu local. Nenhuma das sub-redes da rede local podem se sobrepor às sub-redes de rede virtual às quais você deseja se conectar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="running-powershell-locally"></a>Executando o PowerShell localmente

Se você optar por instalar e usar o PowerShell localmente, instale a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Os cmdlets do PowerShell são atualizados com frequência e geralmente você precisará atualizar seu cmdlets do PowerShell para obter a funcionalidade mais recente do recurso. Se você não atualizar seus cmdlets do PowerShell, os valores especificados poderão falhar. 

Para localizar a versão que você está usando, execute 'Get-Module -ListAvailable Az'. Se você precisa fazer a atualização, confira [Instalar o módulo do Azure PowerShell](/powershell/azure/install-az-ps). Para obter mais informações, confira [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).
Se você estiver executando o PowerShell localmente, também precisará executar ‘Connect-AzAccount’ para criar uma conexão com o Azure.


### <a name="example"></a>Valores de exemplo

Os exemplos neste artigo usam os seguintes valores. Você pode usar esses valores para criar um ambiente de teste ou consultá-los para compreender melhor os exemplos neste artigo.

```
#Example values

VnetName                = VNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.1.0.0/16 
SubnetName              = Frontend 
Subnet                  = 10.1.0.0/24 
GatewaySubnet           = 10.1.255.0/27
LocalNetworkGatewayName = Site1
LNG Public IP           = <On-premises VPN device IP address> 
Local Address Prefixes  = 10.101.0.0/24, 10.101.1.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWPIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite1

```

## <a name="VNet"></a>1. Criar uma rede virtual e uma sub-rede de gateway

Se você ainda não tiver uma rede virtual, crie uma. Ao criar uma rede virtual, certifique-se de que os espaços de endereço que você especificar não se sobreponham nenhum espaço de endereço que você tenha na rede local. 

>[!NOTE]
>Para que essa rede virtual conecte a um local, é necessário coordenar com o administrador de rede local para que ele consiga um intervalo de endereços IP que você pode usar especificamente para essa rede virtual. Se um intervalo de endereços duplicado existir em ambos os lados da conexão de VPN, o tráfego não será roteado da maneira esperada. Além disso, se você deseja se conectar esta rede virtual a outra rede virtual, o espaço de endereços não pode sobrepor outra rede virtual. Tome cuidado e planeje sua configuração de rede de forma adequada.
>
>

### <a name="about-the-gateway-subnet"></a>Sobre a sub-rede de gateway

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="vnet"></a>Criar uma rede virtual e uma sub-rede de gateway

O exemplo a seguir cria uma rede virtual e uma sub-rede de gateway. Se você já tiver uma rede virtual que você precisa adicionar a uma sub-rede de gateway, consulte [Para adicionar uma sub-rede do gateway a uma rede virtual que você já criou](#gatewaysubnet).

Crie um grupo de recursos:

```azurepowershell-interactive
New-AzResourceGroup -Name TestRG1 -Location 'East US'
```

Criar sua rede virtual.

1. Defina as variáveis.

   ```azurepowershell-interactive
   $subnet1 = New-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27
   $subnet2 = New-AzVirtualNetworkSubnetConfig -Name 'Frontend' -AddressPrefix 10.1.0.0/24
   ```
2. Crie a Rede Virtual.

   ```azurepowershell-interactive
   New-AzVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1 `
   -Location 'East US' -AddressPrefix 10.1.0.0/16 -Subnet $subnet1, $subnet2
   ```

### <a name="gatewaysubnet"></a>Para adicionar uma sub-rede do gateway a uma rede virtual já criada

Use as etapas nesta seção quando você já tem uma rede virtual, mas precisa adicionar uma sub-rede de gateway.

1. Defina as variáveis.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
   ```
2. Crie a sub-rede de gateway.

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
   ```
3. Defina a configuração.

   ```azurepowershell-interactive
   Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```

## 2. <a name="localnet"></a>Criar o gateway de rede local

O gateway de rede local (LNG) geralmente se refere ao seu local. Não é o mesmo que um gateway de rede virtual. Você atribui um nome ao site pelo qual o Azure pode fazer referência a ele e especifica o endereço IP do dispositivo VPN local para o qual você criará uma conexão. Você também pode especificar os prefixos de endereço IP que serão roteados por meio do gateway de VPN para o dispositivo VPN. Os prefixos de endereço que você especifica são os prefixos localizados em sua rede local. Se sua rede local for alterada, você poderá atualizar facilmente os prefixos.

Use os seguintes valores:

* O *GatewayIPAddress* é o endereço IP do dispositivo VPN local.
* O *AddressPrefix* é o espaço de endereço local.

Para adicionar um gateway de rede local com um único prefixo de endereço:

  ```azurepowershell-interactive
  New-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.101.0.0/24'
  ```

Para adicionar um gateway de rede local com vários prefixos de endereço:

  ```azurepowershell-interactive
  New-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
  ```

Para modificar os prefixos de endereço IP do seu gateway de rede local:

Às vezes, os prefixos de gateway de rede local são alterados. As etapas usadas para modificar os prefixos de endereço IP dependem de você ter criado uma conexão de gateway de VPN. Consulte a seção [Modificar os prefixos do endereço IP para um gateway da rede local](#modify) deste artigo.

## <a name="PublicIP"></a>3. Solicite um endereço IP público

Um gateway de VPN deve ter um endereço IP público. Você primeiro solicita o recurso de endereço IP e, em seguida, faz referência a ele ao criar seu gateway de rede virtual. O endereço IP é atribuído dinamicamente ao recurso quando o gateway de VPN é criado. 

O gateway de VPN atualmente suporta apenas alocação de endereços IP público *Dinâmico*. Você não pode solicitar uma atribuição de endereço IP Público Estático. No entanto, isso não significa que o endereço IP será alterado após ter sido atribuído ao seu gateway de VPN. A única vez em que o endereço IP Público é alterado é quando o gateway é excluído e recriado. Isso não altera o redimensionamento, a redefinição ou outras manutenções/atualizações internas do seu gateway de VPN.

Solicite um endereço IP público que será atribuído ao gateway de VPN da rede virtual.

```azurepowershell-interactive
$gwpip= New-AzPublicIpAddress -Name VNet1GWPIP -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>4. Criar a configuração de endereçamento IP do gateway

A configuração do gateway define a sub-rede ('GatewaySubnet') e o endereço IP público a serem usados. Use o exemplo a seguir para criar a configuração do gateway:

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>5. Criar o gateway de VPN

Crie o gateway de VPN da rede virtual.

Use os seguintes valores:

* O *-GatewayType* para uma configuração Site a Site é *Vpn*. O tipo de gateway é sempre específico para a configuração que você está implementando. Por exemplo, outras configurações de gateway podem exigir -GatewayType ExpressRoute.
* O *-VpnType* pode ser *RouteBased* (referido como Gateway Dinâmico em alguns documentos) ou *PolicyBased* (referido como Gateway Estático em alguns documentos). Para saber mais sobre os tipos de gateway de VPN, veja [Sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md).
* Selecione a SKU de Gateway que você deseja usar. Há limitações de configuração para alguns SKUs. Para obter mais informações, confira [SKUs de gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku). Se você receber um erro ao criar o gateway de VPN sobre o - GatewaySku, verifique se você instalou a versão mais recente dos cmdlets do PowerShell.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

Depois de executar esse comando, pode levar até 45 minutos para a configuração do gateway ser concluída.

## <a name="ConfigureVPNDevice"></a>6. Configurar o dispositivo de VPN

As conexões Site a Site para uma rede local exigem um dispositivo VPN. Nesta etapa, você deve configurar seu dispositivo VPN. Ao configurar seu dispositivo VPN, você precisará dos itens a seguir:

- Uma chave compartilhada. Essa é a mesma chave compartilhada especificada ao criar a conexão VPN Site a Site. Em nossos exemplos, usamos uma chave compartilhada básica. Recomendamos gerar uma chave mais complexa para uso.
- O endereço IP público do seu gateway de rede virtual. Você pode exibir o endereço IP público usando o portal do Azure, o PowerShell ou a CLI. Para localizar o endereço IP público do seu gateway de rede virtual usando o PowerShell, use o exemplo a seguir. Neste exemplo, VNet1GWPIP é o nome do recurso de endereço IP público que você criou em uma etapa anterior.

  ```azurepowershell-interactive
  Get-AzPublicIpAddress -Name VNet1GWPIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <a name="CreateConnection"></a>7. Criar a conexão VPN

Em seguida, crie a conexão VPN Site a Site entre o gateway de rede virtual e o dispositivo VPN. Substitua os valores pelos seus próprios. A chave compartilhada deve corresponder ao valor usado para a configuração do dispositivo VPN. Observe que o '-ConnectionType' para Site a Site é **IPsec**.

1. Defina as variáveis.
   ```azurepowershell-interactive
   $gateway1 = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```

2. Crie a conexão.
   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1 `
   -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
   -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

Após um instante, a conexão será estabelecida.

## <a name="toverify"></a>8. Verificar a conexão VPN

Existem algumas maneiras diferentes de verificar a conexão VPN.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>Para conectar-se a uma máquina virtual

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Para modificar os prefixos do endereço IP para um gateway de rede local

Se os prefixos de endereço IP que você deseja rotear para o seu local forem alterados, você pode modificar o gateway de rede local. São fornecidos dois conjuntos de instruções. As instruções escolhidas dependem de você já ter criado sua conexão de gateway. Ao usar esses exemplos, modifique os valores de acordo com seu ambiente.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Para modificar o endereço IP do gateway para um gateway de rede local

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Próximas etapas

*  Quando sua conexão for concluída, você poderá adicionar máquinas virtuais às suas redes virtuais. Para saber mais, veja [Máquinas virtuais](https://docs.microsoft.com/azure/).
* Para obter informações sobre o BGP, consulte a [Visão Geral do BGP](vpn-gateway-bgp-overview.md) e [Como configurar o BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Para saber mais sobre como criar uma conexão VPN site a site usando o modelo do Azure Resource Manager, veja [Criar uma conexão VPN site a site](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/).
* Para saber mais sobre como criar uma conexão VPN de rede virtual para rede virtual usando o modelo do Azure Resource Manager, veja [Implantar a replicação geográfica do HBase](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).
