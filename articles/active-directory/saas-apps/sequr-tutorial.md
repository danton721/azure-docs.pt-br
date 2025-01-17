---
title: 'Tutorial: Integração do Azure Active Directory ao Sequr | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Sequr.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a491e2ce-b4e8-41b8-8f4a-a2e263e462c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0dace26f9eb4948a8cfd06be568ab9ec471765d1
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922162"
---
# <a name="tutorial-azure-active-directory-integration-with-sequr"></a>Tutorial: Integração do Azure Active Directory ao Sequr

Neste tutorial, você aprenderá a integrar o Sequr ao Microsoft Azure Active Directory (Azure AD).
A integração do Sequr ao Microsoft Azure Active Directory oferece os seguintes benefícios:

* Você pode controlar no Microsoft Azure Active Directory quem terá acesso ao Sequr.
* Você pode permitir que seus usuários entrem automaticamente no Sequr (logon único) com suas contas do Microsoft Azure Active Directory.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Microsoft Azure Active Directory com Sequr, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura habilitada para logon único do Sequr

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Sequr dá suporte a SSO iniciado por **SP e IDP**

## <a name="adding-sequr-from-the-gallery"></a>Adicionar o Sequr da galeria

Para configurar a integração do Sequr ao Microsoft Azure Active Directory, você precisará adicionar o Sequr da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Sequr da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Sequr**, selecione **Sequr** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Sequr na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Microsoft Azure Active Directory com o Sequr, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Microsoft Azure Active Directory e o usuário relacionado do Sequr.

Para configurar e testar o logon único do Microsoft Azure Active Directory com o Sequr, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Sequr](#configure-sequr-single-sign-on)** : para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Sequr](#create-sequr-test-user)** : para ter um equivalente de Brenda Fernandes no Sequr que esteja vinculado à representação de usuário do Microsoft Azure Active Directory.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Microsoft Azure Active Directory com o Sequr, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Sequr**, clique em **Logon Único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica do SAML**, caso deseje configurar o aplicativo no modo iniciado por **IDP** execute a seguinte etapa:

    ![Informações de logon único em Domínio e URLs do Sequr](common/idp-identifier.png)

    Na caixa de texto **Identificador**, digite a URL: `https://login.sequr.io`

5. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    ![image](common/both-advanced-urls.png)

     a. Na caixa de texto **URL de Logon**, digite a URL: `https://login.sequr.io`

    b. Na caixa de texto **Estado de retransmissão**, você obterá esse valor, que é explicado posteriormente no tutorial.

6. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

7. Na seção **Configurar o Sequr**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-sequr-single-sign-on"></a>Configurar o logon único do Sequr

1. Em outra janela do navegador da Web, faça logon em seu site de empresa do Sequr como administrador.

1. Clique em **Integrações** no painel de navegação esquerdo.

    ![Configuração do Sequr](./media/sequr-tutorial/configure1.png)

1. Role para baixo até a seção **Logon Único** e clique em **Gerenciar**.

    ![Configuração do Sequr](./media/sequr-tutorial/configure2.png)

1. Na seção **Gerenciar Logon Único**, execute as seguintes etapas:

    ![Configuração do Sequr](./media/sequr-tutorial/configure3.png)

     a. Na caixa de texto **URL de logon único de provedor de identidade**, cole o valor da **URL de Logon** que você copiou do portal do Azure.

    b. Arraste e solte o arquivo do **Certificado** que você faz o download pelo portal do Azure ou insira manualmente o conteúdo do certificado.

    c. Depois de salvar a configuração, o valor de estado de retransmissão será gerado. Copie o valor de **Estado de Retransmissão** e cole-o na caixa de texto **Estado de Retransmissão** da seção **Configuração Básica de SAML** no portal do Azure.

    d. Clique em **Save** (Salvar).

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

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Sequr.

1. No portal do Azure, escolha **Aplicativos Empresariais**, escolha **Todos os Aplicativos** e, em seguida, escolha **Sequr**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Sequr**.

    ![O link do Sequr na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-sequr-test-user"></a>Criar um usuário de teste do Sequr

Nesta seção, você criará uma usuária chamada Brenda Fernandes no Sequr. Trabalhe com a [equipe de suporte ao Cliente do Sequr](mailto:support@sequr.io) para adicionar os usuários à plataforma Sequr. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Sequr no Painel de Acesso, você deve ser conectado automaticamente ao Sequr no qual configurou o logon único. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

