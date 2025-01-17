---
title: Criar perfis de aplicativos Web em execução em uma VM do Azure com o Application Insights Profiler | Microsoft Docs
description: Crie perfis de aplicativos Web em uma VM do Azure com o Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: ab30351bfff9c5bbf070a1e8a54a4919e4d2231a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226261"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Crie perfis de aplicativos Web em execução em uma máquina virtual do Azure ou um conjunto de dimensionamento de máquinas virtuais definido com o Application Insights Profiler

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Você também pode implantar o Azure Application Insights Profiler nestes serviços:
* [Serviço de Aplicativo do Azure](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Serviços de Nuvem do Azure](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Implantar o Profiler em uma máquina virtual ou em um conjunto de dimensionamento de máquinas virtuais
Este artigo orientará você pelas etapas necessárias para a execução do Application Insights Profiler em seu conjunto de dimensionamento de máquinas virtuais do Azure ou na VM (máquina virtual) do Azure. O Profiler é instalado com a extensão de Diagnóstico do Azure para VMs. Configure a extensão para executar o Profiler e criar o SDK do Application Insights em seu aplicativo.

1. Adicionar SDK do Application Insights para seu [aplicativo ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net).

   Para exibir perfis das suas solicitações, você precisa enviar a telemetria de solicitação para o Application Insights.

1. Instale a extensão do Diagnóstico do Azure na VM. Para exemplos completos de modelos do Resource Manager, consulte:  
   * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
   * [Conjunto de dimensionamento de máquinas virtuais](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
     A chave fundamental é a ApplicationInsightsProfilerSink no WadCfg. Para que o Diagnóstico do Azure habilite o Profiler a enviar dados para o iKey, adicione outro coletor a esta seção.
    
     ```json
     "SinksConfig": {
       "Sink": [
         {
           "name": "ApplicationInsightsSink",
           "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
         },
         {
           "name": "MyApplicationInsightsProfilerSink",
           "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
         }
       ]
     },
     ```

1. Implante a definição de implantação de ambiente modificada.  

   A aplicação das modificações geralmente envolve uma implantação de modelo completo ou uma publicação baseada em serviço de nuvem por meio dos cmdlets do PowerShell ou do Visual Studio.  

   Os seguintes comandos do PowerShell são uma abordagem alternativa para máquinas virtuais existentes que tocam apenas a extensão de diagnóstico do Azure. Adicione o ProfilerSink mencionada anteriormente para a configuração que é retornada pelo comando Get-AzVMDiagnosticsExtension. Em seguida, passe a configuração atualizada para o comando Set-AzVMDiagnosticsExtension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Se o aplicativo desejado estiver em execução por meio do [IIS](https://www.microsoft.com/web/downloads/platform.aspx), habilite o recurso do Windows `IIS Http Tracing`.

    a. Estabeleça acesso remoto ao ambiente e, em seguida, use a janela [Adicionar recursos do Windows]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/). Ou execute o seguinte comando no PowerShell (como administrador):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Se estabelecer o acesso remoto for um problema, você poderá usar a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) para executar o comando a seguir:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Implante seu aplicativo.

## <a name="set-profiler-sink-using-azure-resource-explorer"></a>Defina o Profiler do coletor usando o Gerenciador de recursos do Azure
Ainda não temos uma maneira de configurar o coletor do Application Insights Profiler do portal. Em vez de usar o powershell, como descrito acima, você pode usar o Gerenciador de recursos do Azure para definir o coletor. Mas observe que, se você implantar a VM novamente, o coletor serão perdido. Você precisará atualizar a configuração que usar ao implantar a VM para preservar essa configuração.

1. Verifique se a extensão de diagnóstico do Windows Azure é instalada, exibindo as extensões instaladas para sua máquina virtual.  

    ![Verifique se a extensão WAD está instalado][wadextension]

1. Localize a extensão de diagnóstico de VM para sua VM. Expanda o grupo de recursos, virtualMachines Microsoft. Compute, nome da máquina virtual e extensões.  

    ![Navegue até a configuração WAD no Gerenciador de recursos do Azure][azureresourceexplorer]

1. Adicione o coletor do Application Insights Profiler para o nó SinksConfig em WadCfg. Se você ainda não tiver uma seção SinksConfig, talvez seja necessário adicionar um. Certifique-se de especificar a iKey do Application Insights adequada em suas configurações. Você precisará alternar o modo de explorers para leitura/gravação no canto superior direito e pressione o botão azul 'Editar'.

    ![Adicionar o coletor do Application Insights Profiler][resourceexplorersinksconfig]

1. Quando você terminar editando a configuração, pressione 'Put'. Se o put for bem-sucedida, uma marca de seleção verde aparecerá no meio da tela.

    ![Enviar uma solicitação put para aplicar alterações][resourceexplorerput]






## <a name="can-profiler-run-on-on-premises-servers"></a>O Profiler pode ser executado em servidores locais?
Não temos planos para dar suporte ao Application Insights Profiler para servidores locais.

## <a name="next-steps"></a>Próximas etapas

- Gere tráfego para seu aplicativo (por exemplo, inicie um [teste de disponibilidade](monitor-web-app-availability.md)). Em seguida, espere de 10 a 15 minutos para que os rastreamentos comecem a ser enviados à instância do Application Insights.
- Consulte [Rastreamentos do criador de perfil](profiler-overview.md?toc=/azure/azure-monitor/toc.json) no portal do Azure.
- Para obter ajuda para a solução de problemas do Profiler, confira [Solução de problemas do Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

[azureresourceexplorer]: ./media/profiler-vm/azure-resource-explorer.png
[resourceexplorerput]: ./media/profiler-vm/resource-explorer-put.png
[resourceexplorersinksconfig]: ./media/profiler-vm/resource-explorer-sinks-config.png
[wadextension]: ./media/profiler-vm/wad-extension.png
