---
title: Comunicar-se com um aplicativo de dispositivo em C por meio de fluxos de dispositivos do Hub IoT do Azure (versão prévia) | Microsoft Docs
description: Neste início rápido, você executará um aplicativo do lado do serviço do C que se comunica com um dispositivo IoT por meio de um fluxo de dispositivos.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: f5e6128c1ecceda181f92b2d81e9ac06effbfce2
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834073"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Início Rápido: comunicar-se com um aplicativo de dispositivo no C por meio de fluxos de dispositivos do Hub IoT (versão prévia)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Atualmente, o Hub IoT do Microsoft Azure dá suporte a fluxos de dispositivos como uma [versão prévia do recurso](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Os [fluxos de dispositivos do Hub IoT](iot-hub-device-streams-overview.md) permitem que aplicativos de serviço e dispositivo se comuniquem de maneira segura e simples para o firewall. Durante a versão prévia pública, o SDK do C dá suporte somente a fluxos de dispositivos no lado do dispositivo. Consequentemente, este início rápido contém apenas instruções para executar o aplicativo no lado do dispositivo. Você deve executar um aplicativo no lado do serviço que está disponível nos seguintes inícios rápidos:
 
   * [Comunicação com aplicativos de dispositivo no C# por meio de fluxos de dispositivo do Hub IoT](./quickstart-device-streams-echo-csharp.md)

   * [Comunicação com aplicativos de dispositivo no Nodejs por meio de fluxos de dispositivo do Hub IoT](./quickstart-device-streams-echo-nodejs.md).

Neste início rápido, o aplicativo C no lado do dispositivo tem a funcionalidade a seguir:

* Estabelecer um fluxo de dispositivos para um dispositivo IoT.

* Receber os dados enviados do lado do serviço e reproduzi-los novamente.

O código demonstrará o processo de inicialização de um fluxo de dispositivo, assim como usá-lo para enviar e receber dados.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Atualmente, a versão prévia dos fluxos de dispositivos só é compatível com os Hubs IoT criados nas seguintes regiões:

  * **Centro dos EUA**

  * **EUA Central EUAP**

* Instale o [Visual Studio 2017](https://www.visualstudio.com/vs/) com a carga de trabalho ["Desenvolvimento de área de trabalho com C++"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.

* Instale a versão mais recente do [Git](https://git-scm.com/download/).

* Execute o comando a seguir para adicionar a Extensão do Microsoft Azure IoT para a CLI do Azure à instância do Cloud Shell. A Extensão de IoT adiciona comandos específicos do Hub IoT, do IoT Edge e do DPS (Serviço de Provisionamento de Dispositivos) no IoT à CLI do Azure.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Preparar o ambiente de desenvolvimento

Neste início rápido, você usará o [SDK do dispositivo IoT do Azure para C](iot-hub-device-sdk-c-intro.md). Você preparará um ambiente de desenvolvimento usado para clonar e criar o [SDK do C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c) no GitHub. O SDK no GitHub inclui o código de exemplo usado neste início rápido.

1. Baixe o [sistema de build CMake](https://cmake.org/download/).

    É importante que os pré-requisitos do Visual Studio (Visual Studio e a carga de trabalho de "Desenvolvimento para Desktop com C++") estejam instalados em seu computador, **antes** da instalação de `CMake`. Após a instalação dos pré-requisitos e verificação do download, instale o sistema de compilação CMake.

2. Abra um prompt de comando ou o shell Bash do Git. Execute o seguinte comando para clonar o repositório do GitHub [SDK de C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c):

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Você deve esperar que essa operação leve alguns minutos.

3. Crie um subdiretório `cmake` no diretório raiz do repositório git e navegue até essa pasta.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Execute os comandos a seguir do diretório `cmake` para build de uma versão do SDK específica para a plataforma cliente de desenvolvimento.

   * No Linux:

      ```bash
      cmake ..
      make -j
      ```

   * No Windows, execute os comandos a seguir no Prompt de Comando do Desenvolvedor para o Visual Studio 2015 ou 2017. Uma solução do Visual Studio para o dispositivo simulado será gerada no diretório `cmake`.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Crie um hub IoT

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar um dispositivo

Um dispositivo deve ser registrado no hub IoT antes de poder se conectar. Nesta seção, você usará o Azure Cloud Shell com a [extensão de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para registrar um dispositivo simulado.

1. Execute o comando a seguir no Azure Cloud Shell para criar a identidade do dispositivo.

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o hub IoT.

   **MyDevice**: Esse é o nome fornecido para o dispositivo registrado. Use MyDevice, conforme mostrado. Se você escolher um nome diferente para seu dispositivo, você também precisará usar esse nome ao longo deste artigo e atualizar o nome de dispositivo nos aplicativos de exemplo antes de executá-los.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Execute os seguintes comandos no Azure Cloud Shell para obter a *cadeia de conexão de dispositivo* referente ao dispositivo que você acabou de registrar:

   **YourIoTHubName**: substitua o espaço reservado abaixo pelo nome escolhido para o hub IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Anote a cadeia de conexão do dispositivo, que se parece com o exemplo a seguir:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Você usará esse valor posteriormente no início rápido.

## <a name="communicate-between-device-and-service-via-device-streams"></a>Comunicação entre o dispositivo e serviço por meio de fluxos de dispositivos

Nesta seção, você executará o aplicativo do lado do dispositivo e o aplicativo do lado do serviço, e estabelecerá comunicação entre os dois.

### <a name="run-the-device-side-application"></a>Executar o aplicativo do lado do dispositivo

Para executar o aplicativo do lado do dispositivo, você precisará executar as etapas a seguir:

1. Forneça as credenciais do dispositivo, editando o arquivo de origem `iothub_client_c2d_streaming_sample.c` na pasta `iothub_client/samples/iothub_client_c2d_streaming_sample` e fornecendo a cadeia de conexão do dispositivo.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Compile o código conforme segue:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Execute o programa compilado:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Executar o aplicativo do lado do serviço

Como mencionado anteriormente, o SDK C do Hub IoT dá suporte apenas a fluxos de dispositivos no lado do dispositivo. Para compilar e executar o aplicativo do lado do serviço, siga as etapas disponíveis em um dos seguintes inícios rápidos:

* [Comunicação com um aplicativo de dispositivo no C# por meio de fluxos de dispositivos do Hub IoT](./quickstart-device-streams-echo-csharp.md)

* [Comunicação com um aplicativo de dispositivo no Node.js por meio de fluxos de dispositivos do Hub IoT](./quickstart-device-streams-echo-nodejs.md).

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você configurou um Hub IoT, registrou um dispositivo, estabeleceu um fluxo de dispositivo entre um aplicativo C no dispositivo e outro aplicativo no lado do serviço e usou o fluxo para enviar dados entre os aplicativos.

Use os links abaixo para saber mais sobre fluxos de dispositivos:

> [!div class="nextstepaction"]
> [Visão geral dos fluxos de dispositivos](./iot-hub-device-streams-overview.md)