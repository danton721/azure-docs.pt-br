---
title: 'Tutorial: Integração do Azure Active Directory com o OfficeSpace Software | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o OfficeSpace Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5e77a8d557c44b76335ac4abab2cb19e099c7d8b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36215912"
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Tutorial: Integração do Active Directory do Azure com o OfficeSpace Software

Neste tutorial, você aprende a integrar o OfficeSpace Software ao Azure AD (Azure Active Directory).

A integração do OfficeSpace Software ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao OfficeSpace Software.
- Você pode permitir que os usuários sejam conectados automaticamente ao OfficeSpace Software (Logon Único) com as respectivas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD ao OfficeSpace Software, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do OfficeSpace Software habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o OfficeSpace Software por meio da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-officespace-software-from-the-gallery"></a>Adicionando o OfficeSpace Software por meio da galeria
Para configurar a integração do OfficeSpace Software no Azure AD, é necessário adicionar o OfficeSpace Software por meio da galeria à lista de aplicativos SaaS gerenciados.

**Para adicionar o OfficeSpace Software por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

4. Na caixa de pesquisa, digite **OfficeSpace Software**, selecione **OfficeSpace Software** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![OfficeSpace Software na lista de resultados](./media/officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configura e testa o logon único do Azure AD com o OfficeSpace Software, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do OfficeSpace Software é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do OfficeSpace Software.

No OfficeSpace Software, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o OfficeSpace Software, é necessário concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criar um usuário de teste do OfficeSpace Software](#create-a-officespace-software-test-user)** – para ter um equivalente de Brenda Fernandes no OfficeSpace Software que esteja vinculado à representação do usuário no Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo OfficeSpace Software.

**Para configurar o logon único do Azure AD com o OfficeSpace Software, realize as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **OfficeSpace Software**, clique em **Logon único**.

    ![Link Configurar logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/officespace-tutorial/tutorial_officespace_samlbase.png)

3. Na seção **URLs e Domínio do OfficeSpace Software**, execute as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do OfficeSpace Software](./media/officespace-tutorial/tutorial_officespace_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `<company name>.officespacesoftware.com`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. Contate a [equipe de suporte ao cliente do OfficeSpace Software](mailto:support@officespacesoftware.com) para obter esses valores. 

4. O aplicativo OfficeSpace Software espera que as declarações SAML estejam em um formato específico. Configure as seguintes declarações para o aplicativo. Você pode gerenciar os valores desses atributos da seção "**Atributos de Usuário**" na página de integração do aplicativo. A captura de tela a seguir mostra um exemplo disso.
    
    ![Configurar atributo](./media/officespace-tutorial/tutorial_officespace_attribute.png)

5. Na seção **Atributos do Usuário**, na caixa de diálogo **Logon único**, selecione **user.mail** como **Identificador de Usuário** e, para cada linha mostrada na tabela a seguir, execute as seguintes etapas:
    
    | Nome do atributo | Valor do atributo |
    | --- | --- |    
    | email | user.mail |
    | Nome | user.displayname |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar Adicionar ](./media/officespace-tutorial/tutorial_attribute_04.png)

    ![Configurar atributo](./media/officespace-tutorial/tutorial_attribute_05.png)
    
    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.
    
    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.
    
    d. Clique em **Ok**
 
6. Na seção **Certificado de Autenticação SAML**, copie o valor da **IMPRESSÃO DIGITAL** do certificado.

    ![O link de download do Certificado](./media/officespace-tutorial/tutorial_officespace_certificate.png) 

7. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/officespace-tutorial/tutorial_general_400.png)

8. Na seção **Configuração do OfficeSpace Software**, clique em **Configurar o OfficeSpace Software** para abrir a janela **Configurar logon**. Copie a **URL de Saída e a URL do Serviço de Logon Único SAML** da **seção Referência Rápida.**

    ![Configuração do OfficeSpace Software](./media/officespace-tutorial/tutorial_officespace_configure.png) 

9. Em outra janela do navegador da Web, faça logon no locatário do OfficeSpace Software como administrador.

10. Acesse **Configurações** e clique em **Conectores**.

    ![Configurar o logon único no lado do aplicativo](./media/officespace-tutorial/tutorial_officespace_002.png)

11. Clique em **Autenticação SAML**.

    ![Configurar o logon único no lado do aplicativo](./media/officespace-tutorial/tutorial_officespace_003.png)

12. Na seção **Autenticação SAML** , realize as seguintes etapas:

    ![Configurar o logon único no lado do aplicativo](./media/officespace-tutorial/tutorial_officespace_004.png)

    a. Na caixa de texto **URL do provedor de logoff**, cole o valor da **URL de Saída** copiado do Portal do Azure.

    b. Na caixa de texto **URL de destino de IdP do cliente**, cole o valor da **URL do Serviço de Logon Único SAML** copiado do Portal do Azure.

    c. Cole o valor da **Impressão digital** copiado do Portal do Azure na caixa de texto **Impressão digital de certificado de IdP do cliente**. 

    d. Clique em **Salvar Configurações**.


> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/officespace-tutorial/create_aaduser_01.png)

2. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/officespace-tutorial/create_aaduser_02.png)

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/officespace-tutorial/create_aaduser_03.png)

4. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/officespace-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-officespace-software-test-user"></a>Criar um usuário de teste do OfficeSpace Software

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no OfficeSpace Software. O OfficeSpace Software dá suporte ao provisionamento Just-In-Time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o OfficeSpace Software, caso ele ainda não exista.

> [!NOTE]
> Se precisar criar um usuário manualmente, será necessário contatar a [equipe de suporte do OfficeSpace Software](mailto:support@officespacesoftware.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao OfficeSpace Software.

![Atribuir a função de usuário][200] 

**Para atribuir Brenda Fernandes ao OfficeSpace Software, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **OfficeSpace Software**.

    ![O link do OfficeSpace Software na lista de Aplicativos](./media/officespace-tutorial/tutorial_officespace_app.png)  

3. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco OfficeSpace Software no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo OfficeSpace Software.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/officespace-tutorial/tutorial_general_01.png
[2]: ./media/officespace-tutorial/tutorial_general_02.png
[3]: ./media/officespace-tutorial/tutorial_general_03.png
[4]: ./media/officespace-tutorial/tutorial_general_04.png

[100]: ./media/officespace-tutorial/tutorial_general_100.png

[200]: ./media/officespace-tutorial/tutorial_general_200.png
[201]: ./media/officespace-tutorial/tutorial_general_201.png
[202]: ./media/officespace-tutorial/tutorial_general_202.png
[203]: ./media/officespace-tutorial/tutorial_general_203.png
