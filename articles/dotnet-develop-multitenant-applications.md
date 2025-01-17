---
title: Padrão de aplicativo Web multilocatário | Microsoft Docs
description: Encontre visões gerais de arquitetura e padrões de design que descrevem como implementar um aplicativo Web multilocatário no Azure.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 92a0caedca34756228dbf57ec9099fd2ece3d84e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66225984"
---
# <a name="multitenant-applications-in-azure"></a>Aplicativos multilocatários no Azure
Um aplicativo multilocatário é um recurso compartilhado que permite que usuários distintos ou "locatários" exibam o aplicativo como se ele fosse de sua propriedade. Um cenário típico que pode ser usado como um aplicativo multilocatário é aquele em que todos os usuários do aplicativo talvez queiram personalizar a experiência do usuário, mas que, de outra forma, têm os mesmos requisitos de negócios básico. Exemplos de grandes aplicativos multilocatários são o Office 365, o Outlook.com e o visualstudio.com.

Da perspectiva de um provedor de um aplicativo, os benefícios da multilocação na maioria das vezes estão relacionados à eficiência operacional e de custo. Uma versão do seu aplicativo pode atender às necessidades de muitos locatários/clientes permitindo a consolidação das tarefas de administração do sistema, como monitoramento, ajuste de desempenho, manutenção de software e backups de dados.

A lista a seguir fornece os objetivos e requisitos mais significativos de uma perspectiva de provedor.

* **Provisionamento**: Você deve ser capaz de provisionar novos locatários para o aplicativo.  Para aplicativos multilocatários com um grande número de locatários, geralmente é necessário automatizar esse processo permitindo provisionamento por autoatendimento.
* **Facilidade de manutenção**: Você deve ser capaz de atualizar o aplicativo e executar outras tarefas de manutenção enquanto vários locatários são usá-lo.
* **Monitoramento**: Você deve ser capaz de monitorar o aplicativo em todos os momentos para identificar problemas e solucioná-los. Isso inclui o monitoramento de como cada locatário está usando o aplicativo.

Um aplicativo multilocatário implementado adequadamente oferece os seguintes benefícios aos usuários.

* **Isolamento**: As atividades de locatários individuais não afetam o uso do aplicativo por outros locatários. Os locatários não podem acessar os dados uns dos outros. Ao locatário, parece que o aplicativo é de seu uso exclusivo.
* **Disponibilidade**: Locatários individuais querem o aplicativo esteja constantemente disponível, talvez com garantias definidas em um SLA. Novamente, as atividades de outros locatários não devem afetar a disponibilidade do aplicativo.
* **Escalabilidade**: O aplicativo é dimensionado para atender à demanda de locatários individuais. A presença e as ações de outros locatários não devem afetar o desempenho do aplicativo.
* **Custos**: Os custos são menores do que a execução de um aplicativo dedicado de único locatário porque a multilocação permite o compartilhamento de recursos.
* **Capacidade de personalização**. A capacidade de personalizar o aplicativo para um locatário individual de várias maneiras, como adicionar ou remover recursos, alterar cores e logotipos ou até mesmo adicionar seu próprio código ou script.

Em suma, embora haja muitas considerações que você deve levar em conta para fornecer um serviço altamente escalonável, também há uma série de metas e requisitos que são comuns a muitos aplicativos multilocatários. Alguns podem não ser relevantes em cenários específicos, e a importância de requisitos e metas individuais será diferente em cada cenário. Como um provedor do aplicativo multilocatário, você também terá metas e requisitos, como o locatário metas e requisitos, lucratividade, cobrança, vários níveis de serviço, provisionamento, monitoramento de facilidade de manutenção e automação de reunião.

