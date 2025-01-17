---
title: 'Tutorial: Integração do Azure Active Directory com o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ee9a924d-ccde-45b0-ab40-877f82f5dfa2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6dc228f3473a4ddca8b5b68cdd99f1fced1a04cd
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922236"
---
# <a name="tutorial-azure-active-directory-integration-with-sensoscientific-wireless-temperature-monitoring-system"></a>Tutorial: Integração do Azure Active Directory com o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific

Neste tutorial, você aprende a integrar o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific ao Azure AD (Azure Active Directory).
A integração do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific ao Azure AD oferece os seguintes benefícios:

* No Azure AD, é possível controlar quem tem acesso ao Sistema de Monitoramento de Temperatura Sem Fio SensoScientific.
* É possível permitir que seus usuários entrem automaticamente no Sistema de Monitoramento de Temperatura Sem Fio SensoScientific (logon único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Sistema de Monitoramento de Temperatura Sem Fio SensoScientific, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Sistema de Monitoramento de Temperatura Sem Fio SensoScientific é compatível com SSO iniciado por **IDP**

## <a name="adding-sensoscientific-wireless-temperature-monitoring-system-from-the-gallery"></a>Adicionar o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific por meio da galeria

Para configurar a integração do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific no Azure AD, você precisa adicionar o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Sistema de Monitoramento de Temperatura Sem Fio SensoScientific**, selecione **Sistema de Monitoramento de Temperatura Sem Fio SensoScientific** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Sistema de Monitoramento de Temperatura Sem Fio SensoScientific na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific.

Para configurar e testar o logon único do Azure AD com o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar logon único do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific](#configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on)** – para configurar as definições de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific](#create-sensoscientific-wireless-temperature-monitoring-system-test-user)** – para ter um equivalente de Brenda Fernandes no Sistema de Monitoramento de Temperatura Sem Fio SensoScientific que esteja vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific, siga as etapas abaixo:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Sistema de Monitoramento de Temperatura Sem Fio SensoScientific**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica do SAML**, o usuário não precisa executar nenhuma etapa, pois o aplicativo já está pré-integrado ao Azure.

    ![Informações de logon único em Domínio e URLs do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific](common/preintegrated.png)

5. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

6. Na seção **Configurar o Sistema de Monitoramento de Temperatura Sem Fio SensoScientific**, copie a URL apropriada de acordo com seus requisitos.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-sensoscientific-wireless-temperature-monitoring-system-single-sign-on"></a>Configurar logon único do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific

1. Faça logon no aplicativo Sistema de Monitoramento de Temperatura Sem Fio SensoScientific como administrador.

1. No menu de navegação na parte superior, clique em **Configuração** e acesse **Configurar** em **Logon Único** para abrir as Configurações de Logon Único e execute as seguintes etapas:

    ![Configurar o logon único](./media/sensoscientific-tutorial/tutorial_sensoscientificwtms_admin.png)

     a. Selecione **Nome do Emissor** como Azure AD.

    b. Na caixa de texto **URL do Emissor**, cole o **Identificador do Azure AD** que você copiou do portal do Azure.

    c. Na caixa de texto **URL de Serviço de Logon Único**, cole a **URL de Logon** que você copiou do portal do Azure.

    d. Na caixa de texto **URL de Serviço de Logoff Único**, cole a **URL de Logoff** que você copiou do portal do Azure.

    e. Procure o certificado que você baixou do portal do Azure e carregue-o aqui.

    f. Clique em **Save** (Salvar).

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

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Sistema de Monitoramento de Temperatura Sem Fio SensoScientific.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Sistema de Monitoramento de Temperatura Sem Fio SensoScientific**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Sistema de Monitoramento de Temperatura Sem Fio SensoScientific**.

    ![O link do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific na lista de aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-sensoscientific-wireless-temperature-monitoring-system-test-user"></a>Criar usuário de teste do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific

Para permitir que os usuários do Azure AD entrem no Sistema de Monitoramento de Temperatura Sem Fio SensoScientific, eles devem ser provisionados no Sistema de Monitoramento de Temperatura Sem Fio SensoScientific. Trabalhe com a [equipe de suporte do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific](https://www.sensoscientific.com/contact-us/) para adicionar os usuários à plataforma Sistema de Monitoramento de Temperatura Sem Fio SensoScientific. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do Sistema de Monitoramento de Temperatura Sem Fio SensoScientific no Painel de Acesso, deverá ser conectado automaticamente ao Sistema de Monitoramento de Temperatura Sem Fio SensoScientific para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

