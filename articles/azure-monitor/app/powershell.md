---
title: Automatizar o Azure Application Insights com o PowerShell | Microsoft Docs
description: Automatize criando testes de disponibilidade, alerta e recursos no PowerShell usando um modelo do Azure Resource Manager.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: mbullwin
ms.openlocfilehash: 07d52544b584adb02cc60790b7cb63c8aee1e366
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514473"
---
#  <a name="create-application-insights-resources-using-powershell"></a>Criar recursos do Application Insights usando o PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Este artigo mostra como automatizar a criação e atualização de recursos do [Application Insights](../../azure-monitor/app/app-insights-overview.md) automaticamente usando o Gerenciamento de Recursos do Azure. Por exemplo, você pode fazer isso como parte de um processo de compilação. Juntamente com o recurso básico do Application Insights, é possível criar [testes na Web de disponibilidade](../../azure-monitor/app/monitor-web-app-availability.md), configurar [alertas](../../azure-monitor/app/alerts.md), definir o [esquema de preços](pricing.md) e criar outros recursos do Azure.

A chave para criar esses recursos são os modelos de JSON para o [Gerenciador de Recursos do Azure](../../azure-resource-manager/manage-resources-powershell.md). Em resumo, o procedimento é: baixar as definições de JSON dos recursos existentes; parametrizar certos valores como nomes; e, em seguida, executar o modelo sempre que você deseja criar um novo recurso. Você pode empacotar vários recursos juntos, criá-los de uma só vez - por exemplo, um monitor de aplicativo com testes de disponibilidade, alertas e armazenamento para exportação contínua. Existem algumas sutilezas para algumas das parametrizações, que vamos explicar aqui.

## <a name="one-time-setup"></a>Configuração única
Se você nunca usou o PowerShell com sua assinatura do Azure:

Instale o módulo do Azure Powershell no computador em que você deseja executar os scripts.