Para obter mais informações sobre considerações de design adicionais de um aplicativo multilocatário, consulte [Hosting a Multi-Tenant Application on Azure][Hosting a Multi-Tenant Application on Azure] (Hospedando um aplicativo multilocatário no Azure). Para obter informações sobre os padrões comuns da arquitetura de dados dos aplicativos do banco de dados SaaS (software como serviço) multilocatário, consulte [Padrões de Design para Aplicativos SaaS multilocatário com o Banco de Dados SQL do Azure](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

O Azure oferece muitos recursos que permitem resolver os principais problemas encontrados durante a criação de um sistema multilocatário.

**Isolamento**

* Segmentar locatários de sites por Cabeçalhos de Host com ou sem comunicação SSL
* Segmentar locatários de site por parâmetros de consulta
* Serviços web em funções de trabalho
  * Funções de trabalho que normalmente processam dados no back-end de um aplicativo.
  * Funções Web que geralmente agem como o front-end de aplicativos.

**Armazenamento**

Gerenciamento de dados, como serviços de banco de dados SQL ou armazenamento do Azure, como o serviço tabela, que fornece serviços para o armazenamento de grandes quantidades de dados não estruturados e o serviço de Blob, que fornece serviços para armazenar grandes quantidades de texto não estruturado ou dados binários, como vídeo, áudio e imagens.

* Protegendo dados de Multilocatários no banco de dados SQL logons do SQL Server por locatário.
* Usando tabelas do Azure para recursos do aplicativo, especificando uma política de acesso no nível do contêiner, você pode ter a capacidade de ajustar as permissões sem precisar emitir novas URLs para os recursos protegidos com assinaturas de acesso compartilhado.
* Filas do Azure para recursos do aplicativo. As filas do Azure geralmente são usadas para processamento de unidades em nome de locatários, mas também podem ser usadas para distribuir o trabalho necessário para provisionamento ou gerenciamento.
* Filas do Service Bus para recursos do aplicativo que enviam trabalho por push para um serviço compartilhado, você pode usar uma única fila onde cada remetente locatário tem apenas permissões (conforme derivado das declarações emitidas no ACS) para envio por push àquela fila, enquanto somente os receptores do serviço têm permissão para efetuar pull na fila dos dados provenientes de vários locatários.

**Serviços de conexão e segurança**

* Service Bus do Azure, uma infraestrutura de mensagens situada entre aplicativos que permite que eles troquem mensagens de uma maneira vagamente acoplada para fornecer escala e resiliência melhoradas.

**Serviços de rede**

O Azure fornece vários serviços de rede que oferecem suporte à autenticação e melhoram a capacidade de gerenciamento dos aplicativos hospedados. esses serviços incluem o seguinte:

* A Rede Virtual do Azure permite que você provisione e gerencie VPNs (redes virtuais privadas) no Azure e vincule-as com segurança à infraestrutura de TI local.
* O Gerenciador de Tráfego de Rede Virtual permite que você equilibre o tráfego de entrada entre os vários serviços hospedados do Azure, quer eles estejam em execução no mesmo datacenter ou em diferentes datacenters ao redor do mundo.
* O Active Directory do Azure (AD do Azure) é um serviço moderno e baseado em REST que fornece recursos de gerenciamento de identidade e de controle de acesso para seus aplicativos na nuvem. Usando o Azure AD para recursos do aplicativo fornece uma maneira fácil de autenticar e autorizar usuários para obter acesso a seus aplicativos web e serviços, permitindo que os recursos de autenticação e autorização sejam fatorados fora do seu código.
* O Service Bus do Azure fornece um serviço de mensagens seguro e o recurso de fluxo de dados para aplicativos distribuídos e híbridos, como a comunicação entre aplicativos hospedados do Azure e aplicativos e serviços locais, sem a necessidade de firewall complexo e de infraestruturas de segurança. Usando a retransmissão do barramento de serviço para recursos do aplicativo para acessar os serviços que são expostos como pontos de extremidade pode pertencer ao locatário (por exemplo, hospedado fora do sistema, como no local) ou podem ser serviços provisionados especificamente para o locatário porque ( dados confidenciais específicos ao locatário trafegam entre eles).

**Provisionando recursos**

O Azure fornece várias maneiras de provisionar novos locatários para o aplicativo. Para aplicativos multilocatários com um grande número de locatários, geralmente é necessário automatizar esse processo permitindo provisionamento por autoatendimento.

* A funções de trabalho permitem provisionar e desprovisionar recursos por locatário (como quando um novo locatário faz ou cancela uma assinatura), coletar métricas para uso de medição e gerenciar a escala seguindo uma determinada agenda ou em resposta ao cruzamento de limites dos principais indicadores de desempenho. Essa mesma função também pode ser usada para enviar atualizações e upgrades por push para a solução.
* Os Blobs do Azure podem ser usados para provisionar computação ou recursos de armazenamento pré-inicializados para novos locatários fornecendo, ao mesmo tempo, políticas de acesso em nível de contêiner para proteger os pacotes, imagens VHD e outros recursos do serviço de computação.
* As opções para provisionamento de recursos de Banco de Dados SQL para um locatário incluem:
  
  * DDL em scripts ou incorporada como recursos em assemblies.
  * Pacotes de DAC do SQL Server 2008 R2 implantados programaticamente.
  * Copiando de um banco de dados mestre de referência.
  * Uso de importação e exportação de banco de dados para provisionar novos bancos de dados de um arquivo.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
