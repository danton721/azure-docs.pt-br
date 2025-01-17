---
title: Criar gatilhos de janela em cascata no Azure Data Factory | Microsoft Docs
description: Saiba como criar um gatilho no Azure Data Factory que execute um pipeline em uma janela em cascata.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: shlo
ms.openlocfilehash: 6fbdee71ab1123c258a5191a78e38f51eb41cbab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66152923"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Criar um gatilho que execute um pipeline em uma janela em cascata
Este artigo fornece etapas para criar, iniciar e monitorar um gatilho de janela em cascata. Para obter informações gerais sobre gatilhos e os tipos com suporte, consulte [Gatilhos e execução de pipeline](concepts-pipeline-execution-triggers.md).

Os gatilhos de janela em cascata são um tipo de gatilho acionado em um intervalo de tempo periódico a partir de uma hora de início especificada, enquanto mantém o estado. As janelas em cascata são uma série de intervalos de tempo de tamanho fixo, não sobrepostos e contíguos. Um gatilho de janela em cascata tem uma relação de um para um com um pipeline, e só pode fazer referência a um único pipeline.

## <a name="data-factory-ui"></a>IU do Data Factory

Para criar um gatilho de janela em cascata no portal do Azure, selecione **disparador > janela em cascata > Avançar**e, em seguida, configure as propriedades que definem a janela em cascata.

![Criar um gatilho de janela em cascata no portal do Azure](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>Propriedades do tipo de gatilho da janela em cascata
Uma janela em cascata tem as seguintes propriedades de tipo de gatilho:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "parameter1": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter2": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
            }
        }
    }
}
```

A seguinte tabela fornece uma visão geral de alto nível dos principais elementos de JSON relacionados à recorrência e ao agendamento de um gatilho de janela em cascata:

| Elemento JSON | Descrição | Type | Valores permitidos | Obrigatório |
|:--- |:--- |:--- |:--- |:--- |
| **type** | O tipo do gatilho. O tipo é o valor fixo "TumblingWindowTrigger." | String | "TumblingWindowTrigger" | Sim |
| **runtimeState** | O estado atual do tempo de execução do gatilho.<br/>**Observação**: Este elemento é \<readOnly >. | String | “Iniciado”, “Interrompido”, “Desabilitado” | Sim |
| **frequency** | Uma cadeia de caracteres que representa a unidade de frequência (em minutos ou horas) no qual o gatilho ocorre periodicamente. Se os valores de dados **startTime** são mais granulares do que o valor de **frequência**, os dados **startTime** são consideradas quando os limites da janela são computados. Por exemplo, se o valor de **frequência** for de hora em hora e o valor **startTime** for 2017-09-01T10:10:10Z, a primeira janela será (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | String | "minuto", "hora"  | Sim |
| **interval** | Um inteiro positivo que indica o intervalo para o valor de **frequência**, que determina a frequência com a qual o gatilho é executado. Por exemplo, se **intervalo** for 3 e **frequência** é "hora", o gatilho é repetido a cada 3 horas. | Integer | Um número inteiro positivo. | Sim |
| **startTime**| A primeira ocorrência, que pode estar no passado. O primeiro intervalo do gatilho é (**startTime**, **startTime** + **intervalo**). | DateTime | Um valor Datetime. | Sim |
| **endTime**| A última ocorrência, que pode estar no passado. | DateTime | Um valor Datetime. | Sim |
| **delay** | A quantidade de tempo para atrasar o início do processamento de dados para a janela. A execução do pipeline é iniciada após o tempo de execução esperado mais a quantidade de **atraso**. O **atraso** define quanto tempo o gatilho espera antes de disparar uma nova execução. O **atraso** não altera a janela **startTime**. Por exemplo, um valor **atraso** igual a 00:10:00 indica um atraso de 10 minutos. | Timespan<br/>(hh:mm:ss)  | Um valor temporal em que o padrão é 00:00:00. | Não |
| **maxConcurrency** | O número de execuções do gatilho simultâneas que são acionadas para janelas prontas. Por exemplo, para fazer o preenchimento por hora é executada para resultados de ontem em 24 janelas. Se **maxConcurrency** = 10, os eventos de gatilho serão disparados somente para as 10 primeiras janelas (00:00-01:00 - 09:00-10:00). Após a conclusão das primeiras 10 execuções de pipeline disparada, as execuções de gatilho serão disparadas para as próximas 10 janelas (10:00-11:00-19:00 às 20:00). Continuando com esse exemplo de **maxConcurrency** = 10, se houver 10 janelas prontas, há 10 execuções totais de pipeline. Se houver apenas 1 janela pronta, há apenas 1 execução do pipeline. | Integer | Um número inteiro entre 1 e 50. | Sim |
| **retryPolicy: Count** | O número de repetições antes da execução do pipeline ser marcada como "Failed."  | Integer | Um inteiro, em que o padrão é 0 (sem repetições). | Não |
| **retryPolicy: intervalInSeconds** | O intervalo entre tentativas de repetição especificado em segundos. | Integer | O número de segundos, em que o padrão é 30. | Não |

### <a name="windowstart-and-windowend-system-variables"></a>Variáveis do sistema WindowStart e WindowEnd

Você pode usar as variáveis de sistema **WindowStart** e **WindowEnd** do gatilho de janela em cascata na sua definição de **pipeline** (ou seja, para a parte de uma consulta). Passar as variáveis de sistema como parâmetros para o pipeline na definição de **gatilho**. O exemplo a seguir mostra como passar essas variáveis como parâmetros:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "MyWindowStart": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "MyWindowEnd": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                }
            }
        }
    }
}
```

