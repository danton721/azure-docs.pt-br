---
title: Configurar um ambiente de desenvolvimento do Windows para compilar aplicativos de Malha do Service Fabric | Microsoft Docs
description: Configure seu ambiente de desenvolvimento do Windows para que você possa criar um aplicativo de malha do Service Fabric e implante-o na malha do Azure Service Fabric.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: 5ab817c65ab562f37b456cc3589624c1876084f0
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428206"
---
# <a name="set-up-your-windows-development-environment-to-build-service-fabric-mesh-apps"></a>Configurar seu ambiente de desenvolvimento do Windows para compilar aplicativos de Malha do Service Fabric

Para compilar e executar os aplicativos da Malha do Azure Service Fabric em seu computador de desenvolvimento do Windows, você precisará de:

* Docker
* Visual Studio 2017 ou posterior
* Tempo de execução da Malha do Service Fabric
* Ferramentas e SDK da Malha do Service Fabric.

E uma das seguintes versões do Windows:

* Windows 10 (Enterprise, Professional ou Education) versões 1709 (Fall Creators Update) ou 1803 (Atualização de abril de 2018 para o Windows 10)
* Windows Server versão 1709
* Windows Server versão 1803

A seguir será de instruções ajuda você a instalar tudo com base na versão do Windows que você está executando.

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="visual-studio"></a>Visual Studio

Visual Studio 2017 ou posterior é necessário para implantar aplicativos de malha do serviço Fabric. [Instale a versão 15.6.0][download-visual-studio] ou uma versão posterior e habilite as cargas de trabalho a seguir:

* Desenvolvimento do ASP.NET e para a Web
* Desenvolvimento do Azure

## <a name="install-docker"></a>Instalar o Docker

Se você já tem o Docker instalado, verifique se você tem a versão mais recente. Docker pode solicitar que você quando uma nova versão for lançada, mas verifique manualmente se você tiver a versão mais recente.

#### <a name="install-docker-on-windows-10"></a>Instalar o Docker no Windows 10

Baixe e instale a versão mais recente do [Docker Community Edition para Windows][download-docker] para dar suporte a aplicativos do Service Fabric em contêineres usados pela Malha do Service Fabric.

Durante a instalação, selecione **Usar contêineres do Windows em vez de contêineres do Linux** quando for solicitado.

Se o Hyper-V não está habilitado em seu computador, o instalador do Docker oferecerá para habilitá-lo. Clique em **OK** para fazê-lo se solicitado.

#### <a name="install-docker-on-windows-server-2016"></a>Instalar o Docker no Windows Server 2016

Se a função do Hyper-V não estiver habilitada, abra o PowerShell como administrador e execute o seguinte comando para instalar o Hyper-V e reinicie o computador. Para saber mais, confira a [Edição Docker Enterprise para Windows Server][download-docker-server].

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
```

Reinicie seu computador.

Abra o PowerShell como um administrador e execute os seguintes comandos para instalar o Docker:

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

## <a name="sdk-and-tools"></a>SDK e ferramentas

Instale o tempo de execução da Malha do Service Fabric, o SDK e as ferramentas na ordem a seguir.

1. Instale o [SDK da malha do Service Fabric][download-sdkmesh] usando o Web Platform Installer. Isso instalará também o tempo de execução e o SDK do Microsoft Azure Service Fabric.
2. Instale a [extensão de Ferramentas da Malha do Service Fabric do Visual Studio (versão prévia)][download-tools] do Visual Studio Marketplace.

## <a name="build-a-cluster"></a>Criar um cluster

> [!IMPORTANT]
> O Docker **deve** estar em execução antes de um cluster ser compilado.
> Teste se o Docker está em execução abrindo uma janela de terminal e executando `docker ps` para ver se ocorre um erro. Se a resposta não indicar um erro, o Docker está em execução e você está pronto para compilar um cluster.

> [!Note]
> Se estiver desenvolvendo em um computador com a atualização Windows Fall Creators Update (versão 1709), você só poderá usar imagens do docker do Windows versão 1709.
> Se estiver desenvolvendo no computador com a atualização de 10 de abril de 2018 do Windows (versão 1803), você poderá usar imagens do Docker do Windows versão 1709 ou 1803.

Se você estiver usando o Visual Studio, você pode ignorar esta seção porque o Visual Studio criará um cluster local para você se você não tiver uma.

Para melhor desempenho de depuração quando você está criando e executando um único aplicativo do Service Fabric por vez, crie um cluster de nó único de desenvolvimento local. Se você estiver executando vários aplicativos por vez, crie um cluster de cinco nós de desenvolvimento local. O cluster precisa estar em execução sempre que você implanta ou depura um projeto de Malha do Service Fabric.

Depois de instalar o tempo de execução, os SDKs, as ferramentas do Visual Studio, Docker e estar com o Docker funcionando, crie um cluster de desenvolvimento.

1. Feche a janela do PowerShell.
2. Abra uma janela nova, com privilégios elevados do PowerShell como administrador. Essa etapa é necessária para carregar os módulos do Service Fabric que foram instalados.
3. Digite o seguinte comando do PowerShell para criar um cluster de desenvolvimento:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateMeshCluster -CreateOneNodeCluster
    ```
4. Para iniciar a ferramenta de gerenciamento do cluster, execute o comando do PowerShell a seguir:

    ```powershell
    . "C:\Program Files\Microsoft SDKs\Service Fabric\Tools\ServiceFabricLocalClusterManager\ServiceFabricLocalClusterManager.exe"
    ```
5. Depois que a ferramenta Gerenciador do cluster de serviço estiver funcionando (aparece na bandeja do sistema), clique nela com o botão direito do mouse e clique em **Iniciar o Cluster Local**.

![Figura 1 – Iniciar o cluster local](./media/service-fabric-mesh-howto-setup-developer-environment-sdk/start-local-cluster.png)

Agora você está pronto para criar os aplicativos de malha do Service Fabric!

## <a name="next-steps"></a>Próximas etapas

Leia o tutorial [Criar um aplicativo do Azure Service Fabric](service-fabric-mesh-tutorial-create-dotnetcore.md).

Encontre respostas para [dúvidas comuns e problemas conhecidos](service-fabric-mesh-faq.md).

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-runtime]: https://aka.ms/sfruntime
[download-sdk]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK
[download-sdkmesh]: https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-SDK-Mesh
[download-tools]: https://aka.ms/sfmesh_vs2017tools
[download-visual-studio]: https://www.visualstudio.com/downloads/