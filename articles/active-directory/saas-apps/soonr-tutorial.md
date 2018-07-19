---
title: 'Tutorial: Integração do Azure Active Directory com o Soonr Workplace | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o Soonr Workplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: bdcce907a5c6c20263edb969f86036d158cc5453
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213185"
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Tutorial: Integração do Active Directory do Azure com o Soonr Workplace

Neste tutorial, você aprende a integrar o Soonr Workplace ao Azure AD (Azure Active Directory).

A integração do Soonr Workplace ao Azure AD oferece os seguintes benefícios:

- No AD do Azure, você pode controlar quem tem acesso ao Soonr Workplace
- Você pode habilitar o logon automático de seus usuários no Soonr Workplace (Logon único) com as contas do AD do Azure
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do AD do Azure ao Soonr Workplace, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Soonr Workplace com logon único habilitado

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Como adicionar o Soonr Workplace a partir da Galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-soonr-workplace-from-the-gallery"></a>Como adicionar o Soonr Workplace a partir da Galeria
Para configurar a integração do Soonr Workplace ao AD do Azure, você precisa adicionar o Soonr Workplace da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Soonr Workplace da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **Soonr Workplace**.

    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/tutorial_soonr_search.png)

5. No painel de resultados, selecione **Soonr Workplace** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/tutorial_soonr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o Soonr Workplace, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Soonr Workplace é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do AD do Azure e o usuário relacionado do Soonr Workplace.

No Soonr Workplace, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do AD do Azure com o Soonr Workplace, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do Soonr Workplace](#creating-a-soonr-workplace-test-user)** : para ter um equivalente de Brenda Fernandes no Soonr Workplace que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure e configurará o logon único no aplicativo do Soonr Workplace.

**Para configurar o logon único do AD do Azure com o Soonr Workplace, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **Soonr Workplace**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_samlbase.png)

3. Na seção **Domínio e URLs do Soonr Workplace**, realize as seguintes etapas:

    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_url.png)

    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/metadata`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/SSO`

4. Na seção **Domínio e URLs do Soonr Workplace**, se você desejar configurar o aplicativo no **Modo iniciado por SP**, siga as etapas abaixo:
    
    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_url1.png)

    a. Clique em **Mostrar configurações de URL avançadas**.

    b. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/SSO`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador, URL de logon e a URL de Resposta reais. Entre em contato com a [equipe de suporte do Soonr Workplace](https://awp.autotask.net/help/) para obter esses valores.
 
5. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_certificate.png) 

6. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/soonr-tutorial/tutorial_general_400.png)

7. Na seção **Configuração do Soonr Workplace**, clique em **Configurar o Soonr Workplace** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_configure.png) 

8. Para configurar o logon único no lado do **Soonr Workplace**, é necessário enviar o **XML de metadados**, a **URL de Saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** baixados para a [equipe de suporte do Soonr Workplace](https://awp.autotask.net/help/). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

    >[!Note]
    >Se você precisar de assistência para configurar o Autotask Workplace, consulte [essa página](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter ajuda com sua conta do Workplace.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/soonr-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-soonr-workplace-test-user"></a>Criar um usuário de teste do Soonr Workplace

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Soonr Workplace. Trabalhe com a [equipe de suporte do Soonr Workplace](https://awp.autotask.net/help/) para criar um usuário na plataforma. Você pode gerar o tíquete de suporte do Soonr <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">aqui</a>.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Soonr Workplace.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Soonr Workplace, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Soonr Workplace**.

    ![Configurar o logon único](./media/soonr-tutorial/tutorial_soonr_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.  

Quando você clica no bloco Soonr Workplace no Painel de Acesso, você deve ser conectado automaticamente ao seu aplicativo Soonr Workplace.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/soonr-tutorial/tutorial_general_01.png
[2]: ./media/soonr-tutorial/tutorial_general_02.png
[3]: ./media/soonr-tutorial/tutorial_general_03.png
[4]: ./media/soonr-tutorial/tutorial_general_04.png

[100]: ./media/soonr-tutorial/tutorial_general_100.png

[200]: ./media/soonr-tutorial/tutorial_general_200.png
[201]: ./media/soonr-tutorial/tutorial_general_201.png
[202]: ./media/soonr-tutorial/tutorial_general_202.png
[203]: ./media/soonr-tutorial/tutorial_general_203.png
