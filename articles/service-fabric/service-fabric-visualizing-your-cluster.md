---
title: Visualizar seu cluster usando o Azure Service Fabric Explorer | Microsoft Docs
description: O Azure Service Fabric Explorer é um aplicativo para inspecionar e gerenciar aplicativos em nuvem e nós em um cluster do Microsoft Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2019
ms.author: mikhegn
ms.openlocfilehash: cbeb0a8944b35af2a68080a2b2f1bb4436c6e58d
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306690"
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualizando o cluster com o Service Fabric Explorer

Service Fabric Explorer (SFX) é uma ferramenta de código-fonte aberto para inspecionar e gerenciar clusters do Azure Service Fabric. O Service Fabric Explorer é um aplicativo da área de trabalho para Windows, macOS e Linux.

## <a name="service-fabric-explorer-download"></a>Download do Service Fabric Explorer

Use os links a seguir para fazer o download do Service Fabric Explorer como um aplicativo de área de trabalho:

-  Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> A versão da área de trabalho do Service Fabric Explorer pode ter mais ou menos recursos ao qual o cluster dá suporte. Você pode voltar para a versão do Service Fabric Explorer implantada para o cluster para garantir a compatibilidade completa do recurso.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Executando o Service Fabric Explorer pelo cluster

O Service Fabric Explorer também está hospedado em um ponto de extremidade de gerenciamento de HTTP de um cluster do Service Fabric. Para iniciar SFX em um navegador da web, navegue até o ponto de extremidade de gerenciamento de HTTP do cluster em qualquer navegador – por exemplo, https:\//clusterFQDN:19080.

Para a instalação da estação de trabalho de desenvolvedor, você pode iniciar o Service Fabric Explorer no cluster local, navegando até https://localhost:19080/Explorer. Leia este artigo para [preparar seu ambiente de desenvolvimento](service-fabric-get-started.md).

> [!NOTE]
> Se seu cluster for protegido por um certificado autoassinado, você receberá uma mensagem de erro do navegador da Web: "Este site não é seguro". Você poderá simplesmente prosseguir por meio de navegadores mais modernos, ignorando o aviso. Em um ambiente de produção, seu cluster deve ser protegido usando o nome comum e um certificado emitido por uma autoridade de certificação. 
>
>

## <a name="connect-to-a-service-fabric-cluster"></a>Conectar a um cluster do Service Fabric
Para se conectar a um cluster do Service Fabric, você precisará do ponto de extremidade de gerenciamento de clusters (IP/FQDN) e a porta do ponto de extremidade de gerenciamento de HTTP (19080 por padrão). Por exemplo, https\:/ / mysfcluster.westus.cloudapp.azure.com:19080. Use a caixa de seleção "Conectar ao localhost" para se conectar a um cluster local em sua estação de trabalho.

### <a name="connect-to-a-secure-cluster"></a>Conectar a um cluster seguro
Você pode controlar o acesso do cliente ao cluster do Service Fabric com certificados ou usando o AAD (Azure Active Directory).

Se você tentar se conectar a um cluster seguro, em seguida, dependendo da configuração do cluster você precisará apresentar um certificado de cliente ou entrar usando o AAD.

## <a name="understand-the-service-fabric-explorer-layout"></a>Entender o layout do Explorador do Service Fabric
Você pode navegar pelo Explorador do Service Fabric usando a árvore à esquerda. Na raiz da árvore, o painel do cluster fornece uma visão geral do cluster, incluindo um resumo do aplicativo e a integridade do nó.

![Painel de clusters do Explorador do Service Fabric][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Exibir o layout do cluster
Nós em um cluster do Service Fabric são colocados em uma grade bidimensional de domínios de falha e domínios de atualização. Esse posicionamento garante que seus aplicativos permaneçam disponíveis na presença de falhas de hardware e atualizações de aplicativos. Você pode exibir o layout do cluster atual usando o mapa de clusters.

![Mapa de clusters do Explorador do Service Fabric][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Exibir aplicativos e serviços
O cluster contém duas subárvores: uma para aplicativos e outra para nós.

Você pode usar a exibição de aplicativos para navegar pela hierarquia lógica do Service Fabric: aplicativos, serviços, partições e réplicas.

No exemplo abaixo, o aplicativo **MyApp** é composto por dois serviços, **MyStatefulService** e **WebService**. Como o **MyStatefulService** tem monitoração de estado, ele inclui uma partição com uma réplica principal e duas secundárias. Por outro lado, o WebSvcService é sem monitoração de estado e contém uma única instância.

![Exibição do aplicativo do Explorador do Service Fabric][sfx-application-tree]

Em cada nível da árvore, o painel principal mostra informações pertinentes sobre o item. Por exemplo, você pode ver o status de integridade e a versão de um serviço específico.

![Painel essencial do Explorador do Service Fabric][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Exibir os nós do cluster
A exibição de nós mostra o layout físico do cluster. Para um nó específico, você pode inspecionar quais aplicativos têm código implantado naquele nó. Mais especificamente, você pode ver quais réplicas estão sendo executadas lá atualmente.

## <a name="actions"></a>Ações
O Explorador do Service Fabric oferece uma maneira rápida de invocar ações em nós, aplicativos e serviços no cluster.

Por exemplo, para excluir uma instância do aplicativo, escolha o aplicativo na árvore à esquerda e escolha **Ações** > **Excluir Aplicativo**.

![Excluir um aplicativo no Explorador do Service Fabric][sfx-delete-application]

> [!TIP]
> Você pode executar as mesmas ações clicando nas reticências ao lado de cada elemento.
>
> Todas as ações que podem ser executadas usando o Explorador do Service Fabric também podem ser executadas usando o PowerShell ou uma API REST para habilitar a automação.
>
>

Você também pode usar o Service Fabric Explorer para criar instâncias de aplicativo para um determinado tipo e versão de aplicativo. Escolha o tipo de aplicativo no modo de exibição de árvore, clique no link **Create app instance (Criar instância de aplicativo)** ao lado da versão desejada no painel da direita.

![Criar uma instância de aplicativo no Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> O Service Fabric Explorer não oferece suporte a parâmetros durante a criação de instâncias do aplicativo. As instâncias do aplicativo usam valores de parâmetro padrão.
>
>

## <a name="event-store"></a>Armazenamento de Eventos
O EventStore é um recurso oferecido pela plataforma que fornece eventos de plataforma do Service Fabric disponíveis no Service Fabric Explorer e por meio da API REST. Você pode ver uma exibição de instantâneo do que está acontecendo em seu cluster para cada entidade, por exemplo, nó, serviço, aplicativo e consulta com base na hora do evento. Você também pode ler mais sobre o EventStore na [Visão geral do EventStore](service-fabric-diagnostics-eventstore.md).   

![EventStore][sfx-eventstore]

>[!NOTE]
>A partir do Service Fabric 6.4. O EventStore não está habilitado por padrão e deve ser habilitado no modelo do Resource Manager

>[!NOTE]
>A partir do Service Fabric 6.4. as APIs do EventStore estão apenas disponíveis para clusters do Windows somente na execução no Azure. Estamos trabalhando mover essa funcionalidade para Linux, assim como para nossos clusters independentes.


## <a name="next-steps"></a>Próximas etapas
* [Gerenciando aplicativos da Malha do Serviço no Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implantação de aplicativos de Malha do Serviço usando o PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
[sfx-eventstore]: ./media/service-fabric-diagnostics-eventstore/eventstore.png
