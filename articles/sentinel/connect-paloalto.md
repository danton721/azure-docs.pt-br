---
title: Conectar-se a dados Palo Alto Networks para versão prévia do Azure Sentinel | Microsoft Docs
description: Saiba como se conectar a dados do Palo Alto Networks a Sentinela do Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 40ee73b8cc9b95a4e2030ac38a6c322918dc878e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389088"
---
# <a name="connect-your-palo-alto-networks-appliance"></a>Conectar seu dispositivo Palo Alto Networks

> [!IMPORTANT]
> No momento, o Azure Sentinel está em versão prévia pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Você pode conectar Sentinel do Azure para qualquer dispositivo Palo Alto Networks, salvando os arquivos de log como Syslog CEF. A integração com o Azure Sentinel permite que você execute facilmente análises e consultas entre os dados do arquivo de log da Palo Alto Networks. Para obter mais informações sobre como o Azure Sentinel ingere dados CEF, consulte [appliances conectar CEF](connect-common-event-format.md).

> [!NOTE]
> Os dados serão armazenados na localização geográfica do espaço de trabalho no qual você está executando Sentinel do Azure.

## <a name="step-1-connect-your-palo-alto-networks-appliance-using-an-agent"></a>Etapa 1: Conectar seu dispositivo Palo Alto Networks usando um agente

Para conectar seu dispositivo Palo Alto Networks Sentinel do Azure, você precisa implantar um agente em um computador dedicado (VM ou no local) para dar suporte a comunicação entre o dispositivo e o Azure Sentinel. Você pode implantar o agente manualmente ou automaticamente. A implantação automática só estará disponível se o computador dedicado for uma nova VM que você está criando no Azure. 

Como alternativa, você pode implantar o agente manualmente em uma VM do Azure existente em uma VM em outra nuvem ou em um computador local.

