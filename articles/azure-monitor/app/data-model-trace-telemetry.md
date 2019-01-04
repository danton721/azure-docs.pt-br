---
title: Modelo de dados do Azure Application Insights Telemetry – telemetria de rastreamento | Microsoft Docs
description: Modelo de dados do Application Insights para telemetria de rastreamento
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: a80c96891f3d91a920519db2915932742bd84d72
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54002290"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetria de rastreamento: Modelo de dados do Application Insights

A telemetria de rastreamento (em [Application Insights](../../application-insights/app-insights-overview.md)) representa instruções de rastreamento de estilo `printf` pesquisadas por texto. `Log4Net`, `NLog` e outras entradas do arquivo de log baseadas em texto são convertidas em instâncias desse tipo. O rastreamento não tem medidas como uma extensibilidade.

## <a name="message"></a>Mensagem

Mensagem de rastreamento.

Tamanho máx. 32768 caracteres

## <a name="severity-level"></a>Nível de severidade

Nível de severidade de rastreamento. O valor pode ser `Verbose`, `Information`, `Warning`, `Error` ou `Critical`.

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Próximas etapas

- [Explore os logs de rastreamento do .NET no Application Insights](../../azure-monitor/app/asp-net-trace-logs.md).
- [Explore os logs de rastreamento de Java no Application Insights](../../azure-monitor/app/java-trace-logs.md).
- Consulte [modelo de dados](data-model.md) para modelo de dados e tipos do Application Insights.
- [Escrever telemetria personalizada de rastreamento](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Confira as [plataformas](../../azure-monitor/app/platforms.md) com suporte do Application Insights.