---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 841027fe8d6b97e661faa038dc9381edbb3d4cd8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226022"
---
## <a name="before-you-begin"></a>Antes de começar

Para concluir o exemplo neste artigo, você precisa ter uma imagem gerenciada existente de uma VM generalizada. Para obter mais informações, confira [Tutorial: Criar uma imagem personalizada de uma VM do Azure com a CLI do Azure 2.0](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Se a imagem gerenciada contém um disco de dados, o tamanho do disco de dados não pode ser mais de 1 TB.

## <a name="launch-azure-cloud-shell"></a>Iniciar o Azure Cloud Shell

O Azure Cloud Shell é um shell interativo grátis que pode ser usado para executar as etapas neste artigo. Ele tem ferramentas do Azure instaladas e configuradas para usar com sua conta. 

Para abrir o Cloud Shell, basta selecionar **Experimentar** no canto superior direito de um bloco de código. Você também pode iniciar o Cloud Shell em uma guia separada do navegador indo até [https://shell.azure.com/bash](https://shell.azure.com/bash). Selecione **Copiar** para copiar os blocos de código, cole o código no Cloud Shell e depois pressione Enter para executá-lo.

Se você preferir instalar e usar a CLI localmente, consulte [instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-an-image-gallery"></a>Criar uma galeria de imagens 

Uma galeria de imagens é o principal recurso usado para habilitar o compartilhamento de imagens. Caracteres permitidos para o nome da galeria são letras maiúsculas ou minúsculas, dígitos, pontos e pontos finais. O nome da Galeria não pode conter traços.   Os nomes das galerias devem ser exclusivos dentro de sua assinatura. 

Criar uma galeria de imagens usando [az sig create](/cli/azure/sig#az-sig-create). O exemplo a seguir cria uma galeria chamada *myGallery* em *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Criar uma definição de imagem

Definições de imagem criar um agrupamento lógico de imagens. Eles são usados para gerenciar informações sobre as versões de imagem que são criados dentro deles. Nomes de definição de imagem podem ser compostos por letras maiusculas ou minúsculas, dígitos, pontos, traços e períodos. Para obter mais informações sobre os valores que você pode especificar para uma definição de imagem, consulte [definições de imagem](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Crie uma definição de imagem inicial na galeria usando [az sig image-definition create](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```


## <a name="create-an-image-version"></a>Criar uma versão de imagem 

Crie versões da imagem conforme necessário usando [az image gallery create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create). Você precisará passar a ID da imagem gerenciada para usar como uma linha de base para criar a versão da imagem. Você pode usar [az image list](/cli/azure/image?view#az-image-list) para obter informações sobre as imagens que estão em um grupo de recursos. 

Caracteres permitidos para a versão da imagem são números e pontos. Os números devem estar dentro do intervalo de um inteiro de 32 bits. Formato: *MajorVersion*. *MinorVersion*. *Patch*.

Neste exemplo, é a versão da nossa imagem *1.0.0* e vamos criar 2 réplicas na *Centro-Oeste dos EUA* região, 1 réplica no *Centro-Sul dos EUA* região e 1 réplica na *Leste dos EUA 2* região usando o armazenamento com redundância de zona.


```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1=Standard_ZRS" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> Você precisa esperar para a versão da imagem concluir completamente que está sendo criado e replicadas antes de você pode usar a mesma imagem gerenciada para criar outra versão da imagem.
>
> Você também pode armazenar todas as suas réplicas de versão de imagem na [armazenamento com redundância de zona](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) adicionando `--storage-account-type standard_zrs` quando você cria a versão da imagem.
>

## <a name="share-the-gallery"></a>Compartilhar a Galeria

É recomendável que você compartilha com outros usuários no nível da Galeria. Para obter a ID de objeto da sua galeria, use [show do az sig](/cli/azure/sig#az-sig-show).

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Use a ID de objeto como um escopo, junto com um endereço de email e [atribuição de função az criar](/cli/azure/role/assignment#az-role-assignment-create) para conceder acesso de usuário a Galeria de imagens compartilhadas.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```


