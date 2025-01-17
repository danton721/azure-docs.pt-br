---
title: Uso do Azure Data Box, pesada de caixa de dados do Azure para enviar dados para quente, frio, arquivar a camada de blob | Microsoft Docs nos dados
description: Descreve como usar o Azure Data Box ou pesada de caixa de dados do Azure para enviar dados para uma camada de armazenamento de blob de bloco apropriado, como quente, frio ou de arquivo
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 05/24/2019
ms.author: alkohli
ms.openlocfilehash: ea208c395e2ef69ce8f28052351643e963cceb05
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427882"
---
# <a name="use-azure-data-box-or-azure-data-box-heavy-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Usar o Azure Data Box ou pesada de caixa de dados do Azure para enviar dados ao nível adequado de blob de armazenamento do Azure

O Azure Data Box move grandes quantidades de dados para o Azure enviando a você um dispositivo de armazenamento proprietário. Você preenche o dispositivo com os dados e retorna-o. Os dados do Data Box são carregados para uma camada padrão associada à conta de armazenamento. Então você pode mover os dados para outra camada de armazenamento.

Este artigo descreve como os dados são carregados pelo Data Box podem ser movidos para uma camada de blob quente, fria ou de arquivos. Este artigo se aplica ao Azure Data Box e pesada de caixa de dados do Azure.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Escolher a camada de armazenamento correta para seus dados

O armazenamento do Azure permite três níveis diferentes para armazenar os dados da maneira mais econômica – quente, frio ou arquivos. A camada de armazenamento quente é otimizada para armazenar dados acessados com frequência. O armazenamento a quente tem custos de armazenamento mais altos do que o armazenamento Cool e Archive, mas os custos de acesso mais baixos.

A camada de armazenamento frio é para dados acessados com pouca frequência que precisam ser armazenados por pelo menos 30 dias. O custo de armazenamento para a camada fria é inferior ao da camada de armazenamento quente, mas os encargos de acesso a dados são altos em comparação aos da camada Quente.

A camada de Arquivos do Azure fica offline e oferece os custos de armazenamento mais baixos, mas também os custos de acesso mais altos. Essa camada destina-se a dados que permanecem no armazenamento de arquivos por pelo menos 180 dias. Para obter detalhes sobre cada uma dessas camadas e o modelo de preços, acesse [Comparação das camadas de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

Os dados da caixa de dados ou dados caixa pesado é carregado em uma camada de armazenamento que está associada com a conta de armazenamento. Quando você cria uma conta de armazenamento, pode especificar a camada de acesso como quente ou fria. Dependendo do padrão de acesso de sua carga de trabalho e do custo, você poderá mover esses dados da camada padrão para outra camada de armazenamento.

Você só pode ordenar os dados de armazenamento de objetos nas contas de Armazenamento de Blobs ou GPv2 (Uso Geral v2). Contas de Uso geral v1 (GPv1) não dão suporte a camadas. Para escolher a camada de armazenamento correta para seus dados, examine as considerações detalhadas em [Armazenamento de Blobs do Azure: camadas de armazenamento Premium, quente, fria e de arquivos](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Defina uma camada de blob padrão

A camada de blob padrão é especificada quando a conta de armazenamento é criada no portal do Azure. Depois que um tipo de armazenamento é selecionado como o Armazenamento de Blobs ou GPv2, o atributo da camada de acesso pode ser especificado. Por padrão, a camada Quente é selecionada.

As camadas não podem ser especificado se você está tentando criar uma nova conta ao ordenar uma caixa de dados ou dados caixa pesada. Depois que a conta for criada, você poderá modificar a conta no portal para definir a camada de acesso padrão.

Como alternativa, você cria uma conta de armazenamento primeiro com o atributo da camada de acesso especificado. Ao criar a ordem de caixa de dados ou dados caixa pesada, selecione a conta de armazenamento existente. Para obter mais informações sobre como definir a camada de blob padrão durante a criação da conta de armazenamento, confira [Criar uma conta de armazenamento no portal do Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Mover dados para uma camada não padrão

Depois que os dados do dispositivo Data Box são carregados para a camada padrão, você talvez queira mover os dados para uma camada diferente do padrão. Há duas maneiras de mover esses dados para uma camada não padrão.

- **Gerenciamento de ciclo de vida do Armazenamento de Blobs do Azure** – você pode usar uma abordagem baseada em política para automaticamente colocar dados em camadas ou expirá-los no final do ciclo de vida. Para obter mais informações, confira [Gerenciando o ciclo de vida do Armazenamento de Blobs do Azure](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Script** – você pode usar uma abordagem com script por meio do Azure PowerShell para habilitar a camada no nível do blob. Você pode chamar a operação `SetBlobTier` para definir a camada no blob.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Usar o Azure PowerShell para definir a camada de blob

As etapas a seguir descrevem como você pode definir a camada de blob como Arquivos usando um script do Azure PowerShell.

1. Abra uma sessão elevada do Windows PowerShell. Verifique se você está executando o PowerShell 5.0 ou posterior. Digite:

   `$PSVersionTable.PSVersion`     

2. Entre no Azure PowerShell. 

   `Login-AzAccount`  

3. Defina as variáveis para a conta de armazenamento, a chave de acesso, o contêiner e o contexto de armazenamento.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Obtenha todos os blobs no contêiner.

    `$blobs = Get-AzStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Defina a camada de todos os blobs no contêiner como Arquivos.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Um exemplo de saída é mostrado abaixo:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Se você desejar que os dados sejam arquivados na ingestão, defina a camada de conta padrão como quente. Se a camada padrão for Fria, haverá uma penalidade de exclusão antecipada de 30 dias se os dados forem movidos para Arquivos imediatamente.

## <a name="next-steps"></a>Próximas etapas

-  Aprenda a lidar com [cenários de camadas de dados comuns com as regras de política de ciclo de vida](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples)

