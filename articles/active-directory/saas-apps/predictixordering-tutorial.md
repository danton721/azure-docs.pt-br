---
title: 'Tutorial: Integração do Azure Active Directory ao Predictix Ordering | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Predictix Ordering.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: d4b4832a0a69ab886529bf304a7a89768ccb830f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214188"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a>Tutorial: Integração do Azure Active Directory com o Predictix Ordering

Neste tutorial, você aprenderá como integrar o Predictix Ordering ao Azure AD (Azure Active Directory).

Integração do Predictix Ordering com o Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Predictix Ordering.
- Você pode permitir que seus usuários façam logon automaticamente no Predictix Ordering (logon único) com suas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao Predictix Ordering, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada do Predictix Ordering com logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Predictix Ordering da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-predictix-ordering-from-the-gallery"></a>Adição do Predictix Ordering da galeria
Para configurar a integração do Predictix Ordering ao Azure AD, você precisa adicionar o Predictix Ordering da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Predictix Ordering da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **Predictix Ordering**, selecione **Predictix Ordering** no painel de resultados e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Predictix Ordering na lista de resultados](./media/predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Predictix Ordering, com base em uma usuária de teste chamada "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Predictix Ordering é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Predictix Ordering.

No Predictix Ordering, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Predictix Ordering, conclua os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste Predictix Ordering](#create-a-predictix-ordering-test-user)** – para ter um equivalente de Brenda Fernandes no Predictix Ordering vinculado à representação do usuário do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure e configurará o logon único em seu aplicativo Predictix Ordering.

**Para configurar o logon único do Azure AD com o Predictix Ordering, execute as seguintes etapas:**

1. No portal do Azure, na página de integração de aplicativos do **Predictix Ordering**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

3. Na seção **URLs e Domínio do Predictix Ordering**, execute as seguintes etapas:

    ![Informações de logon único de URLs e Domínio do Predictix Ordering](./media/predictixordering-tutorial/tutorial_predictixordering_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<companyname-pricing>.ordering.predictix.com/sso/request`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: 
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Entre em contato com a [equipe de suporte do Cliente do Predictix Ordering](https://www.predix.io/support/) para obter esses valores. 
 
4. Na seção **Certificado de Autenticação SAML**, clique em **Certificado (Base64)** e, em seguida, salve o arquivo do certificado em seu computador.

    ![O link de download do Certificado](./media/predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

5. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/predictixordering-tutorial/tutorial_general_400.png)

6. Na seção **Configuração do Predictix Ordering**, clique em **Configurar Predictix Ordering** para abrir a janela **Configurar logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configuração do Predictix Ordering](./media/predictixordering-tutorial/tutorial_predictixordering_configure.png) 

7. Para configurar o logon único no lado do **Predictix Ordering**, é necessário enviar o **Certificado (Base64)** baixado, a **URL de Saída, a ID da Entidade SAML e a URL do Serviço de Logon Único do SAML** para a [equipe de suporte do Predictix Ordering](https://www.predix.io/support/). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/predictixordering-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/predictixordering-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/predictixordering-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/predictixordering-tutorial/create_aaduser_04.png)

   a. Na caixa **Nome**, digite **BrendaFernandes**.

   b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

   c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

   d. Clique em **Criar**.
 
### <a name="create-a-predictix-ordering-test-user"></a>Criar um usuário de teste do Predictix Ordering

Nesta seção, você criará um usuário chamado Brenda Fernandes no Predictix Ordering. Trabalhe com a [equipe de suporte do Predictix Ordering](https://www.predix.io/support/) para adicionar os usuários à plataforma Predictix Ordering.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você habilitará Brenda Fernandes a usar o logon único do Azure concedendo acesso ao Predictix Ordering.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao Predictix Ordering, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Predictix Ordering**.

    ![Link do Predictix Ordering na lista de aplicativos](./media/predictixordering-tutorial/tutorial_predictixordering_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco Predictix Ordering no Painel de Acesso, deve ser conectado automaticamente ao seu aplicativo Predictix Ordering.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/predictixordering-tutorial/tutorial_general_203.png
