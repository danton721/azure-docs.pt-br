---
title: Implantar contêineres com Helm no Kubernetes no Azure
description: Saiba como usar a ferramenta de empacotamento Helm para implantar contêineres em um cluster do serviço de Kubernetes do Azure (AKS)
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 05/23/2019
ms.author: zarhoads
ms.openlocfilehash: 76a5391cbe142851d9b1f60ea9346af2e7a35d6a
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392134"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Instalar aplicativos com o Helm no AKS (Serviço de Kubernetes do Azure)

[Helm][helm] é uma ferramenta de empacotamento de software livre que ajuda a instalar e gerenciar o ciclo de vida de aplicativos Kubernetes. Semelhante a gerenciadores de pacotes do Linux, como *APT* e *Yum*, o Helm é usado para gerenciar gráficos Kubernetes, que são pacotes de recursos de Kubernetes pré-configurados.

Este artigo mostra como configurar e usar o Helm em um cluster Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar

Este artigo considera que já existe um cluster do AKS. Se você precisar de um cluster do AKS, confira o guia de início rápido do AKS [Usando a CLI do Azure][aks-quickstart-cli] ou [Usando o portal do Azure][aks-quickstart-portal].

Você também precisa instalar, a CLI do Helm que é o cliente que é executado no sistema de desenvolvimento. Ele permite que você iniciar, parar e gerenciar aplicativos com Helm. Se você usa o Azure Cloud Shell, o a CLI do Helm já está instalada. Para obter instruções de instalação em sua plataforma local, consulte [instalação do Helm][helm-install].

> [!IMPORTANT]
> Helm destina-se para ser executado em nós do Linux. Se você tiver nós do Windows Server no cluster, você deve garantir que os pods Helm só estão agendados para execução em nós do Linux. Você também precisa garantir que os gráficos do Helm que instalar também estão agendados para execução em nós corretos. Os comandos neste artigo usam [nó Seletores] [ k8s-node-selector] para certificar-se de pods estão agendados para os nós corretos, mas nem todos os gráficos do Helm podem expor um seletor de nó. Você também pode considerar usar outras opções no cluster, tais como [taints][taints].

## <a name="create-a-service-account"></a>Criar uma conta de serviço

Antes de implantar o Helm em um cluster AKS habilitado para RBAC, você precisa de uma conta de serviço e ligação de função para o serviço do Tiller. Para obter mais informações sobre como proteger o Helm / cluster habilitado para o Tiller em um RBAC, consulte [Tiller, Namespaces e RBAC][tiller-rbac]. Se seu cluster do AKS não for habilitado o RBAC, ignore esta etapa.

Crie um arquivo chamado `helm-rbac.yaml` e o copie no YAML a seguir:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Crie a conta de serviço e a associação de funções com o comando `kubectl apply`:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Proteger o Tiller e o Helm

O cliente do Helm e o serviço do Tiller são autenticados e se comunicam entre si usando TLS/SSL. Esse método de autenticação ajuda a proteger o cluster Kubernetes e quais serviços podem ser implantados. Para aumentar a segurança, você pode gerar seus próprios certificados autoassinados. Cada usuário do Helm receberia seu próprio certificado do cliente e o Tiller seria inicializado no cluster Kubernetes com os certificados aplicados. Para obter mais informações, consulte [Usando TLS/SSL entre o Helm e o Tiller][helm-ssl].

Ao usar um cluster Kubernetes habilitado para RBAC, você pode controlar o nível de acesso que o Tiller tem ao cluster. Você pode definir o namespace do Kubernetes em que o Tiller é implantado e pode restringir em quais namespaces o Tiller pode implantar recursos. Essa abordagem permite criar instâncias do Tiller em namespaces diferentes, definir limites de implantação e definir o escopo dos usuários do cliente do Helm a determinados namespaces. Para obter mais informações, confira [Controles de acesso baseados em função do Helm][helm-rbac].

## <a name="configure-helm"></a>Configurar o Helm

