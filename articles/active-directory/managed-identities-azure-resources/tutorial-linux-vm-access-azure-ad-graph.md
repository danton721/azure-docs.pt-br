---
title: Usar uma identidade gerenciada atribuída pelo sistema da VM do Linux para acessar a API do Microsoft Azure AD Graph
description: Um tutorial que orienta você durante o processo de usar uma identidade gerenciada atribuída pelo sistema da VM do Linux para acessar a API do Microsoft Azure AD Graph.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: daveba
ms.openlocfilehash: 548111a6c2b9e0cf8c5b20eee5cc8fa45fe02da8
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47453101"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Tutorial: usar uma identidade gerenciada atribuída pelo sistema da VM do Linux para acessar a API do Microsoft Azure AD Graph

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como usar uma identidade gerenciada atribuída pelo sistema de uma VM (máquina virtual) do Linux para acessar a API do Microsoft Azure AD Graph para recuperar as associações de grupo. As identidades gerenciadas para recursos do Azure são gerenciadas automaticamente pelo Azure e permitem a você autenticar os serviços que dão suporte à autenticação do Azure AD sem necessidade de inserir as credenciais em seu código.  

Neste tutorial, você consultará a associação da sua identidade da VM em grupos do Microsoft Azure AD. As informações de grupo geralmente são usadas em decisões de autorização. Nos bastidores, a identidade gerenciada da sua VM é representada por uma **Entidade de Serviço** no Microsoft Azure AD. 

