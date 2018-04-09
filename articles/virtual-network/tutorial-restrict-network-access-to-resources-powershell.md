---
title: Restringir o acesso à rede para recursos PaaS - Azure PowerShell | Microsoft Docs
description: Saiba como limitar e restringir o acesso à rede para os recursos do Azure, como Armazenamento do Microsoft Azure e Banco de Dados SQL do Azure, com pontos de extremidade de serviço de rede virtual usando PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 7e402af74babda2ce32d4a1597c61d71aba89b9e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Restrinja o acesso à rede a recursos de PaaS com pontos de extremidade de serviço de rede virtual usando PowerShell

Os pontos de extremidade de serviço de rede virtual permitem limitar o acesso à rede a alguns recursos de serviço do Azure para uma sub-rede da rede virtual. Você também pode remover o acesso à internet para os recursos. Os pontos de extremidade de serviço fornecem conexão direta de sua rede virtual para os serviços do Azure com suporte, permitindo que você use o espaço de endereço privado da sua rede virtual para acessar os serviços do Azure. O tráfego destinado aos recursos do Azure por meio de pontos de extremidade de serviço sempre ficam na rede de backbone do Microsoft Azure. Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Criar uma rede virtual com uma sub-rede
> * Adicionar uma sub-rede e habilitar um ponto de extremidade de serviço
> * Criar um recurso do Azure e permitir o acesso à rede para ele de apenas uma sub-rede
> * Implantar uma máquina virtual (VM) para cada sub-rede
> * Confirmar o acesso a um recurso a partir de uma sub-rede
> * A confirmação do acesso é negada para um recurso a partir de uma sub-rede e da internet

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se você optar por instalar e usar o PowerShell localmente, este artigo exigirá o módulo do Azure PowerShell versão 5.4.1 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps). Se você estiver executando o PowerShell localmente, também precisará executar o `Login-AzureRmAccount` para criar uma conexão com o Azure.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de criar uma rede virtual, será necessário criar um grupo de recursos para a rede virtual e todos os outros recursos criados neste artigo. Crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo a seguir cria um grupo de recursos chamado *myResourceGroup*: 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo a seguir cria uma rede virtual nomeada *myVirtualNetwork* com o prefixo de endereço *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Crie uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). O exemplo a seguir cria uma configuração de sub-rede para uma sub-rede denominada *Pública*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Crie a sub-rede na rede virtual gravando a configuração de sub-rede na rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Habilitar um ponto de extremidade de serviço 

Você pode habilitar pontos de extremidade de serviço apenas para serviços que oferecem suporte a pontos de extremidade de serviço. Exiba os serviços habilitados para ponto de extremidade de serviço em um local do Azure com[Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). O exemplo a seguir retorna uma lista de serviços habilitados para ponto de extremidade de serviço disponíveis na região *eastus*. A lista de serviços retornados aumentará ao longo do tempo, visto que mais serviços do Azure se tornam habilitados para ponto de extremidade de serviço.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Crie uma sub-rede adicional na rede virtual. Neste exemplo, uma sub-rede denominada *Privada* é criada com um ponto de extremidade de serviço para *Microsoft.Storage*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Restringir o acesso à rede para e de uma sub-rede

Crie regras de segurança de grupo de segurança de rede com [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). A seguinte regra permite o acesso de saída para os endereços IP públicos atribuídos ao serviço de Armazenamento do Microsoft Azure: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

A regra a seguir nega acesso a todos os endereços IP públicos. A regra anterior substitui essa regra, devido à sua prioridade mais alta, o que permite acesso aos endereços IP públicos do Armazenamento do Microsoft Azure.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

A seguinte regra permite o tráfego de entrada Remote Desktop Protocol (RDP) para a sub-rede de qualquer lugar. Conexões de área de trabalho remota são permitidas na sub-rede, para que você possa confirmar o acesso à rede para um recurso em uma etapa posterior.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Crie um grupo de segurança de rede com [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). O seguinte exemplo cria um grupo de segurança de rede chamado *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Associe o grupo de segurança de rede para a sub-rede *Privada* com [Remote Desktop Protocol](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) e, em seguida, grave a configuração de sub-rede para a rede virtual. O exemplo a seguir associa o grupo de segurança de rede *myNsgPrivate* à sub-rede *Privada*:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Restringir o acesso à rede a um recurso

