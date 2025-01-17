---
title: Limitações da colaboração B2B – Azure Active Directory | Microsoft Docs
description: Limitações atuais à colaboração B2B do Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1466614dfeb8fa4d3d095bae070d01c6503c5bb
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357235"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Limitações da colaboração B2B do Azure AD
A colaboração B2B do Azure Active Directory (Azure AD) está sujeita, atualmente, às limitações descritas neste artigo.

## <a name="possible-double-multi-factor-authentication"></a>Possível autenticação multifator dupla
Com B2B do Azure AD é possível impor a autenticação multifator na organização do recurso (a organização emissora do convite). Os motivos para essa abordagem são detalhados em [Acesso condicional para usuários de colaboração B2B](conditional-access.md). Se um parceiro já tiver uma autenticação multifator configurada e aplicada, é possível que os usuários do parceiro tenham que executar a autenticação uma vez na organização inicial e novamente na sua.

## <a name="instant-on"></a>Instant-on
Nos fluxos de colaboração B2B, adicionamos usuários ao diretório e os atualizamos dinamicamente durante o resgate do convite, atribuição do aplicativo e assim por diante. As atualizações e as gravações em geral ocorrem em uma instância do diretório e devem ser replicadas em todas as instâncias. A replicação estará concluída quando todas as instâncias estiverem atualizadas. Às vezes, quando o objeto é gravado ou atualizado em uma instância e a chamada para recuperar esse objeto for para outra instância, poderão ocorrer latências de replicação. Se isso acontecer, atualize ou tente novamente. Se você estiver gravando um aplicativo utilizando nossa API, então, tentativas com algumas retiradas é uma boa prática defensiva para aliviar esse problema.

## <a name="azure-ad-directories"></a>Diretórios do AD do Azure
O Azure AD B2B está sujeito aos limites de diretório de serviço do Azure AD. Para obter detalhes sobre o número de diretórios que um usuário pode criar e o número de diretórios aos quais um usuário ou usuário convidado pode pertencer, consulte [Limites e restrições do serviço do Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="national-clouds"></a>Nuvens nacionais
[Nuvens nacionais](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) são instâncias isoladas fisicamente do Azure. Não há suporte para colaboração B2B entre limites de nuvem nacional. Por exemplo, se seu locatário do Azure está na nuvem pública, global, você não pode convidar um usuário cuja conta esteja em uma nuvem nacional. Para colaborar com o usuário, peça para outro endereço de email ou criar uma conta de usuário do membro para eles no seu diretório.

## <a name="next-steps"></a>Próximas etapas

Consulte os seguintes artigos na colaboração B2B do Azure AD:

- [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
- [Delegação de convites de colaboração B2B](delegate-invitations.md)

