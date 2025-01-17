---
title: Mover as VMs IaaS do Azure para outra região do Azure como VMs fixas em zonas usando o serviço Azure Site Recovery | Microsoft Docs
description: Use o Azure Site Recovery para mover as VMs IaaS do Azure para outra região do Azure como VMs fixas em zonas.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 7562d720b200e127fbfd56c403f0e29e28b3b5d8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65793752"
---
# <a name="move-azure-vms-into-availability-zones"></a>Mover as VMs do Azure para Zonas de Disponibilidade
As Zonas de Disponibilidade do Azure ajudam a proteger seus aplicativos e seus dados contra falhas do datacenter. Cada Zona de disponibilidade é composta por um ou mais datacenters equipados com energia, resfriamento e rede independentes. Para garantir a resiliência, há um mínimo de três zonas separadas em todas as regiões habilitadas. A separação física das Zonas de Disponibilidade dentro de uma região ajuda a proteger os aplicativos e os dados contra falhas do datacenter. Com as Zonas de Disponibilidade, o Azure oferece um SLA (contrato de nível de serviço) de 99,99% de tempo de atividade de VMs (máquinas virtuais). As Zonas de Disponibilidade são compatíveis com regiões selecionadas, conforme mencionado em [O que são Zonas de Disponibilidade no Azure?](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones).

Em um cenário em que você implantou as VMs como *instância única* em uma região específica e deseja melhorar a disponibilidade movendo-as para uma Zona de Disponibilidade, faça isso usando o Azure Site Recovery. Essa ação pode ser categorizada mais detalhadamente em:

- Mover as VMs de instância única para Zonas de Disponibilidade em uma região de destino
- Mover as VMs em um conjunto de disponibilidade para Zonas de Disponibilidade em uma região de destino

> [!IMPORTANT]
> Atualmente, o Azure Site Recovery dá suporte à movimentação de VMs de uma região para outra, mas não dá suporte à movimentação dentro de uma região.

## <a name="check-prerequisites"></a>Verificar pré-requisitos

- Verifique se a região de destino é [compatível com Zonas de Disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones). Verifique se sua opção de [combinação de região de destino/região de origem é compatível](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support). Tome uma decisão informada na região de destino.
- Verifique se você entende os [componentes e a arquitetura do cenário](azure-to-azure-architecture.md).
- Examine os [requisitos e limitações com suporte](azure-to-azure-support-matrix.md).
- Verifique as permissões de conta. Se acabou de criar sua conta gratuita do Azure, você é o administrador da assinatura. Se você não for o administrador da assinatura, peça para o administrador atribuir as permissões necessárias. Para habilitar a replicação em uma VM e, mais tarde, copiar os dados para o destino usando o Azure Site Recovery, é necessário ter:

    1. Permissões para criar uma VM em recursos do Azure. A função interna *Colaborador da Máquina Virtual* tem essas permissões, que incluem:
        - Permissão para criar uma VM no grupo de recursos selecionado
        - Permissão para criar uma VM na rede virtual selecionada
        - Permissão para gravar na conta de armazenamento selecionada

    2. Permissão para gerenciar tarefas do Azure Site Recovery. A função *Colaborador do Site Recovery* tem todas as permissões necessárias para gerenciar ações do Site Recovery em um cofre dos Serviços de Recuperação.

## <a name="prepare-the-source-vms"></a>Preparar as VMs de origem

1. As VMs deverão usar discos gerenciados caso você deseje movê-las para uma Zona de Disponibilidade usando o Site Recovery. Você pode converter VMs do Windows existentes que usam discos não gerenciados para que passem a usar discos gerenciados. Siga as etapas em [Converter uma máquina virtual do Windows de discos não gerenciados em discos gerenciados](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks). Verifique se o conjunto de disponibilidade está configurado como *gerenciado*.
2. Verifique se todos os certificados raiz mais recentes estão presentes nas VMs do Azure que deseja mover. Se os certificados raiz mais recentes não estiverem presentes, a cópia de dados para a região de destino não poderá ser habilitada devido a restrições de segurança.

