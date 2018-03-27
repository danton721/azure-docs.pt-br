---
title: Arquitetura de replicação de servidor físico/VMware no Azure Site Recovery | Microsoft Docs
description: Este artigo fornece uma visão geral dos componentes e da arquitetura usada ao replicar VMs de VMware ou servidores físicos do Windows/Linux locais para um site VMware secundário com o Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 97a990aa3ed9043280888900d8fc7b604b6c22b5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="vmware-vmphysical-server-to-vmware-replication-architecture"></a>Servidor VM VMware/físico para arquitetura de replicação do VMware

Este artigo descreve a arquitetura e os processos utilizados ao fazer a replicação, o failover e a recuperação de VMs (máquinas virtuais) do VMware ou servidores físicos do Windows/Linux para um site VMware secundário utilizando o [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes de arquitetura

**Área** | **Componente** | **Detalhes**
--- | --- | ---
**As tabelas** | Implante este cenário usando o InMage Scout. | Para obter o InMage Scout, você precisa de uma assinatura do Azure.<br/><br/> Depois de criar um cofre de Serviços de Recuperação, baixe o InMage Scout e instale as atualizações mais recentes para configurar a implantação.
**Servidor de processo** | Localizado no site primário | Implante o servidor de processo para manipular o caching, a compactação e a otimização de dados.<br/><br/> Ele também trata da instalação por push do Agente Unificado nas máquinas que você deseja proteger.
**Servidor de configuração** | Localizado no site secundário | O servidor de configuração gerencia, configura e monitora sua implantação, usando o site de gerenciamento ou o console do vContinuum.
**Servidor vContinuum** | Opcional. Instalado no mesmo local que o servidor de configuração. | Ele fornece um console para o gerenciamento e monitoramento de seu ambiente protegido.
**Servidor de destino mestre** | Localizado no site secundário | O servidor de destino mestre armazena os dados replicados. Ele recebe os dados do servidor de processo, cria uma máquina de réplica no site secundário e mantém os pontos de retenção dos dados.<br/><br/> O número de servidores de destino mestre necessário depende do número de máquinas que você está protegendo.<br/><br/> Se você quiser realizar o failback no site primário, também precisará de um servidor de destino mestre nesse local. O Agente Unificado está instalado neste servidor.
**Servidor VMware ESX/ESXi e vCenter** |  VMs são hospedadas em hosts ESX/ESXi. Hosts são gerenciados com um servidor do vCenter | Você precisa de uma infraestrutura do VMware para replicar VMs VMware.
**VMs/servidores físicos** |  Agente Unificado instalado em VMs VMware ou em servidores físicos que você deseja replicar. | O agente atua como um provedor de comunicação entre todos os componentes.

### <a name="replication-process"></a>Processo de replicação

1. Configure os servidores de componente em cada site (configuração, processo, destino mestre) e instale o Agente Unificado nos computadores que você deseja replicar.
2. Após a replicação inicial, os agentes em cada computador enviam as alterações de replicação delta para o servidor de processo.
3. O servidor de processo otimiza os dados e os transfere para o servidor de destino mestre no site secundário. O servidor de configuração gerencia o processo de replicação.

**Figura 6: replicação do VMware para o VMware**

![VMware para VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Próximas etapas

[Configurar](vmware-physical-secondary-disaster-recovery.md) a recuperação de desastres das VMs do VMware e dos servidores físicos em um site secundário.