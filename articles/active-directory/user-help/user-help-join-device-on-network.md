---
title: Junte seu dispositivo de trabalho à rede da sua organização - Active Directory do Azure | Microsoft Docs
description: Saiba como associar seu dispositivo de trabalho à rede da sua organização.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: lizross
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 62faa47c116cceb941f575a9f76052e8bc57f036
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243266"
---
# <a name="join-your-work-device-to-your-organizations-network"></a>Junte seu dispositivo de trabalho à rede da sua organização
Junte-se ao seu dispositivo Windows 10 de propriedade de trabalho à rede da sua organização para poder acessar recursos possivelmente restritos.

## <a name="what-happens-when-you-join-your-device"></a>O que acontece quando você entra no seu dispositivo
Enquanto você está ingressando no seu dispositivo Windows 10 na rede da sua organização, as seguintes ações ocorrerão:

- Windows registra seu dispositivo à rede da sua organização, permitindo que você acessar seus recursos usando sua conta pessoal. Depois que o dispositivo é registrado, o Windows associa o dispositivo à rede, para que você possa usar o nome de usuário e a senha da sua organização para entrar e acessar recursos restritos.

- Opcionalmente, com base nas escolhas da sua organização, pode ser solicitado que você configure a verificação em duas etapas por meio de [Autenticação de vários fatores](multi-factor-authentication-end-user-first-time.md) ou [informações de segurança](user-help-security-info-overview.md).

- Opcionalmente, com base nas opções da sua organização, você pode estar automaticamente inscrito no gerenciamento de dispositivos móveis, como o Microsoft Intune. Para obter mais informações sobre como se inscrever no Microsoft Intune, consulte [Inscrever seu dispositivo no Intune](https://docs.microsoft.com/intune-user-help/enroll-your-device-in-intune-all).

- Você passará pelo processo de login, usando o login automático com sua conta organizacional.

## <a name="to-join-a-brand-new-windows-10-device"></a>Para ingressar em um dispositivo totalmente novo do Windows 10
Se seu dispositivo está totalmente novo e ainda não foi configurado ainda, percorrer o processo do Windows de OOBE (configuração) para ingressar seu dispositivo à rede.

1. Inicie o novo dispositivo e iniciar o processo OOBE.

2. Sobre o **entrar com a Microsoft** de tela, digite o seu trabalho ou endereço de e-mail da escola.

    ![Tela com o endereço de e-mail de entrada](./media/user-help-join-device-on-network/join-device-oobe-signin.png)

3. Sobre o **inserir sua senha** tela, digite sua senha.

    ![Insira sua tela de senha](./media/user-help-join-device-on-network/join-device-oobe-password.png)

4. Em seu dispositivo móvel, aprove seu dispositivo para que possa acessar sua conta. 

    ![Tela de notificação móvel](./media/user-help-join-device-on-network/join-device-oobe-mobile.png)

5. Conclua o processo OOBE, inclusive definindo suas configurações de privacidade e configurando o Windows Hello (se necessário).

    Seu dispositivo agora está conectado à rede da sua organização.

## <a name="to-make-sure-youre-joined"></a>Para garantir que você está unida
Você pode ter certeza de que você está associado, observando as configurações.

1. Abra **as configurações**e, em seguida, selecione **contas**.

    ![Contas na tela Configurações](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Selecione **acessar trabalho ou escola**e certifique-se de ver o texto que diz algo como **conectado à  *\<your_organization >* do Azure AD**.

    ![Acessar tela de trabalho ou escola com conta contoso conectada](./media/user-help-join-device-on-network/join-device-oobe-verify.png)


## <a name="to-join-an-already-configured-windows-10-device"></a>Para ingressar um dispositivo Windows 10 já configurado
Se você já usa seu dispositivo há algum tempo e já está configurado, siga estas etapas para conectar seu dispositivo à rede.

1. Abra **as configurações**e, em seguida, selecione **contas**.

2. Selecione **acessar trabalho ou escola**e, em seguida, selecione **Connect**.

    ![Acessar trabalho ou escola e conectar-se links](./media/user-help-join-device-on-network/join-device-access-work-school-connect.png)

3. Na tela **Configurar uma conta de trabalho ou escola**, selecione **Junte este dispositivo ao Active Directory do Azure**.

    ![Configurar um trabalho ou escola de tela da conta](./media/user-help-join-device-on-network/join-device-setup-join-aad.png)

4. Sobre o **vamos conectá-lo** tela, digite seu endereço de email (por exemplo, alain@contoso.com) e, em seguida, selecione **próximo**.

    ![Vamos conectá-lo na tela](./media/user-help-join-device-on-network/join-device-setup-get-signed-in.png)

5. Sobre o **digite a senha** tela, digite sua senha e, em seguida, selecione **entrar**.

    ![Digitar senha](./media/user-help-join-device-on-network/join-device-setup-password.png)

6. Em seu dispositivo móvel, aprove seu dispositivo para que possa acessar sua conta. 

    ![Tela de notificação móvel](./media/user-help-join-device-on-network/join-device-setup-mobile.png)

7. No, **certifique-se de que esta é a tela de sua organização**, revise as informações para certificar-se de que está correta e, em seguida, selecione **Join**.

    ![Certifique-se de que esta é a tela de verificação da sua organização](./media/user-help-join-device-on-network/join-device-setup-confirm.png)

8. Na tela **Está tudo pronto**, clique em **Concluído**.

    ![Você está a tela de conjunto de todos os](./media/user-help-join-device-on-network/join-device-setup-finish.png)

## <a name="to-make-sure-youre-joined"></a>Para garantir que você está unida
Você pode ter certeza de que você está associado, observando as configurações.

1. Abra **as configurações**e, em seguida, selecione **contas**.

    ![Contas na tela Configurações](./media/user-help-join-device-on-network/join-device-settings-accounts.png)

2. Selecione **acessar trabalho ou escola**e certifique-se de ver o texto que diz algo como **conectado à  *\<your_organization >* do Azure AD**.

    ![Acessar tela de trabalho ou escola com conta contoso conectada](./media/user-help-join-device-on-network/join-device-setup-verify.png)

## <a name="next-steps"></a>Próximas etapas
Depois de associar seu dispositivo à rede da sua organização, você poderá acessar todos os seus recursos usando o seu trabalho ou de estudante de informações da conta.

- Se sua organização quer que você registrar seu dispositivo pessoal, como seu telefone, consulte [registrar seu dispositivo pessoal na rede da sua organização](user-help-register-device-on-network.md).

- Se sua organização é gerenciada usando o Microsoft Intune e você tiver dúvidas sobre o registro, entrar, ou qualquer outro relacionados ao Intune problema, consulte o [conteúdo de Ajuda do usuário Intune](https://docs.microsoft.com/intune-user-help/use-managed-devices-to-get-work-done).