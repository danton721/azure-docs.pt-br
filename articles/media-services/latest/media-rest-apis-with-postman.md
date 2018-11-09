---
title: Configurar Postman para chamadas de API de REST dos Serviços de Mídia do Azure
description: Saiba como configurar Postman para chamadas de API de REST dos Serviços de Mídia.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.author: juliako
ms.openlocfilehash: b110a57526d6e23ba53afce0a91d735fad6d247c
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215873"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Configurar Postman para chamadas de API de REST dos Serviços de Mídia

Este artigo mostra como configurar o **Postman** para que ele possa ser usado para chamar APIs REST do Azure Media Services (AMS). O artigo mostra como importar arquivos de ambiente e coleção para **Postman**. A coleção contém definições agrupadas de solicitações HTTP que chamam as APIs de REST dos Serviços de Mídia do Azure (AMS). O arquivo de ambiente contém variáveis que são usadas pela coleção.

## <a name="prerequisites"></a>Pré-requisitos

- [Crie uma conta de Serviços de Mídia](create-account-cli-how-to.md). Lembre-se de lembrar o nome do grupo de recursos e o nome da conta do Media Services. 
- Obtenha as informações necessárias para [acessar as APIs](access-api-cli-how-to.md)
- Instale o cliente REST do [Postman](https://www.getpostman.com/) para executar as APIs de REST mostradas em alguns dos tutoriais do REST AMS. 

    Estamos usando o **Postman** mas qualquer ferramenta REST seria adequada. Outras alternativas são: **Visual Studio Code** com o plug-in do REST ou **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Baixar arquivos Postman

Clone um repositório GitHub que contenha os arquivos do ambiente e coleção Postman.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Configurar Postman

Esta seção configura o Postman.

### <a name="configure-the-environment"></a>Configure o ambiente 

1. Abra o aplicativo **Postman**.
2. À direita da tela, selecione a opção **Gerenciar ambiente**.

    ![Gerenciar ambiente](./media/develop-with-postman/postman-import-env.png)
4. Na caixa de diálogo **Gerenciar ambiente**, clique em **Importar**.
2. Navegue até o arquivo `Azure Media Service v3 Environment.postman_environment.json` que foi baixado quando você clonou `https://github.com/Azure-Samples/media-services-v3-rest-postman.git`.
6. O ambiente do **Ambiente do Serviço de Mídia do Azure v3** é adicionado.

    > [!Note]
    > Atualize as variáveis de acesso com valores obtidos a partir da seção **Acessar a API de Serviços de Mídia do Azure** acima.

7. Clique duas vezes no arquivo selecionado e digite os valores que você obteve seguindo as etapas de [acesso a API](#access-the-media-services-api).
8. Feche a caixa de diálogo.
9. Selecione o ambiente, **Ambiente do Serviço de Mídia do Azure v3**, no menu suspenso.

    ![Escolher ambiente](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Configurar a coleção

1. Clique em **Importar** para importar o arquivo de coleção.
1. Navegue até o arquivo `Media Services v3.postman_collection.json` que foi baixado quando você clonou `https://github.com/Azure-Samples/media-services-v3-rest-postman.git`
3. Escolha o arquivo **Serviços de Mídia v3.postman_collection.json**.

    ![Importar um arquivo](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Obter token do Microsoft Azure AD 

Antes de começar a manipular os recursos do AMS v3, você precisa obter e definir o token do Azure AD para Autenticação do Principal de Serviço.

1. Na janela esquerda do Postman, selecione "Etapa 1: Obter token de autenticação do AAD".
2. Em seguida, selecione "Obter token do Microsoft Azure AD para autenticação de entidade de serviço".
3. Pressione **Enviar**.

    A operação **POST** a seguir é enviada.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. A resposta retorna com o token e define a variável de ambiente "AccessToken" como o valor de token. Para ver o código que define "AccessToken", clique na guia **Testes**. 

    ![Obter token do AAD](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="next-steps"></a>Próximas etapas

[Transmitir arquivos com REST](stream-files-tutorial-with-rest.md).  