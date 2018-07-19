---
title: 'Tutorial: integração do Azure Active Directory ao Degreed | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Degreed.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1eda2d1c-b5e2-4c53-ad46-bbeb91cd119a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: bea70a0fe038046f4b39116c730327e26d0f4b11
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215117"
---
# <a name="tutorial-azure-active-directory-integration-with-degreed"></a>Tutorial: integração do Azure Active Directory com o Degreed

Neste tutorial, você aprende a integrar o Degreed ao Azure AD (Azure Active Directory).

A integração do Degreed ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar  quem tem acesso ao Degreed
- Você pode permitir que usuários façam logon automaticamente no Degreed (Logon Único) com as respectivas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o Degreed, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Degreed

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Degreed a partir da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-degreed-from-the-gallery"></a>Adição do Degreed a partir da galeria
Para configurar a integração do Degreed ao Azure AD, você precisa adicionar o Degreed por meio da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Degreed da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **Degreed**.

    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/tutorial_degreed_search.png)

5. No painel de resultados, selecione **Degreed** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/tutorial_degreed_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Degreed com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Degreed é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Degreed.

No Degreed, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Degreed, você precisará concluir os seguintes blocos de construção:

1. **[Configuração do logon único do AD do Azure](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários para usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Como criar um usuário de teste do Degreed](#creating-a-degreed-test-user)** – para ter um equivalente de Brenda Fernandes no Degreed que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Degreed.

**Para configurar o logon único do Azure AD com o Degreed, realize as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Degreed**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/degreed-tutorial/tutorial_degreed_samlbase.png)

3. Na seção **URLs e Domínio do Degreed**, execute as seguintes etapas:

    ![Configurar o logon único](./media/degreed-tutorial/tutorial_degreed_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://degreed.com/?orgsso=<company code>`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://degreed.com/<instancename>`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao cliente do Degreed](mailTo:admin@degreed.com) para obter esses valores. 
 


4. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/degreed-tutorial/tutorial_degreed_certificate.png) 

5. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/degreed-tutorial/tutorial_general_400.png)

6. Para configurar o logon único no lado do **Degreed**, é necessário enviar o **XML de metadados** baixado para a [equipe de suporte do Degreed](mailTo:admin@degreed.com). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/degreed-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-degreed-test-user"></a>Criar um usuário de teste do Degreed

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Degreed. O Degreed dá suporte ao provisionamento just-in-time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o Degreed, caso ele ainda não exista.

> [!NOTE]
> Se você precisar criar um usuário manualmente, entre em contato com a [equipe de suporte do Degreed](mailTo:admin@degreed.com).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Degreed.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Degreed, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Degreed**.

    ![Configurar o logon único](./media/degreed-tutorial/tutorial_degreed_app.png) 

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco Degreed no Painel de Acesso, você deve ser conectado automaticamente ao seu aplicativo Degreed.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


<!--Image references-->

[1]: ./media/degreed-tutorial/tutorial_general_01.png
[2]: ./media/degreed-tutorial/tutorial_general_02.png
[3]: ./media/degreed-tutorial/tutorial_general_03.png
[4]: ./media/degreed-tutorial/tutorial_general_04.png

[100]: ./media/degreed-tutorial/tutorial_general_100.png

[200]: ./media/degreed-tutorial/tutorial_general_200.png
[201]: ./media/degreed-tutorial/tutorial_general_201.png
[202]: ./media/degreed-tutorial/tutorial_general_202.png
[203]: ./media/degreed-tutorial/tutorial_general_203.png
