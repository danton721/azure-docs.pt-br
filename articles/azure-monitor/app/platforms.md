---
title: 'Application Insights: linguagens, plataformas e integrações| Microsoft Docs'
description: Linguagens, plataformas e integrações disponíveis para o Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/16/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 9ef5c9e8b4a46b4bf2bed25027602fb98a09dbf0
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66256101"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Análise para o desenvolvedor: linguagens, plataformas e integrações
Esses itens são implementações do [Application Insights](../../azure-monitor/app/app-insights-overview.md) sobre as quais ouvimos falar, incluindo algumas de terceiros.

## <a name="languages---officially-supported-by-application-insights-team"></a>Linguagens oficialmente suportadas pela equipe do Application Insights
* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [Páginas da Web JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)

## <a name="languages---community-supported"></a>Linguagens com suporte da comunidade
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [Go](https://github.com/Microsoft/ApplicationInsights-go)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Qualquer outra](#projects)

## <a name="platforms-and-frameworks"></a>Plataformas e estruturas
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET – para aplicativos que já estejam ao vivo](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Serviço de Aplicativo do Azure](../../azure-monitor/app/azure-web-apps.md)
* [Serviços de Nuvem do Azure](../../azure-monitor/app/cloudservices.md)&#151; incluindo funções Web e de trabalho
* [Funções do Azure](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
* [Docker](../../azure-monitor/app/docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [Pilha de segurança](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](https://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Aplicativo Universal do Windows](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Funções de trabalho, serviços e aplicativos da área de trabalho do Windows](../../azure-monitor/app/windows-desktop.md)
* [Qualquer outra](#projects)

## <a name="logging-frameworks"></a>Estruturas de registro em log
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog ou System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J ou Logback](../../azure-monitor/app/java-trace-logs.md)
* [Registro Semântico (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) - é integrado ao [Bloco de Aplicativos do Registro Semântico](https://msdn.microsoft.com/library/dn440729.aspx)
* [Teste de carga baseado em nuvem](https://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [Plug-in LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="content-management-systems"></a>Sistemas de Gerenciamento de Conteúdo
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](../../azure-monitor/app/sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Análise de Dados e Exportação
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md )

## <a name="projects"></a> Criar seu próprio SDK
Se ainda não houver um SDK para sua linguagem ou plataforma, você gostaria de criar um? Examine o código dos SDKs existentes listados no [Projeto de SDK do Application Insights no GitHub](https://github.com/Microsoft/AppInsights-Home).
