---
title: Atualizar um cluster do Azure Service Fabric para usar o nome comum do certificado | Microsoft Docs
description: Saiba fazer um cluster do Service Fabric alternar do uso de impressões digitais de certificado para o uso do nome comum do certificado.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/01/2019
ms.author: aljo
ms.openlocfilehash: a94fda5a1f3aedd5842bad92b5348a77177b4137
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66302455"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Alterar o cluster de impressão digital do certificado para nome comum
Dois certificados não podem ter a mesma impressão digital, o que dificulta a substituição ou gerenciamento de certificados de cluster. Vários certificados, no entanto, podem ter o mesmo nome comum ou assunto.  Alternar um cluster implantado do uso de impressões digitais de certificado para o uso de nomes comuns do certificado simplifica muito o gerenciamento de certificados. Este artigo descreve como atualizar um cluster do Service Fabric em execução para usar o nome comum do certificado em vez da impressão digital do certificado.

>[!NOTE]
> Se você tiver duas impressões digitais declaradas em seu modelo, você precisa executar duas implantações.  A primeira implantação é feita antes de seguir as etapas neste artigo.  A primeira implantação define a propriedade da sua **impressão digital** no modelo para o certificado que está sendo usado e remove a propriedade da **impressão digital secundária**.  Para a segunda implantação, siga as etapas neste artigo.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-a-certificate"></a>Obter um certificado
Primeiro, obtenha um [certificado de uma autoridade de certificação (CA)](https://wikipedia.org/wiki/Certificate_authority).  O nome comum do certificado deve ser o nome do host do cluster.  Por exemplo, "nomedomeucluster.southcentralus.cloudapp.azure.com".  

Para fins de teste, você pode obter um certificado assinado CA de uma autoridade de certificação livre ou aberta.

> [!NOTE]
> Não há suporte para certificados autoassinados, inclusive os gerados durante a implantação de um cluster do Service Fabric no Portal do Azure.

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Carregar o certificado e instalá-lo no conjunto de dimensionamento
No Azure, um cluster do Service Fabric é um conjunto de dimensionamento de máquinas virtuais.  Carregue o certificado para um cofre de chaves e, em seguida, instale-o no conjunto de dimensionamento de máquina virtual em que o cluster está em execução.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName `
    -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName `
    -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault `
    -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Verbose `
    -Name $VmssName -VirtualMachineScaleSet $vmss 
```

>[!NOTE]
> Os segredos do conjunto de dimensionamento não dão suporte à mesma ID de recurso para dois segredos separados, pois cada segredo é um recurso exclusivo com controle de versão. 

## <a name="download-and-update-the-template-from-the-portal"></a>Fazer o download e atualizar o modelo a partir do portal
O certificado foi instalado no conjunto de dimensionamento subjacente, mas você também precisará atualizar o cluster do Service Fabric para usar esse certificado e seu nome comum.  Agora, faça o download do modelo para a implantação de cluster.  Entrar para o [portal do Azure](https://portal.azure.com) e navegue até o grupo de recursos que hospeda o cluster.  Em **Configurações**, selecione **Implantações**.  Selecione a implantação mais recente e clique em **Exibir modelo**.

![Exibir modelos][image1]

Faça o download do modelo e dos arquivos de parâmetro JSON para o computador local.

Primeiro, abra o arquivo de parâmetros em um editor de texto e adicione o seguinte valor de parâmetro:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Em seguida, abra o arquivo de modelo em um editor de texto e faça três atualizações para dar suporte ao nome comum do certificado.

1. Na seção **Parâmetros**, adicione um parâmetro *certificateCommonName*:
    ```json
    "certificateCommonName": {
        "type": "string",
        "metadata": {
            "description": "Certificate Commonname"
        }
    },
    ```

    Também Considere remover a *certificateThumbprint*, ela não pode ser referenciada no modelo do Resource Manager.

2. No recurso **Microsoft.Compute/virtualMachineScaleSets**, atualize a extensão de máquina virtual para usar o nome comum em configurações de certificado em vez da impressão digital.  Em **virtualMachineProfile**->**extensionProfile**->**extensões**->**propriedades**->**configurações**->**certificado**, adicionar `"commonNames": ["[parameters('certificateCommonName')]"],` e remover `"thumbprint": "[parameters('certificateThumbprint')]",`.
    ```json
        "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                            "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                            "nodeTypeRef": "[variables('vmNodeType0Name')]",
                            "dataPath": "D:\\SvcFab",
                            "durabilityLevel": "Bronze",
                            "enableParallelJobs": true,
                            "nicPrefixOverride": "[variables('subnet0Prefix')]",
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        "typeHandlerVersion": "1.0"
                    }
                },
    ```

3.  No recurso **Microsoft.ServiceFabric/clusters**, atualize a versão da API para "2018-02-01".  Adicione também uma configuração **certificateCommonNames** com uma propriedade **commonNames** e remova a configuração **certificado** (com a propriedade de impressão digital), como no exemplo a seguir:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
            "addonFeatures": [
                "DnsService",
                "RepairManager"
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": ""
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
    ```

## <a name="deploy-the-updated-template"></a>Implantar o modelo atualizado
Reimplante o modelo atualizado após fazer as alterações.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [segurança de cluster](service-fabric-cluster-security.md).
* Saiba como [substituir um certificado de cluster](service-fabric-cluster-rollover-cert-cn.md)
* [Atualizar e gerenciar certificados do cluster](service-fabric-cluster-security-update-certs-azure.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
