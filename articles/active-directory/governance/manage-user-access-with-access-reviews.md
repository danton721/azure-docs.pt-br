---
title: Gerenciar o acesso de usuário com revisões de acesso - Azure Active Directory | Microsoft Docs
description: Saiba como gerenciar o acesso de usuários como membros de um grupo ou a atribuição de um aplicativo com revisões de acesso do Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d873f1dace3e3be6a8767e77d57db5ac554afd7
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66473813"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Gestão do acesso do usuário com revisões de acesso do Azure AD

Com o Azure Active Directory (Azure AD), você pode garantir facilmente que os usuários tenham o acesso apropriado. Você pode pedir aos próprios usuários, ou a um tomador de decisão, que participem de uma revisão de acesso e reconfirmar (ou atestar) o acesso dos usuários. Os revisores podem fornecer as suas respectivas avaliações sobre a necessidade de acesso contínuo de cada usuário, com base nas sugestões do Azure AD. Quando uma revisão de acesso for finalizada, você poderá alterar e remover o acesso de usuários que não precisam mais dela.

> [!NOTE]
> Se você quiser revisar somente o acesso de usuários convidados, em vez de revisar todos os tipos de acesso de usuários, confira [Gerenciamento de acesso de usuários convidados com revisões de acesso](manage-guest-access-with-access-reviews.md). E se você desejar revisar a associação do usuário em funções administrativas, como administrador global, confira [Iniciar uma revisão de acesso no Azure AD Privileged Identity Management](../privileged-identity-management/pim-how-to-start-security-review.md).

## <a name="prerequisites"></a>Pré-requisitos

- Azure AD Premium P2

Para obter mais informações, consulte [quais usuários devem ter licenças?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="create-and-perform-an-access-review"></a>Criar e executar uma revisão de acesso

Você pode ter um ou mais usuários como revisores de uma revisão de acesso.  

1. Selecione um grupo no Azure AD que tenha um ou mais membros. Ou selecione um aplicativo conectado ao Azure AD que tenha um ou mais usuários atribuídos a ele. 

2. Decida se cada usuário revisará o próprio acesso ou se um ou mais usuários revisarão o acesso de todos.

3. Como um administrador global ou administrador de usuários, vá para o [página de controle de identidade](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Crie a revisão de acesso. Para obter mais informações, consulte [criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md).

5. Quando a revisão de acesso começar, solicite que os revisores enviem opiniões. Por padrão, cada um receba um email do AD do Azure com um link para o painel de acesso, onde eles [revisar o acesso a grupos ou aplicativos](perform-access-review.md).

6. Se os revisores não tiverem fornecido uma avaliação, você pode pedir ao Azure AD para enviar um lembrete a eles. Por padrão, o Azure AD enviará automaticamente um lembrete após passar a metade do tempo para o prazo final para revisores que ainda não responderam.

7. Depois dos revisores enviarem a avaliação, pare a revisão de acesso e aplique as alterações. Para obter mais informações, consulte [concluir uma revisão de acesso de aplicativos ou grupos](complete-access-review.md).


## <a name="next-steps"></a>Próximas etapas

[Criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md)