Para implantar um Tiller básico em um cluster do AKS, use o comando [helm init][helm-init]. Se o cluster não está habilitado o RBAC, remova o `--service-account` argumento e o valor. Se você configurou TLS/SSL para o Tiller e o Helm, ignore esta etapa de inicialização básica e, em vez disso, forneça o `--tiller-tls-` necessário conforme mostrado no exemplo a seguir.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

Se você configurou TLS/SSL entre o Helm e o Tiller, forneça os parâmetros de `--tiller-tls-*` e os nomes de seus próprios certificados, conforme mostrado no exemplo a seguir:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller \
    --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="find-helm-charts"></a>Localizar gráficos Helm

Os gráficos do Helm são usados para implantar aplicativos em um cluster Kubernetes. Para procurar gráficos do Helm pré-criados, use o comando [helm search][helm-search]:

```console
helm search
```

A saída de exemplo condensada a seguir mostra alguns dos gráficos do Helm disponíveis para uso:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Para atualizar a lista de gráficos, use o comando [helm repo update][helm-repo-update]. O exemplo a seguir mostra uma atualização de repositório bem-sucedida:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Executar gráficos Helm

Para instalar gráficos com o Helm, use o comando [helm install][helm-install] e especifique o nome do gráfico a ser instalado. Para ver a instalação de um gráfico do Helm em ação, vamos instalar uma implantação de nginx básico usando um gráfico do Helm. Se tiver configurado TLS/SSL, adicione o parâmetro `--tls` para usar seu certificado de cliente do Helm.

```console
helm install stable/nginx-ingress \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

A saída do exemplo condensado a seguir mostra o status de implantação dos recursos do Kubernetes criados pelo gráfico do Helm:

```
$ helm install stable/nginx-ingress --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux

NAME:   flailing-alpaca
LAST DEPLOYED: Thu May 23 12:55:21 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                      DATA  AGE
flailing-alpaca-nginx-ingress-controller  1     0s

==> v1/Pod(related)
NAME                                                            READY  STATUS             RESTARTS  AGE
flailing-alpaca-nginx-ingress-controller-56666dfd9f-bq4cl       0/1    ContainerCreating  0         0s
flailing-alpaca-nginx-ingress-default-backend-66bc89dc44-m87bp  0/1    ContainerCreating  0         0s

==> v1/Service
NAME                                           TYPE          CLUSTER-IP  EXTERNAL-IP  PORT(S)                     AGE
flailing-alpaca-nginx-ingress-controller       LoadBalancer  10.0.109.7  <pending>    80:31219/TCP,443:32421/TCP  0s
flailing-alpaca-nginx-ingress-default-backend  ClusterIP     10.0.44.97  <none>       80/TCP                      0s
...
```

Leva um minuto ou dois para o *EXTERNAL-IP* endereço do serviço de controlador de ingresso nginx para ser preenchido e permitem que você acessá-lo com um navegador da web.

## <a name="list-helm-releases"></a>Listar versões do Helm

Para ver uma lista de versões instaladas no seu cluster, use o comando [helm list][helm-list]. O exemplo a seguir mostra a versão de entrada nginx implantada na etapa anterior. Se tiver configurado TLS/SSL, adicione o parâmetro `--tls` para usar seu certificado de cliente do Helm.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                 APP VERSION   NAMESPACE
flailing-alpaca   1         Thu May 23 12:55:21 2019    DEPLOYED    nginx-ingress-1.6.13    0.24.1      default
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando você implanta um gráfico Helm, vários recursos do Kubernetes são criados. Esses recursos incluem pods, implantações e serviços. Para limpar esses recursos, use o comando `helm delete` e especifique o nome do release, conforme encontrado no comando `helm list` anterior. O exemplo a seguir exclui a versão nomeada *alpaca flailing*:

```console
$ helm delete flailing-alpaca

release "flailing-alpaca" deleted
```

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como gerenciar implantações de aplicativo do Kubernetes usando o Helm, consulte a documentação do Helm.

> [!div class="nextstepaction"]
> [Documentação do Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
[taints]: operator-best-practices-advanced-scheduler.md