3. Para VMs Windows, instale todas as atualizações do Windows mais recentes na VM para que todos os certificados raiz confiáveis estejam no computador. Em um ambiente desconectado, siga os processos padrão de atualização e de atualização de certificado do Windows para sua organização.

4. Para VMs Linux, siga as diretrizes fornecidas pelo seu distribuidor Linux para obter os certificados raiz confiáveis mais recentes e a lista de certificados revogados na VM.
5. Verifique se você não está usando um proxy de autenticação para controlar a conectividade de rede das VMs que deseja mover.

6. Se a VM que você está tentando mover não tiver acesso à Internet e estiver usando um proxy de firewall para controlar o acesso de saída, verifique os requisitos em [Configurar a conectividade de rede de saída](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms).

7. Identifique o layout da rede de origem e todos os recursos que estão sendo usados no momento – incluindo balanceadores de carga, NSGs e IP público – para a verificação.

## <a name="prepare-the-target-region"></a>Preparar a região de destino

1. Verifique se a sua assinatura do Azure permite criar VMs na região de destino usada para recuperação de desastre. Se necessário, contate o suporte para habilitar a cota necessária.

2. Verifique se a sua assinatura tem recursos suficientes para dar suporte a VMs com tamanhos que correspondem às VMs de origem. Se você usar o Site Recovery para copiar os dados para o destino, ele escolherá o mesmo tamanho ou o tamanho mais próximo possível para a VM de destino.

