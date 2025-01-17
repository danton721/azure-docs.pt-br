---
title: Configurar o ingresso no Azure Active Directory híbrido para domínios gerenciados | Microsoft Docs
description: Saiba como configurar o ingresso no Azure Active Directory híbrido para domínios gerenciados.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: e45b3b19821644142176c5c0cc7646c4643fe17c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235217"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-join-for-managed-domains"></a>Tutorial: Configurar o ingresso no Azure Active Directory híbrido para os domínios gerenciados

De maneira semelhante a um usuário, um dispositivo está se tornando outra identidade que você deseja proteger e também usa para proteger seus recursos a qualquer hora e local. É possível atingir essa meta, colocando as identidades dos dispositivos no Azure AD usando um dos métodos a seguir:

- Ingresso no Azure AD
- Ingresso no Azure AD Híbrido
- Registro do Azure AD

Ao colocar os dispositivos no Azure AD, você maximiza a produtividade dos usuários por meio de SSO (logon único) em toda a nuvem e recursos locais. Ao mesmo tempo, é possível proteger o acesso à sua nuvem e aos recursos locais com [acesso condicional](../active-directory-conditional-access-azure-portal.md).

Neste tutorial, você aprenderá como configurar o ingresso no Azure AD híbrido para dispositivos em domínios gerenciados.

> [!div class="checklist"]
> * Configurar ingresso no Azure AD híbrido
> * Habilitar dispositivos de nível inferior do Windows
> * Verificar dispositivos ingressados 
> * Solucionar problemas 


## <a name="prerequisites"></a>Pré-requisitos

Este tutorial assume que você está familiarizado com:
    
-  [Introdução ao gerenciamento de dispositivos no Azure Active Directory](../device-management-introduction.md)
    
-  [Como planejar a implementação de ingresso no Azure Active Directory híbrido](hybrid-azuread-join-plan.md)

-  [Como controlar o ingresso no Azure AD híbrido de seus dispositivos](hybrid-azuread-join-control.md)

> [!NOTE]
> O Azure AD não dá suporte a cartões inteligentes ou certificados em domínios gerenciados.


Para configurar o cenário neste artigo, é necessário ter:

- A [versão mais recente do Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) (1.1.819.0 ou superior) a ser instalada. 

Verifique se o Azure AD Connect sincronizou os objetos de computador dos dispositivos que você deseja ser o Azure AD híbrido para o Azure AD. Se os objetos de computador pertencem a unidades organizacionais (OU) específicas, essas OUs precisam ser configuradas para sincronização no Azure AD Connect também.

A partir da versão 1.1.819.0, o Azure AD Connect fornece um assistente para configurar o ingresso no Azure AD híbrido. O assistente permite simplificar significativamente o processo de configuração. O assistente relacionado configura os pontos de SCP (pontos de conexão de serviço) para o registro do dispositivo.

As etapas de configuração neste artigo são baseadas neste assistente. 

O ingresso no Azure AD híbrido requer que os dispositivos tenham acesso aos seguintes recursos da Microsoft dentro da rede da organização:  

- https://enterpriseregistration.windows.net
- https://login.microsoftonline.com
- https://device.login.microsoftonline.com
- https://autologon.microsoftazuread-sso.com (Se você estiver usando ou planejando usar SSO Contínuo)

