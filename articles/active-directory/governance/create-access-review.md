---
title: Criar uma revisão de acesso de aplicativos - Azure Active Directory ou grupos | Microsoft Docs
description: Saiba como criar uma revisão de acesso de membros do grupo ou o acesso de aplicativo em revisões de acesso do Active Directory do Azure.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ef72f1649c3f3e0af7fba53b2e8dbcee49d4b59
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734561"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Criar uma revisão de acesso de grupos ou revisões de acesso de aplicativos no Azure AD

Acesso a grupos e aplicativos para funcionários e visitantes muda ao longo do tempo. Para reduzir os riscos associados às atribuições de acesso obsoletas, os administradores podem usar o Azure Active Directory (Azure AD) para criar revisões de acesso para membros do grupo ou usuários atribuídos a um aplicativo. Se você precisar revisar rotineiramente o acesso, você também pode criar as revisões de acesso recorrentes. Para obter mais informações sobre esses cenários, consulte [Gerenciar acesso de usuário](manage-user-access-with-access-reviews.md) e [Gerenciar acesso de convidado](manage-guest-access-with-access-reviews.md).

Este artigo descreve como criar as revisões de acesso de um ou mais para membros do grupo ou o acesso ao aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

- Azure AD Premium P2
- Administrador global ou administrador de usuário

