---
title: SAML SSO para aplicativos locais com o Proxy de aplicativo do Active Directory do Azure (visualização) | Microsoft Docs
description: Saiba como fornecer o logon único para o local aplicativos publicados por meio do Proxy de aplicativo que são protegidas com a autenticação de SAML.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 907cb598d708bfa26f53d2e43fef5456258c21b1
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393041"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>SAML SSO para aplicativos locais com o Proxy de aplicativo (visualização)

Você pode fornecer o logon único (SSO) para aplicativos locais que são protegidos com a autenticação do SAML e fornecem acesso remoto a esses aplicativos por meio do Proxy de aplicativo. Com o SAML SSO, Azure Active Directory (Azure AD) autentica o aplicativo usando a conta do usuário do Azure AD. O Azure AD comunica as informações do logon para o aplicativo por meio de um protocolo de conexão. Você também pode mapear os usuários a funções de aplicativo específicas com base em regras que você define em suas declarações SAML. Habilitando o Proxy de aplicativo, além de SSO do SAML, os usuários terão acesso externo ao aplicativo e uma experiência de SSO contínua.

Os aplicativos devem ser capazes de consumir tokens SAML emitidos pelo **Azure Active Directory**. Essa configuração não se aplica a aplicativos usando um provedor de identidade local. Para esses cenários, é recomendável revisar [recursos para migrar aplicativos para o Azure AD](migration-resources.md).

SSO do SAML com o Proxy de aplicativo também funciona com o recurso de criptografia do token SAML. Para obter mais informações, consulte [criptografia do token de configurar o Azure AD SAML](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Publicar o aplicativo local com o Proxy de aplicativo

Antes de poder fornecer SSO para aplicativos locais, verifique se você tiver habilitado o Proxy de aplicativo e você tiver um conector instalado. Ver [adicionar um aplicativo local para acesso remoto por meio do Proxy de aplicativo no Azure AD](application-proxy-add-on-premises-application.md) para saber como.

Tenha em mente o seguinte quando você vai o tutorial:

* Publique seu aplicativo seguindo as instruções no tutorial. Certifique-se de selecionar **Azure Active Directory** como o **pré-autenticação** método de seu aplicativo (etapa 4 no [adicionar um aplicativo local ao Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Cópia de **URL externa** para o aplicativo.
* Como prática recomendada, use domínios personalizados, sempre que possível para uma experiência de usuário otimizadas. Saiba mais sobre [trabalhando com domínios personalizados no Proxy de aplicativo do Azure AD](application-proxy-configure-custom-domain.md).
* Adicione pelo menos um usuário para o aplicativo e certifique-se que a conta de teste tenha acesso ao aplicativo local. Usando o teste da conta de teste, se você pode alcançar o aplicativo visitando a **URL externa** validar o Proxy de aplicativo está configurado corretamente. Para obter informações de solução de problemas, consulte [problemas de solucionar problemas de Proxy de aplicativo e mensagens de erro](application-proxy-troubleshoot.md).

## <a name="set-up-saml-sso"></a>Configurar o SSO do SAML

1. No portal do Azure, selecione **Azure Active Directory > aplicativos empresariais** e selecione o aplicativo na lista.
1. Do aplicativo do **visão geral** página, selecione **logon único**.
1. Selecione **SAML** como o método de logon único.
1. No **definir o logon único com SAML** página, edite o **básicas de configuração de SAML** dados e siga as etapas [Enter básicas de configuração SAML](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) configurar baseado em SAML autenticação para o aplicativo.

   * Certifique-se a **URL de resposta** corresponde a **URL externa** para o aplicativo no local que é publicado por meio do Proxy de aplicativo ou é um caminho no **URL externa**.
   * Para um fluxo iniciado por IDP em que seu aplicativo requer um outro **URL de resposta** para a configuração de SAML, adicione isso como um **adicionais** URL na lista e marcar a caixa de seleção ao lado dele para designá-lo como o primário **URL de resposta**.
   * Para um fluxo iniciado por SP, verifique se o aplicativo de back-end Especifica correto **URL de resposta** ou URL de serviço do consumidor de declaração a ser usado para receber o token de autenticação.

     ![Inserir dados de configuração básicos do SAML](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Se o aplicativo de back-end espera que o **URL de resposta** para ser a URL interna, você precisará usar [domínios personalizados](application-proxy-configure-custom-domain.md) têm correspondência URLS internas e externas ou instalar a extensão meus aplicativos segura entrar em dispositivos dos usuários. Essa extensão será automaticamente redirecionada para o serviço de Proxy de aplicativo apropriado. Para instalar a extensão, consulte [extensão de entrada segura dos meus aplicativos](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Testar seu aplicativo

Depois de concluir todas essas etapas, seu aplicativo estará pronto para execução. Para testar o aplicativo:

1. Abra um navegador e navegue até a URL externa que você criou quando publicou o aplicativo. 
1. Entre com a conta de teste que você atribuiu ao aplicativo. Você deve ser capaz de carregar o aplicativo e ter o SSO no aplicativo.

## <a name="next-steps"></a>Próximas etapas

- [Como o Proxy de Aplicativo do Azure AD fornece logon único?](application-proxy-single-sign-on.md)
- [Solucionar problemas de Proxy de Aplicativo](application-proxy-troubleshoot.md)
