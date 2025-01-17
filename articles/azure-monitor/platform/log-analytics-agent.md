---
title: Colete dados de log com o agente do Azure Log Analytics | Microsoft Docs
description: Este tópico ajuda o reconhecimento de como coletar dados e monitorar computadores hospedados no Azure, no local ou em outro ambiente de nuvem com o Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: magoedte
ms.openlocfilehash: 436685f3bba58ed7d06dfe834d808e7fe422176b
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751987"
---
# <a name="collect-log-data-with-the-azure-log-analytics-agent"></a>Colete dados de log com o agente do Log Analytics do Azure

O agente do Azure Log Analytics, anteriormente conhecido como agente do Microsoft Monitoring Agent (MMA) ou OMS Linux, foi desenvolvido para gerenciamento abrangente em máquinas locais, computadores monitorados pelo [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/), e máquinas virtuais em qualquer nuvem. Os agentes do Windows e Linux anexar a um Monitor do Azure e armazenam os dados de log coletados de fontes diferentes em seu espaço de trabalho do Log Analytics, bem como qualquer exclusivos logs ou métricas, conforme definido em uma solução de monitoramento. 

Este artigo fornece uma visão geral detalhada do agente, dos requisitos do sistema e da rede e dos diferentes métodos de implantação.   

## <a name="overview"></a>Visão geral

![Diagrama de comunicação do agente do Log Analytics](./media/log-analytics-agent/log-analytics-agent-01.png)

Antes de analisar e agir sobre dados coletados, você precisará primeiro instalar e conectar agentes para todas as máquinas que você deseja enviar dados para o serviço do Azure Monitor. Você pode instalar agentes em suas VMs do Azure usando a extensão VM do Log do Azure para Windows e Linux e para computadores em um ambiente híbrido usando a instalação, a linha de comando ou o DSC (Configuração de Estado Desejado) na Automação do Azure. 

O agente para Linux e Windows se comunica na saída para o serviço do Monitor do Azure pela porta TCP 443 e se o computador se conecta por meio de um firewall ou servidor proxy para se comunicar pela Internet, examine os requisitos abaixo para entender a configuração de rede Necessário. Se suas políticas de segurança não permitir que os computadores na rede para se conectar à Internet, você pode configurar uma [gateway do Log Analytics](gateway.md) e, em seguida, configure o agente para se conectar por meio do gateway nos logs do Azure Monitor. O agente, em seguida, pode receber informações de configuração e enviar os dados coletados dependendo de quais dados as regras de coleta e soluções de monitoramento foi habilitado no seu espaço de trabalho. 

Se você estiver monitorando um computador com o System Center Operations Manager 2012 R2 ou posterior, poderá ser multihomed com o serviço do Azure Monitor para coletar dados e encaminhe para o serviço e ainda ser monitorado pelo [Operations Manager](../../azure-monitor/platform/om-agents.md). Com computadores Linux, o agente não inclui um componente de serviço de integridade, como faz o agente do Windows e informações são coletadas e processa por um servidor de gerenciamento em seu nome. Porque os computadores Linux são monitorados de forma diferente com o Operations Manager, eles não recebem a configuração ou coletar dados diretamente e encaminhar por meio do grupo de gerenciamento, como um sistema gerenciado por agente do Windows faz. Como resultado, esse cenário não tem suporte com computadores Linux reportam ao Operations Manager.  

O agente do Windows pode relatar até quatro áreas de trabalho do Log Analytics, enquanto o agente do Linux suporta apenas o relatório para um único espaço de trabalho.  

