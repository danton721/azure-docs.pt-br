---
title: 'Tutorial: integração do Azure Active Directory com o LinkedIn Learning | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: ffa689e9556e57560138d9629c616bd3a284f9b6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36222300"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: integração do Azure Active Directory com o LinkedIn Learning

Neste tutorial, você aprenderá a integrar o LinkedIn Learning ao Azure AD (Azure Active Directory).

A integração do LinkedIn Learning com o Azure AD oferece os seguintes benefícios:

- É possível controlar, no Azure AD, quem tem acesso ao LinkedIn Learning
- É possível permitir que seus usuários façam logon automaticamente no LinkedIn Learning (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>pré-requisitos

Para configurar a integração do Azure AD com o LinkedIn Learning, serão necessários os seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do LinkedIn Learning habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o LinkedIn Learning da galeria
2. configurar e testar o logon único do AD do Azure

## <a name="adding-linkedin-learning-from-the-gallery"></a>Adicionando o LinkedIn Learning da galeria
Para configurar a integração do LinkedIn Learning com o Azure AD, é necessário adicionar o LinkedIn Learning da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o LinkedIn Learning da galeria, siga as etapas abaixo:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

2. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![APLICATIVOS][3]

4. Na caixa de pesquisa, digite **LinkedIn Learning**. No painel de resultados, clique em **LinkedIn Learning** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o LinkedIn Learning, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do LinkedIn Learning é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do LinkedIn Learning.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no LinkedIn Learning.

Para configurar e testar o logon único do Azure AD com o LinkedIn Learning, é necessário concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do Azure AD com Brenda Fernandes.
3. **[Criando um usuário de teste do LinkedIn Learning](#creating-a-linkedin-learning-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você vai habilitar o logon único do Azure AD no portal do Azure e configurar o logon único em seu aplicativo LinkedIn Learning.

**Para configurar o logon único do Azure AD com o LinkedIn Learning, siga as etapas abaixo:**

1. No Portal do Azure, na página de integração de aplicativos do **LinkedIn Learning**, clique em **Logon único**.

    ![Configurar o logon único][4]

2. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. Em uma janela diferente do navegador da Web, faça logon em seu locatário do LinkedIn Learning como administrador.

4. Em **Centro de Contas**, clique em **Configurações Globais** em **Configurações**. Além disso, selecione **Learning – Padrão** na lista suspensa.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Clique em **OU clique aqui para carregar e copiar campos individuais do formulário** e copie a **ID da Entidade** e a **URL do ACS (Serviço do Consumidor de Declaração)**

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. No portal do Azure, em **URLs e Domínio do LinkedIn Learning**, siga as etapas abaixo para configurar o SSO no modo **iniciado pelo IdP**

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. Na caixa de texto **Identificador**, insira a **ID da Entidade** copiada do Portal do LinkedIn 

    b. Na caixa de texto **URL de Resposta**, insira a **URL ACS (acesso do consumidor de declaração)** copiada do Portal do LinkedIn

7. Se você desejar configurar o SSO em **Iniciado pelo SP**, clique na opção Mostrar Configurações Avançadas de URL na seção de configuração e configure o logon na URL com o seguinte padrão:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. Seu aplicativo LinkedIn Learning espera as asserções SAML em um formato específico, o que exige que você adicione mapeamentos de atributos personalizados à sua configuração de atributos de token SAML. A captura de tela a seguir mostra um exemplo disso. O valor padrão do **identificador de usuário** é **user.userprincipalname**, mas o LinkedIn Learning espera que isso seja mapeado com o endereço de email do usuário. Para que você possa usar o atributo **user. mail** na lista ou usar o valor do atributo apropriado com base na configuração da sua organização. 

    ![Configurar o logon único](./media/linkedinlearning-tutorial/updateusermail.png)
    
9. Na seção **Atributos de Usuário**, clique em **Exibir e editar todos os outros atributos de usuário** e defina os atributos. O usuário precisa adicionar quatro declarações denominadas **email**, **departamento**, **nome** e **sobrenome** e o valor deve ser mapeado com **user.mail**, **user.department**, **user.givenname** e **user.surname** respectivamente

    | Nome do atributo | Valor do atributo |
    | --- | --- |
    | email| user.mail |    
    | department| user.department |
    | nome| user.givenname |
    | sobrenome| user.surname |
    
    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/userattribute.png)
    
    a. Clique em **Adicionar Atributo** para abrir a caixa de diálogo do atributo.

    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/tutorial_attribute_04.png)

    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/tutorial_attribute_05.png)
    
    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.
    
    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.
    
    d. Clique em **Ok**

10. Realize as seguintes etapas no atributo **name**-

    a. Clique no atributo para abrir a janela **Editar Atributo**.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/url_update.png)

    b. Exclua o valor da URL do **namespace**.
    
    c. Clique em **OK** para salvar a configuração.

11. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo XML em seu computador.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png)

12. Clique em **Salvar**.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_general_400.png)

13. Vá para a seção **Configurações de administração do LinkedIn**. Carregue o arquivo XML que você baixou do portal do Azure clicando na opção Carregar Arquivo XML.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Clique em **Ativar** para habilitar o SSO. O status do SSO é alterado de **Não Conectado** para **Conectado**

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/create_aaduser_01.png) 

2. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/create_aaduser_02.png) 

3. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/create_aaduser_03.png) 

4. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.

### <a name="creating-a-linkedin-learning-test-user"></a>Criando um usuário de teste do LinkedIn Learning

O aplicativo LinkedIn Learning dá suporte ao provisionamento de usuário just in time e, após a autenticação, os usuários serão criados automaticamente no aplicativo. Na página de configurações de administração no portal do LinkedIn Learning, coloque a opção **Atribuir licenças automaticamente** na posição de ativar o provisionamento just in time, o que também atribuirá uma licença ao usuário. O LinkedIn Learning também dá suporte ao provisionamento automático do usuário. É possível encontrar [aqui](linkedinlearning-provisioning-tutorial.md) detalhes sobre como configurar o provisionamento automático do usuário.

   ![Criação de um usuário de teste do AD do Azure](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure ao conceder acesso ao LinkedIn Learning.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao LinkedIn Learning, siga as etapas abaixo:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201]

2. Na lista de aplicativos, selecione **LinkedIn Learning**.

    ![Configurar o logon único](./media/linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png)

3. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202]

4. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

5. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

6. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

7. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clica no bloco LinkedIn Learning no Painel de Acesso, você deve obter a página de logon do Azure e, após o logon bem-sucedido, você deve entrar em seu aplicativo LinkedIn Learning.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar Provisionamento de Usuário](linkedinlearning-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/linkedinlearning-tutorial/tutorial_general_203.png