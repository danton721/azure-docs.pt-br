---
title: Introdução ao Azure MFA na nuvem | Microsoft Docs
description: Esta é a página da Autenticação Multifator do Microsoft Azure que descreve como começar a usar o Azure MFA na nuvem.
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/24/2017
ms.author: joflore
ms.openlocfilehash: e6210cf7ece0aa0cdeec8f95b74910893c22b1bb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/19/2018
---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Introdução ao Servidor de Autenticação Multifator do Microsoft Azure na nuvem
Este artigo explica como começar a usar a autenticação multifator do Azure na nuvem.

> [!NOTE]
> A documentação a seguir fornece informações sobre como habilitar os usuários usando o **portal do Azure**. Se você estiver procurando informações sobre como configurar a Autenticação Multifator do Azure para os usuários O365, confira [Configurar a autenticação multifator para o Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA na Nuvem](./media/howto-mfa-getstarted/mfa_in_cloud.png)

## <a name="prerequisite"></a>Pré-requisito
[Inscreva-se para ter uma assinatura do Azure](https://azure.microsoft.com/pricing/free-trial/) - se você ainda não tem uma assinatura do Azure, precisará inscrever-se para ter uma. Se você estiver apenas começando a usar o Azure MFA, poderá usar uma assinatura de avaliação.

## <a name="enable-azure-multi-factor-authentication"></a>Habilitar a Autenticação Multifator do Microsoft Azure
Se os usuários tiverem licenças que incluam a Autenticação Multifator do Microsoft Azure, nada precisa ser feito para ativar o Azure MFA. Você pode começar solicitando uma verificação em duas etapas em base de usuário individual. As licenças que habilitam o Azure MFA são:
- Autenticação Multifator do Azure
- Azure Active Directory Premium
- Enterprise Mobility + Security

Se você não tiver uma dessas três licenças, ou se não possuir licenças suficientes para cobrir todos os seus usuários, também não há problema. Você só precisa cumprir uma etapa extra e [Criar um Provedor de Autenticação Multifator](concept-mfa-authprovider.md) em seu diretório.

## <a name="turn-on-two-step-verification-for-users"></a>Ativar a verificação em duas etapas para usuários

Use um dos procedimentos listados em [Como exigir uma verificação em duas etapas para um usuário ou grupo](../../multi-factor-authentication/multi-factor-authentication-get-started-user-states.md) para começar a usar o Azure MFA. Você pode escolher forçar a verificação em duas etapas para todas as entradas ou criar políticas de acesso condicional para exigir uma verificação em duas etapas somente quando for importante.

## <a name="next-steps"></a>Próximas etapas
Agora que você configurou a Autenticação Multifator do Azure na nuvem, poderá configurar e instalar sua implantação. Confira [Configurar a autenticação multifator do Azure](howto-mfa-mfasettings.md) para obter mais detalhes.
