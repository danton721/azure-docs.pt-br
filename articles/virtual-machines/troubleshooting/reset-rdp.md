---
title: Redefinir a senha ou a configuração da Área de Trabalho Remota em uma VM do Windows | Microsoft Docs
description: Saiba como redefinir uma senha de conta ou serviços da Área de Trabalho Remota em uma VM do Windows usando o Portal do Azure ou o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: cynthn
ms.openlocfilehash: a8db7ef82136bae51c99bcfd2a4743e09ebf5712
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410919"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Como redefinir o serviço Área de Trabalho Remota ou sua senha de logon em uma VM do Windows
Se não conseguir se conectar a uma máquina virtual do Windows (VM), você pode redefinir a senha de administrador local ou a configuração do serviço de Área de Trabalho Remota (sem suporte em Controladores de Domínio do Windows). Você pode usar o Portal do Azure ou a extensão VM Access no Azure PowerShell para redefinir a senha. Após conectar a VM, será necessário redefinir a senha para esse usuário.  
Se estiver usando o PowerShell, verifique se você tem o [último módulo do PowerShell instalado e configurado](/powershell/azure/overview) e se está conectado à sua assinatura do Azure. Você também pode [realizar essas etapas para VMs criadas com o modelo de implantação Clássico](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

## <a name="ways-to-reset-configuration-or-credentials"></a>Maneiras para redefinir a configuração ou as credenciais
É possível redefinir os serviços e as credenciais de Área de Trabalho Remota de várias maneiras diferentes, dependendo de suas necessidades:

- [Redefinir usando o portal do Azure](#azure-portal)
- [Redefinir usando o Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Portal do Azure
Para expandir o menu do portal, clique nas três barras no canto superior esquerdo e clique em **Máquinas virtuais**:

![Navegue até sua VM do Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Redefinir a senha da conta de administrador local**

Selecione sua máquina virtual do Windows e clique em **Suporte + Solução de Problemas** > **Redefinir senha**. A folha de redefinição de senha é exibida:

![Página Redefinir senha](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Insira o nome de usuário e uma nova senha e clique em **Atualizar**. Tente se conectar à VM novamente.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Redefinir a configuração dos serviços de Área de Trabalho Remota**

Selecione sua máquina virtual do Windows e clique em **Suporte + Solução de Problemas** > **Redefinir senha**. A folha de redefinição de senha é exibida. 

![Redefinir configuração do RDP](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Selecione **Redefinir somente configuração** no menu suspenso e clique em **Atualizar**. Tente se conectar à VM novamente.


## <a name="vmaccess-extension-and-powershell"></a>Extensão VMAccess e PowerShell
Verifique se você tem o [último módulo do PowerShell instalado e configurado](/powershell/azure/overview) e se está conectado à sua assinatura do Azure com o cmdlet `Connect-AzureRmAccount`.

### <a name="reset-the-local-administrator-account-password"></a>**Redefinir a senha da conta de administrador local**
Redefina a senha ou o nome de usuário do administrador com o cmdlet [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) do PowerShell. O typeHandlerVersion deve ser 2.0 ou superior, pois a versão 1 foi preterida. 

```powershell
$SubID = "<SUBSCRIPTION ID>" 
$RgName = "<RESOURCE GROUP NAME>" 
$VmName = "<VM NAME>" 
$Location = "<LOCATION>" 
 
Connect-AzureRmAccount 
Select-AzureRMSubscription -SubscriptionId $SubID 
Set-AzureRmVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
```

> [!NOTE] 
> Se você digitar um nome diferente daquele da conta atual do administrador local na VM, a extensão VMAccess adicionará a conta do administrador local com esse nome e atribuirá a senha especificada a essa conta. Se existir uma conta do administrador local na VM, a senha será restaurada e, se a conta estiver desabilitada, a extensão VMAccess a habilitará.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Redefinir a configuração dos serviços de Área de Trabalho Remota**
Redefina o acesso remoto à VM com o cmdlet [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) do PowerShell. O seguinte exemplo redefine a extensão de acesso chamada `myVMAccess` na VM chamada `myVM` no grupo de recursos `myResourceGroup`:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> A qualquer momento, uma VM pode ter apenas um único agente de acesso de VM. Para definir as propriedades do agente de acesso à VM com êxito, a opção `-ForceRerun` pode ser usada. Ao usar `-ForceRerun`, lembre-se de usar o mesmo nome do agente de acesso à VM usado nos comandos anteriores.

Se você ainda não consegue se conectar remotamente à máquina virtual, veja mais etapas a serem testadas em [Solucionar problemas de conexões da Área de Trabalho Remota com uma máquina virtual do Azure baseada no Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Se você perder a conexão com o controlador de domínio do Windows, será necessário restaurá-lo a partir de um backup do controlador de domínio.

## <a name="next-steps"></a>Próximas etapas
Se a extensão de acesso a VM do Azure não responder e não for possível redefinir a senha, você pode [redefinir a senha local do Windows offline](../windows/reset-local-password-without-agent.md). Esse método é um processo mais avançado e exige que você conecte o disco rígido virtual da VM problemática em outra VM. Siga as etapas documentadas neste artigo primeiro; e só tente o método de redefinição de senha offline como último recurso.

[Recursos e extensões de VM do Azure](../extensions/features-windows.md)

[Conectar-se a uma máquina virtual do Azure com RDP ou SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Solucionar problemas de conexões de Área de Trabalho Remota para uma máquina virtual do Azure baseada em Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
