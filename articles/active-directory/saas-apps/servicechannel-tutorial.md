---
title: 'Tutorial: integração do Azure Active Directory com o ServiceChannel | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o ServiceChannel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce9573f78b6e8a9db65f35b7fc7711a8d3534508
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64699566"
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a>Tutorial: integração do Azure Active Directory com o ServiceChannel

Neste tutorial, você aprenderá a integrar o ServiceChannel ao Azure AD (Azure Active Directory).
A integração do ServiceChannel ao Azure AD oferece os seguintes benefícios:

* No Microsoft Azure Active Directory, é possível controlar quem tem acesso ao ServiceChannel.
* Você pode permitir que seus usuários entrem automaticamente no ServiceChannel (logon único) com suas contas do Microsoft Azure Active Directory.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao ServiceChannel, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura do ServiceChannel habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O ServiceChannel dá suporte ao SSO iniciado por **IDP**
* O ServiceChannel é compatível com o provisionamento de usuário **Just-In-Time**

## <a name="adding-servicechannel-from-the-gallery"></a>Adicionar o ServiceChannel da galeria

Para configurar a integração do ServiceChannel ao Azure AD, você precisará adicionar o ServiceChannel à sua lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o ServiceChannel da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **ServiceChannel**, selecione **ServiceChannel** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![O ServiceChannel na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Microsoft Azure Active Directory com o ServiceChannel, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Microsoft Azure Active Directory e o usuário relacionado do Springer ServiceChannel.

Para configurar e testar o logon único do Azure AD com o ServiceChannel, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do ServiceChannel](#configure-servicechannel-single-sign-on)** : para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do ServiceChannel](#create-servicechannel-test-user)** : para ter um equivalente de Brenda Fernandes no ServiceChannel que esteja vinculado à representação dela no Microsoft Azure Active Directory.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Microsoft Azure Active Directory com o ServiceChannel, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **ServiceChannel**, clique em **Logon Único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na página **Configurar Logon Único com SAML**, execute as seguintes etapas:

    ![Informações de logon único para domínio e URLs do ServiceChannel](common/idp-intiated.png)

     a. Na caixa de texto **Identificador**, digite o valor: `http://adfs.<domain>.com/adfs/service/trust`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://<customer domain>.servicechannel.com/saml/acs`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com o Identificador e a URL de Resposta reais. Aqui, sugerimos que você use o valor exclusivo de cadeia de caracteres no Identificador. Entre em contato com a [equipe de suporte do ServiceChannel](https://servicechannel.zendesk.com/hc/en-us) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. Seu aplicativo ServiceChannel espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão, em que **nameidentifier** é mapeado com **user.userprincipalname**. O aplicativo ServiceChannel espera que **nameidentifier** seja mapeado com **user.mail**. Portanto, você precisa alterar o mapeamento de atributo clicando no ícone **Editar**.

    Você pode consultar o guia do ServiceChannel [aqui](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) para obter diretrizes sobre declarações.

    ![image](common/edit-attribute.png)

    > [!NOTE]
    > Confira [Gerenciar o acesso usando RBAC e o portal do Azure](../../role-based-access-control/role-assignments-portal.md) para saber como configurar a **Função** no Azure AD.

6. Se você planeja habilitar o provisionamento de usuário Just-In-Time, deve adicionar as declarações a seguir conforme mostrado abaixo. A declaração **Função** precisa ser mapeada para **user.assignedroles**, que contém a função do usuário. Na seção **Declarações de Usuário** da caixa de diálogo **Atributos de Usuário**, execute as seguintes etapas para adicionar o atributo de token SAML, conforme mostrado na tabela abaixo:

    | NOME   |  Atributo de Origem |
    | ------ | --- |
    | Função   | user.assignedroles |

     a. Clique em **Adicionar nova reivindicação** para abrir a caixa de diálogo **Gerenciar declarações de usuários**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Deixe o **Namespace** em branco.

    d. Escolha Origem como **Atributo**.

    e. Na lista **Atributo de origem**, digite o valor do atributo mostrado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Save** (Salvar).

7. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

8. Na seção **Configurar o ServiceChannel**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-servicechannel-single-sign-on"></a>Configurar o logon único do ServiceChannel

Para configurar o logon único no lado do **ServiceChannel**, é necessário enviar o **Certificado (Base64)** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do ServiceChannel](https://servicechannel.zendesk.com/hc/en-us). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite `brittasimon@yourcompanydomain.extension`  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao ServiceChannel.

1. No portal do Azure, escolha **Aplicativos Empresariais**, **Todos os Aplicativos** e, em seguida, escolha **ServiceChannel**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **ServiceChannel**.

    ![O link ServiceChannel na lista Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-servicechannel-test-user"></a>Criar um usuário de teste do ServiceChannel

O aplicativo dá suporte ao provisionamento de usuário just-in-time e, após a autenticação, os usuários serão automaticamente criados no aplicativo. Para um provisionamento de usuário completo, entre em contato com [a equipe de suporte do ServiceChannel](https://servicechannel.zendesk.com/hc/en-us)

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco ServiceChannel no Painel de Acesso, você deve ser conectado automaticamente ao ServiceChannel no qual você configurou o logon único. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