> [!div class="checklist"]
> * Conecte-se ao AD do Azure
> * Adicionar sua identidade de VM a um grupo no Microsoft Azure AD 
> * Conceder à identidade da VM acesso ao Microsoft Azure AD Graph 
> * Obter um token de acesso usando a identidade da VM e usá-lo para chamar o Microsoft Azure AD Graph

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Entrar no portal do Azure](https://portal.azure.com)

- [Criar uma máquina virtual do Linux](/azure/virtual-machines/linux/quick-create-portal)

- [Habilitar identidade gerenciada atribuída pelo sistema em sua máquina virtual](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

- [Instalar a versão mais recente da CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

- Para conceder um acesso de identidade da VM ao Microsoft Azure AD Graph, sua conta precisa receber a atribuição da função **Administrador Global** no Microsoft Azure AD.

## <a name="connect-to-azure-ad"></a>Conecte-se ao AD do Azure

Você precisa se conectar ao Microsoft Azure AD para atribuir a VM a um grupo, bem como conceder permissão à VM para recuperar suas associações de grupo.

```cli
az login
```

## <a name="add-your-vms-identity-to-a-group-in-azure-ad"></a>Adicionar a identidade da sua VM a um grupo no Microsoft Azure AD

Quando você habilitou a identidade gerenciada atribuída pelo sistema na VM do Linux, foi criada uma entidade de serviço no Microsoft Azure AD.  É preciso adicionar a VM a um grupo. Consulte o seguinte artigo para obter instruções sobre como adicionar sua VM a um grupo no Microsoft Azure AD:

- [Adicionar membros do grupo](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add)

## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Conceder à VM o acesso à API do Microsoft Azure AD Graph

Usando a identidades gerenciadas para recursos do Azure, seu código pode obter tokens de acesso para autenticar para recursos que oferecem suporte à autenticação do Azure AD. A API do Microsoft Azure AD Graph é compatível com a autenticação do Microsoft Azure AD. Nesta etapa, você concederá acesso à entidade de serviço da identidade da sua VM ao Microsoft Azure AD Graph para que ele possa consultar as associações de grupo. As entidades de serviço recebem acesso à Microsoft ou Azure AD Graph por meio de **Permissões de Aplicativo**. O tipo de permissão de aplicativo que você precisa conceder depende da entidade que você deseja acessar no MS ou Azure AD Graph.

Neste tutorial, você concederá à identidade da VM a capacidade de consultar associações de grupo usando a permissão de aplicativo `Directory.Read.All`. Para conceder essa permissão, você precisará de uma conta de usuário que recebeu a atribuição da função de Administrador Global no Microsoft Azure AD. Normalmente, uma permissão de aplicativo é concedida visitando o registro do aplicativo no portal do Azure e adicionando a permissão nele. No entanto, identidades gerenciadas para recursos do Azure não registram objetos de aplicativo no Microsoft Azure AD, registrando apenas as entidades de serviço. Para registrar a permissão de aplicativo, você usará a ferramenta de linha de comando do Microsoft Azure AD PowerShell. 

Microsoft Azure AD Graph:
- AppId de entidade de serviço (usado ao conceder permissão ao aplicativo): 00000002-0000-0000-c000-000000000000
- ID do recurso (usada ao solicitar o token de acesso de identidades gerenciadas para recursos do Azure): https://graph.windows.net
- Referência de escopo de permissão: [referência de permissões do Microsoft Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-curl"></a>Conceder permissões de aplicativo usando o CURL

1. Recuperar um token para fazer solicitações CURL:

   ```cli
   az account get-access-token --resource "https://graph.windows.net/"
   ```

2. Você precisará recuperar e anotar o `objectId` da sua VM. Ele é usado nas próximas etapas para conceder permissões à VM para ler a associação de grupo dela. Substitua `<ACCESS TOKEN>` pelo token de acesso que você recuperou na etapa anterior.

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals?$filter=startswith%28displayName%2C%27myVM%27%29&api-version=1.6' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

3. Usando a appID do Microsoft Azure AD Graph, 00000002-0000-0000-c000-000000000000, recupere e anote os `objectId` para `odata.type: Microsoft.DirectoryServices.ServicePrincipal` e o `id` para a permissão da função de aplicativo `Directory.Read.All`.  Substitua `<ACCESS TOKEN>` pelo token de acesso que você recuperou anteriormente.

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals?api-version=1.6&%24filter=appId%20eq%20'00000002-0000-0000-c000-000000000000'" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Resposta:

   ```json
   "odata.metadata":"https://graph.windows.net/myorganization/$metadata#directoryObjects",
   "value":[
      {
         "odata.type":"Microsoft.DirectoryServices.ServicePrincipal",
         "objectType":"ServicePrincipal",
         "objectId":"81789304-ff96-402b-ae73-07ec0db26721",
         "deletionTimestamp":null,
         "accountEnabled":true,
         "addIns":[
         ],
         "alternativeNames":[
         ],
         "appDisplayName":"Windows Azure Active Directory",
         "appId":"00000002-0000-0000-c000-000000000000",
         "appOwnerTenantId":"f8cdef31-a31e-4b4a-93e4-5f571e91255a",
         "appRoleAssignmentRequired":false,
         "appRoles":[
            {
               "allowedMemberTypes":[
                  "Application"
               ],
               "description":"Allows the app to read data in your company or school directory, such as users, groups, and apps.",
               "displayName":"Read directory data",
               "id":"5778995a-e1bf-45b8-affa-663a9f3f4d04",
               "isEnabled":true,
               "value":"Directory.Read.All"
            },
            {
               //other appRoles values
            }
   ``` 

4. Agora, conceda acesso de leitura à entidade de serviço da VM aos objetos do diretório do Microsoft Azure AD usando a API do Microsoft Azure AD Graph.  O valor `id` é o valor para a permissão de função de aplicativo `Directory.Read.All` e o `resourceId` é o `objectId` para o `odata.type:Microsoft.DirectoryServices.ServicePrincipal` da entidade de serviço (os valores que você anotou na etapa anterior).

   ```bash
   curl "https://graph.windows.net/myorganization/servicePrincipals/<VM Object ID>/appRoleAssignments?api-version=1.6" -X POST -d '{"id":"5778995a-e1bf-45b8-affa-663a9f3f4d04","principalId":"<VM Object ID>","resourceId":"81789304-ff96-402b-ae73-07ec0db26721"}'-H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ``` 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-ad-graph"></a>Obter um token de acesso usando a identidade da VM e usá-lo para chamar o Microsoft Azure AD Graph 

Para concluir essas etapas, você precisará de cliente SSH. Se você estiver usando o Windows, poderá usar o cliente SSH no [Subsistema do Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda para configurar as chaves do cliente SSH, confira [Como usar chaves SSH com o Windows no Azure](../../virtual-machines/linux/ssh-from-windows.md), ou [Como criar e usar um par de chaves SSH pública e privada para VMs Linux no Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal, navegue até a VM do Linux e em **Visão geral**, clique em **Conectar**.  
2. **Conecte-se** à VM com um cliente SSH de sua escolha. 
3. Na janela do terminal, usando CURL, faça uma solicitação para as identidades gerenciadas locais para o ponto de extremidade de recursos do Azure para obter um token de acesso para o Microsoft Azure AD Graph.  
    
   ```bash
   curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -H Metadata:true
   ```    
   A resposta inclui o token de acesso que você precisa para acessar o Microsoft Azure AD Graph.

   Resposta:

   ```bash
   {
       "access_token":"eyJ0eXAiOiJKV...",
       "expires_in":"3599",
       "expires_on":"1519622535",
       "not_before":"1519618635",
       "resource":"https://graph.windows.net",
       "token_type":"Bearer"
   }
   ```

4. Usando a ID de objeto da entidade de serviço da sua VM (o valor recuperado nas etapas anteriores), você pode consultar a API do Microsoft Azure AD Graph para recuperar suas associações de grupo. Substitua `<OBJECT-ID>` pela ID de objeto da entidade de serviço da sua VM e `<ACCESS-TOKEN>` pelo token de acesso obtido anteriormente:

   ```bash
   curl 'https://graph.windows.net/myorganization/servicePrincipals/<OBJECT-ID>/getMemberGroups?api-version=1.6' -X POST -d "{\"securityEnabledOnly\": false}" -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS-TOKEN>"
   ```

   Resposta:

   ```bash   
   Content : {"odata.metadata":"https://graph.windows.net/myorganization/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a usar uma identidade gerenciada atribuída pelo sistema da VM do Linux para acessar o Microsoft Azure AD Graph.  Para saber mais sobre o Microsoft Azure AD Graph, consulte:

>[!div class="nextstepaction"]
>[Microsoft Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)