As etapas necessárias para restringir o acesso à rede a recursos criados por meio de serviços do Azure habilitados para pontos de extremidade de serviço variam de acordo com os serviços. Consulte a documentação de serviços individuais para etapas específicas de cada serviço. O restante deste artigo inclui etapas para restringir o acesso à rede de uma conta de Armazenamento do Microsoft Azure como um exemplo.

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Crie uma conta de armazenamento do Azure com [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Substitua `<replace-with-your-unique-storage-account-name>` por um nome que seja exclusivo em todos os locais do Azure, entre 3 a 24 caracteres de comprimento, usando apenas números e letras minúsculas.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Depois que a conta de armazenamento é criada, recupere a chave para a conta de armazenamento em uma variável com [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

A chave é usada para criar um compartilhamento de arquivos em uma etapa posterior. Insira `$storageAcctKey` e observe o valor, já que você também precisará inseri-lo manualmente em uma etapa posterior ao mapear o compartilhamento de arquivos para uma unidade em uma VM.

### <a name="create-a-file-share-in-the-storage-account"></a>Criar um compartilhamento de arquivos na conta de armazenamento

Crie um contexto para sua conta e chave de armazenamento com [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). O contexto encapsula o nome da conta de armazenamento e a chave da conta:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Crie um compartilhamento de arquivos com [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share = New-AzureStorageShare my-file-share -Context $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Negar todo acesso à rede para uma conta de armazenamento

Por padrão, as contas de armazenamento aceitam conexões de rede de clientes em qualquer rede. Para limitar o acesso às redes selecionadas, altere a ação padrão para *Negar* com [Update-AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Depois que o acesso à rede for negado, a conta de armazenamento não estará acessível em nenhuma rede.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Habilitar o acesso à rede de uma sub-rede

Recupere a rede virtual criada com [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) e, em seguida, recupere o objeto de sub-rede privada em uma variável com [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Permita o acesso à rede para a conta de armazenamento a partir da sub-rede *Privada* com [Add-AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Para testar o acesso à rede para uma conta de armazenamento, implante uma VM para cada sub-rede.

### <a name="create-the-first-virtual-machine"></a>Criar a primeira máquina virtual

Crie uma máquina virtual na sub-rede *Pública* com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Ao executar o comando a seguir, as credenciais serão solicitadas. Os valores que você inseriu são configurados como o nome de usuário e senha para a VM. A opção `-AsJob` cria a VM em segundo plano para que você possa prosseguir para a próxima etapa.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

É retornada uma saída semelhante ao exemplo a seguir:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>Criar uma segunda máquina virtual

Crie uma máquina virtual na sub-rede *Privada*:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

São necessários alguns minutos para o Azure criar a VM. Não prossiga para a próxima etapa até que o Azure termine de criar a VM e retorne a saída para o PowerShell. 

## <a name="confirm-access-to-storage-account"></a>Confirmar acesso à conta de armazenamento

Use [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para retornar o endereço IP público de uma VM. O exemplo a seguir retorna o endereço IP público da VM *myVmPrivate*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Substitua `<publicIpAddress>` no seguinte comando pelo endereço IP público retornado do comando anterior e, em seguida, insira o comando a seguir: 

```powershell
mstsc /v:<publicIpAddress>
```

Um arquivo .rdp (Remote Desktop Protocol) é criado e baixado para o computador. Abra o arquivo rdp baixado. Se solicitado, selecione **Conectar**. Insira o nome de usuário e senha que você especificou ao criar a VM. Talvez seja necessário selecionar **Mais escolhas** e, em seguida, **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM. Selecione **OK**. Você pode receber um aviso do certificado durante o processo de logon. Se você receber o aviso, selecione **Sim** ou **Continuar**, para prosseguir com a conexão.

Na VM *myVmPrivate*, mapeie o compartilhamento de arquivos do Azure para a unidade Z usando o PowerShell. Antes de executar os comandos a seguir, substitua `<storage-account-key>` e `<storage-account-name>` por valores que você forneceu ou recuperou em [Criar uma conta de armazenamento](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
O PowerShell retorna saídas semelhantes à seguinte saída de exemplo:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

O compartilhamento de arquivos do Azure mapeado com êxito para a unidade Z.

Confirme se a VM não tem nenhuma conectividade de saída com nenhum outro endereço IP público:

```powershell
ping bing.com
```

Você não recebe nenhuma resposta porque o grupo de segurança de rede associado à sub-rede *Privada* não permite acesso de saída para endereços IP públicos que não sejam os endereços atribuídos ao serviço de Armazenamento do Microsoft Azure.

Feche a sessão da área de trabalho remota para a VM *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Confirmar que o acesso está negado para a conta de armazenamento

Obtenha o endereço IP público da VM *myVmPublic*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Substitua `<publicIpAddress>` no seguinte comando pelo endereço IP público retornado do comando anterior e, em seguida, insira o comando a seguir: 

```powershell
mstsc /v:<publicIpAddress>
```

Na VM *myVmPublic*, tente mapear o compartilhamento de arquivos do Azure para a unidade Z. Antes de executar os comandos a seguir, substitua `<storage-account-key>` e `<storage-account-name>` por valores que você forneceu ou recuperou em [Criar uma conta de armazenamento](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

O acesso ao compartilhamento é negado, e você recebe um erro `New-PSDrive : Access is denied`. Acesso negado porque a VM *myVmPublic* é implantada na sub-rede *Pública*. A sub-rede *Pública* não tem um ponto de extremidade de serviço habilitado para Armazenamento do Microsoft Azure, e a conta de armazenamento só permite o acesso à rede a partir da sub-rede *Privada*, não da sub-rede *Pública*.

Feche a sessão da área de trabalho remota para a VM *myVmPublic* VM.

Em seu computador, tente exibir os compartilhamentos de arquivos na conta de armazenamento com o seguinte comando:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Acesso negado, e você recebe um *Get-AzureStorageFile : O servidor remoto retornou um erro: (403) Proibido. Código de status de HTTP: 403 - Mensagem de erro de HTTP: Esta solicitação não está autorizada a executar esta operação*, porque o computador não está na sub-rede *Privada* da rede virtual *MyVirtualNetwork*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, você poderá utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que ele contém:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você habilitou um ponto de extremidade de serviço para uma sub-rede de rede virtual. Você aprendeu que os pontos de extremidade de serviço podem ser habilitados para os recursos implantados com vários serviços do Azure. Você criou uma conta de Armazenamento do Microsoft Azure e limitou o acesso à rede para a conta de armazenamento, restringindo apenas aos recursos em uma sub-rede de rede virtual. Antes de criar pontos de extremidade de serviço em redes virtuais de produção, é recomendável que você se familiarize completamente com os [pontos de extremidade de serviço](virtual-network-service-endpoints-overview.md).

Se tiver várias redes virtuais em sua conta, você poderá conectar duas redes virtuais em conjunto para que os recursos em cada rede virtual possam se comunicar uns com os outros. Avance para o próximo tutorial para saber como conectar redes virtuais.

> [!div class="nextstepaction"]
> [Conectar redes virtuais](./tutorial-connect-virtual-networks-powershell.md)