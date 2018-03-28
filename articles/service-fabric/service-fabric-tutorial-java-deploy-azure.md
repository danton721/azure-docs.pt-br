---
title: Implantar um aplicativo Java do Service Fabric em um cluster no Azure | Microsoft Docs
description: Neste tutorial, saiba como implantar um aplicativo Java do Service Fabric em um cluster do Azure Service Fabric.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/26/2018
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 5245e53429278f2a346077cdb70426aaca339488
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-deploy-a-java-application-to-a-service-fabric-cluster-in-azure"></a>Tutorial: implantar um aplicativo Java em um cluster do Service Fabric no Azure
Este tutorial é a parte três de uma série e mostra como implantar um aplicativo do Service Fabric em um cluster no Azure.

Na terceira parte da série, você aprenderá a:

> [!div class="checklist"]
> * Criar um cluster seguro do Linux no Azure 
> * Implantar um aplicativo para o cluster

Nesta série de tutoriais, você aprenderá a:
> [!div class="checklist"]
> *  [Compilar um aplicativo Java do Reliable Services no Service Fabric](service-fabric-tutorial-create-java-app.md)
> * [Implantar e depurar o aplicativo em um cluster local](service-fabric-tutorial-debug-log-local-cluster.md)
> * Implantar o aplicativo em um cluster do Azure
> * [Configurar monitoramento e diagnóstico para o aplicativo](service-fabric-tutorial-java-elk.md)
> * [Configurar CI/CD](service-fabric-tutorial-java-jenkins.md)

