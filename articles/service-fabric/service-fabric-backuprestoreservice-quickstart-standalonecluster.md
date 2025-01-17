---
title: Backup periódico e restauração no Azure Service Fabric | Microsoft Docs
description: Use o recurso de restauração e backup periódico do Service Fabric para habilitar o backup periódico de dados dos dados do seu aplicativo.
services: service-fabric
documentationcenter: .net
author: hrushib
manager: chackdan
editor: hrushib
ms.assetid: FAADBCAB-F0CF-4CBC-B663-4A6DCCB4DEE1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/24/2019
ms.author: hrushib
ms.openlocfilehash: 154efffcb1f86907fefecc060419c1d9450470f8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237334"
---
# <a name="periodic-backup-and-restore-in-azure-service-fabric"></a>Backup e restauração periódicos no Azure Service Fabric
> [!div class="op_single_selector"]
> * [Clusters no Azure](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 
> * [Clusters independentes](service-fabric-backuprestoreservice-quickstart-standalonecluster.md)
> 

O Service Fabric é uma plataforma de sistemas distribuídos que facilita o desenvolvimento e gerenciamento de microsserviços distribuídos confiáveis e baseados em aplicativos na nuvem. Ele permite a execução de microsserviços com e sem estado. Serviços com estado podem manter o estado autoritativo mutável além da solicitação e resposta ou de uma transação concluída. Se um serviço com estado ficar inativo por um longo tempo ou perder informações devido a um desastre, ele precisará ser restaurado para algum backup recente do seu estado para continuar a fornecer o serviço depois de ser reativado.

O Service Fabric replica o estado em vários nós para garantir que o serviço esteja altamente disponível. Mesmo se um nó do cluster falhar, os serviços continuarão disponíveis. Em alguns casos, no entanto, ainda é desejável que os dados de serviço sejam confiáveis contra falhas mais amplas.
 
Por exemplo, convém fazer o backup dos dados de um serviço para se proteger nos seguintes cenários:
- Perda permanente de um cluster inteiro do Service Fabric.
- Perda permanente da maioria das réplicas de uma partição de serviço
- Erros administrativos nos quais o estado é acidentalmente excluído ou corrompido. Por exemplo, um administrador com privilégios suficientes exclui o serviço por engano.
- Bugs no serviço que venham a corromper os dados. Por exemplo, isso pode ocorrer quando uma atualização de código de serviço inicia a gravação de dados com falha em uma coleção confiável. Nesse caso, convém reverter o código e os dados para um estado anterior.
- Processamento de dados offline. Talvez seja conveniente processar no modo offline os dados de business intelligence que ocorrem separadamente do serviço que gera os dados.

O Service Fabric fornece uma API interna para realizar [backup e restauração](service-fabric-reliable-services-backup-restore.md) pontuais. Os desenvolvedores de aplicativos podem usar essas APIs para fazer backup do estado do serviço periodicamente. Além disso, se os administradores de serviço quiserem disparar um backup de fora do serviço em um momento específico, como antes de atualizar o aplicativo, os desenvolvedores precisarão expor o backup (e a restauração) como uma API do serviço. Manter os backups é um custo adicional a este. Por exemplo, pode ser útil realizar cinco backups incrementais a cada meia hora, seguidos por um backup completo. Após o backup completo, você poderá excluir os backups incrementais anteriores. Essa abordagem requer código adicional, o que significa custos adicionais durante o desenvolvimento do aplicativo.

O backup periódico dos dados do aplicativo é uma necessidade básica para gerenciar um aplicativo distribuído e protegê-lo contra perda de dados ou perda prolongada da disponibilidade do serviço. O Service Fabric fornece um serviço opcional de backup e restauração, que permite que você configure o backup periódico dos Reliable Services com estado (inclusive Serviços de Ator) sem a necessidade de escrever código adicional. Ele também facilita a restauração dos backup obtidos anteriormente. 

O Service Fabric fornece um conjunto de APIs para obter a funcionalidade a seguir relacionada ao recurso de backup e restauração periódicos:

- Agende o backup periódico dos serviços confiáveis com estado e do Reliable Actors compatível com upload de backup para locais de armazenamento (externos). Locais de armazenamento compatíveis
    - Armazenamento do Azure
    - Compartilhamento de arquivos (local)
- Enumerar os backups
- Disparar um backup ad hoc de uma partição
- Restaurar uma partição usando o backup anterior
- Suspender backups temporariamente
- Gerenciamento de backups de retenção (em breve)

## <a name="prerequisites"></a>Pré-requisitos
* Cluster do Service Fabric com Fabric versão 6.2 e superior. O cluster deve ser configurado no Windows Server. Consulte o [artigo](service-fabric-cluster-creation-for-windows-server.md) para ver as etapas para baixar o pacote necessário.
* Certificado X.509 para criptografia de segredos necessária para se conectar ao armazenamento para armazenar backups. Consulte o [artigo](service-fabric-windows-cluster-x509-security.md) para saber como adquirir ou criar um certificado X.509 autoassinado.

* Aplicativo Service Fabric Reliable Stateful criado usando o SDK do Service Fabric versão 3.0 ou superior. Para aplicativos voltados para o .Net Core 2.0, o aplicativo deve ser criado usando o SDK do Service Fabric versão 3.1 ou superior.
* Instale o módulo de Microsoft.ServiceFabric.Powershell.Http [versão prévia em] para fazer chamadas de configuração.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

* Certifique-se de que o Cluster está conectado usando o `Connect-SFCluster` comando antes de fazer qualquer solicitação de configuração usando o módulo Microsoft.ServiceFabric.Powershell.Http.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```

## <a name="enabling-backup-and-restore-service"></a>Habilitando o serviço de backup e restauração
Primeiro, você precisa habilitar o _serviço de backup e restauração_ no seu cluster. Obtenha o modelo para o cluster que você deseja implantar. Você pode usar os [modelos de exemplo](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples). Habilite o _serviço de backup e restauração_ com as seguintes etapas:

1. Verifique se `apiversion` está definido como `10-2017` no arquivo de configuração do cluster e, se não estiver, atualize-o conforme mostrado no snippet a seguir:

    ```json
    {
        "apiVersion": "10-2017",
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        ...
    }
    ```

2. Agora, habilite o _serviço de backup e de restauração_ adicionando a seção `addonFeatures` a seguir abaixo da seção `properties`, conforme mostrado neste snippet: 

    ```json
        "properties": {
            ...
            "addonFeatures": ["BackupRestoreService"],
            "fabricSettings": [ ... ]
            ...
        }

    ```

3. Configure um certificado X.509 para criptografia de credenciais. Isso é importante para garantir que as credenciais fornecidas, se houver, se conectem ao armazenamento criptografadas antes de persistirem. Configure um certificado de criptografia adicionando a seção `BackupRestoreService` a seguir abaixo da seção `fabricSettings`, conforme mostrado neste snippet: 

    ```json
    "properties": {
        ...
        "addonFeatures": ["BackupRestoreService"],
        "fabricSettings": [{
            "name": "BackupRestoreService",
            "parameters":  [{
                "name": "SecretEncryptionCertThumbprint",
                "value": "[Thumbprint]"
            }]
        }
        ...
    }
    ```

4. Depois de atualizar o arquivo de configuração de cluster com as alterações anteriores, aplique-as e permita que a implantação/atualização seja concluída. Depois de concluído, o _serviço de backup e restauração_ começa a ser executado no seu cluster. O URI deste serviço é `fabric:/System/BackupRestoreService` e o serviço pode ser localizado na seção de serviço do sistema no Service Fabric Explorer. 

## <a name="enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors"></a>Habilitar o backup periódico para o serviço confiável com estado e Reliable Actors
Vamos percorrer as etapas para habilitar o backup periódico do serviço confiável com estado e do Reliable Actors. Essas etapas pressupõem que
- O cluster está configurado com o _serviço de backup e restauração_.
- Um serviço confiável com estado é implantado no cluster. Para a finalidade deste guia de início rápido, o URI do aplicativo é `fabric:/SampleApp` e o URI para o serviço confiável com estado que pertence a este aplicativo é `fabric:/SampleApp/MyStatefulService`. Esse serviço é implantado com uma única partição, e a ID da partição é `23aebc1e-e9ea-4e16-9d5c-e91a614fefa7`.  

### <a name="create-backup-policy"></a>Criar política de backup

A primeira etapa é criar a política de backup que descreve a agenda de backup, o armazenamento de destino para os dados de backup, o nome da política e o máximo de backups incrementais permitidos antes de disparar o armazenamento de backup. 

Para armazenamento de backup, crie o compartilhamento de arquivos e conceda acesso ReadWrite a ele para todos os computadores do nó do Service Fabric. Este exemplo supõe que o compartilhamento com o nome `BackupStore` existe em `StorageServer`.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell usando o módulo Microsoft.ServiceFabric.Powershell.Http

```powershell

New-SFBackupPolicy -Name 'BackupPolicy1' -AutoRestoreOnDataLoss $true -MaxIncrementalBackups 20 -FrequencyBased -Interval 00:15:00 -FileShare -Path '\\StorageServer\BackupStore' -Basic -RetentionDuration '10.00:00:00'

```
#### <a name="rest-call-using-powershell"></a>Chamada de REST usando o Powershell

Execute o seguinte script do PowerShell para invocar a API REST necessária para criar nova política.

```powershell
$ScheduleInfo = @{
    Interval = 'PT15M'
    ScheduleKind = 'FrequencyBased'
}   

$StorageInfo = @{
    Path = '\\StorageServer\BackupStore'
    StorageKind = 'FileShare'
}

$RetentionPolicy = @{ 
    RetentionPolicyType = 'Basic'
    RetentionDuration =  'P10D'
}

$BackupPolicy = @{
    Name = 'BackupPolicy1'
    MaxIncrementalBackups = 20
    Schedule = $ScheduleInfo
    Storage = $StorageInfo
    RetentionPolicy = $RetentionPolicy
}

$body = (ConvertTo-Json $BackupPolicy)
$url = "http://localhost:19080/BackupRestore/BackupPolicies/$/Create?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
```

### <a name="enable-periodic-backup"></a>Habilitar backup periódico
Depois de definir a política para atender aos requisitos de proteção de dados do aplicativo, a política de backup deve ser associada ao aplicativo. Dependendo do requisito, a política de backup pode ser associada um aplicativo, serviço ou partição.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell usando o módulo Microsoft.ServiceFabric.Powershell.Http

```powershell
Enable-SFApplicationBackup -ApplicationId 'SampleApp' -BackupPolicyName 'BackupPolicy1'
```

#### <a name="rest-call-using-powershell"></a>Chamada de REST usando o Powershell
Execute o seguinte script do PowerShell para invocar a API REST necessária para associar a política de backup com o nome `BackupPolicy1` criada no etapa anterior com o aplicativo `SampleApp`.

```powershell
$BackupPolicyReference = @{
    BackupPolicyName = 'BackupPolicy1'
}

$body = (ConvertTo-Json $BackupPolicyReference)
$url = "http://localhost:19080/Applications/SampleApp/$/EnableBackup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json'
``` 

### <a name="verify-that-periodic-backups-are-working"></a>Verificar se os backups periódicos estão funcionando

Depois de habilitar o backup para o aplicativo, todas as partições que pertencem aos serviços confiáveis com estado e ao Reliable Actors nesse aplicativo passarão por backup periódicos de acordo com a política de backup associada.

![Evento de integridade do backup de partição][0]

### <a name="list-backups"></a>Listar Backups

Backups associados a todas as partições que pertencem aos serviços confiáveis com estado e Reliable Actors do aplicativo podem ser enumerados usando a API _GetBackups_. Dependendo do requisito, os backups podem ser enumerados por aplicativo, serviço ou partição.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>PowerShell usando o módulo Microsoft.ServiceFabric.Powershell.Http

```powershell
    Get-SFApplicationBackupList -ApplicationId WordCount     
```

#### <a name="rest-call-using-powershell"></a>Chamada de REST usando o Powershell

Execute o seguinte script do PowerShell para invocar a API HTTP para enumerar os backups criados para todas as partições dentro do aplicativo `SampleApp`.

```powershell
$url = "http://localhost:19080/Applications/SampleApp/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get

$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```

Exemplo de saída da execução acima:

```
BackupId                : d7e4038e-2c46-47c6-9549-10698766e714
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.39.40.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2058
CreationTimeUtc         : 2018-04-01T19:39:40Z
FailureError            : 

BackupId                : 8c21398a-2141-4133-b4d7-e1a35f0d7aac
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 19.54.38.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2237
CreationTimeUtc         : 2018-04-01T19:54:38Z
FailureError            : 

BackupId                : fc75bd4c-798c-4c9a-beee-e725321f73b2
BackupChainId           : d7e4038e-2c46-47c6-9549-10698766e714
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=23aebc1e-e9ea-4e16-9d5c-e91a614fefa7}
BackupLocation          : SampleApp\MyStatefulService\23aebc1e-e9ea-4e16-9d5c-e91a614fefa7\2018-04-01 20.09.44.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131670844862460432; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 2437
CreationTimeUtc         : 2018-04-01T20:09:44Z
FailureError            : 
```

## <a name="limitation-caveats"></a>Limite/Limitações
- Cmdlets do PowerShell do Service Fabric estão no modo de visualização.
- Não há suporte para clusters do Service Fabric no Linux.

## <a name="next-steps"></a>Próximas etapas
- [Noções básicas sobre a configuração de backup periódico](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referência da API REST de backup e restauração](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/PartitionBackedUpHealthEvent.png