Para obter mais informações, consulte [quais usuários devem ter licenças?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="create-one-or-more-access-reviews"></a>Criar um ou mais revisões de acesso

1. Entrar no portal do Azure e abra o [página de controle de identidade](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. No menu à esquerda, clique em **revisões de acesso**.

1. Clique em **Nova análise de acesso** para criar uma nova revisão de acesso.

    ![Revisão do acesso - Controles](./media/create-access-review/access-reviews.png)

1. Nomeie a revisão de acesso. Opcionalmente, forneça uma descrição à revisão. O nome e a descrição são mostrados aos revisores.

    ![Criar uma revisão de acesso - nome da revisão e descrição](./media/create-access-review/name-description.png)

1. Defina a **Data de início**. Por padrão, uma revisão de acesso ocorre uma vez, inicia na mesma hora em que é criada e termina em um mês. Você pode alterar as datas de início e de término para iniciar uma análise de acesso em uma data futura e que dure quantos dias você desejar.

    ![Criar uma revisão de acesso - Data inicial e final](./media/create-access-review/start-end-dates.png)

1. Para fazer a revisão de acesso recorrente, altere o **frequência** definindo a partir **uma vez** para **semanal**, **mensal**,  **Trimestral** ou **anualmente**. Use o **duração** controle deslizante ou caixa de texto para definir quantos dias de cada análise da série de recorrentes será aberto para a entrada de revisores. Por exemplo, a duração máxima que você pode definir para uma revisão mensal é de 27 dias, para evitar revisões sobrepostas.

1. Use a configuração **Final** para especificar como terminar a série de revisão de acesso recorrente. A série pode terminar de três maneiras: ela é executada continuamente para iniciar revisões indefinidamente, até uma data específica ou após a conclusão de um número definido de ocorrências. Você, administrador de outro usuário ou outro administrador Global pode parar a série após a criação, alterando a data no **configurações**, de modo que ele termina nessa data.

1. No **usuários** seção, especifique os usuários que se aplica a revisão de acesso ao. As revisões de acesso podem ser de membros de um grupo ou usuários que foram atribuídos a um aplicativo. Você pode detalhar ainda mais o escopo da análise de acesso para examinar apenas os usuários convidados que são membros (ou atribuídos ao aplicativo), em vez de examinar todos os usuários que são membros ou que têm acesso ao aplicativo.

    ![Criar uma revisão de acesso - Usuários](./media/create-access-review/users.png)

1. No **grupo** , selecione um ou mais grupos que você gostaria de examinar a associação de.

    > [!NOTE]
    > Selecionar mais de um grupo, você criará várias revisões de acesso. Por exemplo, selecionar cinco grupos criará cinco revisões de acesso separados.
    
    ![Criar uma revisão de acesso - selecione o grupo](./media/create-access-review/select-group.png)

1. No **aplicativos** seção (se você selecionou **atribuídos a um aplicativo** na etapa 8), selecione os aplicativos que você gostaria de examinar o acesso ao.

    > [!NOTE]
    > Selecionar mais de um aplicativo, você criará várias revisões de acesso. Por exemplo, selecionar cinco aplicativos criará cinco revisões de acesso separados.
    
    ![Criar uma revisão de acesso – Selecionar aplicativo](./media/create-access-review/select-application.png)

1. Na seção **Revisores**, selecione uma ou mais pessoas para examinar todos os usuários no escopo. Ou você pode selecionar para que os membros examinem seus próprios acessos. Se o recurso for um grupo, você pode pedir que a revisão seja realizada pelos proprietários de grupo. Você também pode exigir que os revisores forneçam um motivo ao aprovar o acesso.

    ![Criar uma revisão de acesso - Revisores](./media/create-access-review/reviewers.png)

1. Na seção **Programas**, selecione o programa que você deseja usar. O **Programa Padrão** sempre está presente.

    ![Criar uma revisão de acesso - Programas](./media/create-access-review/programs.png)

    É possível simplificar a maneira de rastrear e coletar revisões de acesso para finalidades diferentes, organizando-as em programas. Cada análise de acesso pode ser vinculada a um programa. Em seguida, ao preparar relatórios para um auditor, você poderá concentrar-se nas revisões de acesso no escopo de uma iniciativa específica. Programas e os resultados da revisão de acesso são visíveis aos usuários o Administrador Global, administrador de usuário, administrador de segurança ou função de leitor de segurança.

    Para ver uma lista de programas, vá para o acesso de revisões de página e selecione **programas**. Se você estiver usando um Administrador Global ou a função de administrador do usuário, você pode criar programas adicionais. Por exemplo, é possível optar por ter um programa para cada iniciativa de conformidade ou meta de negócios. Se você não precisar mais de um programa e se ele não tiver nenhum controle vinculado a ele, você poderá excluí-lo.

### <a name="upon-completion-settings"></a>Após configurações de conclusão

1. Para especificar o que acontece após a conclusão de uma revisão, expanda a seção **Após configurações de conclusão**.

    ![Após configurações de conclusão](./media/create-access-review/upon-completion-settings.png)

1. Se você quiser remover automaticamente o acesso para usuários que foram negados, defina **Resultados de aplicação automática ao recurso** para **Habilitar**. Se você deseja aplicar manualmente os resultados quando a revisão for concluída, defina a opção para **Desabilitar**.

1. Use a lista **Se o revisor não responder** para especificar o que acontece para usuários que não foram examinados pelo revisor dentro do período de revisão. Essa configuração não afeta os usuários que foram revisados pelos revisores manualmente. Se a decisão do revisor final for negar o acesso do usuário será removido.

    - **Nenhuma alteração** - deixar o acesso do usuário inalterado
    - **Remover o acesso** - remover o acesso do usuário
    - **Aprovar o acesso** - aprovar o acesso do usuário
    - **Fazer recomendações** - levar a recomendação do sistema ao negar ou aprovar o acesso contínuo do usuário

### <a name="advanced-settings"></a>Configurações avançadas

1. Para especificar configurações adicionais, expanda a seção **Configurações avançadas**.

    ![Configurações avançadas](./media/create-access-review/advanced-settings.png)

1. Definir **Mostrar recomendações** à **Habilitar** para mostrar aos revisores as recomendações do sistema com base nas informações de acesso do usuário.

1. Definir **Requer motivo sob aprovação** para **Habilitar** para exigir que o revisor forneça um motivo para aprovação.

1. Definir **Notificações por email** para **Habilitar** para que o Azure Active Directory envie notificações por email para os revisores quando uma revisão de acesso começar e para os administradores quando uma revisão terminar.

1. Defina **Lembretes** para **Habilitar** para que o Azure Active Directory envie lembretes de análises de acesso em andamento para os revisores que não concluíram a sua análise.

    Por padrão, o Azure AD enviará automaticamente um lembrete após passar a metade do tempo para o prazo final para revisores que ainda não responderam.

## <a name="start-the-access-review"></a>Inicie a revisão de acesso

Depois de especificar as configurações para uma revisão de acesso, clique em **Iniciar**. A revisão de acesso será exibido na lista com um indicador de seu status.

![Lista de revisões de acesso](./media/create-access-review/access-reviews-list.png)

Por padrão, o Azure AD envia um email para os revisores logo após o início da análise. Se você optar pelo não envio do email pelo Azure AD, certifique-se de informar aos revisores que eles devem concluir uma análise de acesso pendente. Você pode mostrar as instruções de como a [revisar o acesso a grupos ou aplicativos](perform-access-review.md). Se sua revisão for para convidados revisem seus próprios acessos, exibir as instruções de como [revisar o acesso a grupos ou aplicativos por conta própria](review-your-access.md).

Se você tiver atribuído convidados como revisores e eles não tiverem aceitado o convite, eles não receberão um email das revisões de acesso porque eles devem primeiro aceitar o convite antes da revisão.

## <a name="create-reviews-via-apis"></a>Criar revisões via APIs

Você também pode criar as revisões de acesso usando as APIs. O que você faz para gerenciar as revisões de acesso dos grupos e usuários do aplicativo no portal do Azure também pode ser feito usando as APIs do Microsoft Graph. Para obter mais informações, consulte o [referência da API de revisões de acesso do Azure AD](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Para obter um exemplo de código, consulte [revisões de exemplo de como recuperar o acesso do Azure AD por meio do Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Próximas etapas

- [Examinar o acesso a grupos ou aplicativos](perform-access-review.md)
- [Examinar o acesso a grupos ou aplicativos por conta própria](review-your-access.md)
- [Concluir uma revisão de acesso de aplicativos ou grupos](complete-access-review.md)
