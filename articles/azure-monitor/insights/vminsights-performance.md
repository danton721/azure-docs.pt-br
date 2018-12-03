---
title: Como mapear o desempenho com o Monitor do Azure para VMs (Visualizar) | Microsoft Docs
description: O desempenho é um recurso do Monitor do Azure para VMs que descobre automaticamente os componentes do aplicativo nos sistemas Windows e Linux e mapeia a comunicação entre os serviços. Este artigo fornece detalhes sobre como usá-lo em vários cenários.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 2ece5700965018a8a9a98e16a1577309a909b7b7
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334986"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms-preview"></a>Como mapear o desempenho com o Monitor do Azure para VMs (visualização)
O Monitor do Azure para VMs inclui um conjunto de gráficos de desempenho que segmentam vários KPIs (principais indicadores de desempenho) para ajudá-lo a determinar o desempenho de uma máquina virtual. Os gráficos mostram a utilização de recursos durante um período de tempo para que você possa identificar afunilamentos, anomalias ou alternar para uma perspectiva listando cada máquina para exibir a utilização de recursos com base na métrica selecionada. Embora haja vários elementos a serem considerados ao lidar com o desempenho, o Monitor do Azure para VMs é focado no sistema operacional, conforme manifestado no processador, na memória, nos adaptadores de rede e nos discos. O desempenho complementa o recurso de monitoramento de integridade e ajuda a expor problemas que indicam uma possível falha do componente do sistema, suporte ao ajuste e otimização para obter eficiência ou suportar o planejamento da capacidade.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Perspectiva de várias VMs do Azure Monitor
No Azure Monitor, o recurso Desempenho fornece uma exibição de máquina virtual múltipla de todas as VMs monitoradas implementadas em grupos de recursos em suas assinaturas ou em seu ambiente.  Para acessar do Monitor do Azure, execute as etapas a seguir. 

1. No portal do Azure, selecione **Monitor**. 
2. Escolher **máquinas virtuais (versão prévia)** na **soluções** seção.
3. Selecione o **desempenho** guia.

![Exibição de desempenho superior N lista de insights VM](./media/vminsights-performance/vminsights-performance-aggview-01.png)

Na guia **Top N Charts**, se você tiver mais de uma área de trabalho do Log Analytics, escolha a área de trabalho habilitada com a solução do seletor **Workspace** na parte superior da página. O seletor **Grupo** retornará assinaturas, grupos de recursos, [grupos de computadores](../../log-analytics/log-analytics-computer-groups.md) e conjuntos de computadores em escala virtual relacionados à área de trabalho selecionada que você pode usar para filtrar ainda mais os resultados apresentados nos gráficos esta página e nas outras páginas. Sua seleção só se aplica ao recurso Performance e não é transferida para Health ou Map.  

Por padrão, os gráficos mostram as últimas 24 horas. Usando o seletor **TimeRange**, você pode consultar intervalos de tempo históricos de até 30 dias para mostrar como o desempenho parecia no passado.   

Os gráficos de utilização de cinco capacidade mostrados na página são:

* % De utilização da CPU - mostra as cinco principais máquinas com a utilização média mais alta do processador 
* Memória disponível - mostra as cinco principais máquinas com a menor quantidade média de memória disponível 
* Espaço em disco lógico usado% - mostra as cinco principais máquinas com o maior espaço em disco usado% em todos os volumes de disco 
* Bytes Sent Rate - mostra as cinco principais máquinas com a maior média de bytes enviados 
* Bytes Receive Rate - mostra as cinco principais máquinas com a maior média de bytes enviados 

Clicar no canto superior direito de qualquer um dos cinco gráficos abrirá a visualização **Top N List**.  Aqui você pode ver a utilização de recursos para essa métrica de desempenho por VM individual em uma exibição de lista e qual máquina está tendências mais alta.  

