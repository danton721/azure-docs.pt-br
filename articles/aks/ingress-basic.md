---
title: Crie um controlador de entrada básico no AKS (Serviço de Kubernetes do Azure)
description: Aprenda a instalar e configurar um controlador de entrada NGINX básico em um cluster do AKS (Serviço de Kubernetes do Azure).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 3ae7a3193e0a4bacc64524f477b6c179ead20b6b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44354402"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Criar um controlador de entrada no AKS (Serviço de Kubernetes do Azure)

Um controlador de entrada é uma parte do software que fornece proxy reverso, roteamento de tráfego configurável e terminação TLS para serviços de Kubernetes. Os recursos de entrada de Kubernetes são usados para configurar as regras de entrada e as rotas para os serviços de Kubernetes individuais. Usando um controlador de entrada e regras de ingresso, um único endereço IP pode ser usado para rotear tráfego a vários serviços em um cluster de Kubernetes.

Este artigo mostra como implantar o [controlador de ingresso NGINX][nginx-ingress] em um Cluster do Serviço de Kubernetes do Azure (AKS0) Dois aplicativos são então executados no cluster do AKS, cada um dos quais é acessível pelo endereço IP único.

Você também pode:

- [Habilitar o complemento de roteamento de aplicativo HTTP][aks-http-app-routing]
- [Criar um controlador de entrada que usa uma rede privada interna e um endereço IP][aks-ingress-internal]
- [Criar um controlador de entrada com um IP público dinâmico e configurar Vamos Criptografar para gerar certificados TLS automaticamente][aks-ingress-tls]
- [Criar um controlador de entrada com um endereço IP público estático e configurar Vamos Criptografar para gerar certificados TLS automaticamente][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Antes de começar

Este artigo usa o Helm para instalar o NGINX ingress controller, Gerenciador de certificado e um aplicativo web de exemplo. Você precisa ter o Helm inicializado dentro do cluster do AKS e usar uma conta de serviço para Tiller. Para obter mais informações sobre como configurar e usar o Helm, consulte [Instalar aplicativos com Helm no Serviço de Kubernetes do Azure (AKS)][use-helm].

Este artigo exige que você esteja executando a versão 2.0.41 ou posterior da CLI do Azure. Execute `az --version` para encontrar a versão. Se precisar instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Criar um controlador de entrada

Para criar o controlador de entrada, use `Helm` para instalar *nginx-ingress*.

> [!TIP]
> O exemplo a seguir instala o controlador de entrada no namespace `kube-system`. Você poderá especificar um namespace diferente para seu próprio ambiente se desejar. Se o cluster AKS não for habilitado para RBAC, adicione `--set rbac.create=false` aos comandos.

```console
helm install stable/nginx-ingress --namespace kube-system
```

Quando o serviço de balanceador de carga do Kubernetes é criado para o controlador de entrada NGINX, um endereço IP público dinâmico é atribuído, conforme mostrado na seguinte saída de exemplo:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
masked-otter-nginx-ingress-controller        LoadBalancer   10.0.92.99    40.117.74.8   80:31077/TCP,443:32592/TCP   7m
masked-otter-nginx-ingress-default-backend   ClusterIP      10.0.46.106   <none>        80/TCP                       7m
```

Nenhuma regra de ingresso foi criada ainda, portanto, a página 404 padrão do controlador de ingresso NGINX é exibida se você navega para o endereço IP interno. As regras de ingresso são configuradas nas etapas a seguir.

## <a name="run-demo-applications"></a>Executar aplicativos de demonstração

Para ver o controlador de entrada em ação, executaremos dois aplicativos de demonstração no cluster do AKS. Neste exemplo, o Helm é utilizado para executar várias instâncias de um aplicativo simples hello world.

Antes de instalar os gráficos do Helm de exemplo, adicione o repositório de exemplos do Azure para seu ambiente de Helm da seguinte maneira:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Crie o primeiro aplicativo de demonstração de um gráfico do Helm com o seguinte comando:

```console
helm install azure-samples/aks-helloworld
```

Em seguida, instale uma segunda instância do aplicativo demo. Para a segunda instância, especifique um novo título para que os dois aplicativos sejam visualmente distintos. Você também precisa especificar um nome de serviço exclusivo:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Criar rota de entrada

Ambos os aplicativos agora estão em execução no cluster do Kubernetes. Para rotear o tráfego para cada aplicativo, crie um recurso de entrada do Kubernetes. O recurso de entrada configura as regras que roteiam o tráfego para um dos dois aplicativos.

No exemplo a seguir, o tráfego para o endereço `http://40.117.74.8/` é roteado para o serviço nomeado `aks-helloworld`. O tráfego para o endereço `http://40.117.74.8/hello-world-two` é roteado para o serviço `ingress-demo`.

Crie um arquivo chamado `hello-world-ingress.yaml` e copie-o no YAML de exemplo a seguir.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Crie o recurso de entrada usando o `kubectl apply -f hello-world-ingress.yaml` comando.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Testar o controlador de entrada

Para testar as rotas para o controlador de entrada, navegue até os dois aplicativos. Abra um navegador da Web para o endereço IP do controlador de entrada NGINX, tal como *http://40.117.74.8*. O primeiro aplicativo de demonstração é exibido no navegador da Web, conforme mostrado no exemplo a seguir:

![Primeiro aplicativo em execução por trás do controlador de entrada](media/ingress-basic/app-one.png)

Agora, adicione o caminho */hello-world-two* ao endereço IP, assim como *http://40.117.74.8/hello-world-two*. O segundo aplicativo de demonstração com o título personalizado é mostrado:

![Segundo aplicativo em execução por trás do controlador de entrada](media/ingress-basic/app-two.png)

## <a name="next-steps"></a>Próximas etapas

Este artigo incluído alguns componentes externos no AKS. Para saber mais sobre esses componentes, consulte as seguintes páginas do projeto:

- [CLI do Helm][helm-cli]
- [Controlador de entrada NGINX ][nginx-ingress]

Você também pode:

- [Habilitar o complemento de roteamento de aplicativo HTTP][aks-http-app-routing]
- [Criar um controlador de entrada que usa uma rede privada interna e um endereço IP][aks-ingress-internal]
- [Criar um controlador de entrada com um IP público dinâmico e configurar Vamos Criptografar para gerar certificados TLS automaticamente][aks-ingress-tls]
- [Criar um controlador de entrada com um endereço IP público estático e configurar Vamos Criptografar para gerar certificados TLS automaticamente][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md