O agente para Linux e Windows não é apenas para se conectar ao Azure Monitor, ele também dá suporte a automação do Azure para hospedar a função Hybrid Runbook worker e outros serviços, como [Change Tracking](../../automation/change-tracking.md) e [degerenciamentodeatualizações](../../automation/automation-update-management.md). Para obter mais informações sobre a função Hybrid Runbook Worker, consulte [Hybrid Runbook Worker de Automação do Azure](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Sistemas operacionais Windows compatíveis
Há suporte oficial para as seguintes versões do sistema operacional Windows para o agente para Windows:

* Windows Server 2019
* Windows Server 2008 R2, 2012, 2012 R2, 2016, versão 1709 e 1803
* Windows 7 SP1 e posterior

## <a name="supported-linux-operating-systems"></a>Sistemas operacionais Linux com suporte
Esta seção fornece detalhes sobre as distribuições de Linux com suporte.    

Começando com versões lançadas depois de agosto de 2018, estamos fazendo as seguintes alterações ao nosso modelo de suporte:  

* Somente o servidor versões têm suporte, um não cliente.  
* Novas versões de [distros do Azure Linux Endorsed](../../virtual-machines/linux/endorsed-distros.md) são sempre suportadas.  
* Todas as versões secundárias têm suporte para cada versão principal listada.
* As versões que passaram a data de fim de suporte do fabricante não são suportadas.  
* Não há suporte para novas versões do AMI.  
* Apenas versões que executam o SSL 1.x por padrão são suportadas.

>[!NOTE]
>Se você estiver usando uma distribuição ou versão que não é suportada no momento e não se alinha ao nosso modelo de suporte, recomendamos que você distribua esse repositório, reconhecendo que o suporte da Microsoft não fornecerá assistência com as versões do agente bifurcado.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86 x64) e 7 (x64)  
* Oracle Linux 6 e 7 (x86 x64) 
* Red Hat Enterprise Linux Server 6 (x86 x64) e 7 (x64)
* Debian GNU/Linux 8 e 9 (x86 x64)
* Ubuntu 14.04 18.04 LTS (x64), 16.04 LTS (x86 x64) e LTS (x86 x64)
* SUSE Linux Enterprise Server 12 (x64)

>[!NOTE]
>OpenSSL 1.1.0 só tem suporte em plataformas de x86_x64 (64 bits) e OpenSSL mais cedo do que 1. x não tem suporte em qualquer plataforma.
>

### <a name="agent-prerequisites"></a>Pré-requisitos do agente

A tabela a seguir destaca os pacotes necessários para distribuições Linux com suporte que o agente será instalado no.

|Pacote necessário |DESCRIÇÃO |Versão mínima |
|-----------------|------------|----------------|
|Glibc |    Biblioteca GNU C | 2.5-12 
|Openssl    | Bibliotecas OpenSSL | 1.0 ou 1.1 |
|Curl | cliente Web cURL | 7.15.5 |
|Python-ctypes | | 
|PAM | Módulos de autenticação conectáveis | | 

