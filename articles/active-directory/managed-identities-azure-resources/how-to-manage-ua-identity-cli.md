---
title: Como gerenciar uma identidade gerenciada atribuída ao usuário usando CLI do Azure
description: Instruções passo a passo sobre como criar, listar e excluir uma identidade gerenciada atribuída ao usuário usando a CLI do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 19963edc3742cc5f09ed02249d313728410711c8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973483"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Criar, listar ou excluir uma identidade gerenciada atribuída ao usuário usando a CLI do Azure

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

As identidades gerenciadas dos recursos do Azure fornecem aos serviços do Azure uma identidade gerenciada no Azure Active Directory. Você pode usar essa identidade para autenticar em qualquer serviço que seja compatível com a autenticação do Azure Active Directory sem precisar ter as credenciais no seu código. 

Neste artigo, você aprende como criar, listar e excluir uma identidade gerenciada atribuída ao usuário usando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Se você não estiver familiarizado com identidades gerenciadas para recursos do Azure, confira a [seção de visão geral](overview.md). **Revise a [diferença entre uma identidade gerenciada atribuída ao sistema e atribuída ao usuário](overview.md#how-does-it-work)**.
- Se você ainda não tiver uma conta do Azure, [inscreva-se em uma conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para realizar as operações de gerenciamento deste artigo, a conta precisará das seguintes atribuições de função:
    - [Colaborador de Identidade Gerenciada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (listar), atualizar e excluir uma identidade gerenciada atribuída ao usuário.
    - Função de [Operador de Identidade Gerenciada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para ler (listar) as propriedades de uma identidade atribuída ao usuário.
- Para executar os exemplos de script da CLI, você tem três opções:
    - Usar o [Azure Cloud Shell](../../cloud-shell/overview.md) no Portal do Azure (confira a próxima seção).
    - Usar o Azure Cloud Shell inserido por meio do botão "Experimentar", localizado no canto superior direito de cada bloco de código.
    - [Instale a versão mais recente da CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou mais recente), se você preferir usar um console da CLI local. Entre no Azure usando `az login`, usando uma conta associada à assinatura do Azure na qual você quer implantar a identidade gerenciada atribuída ao usuário.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Criar uma identidade gerenciada atribuída ao usuário 

Para criar uma identidade gerenciada atribuída ao usuário, use o comando [az identity create](/cli/azure/identity#az-identity-create). O parâmetro `-g` especifica o grupo de recursos no qual a identidade gerenciada atribuída ao usuário é criada e o parâmetro `-n` especifica o nome. Substitua os valores de parâmetro `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Listar identidades gerenciadas atribuídas ao usuário

Para listar as identidades gerenciadas atribuídas ao usuário, use o comando [az identity list](/cli/azure/identity#az-identity-list). Substitua o `<RESOURCE GROUP>` por seu próprio valor:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Na resposta json, as identidades gerenciadas atribuídas ao usuário têm valor `"Microsoft.ManagedIdentity/userAssignedIdentities"` retornado para a chave, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Excluir uma identidade gerenciada atribuída ao usuário

Para excluir uma identidade gerenciada atribuída ao usuário, use o comando [az identity delete](/cli/azure/identity#az-identity-delete).  O parâmetro -n especifica o nome e o parâmetro -g especifica o grupo de recursos em que a identidade gerenciada atribuída ao usuário foi criada. Substitua os valores de parâmetros `<USER ASSIGNED IDENTITY NAME>` e `<RESOURCE GROUP>` pelos seus próprios valores:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> A exclusão de uma identidade gerenciada atribuída ao usuário não removerá a referência de nenhum recurso ao qual foi atribuída. Remova-as da VM/VMSS usando o comando `az vm/vmss identity remove`

## <a name="next-steps"></a>Próximas etapas

Para obter uma lista completa de comandos de identidade da CLI do Azure, consulte [az identity](/cli/azure/identity).

Para obter informações sobre como atribuir uma identidade gerenciada atribuída ao usuário a uma VM do Azure, consulte, [Configurar identidades gerenciadas para recursos do Azure em uma VM do Azure usando CLI do Azure](qs-configure-cli-windows-vm.md#user-assigned-managed-identity)


 