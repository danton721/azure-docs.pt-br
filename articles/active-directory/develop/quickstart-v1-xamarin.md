---
title: Introdução ao Xamarin do Azure AD | Microsoft Docs
description: Crie aplicativos Xamarin que se integrem ao Azure AD para entrar e chame APIs protegidas do Azure AD usando OAuth.
services: active-directory
documentationcenter: xamarin
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3470d9220ed471a05792ed5b3bb259e0dcbe0a6
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121905"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Início Rápido: Criar um aplicativo Xamarin que integra a entrada com a conta da Microsoft

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Com o Xamarin, você pode gravar aplicativos móveis em C# que podem ser executados no iOS, no Android e no Windows (dispositivos móveis e computadores). Se você estiver criando um aplicativo usando o Xamarin, o Azure Active Directory (Azure AD) facilita autenticar usuários com suas contas do Azure AD. O aplicativo também pode consumir com segurança qualquer API Web protegida pelo Azure AD, por exemplo as APIs do Office 365 ou a API do Azure.

Para aplicativos do Xamarin que precisam acessar recursos protegidos, o Azure AD fornece a Biblioteca de Autenticação do Active Directory (ADAL). A única finalidade da ADAL é tornar mais fácil para seus aplicativos obter tokens de acesso. Para demonstrar como é fácil, este artigo mostra como criar aplicativos DirectorySearcher que:

* Execute no iOS, no Android, na Área de Trabalho do Windows, no Windows Phone e na Windows Store.
* Use uma única PCL (portable class library - biblioteca de classes portátil) para autenticar usuários e obter tokens para a API do Graph do Azure AD.
* Pesquise um diretório para usuários com um determinado UPN.

## <a name="prerequisites"></a>Pré-requisitos

