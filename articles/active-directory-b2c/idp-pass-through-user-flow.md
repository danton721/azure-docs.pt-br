---
title: Passar um token de acesso por meio de um fluxo de usuário para seu aplicativo - Azure Active Directory B2C | Microsoft Docs
description: Saiba como você pode passar por meio de um token de acesso para provedores de identidade do OAuth 2.0 como uma declaração de um fluxo de usuário no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: db4b799aa31a4132609b0dd158b65070fb2474b7
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66510961"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>Passar um token de acesso por meio de um fluxo de usuário para seu aplicativo no Azure Active Directory B2C

> [!NOTE]
> Esse recurso está atualmente em visualização pública.

Um [fluxo de usuário](active-directory-b2c-reference-policies.md) no Azure Active Directory (Azure AD) B2C fornece aos usuários do seu aplicativo uma oportunidade de se inscrever ou entrar com um provedor de identidade. Quando a jornada é iniciada, o Azure AD B2C recebe um [token de acesso](active-directory-b2c-reference-tokens.md) do provedor de identidade. O Azure Active Directory B2C usa esse token para recuperar informações sobre o usuário. Habilitar uma declaração no seu fluxo de usuário para passar o token por meio para os aplicativos que você se registrar no Azure AD B2C.

O Azure Active Directory B2C atualmente suporta apenas passar o token de acesso dos provedores de identidade do [OAuth 2.0](active-directory-b2c-reference-oauth-code.md), que incluem [Facebook](active-directory-b2c-setup-fb-app.md) e [Google](active-directory-b2c-setup-goog-app.md). Para todos os outros provedores de identidade, a declaração é retornada em branco.

## <a name="prerequisites"></a>Pré-requisitos

- Seu aplicativo deve estar usando um [fluxo de usuário v2](user-flow-versions.md).
- Seu fluxo de usuário é configurado com um provedor de identidade do OAuth 2.0.

## <a name="enable-the-claim"></a>Habilitar a declaração

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
2. Verifique se que você estiver usando o diretório que contém o seu locatário do Azure AD B2C. Selecione o **filtro de diretório e assinatura** no menu superior e escolha o diretório que contém o seu locatário.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Selecione **fluxos de usuário (diretivas)** e, em seguida, selecione seu fluxo de usuário. Por exemplo, **B2C_1_signupsignin1**.
5. Selecione **Declarações do aplicativo**.
6. Habilitar o **Token de acesso do provedor de identidade** de declaração.

    ![Permitir que a declaração de Token de acesso do provedor identidade](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. Clique em **Salvar** para salvar o fluxo de usuário.

## <a name="test-the-user-flow"></a>Testar o fluxo de usuário

Ao testar seus aplicativos no Azure AD B2C, pode ser útil ter o token do Azure AD B2C retornado ao `https://jwt.ms` para examinar as declarações.

1. Na página de visão geral do fluxo de usuário, selecione **Executar o fluxo de usuário**.
2. Para **Aplicativo**, selecione seu aplicativo que você registrou anteriormente. Para ver o token no exemplo a seguir, o **URL de resposta** deve mostrar `https://jwt.ms`.
3. Clique em **Executar o fluxo de usuário**, e em seguida, entre com suas credenciais de conta. Você deve ver o token de acesso do provedor de identidade na declaração **idp_access_token**.

    Você deverá ver algo semelhante ao texto a seguir:

    ![Token decodificado](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.png)

## <a name="next-steps"></a>Próximas etapas

Saiba mais na [visão geral dos tokens do Azure AD B2C](active-directory-b2c-reference-tokens.md).




