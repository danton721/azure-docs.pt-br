---
title: Sem senha entrar com o aplicativo Microsoft Authenticator (visualização) – Azure Active Directory
description: Entrar no Azure AD usando o aplicativo Microsoft Authenticator sem usar sua senha (versão prévia pública)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb10378d890c2b7156b6764321e177a22ffc538a
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66472748"
---
# <a name="password-less-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Entrada sem senha pelo telefone com o aplicativo Microsoft Authenticator (visualização pública)

O aplicativo Microsoft Authenticator pode ser usado para entrar em qualquer conta do Azure AD sem usar uma senha. Similar à tecnologia do [Windows Hello para Empresas](/windows/security/identity-protection/hello-for-business/hello-identity-verification), o Microsoft Authenticator usa a autenticação baseada em chave para habilitar uma credencial de usuário que esteja associada a um dispositivo e use biometria ou PIN.

![Exemplo de um navegador entrar pedindo para aprovar a entrada do usuário](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

Em vez de ver um prompt de senha após inserir um nome de usuário, uma pessoa que tenha habilitado entrada pelo telefone no aplicativo Microsoft Authenticator verá uma mensagem dizendo para tocar em um número em seu aplicativo. No aplicativo, o usuário deverá indicar o mesmo número, escolher Aprovar, informar o PIN ou biometria e, em seguida, a autenticação será concluída.

## <a name="enable-my-users"></a>Habilitar meus usuários

Para versão prévia pública, um administrador deve primeiro adicionar uma política por meio do powershell para permitir o uso da credencial no locatário. Examine a seção "Problemas conhecidos" antes de realizar esta etapa.

### <a name="tenant-prerequisites"></a>Pré-requisitos do locatário

* Azure Active Directory
* Usuários finais habilitados para Autenticação Multifator do Azure
* Os usuários podem registrar seus dispositivos

### <a name="steps-to-enable"></a>Etapas para habilitar

1. Verifique se você tem a versão mais recente da liberação da Versão Prévia Pública do Módulo do PowerShell do Azure Active Directory V2. Você pode querer desinstalar e reinstalar para confirmar isso, executando os seguintes comandos:

    ```powershell
    Uninstall-Module -Name AzureADPreview
    Install-Module -Name AzureADPreview
    ```

2. Autentique-se para o locatário do Azure AD para usar o módulo do PowerShell do Azure AD V2. A conta usada deve ser um Administrador de Segurança ou um Administrador Global.

    ```powershell
    Connect-AzureAD
    ```

3. Crie a política de Entrada do Autenticador:

    ```powershell
    New-AzureADPolicy -Type AuthenticatorAppSignInPolicy -Definition '{"AuthenticatorAppSignInPolicy":{"Enabled":true}}' -isOrganizationDefault $true -DisplayName AuthenticatorAppSignIn
    ```

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>Como os meus usuários finais habilitam entrada pelo telefone?

Para versão prévia pública, não há nenhuma maneira de impor que os usuários criem ou usem essa nova credencial. Um usuário final apenas encontrará entrada sem senha depois que um administrador tiver habilitado seu locatário e o usuário tiver atualizado o aplicativo Microsoft Authenticator para habilitar entrada pelo telefone.

> [!NOTE]
> Essa funcionalidade está no aplicativo desde março de 2017, portanto, existe a possibilidade de que, quando a política estiver habilitada para um locatário, os usuários possam encontrar esse fluxo imediatamente. Esteja atento e prepare seus usuários para essa alteração.
>

1. Registrar-se na Autenticação Multifator do Azure
1. Versão mais recente do Microsoft Authenticator instalada em dispositivos que executam o iOS 8.0 ou superior ou o Android 6.0 ou superior.
1. Conta corporativa ou de estudante com notificações por push adicionadas ao aplicativo. A documentação do usuário final pode ser encontrada em [https://aka.ms/authappstart](https://aka.ms/authappstart).

Depois que o usuário tem a conta MFA com notificações por push no aplicativo Microsoft Authenticator, ele pode seguir as etapas no artigo [Entrar com seu telefone, não sua senha](../user-help/microsoft-authenticator-app-phone-signin-faq.md) para concluir o registro de entrada com telefone.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="ad-fs-integration"></a>Integração do AD FS

Quando um usuário tiver habilitado a credencial sem senha do Microsoft Authenticator, a autenticação para esse usuário sempre enviará como padrão uma notificação de aprovação. Essa lógica impede que os usuários em um locatário híbrido sejam direcionados para o ADFS para verificação de entrada sem que o usuário realize uma etapa adicional de clicar em "Usar sua senha em vez disso". Esse processo também ignorará quaisquer políticas de Acesso Condicional locais e fluxos de autenticação de Passagem. A exceção a esse processo é se um login_hint for especificado, um usuário será autoforwarded para o AD FS e ignorar a opção para usar a credencial sem senha.

### <a name="azure-mfa-server"></a>Servidor MFA do Azure

Usuários finais habilitados para MFA por meio do servidor de MFA do Azure local da organização ainda podem criar e usar uma credencial de logon único de telefone sem senha. Se o usuário tentar atualizar várias instalações (+5) do Microsoft Authenticator com a credencial, essa alteração poderá resultar em um erro.  

### <a name="device-registration"></a>Registro de dispositivo

Um dos pré-requisitos para criar essa nova credencial forte é que o dispositivo no qual ela reside esteja registrado no locatário do Azure AD para um usuário individual. Devido às restrições de registro de dispositivo, um dispositivo só pode ser registrado em um único locatário. Esse limite significa que apenas uma conta corporativa ou de estudante no aplicativo Microsoft Authenticator pode ser habilitada para entrada por telefone.

## <a name="next-steps"></a>Próximas etapas

[Saiba mais sobre registro de dispositivo](../devices/overview.md#getting-devices-in-azure-ad)

[Saiba mais sobre a Autenticação Multifator do Azure](../authentication/howto-mfa-getstarted.md)