## <a name="prerequisites"></a>pré-requisitos
Antes de começar este tutorial:
- Se você não tem uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Instalar a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Instalar o SDK do Service Fabric para [Mac](service-fabric-get-started-mac.md) ou [Linux](service-fabric-get-started-linux.md)
- [Instalar o Python 3](https://wiki.python.org/moin/BeginnersGuide/Download)

## <a name="create-a-service-fabric-cluster-in-azure"></a>Criar um cluster do Service Fabric no Azure

As etapas a seguir criam os recursos necessários para implantar seu aplicativo em um cluster do Service Fabric. Além disso, são configurados os recursos necessários para monitorar a integridade de sua solução usando a pilha ELK (Elasticsearch, Logstash, Kibana). Especificamente, [Hubs de Eventos](https://azure.microsoft.com/services/event-hubs/) são usados como um coletor de logs do Service Fabric. Eles são configurados para enviar logs do cluster do Service Fabric para sua instância Logstash. 

1. Abra um terminal e baixe o pacote a seguir que contém os scripts auxiliares e modelos necessários para criar os recursos no Azure

    ```bash
    git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
    ```

2. Fazer logon na sua conta do Azure 

    ```bash
    az login
    ```

3. Defina a assinatura do Azure que você deseja usar para criar os recursos 

    ```bash
    az account set --subscription [SUBSCRIPTION-ID]
    ``` 

4. Na pasta *service-fabric-java-quickstart/AzureCluster*, execute o comando a seguir para criar um certificado de cluster no Key Vault. Esse certificado é usado para proteger seu cluster do Service Fabric. Forneça a região (deve ser a mesma do cluster do Service Fabric), o nome do grupo de recursos do cofre de chaves, o nome do cofre de chaves, a senha do certificado e o nome DNS do cluster. 

    ```bash
    ./new-service-fabric-cluster-certificate.sh [REGION] [KEY-VAULT-RESOURCE-GROUP] [KEY-VAULT-NAME] [CERTIFICATE-PASSWORD] [CLUSTER-DNS-NAME-FOR-CERTIFICATE]
    
    Example: ./new-service-fabric-cluster-certificate.sh 'westus' 'testkeyvaultrg' 'testkeyvault' '<password>' 'testservicefabric.westus.cloudapp.azure.com'
    ```

    O comando anterior retorna as informações a seguir, que devem ser anotadas para uso posterior.

    ```
    Source Vault Resource Id: /subscriptions/<subscription_id>/resourceGroups/testkeyvaultrg/providers/Microsoft.KeyVault/vaults/<name>
    Certificate URL: https://<name>.vault.azure.net/secrets/<cluster-dns-name-for-certificate>/<guid>
    Certificate Thumbprint: <THUMBPRINT>
    ```

5. Crie um grupo de recursos para a conta de armazenamento que armazena os logs 

    ```bash
    az group create --location [REGION] --name [RESOURCE-GROUP-NAME]
    
    Example: az group create --location westus --name teststorageaccountrg
    ```

6. Crie uma conta de armazenamento que será usada para armazenar os logs que serão produzidos

    ```bash
    az storage account create -g [RESOURCE-GROUP-NAME] -l [REGION] --name [STORAGE-ACCOUNT-NAME] --kind Storage
    
    Example: az storage account create -g teststorageaccountrg -l westus --name teststorageaccount --kind Storage
    ```

7. Acesse o [portal do Azure](https://portal.azure.com) e navegue até a guia **Assinatura de Acesso Compartilhado** da sua conta de armazenamento. Gere o token SAS da seguinte maneira. 

    ![Gerar SAS para armazenamento](./media/service-fabric-tutorial-java-deploy-azure/storagesas.png)

8. Copie a URL da SAS de conta e separe-a para usar quando criar o cluster do Service Fabric. Ela é semelhante à URL a seguir:

    ```
    https://teststorageaccount.table.core.windows.net/?sv=2017-04-17&ss=bfqt&srt=sco&sp=rwdlacup&se=2018-01-31T03:24:04Z&st=2018-01-30T19:24:04Z&spr=https,http&sig=IrkO1bVQCHcaKaTiJ5gilLSC5Wxtghu%2FJAeeY5HR%2BPU%3D
    ```

9. Crie um grupo de recursos que contenha os recursos do Hub de Eventos. Os Hubs de Eventos são usados para enviar mensagens do Service Fabric para o servidor que executa os recursos ELK.

    ```bash
    az group create --location [REGION] --name [RESOURCE-GROUP-NAME]
    
    Example: az group create --location westus --name testeventhubsrg
    ```

10. Crie um recurso de Hubs de Eventos usando o comando a seguir. Siga os prompts para inserir detalhes para namespaceName, eventHubName, consumerGroupName, sendAuthorizationRule e receiveAuthorizationRule. 

    ```bash
    az group deployment create -g [RESOURCE-GROUP-NAME] --template-file eventhubsdeploy.json
    
    Example: 
    az group deployment create -g testeventhubsrg --template-file eventhubsdeploy.json
    Please provide string value for 'namespaceName' (? for help): testeventhubnamespace
    Please provide string value for 'eventHubName' (? for help): testeventhub
    Please provide string value for 'consumerGroupName' (? for help): testeventhubconsumergroup
    Please provide string value for 'sendAuthorizationRuleName' (? for help): sender
    Please provide string value for 'receiveAuthorizationRuleName' (? for help): receiver
    ```

    Copie o conteúdo do campo **saída** na saída JSON do comando anterior. As informações do remetente são usadas quando o cluster do Service Fabric é criado. O nome do destinatário e a chave devem ser salvos para serem usados no próximo tutorial, quando o serviço de Logstash será configurado para receber mensagens do Hub de Eventos. O blob a seguir é um exemplo de saída JSON:     
    
    ```json
    "outputs": {
        "receiver Key": {
            "type": "String",
            "value": "[KEY]"
        },
        "receiver Name": {
            "type": "String",
            "value": "receiver"
        },
        "sender Key": {
            "type": "String",
            "value": "[KEY]"
        },
        "sender Name": {
            "type": "String",
            "value": "sender"
        }
    }
    ```

11. Execute o script *eventhubssastoken.py* para gerar a URL de SAS para o recurso de EventHubs que você criou. Essa URL de SAS é usada pelo cluster do Service Fabric para enviar logs para os Hubs de Eventos. Como resultado, a política de **remetente** é usada para gerar a URL. O script retorna a URL de SAS para o recurso de Hubs de Eventos que é usado na etapa a seguir:

    ```python
    python3 eventhubssastoken.py 'testeventhubs' 'testeventhubs' 'sender' '[PRIMARY-KEY]'
    ```

    Copie o valor do campo **sr** no JSON retornado. O valor do campo **sr** é o token SAS para EventHubs. A URL a seguir é um exemplo do campo **sr**:

    ```bash
    https%3A%2F%2Ftesteventhubs.servicebus.windows.net%2Ftesteventhubs&sig=7AlFYnbvEm%2Bat8ALi54JqHU4i6imoFxkjKHS0zI8z8I%3D&se=1517354876&skn=<policy_name>
    ```

    A URL de SAS para os EventHubs segue a estrutura: https://<namespacename>.servicebus.windows.net/<eventhubsname>?sr=<sastoken>. Por exemplo, https://testeventhubs.servicebus.windows.net/testeventhubs?sr=https%3A%2F%2Ftesteventhubs.servicebus.windows.net%2Ftesteventhubs&sig=7AlFYnbvEm%2Bat8ALi54JqHU4i6imoFxkjKHS0zI8z8I%3D&se=1517354876&skn=sender

12. Abra o arquivo *sfdeploy.parameters.json* e substitua o conteúdo a seguir das etapas anteriores 

    ```
    "applicationDiagnosticsStorageAccountName": {
        "value": "teststorageaccount"
    },
    "applicationDiagnosticsStorageAccountSasToken": {
        "value": "[SAS-URL-STORAGE-ACCOUNT]"
    },
    "loggingEventHubSAS": {
        "value": "[SAS-URL-EVENT-HUBS]"
    }
    ```

13. Execute o comando a seguir para criar o cluster do Service Fabric

    ```bash
    az sf cluster create --location 'westus' --resource-group 'testlinux' --template-file sfdeploy.json --parameter-file sfdeploy.parameters.json --secret-identifier <certificate_url_from_step4>
    ```

## <a name="deploy-your-application-to-the-cluster"></a>Implantar o aplicativo no cluster

1. Antes de implantar seu aplicativo, você precisa adicionar o trecho de código a seguir no arquivo *Voting/VotingApplication/ApplicationManifest.xml*. O campo **X509FindValue** é a impressão digital retornada na Etapa 4 da seção **Criar um cluster do Service Fabric no Azure**. Este trecho de código está aninhado no campo **ApplicationManifest** (o campo raiz). 

    ```xml
    <Certificates>
          <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[CERTIFICATE-THUMBPRINT]" />
    </Certificates>
    ```

2. Para implantar seu aplicativo nesse cluster, você deve usar o SFCTL para estabelecer uma conexão com o cluster. O SFCTL requer um arquivo PEM com as chaves pública e privada para se conectar ao cluster e, como resultado, executar o comando a seguir para produzir um arquivo PEM com as chaves pública e privada. 

    ```bash
    openssl pkcs12 -in testservicefabric.westus.cloudapp.azure.com.pfx -out sfctlconnection.pem -nodes -passin pass:<password>
    ```

3. Execute o comando a seguir para se conectar com o cluster.

    ```bash
    sfctl cluster select --endpoint https://testlinuxcluster.westus.cloudapp.azure.com:19080 --pem sfctlconnection.pem --no-verify
    ```

4. Para implantar seu aplicativo, navegue até a pasta *Voting/Scripts* e execute o script **install.sh**. 

    ```bash
    ./install.sh
    ```

5. Para acessar o Service Fabric Explorer, abra seu navegador favorito e digite https://testlinuxcluster.westus.cloudapp.azure.com:19080. Selecione o certificado do repositório de certificados que você deseja usar para se conectar a esse ponto de extremidade. Se você estiver usando um computador Linux, os certificados que foram gerados pelo script *new-service-fabric-cluster-certificate.sh* devem ser importados no Chrome para exibir o Service Fabric Explorer. Se estiver usando um Mac, é preciso instalar o arquivo PFX em seu conjunto de chaves. Observe que o aplicativo foi instalado no cluster. 

    ![SFX Java do Azure](./media/service-fabric-tutorial-java-deploy-azure/sfxjavaonazure.png)

6. Para acessar seu aplicativo, digite https://testlinuxcluster.westus.cloudapp.azure.com:8080 

    ![Aplicativo de votação Java do Azure](./media/service-fabric-tutorial-java-deploy-azure/votingappjavaazure.png)

7. Para desinstalar seu aplicativo do cluster, execute o script *uninstall.sh* na pasta **Scripts** 

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar um cluster seguro do Linux no Azure 
> * Criar os recursos necessários para o monitoramento com ELK 
> * Opcional: como usar clusters de terceiros para experimentar o Service Fabric

Prosseguir para o próximo tutorial:
> [!div class="nextstepaction"]
> [Configurar monitoramentos e diagnósticos](service-fabric-tutorial-java-elk.md)