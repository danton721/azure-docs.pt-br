---
title: Visão geral das contas de usuários no Azure Active Directory B2C | Microsoft Docs
description: Saiba mais sobre contas de usuários no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 42a38b47f9e49652b5fe80aa5bbd0047dc7d1e74
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511956"
---
# <a name="overview-of-user-accounts-in-azure-active-directory-b2c"></a>Visão geral das contas de usuários no Azure Active Directory B2C

No Azure AD (Azure Active Directory) B2C, é possível usar diferentes tipos de contas. O Azure Active Directory, o Azure Active Directory B2B e o Azure Active Directory B2C compartilham os tipos de contas de usuários que podem ser usados.

Os seguintes tipos de contas estão disponíveis:

- **Conta corporativa** - Uma conta corporativa pode acessar recursos em um locatário e, com uma função de administrador, pode gerenciar locatários.
- **Conta Convidado** - Uma conta convidado pode ser apenas uma conta Microsoft ou um usuário do Azure Active Directory que pode ser usado para acessar aplicativos ou gerenciar locatários. 
- **Conta Consumidor** - Uma conta consumidor é criada por meio de um fluxo de usuário de inscrição em um aplicativo do Azure AD B2C, ou usando a API do Graph do Azure AD, e é usada por usuários dos aplicativos registrados no Azure AD B2C. 

## <a name="work-account"></a>Conta corporativa

Uma conta corporativa é criada da mesma maneira para todos os locatários com base no Azure AD. Para criar uma conta corporativa, é possível usar as informações em [Início rápido: Adicionar novos usuários ao Azure Active Directory](../active-directory/fundamentals/add-users-azure-active-directory.md). Uma conta corporativa é criada usando a opção **Novo usuário** no portal do Azure.

Ao adicionar uma nova conta corporativa, será necessário considerar as seguintes configurações:

- **Nome** e **Nome de usuário** - A propriedade **Nome** contém o nome e sobrenome do usuário. O **Nome de usuário** é o identificador que o usuário insere para entrar. O nome de usuário inclui o domínio completo. A parte do nome de domínio do nome de usuário deve ser o nome de domínio padrão inicial *seu-domínio.onmicrosoft.com* ou um nome de domínio personalizado [não federado verificado, ](../active-directory/fundamentals/add-custom-domain.md) como *contoso.com*.
- **Perfil** - A conta é configurada com um perfil de dados do usuário. É possível inserir um nome, sobrenome, cargo e nome do departamento. É possível editar o perfil depois que a conta é criada.
- **Grupos** - Use um grupo para executar tarefas de gerenciamento como atribuir licenças ou permissões a vários usuários ou dispositivos de uma só vez. Você pode colocar a nova conta em um [grupo](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) existente no locatário. 
- **Função do diretório** - É necessário especificar o nível de acesso que a conta de usuário tem aos recursos no locatário. Os níveis de permissão a seguir estão disponíveis:

    - **Usuário** - Os usuários podem acessar recursos atribuídos, mas não podem gerenciar a maioria dos recursos do locatário.
    - **Administrador global** - Os administradores globais têm controle total sobre todos os recursos do locatário.
    - **Administrador limitado** - Selecione a função ou funções administrativas para o usuário. Para obter mais informações sobre as funções que podem ser selecionadas, consulte [Atribuindo funções de administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md). 

### <a name="create-a-work-account"></a>Crie uma conta corporativa

É possível usar as seguintes informações para criar uma nova conta corporativa:

- [Portal do Azure](../active-directory/fundamentals/add-users-azure-active-directory.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0)

### <a name="update-a-user-profile"></a>Atualizar um perfil do usuário

É possível usar as seguintes informações para atualizar o perfil de um usuário:

- [Portal do Azure](../active-directory/fundamentals/active-directory-users-profile-azure-portal.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0)

### <a name="reset-a-password-for-a-user"></a>Redefinir uma senha para um usuário

É possível usar as seguintes informações para redefinir a senha de um usuário: 

- [Portal do Azure](../active-directory/fundamentals/active-directory-users-reset-password-azure-portal.md)
- [Microsoft Graph](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0)

## <a name="guest-user"></a>Usuário Convidado

É possível convidar usuários externos para o locatário como usuário convidado. Um cenário típico para convidar um usuário convidado para o locatário do Azure AD B2C é compartilhar responsabilidades de administração. Para um exemplo de uso de uma conta Convidado, consulte [Propriedades de um usuário de colaboração do Azure Active Directory B2C](../active-directory/b2b/user-properties.md).

Ao convidar um usuário convidado para o locatário, você fornece o endereço de email do destinatário e uma mensagem descrevendo o convite. O link de convite leva o usuário à página de consentimento, onde o botão **Iniciar** é selecionado e a revisão de permissões é aceita. Se uma caixa de entrada não estiver anexada ao endereço de email, o usuário poderá navegar até a página de consentimento, acessando uma página da Microsoft usando as credenciais de convidado. O usuário é forçado a resgatar o convite da mesma maneira que clicar no link no email. Por exemplo: `https://myapps.microsoft.com/B2CTENANTNAME`.

Também é possível usar a [API do Microsoft Graph](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-beta) para convidar um usuário convidado.

## <a name="consumer-user"></a>Usuário Consumidor

O usuário consumidor pode entrar em aplicativos protegidos pelo Azure AD B2C, mas não pode acessar recursos do Azure, como o portal do Azure.  O usuário consumidor pode usar uma conta local ou contas federadas como Facebook ou Twitter. Uma conta Consumidor é criada, usando um [fluxo de usuário de inscrição ou de entrada](../active-directory-b2c/active-directory-b2c-reference-policies.md).

É possível especificar os dados que são coletados quando uma conta de usuário Consumidor é criada, usando atributos de usuário personalizados. Para obter mais informações, consulte [Definir atributos personalizados no Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-reference-custom-attr.md).

Você pode usar as informações da seção **Criar contas de usuários do consumidor** de [Usar a API do Graph do Azure AD](active-directory-b2c-devquickstarts-graph-dotnet.md) para criar uma conta Consumidor do Azure AD B2C. Também é possível usar as informações na seção **Atualizar contas de usuários Consumidor** no mesmo artigo para gerenciar as propriedades da conta.

### <a name="migrate-consumer-user-accounts"></a>Migrar contas de usuários Consumidor

Talvez seja necessário migrar as contas de usuários Consumidor existentes de qualquer provedor de identidade para o Azure AD B2C. Para obter mais informações, consulte [Migração de usuário](active-directory-b2c-user-migration.md) ou [Migrar usuários com identidades sociais](active-directory-b2c-social-migration.md).
