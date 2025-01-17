---
title: Configurar a inscrição e a entrada com a conta do Facebook usando o Azure Active Directory B2C | Microsoft Docs
description: Forneça a inscrição e entrada aos consumidores com contas do Facebook em seus aplicativos usando o Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6b90ba89f17b42f4d92c666c3f539fcad3e3227c
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733536"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e entrada com a conta do Facebook usando o Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Criar um aplicativo do Facebook

Para usar uma conta do Facebook como um [provedor de identidade](active-directory-b2c-reference-oauth-code.md) no Azure AD (Azure Active Directory) B2C, é preciso criar um aplicativo no locatário que o representa. Se ainda não tiver uma conta do Facebook, consiga uma conta em [https://www.facebook.com/](https://www.facebook.com/).

1. Entre no site [Desenvolvedores do Facebook](https://developers.facebook.com/) com suas credenciais de conta do Facebook.
2. Se ainda não tiver feito isso, você precisará registrar-se como desenvolvedor do Facebook. Para fazer isso, selecione **começar** no canto superior direito da página, aceite as políticas do Facebook e conclua as etapas de registro.
3. Selecione **meus aplicativos** e, em seguida **adicionar novo aplicativo**.
4. Insira um **Nome de Exibição** e um **Email de Contato** válido.
5. Clique em **Criar ID de Aplicativo**. Isso pode exigir a aceitação das políticas de plataforma do Facebook e a conclusão de uma verificação de segurança online.
6. Escolha **Configurações** > **Básicas**.
7. Escolha uma **Categoria**, por exemplo, `Business and Pages`. Esse valor é exigido pelo Facebook, mas não é usado para o Azure AD B2C.
8. Na parte inferior da página, escolha **Adicionar Plataforma** e, em seguida, escolha **Site**.
9. Na **URL do Site**, insira `https://your-tenant-name.b2clogin.com/` substituindo `your-tenant-name` pelo nome do seu locatário. Insira uma URL para a **URL da Política de Privacidade**, por exemplo `http://www.contoso.com`. A URL da política é uma página que você mantém para fornecer informações de privacidade para o seu aplicativo.
10. Selecione **Salvar alterações**.
11. Na parte superior da página, copie o valor de **ID do Aplicativo**.
12. Clique em **Mostrar** e copie o valor de **Segredo do Aplicativo**. Você usará ambos para configurar o Facebook como um provedor de identidade em seu locatário. **Segredo do Aplicativo** é uma credencial de segurança importante.
13. Selecione o sinal de adição ao lado **produtos**e, em seguida, selecione **configurar** sob **logon do Facebook**.
14. Sob **logon do Facebook**, selecione **configurações**.
15. Em **URIs de Redirecionamento do OAuth Válidos**, insira `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Substitua `your-tenant-name` pelo nome do seu locatário. Clique em **Salvar Alterações** na parte inferior da página.
16. Para disponibilizar seu aplicativo do Facebook ao Azure AD B2C, clique no seletor de Status na parte superior direita da página e **Ative-o** para tornar o aplicativo público e, depois, clique em **Confirmar**.  Neste ponto, o Status deverá mudar de **Desenvolvimento** para **Ativo**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configurar o Facebook como um provedor de identidade

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
2. Verifique se você está usando o diretório que contém o locatário do Azure AD B2C clicando no **filtro Diretório e assinatura** no menu superior e escolhendo o diretório que contém seu locatário.
3. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
4. Escolha **Provedores de identidade** e escolha **Adicionar**.
5. Insira um **Nome**. Por exemplo, insira *Facebook*.
6. Escolha **Tipo de provedor de identidade**, marque **Facebook** e clique em **OK**.
7. Escolha **Configurar este provedor de identidade** e insira a ID do Aplicativo que você registrou anteriormente como a **ID do Cliente** e insira o Segredo do Aplicativo que você registrou como o **Segredo do Cliente** do aplicativo de conta do Facebook criado anteriormente.
8. Clique em **OK** e em **Criar** para salvar sua configuração do Facebook.