* Baixe um [projeto de esqueleto](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) ou baixe a [amostra concluída](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Cada download é uma solução do Visual Studio 2013.
* Você também precisará de um locatário do Azure AD no qual criar usuários e registrar o aplicativo. Se você ainda não tiver um locatário [saiba como obter um](quickstart-create-new-tenant.md).

Quando estiver pronto, siga os procedimentos nas próximas quatro seções.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Etapa 1: Configurar seu ambiente de desenvolvimento do Xamarin

Como este tutorial inclui projetos para iOS, Android e Windows, são necessários o Visual Studio e o Xamarin. Para criar o ambiente necessário, conclua o processo em [definido para cima e instale o Visual Studio e Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) no MSDN. Essas instruções incluem material que você pode examinar para saber mais sobre o Xamarin enquanto aguarda a conclusão das instalações.

Depois de concluir a instalação, abra a solução no Visual Studio. Lá, você encontrará seis projetos: cinco projetos específicos de plataforma e um PCL, DirectorySearcher.cs, que será compartilhado entre todas as plataformas.

## <a name="step-2-register-the-directorysearcher-app"></a>Etapa 2: Registrar o aplicativo DirectorySearcher

Para habilitar o aplicativo para obter tokens, primeiro será necessário registrá-lo no seu locatário do Azure AD e conceder permissão para acessar a API do Graph do Azure AD. Faça assim:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Na barra superior, clique em sua conta. Em seguida, na lista **Diretório**, selecione o locatário do Active Directory em que você deseja registrar o aplicativo.
3. Clique em **Todos os serviços** no painel esquerdo e, em seguida, selecione **Azure Active Directory**.
4. Clique em **Registros de aplicativo** e selecione **Novo registro**.
5. Siga os prompts para criar um novo aplicativo cliente.
   * **Nome** descreve o aplicativo aos usuários.
   * Em **Tipos de conta com suporte**, selecione **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**.
   * O **URI de redirecionamento** é uma combinação de esquema e de cadeia de caracteres que o Azure AD usará para retornar respostas de tokens. Insira um valor (por exemplo, `http://DirectorySearcher`).
6. Depois que você concluir o registro, o Azure AD atribuirá uma ID do aplicativo exclusiva ao aplicativo. Copie o valor da guia **Aplicativo**, pois você precisará dele mais tarde.
7. Na página **Permissões de API**, escolha **Adicionar uma permissão**. Em **Selecionar uma API**, escolha ***Microsoft Graph***.
8. Em **Permissões delegadas**, escolha a permissão **User.Read** e pressione **Adicionar** para salvar. Essa permissão configurará o aplicativo para consultar a API do Graph do Azure AD para os usuários.

## <a name="step-3-install-and-configure-adal"></a>Etapa 3: Instalar e configurar a ADAL

Agora que você tem um aplicativo no Azure AD, você pode instalar a ADAL e escrever seu código relacionado à identidade. Para permitir que a ADAL se comunique com o Azure AD, dê a ela algumas informações sobre o registro do aplicativo.

1. Adicione a ADAL ao projeto DirectorySearcher usando o Console do Gerenciador de Pacotes.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    ```

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    ```

    Observe que as duas referências de biblioteca são adicionadas a cada projeto - a parte PCL da ADAL e uma parte específica da plataforma.
2. No projeto DirectorySearcherLib, abra DirectorySearcher.cs.
3. Substitua os valores de membro de classe pelos valores que você inseriu no portal do Azure. Seu código fará referência a esses valores sempre que ele usar a ADAL.

   * O *tenant* é o domínio do seu locatário do Azure AD, por exemplo, contoso.onmicrosoft.com.
   * A *clientId* é a ID do cliente do aplicativo, que você copiou do portal.
   * O *returnUri* é o URI de redirecionamento que você inseriu no portal (por exemplo, `http://DirectorySearcher`).

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Etapa 4: Usar a ADAL para obter tokens do Azure AD

Quase toda lógica de autenticação do aplicativo está em `DirectorySearcher.SearchByAlias(...)`. Tudo o que é necessário nos projetos específicos da plataforma é passar um parâmetro contextual para o PCL do `DirectorySearcher`.

1. Abra DirectorySearcher.cs e, em seguida, adicione um novo parâmetro para o `SearchByAlias(...)` método. `IPlatformParameters` é o parâmetro contextual que encapsula os objetos específicos da plataforma de que a ADAL precisa para realizar a autenticação.

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Inicialize `AuthenticationContext`, que é a classe primária da ADAL. Essa ação passa à ADAL as coordenadas necessárias para se comunicar com o Azure AD.
3. Chame `AcquireTokenAsync(...)`, que aceita o objeto `IPlatformParameters` e invoca o fluxo de autenticação necessário para retornar um token para o aplicativo.

    ```csharp
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)` tenta primeiro retornar um token para o recurso solicitado (a API do Graph neste caso) sem solicitar que os usuários insiram suas credenciais (por meio de caching ou atualizando tokens antigos). Se necessário, ele exibe ao usuário a página de entrada do Azure AD antes de adquirir o token solicitado.
4. Anexe o token de acesso à solicitação da API do Graph no cabeçalho **Autorização**:

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Isso é tudo para a PCL do `DirectorySearcher` e o código relacionado à identidade do aplicativo. Tudo o que resta fazer é chamar o método `SearchByAlias(...)` nos modos de exibição de cada plataforma e, onde necessário, adicionar código para tratar corretamente o ciclo de vida da interface do usuário.

### <a name="android"></a>Android
1. No MainActivity.cs, adicione uma chamada para `SearchByAlias(...)` manipulador de clique do botão:

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Substitua o método de ciclo de vida `OnActivityResult` para encaminhar qualquer autenticação, fazendo com que ela seja redirecionada para o método apropriado. A ADAL fornece um método auxiliar para isso no Android:

    ```csharp
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Área de Trabalho do Windows

Em MainWindow.xaml.cs, fazer uma chamada para `SearchByAlias(...)` passando um `WindowInteropHelper` na área de trabalho `PlatformParameters` objeto:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
Em DirSearchClient_iOSViewController.cs, o iOS `PlatformParameters` objeto pega uma referência para o controlador de exibição:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
No Windows Universal, abra o arquivo MainPage.xaml.cs e implementar o `Search` método. Esse método usa um método auxiliar em um projeto compartilhado para atualizar a interface do usuário conforme necessário.

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Agora você tem um aplicativo Xamarin em funcionamento que pode autenticar usuários e chamar as APIs de Web com segurança usando OAuth 2.0 em cinco plataformas diferentes.

## <a name="step-5-populate-your-tenant"></a>Etapa 5: Preencher o locatário 

Se você ainda não fez isso, agora é o momento de popular seu locatário com alguns usuários.

1. Execute o aplicativo DirectorySearcher e então entre com um dos usuários.
2. Procure por outros usuários com base em seus UPNs.

## <a name="next-steps"></a>Próximas etapas

A ADAL facilita a incorporação de recursos comuns de identidade no aplicativo. Ele se encarrega de todo o trabalho difícil para você, por exemplo, gerenciamento de cache, suporte a protocolo OAuth, apresentação de uma interface do usuário de logon ao usuário e atualização de tokens expirados. Você precisa conhecer apenas uma única chamada à API, `authContext.AcquireToken*(…)`.

* Faça o download da [amostra concluída](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (sem seus valores de configuração).
* Aprenda a [Proteger um .NET Web API com o Azure AD](quickstart-v1-dotnet-webapi.md).
