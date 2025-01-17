---
title: Pré-requisitos - Azure Disk Encryption para VMs de IaaS | Microsoft Docs
description: Este artigo fornece os pré-requisitos para usar o Microsoft Azure Disk Encryption para VMs IaaS.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 03/25/2019
ms.custom: seodec18
ms.openlocfilehash: 6874258c31d4dd7d2a0aa0042624ee57616c0a89
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234276"
---
# <a name="azure-disk-encryption-prerequisites"></a>Pré-requisitos de criptografia de disco do Azure

Neste artigo, pré-requisitos de criptografia de disco do Azure, explica os itens que precisam estar em vigor antes de usar o Azure Disk Encryption. O Azure Disk Encryption é integrado ao [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) para ajudar gerenciar as chaves de criptografia. Você pode usar o [Azure PowerShell](/powershell/azure/overview), a [CLI do Azure](/cli/azure/) ou o [portal do Azure](https://portal.azure.com) para configurar o Azure Disk Encryption.

Antes de habilitar o Azure Disk Encryption em VMs IaaS do Azure para os cenários com suporte que foram discutidos na [visão geral de criptografia de disco do Azure](azure-security-disk-encryption-overview.md) do artigo, certifique-se de ter os pré-requisitos em vigor. 

> [!WARNING]
> - Se você já tiver usado o [Azure Disk Encryption com aplicativo do Microsoft Azure Active Directory](azure-security-disk-encryption-prerequisites-aad.md) para criptografar essa VM, precisará continuar a usar essa opção para criptografar a sua VM. Não é possível usar o [Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) nessa VM criptografada pois esse cenário não é suportado, o que significa abandonar o aplicativo Azure Active Directory nessa VM criptografada que ainda não tem suporte.
> - Determinadas recomendações podem aumentar o uso de recursos de dados, rede ou computação, resultando em custos adicionais de licença ou inscrição. Você deve ter uma assinatura ativa válida do Azure para criar recursos no Azure nas regiões com suporte.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="bkmk_OSs"></a> Sistemas operacionais com suporte
O Azure Disk Encryption tem suporte nos seguintes sistemas operacionais:

- Versões do Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2012 R2 Server Core e servidor do Windows Server 2016 core.
Para o Windows Server 2008 R2, você deve ter o .NET Framework 4.5 instalado antes de habilitar a criptografia no Azure. Para instalá-lo a partir do Windows Update com a atualização opcional Microsoft .NET Framework 4.5.2 para sistemas baseados em x64 do Windows Server 2008 R2 (KB2901983).
- Núcleo do Windows Server 2012 R2 e Windows Server 2016 Core têm suporte pelo Azure Disk Encryption, depois que o componente bdehdcfg for instalado na VM.
- Versões de cliente do Windows: Cliente Windows 8 e cliente Windows 10.
- O Azure Disk Encryption tem suporte somente em distribuições e versões específicas do servidor Linux baseado na Galeria do Azure. Para obter a lista das versões com suporte no momento, consulte o [perguntas frequentes sobre o Azure Disk Encryption](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Se referir à [distribuições do Linux endossadas no Azure](../virtual-machines/linux/endorsed-distros.md) para obter a lista de imagens com suporte pela Microsoft e para o [quais distribuições do Linux oferece suporte de Azure Disk Encryption?](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) no [Azure Perguntas frequentes sobre a criptografia de disco](azure-security-disk-encryption-faq.md) para a lista de versões com suporte no momento, com base nas distribuições endossadas imagem.
- O Azure Disk Encryption exige que seu cofre de chaves e as VMs residam na mesma região e assinatura do Azure. Configurá os recursos em regiões separadas causa uma falha na habilitação do recurso de Azure Disk Encryption.

## <a name="bkmk_LinuxPrereq"></a> Pré-requisitos adicionais para VMs IaaS Linux 

- O Azure Disk Encryption para Linux requer 7 GB de RAM na VM para habilitar a criptografia de disco do sistema operacional em [imagens com suporte](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). Quando o processo de criptografia de disco do sistema operacional for concluído, a VM pode ser configurada para executar com menos memória.
- Azure Disk Encryption exige o módulo vfat esteja presente no sistema.  Remover ou desabilitar esse módulo a partir da imagem padrão impedirá que o sistema seja capaz de ler o volume de chave e obter a chave necessária para desbloquear os discos em reinicializações subsequentes. Etapas de proteção do sistema que remova o módulo vfat do sistema não são compatíveis com o Azure Disk Encryption. 
- Antes de habilitar a criptografia, os discos de dados a serem criptografados precisam ser listados corretamente em /etc/fstab. Use um nome de dispositivo de bloco persistente para essa entrada, pois nomes de dispositivo no formato "/dev/sdX" não podem ser considerados para serem associados com o mesmo disco entre as reinicializações, principalmente depois que a criptografia é aplicada. Para obter mais detalhes sobre esse comportamento, consulte: [Solucionar problemas de alterações do nome do dispositivo da VM do Linux](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- Verifique se as configurações de /etc/fstab estão definidas corretamente para a montagem. Para definir essas configurações, execute o comando mount - a ou reinicie a VM e disparar a remontagem dessa forma. Quando terminar, verifique a saída do comando lsblk para verificar se a unidade ainda está montada. 
  - Se o arquivo /etc/fstab não montar a unidade corretamente antes de habilitar a criptografia, o Azure Disk Encryption não será capaz de montá-la corretamente.
  - O processo de criptografia de disco do Azure se moverá as informações de montagem fora /etc/fstab e em seu próprio arquivo de configuração como parte do processo de criptografia. Não se assuste para ver a entrada ausente do /etc/fstab após a criptografia de unidade de dados é concluída.
  -  Após a reinicialização, ela levará tempo para o processo de criptografia de disco do Azure montar os discos criptografados recentemente. Eles não estarão disponíveis imediatamente após uma reinicialização. O processo precisa de tempo para iniciar, desbloquear e, em seguida, montar as unidades criptografadas antes de estar disponível para ser acessado por outros processos. Esse processo pode levar mais de um minuto após a reinicialização, dependendo das características do sistema.

Um exemplo de comandos que podem ser usados para montar os discos de dados e criar as entradas/ etc/fstab necessárias pode ser encontrado nas [linhas 244-248 desse arquivo de script ](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L244-L248). 


## <a name="bkmk_GPO"></a> Sistema de rede e a diretiva de grupo

**Para habilitar o recurso de Azure Disk Encryption, as VMs IaaS devem atender aos seguintes requisitos de configuração do ponto de extremidade de rede:**
  - Para obter um token para se conectar ao seu cofre de chaves, a VM IaaS deve ser capaz de se conectar a um ponto de extremidade do Azure Active Directory, \[login.microsoftonline.com\].
  - Para gravar as chaves de criptografia no cofre de chaves, a VM IaaS deve ser capaz de se conectar ao ponto de extremidade do cofre de chaves.
  - A VM IaaS deve ser capaz de se conectar a um ponto de extremidade do armazenamento do Azure que hospeda o repositório de extensão do Azure e uma conta de armazenamento do Azure que hospeda os arquivos VHD.
  -  Se a política de segurança limita o acesso de VMs do Azure à Internet, você pode resolver o URI anterior e configurar uma regra específica para permitir a conectividade de saída para os IPs. Para obter mais informações, consulte [Azure Key Vault por trás de um firewall](../key-vault/key-vault-access-behind-firewall.md).    


**Política de grupo:**
 - A solução de Azure Disk Encryption usa o protetor de chave externa BitLocker para VMs IaaS do Windows. Para VMs ingressado no domínio, não envie por push todas as políticas de grupo que imponham protetores TPM. Para obter informações sobre a política de grupo "Permitir BitLocker sem um TPM compatível", confira [Referência de política de grupo do BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  A política do BitLocker em máquinas virtuais ingressadas no domínio com política de grupo personalizada deve incluir a seguinte configuração: [Configurar o armazenamento de usuário de informações de recuperação do bitlocker -> Permitir chave de recuperação de 256 bits](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). O Azure Disk Encryption falha quando as configurações da política de grupo personalizada para o BitLocker são incompatíveis. Em computadores que não tinham a configuração de política correta, aplique a nova política, force a atualização da nova política (gpupdate.exe /force) e, em seguida, pode ser necessário reiniciar.

- O Azure Disk Encryption falhará se a política de grupo no nível do domínio bloqueia o algoritmo AES-CBC, que é usado pelo Bitlocker.


## <a name="bkmk_PSH"></a>PowerShell do Azure
O [Azure PowerShell](/powershell/azure/overview) fornece um conjunto de cmdlets que usa o modelo do [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para gerenciar os recursos do Azure. Você pode usá-lo em seu navegador com [Azure Cloud Shell](../cloud-shell/overview.md), ou você pode instalá-lo em seu computador local usando as instruções abaixo para usá-lo em qualquer sessão do PowerShell. Se você já tiver instalado localmente, verifique se que você usar a versão mais recente do SDK do Azure PowerShell para configurar o Azure Disk Encryption. Baixe a última versão do [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Instale o Azure PowerShell para uso em seu computador local (opcional): 
1. Em seguida, siga as instruções nos links para o seu sistema operacional, porém continuar o restante das etapas a seguir.      
   - [Instale e configure o Azure PowerShell](/powershell/azure/install-az-ps). 
     - Instalar o PowerShellGet, Azure PowerShell e carregar o módulo do Az. 

2. Verifique se as versões instaladas do módulo Az. Se necessário, [atualize o módulo do Azure PowerShell](/powershell/azure/install-az-ps#update-the-azure-powershell-module).
    É recomendável usar a versão mais recente do módulo Az.

     ```powershell
     Get-Module Az -ListAvailable | Select-Object -Property Name,Version,Path
     ```

3. Entrar no Azure usando o [AzAccount Connect](/powershell/module/az.accounts/connect-azaccount) cmdlet.
     
     ```azurepowershell-interactive
     Connect-AzAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzAccount –Environment (Get-AzEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzSubscription and 
     specify it with Set-AzContext.  #>
     Get-AzSubscription
     Set-AzContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

4.  Se necessário, examine a [Introdução ao Azure PowerShell](/powershell/azure/get-started-azureps).

## <a name="bkmk_CLI"></a> Instalar a CLI do Azure para uso em seu computador local (opcional)

O [CLI 2.0 do Azure](/cli/azure) é uma ferramenta de linha de comando para gerenciar recursos do Azure. A CLI é projetada para consultar dados com flexibilidade, dar suporte a operações de longa execução como processos desbloqueados e facilitar o script. Você pode usá-lo em seu navegador com o [Azure Cloud Shell](../cloud-shell/overview.md), ou você pode instalá-lo em seu computador local e usá-lo em qualquer sessão do PowerShell.

1. [Instalar CLI do Azure](/cli/azure/install-azure-cli) para uso em seu computador local (opcional):

2. Verifique se a versão instalada.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Entrar no Azure usando [login az](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Revise a [Introdução à CLI 2.0 do Azure](/cli/azure/get-started-with-azure-cli), se necessário. 


## <a name="prerequisite-workflow-for-key-vault"></a>Fluxo de trabalho pré-requisito para o Key Vault
Se você já estiver familiarizado com os pré-requisitos do Key Vault e do Azure AD para o Azure Disk Encryption, você pode usar o [script do PowerShell de pré-requisitos do Azure Disk Encryption](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/Compute/Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Para obter mais informações sobre como usar o script de pré-requisitos, veja o [Início rápido para criptografar uma VM](quick-encrypt-vm-powershell.md) e o [Apêndice do Azure Disk Encryption](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Se necessário, crie um grupo de recursos.
2. Crie um cofre da chave. 
3. Defina as políticas de acesso avançado da chave de segurança.

>[!WARNING]
>Antes de excluir um cofre de chaves, verifique se você não criptografou nenhuma VM existente. Para proteger um cofre de exclusão acidental, [habilite a exclusão reversível](../key-vault/key-vault-soft-delete-powershell.md#enabling-soft-delete) e o [bloqueio de recurso](../azure-resource-manager/resource-group-lock-resources.md) no cofre. 
 
## <a name="bkmk_KeyVault"></a> Criar um cofre de chaves 
O Azure Disk Encryption se integra [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para ajudá-lo a controlar e gerenciar as chaves de criptografia de disco e segredos em sua assinatura do Cofre de chaves. Você pode criar um cofre de chaves ou usar um existente para o Azure Disk Encryption. Para obter mais informações sobre cofres-chave, consulte [Introdução ao Cofre de Chaves do Azure](../key-vault/key-vault-get-started.md) e [Proteja seu cofre de chaves](../key-vault/key-vault-secure-your-key-vault.md). Você pode usar um modelo do Resource Manager, o Azure PowerShell ou a CLI do Azure para criar um cofre de chaves. 


>[!WARNING]
>Para garantir que os segredos de criptografia não ultrapassem os limites regionais, o Azure Disk Encryption precisa que o Key Vault e as VMs sejam colocadas na mesma região. Crie e use um cofre de chaves que esteja na mesma região da VM a ser criptografada. 


### <a name="bkmk_KVPSH"></a> Criar um cofre de chaves com o PowerShell

Você pode criar um cofre de chaves com o Azure PowerShell usando o [New-AzKeyVault](/powershell/module/az.keyvault/New-azKeyVault) cmdlet. Para outros cmdlets para o Cofre de chaves, consulte [Az.KeyVault](/powershell/module/az.keyvault/). 

1. Se necessário, [conectar-se à sua assinatura do Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Criar um novo grupo de recursos, se necessário, com [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup).  Para listar os locais de centro de dados, use [Get-AzLocation](/powershell/module/az.resources/get-azlocation). 
     
     ```azurepowershell-interactive
     # Get-AzLocation 
     New-AzResourceGroup –Name 'MyKeyVaultResourceGroup' –Location 'East US'
     ```

3. Criar um novo cofre de chaves usando [AzKeyVault novo](/powershell/module/az.keyvault/New-azKeyVault)
    
      ```azurepowershell-interactive
     New-AzKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -Location 'East US'
     ```

4. Observe a **nome do cofre**, **nome do grupo de recursos**, **ID do recurso**, **URI do cofre**e o **ID de objeto** que são retornados para uso posterior ao criptografar os discos. 


### <a name="bkmk_KVCLI"></a> Criar um cofre de chaves com CLI do Azure
Você pode gerenciar o Cofre de chaves com CLI do Azure usando os comandos [keyvault az](/cli/azure/keyvault#commands). Para criar um cofre de chaves, use [az creata](/cli/azure/keyvault#az-keyvault-create).

1. Se necessário, [conectar-se à sua assinatura do Azure](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Criar um novo grupo de recursos, se necessário, com [criar grupo de az](/cli/azure/group#az-group-create). Para listar os locais, use [locais de lista az conta](/cli/azure/account#az-account-list) 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MyKeyVaultResourceGroup" -l "East US"
     ```

3. Crie um novo cofre de chave usando [az keyvault create](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --location "East US"
     ```

4. Observação o **nome do cofre** (nome), **nome do grupo de recursos**, **ID do recurso** (ID), **URI do cofre**e o **deIDdeobjeto** que são retornados para uso posterior. 

### <a name="bkmk_KVRM"></a> Criar um cofre de chaves com um modelo do Resource Manager

Você pode criar um cofre de chaves usando o modelo [Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. No modelo de início rápido do Azure, clique em **Implantar no Azure**.
2. Selecione a assinatura, o grupo de recursos, o local do grupo de recursos, o nome do Key Vault, o ID do Objeto, os termos legais e o contrato e clique em **Compra**. 


## <a name="bkmk_KVper"></a> Definir as políticas de acesso avançado do cofre de chaves
A plataforma Azure precisa acessar as chaves de criptografia ou os segredos no cofre de chaves para disponibilizá-los para a máquina virtual para inicialização e descriptografar os volumes. Habilite a criptografia de disco no cofre da chave ou as implantações falharão.  

### <a name="bkmk_KVperPSH"></a> Definir políticas de acesso avançado ao cofre de chaves com o Azure PowerShell
 Use o cmdlet de PowerShell do Cofre de chaves [AzKeyVaultAccessPolicy conjunto](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) para habilitar a criptografia de disco para o Cofre de chaves.

  - **Habilitar o Key Vault para criptografia de disco:** EnabledForDiskEncryption é necessário para o Azure Disk Encryption.
      
     ```azurepowershell-interactive 
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDiskEncryption
     ```

  - **Habilitar o Key Vault para a implantação, se necessário:** Permite que o provedor de recursos Microsoft.Compute recupere segredos desse cofre de chaves quando esse cofre de chaves é referenciado na criação de recursos, por exemplo, ao criar uma máquina virtual.

     ```azurepowershell-interactive
      Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForDeployment
     ```

  - **Habilitar o Key Vault para implantação de modelo, se necessário:** Permite que o Azure Resource Manager obtenha segredos desse cofre de chaves quando esse cofre de chaves é referenciado em uma implantação de modelo.

     ```azurepowershell-interactive             
     Set-AzKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MyKeyVaultResourceGroup' -EnabledForTemplateDeployment
     ```

### <a name="bkmk_KVperCLI"></a> Definir Cofre de chaves avançadas de políticas de acesso usando a CLI do Azure
Use [atualização de keyvault az](/cli/azure/keyvault#az-keyvault-update) para habilitar a criptografia de disco para o Cofre de chaves. 

 - **Habilitar o Key Vault para criptografia de disco:** Enabled-for-disk-encryption é necessário. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-disk-encryption "true"
     ```  

 - **Habilitar o Key Vault para a implantação, se necessário:** Permite que o provedor de recursos Microsoft.Compute recupere segredos desse cofre de chaves quando esse cofre de chaves é referenciado na criação de recursos, por exemplo, ao criar uma máquina virtual.

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-deployment "true"
     ``` 

 - **Habilitar o Key Vault para implantação de modelo, se necessário:** Permite que o Resource Manager recupere segredos do cofre.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MyKeyVaultResourceGroup" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Defina as políticas de acesso avançado ao cofre de chaves por meio do portal do Azure

1. Selecione sua keyvault, vá para **Access Policies** e **Clique para mostrar as políticas de acesso avançadas**.
2. Selecione a caixa rotulada **habilitar o acesso ao Azure Disk Encryption para criptografia de volume**.
3. Selecione **habilitar o acesso às máquinas virtuais do Azure para implantação** e/ou **habilitar acesso ao Azure Resource Manager para implantação de modelo**, se necessário. 
4. Clique em **Salvar**.

![Cofre de chaves do Azure, políticas de acesso avançadas](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Configurar uma chave de criptografia de chave (opcional)
Se você quiser usar uma chave de criptografia (KEK) para uma camada adicional de segurança para chaves de criptografia, adicione uma KEK ao seu cofre de chaves. Use o [AzKeyVaultKey adicionar](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet para criar uma chave de criptografia de chave no cofre de chaves. Você também pode importar a KEK do HSM do gerenciamento de chaves local. Para obter mais informações, consulte [documentação do cofre de chaves](../key-vault/key-vault-hsm-protected-keys.md). Quando uma chave de criptografia de chave é especificada, o Azure Disk Encryption usa essa chave para agrupar os segredos de criptografia antes de gravar no Key Vault.

* Ao gerar as chaves, use um tipo de chave RSA. O Azure Disk Encryption ainda não dá suporte usando as chaves de curva elíptica.

* O segredo do cofre de chaves e as URLs KEK devem ter controle de versão. O Azure impõe essa restrição de controle de versão. Para um segredo válido e URLs KEK, confira os seguintes exemplos:

  * Exemplo de uma URL secreta válida: *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Exemplo de uma URL KEK válida: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* o Azure Disk Encryption não dá suporte à especificação de números de portas como parte de segredos do cofre de chaves e URLs KEK. Para exemplos de URLs do cofre de chaves não suportados e suportados, consulte os seguintes exemplos:

  * URL do cofre de chaves inaceitável  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * URL do cofre de chaves aceitável:  *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*


### <a name="bkmk_KEKPSH"></a> Configurar uma chave de criptografia chave com o Azure PowerShell 
Antes de usar o script do PowerShell, você deve estar familiarizado com os pré-requisitos do Azure Disk Encryption para entender as etapas no script. O script de amostra pode precisar de mudanças para seu ambiente. Esse script cria todos os pré-requisitos da criptografia de disco do Azure e criptografa uma VM IaaS existente, envolvendo a chave de criptografia de disco usando uma chave de criptografia de chave. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MyKeyVaultResourceGroup', and 'MySecureVault' with your values.
     # Use Get-AzLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzResourceGroup
     
     $Loc = 'MyLocation';
     $KVRGname = 'MyKeyVaultResourceGroup';
     $KeyVaultName = 'MySecureVault'; 
     New-AzResourceGroup –Name $KVRGname –Location $Loc;
     New-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname -Location $Loc;
     $KeyVault = Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname;
     $KeyVaultResourceId = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzKeyVault -VaultName $KeyVaultName -ResourceGroupName $KVRGname).VaultUri;
     
 #Step 2: Enable the vault for disk encryption.
     Set-AzKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $KVRGname -EnabledForDiskEncryption;
      
 #Step 3: Create a new key in the key vault with the Add-AzKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'HSM';
     $keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 4: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' and 'MyVirtualMachineResourceGroup' with your values. 
     
     $VMName = 'MySecureVM';
     $VMRGName = 'MyVirtualMachineResourceGroup';
     Set-AzVMDiskEncryptionExtension -ResourceGroupName $VMRGName -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```


 
## <a name="next-steps"></a>Próximas etapas
> [!div class="nextstepaction"]
> [Habilitar o Azure Disk Encryption para Windows](azure-security-disk-encryption-windows.md)

> [!div class="nextstepaction"]
> [Habilitar o Azure Disk Encryption para Linux](azure-security-disk-encryption-linux.md)

