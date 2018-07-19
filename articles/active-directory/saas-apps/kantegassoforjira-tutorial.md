---
title: 'Tutorial: Integração do Azure Active Directory ao SSO do Kantega para o JIRA | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o SSO do Kantega para o JIRA.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ebbb61b8612418fbec37769ee56907ff99d5214a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228757"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Tutorial: Integração do Azure Active Directory ao SSO do Kantega para o JIRA

Neste tutorial, você aprende a integrar o SSO do Kantega para o JIRA ao Azure AD (Azure Active Directory).

A integração do SSO do Kantega para o JIRA ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao SSO do Kantega para o JIRA
- É possível permitir que os usuários se conectem automaticamente ao SSO do Kantega para o JIRA (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao SSO do Kantega para o JIRA, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do SSO do Kantega para o JIRA

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SSO do Kantega para o JIRA por meio da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Adicionando o SSO do Kantega para o JIRA por meio da galeria
Para configurar a integração do SSO do Kantega para o JIRA ao Azure AD, é necessário adicionar o SSO do Kantega para o JIRA à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o SSO do Kantega para o JIRA por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **SSO do Kantega para o JIRA**.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_search.png)

5. No painel de resultados, selecione **SSO do Kantega para o JIRA** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o SSO do Kantega para o JIRA, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do SSO do Kantega para o JIRA é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do SSO do Kantega para o JIRA.

No SSO do Kantega para o JIRA, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o SSO do Kantega para o JIRA, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criando um usuário de teste do SSO do Kantega para o JIRA](#creating-a-kantega-sso-for-jira-test-user)** – para ter um equivalente de Brenda Fernandes no SSO do Kantega para o JIRA que esteja vinculado à representação de usuário do Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo SSO do Kantega para o JIRA.

**Para configurar o logon único do Azure AD com o SSO do Kantega para o JIRA, realize as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **SSO do Kantega para o JIRA**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_samlbase.png)

3. No modo iniciado pelo **IDP**, na seção **Domínio e URLs do SSO do Kantega para o JIRA**, realize a seguinte etapa:

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_url1.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. No modo iniciado pelo **SP**, marque a opção **Mostrar configurações de URL avançadas** e realize a seguinte etapa:

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_url2.png)

    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Esses valores são recebidos durante a configuração do plug-in do Jira, que é explicada adiante no tutorial.

5. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_certificate.png) 

6. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_general_400.png)
    
7. Em outra janela do navegador da Web, faça logon no servidor local do JIRA como administrador.

8. Passe o cursor do mouse sobre a engrenagem e clique em **Complementos**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon1.png)

9. Na seção da guia Complementos, clique em **Localizar novos complementos**. Pesquise **SSO do Kantega para o JIRA (SAML e Kerberos)** e clique no botão **Instalar** para instalar o novo plug-in do SAML.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon2.png)

10. A instalação do plug-in é iniciada.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon3.png)

11. Quando a instalação for concluída. Clique em **fechar**

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon33.png)

12. Clique em **Gerenciar**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon34.png)
    
13. O novo plug-in é listado em **INTEGRAÇÕES**. Clique em **Configurar** para configurar o novo plug-in.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon35.png)

14. Na seção **SAML**. Selecione **Azure AD (Azure Active Directory)** na lista suspensa **Adicionar provedor de identidade**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon4.png)

15. Selecione o nível de assinatura como **Básico**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon5.png)       

16. Na seção **Propriedades do aplicativo**, realize as seguintes etapas: 

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon6.png)

    a. Copie o valor da **URI da ID do Aplicativo** e use-o como **o Identificador, a URL de Resposta e a URL de Logon** na seção **Domínio e URLs do SSO do Kantega para o JIRA** do portal do Azure.

    b. Clique em **Próximo**.

17. Na seção **Importação de metadados**, realize as seguintes etapas: 

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon7.png)

    a. Selecione **Arquivo de metadados no meu computador** e carregue um arquivo de metadados baixado no portal do Azure.

    b. Clique em **Próximo**.

18. Na seção **Nome e localização de SSO**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon8.png)
    
    a. Adicione Nome do Provedor de Identidade à caixa de texto **Nome do provedor de identidade** (por exemplo, Azure AD).

    b. Clique em **Próximo**.

19. Verifique o Certificado de autenticação e clique em **Avançar**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon9.png)

20. Na seção **Contas de usuário do JIRA**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon10.png)

    a. Selecione **Criar usuários no Diretório interno do JIRA, se necessário** e insira o nome apropriado do grupo de usuários (podem ser vários números de grupos separados por vírgula).

    b. Clique em **Próximo**.

21. Clique em **Concluir**.   

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon11.png)

22. Na seção **Domínios conhecidos do Azure AD**, realize as seguintes etapas: 

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/addon12.png)

    a. Selecione **Domínios conhecidos** no painel esquerdo da página.

    b. Insira o nome de domínio na caixa de texto **Domínios conhecidos**.

    c. Clique em **Salvar**. 

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforjira-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-kantega-sso-for-jira-test-user"></a>Criando um usuário de teste do SSO do Kantega para o JIRA

Para permitir que os usuários do Azure AD façam logon no JIRA, eles devem ser provisionados no JIRA. No SSO do Kantega para o JIRA, o provisionamento é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Faça logon no servidor local do JIRA como administrador.

2. Passe o cursor do mouse sobre a engrenagem e clique em **Gerenciamento de usuário**.

    ![Adicionar Funcionário](./media/kantegassoforjira-tutorial/user1.png) 

3. Na seção da guia **Gerenciamento de usuário**, clique em **Criar usuário**.

    ![Adicionar Funcionário](./media/kantegassoforjira-tutorial/user2.png) 

4. Na página da caixa de diálogo **"Criar novo usuário"**, execute as seguintes etapas:

    ![Adicionar Funcionário](./media/kantegassoforjira-tutorial/user3.png) 

    a. Na caixa de texto **Endereço de email**, digite o endereço de email do usuário, como Brittasimon@contoso.com.

    b. Na caixa de texto **Nome completo**, digite o nome completo do usuário, como Brenda Fernandes.

    c. Na caixa de texto **Nome de usuário**, digite o email do usuário, como Brittasimon@contoso.com.

    d. Na caixa de texto **Senha**, digite a senha do usuário.

    e. Clique em **Criar usuário**.   

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao SSO do Kantega para o JIRA.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao SSO do Kantega para o JIRA, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **SSO do Kantega para o JIRA**.

    ![Configurar o logon único](./media/kantegassoforjira-tutorial/tutorial_kantegassoforjira_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do SSO do Kantega para o JIRA no Painel de Acesso, deverá ser conectado automaticamente ao aplicativo SSO do Kantega para o JIRA.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/kantegassoforjira-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforjira-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforjira-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforjira-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforjira-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforjira-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforjira-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforjira-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforjira-tutorial/tutorial_general_203.png