![Exibição de lista de N superior de uma métrica de desempenho selecionados](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Quando você clica na máquina virtual, o painel **Propriedades** é expandido à direita para mostrar as propriedades do item selecionado, como as informações do sistema relatadas pelo sistema operacional, as propriedades da VM do Azure etc. Clicar em uma das opções na seção **Links rápidos** o redirecionará para esse recurso diretamente da VM selecionada.  

![Painel de propriedades da máquina virtual](./media/vminsights-performance/vminsights-properties-pane-01.png)

Alterne para a guia **Gráficos agregados** para visualizar as métricas de desempenho filtradas por medidas médias ou de percentis.  

![Visão geral do desempenho das informações de VM](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Os seguintes gráficos de utilização de capacidade são fornecidos:

* CPU Utilization% - padrões que mostram o percentual médio e superior 95 
* Memória disponível - padrões mostrando a média, o 5º e o 10º percentil 
* Espaço em disco lógico usado% - padrões mostrando a média e o percentil 95 
* Bytes Sent Rate - padrões mostrando os bytes médios enviados 
* Taxa de recebimento de bytes - padrões que mostra a média de bytes recebidos

Você também pode alterar a granularidade dos gráficos dentro do intervalo de tempo, selecionando **Avg**, **Min**, **Max**, **percentis 50**,  **90 º**, e **95 º** no seletor de percentil.   

Para exibir a utilização de recursos por VM individual em uma exibição de lista e ver qual máquina está tendências com maior utilização, selecione a guia **Top N lista**.  A página **Top N List** mostra as 20 principais máquinas classificadas pelas mais utilizadas pelo percentil 95 da métrica *CPU Utilization%*.  Você pode ver mais máquinas, selecionando **carga mais**, e os resultados se expandem para mostrar as máquinas da parte superior a 500. 

>[!NOTE]
>A lista não é possível mostrar mais de 500 máquinas cada vez.  
>

![Exemplo de página de lista de N superior](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Para filtrar os resultados em uma máquina virtual específica na lista, insira o nome do computador na caixa de texto **Pesquisar por nome**.  

Se você em vez disso, seria exibir utilização de uma métrica de desempenho diferentes, do **métrica** lista suspensa, selecione **memória disponível**, **% de espaço em disco lógico usado**,  **Bytes/s de recebimento de rede**, ou **bytes/s de rede enviados** e a lista é atualizada para mostrar a utilização no escopo dessa métrica.  

A seleção de uma máquina virtual a partir da lista abre o painel **Propriedades** no lado direito da página e, a partir daqui, você pode selecionar **Detalhes de desempenho**.  A página **Detalhes da Máquina Virtual** é aberta e tem o escopo definido para essa VM, com experiência semelhante ao acessar o VM Insights Performance diretamente da VM do Azure.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Exibir desempenho diretamente de uma VM do Azure
Para acessar diretamente a partir de uma máquina virtual, execute o seguinte procedimento.

1. No Portal do Azure, selecione **Máquinas Virtuais**. 
2. Na lista, escolha uma máquina virtual e, na **Monitoring** seção escolher **Insights (visualização)**.  
3. Selecione o **desempenho** guia. 

Esta página inclui não apenas gráficos de utilização de desempenho, mas também uma tabela mostrando cada disco lógico descoberto, sua capacidade, utilização e média total de cada medida.  

Os seguintes gráficos de utilização de capacidade são fornecidos:

* CPU Utilization% - padrões que mostram o percentual médio e superior 95 
* Memória disponível - padrões mostrando a média, o 5º e o 10º percentil 
* Espaço em disco lógico usado% - padrões mostrando a média e o percentil 95 
* IOPS de disco lógico - padrões mostrando a média e o percentil 95
* MB / s de disco lógico - padrões mostrando a média e o percentil 95
* Max Logical Disk Used% - padrões mostrando a média e o percentil 95
* Bytes Sent Rate - padrões mostrando os bytes médios enviados 
* Taxa de recebimento de bytes - padrões que mostra a média de bytes recebidos

![Diretamente o insights VM-desempenho da VM a exibir](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="alerting-and-alert-management"></a>Gerenciamento de alerta e alerta 
As métricas de desempenho ativadas como parte do Monitor do Azure para VMs não incluem regras de alerta pré-configuradas. Embora existam [alertas de integridade](vminsights-health.md#alerting-and-alert-management) correspondentes a problemas de desempenho detectados na VM do Azure, como alta utilização da CPU, pouca memória disponível, I/O de disco, pouco espaço em disco etc., esses alertas de integridade são aplicados somente a todas as VMs conectadas à mesma área de trabalho do Log Analytics ativada para o Monitor do Azure para VMs. 

No entanto, só podemos coletar e armazenar um subconjunto das métricas de desempenho necessárias no espaço de trabalho do Log Analytics. Se sua estratégia de monitoramento exigir análise ou alerta que inclua outras métricas de desempenho para avaliar efetivamente a capacidade ou a integridade da máquina virtual, ou você precisar da flexibilidade para especificar seus próprios critérios de alerta ou lógica, é possível configurar a [coleta desses desempenhos contadores](../../azure-monitor/platform/data-sources-performance-counters.md?toc=/azure/azure-monitor/toc.json) no Log Analytics e definir [alertas de log](../../monitoring-and-diagnostics/alert-log.md?toc=/azure/azure-monitor/toc.json). Embora o Log Analytics permita que você realize análises complexas com outros tipos de dados e forneça uma retenção mais longa para suportar a análise de tendências, as métricas, por outro lado, são leves e capazes de suportar cenários quase em tempo real. Eles são coletados pelo [Agente de Diagnóstico do Azure](../../virtual-machines/windows/monitor.md) e armazenados no repositório de métricas do Monitor do Azure, permitindo que você crie alertas com menor latência e a um custo menor.

Revise a visão geral da [coleção de métricas e logs com o Monitor do Azure](../../azure-monitor/platform/data-collection.md?toc=/azure/azure-monitor/toc.json) para entender melhor as diferenças fundamentais e outras considerações antes de configurar a coleta dessas métricas adicionais e regras de alerta.  

## <a name="next-steps"></a>Próximas etapas
Para saber como usar o recurso de integridade, consulte [modo de exibição do Azure Monitor de integridade de VMs](vminsights-health.md), ou para exibir dependências de aplicativos descobertos, consulte [modo de exibição do Azure Monitor para VMs mapa](vminsights-maps.md). 