Se a organização precisar de acesso à Internet por meio de um proxy de saída, iniciando com Windows 10 1709, será possível [definir as configurações de proxy no computador usando um GPO (Objeto de Política de Grupo)](https://blogs.technet.microsoft.com/netgeeks/2018/06/19/winhttp-proxy-settings-deployed-by-gpo/). Se o computador estiver executando alguma versão mais antiga que Windows 10 1709, será necessário implementar WPAD (Descoberta Automática de Proxy da Web) para permitir que computadores com Windows 10 façam o registro do dispositivo com Azure AD. 

Se a organização exigir acesso à Internet por meio de um proxy de saída autenticado, será necessário garantir que os computadores com Windows 10 possam ser autenticados com êxito no proxy de saída. Como computadores com Windows 10 executam o registro de dispositivos usando o contexto do computador, será necessário configurar a autenticação de proxy de saída usando o contexto do computador. Acompanhe com o provedor de proxy de saída nos requisitos de configuração. 



## <a name="configure-hybrid-azure-ad-join"></a>Configurar ingresso no Azure AD híbrido

Para configurar um ingresso no Azure AD híbrido usando o Azure AD Connect, será necessário ter:

- Credenciais de um administrador global para o locatário do Azure AD.  

- As credenciais de administrador corporativo para cada uma das florestas.


**Para configurar um ingresso no Azure AD híbrido usando o Azure AD Connect:**

1. Inicie o Azure AD Connect e clique em **Configurar**.

    ![Bem-Vindo](./media/hybrid-azuread-join-managed-domains/11.png)

2. Na página **Tarefas adicionais**, selecione **Configurar opções de dispositivo** e, em seguida, clique em **Avançar**. 

    ![Tarefas adicionais](./media/hybrid-azuread-join-managed-domains/12.png)

3. Na página de **Visão Geral**, clique em **Avançar**. 

    ![Visão geral](./media/hybrid-azuread-join-managed-domains/13.png)

4. Na página **Conectar o Azure AD**, insira as credenciais de um administrador global para o locatário do Azure AD.  

    ![Conecte-se ao AD do Azure](./media/hybrid-azuread-join-managed-domains/14.png)

5. Na página **Opções do dispositivo**, selecione **Configurar ingresso no Azure AD Híbrido** e, em seguida, clique em **Avançar**. 

    ![Opções do dispositivo](./media/hybrid-azuread-join-managed-domains/15.png)

6. Na página **SCP**, para cada floresta que você deseja que o Azure Active Directory Connect para configurar o SCP, execute as etapas a seguir e clique em **Avançar**: 

    ![SCP](./media/hybrid-azuread-join-managed-domains/16.png)

     a. Selecione a floresta.

    b. Selecione o serviço de autenticação.

    c. Clique em **Adicionar** para inserir as credenciais do administrador corporativo.


7. Na página **Sistemas operacionais do dispositivo**, selecione os sistemas operacionais usados pelos dispositivos no ambiente do Active Directory e clique em **Avançar**. 

    ![Sistema operacional do dispositivo](./media/hybrid-azuread-join-managed-domains/17.png)


8. Na página **Pronto para configurar**, clique em **Configurar**. 

    ![Pronto para configurar](./media/hybrid-azuread-join-managed-domains/19.png)

9. Na página **Configuração completa**, clique em **Sair**. 

    ![Configuração concluída](./media/hybrid-azuread-join-managed-domains/20.png)




## <a name="enable-windows-down-level-devices"></a>Habilitar dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos ingressados no domínio forem dispositivos de nível inferior do Windows, será necessário:

- Atualizar configurações do dispositivo
 
- Definir as configurações de Intranet Local para registro do dispositivo

- Configurar logon único (SSO) contínuo

- Controlar os dispositivos de nível inferior do Windows 


### <a name="update-device-settings"></a>Atualizar configurações do dispositivo 

Para registrar os dispositivos de nível inferior do Windows, será necessário verificar se as configurações do dispositivo para permitir que os usuários registrem dispositivos no Azure AD estão definidas. No portal do Azure, é possível localizar essas configurações em:

`Home > [Name of your tenant] > Devices - Device settings`  


    
A política a seguir deve ser definida como **Tudo**: **Os usuários podem registrar seus dispositivos com o Azure AD**

![Registrar dispositivos](media/hybrid-azuread-join-managed-domains/23.png)



### <a name="configure-the-local-intranet-settings-for-device-registration"></a>Definir as configurações de Intranet Local para registro do dispositivo

Para concluir com êxito o ingresso no Azure AD híbrido dos dispositivos de nível inferior do Windows e evitar prompts de certificado quando os dispositivos autenticarem no Azure AD, envie uma política por push aos dispositivos ingressados no domínio para adicionar as seguintes URLs à zona da Intranet Local no Internet Explorer:

- `https://device.login.microsoftonline.com`

- `https://autologon.microsoftazuread-sso.com`.

Além disso, é necessário habilitar **Permitir atualizações na barra de status via script** na zona da Intranet Local do usuário.


### <a name="configure-seamless-sso"></a>Configurar SSO contínuo

Para concluir a junção híbrida do Azure AD de seus dispositivos de nível inferior do Windows em um domínio gerenciado usando Autenticação de Passagem (PTA) ou Sincronização de Hash de Senha (PHS) como o método de autenticação de nuvem do Azure AD, você deve também [configurar o SSO contínuo](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso-quick-start#step-2-enable-the-feature). Se você estiver tendo problemas ao configurar o SSO contínuo, confira o artigo [Solucionar problemas do logon único contínuo do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso).


### <a name="control-windows-down-level-devices"></a>Controlar os dispositivos de nível inferior do Windows 

Para registrar os dispositivos de nível inferior do Windows, é necessário baixar e instalar um pacote do Windows Installer (.msi) do Centro de Download. Para obter mais informações, confira o artigo [Controlar o ingresso de seus dispositivos no Azure AD híbrido](hybrid-azuread-join-control.md#control-windows-down-level-devices). 


## <a name="verify-the-registration"></a>Verificar o registro

Para verificar o estado do registro do dispositivo no locatário do Azure, é possível usar o cmdlet **[Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice)** no **[módulo do PowerShell do Azure Active Directory](/powershell/azure/install-msonlinev1?view=azureadps-2.0)** .

Ao usar o cmdlet **Get-MSolDevice** para verificar os detalhes do serviço:

- Deverá existir um objeto com a **identificação do dispositivo**  correspondendo com a ID no cliente do Windows.
- O valor para **DeviceTrustType** deverá ser **Ingressado no Domínio**. Isso equivale ao estado **ingressado no Azure AD híbrido** na página Dispositivos no portal do Azure AD.
- O valor para **Habilitado** deverá ser **True** e **DeviceTrustLevel** deve ser **Gerenciado** para dispositivos usados em acesso condicional. 


**Para verificar os detalhes do serviço:**

1. Abra o **Windows PowerShell** como administrador.

2. Digite `Connect-MsolService` para conectar o locatário do Azure.  

3. Digite `get-msoldevice -deviceId <deviceId>`.

6. Verifique se **Habilitado** está definido como **Verdadeiro**.





## <a name="troubleshoot-your-implementation"></a>Solucionar problemas de implementação

Se estiver tendo problemas para concluir o ingresso do Azure AD híbrido para dispositivos Windows ingressados no domínio, consulte:

- [Solucionar problemas de ingresso no Azure AD Híbrido para dispositivos atuais do Windows](troubleshoot-hybrid-join-windows-current.md)
- [Solucionar problemas de ingresso no Azure AD Híbrido para dispositivos de nível inferior do Windows](troubleshoot-hybrid-join-windows-legacy.md)


## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configurar ingresso no Azure Active Directory híbrido para domínios federados](hybrid-azuread-join-federated-domains.md)
> [Configurar ingresso no Azure Active Directory híbrido manualmente](hybrid-azuread-join-manual.md)

