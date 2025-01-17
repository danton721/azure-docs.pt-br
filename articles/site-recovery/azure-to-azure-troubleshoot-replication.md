---
title: Solução de problemas do Azure Site Recovery para problemas de replicação do Azure para Azure em andamento | Microsoft Docs
description: Solucionando erros e problemas durante a replicação de máquinas virtuais do Azure para recuperação de desastre
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: bf24b2d1395e128dc73361670ea93ac938574146
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258779"
---
# <a name="troubleshoot-ongoing-problems-in-azure-to-azure-vm-replication"></a>Solucionar problemas em andamento na replicação de VM do Azure para Azure

Este artigo descreve problemas comuns no Azure Site Recovery quando você está replicando e recuperando máquinas virtuais do Azure de uma região para outra. Ele também explica como solucioná-los. Para obter mais informações sobre configurações com suporte, consulte a [matriz de suporte para replicar máquinas virtuais do Azure](site-recovery-support-matrix-azure-to-azure.md).

O Azure Site Recovery replica consistentemente os dados da região de origem para a região de recuperação de desastre e cria um ponto de recuperação consistente de falha a cada 5 minutos. Se o Site Recovery não puder criar pontos de recuperação durante 60 minutos, ele alertará você com estas informações:

Mensagem de erro: “Nenhum ponto de recuperação com falha consistente disponível para a VM nos últimos 60 minutos.”</br>
ID do erro: 153007 </br>

As seções a seguir descrevem as causas e soluções.

## <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Alta taxa de alteração de dados na máquina virtual de origem
O Azure Site Recovery aciona um evento se a taxa de alteração de dados na máquina virtual de origem for maior que os limites com suporte. Para verificar se o problema é devido à alta rotatividade, vá para **Itens replicados** > **VM** > **Eventos – Últimas 72 horas**.
Você deve ver o evento “Taxa de alteração de dados além dos limites com suporte”:

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Se você selecionar o evento, deverá ver as informações exatas do disco:

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


### <a name="azure-site-recovery-limits"></a>Limites da Azure Site Recovery
A tabela a seguir fornece os limites do Azure Site Recovery. Esses limites baseiam-se nos nossos testes, mas não podem abranger todas as combinações possíveis de E/S de aplicativos. Os resultados reais podem variar dependendo da combinação de E/S do aplicativo. 

Há dois limites a serem considerados, rotatividade dos dados por disco e rotatividade dos dados por máquina virtual. Por exemplo, vamos examinar o disco Premium P20 na tabela a seguir. O Site Recovery pode lidar com 5 MB/s de rotatividade por disco com, no máximo, cinco desses discos por VM, devido ao limite de 25 MB/s de rotatividade total por VM.

**Destino de armazenamento de replicação** | **Tamanho médio da E/S do disco de origem** |**Rotatividade média dos dados para o disco de origem** | **Rotatividade total dos dados por dia para o disco de dados de origem**
---|---|---|---
Armazenamento Standard | 8 KB | 2 MB/s | 168 GB por disco
Disco Premium P10 ou P15 | 8 KB  | 2 MB/s | 168 GB por disco
Disco Premium P10 ou P15 | 16 KB | 4 MB/s |  336 GB por disco
Disco Premium P10 ou P15 | 32 KB ou maior | 8 MB/s | 672 GB por disco
Disco Premium P20 ou P30 ou P40 ou P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20 ou P30 ou P40 ou P50 | 16 KB ou maior |10 MB/s | 842 GB por disco

### <a name="solution"></a>Solução
O Azure Site Recovery tem limites de taxa de alteração de dados com base no tipo de disco. Para saber se esse problema é recorrente ou momentâneo, localize a taxa de alteração de dados da máquina virtual afetada. Acesse a máquina virtual de origem, localize as métricas em **Monitoramento** e adicione-as conforme mostrado nesta captura de tela:

![Processo de três etapas para localizar a taxa de alteração de dados](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Selecione **Adicionar métrica** e adicione **Bytes de gravação de disco do sistema operacional/s** e **Bytes de gravação de disco de dados/s**.
2. Monitore o pico, conforme mostrado na captura de tela.
3. Exiba as operações de gravação totais que estão acontecendo nos discos do sistema operacional e em todos os discos de dados combinados. Essas métricas talvez não forneçam informações por disco, mas elas indicam o padrão total da rotatividade de dados.

Se um pico for decorrente de uma intermitência de dados ocasional e a taxa de alteração de dados for superior a 10 MB/s (para Premium) e 2 MB/s (para Standard) por algum tempo e cair, a replicação será alcançada. No entanto, se a rotatividade estiver além do limite com suporte na maioria das vezes, considere uma das opções abaixo se possível:

* **Exclua o disco que está causando uma alta taxa de alteração de dados**: Você pode excluir o disco usando [PowerShell](./azure-to-azure-exclude-disks.md). Para excluir o disco que você precisa desabilitar a replicação pela primeira vez. 
* **Altere a camada do disco de armazenamento da recuperação de desastre**: essa opção será possível apenas se a rotatividade do disco de dados for menor que 10 MB/s. Digamos que uma VM com um disco P10 está tendo uma rotatividade de dados superior a 8 MB/s, mas inferior a 10 MB/s. Se o cliente puder usar um disco P30 para o armazenamento de destino durante a proteção, o problema poderá ser resolvido.

## <a name="Network-connectivity-problem"></a>Problemas de conectividade de rede

### <a name="network-latency-to-a-cache-storage-account"></a>Latência de rede para uma conta de armazenamento em cache
O Site Recovery envia dados replicados à conta de armazenamento em cache. Talvez você veja a latência de rede se o carregamento dos dados de uma máquina virtual para a conta de armazenamento em cache for mais lento do que 4 MB em 3 segundos. 

Para verificar se há um problema relacionado à latência, use [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) para carregar os dados da máquina virtual para a conta de armazenamento em cache. Se a latência for alta, verifique se você está usando um NVA (dispositivo virtual de rede) para controlar o tráfego de rede de saída das VMs. O appliance pode ser acelerado se todo o tráfego de replicação passar pelo NVA. 

Recomendamos criar um ponto de extremidade de serviço de rede em sua rede virtual para "Armazenamento", para que o tráfego da replicação não acesse a NVA. Para saber mais, confira [Network virtual appliance configuration](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration) (Configuração da solução de virtualização de rede).

### <a name="network-connectivity"></a>Conectividade de rede
Para replicação de recuperação de Site para o trabalho, conectividade de saída para intervalos específicos de IP ou URLs é necessária da VM. Se a VM estiver atrás de um firewall ou usar regras NSG (grupo de segurança de rede) para controlar a conectividade de saída, você poderá enfrentar um desses problemas. Para garantir que todas as URLs estão conectadas, confira [Conectividade de saída para URLs do Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges). 

## <a name="error-id-153006---no-app-consistent-recovery-point-available-for-the-vm-in-the-last-xxx-minutes"></a>ID do erro 153006 - nenhum ponto de recuperação consistente do aplicativo disponível para a VM nos últimos minutos 'XXX'

Alguns dos problemas mais comuns estão listados abaixo

#### <a name="cause-1-known-issue-in-sql-server-20082008-r2"></a>Causa 1: Problema conhecido no SQL server 2008/2008 R2 
**Como corrigir** : Há um problema conhecido com o SQL server 2008/2008 R2. Consulte este artigo do KB [agente de recuperação de Site do Azure ou outro VSS não componente de backup falhar para um servidor que hospeda o SQL Server 2008 R2](https://support.microsoft.com/help/4504103/non-component-vss-backup-fails-for-server-hosting-sql-server-2008-r2)

#### <a name="cause-2-azure-site-recovery-jobs-fail-on-servers-hosting-any-version-of-sql-server-instances-with-autoclose-dbs"></a>Causa 2: Falham de trabalhos do Azure Site Recovery nos servidores que hospedam qualquer versão das instâncias do SQL Server com bancos de dados AUTO_CLOSE 
**Como corrigir** : Consulte o Kb [artigo](https://support.microsoft.com/help/4504104/non-component-vss-backups-such-as-azure-site-recovery-jobs-fail-on-ser) 


#### <a name="cause-3-known-issue-in-sql-server-2016-and-2017"></a>Causa 3: Problema conhecido no SQL Server 2016 e 2017
**Como corrigir** : Consulte o Kb [artigo](https://support.microsoft.com/help/4493364/fix-error-occurs-when-you-back-up-a-virtual-machine-with-non-component) 

#### <a name="cause-4-you-are-using-storage-spaces-direct-configuration"></a>Causa 4: Você está usando a configuração de espaços de armazenamento diretos
**Como corrigir** : O Azure Site Recovery não é possível criar o ponto de recuperação consistente de aplicativo para a configuração de espaços de armazenamento diretos. Consulte o artigo para corretamente [configurar a política de replicação](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-enable-replication-s2d-vms)

### <a name="more-causes-due-to-vss-related-issues"></a>Problemas relacionados ao mais causas devido ao VSS:

Para solucionar outros problemas, verifique os arquivos no computador de origem para obter o código de erro exato da falha:
    
    C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\Application Data\ApplicationPolicyLogs\vacp.log

Como localizar os erros no arquivo?
Pesquisar a cadeia de caracteres "vacpError" abrindo o arquivo vacp.log em um editor
        
    Ex: vacpError:220#Following disks are in FilteringStopped state [\\.\PHYSICALDRIVE1=5, ]#220|^|224#FAILED: CheckWriterStatus().#2147754994|^|226#FAILED to revoke tags.FAILED: CheckWriterStatus().#2147754994|^|

No exemplo acima **2147754994** é o código de erro que informa você sobre a falha, conforme mostrado abaixo

#### <a name="vss-writer-is-not-installed---error-2147221164"></a>O gravador VSS não está instalado - erro 2147221164 

*Como corrigir*: Para gerar a marca de consistência do aplicativo, o Azure Site Recovery usa a cópia de sombra de Volume da Microsoft Service (VSS). Ele instala um provedor de VSS para sua operação para tirar instantâneos de consistência do aplicativo. Esse provedor de VSS é instalado como um serviço. No caso do serviço de provedor do VSS não está instalado, a criação de instantâneo de consistência do aplicativo falhará com a id de erro 0x80040154 "Classe não registrada". </br>
Consulte [artigo para solucionar problemas do VSS writer instalação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-troubleshoot-push-install#vss-installation-failures) 

#### <a name="vss-writer-is-disabled---error-2147943458"></a>O gravador VSS está desativado - erro 2147943458

**Como corrigir**: Para gerar a marca de consistência do aplicativo, o Azure Site Recovery usa a cópia de sombra de Volume da Microsoft Service (VSS). Ele instala um provedor de VSS para sua operação para tirar instantâneos de consistência do aplicativo. Esse provedor de VSS é instalado como um serviço. No caso do serviço do provedor do VSS está desabilitado, a criação de instantâneo de consistência do aplicativo falhará com a id de erro "o serviço especificado está desabilitado e não pode ser started(0x80070422)". </br>

- Se estiver desabilitado VSS,
    - Verificar se o tipo de inicialização do serviço do provedor do VSS está definido como **automática**.
    - Reinicie os serviços a seguir:
        - Serviço VSS
        - Provedor de VSS do Azure Site Recovery
        - Serviço VDS

####  <a name="vss-provider-notregistered---error-2147754756"></a>NOT_REGISTERED do provedor de VSS - erro 2147754756

**Como corrigir**: Para gerar a marca de consistência do aplicativo, o Azure Site Recovery usa a cópia de sombra de Volume da Microsoft Service (VSS). Verifique se o serviço Azure Site Recovery VSS Provider está instalado ou não. </br>

- Tente novamente a instalação do provedor usando os seguintes comandos:
- Desinstale provedor existente: C:\Program arquivos (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Uninstall.cmd
- Reinstale: C:\Program arquivos (x86) \Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd
 
Verificar se o tipo de inicialização do serviço do provedor do VSS está definido como **automática**.
    - Reinicie os serviços a seguir:
        - Serviço VSS
        - Provedor de VSS do Azure Site Recovery
        - Serviço VDS
