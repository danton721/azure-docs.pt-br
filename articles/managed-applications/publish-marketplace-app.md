---
title: Aplicativos gerenciados do Azure no Marketplace | Microsoft Docs
description: Descreve os aplicativos gerenciados do Azure que estão disponíveis por meio do Marketplace.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 07/10/2018
ms.author: tomfitz
ms.openlocfilehash: 61cac49c34eb193d641a94c9a7839282289dd9c7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64572590"
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Aplicativos gerenciados do Azure no Marketplace

Os fornecedores podem usar os aplicativos gerenciados do Azure para oferecer suas soluções para todos os clientes do Azure Marketplace. Os fornecedores podem incluir os provedores de serviços gerenciados (MSPs), os fornecedores de software independentes (ISVs) e os integradores de sistemas (Sis). Os aplicativos gerenciados reduzem a sobrecarga de manutenção e de serviço para os clientes. Os editores podem vender infraestrutura e software por meio do marketplace. Eles podem anexar serviços e suporte operacional para aplicativos gerenciados. Para saber mais, consulte [Visão geral do aplicativo gerenciado](overview.md).

Este artigo explica como você pode publicar um aplicativo no marketplace e torná-lo amplamente disponível aos clientes.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Pré-requisitos para publicar um aplicativo gerenciado

Para concluir este artigo, você já deve ter o arquivo .zip para a sua definição de aplicativo gerenciado. Para saber mais, veja [Criar aplicativo de catálogo de serviços](publish-service-catalog-app.md).

Além disso, existem vários pré-requisitos de negócios. Eles são:

* Sua empresa, ou subsidiária, deve estar localizada em um país/região no qual as vendas têm suporte no marketplace.
* O produto deve ser licenciado de forma que seja compatível com modelos de cobrança com suporte no marketplace.
* Disponibilize o suporte técnico para os clientes de modo comercialmente razoável. O suporte pode ser gratuito, pago ou oferecido por meio de suporte da comunidade.
* Licencie seu software e quaisquer dependências de software de terceiros.
* Forneça conteúdo que atenda aos critérios da oferta a ser listada no Marketplace e no Portal do Azure.
* Concorde com os termos das Políticas de Participação do Azure Marketplace e com o Contrato do Editor.
* Concorde em cumprir os Termos de uso, a Política de privacidade da Microsoft e o Contrato do programa de certificação do Microsoft Azure.

## <a name="become-a-publisher"></a>Torna-se um editor

Para tornar-se um editor no Microsoft Azure Marketplace, você deverá:

1. Criar uma ID da Microsoft - Crie sua conta Microsoft usando um endereço de email pertencente ao domínio da sua empresa, mas não a um único indivíduo. Esse endereço de email será usado tanto para a Central de Desenvolvedores da Microsoft como para o Portal do Cloud Partner. Para obter mais informações, consulte [Guia do Editor do Microsoft Azure Marketplace](https://aka.ms/sellerguide).
1. Enviar o [Formulário de Nominação do Microsoft Azure Marketplace](https://aka.ms/ampnomination) - Para **Solução que você pretende publicar?** , selecione **Aplicativo Gerenciado**. Quando o formulário for enviado, a equipe de Integração do Marketplace revisará o aplicativo e validará a solicitação. O processo de aprovação poderá demorar de um a três dias. Quando a nomeação for aprovada, você receberá um código promocional para renunciar à taxa de registro para a central de desenvolvedores. Se você **não** concluir o Formulário de Nominação do Marketplace, será solicitado a você pagar uma taxa de registro de $ 99.
1. Registre-se no [Centro de Desenvolvedores](https://dev.windows.com/registration?accountprogram=azure): a Microsoft verificará que sua organização é uma entidade legal válida com uma ID DO IMPOSTO válida para o país/a região em que está registrada. O processo de aprovação poderá demorar de 5 a 10 dias. Para evitar a taxa de registro, utilize o código promocional recebido no email do processo de nominação. Para obter mais informações, consulte [Guia do Editor do Microsoft Azure Marketplace](https://aka.ms/sellerguide).
1. Entre no [Portal do Cloud Partner](https://cloudpartner.azure.com) - No perfil do editor, associe sua conta da Central de Desenvolvedores ao Perfil do Editor do Marketplace. Para obter mais informações, consulte [Guia do Editor do Microsoft Azure Marketplace](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Criar uma nova oferta de aplicativo do Azure

Após criar sua conta de portal de parceiro, você estará pronto para criar sua oferta de aplicativo gerenciado.

### <a name="set-up-an-offer"></a>Configurar uma oferta

A oferta para um aplicativo gerenciado corresponde a uma classe de oferta de produtos de um publicador. Se você tiver um novo tipo de aplicativo que queira disponibilizar no marketplace, configure-o como uma oferta nova. Uma oferta é uma coleção de SKUs. Cada oferta aparece como sua própria entidade no marketplace.

1. Entre no [Portal do Cloud Partner](https://cloudpartner.azure.com/).

1. No painel de navegação à esquerda, selecione **+ Nova oferta** > **Aplicativos do Azure**.

1. No modo de exibição **Editor**, você verá os formulários necessários. Cada formulário será descrito posteriormente neste artigo.

## <a name="offer-settings-form"></a>Formulário de Configurações de Oferta

Os campos para o formulário **Configurações da Oferta** são:

* **ID da Oferta**: Esse identificador exclusivo identifica a oferta em um perfil de editor. Essa ID está visível em URLs de produto, modelos do Resource Manager e relatórios de cobrança. Ele só pode ser composto de caracteres alfanuméricos minúsculos ou traços (-). A ID não pode terminar com um traço. Ela tem um limite máximo de 50 caracteres. Após a ativação da oferta, esse campo é bloqueado.
* **ID do Editor**: Use essa lista suspensa para escolher o perfil de editor no qual deseja publicar essa oferta. Após a ativação da oferta, esse campo é bloqueado.
* **Nome**: O nome de exibição da oferta é exibido no Marketplace e no portal. Ele pode ter um máximo de 50 caracteres. Inclua um nome de marca reconhecível para o seu produto. Não inclua o nome de sua empresa aqui, a menos que seja a maneira como ela é comercializada. Se você estiver comercializando essa oferta em seu próprio site, certifique-se de que o nome seja exatamente como aparece em seu site.

Quando terminar, selecione **Salvar** para salvar o progresso.

## <a name="skus-form"></a>Formulário de SKUs

A próxima etapa é adicionar SKUs para a sua oferta.

Uma SKU é a menor unidade de compra de uma oferta. Use um SKU dentro da mesma classe de produto (oferta) para diferenciar entre:

* Diferentes recursos compatíveis
* Se a oferta é gerenciada ou não
* Modelos de cobrança compatíveis

Um SKU aparece sob a oferta pai no marketplace. Ele aparece como sua própria entidade disponível para compra no portal do Azure.

1. Selecione **SKUs** > **Novo SKU**.

1. Insira uma **ID de SKU**. Uma ID de SKU é um identificador exclusivo para o SKU dentro de uma oferta. Essa ID está visível em URLs de produto, modelos do Resource Manager e relatórios de cobrança. Ele só pode ser composto de caracteres alfanuméricos minúsculos ou traços (-). A ID não pode terminar com um traço e está limitada a 50 caracteres no máximo. Após a ativação da oferta, esse campo é bloqueado. Pode existir várias SKUs em uma oferta. Você precisa de um SKU para cada imagem que planeja publicar.

1. Preencha a seção **Detalhes do SKU** no seguinte formulário:

   Preencha os campos a seguir:

   * **Título**: Insira um título para esse SKU. Esse título aparecerá na galeria para este item.
   * **Resumo**: Insira um breve resumo para esse SKU. Esse texto aparece abaixo do título.
   * **Descrição**: Insira uma descrição detalhada sobre o SKU.
   * **Tipo de SKU**: Os valores permitidos são *Aplicativo Gerenciado* e *Modelos de Solução*. Nesse caso, selecione *Aplicativo Gerenciado*.
   * **Disponibilidade por País/Região**: Selecione os países/as regiões nos quais o aplicativo gerenciado está disponível.
   * **Preço**: Forneça um preço para o gerenciamento do aplicativo. Selecione os países/as regiões disponíveis antes de configurar o preço.

1. Adicione um novo pacote. Preencha a seção **Detalhes do Pacote** no seguinte formulário:

   Preencha os campos a seguir:

   * **Versão**: Insira uma versão para o pacote carregado. Ela deve estar no formato `{number}.{number}.{number}{number}`.
   * **Arquivo de pacote (.zip)** : Esse pacote contém dois arquivos necessários compactados em um arquivo .zip. Um arquivo é um modelo do Resource Manager que define os recursos para implantar o aplicativo gerenciado. O outro arquivo define a [interface do usuário](create-uidefinition-overview.md) para os consumidores implantarem o aplicativo gerenciado por meio do portal. Na interface do usuário, você especifica os elementos que permitem aos consumidores fornecer valores de parâmetro.
   * **PrincipalId**: Essa propriedade é o identificador do Azure AD (Azure Active Directory) de um usuário, um grupo de usuários ou um aplicativo que recebeu o acesso aos recursos na assinatura do cliente. A Definição de Função descreve as permissões.
   * **Definição de Função**: Essa propriedade é uma lista de todas as funções internas do RBAC (controle de acesso baseado em função) fornecidas pelo Azure AD. Você pode selecionar a função mais apropriada para gerenciar os recursos em nome do cliente.
   * **Configurações de Política**: Aplique um [Azure Policy](../governance/policy/overview.md) ao aplicativo gerenciado para especificar os requisitos de conformidade para as soluções implantadas. Entre as opções disponíveis, selecione as políticas a serem aplicadas. Para **Parâmetros de Política**, forneça uma cadeia de caracteres JSON com os valores de parâmetro. Para definições de política e o formato dos valores de parâmetro, veja [Exemplos de Azure Policy](../governance/policy/samples/index.md).

Você pode adicionar várias autorizações. Recomendamos que você crie um grupo de usuários do AD e especifica sua ID em **PrincipalId**. Dessa forma, você pode adicionar mais usuários ao grupo de usuários sem a necessidade de atualizar o SKU.

Para saber mais sobre RBAC, consulte [Introdução ao RBAC no Portal do Azure](../role-based-access-control/overview.md).

## <a name="marketplace-form"></a>Formulário do Marketplace

O formulário do Marketplace pede campos que aparecem no [Azure Marketplace](https://azuremarketplace.microsoft.com) e no [portal do Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>IDs de assinatura para versão prévia

Insira uma lista de IDs de assinatura do Azure que podem acessar a oferta após a publicação. Use essas assinaturas da lista branca para testar a oferta de versão prévia antes de ativá-la. Você pode compilar uma lista branca com até 100 assinaturas no portal de parceiros.

### <a name="suggested-categories"></a>Categorias sugeridas

Selecione até cinco categorias da lista às quais sua oferta pode ser associada. Essas categorias selecionadas serão usadas para mapear sua oferta para as categorias de produto disponíveis no [Azure Marketplace](https://azuremarketplace.microsoft.com) e no [Portal do Azure](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

O resumo de seu aplicativo gerenciado exibe os campos a seguir:

![Resumo do marketplace](./media/publish-marketplace-app/publishvm10.png)

A guia **Visão geral** de seu aplicativo gerenciado exibe os campos a seguir:

![Visão geral do Marketplace](./media/publish-marketplace-app/publishvm11.png)

A guia **Planos + Preços** de seu aplicativo gerenciado exibe os campos a seguir:

![Planos do marketplace](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Portal do Azure

O resumo de seu aplicativo gerenciado exibe os campos a seguir:

![Resumo do portal](./media/publish-marketplace-app/publishvm12.png)

A guia visão geral de seu aplicativo gerenciado exibe os campos a seguir:

![Visão geral do portal](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Diretrizes de logotipo

Siga estas diretrizes para qualquer logotipo carregado no Portal do Cloud Partner:

*   O design do Azure tem uma paleta de cores simples. Limite o número de cores primárias e secundárias em seu logotipo.
*   As cores do tema do portal são branca e preta. Não use essas cores como a cor de fundo de seu logotipo. Use uma cor que destaque seu logotipo no portal. É recomendável usar cores primárias simples. *Se você usar um plano de fundo transparente, certifique-se de que o logotipo e o texto não sejam branco, preto ou azul.*
*   Não use um fundo gradiente no logotipo.
*   Não coloque texto no logotipo, nem mesmo o nome ou marca da sua empresa. A aparência de seu logotipo deve ser simples e evitar gradientes.
*   Verifique se o logotipo não está esticado.

#### <a name="hero-logo"></a>Logotipo Hero

O logotipo hero é opcional. O publicador pode optar por não carregar um logotipo hero. Após o upload do logotipo hero, não é possível exclui-lo. Nessa ocasião, o parceiro deve seguir as diretrizes do Azure Marketplace para ícones hero.

Siga estas diretrizes para o ícone do logotipo hero:

*   O nome de exibição do publicador, o título do plano e o resumo longo da oferta são exibidas em branco. Portanto, não use uma cor clara de fundo do ícone hero. Não há permissão para telas de fundo pretas, brancas ou transparentes para ícones hero.
*   Depois que a oferta é listada, os elementos serão inseridos por meio de programação dentro do logotipo hero. Os elementos inseridos incluem o nome de exibição do editor, o título do plano, o resumo longo da oferta e o botão **Criar**. Consequentemente, não insira qualquer texto enquanto projeta o logotipo hero. Deixe o espaço vazio à direita, pois o texto é incluído nesse espaço por meio de programação. O espaço vazio para o texto deve ter 415 x 100 pixels à direita. É um deslocamento de 370 pixels a partir da esquerda.

    ![Exemplo de logotipo hero](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Formulário de suporte

Preencha o formulário **Suporte** com contatos de suporte de sua empresa. Essas informações podem ser de contatos da engenharia e do atendimento ao cliente.

## <a name="publish-an-offer"></a>Publicar uma oferta

Depois de preencher todas as seções, selecione **Publicar** para iniciar o processo de disponibilização de sua oferta aos clientes.

## <a name="next-steps"></a>Próximas etapas

* Para obter uma introdução aos aplicativos gerenciados, consulte [Visão geral de aplicativos gerenciados](overview.md).
* Para obter informações sobre como publicar um aplicativo gerenciado do Catálogo de Serviços, consulte [Criar e publicar um aplicativo gerenciado do Catálogo de Serviços](publish-service-catalog-app.md).