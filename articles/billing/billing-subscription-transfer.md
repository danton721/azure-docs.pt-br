---
title: Transferir a propriedade de assinatura do Azure para outra conta | Microsoft Docs
description: Descreve como transferir uma assinatura do Azure para outro usuário e algumas perguntas frequentes sobre o processo
keywords: transferir assinatura do Azure, assinatura de transferência do Azure, mover assinatura do Azure para outra conta, alterar proprietário da assinatura do Azure, transferir assinatura do Azure para outra conta
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91880e43382662b5d55f112455ee8f4c92ad01c5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66471589"
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Transferir a propriedade de uma assinatura do Azure para outra conta

Transfira sua assinatura para outro usuário no Centro de Contas para alterar o Administrador da Conta e passar a propriedade da cobrança da assinatura. Para alterar sua assinatura para uma oferta diferente, consulte [Trocar a assinatura do Azure por outra oferta](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
>
> Se você transferir uma assinatura para um novo locatário do Azure Active Directory, todas as atribuições de função no [ RBAC (controle de acesso baseado em função)](../role-based-access-control/overview.md) serão excluídas permanentemente do locatário de origem e não serão migradas para o locatário de destino. Você também precisará recriar manualmente as identidades gerenciadas para recursos do Azure. Para obter mais informações, consulte [identidades gerenciadas de perguntas frequentes e problemas conhecidos com](../active-directory/managed-identities-azure-resources/known-issues.md).

## <a name="transfer-ownership-of-an-azure-subscription"></a>Transferir a propriedade de uma assinatura do Azure

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]


