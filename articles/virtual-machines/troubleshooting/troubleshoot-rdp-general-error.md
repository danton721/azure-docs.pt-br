---
title: Solucionar um problema de erro geral de protocolo RDP em uma VM Windows no Azure | Microsoft Docs
description: Aprenda a solucionar um problema de erro geral de protocolo RDP em uma VM Windows no Azure | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 7f5e1f2141a58f666367d253d5fc313499e64c9f
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239383"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Solucionar um problema de erro geral de protocolo RDP em uma VM do Azure

Este artigo descreve um erro geral que você pode encontrar ao fazer uma conexão de protocolo RDP para uma VM (Máquina Virtual) Windows no Azure.

## <a name="symptom"></a>Sintoma

Quando você faz uma conexão de protocolo RDP em uma VM Windows no Azure, você pode receber a seguinte mensagem de erro geral:

**A Área de Trabalho Remota não pode se conectar ao computador remoto por um dos seguintes motivos:**

1. **O acesso remoto ao servidor não está habilitado**

2. **O computador remoto está desligado**

3. **O computador remoto não está disponível na rede**

**Verifique se o computador remoto está ligado e conectado à rede e que o acesso remoto está habilitado.**

## <a name="cause"></a>Causa

Esse problema pode ocorrer devido às causas a seguir:

### <a name="cause-1"></a>Causa 1

O componente RDP está desabilitado da seguinte maneira:

- No nível do componente
- No nível do ouvinte
- No servidor Host da Sessão da Área de Trabalho Remota
- Na função de Host da Sessão da Área de Trabalho Remota

### <a name="cause-2"></a>Causa 2

Os Serviços de Área de Trabalho Remota (TermService) não estão em execução.

### <a name="cause-3"></a>Causa 3

O ouvinte RDP está configurado incorretamente.

## <a name="solution"></a>Solução

Para resolver esse problema, [faça backup do disco do sistema operacional](../windows/snapshot-copy-managed-disk.md) e [anexe o disco do sistema operacional a uma VM de resgate](troubleshoot-recovery-disks-portal-windows.md), depois siga as etapas.

### <a name="serial-console"></a>Console serial

#### <a name="step-1-turn-on-remote-deskop"></a>Etapa 1: ativar a Área de Trabalho Remota

1. Acesso ao [Console Serial](serial-console-windows.md) selecionando **Suporte e solução de problemas** > **Console serial (visualização)**. Se o recurso estiver habilitado na VM, você poderá conectar a VM com êxito.

2. Crie um novo canal para uma instância de CMD. Digite **CMD** para iniciar o canal e obter o nome do canal.

3. Alterne para o canal que estiver executando a instância de CMD, nesse caso, deve ser o canal 1.

   ```
   ch -si 1
   ```
4. Verifique os valores das chaves do Registro da seguinte maneira:

   1. Verifique se o componente RDP está habilitado.

      ```
      REM Get the local policy
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      Se a política de domínio existir, a configuração na política local será substituída.

         - Se a política de domínio declara que o RDP está desabilitado (1), atualize a política do AD por meio do controlador de domínio.
         - Se a política de domínio indica que o RDP está habilitado (0), nenhuma atualização é necessária.

      Se a política de domínio não existe e a política local declara que o RDP está desabilitado (1), habilite o RDP usando o seguinte comando:

         ```
         reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
         ```

   2. Verifique a configuração atual do servidor Host da Sessão da Área de Trabalho Remota.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

   3. Se o comando retornar 0, o servidor Host da Sessão da Área de Trabalho Remota estará desabilitado. Em seguida, habilite o servidor Host da Sessão da Área de Trabalho Remota da seguinte maneira:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

   4. O módulo de Servidor de Terminal será configurado para o modo de drenagem se o servidor estiver em um farm de servidores de terminal (RDS ou Citrix). Verifique o modo atual do módulo do servidor Host da Sessão da Área de Trabalho Remota.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

   5. Se o comando retornar 1, o módulo de servidor Host da Sessão da Área de Trabalho Remota será definido para o modo de drenagem. Em seguida, defina o módulo para o modo de funcionamento, da seguinte maneira:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

   6. Verifique se você pode se conectar ao servidor Host da Sessão da Área de Trabalho Remota.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

   7. Se o comando retornar 1, você não poderá se conectar ao servidor Host da Sessão da Área de Trabalho Remota. Em seguida, habilite a conexão da seguinte maneira:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```

   8. Verifique a configuração atual do ouvinte RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

   9. Se o comando retornar 0, o ouvinte RDP estará desabilitado. Em seguida, habilite o ouvinte da seguinte maneira:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

   10. Verifique se você pode se conectar ao ouvinte RDP.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   11. Se o comando retornar 1, você não poderá se conectar ao ouvinte RDP. Em seguida, habilite a conexão da seguinte maneira:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

6. Reinicie a VM.

7. Saia da instância do CMD digitando `exit` e, em seguida, pressione **Enter** duas vezes.

8. Reinicie a VM digitando `restart`.

Se o problema ainda ocorrer, passe para a etapa 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Etapa 2: habilitar Serviços de Área de Trabalho Remota

Para obter mais informações, confira [Os Serviços de Área de Trabalho Remota não estão iniciando em uma VM do Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Etapa 3: redefinir o ouvinte RDP

Para obter mais informações, confira [A Área de Trabalho Remota se desconecta com frequência na VM do Azure](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Reparo offline

#### <a name="step-1-turn-on-remote-deskop"></a>Etapa 1: ativar a Área de Trabalho Remota

1. [Anexar o disco de SO a uma VM de recuperação](../windows/troubleshoot-recovery-disks-portal.md).
2. Iniciar uma Conexão de Área de Trabalho Remota à VM de recuperação.
3. Verifique se o disco está sinalizado como **Online** no console de Gerenciamento de Disco. Observe a letra da unidade atribuída ao disco do sistema operacional anexado.
3. Iniciar uma Conexão de Área de Trabalho Remota à VM de recuperação.
4. Abra uma sessão de prompt de comandos com privilégios elevados (**Executar como administrador**). Execute os scripts a seguir. Nesse script, presumimos que a letra da unidade atribuída ao disco do SO anexado é F. Substitua essa letra da unidade pelo valor apropriado para a VM.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

3. Se a VM estiver ingressada no domínio, verifique a seguinte chave do Registro para ver se há uma política de grupo que desabilitará o RDP. 

   ```
   HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
   ```


      Se esse valor de chave está definido como 1, isso significa que o RDP está desabilitado pela política. Para habilitar a Área de Trabalho Remota por meio da política de GPO, altere a seguinte política por meio do controlador de domínio:

   ```
   Computer Configuration\Policies\Administrative Templates: Policy definitions\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections\Allow users to connect remotely by using Remote Desktop Services
   ```

4. Desanexe o disco fixo da VM de resgate.
5. [Crie uma nova VM do disco](../windows/create-vm-specialized.md).

Se o problema ainda ocorrer, passe para a etapa 2.

#### <a name="step-2-enable-remote-desktop-services"></a>Etapa 2: habilitar Serviços de Área de Trabalho Remota

Para obter mais informações, confira [Os Serviços de Área de Trabalho Remota não estão iniciando em uma VM do Azure](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>Etapa 3: redefinir o ouvinte RDP

Para obter mais informações, confira [A Área de Trabalho Remota se desconecta com frequência na VM do Azure](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contate o suporte

Se ainda tiver dúvidas, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.