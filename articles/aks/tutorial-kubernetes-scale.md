---
title: Tutorial do Kubernetes no Azure - Dimensionar Aplicativo
description: Neste tutorial do AKS (Serviço de Kubernetes do Azure), você aprenderá a dimensionar nós e pods no Kubernetes e a implementar o dimensionamento automático horizontal de pods.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: twhitney
ms.custom: mvc
ms.openlocfilehash: 062e16c0d196cf91d6e0adde46ed973f1c0d1191
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304436"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Dimensionar aplicativos no AKS (Serviço de Kubernetes do Azure)

Se você tiver seguido os tutoriais, terá um cluster de Kubernetes operacional no AKS e já implantou o aplicativo Azure Voting de exemplo. Neste tutorial, parte cinco de sete, você escala horizontalmente os pods no aplicativo e experimenta o dimensionamento automático do pod. Você também aprenderá como dimensionar o número de nós da VM do Azure para alterar a capacidade do cluster para hospedagem de cargas de trabalho. Você aprenderá como:

> [!div class="checklist"]
> * Dimensionar os nós Kubernetes
> * Dimensionar pods Kubernetes manualmente que executam seu aplicativo
> * Configurar os pods de dimensionamento automático que executam o front-end do aplicativo

Nos tutoriais adicionais, o aplicativo Azure Vote é atualizado para uma nova versão.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, um aplicativo foi empacotado em uma imagem de contêiner. Essa imagem foi carregada no Registro de Contêiner do Azure e você criou um cluster do AKS. O aplicativo foi então implantado no cluster do AKS. Se você ainda não executou essas etapas e deseja continuar acompanhando, comece com o [Tutorial 1 – Criar imagens de contêiner][aks-tutorial-prepare-app].

Este tutorial exige a execução da CLI do Azure versão 2.0.53 ou posterior. Execute `az --version` para encontrar a versão. Se precisar instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

## <a name="manually-scale-pods"></a>Dimensionar pods manualmente

Quando a instância do Redis e o front-end do Azure Vote foram implantados nos tutoriais anteriores, uma única réplica foi criada. Para ver o número e o estado de pods em seu cluster, use o comando [kubectl get][kubectl-get] da seguinte maneira:

```console
kubectl get pods
```

A saída de exemplo a seguir mostra um pod de front-end e um pod de back-end:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Para alterar manualmente o número de pods na implantação *azure-vote-front*, use o comando [kubectl scale][kubectl-scale]. O exemplo a seguir aumenta o número de pods de front-end *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Execute [kubectl get pods][kubectl-get] novamente para verificar se o AKS cria os pods adicionais. Após aproximadamente um minuto, os pods adicionais estão disponíveis em seu cluster:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Dimensionamento automático de pods

O Kubernetes dá suporte ao [dimensionamento automático horizontal de pods][kubernetes-hpa] para ajustar o número de pods em uma implantação, dependendo da utilização da CPU ou de outras métricas selecionadas. O [Servidor de Métricas][metrics-server] é usado para fornecer a utilização de recursos para Kubernetes e é implantado automaticamente em clusters AKS versões 1.10 e superior. Para ver a versão do cluster do AKS, use o comando [az aks show][az-aks-show] conforme mostrado no exemplo a seguir:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion
```

Se o cluster do AKS for menor do que *1.10*, instale o Servidor de Métricas, caso contrário, ignore esta etapa. Para instalar, clone o repositório do GitHub `metrics-server` e instale as definições de recurso de exemplo. Para exibir o conteúdo dessas definições YAML, veja [Servidor de métricas para Kuberenetes 1.8 +][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Para usar o dimensionador automático, todos os contêineres em seus pods devem ter limites e solicitações de CPU definidos. Na implantação, `azure-vote-front` o contêiner de front-end já solicita 0,25 CPU, com um limite de 0,5 CPU. Essas solicitações e limites de recursos são definidos como mostrado no snippet de exemplo a seguir:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

O exemplo a seguir usa o comando [kubectl autoscale][kubectl-autoscale] para dimensionar automaticamente o número de pods na implantação *azure-vote-front*. Se a utilização da CPU exceder 50%, o dimensionador automático aumentará os pods até um máximo de *10* instâncias. Um mínimo de *3* instâncias então é definido para a implantação:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Para ver o status do dimensionador automático, use o comando `kubectl get hpa` da seguinte forma:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Após alguns minutos, com carga mínima no aplicativo Azure Vote, o número de réplicas de pod diminui automaticamente para três. Você pode usar `kubectl get pods` novamente para ver os pods desnecessários que está sendo removidos.

## <a name="manually-scale-aks-nodes"></a>Dimensionar nós do AKS manualmente

Se você criou o cluster Kubernetes usando os comandos no tutorial anterior, ele tem um nó. Você pode ajustar o número de nós manualmente se planejar ter mais ou menos cargas de trabalho de contêiner no cluster.

O exemplo a seguir aumenta o número de nós para três no cluster Kubernetes chamado *myAKSCluster*. Esse comando leva alguns minutos para ser concluído.

```azurecli
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

Quando o cluster tiver sido dimensionado com êxito, a saída será semelhante ao exemplo a seguir:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você usou diferentes recursos de colocação em escala em seu cluster Kubernetes. Você aprendeu como:

> [!div class="checklist"]
> * Dimensionar pods Kubernetes manualmente que executam seu aplicativo
> * Configurar os pods de dimensionamento automático que executam o front-end do aplicativo
> * Dimensionar manualmente os nós de Kubernetes

Avance para o próximo tutorial para saber mais sobre como atualizar um aplicativo no Kubernetes.

> [!div class="nextstepaction"]
> [Atualizar um aplicativo no Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://v1-12.docs.kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