1. Entre no [Centro de Contas do Azure](https://account.windowsazure.com/Subscriptions) como o Administrador da Conta. Para descobrir quem é o Administrador da Conta da assinatura, consulte as [Perguntas frequentes](#faq).

1. Selecione a assinatura para transferir.

1. Verifique se sua assinatura está qualificada para transferência de autoatendimento verificando a **Oferta** e a **ID da oferta** com a [lista de ofertas com suporte](#supported).

   ![Verifique a ID de oferta da assinatura no Centro de Contas](./media/billing-subscription-transfer/image0.png)
1. Clique em **Transferir assinatura**.

   ![Guia de assinaturas de conta do Azure](./media/billing-subscription-transfer/image1.png)
1. Especifique o destinatário.

   > [!IMPORTANT]
   >
   > Se você transferir uma assinatura para um novo locatário do Azure Active Directory, todas as atribuições de função no [ RBAC (controle de acesso baseado em função)](../role-based-access-control/overview.md) serão excluídas permanentemente do locatário de origem e não serão migradas para o locatário de destino. Você também precisará recriar manualmente as identidades gerenciadas para recursos do Azure. Para obter mais informações, consulte [identidades gerenciadas de perguntas frequentes e problemas conhecidos com](../active-directory/managed-identities-azure-resources/known-issues.md).

   ![Caixa de diálogo de assinatura de transferência](./media/billing-subscription-transfer/image2.PNG)

1. O destinatário recebe automaticamente um email com um link de aceitação.

   ![Email de transferência de assinatura para o destinatário](./media/billing-subscription-transfer/image3.png)
1. O destinatário clica no link e segue as instruções, incluindo inserir suas informações de pagamento.

   ![Primeira página da Web de transferência de assinatura](./media/billing-subscription-transfer/image4.png)

   ![Segunda página da Web de transferência de assinatura](./media/billing-subscription-transfer/image5.png)
1. Sucesso! Agora a assinatura será transferida.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-ea-customers"></a>Transferir a propriedade de assinatura para clientes do EA

O Administrador Corporativo pode transferir a propriedade das assinaturas em um registro. Para começar, confira [Transferir Propriedade da Conta](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) no portal do EA.

## <a name="next-steps-after-accepting-ownership"></a>Próximas etapas após aceitar a posse

1. Agora, você é o Administrador da Conta. Examine e atualize o Administrador de Serviços, os Coadministradores e outras funções do RBAC. Para obter mais informações, confira [Adicionar ou alterar os administradores de assinatura do Azure](billing-add-change-azure-subscription-administrator.md) e [Gerenciar o acesso usando o RBAC e o portal do Azure](../role-based-access-control/role-assignments-portal.md).
1. Atualize as credenciais associadas aos serviços dessa assinatura, incluindo:
   1. Certificados de gerenciamento que concedem ao usuário direitos de administrador aos recursos de assinatura. Para saber mais, confira [Criar e carregar um certificado de gerenciamento do Azure](../cloud-services/cloud-services-certs-create.md)
   1. Teclas de acesso para serviços como Armazenamento. Para saber mais, consulte [Sobre as contas de Armazenamento do Azure](../storage/common/storage-create-storage-account.md)
   1. Credenciais de Acesso Remoto para serviços como Máquinas Virtuais do Azure.
1. Se estiver trabalhando com um parceiro, considere a atualização da ID do parceiro nessa assinatura. Você pode atualizar a ID do parceiro no [Portal do Azure](https://portal.azure.com).

<a id="supported"></a>

## <a name="supported-offers"></a>Ofertas com suporte

A transferência de assinatura de autoatendimento está disponível para as ofertas ou tipos de assinatura listados na tabela a seguir. Atualmente, não é possível transferir assinaturas de Avaliação Gratuita ou [AIO (Azure via Open)](https://azure.microsoft.com/offers/ms-azr-0111p/). Para uma solução alternativa, consulte [Mover recursos para um novo grupo de recursos ou uma nova assinatura](../azure-resource-manager/resource-group-move-resources.md). Para transferir outras assinaturas, como [Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) ou planos de suporte, [contate o Suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Nome da oferta                                                                             | Número da oferta |
|----------------------------------------------------------------------------------------|--------------|
| [EA (Contrato Enterprise)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR-0017P        |
| [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR-0025P        |
| [Plataformas MSDN](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR-0062P        |
| [Pré-paga](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR-0003P        |
| [Desenvolvimento/Teste pré-pago](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR-0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR-0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR-0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR-0060P        |

\* [Via portal de EA](#EA)

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ)

### <a name="whoisaa"></a> Quem é o Administrador da Conta da assinatura?

O Administrador da Conta é a pessoa que se inscreveu ou comprou a assinatura do Azure. Ele está autorizado a acessar o [Centro de Contas](https://account.azure.com/Subscriptions) e a realizar várias tarefas de gerenciamento, como criar assinaturas, cancelar assinaturas, alterar a cobrança de uma assinatura ou alterar o administrador de serviços. Para obter mais informações sobre noções básicas sobre funções de administrador e permissões, consulte [permissões da função de administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

Se você não tiver certeza de quem é o administrador da conta de uma assinatura, use as etapas a seguir para descobrir.

1. Acesse a [página Assinaturas no Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a assinatura que você deseja verificar e olhe as **Configurações**.
1. Selecione **Propriedades**. O administrador da conta da assinatura será exibido na caixa **Administrador da Conta** .

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Tudo é transferido? Incluindo grupos de recursos, VMs, discos e outros serviços em execução?

Todos os recursos, como VMs, discos e sites, são transferidos para o novo proprietário. No entanto, todas as [funções de administrador](billing-add-change-azure-subscription-administrator.md) e políticas [RBAC (Controle de Acesso Baseado em Função)](../role-based-access-control/role-assignments-portal.md) configuradas não são transferidas entre diretórios diferentes. Além disso, os [registros de aplicativo](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) e outros serviços específicos de locatário também não são transferidos.

### <a id="no-button"></a> Por que não vejo o botão “Transferir Assinatura”?

Infelizmente, transferência de assinatura de autoatendimento não está disponível para sua oferta. Exibir a lista de ofertas com suporte nas [suporte para ofertas](#supported-offers) seção deste artigo. Além disso, não bloquearemos a transferência de assinatura para todos os países. No entanto, país transferência não há suporte para. Para transferir sua assinatura cruzada país, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 


### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Posso transferir a propriedade a uma conta em outro país?

Infelizmente, o Azure não permite transferir o país. Para transferir sua assinatura cruzada país, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Uma transferência de assinatura resulta em qualquer tempo de inatividade do serviço?

Não há nenhum impacto no serviço. A transferência da assinatura cancela a assinatura para o Administrador da Conta atual e cria uma assinatura na conta do destinatário. A nova assinatura é associada aos serviços do Azure subjacentes. A ID da assinatura permanece a mesma.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Como usar esse processo para alterar o diretório da assinatura?

Uma assinatura do Azure é criada no diretório ao qual o Administrador da Conta pertence. Para alterar o diretório, transfira a assinatura para uma conta de usuário no diretório de destino. Quando o usuário conclui as etapas para aceitar a transferência, a assinatura passa automaticamente para o diretório de destino.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Se eu assumir a propriedade da cobrança de uma assinatura de outra organização, eles continuarão a ter acesso aos meus recursos?

Se a assinatura for transferida para outro locatário, os usuários associados ao locatário anterior perderão o acesso à assinatura. Mesmo que um usuário não seja mais um Administrador ou Coadministrador de Serviços, ele ainda terá acesso à assinatura por meio de outros mecanismos de segurança, que incluem:

* Certificados de gerenciamento que concedem ao usuário direitos de administrador aos recursos de assinatura. Para saber mais, confira [Criar e carregar um certificado de gerenciamento do Azure](../cloud-services/cloud-services-certs-create.md).
* Teclas de acesso para serviços como Armazenamento. Para saber mais, confira [Sobre as contas de armazenamento do Azure](../storage/common/storage-create-storage-account.md).
* Credenciais de Acesso Remoto para serviços como Máquinas Virtuais do Azure.

Se o destinatário precisar restringir o acesso a seus recursos, ele deverá considerar a atualização dos segredos associados ao serviço. A maioria dos recursos pode ser atualizada usando as seguintes etapas:

  1. Vá para o [Portal do Azure](https://portal.azure.com).
  2. No menu Hub, selecione **Todos os recursos**.
  3. Selecione o recurso.
  4. Na folha de recursos, clique em **configurações**. Aqui você pode exibir e atualizar os segredos existentes.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Se eu transferir a assinatura no meio do ciclo de cobrança, o destinatário pagará por todo o ciclo de cobrança?

O remetente é responsável pelo pagamento por qualquer uso que foi relatado até o ponto no qual a transferência foi concluída. O destinatário é responsável por uso relatado a partir do momento da transferência em diante. Pode haver algum uso que ocorreu antes da transferência, mas foi relatado posteriormente. O uso está incluído na fatura do destinatário.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>O destinatário tem acesso ao histórico de cobrança e de uso?

  As únicas informações disponíveis para o destinatário serão o valor da última conta ou, se a assinatura tiver sido transferida antes que a primeira conta fosse gerada, o saldo atual. O restante do histórico de cobrança e uso não é transferido com a assinatura.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>A oferta pode ser alterada durante uma transferência?

A oferta deve permanecer a mesma. Para alterar sua oferta, confira [Switch your Azure subscription to another offer](billing-how-to-switch-azure-offer.md) (Alternar assinatura do Azure para outra oferta).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-countryregion"></a>Posso transferir uma assinatura para uma conta de usuário em outro país/região?

Não, não há suporte para transferir uma assinatura para uma conta de usuário em outro país/região. Conta de usuário do destinatário deve estar no mesmo país/região.

### <a name="can-the-recipient-use-a-different-payment-method"></a>O destinatário pode usar um método de pagamento diferente?

Sim. Mas o histórico de cobrança da assinatura é dividido em duas contas.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>O método de pagamento será afetado depois que eu transferir uma assinatura do Azure?

Para aceitar uma transferência de assinatura, um cartão de crédito ou uma forma de pagamento semelhante deve ser fornecida para pagar pela assinatura. Por exemplo, se Pedro transferir uma assinatura para Clara e ela aceitar a transferência, Clara deverá fornecer uma forma de pagamento para pagar pela assinatura. Depois que a transferência for concluída, Clara será cobrada pela assinatura, não Pedro.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Como faço para migrar dados e serviços da minha assinatura do Azure para uma nova assinatura?

Se não for possível transferir a propriedade da assinatura, migre os recursos manualmente. Consulte [Mover recursos para um novo grupo de recursos ou uma nova assinatura](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Se você tiver dúvidas ou precisar de Ajuda, [criar uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- Examine e atualize o Administrador de Serviços, os Coadministradores e outras funções do RBAC. Para obter mais informações, confira [Adicionar ou alterar os administradores de assinatura do Azure](billing-add-change-azure-subscription-administrator.md) e [Gerenciar o acesso usando o RBAC e o portal do Azure](../role-based-access-control/role-assignments-portal.md).
