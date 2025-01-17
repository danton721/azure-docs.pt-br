---
title: Tutorial – Criar uma zona privada do DNS do Azure usando o Azure PowerShell
description: Neste tutorial, você cria e testa uma zona e um registro DNS privados no DNS do Azure. Este é uma guia passo a passo para criar e gerenciar sua primeira zona e registro DNS privado usando o Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 2b88454f06d2e2d42298e52feeaa26ae9d1a4902
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730257"
---
# <a name="tutorial-create-an-azure-dns-private-zone-using-azure-powershell"></a>Tutorial: Criar uma zona privada do DNS do Azure usando o Azure PowerShell

Este tutorial explica as etapas para criar sua primeira zona e registro DNS privado usando o Azure PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Uma zona DNS é usada para hospedar os registros DNS para um domínio específico. Para iniciar a hospedagem do seu domínio no DNS do Azure, você precisará criar uma zona DNS para esse nome de domínio. Cada registro DNS para seu domínio é criado dentro dessa zona DNS. Para publicar uma zona de DNS privado em sua rede virtual, você deve especificar a lista de redes virtuais que podem resolver registros na zona.  Elas são chamadas de *redes virtuais de resolução*. Também é possível especificar uma rede virtual para a qual o DNS do Azure mantenha registros de nome do host sempre que uma VM for criada, mudar de IP ou for excluída.  Ela é chamada de *rede virtual de registro*.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma zona de DNS privado
> * Criar máquinas virtuais de teste
> * Criar um registro DNS adicional
> * Testar a zona privada

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Se preferir, você pode concluir este tutorial usando a [CLI do Azure](private-dns-getstarted-cli.md).

<!--- ## Get the Preview PowerShell modules
These instructions assume you have already installed and signed in to Azure PowerShell, including ensuring you have the required modules for the Private Zone feature. -->

<!---[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)] -->

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Criar o grupo de recursos

Primeiro, crie um grupo de recursos para conter a zona DNS: 

```azurepowershell
New-AzResourceGroup -name MyAzureResourceGroup -location "eastus"
```

## <a name="create-a-dns-private-zone"></a>Criar uma zona de DNS privado

Uma zona DNS é criada usando o cmdlet `New-AzDnsZone` com um valor de *Privado* para o parâmetro **ZoneType**. O exemplo a seguir cria uma zona DNS chamada **private.contoso.com** no grupo de recursos chamado **MyAzureResourceGroup** e a disponibiliza na rede virtual chamada **MyAzureVnet**.

Observe que, se o parâmetro **ZoneType** for omitido, a zona será criada como uma zona pública e, portanto, ele é necessário para criar uma zona privada. 

```azurepowershell
$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix "10.2.0.0/24"
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName MyAzureResourceGroup `
  -Location eastus `
  -Name myAzureVNet `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $backendSubnet

New-AzDnsZone -Name private.contoso.com -ResourceGroupName MyAzureResourceGroup `
   -ZoneType Private `
   -RegistrationVirtualNetworkId @($vnet.Id)
```

Se você quisesse criar uma zona apenas para a resolução de nome (sem criação automática de nome de host), poderia usar o parâmetro *ResolutionVirtualNetworkId* em vez do parâmetro *RegistrationVirtualNetworkId*.

> [!NOTE]
> Você não poderá ver os registros de nome de host criados automaticamente. Mas, posteriormente, você irá testá-los para verificar se eles existem.

### <a name="list-dns-private-zones"></a>Listar zonas de DNS privado

Omitindo o nome da zona de `Get-AzDnsZone`, você pode enumerar todas as zonas em um grupo de recursos. Essa operação retorna uma matriz de objetos de zona.

```azurepowershell
Get-AzDnsZone -ResourceGroupName MyAzureResourceGroup
```

Omitindo o nome da zona e o nome do grupo de recursos do `Get-AzDnsZone`, você pode enumerar todas as regiões na assinatura do Azure.

```azurepowershell
Get-AzDnsZone
```

## <a name="create-the-test-virtual-machines"></a>Criar as máquinas virtuais de teste

Agora, crie duas máquinas virtuais para que você possa testar a zona DNS privada:

```azurepowershell
New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM01" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389

New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM02" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389
```

Isso levará alguns minutos para ser concluído.

## <a name="create-an-additional-dns-record"></a>Criar um registro DNS adicional

Você cria conjuntos de registros usando o cmdlet `New-AzDnsRecordSet`. O exemplo a seguir cria um registro com o nome relativo **db** na Zona DNS **private.contoso.com** no grupo de recursos **MyAzureResourceGroup**. O nome totalmente qualificado do conjunto de registros é **db.private.contoso.com**. O tipo de registro é "A", com o endereço IP "10.2.0.4" e a TTL de 3600 segundos.

```azurepowershell
New-AzDnsRecordSet -Name db -RecordType A -ZoneName private.contoso.com `
   -ResourceGroupName MyAzureResourceGroup -Ttl 3600 `
   -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.2.0.4")
```

### <a name="view-dns-records"></a>Exibir registros DNS

Para listar os registros DNS em sua zona, execute:

```azurepowershell
Get-AzDnsRecordSet -ZoneName private.contoso.com -ResourceGroupName MyAzureResourceGroup
```
Lembre-se de que você não verá registros A criados automaticamente para as duas máquinas virtuais de teste.

## <a name="test-the-private-zone"></a>Testar a zona privada

Agora você pode testar a resolução de nome para a zona privada **private.contoso.com**.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Configurar VMs para permitir ICMP de entrada

Você pode usar o comando ping para testar a resolução de nome. Portanto, configure o firewall em ambas as máquinas virtuais para permitir pacotes ICMP de entrada.

1. Conecte-se a myVM01 e abra uma janela do Windows PowerShell com privilégios de administrador.
2. Execute o comando a seguir:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

Repita para myVM02.

### <a name="ping-the-vms-by-name"></a>Executar ping de VMs por nome

1. No prompt de comando do Windows PowerShell myVM02, execute ping em myVM01 usando o nome de host registrado automaticamente:
   ```
   ping myVM01.private.contoso.com
   ```
   Você deve ver uma saída semelhante a esta:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Execute ping no nome **db** que você criou anteriormente:
   ```
   ping db.private.contoso.com
   ```
   Você deve ver uma saída semelhante a esta:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Excluir todos os recursos

Quando não for mais necessário, exclua o grupo de recursos **MyAzureResourceGroup** para excluir os recursos criados neste tutorial.

```azurepowershell
Remove-AzResourceGroup -Name MyAzureResourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você implantou uma zona DNS privada, criou um registro DNS e testou a zona.
Agora você pode aprender mais sobre zonas DNS privadas.

> [!div class="nextstepaction"]
> [Usando o DNS do Azure para domínios privados](private-dns-overview.md)