Para ver um diagrama de rede de ambas as opções, consulte [conectar fontes de dados](connect-data-sources.md#agent-options).


### <a name="deploy-the-agent-in-azure"></a>Implantar o agente no Azure

1. No portal do Azure Sentinel, clique em **conectores de dados** e selecione o tipo de dispositivo. 

1. Sob **configuração do agente de Syslog do Linux**:
   - Escolher **implantação automática** se você deseja criar uma nova máquina que é pré-instalado com o agente do Azure Sentinel e inclui todos os a configuração necessária, conforme descrito acima. Selecione **implantação automática** e clique em **implantação automática do agente**. Isso leva você até a página de compra para uma VM dedicada que é conectado automaticamente ao seu espaço de trabalho, é. A VM é uma **v3 de D2s standard (2 vCPUs, 8 GB de memória)** e tem um endereço IP público.
      1. No **implantação personalizada** página, fornecer seus detalhes e escolha um nome de usuário e uma senha e se você concordar com os termos e condições, a VM de compra.
      1. Configure seu dispositivo para enviar logs usando as configurações listadas na página de conexão. Para o conector do formato comum de evento genérico, use estas configurações:
         - Protocolo = UDP
         - Porta = 514
         - Recurso = 4 Local
         - Formato = CEF
   - Escolher **implantação Manual** se você quiser usar uma VM existente como o computador Linux dedicado para o qual o agente do Azure Sentinel deve ser instalado. 
      1. Sob **Baixe e instale o agente do Syslog**, selecione **máquina virtual Linux do Azure**. 
      1. No **máquinas virtuais** tela for aberta, selecione a máquina que você deseja usar e clique em **Connect**.
      1. Na tela de conector, sob **configurar e Syslog de encaminhamento**, defina se o daemon do Syslog está **rsyslog.d** ou **syslog-ng**. 
      1. Copie estes comandos e executá-los em seu dispositivo:
          - Se você selecionou rsyslog.d:
              
            1. Informe o daemon do Syslog para escutar em local_4 de recurso e enviar as mensagens do Syslog para o agente do Azure Sentinel usando a porta 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Baixe e instale o [arquivo de configuração security_events](https://aka.ms/asi-syslog-config-file-linux) que configura o agente de Syslog para escutar na porta 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Onde {0} deve ser substituído pelo seu GUID do espaço de trabalho.
            
            1. Reinicie o daemon do syslog `sudo service rsyslog restart`
             
          - Se você selecionou syslog-ng:

              1. Informe o daemon do Syslog para escutar em local_4 de recurso e enviar as mensagens do Syslog para o agente do Azure Sentinel usando a porta 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Baixe e instale o [arquivo de configuração security_events](https://aka.ms/asi-syslog-config-file-linux) que configura o agente de Syslog para escutar na porta 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Onde {0} deve ser substituído pelo seu GUID do espaço de trabalho.

              3. Reinicie o daemon do syslog `sudo service syslog-ng restart`
      2. Reinicie o agente do Syslog usando este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que há não há erros no log do agente executando este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Implantar o agente em um servidor Linux local

Se você não estiver usando o Azure, implante manualmente o agente de sentinela do Azure para ser executado em um servidor dedicado do Linux.


1. No portal do Azure Sentinel, clique em **conectores de dados** e selecione o tipo de dispositivo.
1. Para criar uma VM do Linux dedicado, sob **configuração do agente de Syslog do Linux** escolher **implantação Manual**.
   1. Sob **Baixe e instale o agente do Syslog**, selecione **computador não Azure Linux**. 
   1. No **agente direto** tela que é aberta, selecione **Agent para Linux** para baixar o agente ou execute este comando para baixá-lo em seu computador Linux:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. Na tela de conector, sob **configurar e Syslog de encaminhamento**, defina se o daemon do Syslog está **rsyslog.d** ou **syslog-ng**. 
      1. Copie estes comandos e executá-los em seu dispositivo:
         - Se você selecionou rsyslog:
           1. Informe o daemon do Syslog para escutar em local_4 de recurso e enviar as mensagens do Syslog para o agente do Azure Sentinel usando a porta 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Baixe e instale o [arquivo de configuração security_events](https://aka.ms/asi-syslog-config-file-linux) que configura o agente de Syslog para escutar na porta 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Onde {0} deve ser substituído pelo seu GUID do espaço de trabalho.
           3. Reinicie o daemon do syslog `sudo service rsyslog restart`
         - Se você selecionou syslog-ng:
            1. Informe o daemon do Syslog para escutar em local_4 de recurso e enviar as mensagens do Syslog para o agente do Azure Sentinel usando a porta 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Baixe e instale o [arquivo de configuração security_events](https://aka.ms/asi-syslog-config-file-linux) que configura o agente de Syslog para escutar na porta 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Onde {0} deve ser substituído pelo seu GUID do espaço de trabalho.
            3. Reinicie o daemon do syslog `sudo service syslog-ng restart`
      1. Reinicie o agente do Syslog usando este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que há não há erros no log do agente executando este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Etapa 2: Encaminhar logs de redes Palo Alto para o agente do Syslog

Configure redes de Palo Alto para encaminhar mensagens de Syslog em formato CEF para seu espaço de trabalho do Azure através do agente do Syslog:
1.  Vá para [guias de configuração do formato de evento comum (CEF)](https://docs.paloaltonetworks.com/resources/cef) e baixe o pdf do seu tipo de dispositivo. Siga todas as instruções no guia de configurar seu dispositivo Palo Alto Networks para coletar eventos CEF. 

1.  Vá para [Syslog configurar monitoramento](https://aka.ms/asi-syslog-paloalto-forwarding) e siga as etapas 2 e 3 para configurar o encaminhamento de evento CEF do seu dispositivo Palo Alto Networks para Sentinel do Azure.

    1. Certifique-se de definir as **formato de servidor do Syslog** à **BSD**.
    1. Verifique se você definiu o **número de instalações** para o mesmo valor definido no agente do Syslog.

> [!NOTE]
> As operações de copiar/colar do PDF podem alterar o texto e inserir caracteres aleatórios. Para evitar isso, copie o texto para um editor e remova os caracteres que podem interromper o formato de log antes de colar, como você pode ver neste exemplo.
 
  ![Problema de cópia do texto CEF](./media/connect-cef/paloalto-text-prob1.png)

6. Para usar o esquema relevante no Log Analytics para os eventos do Palo Alto Networks, pesquise **CommonSecurityLog**.

## <a name="step-3-validate-connectivity"></a>Etapa 3: Validar a conectividade

Pode levar mais de 20 minutos até que seus logs comecem a aparecer no Log Analytics. 

1. Verifique se que você usar o recurso correto. O recurso deve ser o mesmo no seu dispositivo e no Azure Sentinel. Você pode verificar qual arquivo de recurso, você está usando no Azure Sentinel e modificá-lo no arquivo `security-config-omsagent.conf`. 

2. Certifique-se de que os logs estão obtendo à porta à direita no agente do Syslog. Execute este comando no computador do agente do Syslog: `tcpdump -A -ni any  port 514 -vv` Este comando mostra os logs de fluxos do dispositivo para a máquina de Syslog. Certifique-se de que os logs estão sendo recebidos do dispositivo de origem na porta direita e recurso certo.

3. Certifique-se de que os logs que você enviar obedecer [RFC 5424](https://tools.ietf.org/html/rfc542).

4. No computador que executa o agente do Syslog, verifique se essas portas 514, 25226 são abertos e escuta, usando o comando `netstat -a -n:`. Para obter mais informações sobre como usar esse comando, consulte [netstat(8) - página do manual Linux](https://linux.die.net/man/8/netstat). Se ele está escutando corretamente, você verá isso:

   ![Portas de sentinela do Azure](./media/connect-cef/ports.png) 

5. Verifique se que o daemon está definido para escutar na porta 514, no qual você está enviando logs.
    - Para rsyslog:<br>Certifique-se de que o arquivo `/etc/rsyslog.conf` inclui essa configuração:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Para obter mais informações, consulte [imudp: Módulo de entrada de Syslog de UDP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) e [imtcp: Módulo de entrada de Syslog TCP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module)

   - Para syslog-ng:<br>Certifique-se de que o arquivo `/etc/syslog-ng/syslog-ng.conf` inclui essa configuração:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Para obter mais informações, consulte [imudp: Módulo de entrada de Syslog UDP] (para obter mais informações, consulte o [syslog-ng Open Source Edition 3.16 - guia de administração](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Verifique se há comunicação entre o daemon do Syslog e o agente. Execute este comando no computador do agente do Syslog: `tcpdump -A -ni any  port 25226 -vv` Este comando mostra os logs de fluxos do dispositivo para a máquina de Syslog. Certifique-se de que os logs também estão sendo recebidos no agente.

6. Se ambos os comandos fornecidos resultados bem-sucedidos, verifique o Log Analytics para ver se os logs chegam. Todos os eventos que são transmitidos desses dispositivos são exibidos em formato bruto no Log Analytics em `CommonSecurityLog` tipo.

7. Para verificar se há erros ou se os logs não são recebidos, procure no `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Se ele diz que há erros de incompatibilidade de formato de log, vá para `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` e examine o arquivo `security_events.conf`e certifique-se de que seus logs de correspondem o formato de regex que você vê neste arquivo.

8. Certifique-se de que seu tamanho de padrão de mensagem do Syslog é limitado a 2048 bytes (2KB). Se os logs são muito longos, atualize o security_events usando este comando: `message_length_limit 4096`








## <a name="next-steps"></a>Próximas etapas
Neste documento, você aprendeu como conectar dispositivos Palo Alto Networks a Sentinela do Azure. Para saber mais sobre o Azure Sentinel, consulte os seguintes artigos:
- Saiba como [Obtenha visibilidade sobre seus dados e possíveis ameaças](quickstart-get-visibility.md).
- Introdução ao [detecção de ameaças com o Azure Sentinel](tutorial-detect-threats.md).

