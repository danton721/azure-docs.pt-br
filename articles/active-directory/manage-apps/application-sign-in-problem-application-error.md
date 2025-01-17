---
title: Erro em uma página de aplicativo após a entrada | Microsoft Docs
description: Como resolver problemas com a entrada do Azure AD quando o próprio aplicativo emite um erro
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: adfc96d2d7abf38c00f32a5d53615bb7c99c320e
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742379"
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Erro em uma página de aplicativo após a entrada

Nesse cenário, o Azure Active Directory conectou o usuário, mas o aplicativo está exibindo um erro que não permite ao usuário concluir o fluxo de entrada com êxito. Nesse cenário, o aplicativo não está aceitando a resposta emitida pelo AD do Azure.

Há alguns motivos possíveis por que o aplicativo não aceitou a resposta do Azure AD. Se o erro no aplicativo não for suficientemente claro para saber o que está faltando na resposta, então:

-   Se o aplicativo for a Galeria do Azure AD, verifique se você seguiu todas as etapas do artigo [Como depurar o Logon único baseado em SAML para aplicativos no Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Use uma ferramenta como [Fiddler](https://www.telerik.com/fiddler) para capturar solicitação SAML, resposta SAML e token SAML.

-   Compartilhe a resposta SAML com o fornecedor do aplicativo para saber o que está faltando.

## <a name="missing-attributes-in-the-saml-response"></a>Atributos ausentes na resposta SAML

Para adicionar um atributo na configuração do Azure Active Directory para ser enviado na resposta do Azure Active Directory, siga estas etapas:

1. Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador.**

2. Abra a **Extensão do Azure Active Directory** clicando em **Todos os serviços** na parte superior do menu de navegação esquerdo principal.

3. Digite **“Azure Active Directory**” na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4. clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5. clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se não vir o aplicativo desejado, use o controle **Filtro** na parte superior da **Lista com Todos os Aplicativos** e defina a opção **Mostrar** como **Todos os Aplicativos.**

6. Selecione o aplicativo para o qual você deseja configurar o logon único.

7. Após o carregamento do aplicativo, clique em **Logon único** no menu de navegação esquerdo do aplicativo.

8. clique em **Exibir e editar todos os outros atributos de usuário em** na seção **Atributos de Usuário** para editar os atributos a serem enviados ao aplicativo no token SAML quando o usuário entrar.

   Para adicionar um atributo:

   * clique em **Adicionar atributo**. Insira o **Nome** e selecione o **Valor** da lista suspensa.

   * Clique em **Salvar.** Você verá o novo atributo na tabela.

9. Salve a configuração.

Na próxima vez em que o usuário entrar no aplicativo, o Azure AD enviará o novo atributo na resposta SAML.

## <a name="the-application-doesnt-identify-the-user"></a>O aplicativo não identifica o usuário

Na entrada para o aplicativo está falhando porque a resposta SAML está faltando atributos como funções ou porque o aplicativo está esperando um formato diferente ou um valor para o atributo EntityID.

Se você estiver usando [do Azure AD provisionamento automatizado de usuários](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) para criar, manter e remover usuários no aplicativo. Em seguida, verifique se que o usuário foi provisionado com êxito para o aplicativo SaaS. Para obter mais informações, consulte [nenhum usuário está sendo provisionado para um aplicativo de galeria do Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem-no-users-provisioned)

## <a name="add-an-attribute-in-the-azure-ad-application-configuration"></a>Adicione um atributo na configuração do aplicativo do Azure AD:

Para alterar o valor do Identificador de Usuário, siga estas etapas:

1. Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador.**

2. Abra a **Extensão do Azure Active Directory** clicando em **Todos os serviços** na parte superior do menu de navegação esquerdo principal.

3. Digite **“Azure Active Directory**” na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4. clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5. clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se não vir o aplicativo desejado, use o controle **Filtro** na parte superior da **Lista com Todos os Aplicativos** e defina a opção **Mostrar** como **Todos os Aplicativos.**

6. Selecione o aplicativo para o qual você deseja configurar o logon único.

7. Após o carregamento do aplicativo, clique em **Logon único** no menu de navegação esquerdo do aplicativo.

8. Nos **Atributos de usuário**, selecione o identificador exclusivo para os usuários na lista suspensa **Identificador de usuário**.

## <a name="change-entityid-user-identifier-format"></a>Alterar o formato EntityID (Identificador de Usuário)

Se o aplicativo espera outro formato para o atributo EntityID. Então, não será possível selecionar o formato EntityID (Identificador de Usuário) que o Azure AD envia para o aplicativo na resposta após a autenticação do usuário.

O Azure AD seleciona o formato para o atributo NameID (Identificador de Usuário) com base no valor selecionado ou no formato solicitado pelo aplicativo no AuthRequest do SAML. Para obter mais informações, consulte o artigo [Protocolo SAML de Logon Único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) na seção NameIDPolicy.

## <a name="the-application-expects-a-different-signature-method-for-the-saml-response"></a>O aplicativo espera um método de assinatura diferente para a resposta SAML

Para alterar quais partes do token SAML são assinadas digitalmente pelo Active Directory do Azure. Siga estas etapas:

1. Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador.**

2. Abra a **Extensão do Azure Active Directory** clicando em **Todos os serviços** na parte superior do menu de navegação esquerdo principal.

3. Digite **“Azure Active Directory**” na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4. clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5. clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se não vir o aplicativo desejado, use o controle **Filtro** na parte superior da **Lista com Todos os Aplicativos** e defina a opção **Mostrar** como **Todos os Aplicativos.**

6. Selecione o aplicativo para o qual você deseja configurar o logon único.

7. Após o carregamento do aplicativo, clique em **Logon único** no menu de navegação esquerdo do aplicativo.

8. Clique em **Mostrar configurações avançadas de assinatura de certificado** na seção **Certificado de Autenticação SAML**.

9. Selecione a **Opção de Assinatura** apropriada esperada pelo aplicativo:

   * Assinar resposta SAML

   * Assinar resposta SAML e declaração

   * Assinar declaração SAML

Na próxima vez em que o usuário entrar no aplicativo, o Azure AD assinará a parte da resposta SAML selecionada.

## <a name="the-application-expects-the-signing-algorithm-to-be-sha-1"></a>O aplicativo espera que o algoritmo de assinatura seja SHA-1

Por padrão, o Azure AD assina o token SAML usando o algoritmo de maior segurança. Não é recomendável alterar o algoritmo de assinatura SHA-1, exceto se exigido pelo aplicativo.

Para alterar o algoritmo de assinatura, siga estas etapas:

1. Abra o [**Portal do Azure**](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador.**

2. Abra a **Extensão do Azure Active Directory** clicando em **Todos os serviços** na parte superior do menu de navegação esquerdo principal.

3. Digite **“Azure Active Directory**” na caixa de pesquisa do filtro e selecione o item **Azure Active Directory**.

4. clique em **Aplicativos Empresariais** no menu de navegação esquerdo do Azure Active Directory.

5. clique em **Todos os Aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se não vir o aplicativo desejado, use o controle **Filtro** na parte superior da **Lista com Todos os Aplicativos** e defina a opção **Mostrar** como **Todos os Aplicativos.**

6. Selecione o aplicativo para o qual você deseja configurar o logon único.

7. Após o carregamento do aplicativo, clique em **Logon único** no menu de navegação esquerdo do aplicativo.

8. Clique em **Mostrar configurações avançadas de assinatura de certificado** na seção **Certificado de Autenticação SAML**.

9. Selecione o SHA-1, no **Algoritmo de Assinatura**.

Na próxima vez em que o usuário entrar no aplicativo, o Azure AD assinará o token SAML usando o algoritmo SHA-1.

## <a name="next-steps"></a>Próximas etapas
[Como depurar o logon único baseado em SAML em aplicativos no Active Directory do Azure](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