Para usar os valores variáveis de sistema de **WindowStart** e **WindowEnd** na definição de pipeline, use adequadamente seus parâmetros de "MyWindowStart" e "MyWindowEnd".

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Ordem de execução de janelas em um cenário de compensação
Quando há várias janelas para execução (especialmente no cenário de compensação), a ordem de execução das janelas será determinante, dos intervalos mais antigos para os mais recentes. Atualmente, esse comportamento não pode ser modificado.

### <a name="existing-triggerresource-elements"></a>Elementos TriggerResource existentes
Os pontos a seguir se aplicam a elementos **TriggerResource** existentes:

* Se o valor para o elemento **frequência** (ou tamanho da janela) do gatilho altera, o estado das janelas que já foram processadas *não* é redefinido. O gatilho continua acionando para as janelas, desde a última janela executada usando o novo tamanho de janela.
* Se o valor para o elemento **endTime** do gatilho altera (adicionado ou atualizado), o estado das janelas que já foram processadas *não* é redefinido. O gatilho respeita o novo valor **endTime**. Se o novo valor **endTime** estiver antes da janela que já foi executado, o gatilho é interrompido. Caso contrário, o gatilho interrompe quando o novo valor **endTime** for encontrado.

## <a name="sample-for-azure-powershell"></a>Exemplo para o Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Esta seção mostra como usar o Azure PowerShell para criar, iniciar e monitorar um gatilho.

1. Crie um arquivo JSON chamado **MyTrigger.json** na pasta C:\ADFv2QuickStartPSH\ com o seguinte conteúdo:

    > [!IMPORTANT]
    > Antes de salvar o arquivo JSON, defina o valor do elemento **startTime** com a hora UTC atual. Defina o valor do elemento **endTime** como uma hora após a hora UTC atual.

    ```json
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```

2. Criar um gatilho usando o **AzDataFactoryV2Trigger conjunto** cmdlet:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. Confirme se o status do gatilho está **Stopped** usando o **Get-AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Inicie o gatilho usando o **AzDataFactoryV2Trigger início** cmdlet:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Confirme se o status do gatilho está **iniciado** usando o **Get-AzDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. Obter o gatilho é executado no Azure PowerShell usando o **Get-AzDataFactoryV2TriggerRun** cmdlet. Para obter informações sobre as execuções do gatilho, execute o comando a seguir periodicamente. Atualize os valores de **TriggerRunStartedAfter** e **TriggerRunStartedBefore** para que correspondam aos valores na definição de gatilho:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Para monitorar execuções de gatilho e de pipeline no portal do Azure, consulte [Monitorar execuções de pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações detalhadas sobre gatilhos, consulte [Gatilhos e execução de pipeline](concepts-pipeline-execution-triggers.md#triggers).
