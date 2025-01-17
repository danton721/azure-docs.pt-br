---
title: Gerenciar certificados em um cluster do Azure Service Fabric | Microsoft Docs
description: Descreve como adicionar novos certificados, sobrepor certificados e remover certificados de e para um cluster do Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: 91adc3d3-a4ca-46cf-ac5f-368fb6458d74
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2018
ms.author: aljo
ms.openlocfilehash: f1998ec2fe82b9fd52547fbccb208542b22bc949
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306907"
---
# <a name="add-or-remove-certificates-for-a-service-fabric-cluster-in-azure"></a>Adicionar ou remover certificados para um cluster do Service Fabric no Azure
É recomendável que você se familiarize com o modo como o Service Fabric usa certificados X.509 e com os [Cenários de segurança do cluster do cluster](service-fabric-cluster-security.md). Você deve entender o que é um certificado de cluster e qual sua finalidade, antes de continuar.

O comportamento de carregamento do certificado padrão do SDK do Azure Service Fabrics, é implantar e usar um certificado definido com uma data de expiração mais distante no futuro; independentemente de sua definição de configuração primária ou secundária. Voltando para o comportamento clássico é não avançada ação recomendada e exige a definir o valor do parâmetro de configuração "UseSecondaryIfNewer" como false na sua configuração Fabric.Code.

O Service Fabric permite especificar dois certificados de cluster, um primário e um secundário, quando você configura a segurança do certificado durante a criação do cluster, além de certificados de cliente. Veja a [criação de um cluster do Service Fabric por meio do portal](service-fabric-cluster-creation-via-portal.md) ou a [criação de um cluster do azure por meio do Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) para obter detalhes sobre a configuração deles no tempo de criação. Se você especificar apenas um certificado de cluster no momento da criação, em seguida, que é usado como o certificado principal. Após a criação do cluster, é possível adicionar um novo certificado como um secundário.

> [!NOTE]
> Para um cluster seguro, você sempre precisará de, pelo menos, um certificado de cluster (primário ou secundário) válido (não revogado ou expirado) implantado; caso contrário, o cluster vai parar de funcionar. Em 90 dias antes que todos os certificados válidos alcancem a expiração, o sistema gera um rastreamento de avisos e também um evento de integridade de aviso no nó. Atualmente, não há nenhum email nem qualquer outra notificação enviada pelo Service Fabric sobre esse artigo. 
> 
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add-a-secondary-cluster-certificate-using-the-portal"></a>Adicionar um certificado de cluster secundário usando o portal
Certificado de cluster secundário não pode ser adicionado por meio do portal do Azure, use o powershell do Azure. O processo será descrito posteriormente neste documento.

## <a name="remove-a-cluster-certificate-using-the-portal"></a>Remover um certificado de cluster usando o portal
Para um cluster seguro, sempre será necessário pelo menos um certificado (não revogado ou expirado) válido. O certificado implantado com a data mais distante na data de expiração futura estará em uso e removê-lo fará o cluster parar de funcionar; certifique-se de remover somente o certificado que expirou ou um certificado não utilizado que expira em breve.

Para remover um certificado de segurança de cluster não usado, navegue até a seção Segurança e selecione a opção 'Excluir' no menu de contexto do certificado não usado.

Se a sua intenção for remover o certificado que está marcado como primário,você precisará implantar um certificado secundário com uma data de expiração ainda mais no futuro que o certificado primário, permitindo o comportamento de sobreposição automática. Exclua o certificado primário após a substituição automática ser concluída.