1. Instale o [Microsoft Web Platform Installer (v5 ou superior)](https://www.microsoft.com/web/downloads/platform.aspx).
2. Use-o para instalar o Microsoft Azure PowerShell

## <a name="create-an-azure-resource-manager-template"></a>Criar um modelo do Azure Resource Manager
Criar um novo arquivo .json - vamos chamá-lo de `template1.json` neste exemplo. Copie este conteúdo nele:

```JSON
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "other"
                ],
                "metadata": {
                    "description": "Enter the application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter the application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Per GB (Basic), 2 = Per Node (Enterprise)"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a>Criar recursos do Application Insights
1. No PowerShell, entre no Azure:
   
    `Connect-AzAccount`
2. Execute um comando como este:
   
    ```PS
   
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName` é o grupo em que você deseja criar novos recursos.
   * `-TemplateFile` deve ocorrer antes dos parâmetros personalizados.
   * `-appName` O nome do recurso a ser criado.

Você pode adicionar outros parâmetros – você encontrará suas descrições na seção de parâmetros do modelo.

## <a name="to-get-the-instrumentation-key"></a>Para obter a chave de instrumentação
Depois de criar um recurso de aplicativo, você desejará a chave de instrumentação: 

```PS
    $resource = Find-AzResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>Definir o plano de preço

É possível definir o [plano de preço](pricing.md).

Para criar um recurso de aplicativo com o plano de preço Enterprise, usando o modelo acima:

```PS
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|plan|
|---|---|
|1|Basic|
|2|Enterprise|

* Se desejar usar apenas o plano de preço Básico padrão, omita o recurso CurrentBillingFeatures do modelo.
* Se você quiser alterar o plano de preço depois que o recurso do componente tiver sido criado, você poderá usar um modelo que omita o recurso "microsoft.insights/components". Além disso, omita o nó `dependsOn` do recurso de cobrança. 

Para verificar o plano de preços atualizado, consulte a página **Uso e custos estimados** no navegador. **Atualize a exibição do navegador** para certificar-se de que você vê o estado mais recente.



## <a name="add-a-metric-alert"></a>Adicionar um alerta de Métrica

Para configurar um alerta de métrica simultaneamente ao recurso de aplicativo, mescle código como este no arquivo de modelo:

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Quando você invoca o modelo, você pode adicionar este parâmetro:

    `-responseTime 2`

É claro que você pode parametrizar outros campos. 

Para descobrir os nomes de tipo e os detalhes de configuração de outras regras de alerta, crie uma regra manualmente e, em seguida, inspecione-a no [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Adicionar um teste de disponibilidade

Este exemplo é para um teste de ping (para testar uma única página).  

**Há duas partes** em um teste de disponibilidade: o próprio teste e o alerta que notifica você sobre falhas.

Mescle o código a seguir no arquivo de modelo que cria o aplicativo.

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExecutionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Para descobrir os códigos para outros locais de teste ou para automatizar a criação de testes da Web mais complexos, crie um exemplo manualmente e, em seguida, parametrize o código do [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Adicionar mais recursos

Para automatizar a criação de qualquer outro recurso de qualquer variante, crie um exemplo manualmente e, em seguida, copie e parametrize seu código do [Azure Resource Manager](https://resources.azure.com/). 

1. Abra o [Gerenciador de Recursos do Azure](https://resources.azure.com/). Navegar por `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components` até o recurso do aplicativo. 
   
    ![Navegação no Gerenciador de recursos do Azure](./media/powershell/01.png)
   
    *Componentes* são os recursos básicos do Application Insights para exibir aplicativos. Há recursos separados para as regras de alerta associadas e testes da web de disponibilidade.
2. Copie o JSON do componente para o local apropriado no `template1.json`.
3. Exclua essas propriedades:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Abra as seções webtests e alertrules e copie o JSON para itens individuais no seu modelo. (Não copie de nós webtests ou alertrules: vá para os itens sob os mesmos.)
   
    Cada teste da web tem uma regra de alerta associada, então você precisa copiar ambos.
   
    Você também pode incluir alertas sobre métricas. [Nomes de métrica](powershell-alerts.md#metric-names).
5. Insira esta linha em cada recurso:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Parametrizar o modelo
Agora você tem que substituir os nomes específicos por parâmetros. Para [parametrizar um modelo](../../azure-resource-manager/resource-group-authoring-templates.md), escreva expressões que usam um [conjunto de funções auxiliares](../../azure-resource-manager/resource-group-template-functions.md). 

Você não pode parametrizar apenas uma parte de uma cadeia de caracteres, então use `concat()` para criar cadeias de caracteres.

Estes são exemplos das substituições que você vai querer fazer. Há várias ocorrências de cada substituição. Talvez seja necessário outras em seu modelo. Esses exemplos usam os parâmetros e variáveis definidos na parte superior do modelo.

| find | substitua por |
| --- | --- |
| `"hidden-link:/subscriptions/.../../components/MyAppName"` |`"[concat('hidden-link:',`<br/>`resourceId('microsoft.insights/components',` <br/> `parameters('appName')))]"` |
| `"/subscriptions/.../../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (minúscula) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>`Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Exclua Guid e ID. |

### <a name="set-dependencies-between-the-resources"></a>Definir dependências entre os recursos
O Azure deve configurar os recursos na ordem explícita. Para certificar-se de que a instalação é concluída antes do início da próxima, adicione linhas de dependência:

* No recurso do teste de disponibilidade:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* No recurso de alerta para um teste de disponibilidade:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Próximas etapas
Outros artigos sobre automação:

* [Criar um recurso do Application Insights](powershell-script-create-resource.md) -método rápido sem usar um modelo.
* [Configurar alertas](powershell-alerts.md)
* [Criar testes na Web](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Enviar o Diagnóstico do Azure para o Application Insights](powershell-azure-diagnostics.md)
* [Implantar no Azure pelo GitHub](https://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Criar anotações de versão](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)