>[!NOTE]
>Rsyslog ou syslog-ng são necessários para coletar mensagens de syslog. O daemon syslog padrão na versão 5 do Red Hat Enterprise Linux, CentOS e na versão Oracle Linux (sysklog) não tem suporte para a coleta de eventos de syslog. Para coletar dados de syslog nessa versão das distribuições, o daemon rsyslog deverá ser instalado e configurado para substituir sysklog.

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2
Para garantir a segurança dos dados em trânsito para os logs do Azure Monitor, recomendamos que você configure o agente para usar pelo menos segurança de camada de transporte (TLS) 1.2. Constatou-se que versões mais antigas do protocolo TLS/protocolo SSL eram vulneráveis e embora elas ainda funcionem no momento para permitir a compatibilidade com versões anteriores, elas **não são recomendadas**.  Para obter mais informações, examine [Enviando dados com segurança usando o TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Requisitos de firewall de rede
As informações abaixo listam as informações de configuração de proxy e firewall necessárias para o agente do Linux e Windows para se comunicar com os logs do Azure Monitor.  

|Recurso de agente|Portas |Direction |Ignorar a inspeção de HTTPS|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Porta 443 |Saída|Sim |  
|*.oms.opinsights.azure.com |Porta 443 |Saída|Sim |  
|*.blob.core.windows.net |Porta 443 |Saída|Sim |  
|*.azure-automation.net |Porta 443 |Saída|Sim |  

Para obter informações de firewall necessárias para o Azure governamental, consulte [gerenciamento do Azure governamental](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Se você planeja usar o Hybrid Runbook Worker da Automação do Azure para conectar e se registrar no serviço de automação para usar runbooks em seu ambiente, é necessário ter acesso ao número da porta e as URLs descritas em [Configurar sua rede para o Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md#network-planning). 

O agente do Windows e Linux dá suporte à comunicação por meio de um servidor proxy ou gateway do Log Analytics para o Azure Monitor usando o protocolo HTTPS.  Há suporte para a autenticação anônima e básica (nome de usuário/senha).  Para o agente do Windows conectado diretamente ao serviço, a configuração do proxy é especificada durante a instalação ou [após a implementação](agent-manage.md#update-proxy-settings) no Painel de Controle ou no PowerShell.  

Para o agente Linux, o servidor proxy pode ser especificado durante a instalação ou modificando o arquivo de configuração proxy.conf [após a instalação](agent-manage.md#update-proxy-settings).  O valor de configuração de proxy do agente do Linux tem a seguinte sintaxe:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Se seu servidor proxy não exigir autenticação, o agente para Linux exigirá mesmo assim fornecendo um pseudo usuário/senha. Isso pode ser qualquer nome de usuário ou senha.

|Propriedade| DESCRIÇÃO |
|--------|-------------|
|Protocol | HTTPS |
|usuário | Nome de usuário opcional para autenticação de proxy |
|password | Senha opcional para autenticação de proxy |
|proxyhost | Endereço ou FQDN do servidor proxy/gateway do Log Analytics |
|port | Número da porta opcional para o servidor proxy/gateway do Log Analytics |

Por exemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Se você usar caracteres especiais, como “\@” em sua senha, você receberá um erro de conexão de proxy porque o valor é analisado incorretamente.  Para contornar esse problema, codifique a senha na URL usando uma ferramenta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalar e configurar o agente 
Conectar máquinas na sua assinatura do Azure ou um ambiente híbrido diretamente com os logs do Azure Monitor pode ser feito usando métodos diferentes, dependendo de suas necessidades. A tabela a seguir realça cada método para determinar o que funciona melhor em sua organização.

|`Source` | Método | DESCRIÇÃO|
|-------|-------------|-------------|
|VM do Azure| - Extensão de VM do Log Analytics para [Windows](../../virtual-machines/extensions/oms-windows.md) ou [Linux](../../virtual-machines/extensions/oms-linux.md) usando a CLI do Azure ou com um modelo do Azure Resource Manager<br>- [Manualmente no portal do Azure](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json). | A extensão instala o agente do Log Analytics nas máquinas virtuais do Azure e as registra em uma área de trabalho do Azure Monitor existente.|
| Computador Windows híbrido|- [Instalação manual](agent-windows.md)<br>- [DSC de Automação do Azure](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Modelo do Resource Manager com o Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instalar o agente Microsoft Monitoring da linha de comando ou usando um método automatizado como DSC de automação do Azure, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), ou com um modelo do Azure Resource Manager, se você implantou o Microsoft Azure Stack no seu datacenter.| 
| Computador Linux híbrido| [Instalação manual](../../azure-monitor/learn/quick-collect-linux-computer.md)|Instale o agente para Linux chamando um script de wrapper hospedado no GitHub. | 
| System Center Operations Manager|[Conectar o Operations Manager ao Log Analytics](../../azure-monitor/platform/om-agents.md) | Configurar a integração entre os logs do Operations Manager e do Azure Monitor para encaminhar os dados coletados de computadores Windows subordinados a um grupo de gerenciamento.|  

## <a name="next-steps"></a>Próximas etapas

* Consultar as [fontes de dados](../../azure-monitor/platform/agent-data-sources.md) para entender as fontes de dados disponíveis para coletar dados do sistema Windows ou Linux. 

* Saiba mais sobre [registrar consultas](../../azure-monitor/log-query/log-query-overview.md) para analisar os dados coletados de fontes de dados e soluções. 

* Conheça as [soluções de monitoramento](../../azure-monitor/insights/solutions.md) que adicionam funcionalidade ao Azure Monitor e também coletam dados no espaço de trabalho do Log Analytics.
