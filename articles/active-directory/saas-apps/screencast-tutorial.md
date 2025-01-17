---
title: 'Tutorial: Integração do Azure Active Directory com o Screencast-O-Matic | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Screencast-O-Matic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 836ef454e5bdb14fcc3f519f9c6d9f6dfb4b5703
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686305"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Tutorial: Integração do Azure Active Directory com o Screencast-O-Matic

Neste tutorial, você aprenderá a integrar o Screencast-O-Matic ao Azure AD (Azure Active Directory).
A integração do Screencast-O-Matic ao Azure AD oferece os seguintes benefícios:

* No Azure AD, é possível controlar quem tem acesso ao Screencast-O-Matic.
* É possível permitir que os usuários sejam automaticamente conectados ao Screencast-O-Matic (logon único) com suas contas do Microsoft Azure Active Directory.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Screencast-O-Matic, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura habilitada para logon único do Screencast-O-Matic

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Screencast-O-Matic dá suporte ao SSO iniciado por **SP**
* O Screencast-O-Matic é compatível com o provisionamento de usuário **Just-In-Time**

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Adicionando Screencast O Matic da galeria

Para configurar a integração do Screencast-O-Matic ao Azure AD, é necessário adicionar o Screencast-O-Matic à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Screencast-O-Matic da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Screencast-O-Matic**, selecione **Screencast-O-Matic** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Screencast-O-Matic na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Microsoft Azure Active Directory com o Screencast-O-Matic, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Microsoft Azure Active Directory e o usuário relacionado do Screencast-O-Matic.

Para configurar e testar o logon único do Azure AD com o Screencast-O-Matic, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Screencast-O-Matic](#configure-screencast-o-matic-single-sign-on)** : para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Screencast-O-Matic](#create-screencast-o-matic-test-user)** : para ter um equivalente de Brenda Fernandes no Screencast-O-Matic vinculado à representação do usuário no Microsoft Azure Active Directory.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Microsoft Azure Active Directory com o Screencast-O-Matic, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Screencast-O-Matic**, clique em **Logon Único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração básica de SAML**, realize as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Screencast-O-Matic](common/sp-signonurl.png)

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE]
    > O valor não é real. Atualize o valor com a URL de Logon real. Entre em contato com a [Equipe de suporte do Cliente Screencast Matic O](mailto:support@screencast-o-matic.com) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

4. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Download** para baixar o **XML de metadados** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/metadataxml.png)

6. Na seção **Configurar o Screencast-O-Matic**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-screencast-o-matic-single-sign-on"></a>Configurar o logon único do Screencast-O-Matic

1. Em outra janela do navegador da Web, faça logon no Screencast-O-Matic como administrador.

2. Clique em **Assinatura**.

    ![A Assinatura](./media/screencast-tutorial/tutorial_screencast_sub.png)

3. Na seção **página Acesso**, clique em **Instalação**.

    ![O Acesso](./media/screencast-tutorial/tutorial_screencast_setup.png)

4. Na **Página de Acesso da Instalação**, execute as seguintes etapas:

   * Na seção **URL de acesso**, digite o nome de instância na caixa de texto especificada.

    ![O Acesso](./media/screencast-tutorial/tutorial_screencast_access.png)

   * Selecione **Exigir Usuário do Domínio** na seção **Restrição de Usuário SAML (opcional)** .

   * Em **Carregar arquivo XML de metadados IDP**, clique em **Escolher Arquivo** para carregar os metadados que você baixou do portal do Azure.

   * Clique em **OK**.

    ![O Acesso](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD 

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite `brittasimon@yourcompanydomain.extension`. Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao Screencast-O-Matic.

1. No portal do Azure, escolha **Aplicativos Empresariais**, escolha **Todos os Aplicativos** e, em seguida, escolha **Screencast-O-Matic**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Matic O Screencast**.

    ![O link do Screencast-O-Matic na lista de aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-screencast-o-matic-test-user"></a>Criar um usuário de teste do Screencast-O-Matic

Nesta seção, um usuário chamado Brenda Fernandes é criado no Screencast-O-Matic. O Screencast-O-Matic é compatível com o provisionamento de usuário Just-In-Time, que está habilitado por padrão. Não há itens de ação para você nesta seção. Se o usuário ainda não existir no Screencast-O-Matic, será criado um novo usuário após a autenticação. Se você precisar criar um usuário manualmente, contate a  [equipe de suporte ao Cliente do Screencast-O-Matic](mailto:support@screencast-o-matic.com).

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Screencast-O-Matic no Painel de Acesso, você deve ser conectado automaticamente ao Screencast-O-Matic no qual configurou o logon único. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