## <a name="add-a-secondary-certificate-using-resource-manager-powershell"></a>Adicionar um certificado secundário usando o Powershell do Resource Manager
> [!TIP]
> Há agora uma maneira melhor e mais fácil para adicionar um certificado secundário usando o [AzServiceFabricClusterCertificate adicionar](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet. Não é necessário seguir o restante das etapas nesta seção.  Além disso, você não precisa que o modelo usado originalmente para criar e implantar o cluster ao usar o [AzServiceFabricClusterCertificate adicionar](/powershell/module/az.servicefabric/add-azservicefabricclustercertificate) cmdlet.

Essas etapas pressupõem que você esteja familiarizado com o funcionamento do Gerenciador de Recursos e tenha implantado pelo menos um cluster do Service Fabric usando um modelo do Resource Manager e tenha o modelo que você usou para configurar o cluster à mão. Presume-se também que você esteja familiarizado com o uso do JSON.

> [!NOTE]
> Se você estiver procurando um modelo de exemplo e parâmetros que você pode usar para acompanhar ou como um ponto de partida, baixe-o deste [repositório Git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample). 
> 
> 

### <a name="edit-your-resource-manager-template"></a>Editar o modelo do Resource Manager

Para facilitar o acompanhamento, o exemplo 5-VM-1-NodeTypes-Secure_Step2.JSON contém todas as edições que faremos. o exemplo está disponível no [repositório Git](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample).

**Certifique-se de seguir todas as etapas**

1. Abra o modelo do Resource Manager que foi usado para implantar o cluster. (Se você baixou o exemplo do repositório anterior, use 5-VM-1-NodeTypes-Secure_Step1.JSON para implantar um cluster seguro e, em seguida, abra o modelo).

2. Adicione **dois novos parâmetros** "secCertificateThumbprint" e "secCertificateUrlValue" do tipo "string" para a seção de parâmetro do modelo. Você pode copiar o snippet de código a seguir e adicioná-lo ao modelo. Dependendo da fonte do seu modelo, você talvez já tenha esses definido, se assim mover para a próxima etapa. 
 
    ```json
       "secCertificateThumbprint": {
          "type": "string",
          "metadata": {
            "description": "Certificate Thumbprint"
          }
        },
        "secCertificateUrlValue": {
          "type": "string",
          "metadata": {
            "description": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
          }
        },
    
    ```

3. Faça alterações no recurso **Microsoft.ServiceFabric/clusters** - localize a definição do recurso "Microsoft.ServiceFabric/clusters" em seu modelo. Nas propriedades dessa definição, você encontrará a marcação do JSON “Certificate”, que deve ser parecida com o snippet do JSON a seguir:
   
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Adicione uma nova marcação “thumbprintSecondary” e atribua a ela um valor de “[parameters('secCertificateThumbprint')]”.  

    Agora a definição de recurso deve ser parecida com a seguinte (dependendo da fonte do modelo, ela poderá não ser exatamente como o snippet abaixo). 

    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('certificateThumbprint')]",
              "thumbprintSecondary": "[parameters('secCertificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

    Se você quiser **substituir o certificado**, especifique o novo certificado como primário e mova a réplica primária atual como secundário. Isso resulta na substituição do certificado primário atual pelo novo certificado em uma etapa de implantação.
    
    ```JSON
          "properties": {
            "certificate": {
              "thumbprint": "[parameters('secCertificateThumbprint')]",
              "thumbprintSecondary": "[parameters('certificateThumbprint')]",
              "x509StoreName": "[parameters('certificateStoreValue')]"
         }
    ``` 

4. Faça alterações em **todas** as definições do recurso **Microsoft.Compute/virtualMachineScaleSets** - localize a definição do recurso Microsoft.Compute/virtualMachineScaleSets. Role até "publisher": "Microsoft", em "virtualMachineProfile".

    Nas configurações de editor do Service Fabric, você verá algo assim.
    
    ![Json_Pub_Setting1][Json_Pub_Setting1]
    
    Adicionar novas entradas de cert a ele
    
    ```json
                   "certificateSecondary": {
                        "thumbprint": "[parameters('secCertificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    
    ```

    As propriedades devem ter esta aparência
    
    ![Json_Pub_Setting2][Json_Pub_Setting2]
    
    Se você quiser **substituir o certificado**, especifique o novo certificado como primário e mova a réplica primária atual como secundário. Isso resulta na substituição do certificado atual pelo novo certificado em uma etapa de implantação.     

    ```json
                   "certificate": {
                       "thumbprint": "[parameters('secCertificateThumbprint')]",
                       "x509StoreName": "[parameters('certificateStoreValue')]"
                         },
                   "certificateSecondary": {
                        "thumbprint": "[parameters('certificateThumbprint')]",
                        "x509StoreName": "[parameters('certificateStoreValue')]"
                        }
                      },
    ```

    As propriedades devem ter esta aparência    
    ![Json_Pub_Setting3][Json_Pub_Setting3]

