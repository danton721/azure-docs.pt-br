---
title: 'Tutorial: integração do Azure Active Directory com o Workstars | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Workstars.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: db646c48c1332f986840d54127729c766ffb6dd7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64920322"
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a>Tutorial: integração do Azure Active Directory com o Workstars

Neste tutorial, você aprenderá a integrar o Workstars ao Azure AD (Azure Active Directory).
A integração do Workstars ao Azure AD oferece os seguintes benefícios:

* No Azure AD, é possível controlar quem tem acesso ao Workstars.
* Você pode permitir que os usuários sejam conectados automaticamente ao Workstars (Logon Único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Workstars, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura do Workstars habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Workstars é compatível com SSO iniciado por **IDP**

## <a name="adding-workstars-from-the-gallery"></a>Adicionar o Workstars da galeria

Para configurar a integração do Workstars ao Azure AD, você precisará adicionar o Workstars da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Workstars da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Workstars**, selecione **Workstars** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Workstars na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Workstars, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Workstars.

Para configurar e testar o logon único do Azure AD com o Workstars, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Workstars](#configure-workstars-single-sign-on)** – para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar usuário de teste do Workstars](#create-workstars-test-user)** – para ter um equivalente de Brenda Fernandes no Workstars vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Workstars, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Workstars**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na página **Configurar Logon Único com SAML**, execute as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Workstars](common/idp-intiated.png)

     a. Na caixa de texto **Identificador**, digite uma URL: `https://workstars.com`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://<subdomain>.workstars.com/saml/login_check`

    > [!NOTE]
    > O valor não é real. Atualize o valor com a URL de Resposta real. Contate a [equipe de suporte ao cliente do Workstars](https://support.workstars.com/) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

6. Na seção **Configurar o Workstars**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-workstars-single-sign-on"></a>Configurar o logon único do Workstars

1. Em outra janela do navegador, entre em seu site de empresa do Workstars como administrador.

2. Na barra de ferramentas principal, clique em **Configurações**.

    ![Configurações do Workstars](./media/workstars-tutorial/tutorial_workstars_sett.png)

3. Vá para **Logon** > **Configurações**.

    ![Logon do Workstars](./media/workstars-tutorial/tutorial_workstars_signon.png)

    ![Configurações do Workstars](./media/workstars-tutorial/tutorial_workstars_settings.png)

4. Na página **Configurações de Logon Único (SAML) – Configurações**, realize as seguintes etapas:
    
    ![SAML do Workstars](./media/workstars-tutorial/tutorial_workstars_saml.png)

     a. Na caixa de texto **Nome do Provedor de Identidade**, digite **Office 365**.

    b. Na caixa de texto **ID da Entidade do Provedor de Identidade**, cole o valor de **Identificador do Azure AD**, copiado do portal do Azure.

    c. Copie o conteúdo do arquivo de certificado baixado no bloco de notas e então cole-o na caixa de texto **Certificado x509**. 

    d. Na caixa de texto **URL de SSO do SAML**, cole o valor da **URL de Logon** copiado do portal do Azure.
    
    e. Na caixa de texto **URL de Logoff Remoto**, cole o valor da **URL de Logoff** copiado do portal do Azure. 

    f. Selecione **ID de Nome** como **Email (Padrão)** .

    g. Clique em **Confirmar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite brittasimon@yourcompanydomain.extension. Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Workstars.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Workstars**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Workstars**.

    ![O link do Workstars na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-workstars-test-user"></a>Criar usuário de teste do Workstars

Nesta seção, você criará uma usuária chamada Brenda Fernandes no Workstars. Trabalhe com a [equipe de suporte do Workstars](https://support.workstars.com) para adicionar os usuários na plataforma do Workstars.

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Workstars no Painel de Acesso, você deverá ser conectado automaticamente ao Workstars, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

