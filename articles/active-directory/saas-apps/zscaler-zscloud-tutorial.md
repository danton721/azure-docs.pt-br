---
title: 'Tutorial: Integração do Azure Active Directory com Zscaler ZSCloud | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Zscaler ZSCloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.openlocfilehash: 35886a65020d7cc5a77f9f413d2d65637cb18382
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719310"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Tutorial: Integração do Azure Active Directory com Zscaler ZSCloud

Neste tutorial, você aprenderá a integrar o Zscaler ZSCloud com o Azure AD (Azure Active Directory).
A integração do Zscaler ZSCloud com o Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem tem acesso ao Zscaler ZSCloud.
* É possível permitir que seus usuários entrem automaticamente no Zscaler ZSCloud (logon único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zscaler ZSCloud, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura do Zscaler ZSCloud habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Zscaler ZSCloud é compatível com SSO iniciado por **SP**

* O Zscaler ZSCloud é compatível com o provisionamento de usuário **Just-In-Time**

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Adicionando o Zscaler ZSCloud por meio da galeria

Para configurar a integração do Zscaler ZSCloud ao Azure AD, você precisa adicionar o Zscaler ZSCloud da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Zscaler ZSCloud da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Zscaler ZSCloud**, selecione **Zscaler ZSCloud** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Zscaler ZSCloud na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Zscaler ZSCloud com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Zscaler ZSCloud.

Para configurar e testar o logon único do Azure AD com o Zscaler ZSCloud, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar logon único do Zscaler ZSCloud](#configure-zscaler-zscloud-single-sign-on)** – para configurar as definições de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Zscaler ZSCloud](#create-zscaler-zscloud-test-user)** – para ter um equivalente de Brenda Fernandes no Zscaler ZSCloud que esteja vinculado à representação do usuário no Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Zscaler ZSCloud, siga as etapas abaixo:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Zscaler ZSCloud**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração básica de SAML**, realize as seguintes etapas:

    ![Informações de logon único em Domínio e URLs do Zscaler ZSCloud](common/sp-signonurl.png)

    Na caixa de texto **URL de Entrada**, digite a URL usada pelos usuários para se conectar ao aplicativo ZScaler ZSCloud.

    > [!NOTE]
    > É necessário atualizar o valor com a URL de Logon real. Contate a [equipe de suporte ao cliente do Zscaler ZSCloud](https://help.zscaler.com/) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. O aplicativo Zscaler ZSCloud espera as declarações SAML em um formato específico, o que exige que você adicione mapeamentos de atributos personalizados à sua configuração de atributos de token SAML. A captura de tela a seguir mostra a lista de atributos padrão. Clique no ícone **Editar** para abrir a caixa de diálogo **Atributos de Usuário** .

    ![image](common/edit-attribute.png)

6. Além do indicado acima, o aplicativo Zscaler ZSCloud espera que mais alguns atributos sejam passados novamente na resposta SAML. Na seção **Declarações de Usuário** da caixa de diálogo **Atributos de Usuário**, execute as seguintes etapas para adicionar o atributo de token SAML, conforme mostrado na tabela abaixo:
    
    | NOME | Atributo de Origem |
    | ---------| ------------ |
    | memberOf     | user.assignedroles |

     a. Clique em **Adicionar nova reivindicação** para abrir a caixa de diálogo **Gerenciar declarações de usuários**.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Deixe o **Namespace** em branco.

    d. Escolha Origem como **Atributo**.

    e. Na lista **Atributo de origem**, digite o valor do atributo mostrado para essa linha.
    
    f. Clique em **Save** (Salvar).

    > [!NOTE]
    > Clique [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) para saber como configurar a Função no Azure AD

7. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

8. Na seção **Configurar o Zscaler ZSCloud**, copie a URL apropriada de acordo com seus requisitos.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-zscaler-zscloud-single-sign-on"></a>Configurar logon único do Zscaler ZSCloud

1. Para automatizar a configuração no Zscaler ZSCloud, é necessário instalar a **Extensão do navegador de Entrada Segura dos Meus Aplicativos**, clicando em **Instalar a extensão**.

    ![Extensão dos meus aplicativos](common/install-myappssecure-extension.png)

2. Após adicionar a extensão ao navegador, clicar em **Instalar o Zscaler ZSCloud** direcionará você ao aplicativo Zscaler ZSCloud. Nele, forneça as credenciais de administrador para entrar no Zscaler ZSCloud. A extensão do navegador configurará automaticamente o aplicativo e automatizará as etapas de 3 a 6.

    ![SSO de instalação](common/setup-sso.png)

3. Se desejar configurar o Zscaler ZSCloud manualmente, abra uma nova janela do navegador da Web, entre no site da empresa Zscaler ZSCloud como administrador e execute as seguintes etapas:

4. Vá até **Administração > Autenticação > Configurações de Autenticação** e execute as seguintes etapas:
   
    ![Administração](./media/zscaler-zscloud-tutorial/ic800206.png "Administração")

     a. Em Tipo de Autenticação, escolha **SAML**.

    b. Clique em **Configurar SAML**.

5. Na janela **Editar SAML**, execute as seguintes etapas: e clique em Salvar.  
            
    ![Gerenciar usuários e autenticação](./media/zscaler-zscloud-tutorial/ic800208.png "Gerenciar usuários e autenticação")
    
     a. Na caixa de texto **URL do Portal SAML**, cole o a **URL de Logon** copiada do portal do Azure.

    b. Na caixa de texto **Atributo de Nome de Logon**, insira **NameID**.

    c. Clique em **Upload** para fazer upload do certificado de assinatura SAML do Azure que você baixou do portal do Azure para o **Certificado SSL Público**.

    d. Acione **Habilitar Provisionamento Automático do SAML**.

    e. Na caixa de texto **Atributo de Nome de Exibição do Usuário**, insira **displayName** se você quiser habilitar o provisionamento automático do SAML para atributos de displayName.

    f. Na caixa de texto **Atributo de Nome do Grupo**, insira **memberOf** se você quiser habilitar o provisionamento automático do SAML para atributos de memberOf.

    g. Em **Atributo de Nome do Departamento**, insira **departamento** se você quiser habilitar o provisionamento automático do SAML para atributos de departamento.

    h. Clique em **Save** (Salvar).

6. Na página de caixa de diálogo **Configurar Autenticação de Usuário** , execute as seguintes etapas:

    ![Administração](./media/zscaler-zscloud-tutorial/ic800207.png)

     a. Passe o mouse sobre o menu **Ativação** na parte inferior esquerda.

    b. Clique em **Ativar**.

## <a name="configuring-proxy-settings"></a>Definindo as configurações de proxy
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para definir as configurações de proxy no Internet Explorer

1. Inicie o **Internet Explorer**.

2. Selecione **Opções da Internet** no menu **Ferramentas** para abrir a caixa de diálogo **Opções da Internet**.   
    
     ![Opções da Internet](./media/zscaler-zscloud-tutorial/ic769492.png "Opções da Internet")

3. Clique na guia **Conexões** .   
  
     ![Conexões](./media/zscaler-zscloud-tutorial/ic769493.png "Conexões")

4. Clique em **Configurações da LAN** para abrir a caixa de diálogo **Configurações da LAN**.

5. Na seção Servidor de proxy, execute as seguintes etapas:   
   
    ![Servidor proxy](./media/zscaler-zscloud-tutorial/ic769494.png "Servidor proxy")

     a. Selecione **Usar um servidor proxy para LAN**.

    b. Na caixa de texto Endereço, digite **gateway.Zscaler ZSCloud.net**.

    c. Na caixa de texto Porta, digite **80**.

    d. Selecione **Ignorar servidor proxy para endereços locais**.

    e. Clique em **OK** para fechar a caixa de diálogo **Configurações da Rede Local (LAN)** .

6. Clique em **OK** para fechar a caixa de diálogo **Opções da Internet**.

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

Nesta seção, você habilitará a Brenda Fernandes a usar o logon único do Azure através da concessão de acesso ao Zscaler ZSCloud.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Zscaler ZSCloud**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Zscaler ZSCloud**.

    ![O link do Zscaler ZSCloud na lista de aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, selecione o usuário como **Brenda Fernandes** na lista e, em seguida, clique no botão **Selecionar** na parte inferior da tela.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_users.png)

6. Na caixa de diálogo **Selecionar Função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_roles.png)

7. Na caixa de diálogo **Adicionar Atribuição** selecione o botão **Atribuir**.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_assign.png)

    >[!NOTE]
    >A função de acesso padrão não é compatível, pois isso interromperá o provisionamento; portanto, a função de usuário não pode ser selecionada ao atribuir um usuário.

### <a name="create-zscaler-zscloud-test-user"></a>Criar usuário de teste do Zscaler ZSCloud

Nesta seção, uma usuária chamada Brenda Fernandes será criada no Zscaler ZSCloud. O Zscaler ZSCloud é compatível com o provisionamento de usuário Just-In-Time, que é habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no Zscaler ZSCloud, um será criado após a autenticação.

>[!Note]
>Caso precise criar um usuário manualmente, contate a [equipe de suporte do Zscaler ZSCloud](https://help.zscaler.com/).

### <a name="test-single-sign-on"></a>Testar logon único 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do Zscaler ZSCloud no Painel de Acesso, deverá ser conectado automaticamente ao Zscaler ZSCloud para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

