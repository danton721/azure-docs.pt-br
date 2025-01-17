---
title: Início rápido de Adicionar política de nomenclatura para grupos do Office 365 – Azure Active Directory | Microsoft Docs
description: Explica como adicionar novos usuários no Azure Active Directory ou excluir usuários existentes dele
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 04/24/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0e600204479bc54a590df6bf1bbcd634eaac7fc
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65605629"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Início Rápido: política de nomenclatura para grupos no Azure Active Directory

Neste início rápido, você configurará a política de nomenclatura em seu locatário do Azure AD (Azure Active Directory) para grupos do Office 365 criados pelo usuário, para ajudar você a classificar e pesquisar os grupos do seu locatário. Por exemplo, você poderia usar a política de nomenclatura para:

* Comunicar a função de um grupo, associação, região geográfica ou quem criou o grupo.
* Ajude a categorizar grupos no catálogo de endereços.
* Bloqueie o uso de palavras específicas em nomes de grupo e alias.

Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-portal-preview"></a>Configurar a política de nomenclatura de grupos para um locatário no portal do Azure (versão prévia)

1. Entre no [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com uma conta de administrador.
1. Para abrir a página da política de nomenclatura, escolha **Grupos** e **Política de nomenclatura**.

    ![Abrir a página da política de nomenclatura no centro de administração](./media/groups-naming-policy/policy-preview.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Exibir ou editar a política de nomenclatura de prefixo/sufixo

1. Na página **Política de nomenclatura**, escolha **Política de nomenclatura de grupos**.
1. Para exibir ou editar individualmente as políticas de nomenclatura de prefixo ou sufixo atuais, escolha os atributos ou cadeias de caracteres que você deseja aplicar como parte da política de nomenclatura.
1. Para remover um prefixo ou sufixo da lista, escolha o prefixo ou sufixo e, em seguida, escolha **Excluir**. É possível excluir vários itens ao mesmo tempo.
1. Escolha **Salvar** para que as alterações da política entrem em vigor.

### <a name="view-or-edit-the-custom-blocked-words"></a>Exibir ou editar palavras bloqueadas personalizadas

1. Na página **Política de nomenclatura**, escolha **Palavras bloqueadas**.

    ![Editar e carregar uma lista de palavras bloqueadas da política de nomenclatura](./media/groups-naming-policy/blockedwords-preview.png)

1. Para exibir ou editar a lista atual de palavras bloqueadas personalizadas, escolha **Baixar**.
1. Carregue a nova lista de palavras bloqueadas personalizadas escolhendo o ícone de arquivo.
1. Escolha **Salvar** para que as alterações da política entrem em vigor.

É isso. Você definiu sua política de nomenclatura e adicionou suas palavras bloqueadas personalizadas.

## <a name="clean-up-resources"></a>Limpar recursos

### <a name="remove-the-naming-policy-using-azure-portal-preview"></a>Remover a política de nomenclatura usando o portal do Azure (versão prévia)

1. Na página **Política de nomenclatura**, escolha **Excluir Política**.
1. Depois de confirmar a exclusão, a política de nomenclatura será removida, incluindo toda a política de nomenclatura de prefixo-sufixo e palavras bloqueadas personalizadas.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu a definir a política de nomenclatura para uma organização do Azure AD no portal do Azure.

Avance para o próximo artigo para ver mais detalhes, incluindo os cmdlets do PowerShell da política de nomenclatura, restrições técnicas, como adicionar uma lista de palavras bloqueadas personalizadas e as experiências do usuário final nos aplicativos do Office 365.
> [!div class="nextstepaction"]
> [PowerShell da política de nomenclatura](groups-naming-policy.md)