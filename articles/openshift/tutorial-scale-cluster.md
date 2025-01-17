---
title: Tutorial – Dimensionar um cluster do Red Hat OpenShift no Azure | Microsoft Docs
description: Saiba como dimensionar um cluster da Microsoft no Red Hat OpenShift no Azure usando a CLI do Azure
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.topic: tutorial
ms.service: openshift
ms.date: 05/06/2019
ms.openlocfilehash: b25e17e7064006a1421142dfcd32997cb4426e8e
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66305978"
---
# <a name="tutorial-scale-an-azure-red-hat-openshift-cluster"></a>Tutorial: Dimensionar um cluster do Red Hat OpenShift no Azure

Este tutorial é a parte dois de uma série. Você aprenderá a criar um cluster da Microsoft no Red Hat OpenShift no Azure usando a CLI do Azure, dimensioná-lo e, em seguida, excluí-lo para limpar os recursos.

Na segunda parte da série, você aprenderá como:

> [!div class="checklist"]
> * Dimensionar um cluster do Red Hat OpenShift

Nesta série de tutoriais, você aprenderá a:
> [!div class="checklist"]
> * [Criar um cluster do Red Hat OpenShift no Azure](tutorial-create-cluster.md)
> * Dimensionar um cluster do Red Hat OpenShift no Azure
> * [Excluir um cluster do Red Hat OpenShift no Azure](tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Crie um cluster, seguindo o tutorial [Criar um cluster do Red Hat OpenShift no Azure](tutorial-create-cluster.md).

## <a name="step-1-sign-in-to-azure"></a>Etapa 1: Entrar no Azure

Se você estiver executando a CLI do Azure localmente, execute `az login` para entrar no Azure.

```bash
az login
```

Se você tiver acesso a várias assinaturas, execute `az account set -s {subscription ID}` substituindo `{subscription ID}` com a assinatura que você deseja usar.

## <a name="step-2-scale-the-cluster-with-additional-nodes"></a>Etapa 2: Dimensionar o cluster com nós adicionais

Em um terminal Bash, defina a variável CLUSTER_NAME com o nome do seu cluster:

```bash
CLUSTER_NAME=yourclustername
```

Agora vamos dimensionar o cluster com cinco nós usando a CLI do Azure:

```bash
az openshift scale --resource-group $CLUSTER_NAME --name $CLUSTER_NAME --compute-count 5
```

Depois de alguns minutos, `az openshift scale` será concluído com êxito e retornará um documento JSON contendo os detalhes do cluster dimensionado.

## <a name="next-steps"></a>Próximas etapas

Nesta parte do tutorial, você aprendeu a:

> [!div class="checklist"]
> * Dimensionar um cluster do Red Hat OpenShift no Azure

Prosseguir para o próximo tutorial:
> [!div class="nextstepaction"]
> [Excluir um cluster do Red Hat OpenShift no Azure](tutorial-delete-cluster.md)