5. Faça alterações em **todas** as definições do recurso **Microsoft.Compute/virtualMachineScaleSets** - localize a definição do recurso Microsoft.Compute/virtualMachineScaleSets. Role até "vaultCertificates":, em "OSProfile". O resultado deve ser semelhante a este.

    ![Json_Pub_Setting4][Json_Pub_Setting4]
    
    Adicione o secCertificateUrlValue a ele. use este snippet:
    
    ```json
                      {
                        "certificateStore": "[parameters('certificateStoreValue')]",
                        "certificateUrl": "[parameters('secCertificateUrlValue')]"
                      }
    
    ```
    Agora o Json resultante deve ter esta aparência.
    ![Json_Pub_Setting5][Json_Pub_Setting5]


> [!NOTE]
> Repita as etapas 4 e 5 para todas as definições de recurso Nodetypes/Microsoft.Compute/virtualMachineScaleSets em seu modelo. Se você perder um deles, o certificado será instalado naquele conjunto de dimensionamento de máquinas virtuais e você terá resultados imprevisíveis em seu cluster, incluindo o cluster ficar inativo (se você terminar com nenhum certificado válido que o cluster possa usar para segurança. Portanto, verifique antes de continuar.
> 
> 

### <a name="edit-your-template-file-to-reflect-the-new-parameters-you-added-above"></a>Edite o arquivo de modelo para refletir os novos parâmetros adicionados acima
Se você estiver usando a amostra do [ git-repo ](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Cert%20Rollover%20Sample) para acompanhar, você pode começar a fazer alterações na amostra 5-VM-1-NodeTypes-Secure.parameters_Step2.JSON 

Editar seu parâmetro de modelo do Resource Manager de arquivo, adicione dois novos parâmetros para secCertificateThumbprint e secCertificateUrlValue. 

```JSON
    "secCertificateThumbprint": {
      "value": "thumbprint value"
    },
    "secCertificateUrlValue": {
      "value": "Refers to the location URL in your key vault where the certificate was uploaded, it is should be in the format of https://<name of the vault>.vault.azure.net:443/secrets/<exact location>"
     },

```

### <a name="deploy-the-template-to-azure"></a>Implantar o modelo no Azure

- Agora você está pronto para implantar o modelo no Azure. Abra um prompt de comando do Azure PS versão 1+.
- Entre sua conta do Azure e selecione a assinatura do azure específica. Essa é uma etapa importante para pessoas que têm acesso a mais de uma assinatura do Azure.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionId <Subscription ID> 

```

Teste o modelo antes de implantá-lo. Use o mesmo Grupo de Recursos no qual o cluster está atualmente implantado.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>

```

Implante o modelo no grupo de recursos. Use o mesmo Grupo de Recursos no qual o cluster está atualmente implantado. Execute o comando New-AzResourceGroupDeployment. Você não precisa especificar o modo, já que o valor padrão é **incremental**.

> [!NOTE]
> Se você definir o Modo como Concluído, será possível excluir inadvertidamente os recursos que não estão no modelo. Portanto, não o use neste cenário.
> 
> 

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <Resource Group that your cluster is currently deployed to> -TemplateFile <PathToTemplate>
```

Veja um exemplo preenchido do mesmo comando powershell.

```powershell
$ResourceGroup2 = "chackosecure5"
$TemplateFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure_Step2.json"
$TemplateParmFile = "C:\GitHub\Service-Fabric\ARM Templates\Cert Rollover Sample\5-VM-1-NodeTypes-Secure.parameters_Step2.json"

New-AzResourceGroupDeployment -ResourceGroupName $ResourceGroup2 -TemplateParameterFile $TemplateParmFile -TemplateUri $TemplateFile -clusterName $ResourceGroup2

```

Após a conclusão da implantação, conecte-se ao cluster usando o novo Certificado e realize algumas consultas. Faça isso se conseguir. Em seguida, você pode excluir o certificado antigo. 

Se estiver usando um certificado autoassinado, não se esqueça de importá-lo para seu repositório de certificados local TrustedPeople.

```powershell
######## Set up the certs on your local box
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My -FilePath c:\Mycertificates\chackdanTestCertificate9.pfx -Password (ConvertTo-SecureString -String abcd123 -AsPlainText -Force)

```
Para referência rápida, este é o comando para se conectar a um cluster seguro 

```powershell
$ClusterName= "chackosecure5.westus.cloudapp.azure.com:19000"
$CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7SD1D630F8F3" 

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $CertThumbprint  `
    -FindType FindByThumbprint `
    -FindValue $CertThumbprint `
    -StoreLocation CurrentUser `
    -StoreName My
```
Para referência rápida, este é o comando para obter a integridade do cluster

```powershell
Get-ServiceFabricClusterHealth 
```

## <a name="deploying-client-certificates-to-the-cluster"></a>Implantar certificados de cliente para o cluster.

Você pode usar as mesmas etapas conforme descrito nas Etapas 5 anteriores para que os certificados implantados de um Key Vault para os nós. É necessário apenas definir e usar parâmetros diferentes.


## <a name="adding-or-removing-client-certificates"></a>Adicionando ou removendo certificados de cliente

Além de certificados de cluster, você pode adicionar certificados de cliente para realizar operações de gerenciamento em um cluster do Service Fabric.

Você pode adicionar dois tipos de certificados de cliente - Admin ou Somente leitura. Eles então podem ser usados para controlar o acesso a operações de administração e operações de consulta no cluster. Por padrão, os certificados de cluster são adicionados à lista de certificados permitida Admin.

Você pode especificar qualquer número de certificados de cliente. Cada adição ou exclusão resulta em uma atualização de configuração para o cluster de Service Fabric


### <a name="adding-client-certificates---admin-or-read-only-via-portal"></a>A adição de certificados de cliente - Admin ou somente leitura por meio do portal

1. Navegue até a seção de segurança e selecione o '+ autenticação' botão na parte superior da seção de segurança.
2. Na seção ‘Adicionar Autenticação’, escolha o 'Tipo de Autenticação' - 'Cliente somente leitura' ou 'cliente Admin'
3. Agora, escolha o método de autorização. Indica ao Service Fabric se ele precisa buscar esse certificado usando o nome do assunto ou a impressão digital. Em geral, não é uma boa prática de segurança para usar o método de autorização do nome da entidade. 

![Adicionar certificado de cliente][Add_Client_Cert]

### <a name="deletion-of-client-certificates---admin-or-read-only-using-the-portal"></a>Exclusão de certificados de cliente - Admin ou somente leitura usando o portal

Para remover um certificado secundário sejam usados para segurança de cluster, navegue até a seção de segurança e selecione a opção 'Excluir' no menu de contexto do certificado específico.

## <a name="next-steps"></a>Próximas etapas
Leia estes artigos para obter mais informações sobre gerenciamento de cluster:

* [Processo de atualização de Cluster de Malha do Serviço e as suas expectativas](service-fabric-cluster-upgrade.md)
* [Configurar o acesso baseado em funções para clientes](service-fabric-cluster-security-roles.md)

<!--Image references-->
[Add_Client_Cert]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_13.PNG
[Json_Pub_Setting1]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_14.PNG
[Json_Pub_Setting2]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_15.PNG
[Json_Pub_Setting3]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_16.PNG
[Json_Pub_Setting4]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_17.PNG
[Json_Pub_Setting5]: ./media/service-fabric-cluster-security-update-certs-azure/SecurityConfigurations_18.PNG


