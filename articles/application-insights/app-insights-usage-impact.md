---
title: Impacto do uso do Azure Application Insights | Microsoft Docs
description: "Analise como propriedades diferentes podem afetar as taxas de conversão para partes dos aplicativos."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Análise do Impact com o Application Insights

O Impact analisa como tempos de carregamento e outras propriedades influenciam taxas de conversão de várias partes do aplicativo. Para sermos mais precisos, ele descobre como **qualquer dimensão** de uma **exibição de página**, **evento personalizado** ou **solicitação** afeta o uso de uma **exibição de página** diferente ou um **evento personalizado**. 

![Ferramenta Impact](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Ainda não sabe o que o Impact faz?

Uma maneira de ver o Impact é como a ferramenta definitiva para apresentar argumentos a alguém na equipe sobre como a lentidão em um aspecto do site está afetando se os usuários voltam. Embora os usuários possam tolerar uma determinada lentidão, o Impact fornece informações sobre como equilibrar melhor a otimização e o desempenho para maximizar a conversão do usuário.

Mas, a análise do desempenho é apenas um subconjunto dos recursos do Impact. Como o Impact oferece suporte a dimensões e eventos personalizados, responder a perguntas como de que maneira a escolha do navegador do usuário se correlaciona com taxas de conversão diferentes está a apenas alguns cliques.

![Conversão da captura de tela por navegadores](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> O recurso Application Insights deve conter exibições de página ou eventos personalizados para usar a ferramenta Impact. [Saiba como configurar seu aplicativo para coletar exibições de página automaticamente com o SDK do JavaScript do Application Insights](app-insights-javascript.md). Também se lembre de que, como você está analisando a correlação, o tamanho de exemplo importa.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>O tempo de carregamento da página está afetando quantas pessoas convertendo na minha página?

Para começar a responder a perguntas com a ferramenta Impact, escolha uma exibição de página inicial, um evento personalizado ou uma solicitação.

![Ferramenta Impact](./media/app-insights-usage-impact/0002-dropdown.png)

1. Selecione uma exibição de página no menu suspenso **Para a exibição de página**.
2. Deixe o menu suspenso **analisar como** na seleção padrão de **Duração** (neste contexto, **Duração** é um alias para **o Tempo de Carregamento da Página**).
3. Para o menu suspenso **afeta o uso do**, selecione um evento personalizado. Esse evento deve corresponder a um elemento de interface do usuário na exibição de página que você selecionou na etapa 1.

![Captura de tela dos resultados](./media/app-insights-usage-impact/0003-results.png)

Nesta instância, à medida que o tempo de carregamento da **Página do Produto** aumenta, a taxa de conversão para **Produto de Compra clicado** diminui. Com base na distribuição acima, uma duração de carregamento de página ideal de 3,5 segundos pode ser direcionada para atingir uma taxa de conversão de 55% em potencial. Mais melhorias de desempenho para reduzir o tempo de carregamento abaixo de 3,5 segundos não estão correlacionadas atualmente a benefícios de conversão adicionais.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>E se eu estou controlando exibições de página ou tempos de carregamento de maneiras personalizadas?

O Impact dá suporte a propriedades padrão e personalizadas, além de medidas. Use o que preferir. Em vez de duração, use filtros nos eventos primários e secundários para ser mais específico.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Usuários de países ou regiões diferentes são convertidos em taxas diferentes?

1. Selecione uma exibição de página no menu suspenso **Para a exibição de página**.
2. Escolha “País ou região” no menu suspenso **analisar como o**
3. Para o menu suspenso **afeta o uso do**, selecione um evento personalizado que corresponda a um elemento de interface do usuário na exibição de página que você escolheu na etapa 1.

Neste caso, os resultados deixam de se ajustar a um modelo de eixo x contínuo como fizeram no primeiro exemplo. Em vez disso, uma visualização semelhante a um funil segmentado é apresentada. Classifique por **Uso** para exibir a variação de conversão do evento personalizado com base no país.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Como a ferramenta Impact calcula essas taxas de conversão?

Nos bastidores, a ferramenta Impact depende do [Coeficiente de correlação Pearson] (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Os resultados são computados entre -1 e 1 com -1 que representando a correlação de zero e 1 representando uma correlação positiva.

A análise básica de como o Impact Analysis funciona é a seguinte:

Sendo _A_ = a exibição de página principal/evento personalizado/solicitação selecionada no primeiro menu suspenso. (**Para a exibição de página**).

Sendo _B_ = a exibição de página secundária/evento personalizado selecionada (**afeta o uso do**).

Impact analisa um exemplo de todas as sessões de usuários no intervalo de tempo selecionado. Para cada sessão, ela procura cada ocorrência de _A_.

Em seguida, as sessões são divididas em dois tipos diferentes de _subsessões_ com base em uma das duas condições:

- Uma subsessão convertida consiste em uma sessão que termina com um evento _B_ evento e abrange todos os eventos _A_ ocorridos antes de _B_.
- Uma subsessão não convertida ocorre quando todos os _A_s ocorrem sem um terminal _B_.

Como o impacto acaba sendo calculado varia de acordo com a análise feita por métrica ou dimensão. Para métricas, a média de todos os _A_s em uma subsessão é calculada. Embora para dimensões o valor de cada _A_ contribua _1/N_ para o valor atribuído a _B_ em que _N_ é o número de _A_s na subsessão.

## <a name="next-steps"></a>Próximas etapas

- Para habilitar as experiências de uso, comece enviando [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [exibições de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se você já envia eventos personalizados ou exibições de página, explore as ferramentas de uso para saber como os usuários utilizam o seu serviço.
    - [Funis](usage-funnels.md)
    - [Retenção](app-insights-usage-retention.md)
    - [Fluxos de Usuário](app-insights-usage-flows.md)
    - [Pastas de trabalho](app-insights-usage-workbooks.md)
    - [Adicionar contexto de usuário](app-insights-usage-send-user-context.md)