3. Crie um recurso de destino para cada componente identificado no layout da rede de origem. Essa ação assegura que, após você realizar a substituição para a região de destino, as VMs tenham todos os recursos e as funcionalidades que tinham na origem.

    > [!NOTE]
    > Quando você habilita a replicação para a VM de origem, o Azure Site Recovery automaticamente descobre e cria uma conta de armazenamento e rede virtual. Você também pode pré-criar esses recursos e atribuir à VM como parte da etapa de habilitação da replicação. Porém, para quaisquer outros recursos, conforme mencionado posteriormente, será necessário criá-los manualmente na região de destino.

     Os documentos a seguir informam como criar os recursos de rede mais comumente usados que são relevantes para você, com base na configuração da VM de origem.

    - [Grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Balanceadores de carga](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [IP público](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Para outros componentes de rede, veja a [documentação](https://docs.microsoft.com/azure/#pivot=products&panel=network) da rede.

    > [!IMPORTANT]
    > Use um balanceador de carga com redundância de zona no destino. Você pode ler mais sobre isso em [Standard Load Balancer e Zonas de Disponibilidade](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).

4. [Crie uma rede de não produção](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) manualmente na região de destino caso deseje testar a configuração antes de realizar a substituição para a região de destino. Recomendamos essa abordagem porque ela resulta em uma interferência mínima no ambiente de produção.

## <a name="enable-replication"></a>Habilitar a replicação
As etapas a seguir orientarão você pelo uso do Azure Site Recovery para habilitar a replicação de dados para a região de destino antes que, mais tarde, eles sejam movidos para as Zonas de Disponibilidade.

> [!NOTE]
> Essas etapas são para uma única VM. Você pode estender o mesmo para várias VMs. Vá para o cofre dos Serviços de Recuperação, selecione **+ Replicar** e selecione as VMs relevantes juntas.

1. No portal do Azure, selecione **Máquinas virtuais** e selecione a VM que deseja mover para as Zonas de Disponibilidade.
2. Em **Operações**, clique em **Recuperação de desastre**.
3. Em **Configurar a recuperação de desastre** > **Região de destino**, selecione a região de destino para a qual você replicará. Garanta que essa região [dê suporte](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) às Zonas de Disponibilidade.

    ![Seleção de região de destino](media/azure-vms-to-zones/enable-rep-1.PNG)

4. Selecione **Avançar: Configurações avançadas**.
5. Escolha os valores apropriados para a assinatura de destino, bem como para o grupo de recursos e a rede virtual da VM de destino.
6. Na seção **Disponibilidade**, escolha a Zona de Disponibilidade para a qual deseja mover a VM. 
   > [!NOTE]
   > Se a opção de conjunto de disponibilidade ou Zona de Disponibilidade não for exibida, garanta que os [pré-requisitos](#prepare-the-source-vms) sejam atendidos e que a [preparação](#prepare-the-source-vms) das VMs de origem seja concluída.
  
    ![Seleções para escolher uma Zona de Disponibilidade](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Selecione **Habilitar Replicação**. Essa ação inicia um trabalho para habilitar a replicação para a VM.

## <a name="check-settings"></a>Verificar as configurações

Após o trabalho de replicação, você poderá verificar o status de replicação, modificar as configurações de replicação e testar a implantação.

1. No menu da VM, selecione **Recuperação de desastre**.
2. Você pode verificar a integridade da replicação, os pontos de recuperação que foram criados e as regiões de origem e destino no mapa.

   ![Status de replicação](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Testar a configuração

1. No menu da máquina virtual, clique em **Recuperação de Desastre**.
2. Selecione o ícone **Failover de Teste**.
3. Em **Failover de Teste**, selecione um ponto de recuperação para usar no failover:

   - **Mais recente processado**: Faz failover da VM para o ponto de recuperação único que foi processado pelo serviço do Site Recovery. A carimbo de data/hora é mostrado. Com essa opção, nenhum tempo é gasto no processamento de dados, portanto, ele fornece um RTO (Objetivo do Tempo de Recuperação) baixo.
   - **Consistente com o aplicativo mais recente**: Essa opção falha em todas as VMs para o ponto de recuperação consistente com o aplicativo mais recente. A carimbo de data/hora é mostrado.
   - **Personalizado**: Selecione qualquer ponto de recuperação.

3. Selecione a rede virtual de destino de teste do Azure para a qual deseja mover as VMs do Azure para testar a configuração. 

    > [!IMPORTANT]
    > Recomendamos que você use uma rede de VMs do Azure separada para a falha de teste, não a rede de produção na região de destino para a qual deseja mover as VMs.

4. Para começar a testar a movimentação, selecione **OK**. Para acompanhar o progresso, selecione a VM para abrir suas propriedades. Ou você pode selecionar o trabalho **Failover de Teste** no nome do cofre > **Configurações** > **Trabalhos** > **Trabalhos do Site Recovery**.
5. Após a conclusão do failover, a réplica da VM do Azure aparece no portal do Azure > **Máquinas Virtuais**. Verifique se a VM está em execução, tem o tamanho apropriado e está conectada à rede apropriada.
6. Caso deseje excluir a VM criada como parte do teste da movimentação, selecione **Limpar failover de teste** no item replicado. Em **Observações**, registre e salve as observações associadas ao teste.

## <a name="move-to-the-target-region-and-confirm"></a>Mova-a para a região de destino e confirme

1.  No menu da máquina virtual, clique em **Recuperação de Desastre**.
2. Selecione o ícone **Failover**.
3. Em **Failover**, selecione **Mais recente**. 
4. Selecione **Desligar o computador antes do início do failover**. O Site Recovery tenta desligar a VM de origem antes de acionar o failover. O failover continuará mesmo o desligamento falhar. Você pode acompanhar o progresso do failover na página **Trabalhos** . 
5. Depois que o trabalho for concluído, verifique se a VM é exibida na região de destino do Azure conforme esperado.
6. Em **Itens replicados**, clique com o botão direito do mouse em VM > **Confirmar**. Isso concluirá o processo de movimentação para a região de destino. Aguarde até o trabalho de confirmação ser concluído.

## <a name="discard-the-resource-in-the-source-region"></a>Descartar o recurso na região de origem

Vá até a VM. Selecione **Desabilitar a Replicação**. Essa ação interrompe o processo de cópia dos dados para a VM.  

> [!IMPORTANT]
> Execute a etapa anterior para evitar ser cobrado por replicação do Site Recovery após a movimentação. As configurações de replicação de origem são limpas automaticamente. Observe que a extensão de Site Recovery instalada como parte da replicação não é removida e precisa ser removida manualmente.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aumentou a disponibilidade de uma VM do Azure movendo-a para um conjunto de disponibilidade ou uma Zona de Disponibilidade. Agora é possível definir a recuperação de desastres para a VM do Azure movida.

> [!div class="nextstepaction"]
> [Configurar a recuperação de desastres após a migração](azure-to-azure-quickstart.md)


