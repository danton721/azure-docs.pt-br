---
title: 'Tutorial: Integração do Azure Active Directory com o Boomi | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1fe436632eee12157dde2b082a5c77e67e7977cc
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957173"
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Tutorial: Integração do Azure Active Directory com o Boomi

Neste tutorial, você aprenderá a integrar o Boomi ao Azure AD (Azure Active Directory).
A integração do Boomi ao Azure AD oferece os seguintes benefícios:

* No Azure AD você pode controlar quem tem acesso ao Boomi.
* Você pode permitir que os usuários sejam conectados automaticamente ao Boomi (Logon Único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Boomi, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Uma assinatura do Boomi habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Boomi é compatível com SSO iniciado por **IDP**

## <a name="adding-boomi-from-the-gallery"></a>Adicionar Boomi da galeria

Para configurar a integração do Boomi ao Azure AD, você precisa adicionar o Boomi por meio da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Boomi da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Boomi**, selecione **Boomi** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Boomi na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Boomi com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Boomi.

Para configurar e testar o logon único do Azure AD com o Boomi, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o Logon Único do Boomi](#configure-boomi-single-sign-on)** – para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Boomi](#create-boomi-test-user)** – para ter um equivalente de Brenda Fernandes no Boomi que esteja vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Boomi, execute as seguintes etapas:

1. No [Portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Boomi**, selecione **Logon Único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na página **Definir logon único com SAML**, clique no botão **Editar** para abrir o diálogo **Configuração básica de SAML**.

    ![Informações de logon único de Domínio e URLs do Boomi](common/idp-intiated.png)

     a. Na caixa de texto **Identificador**, digite uma URL: `https://platform.boomi.com/`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://platform.boomi.com/sso/<boomi-tenant>/saml`

    > [!NOTE]
    > O valor de URL de Resposta não é real. Atualize o valor com a URL de Resposta real. Entre em contato com a [equipe de suporte ao cliente do Boomi](https://boomi.com/company/contact/) para obter esse valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. O aplicativo Boomi espera que as declarações SAML estejam em um formato específico. Configure as declarações a seguir para este aplicativo. Você pode gerenciar os valores desses atributos da seção **Atributos de Usuário** na página de integração de aplicativos. Na página **Definir Logon Único com SAML**, clique no botão **Editar** para abrir a caixa de diálogo **Atributos do Usuário**.

    ![image](common/edit-attribute.png)

6. Na seção **Declarações de Usuário** do diálogo **Atributos de Usuário**, configure o atributo de token SAML conforme mostrado na imagem acima e execute as seguintes etapas:

    | NOME |  Atributo de Origem|
    | ---------------|  --------- |
    | FEDERATION_ID | user.mail |

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

8. Na seção **Configurar o Boomi**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure Ad

    c. URL de logoff

### <a name="configure-boomi-single-sign-on"></a>Configurar logon único do Boomi

1. Em outra janela do navegador da Web, faça logon em seu site de empresa Boomi como um administrador. 

2. Navegue até **Nome da Empresa** e vá para **Configurar**.

3. Clique na guia **Opções de SSO** e execute etapas a seguir.

    ![Configurar o logon único no lado do aplicativo](./media/boomi-tutorial/tutorial_boomi_11.png)

     a. Marque a caixa de seleção **Habilitar Logon Único do SAML**.

    b. Clique em **Importar** para carregar o certificado baixado do Azure AD no **Certificado do Provedor de Identidade**.

    c. Na caixa de texto **URL de Logon do Provedor de Identidade**, insira o valor da **URL de Logon** da janela de configuração de aplicativo do Azure AD.

    d. Para **Local do ID de Federação**, selecione o botão de opção **A Id de Federação está contida no elemento Atributo FEDERATION_ID**.

    e. Clique no botão **Salvar** .

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite **brendafernandes\@dominiodaempresa.extensao**  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure ao conceder acesso ao Boomi.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Boomi**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Boomi**.

    ![Link do Boomi na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-boomi-test-user"></a>Criar um usuário de teste do Boomi

Para permitir que os usuários do Azure AD façam logon no Boomi, eles deverão ser provisionados no Boomi. No caso do Boomi, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para provisionar uma conta de usuário, execute as seguintes etapas:

1. Faça logon em seu site de empresa Boomi como um administrador.

2. Depois de fazer logon, navegue até **Gerenciamento de Usuário** e vá até **Usuários**.

    ![Usuários](./media/boomi-tutorial/tutorial_boomi_001.png "Usuários")

3. Clique no ícone **+** e o diálogo **Adicionar/Manter Funções de Usuário** será aberto.

    ![Usuários](./media/boomi-tutorial/tutorial_boomi_002.png "Usuários")

    ![Usuários](./media/boomi-tutorial/tutorial_boomi_003.png "Usuários")

     a. Na caixa de texto **Endereço de email do usuário**, digite o email do usuário, como BrittaSimon@contoso.com.

    b. Na caixa de texto **Nome**, digite o Nome do usuário como Brenda.

    c. Na caixa de texto **Sobrenome**, digite o Sobrenome do usuário como Fernandes.

    d. Insira a **ID de Federação** do usuário. Cada usuário deve ter uma ID de Federação que o identifique exclusivamente na conta.

    e. Atribua a função **Usuário Padrão** ao usuário. Não atribua a função Administrador, pois isso concederia a ele o acesso normal ao Atmosphere, além do acesso de logon único.

    f. Clique em **OK**.

    > [!NOTE]
    > O usuário não receberá um email de notificação de boas-vindas contendo uma senha que pode ser usada para fazer logon na conta do AtomSphere porque a senha é gerenciada por meio do provedor de identidade. É possível usar qualquer outra ferramenta de criação da conta de usuário do Boomi ou as APIs fornecidas pelo Boomi para provisionar as contas de usuário do AAD.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Boomi no Painel de Acesso, você deverá ser conectado automaticamente ao Boomi, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

