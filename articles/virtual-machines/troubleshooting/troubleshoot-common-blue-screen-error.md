---
title: Erros de tela azul ao inicializar uma VM do Azure| Microsoft Docs
description: Saiba como solucionar o problema que gera um erro de tela azul durante a inicialização | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 00/19/2018
ms.author: genli
ms.openlocfilehash: 05529b1d9397fb14e4a0eece5587023321b955b7
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586849"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>O Windows mostra um erro de tela azul durante a inicialização de uma VM do Azure
Este artigo descreve o erro de tela azul que você pode encontrar ao inicializar uma VM (Máquina Virtual) do Windows no Microsoft Azure. Ele fornece as etapas para ajudar você a coletar dados para um tíquete de suporte. 

> [!NOTE] 
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Gerenciador de Recursos e clássico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo descreve o uso do modelo de implantação do Resource Manager, que recomendamos usar para novas implantações em vez do modelo de implantação clássico.

## <a name="symptom"></a>Sintoma 

Uma VM do Windows não inicia. Ao verificar as capturas de tela da inicialização no [Diagnóstico de inicialização](./boot-diagnostics.md), você verá uma das seguintes mensagens de erro em uma tela azul:

- Seu PC teve um problema e precisa ser reiniciado. Estamos apenas coletando algumas informações sobre o erro e, em seguida, você pode reiniciá-lo.
- Seu PC teve um problema e precisa ser reiniciado.

Esta seção lista as mensagens de erro comuns que você pode encontrar ao gerenciar VMs:

## <a name="cause"></a>Causa

Pode haver vários motivos para você receber um erro com parada. As causas mais comuns são:

- Problema com um driver
- Memória ou arquivo do sistema corrompido
- Um aplicativo acessa a um setor proibido da memória

## <a name="collect-memory-dump-file"></a>Coletar o arquivo de despejo de memória

Para resolver o problema, primeiro você precisaria coletar o arquivo de despejo da falha e entrar em contato com o suporte de posse do arquivo. Para coletar o arquivo de despejo, siga estas etapas:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Anexar o disco de SO a uma VM de recuperação

1. Tire um instantâneo do disco do SO da VM afetada como um backup. Para obter mais informações, consulte [Instantâneo de um disco](../windows/snapshot-copy-managed-disk.md).
2. [Anexar o disco de SO a uma VM de recuperação](../windows/troubleshoot-recovery-disks-portal.md). 
3. Área de trabalho remota para a VM de recuperação.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Localize o arquivo de despejo e envie um tíquete de suporte

1. Na VM de recuperação, vá até a pasta do Windows no disco do SO anexado. Se a letra da unidade atribuída ao disco do SO anexado for F, você precisará ir até F:\Windows.
2. Localize o arquivo memory.dmp e, em seguida, [envie um tíquete de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) com o arquivo de despejo. 

Se não encontrar o arquivo de despejo, passe para a próxima etapa para habilitar o log de despejo e o Console Serial.

### <a name="enable-dump-log-and-serial-console"></a>Habilitar o log de despejo e o Console Serial

Para habilitar o log de despejo e o Console Serial, execute o script a seguir.

1. Abra a sessão de prompt de comandos com privilégios elevados (executar como administrador).
2. Execute o seguinte script:

    Nesse script, presumimos que a letra da unidade atribuída ao disco do SO anexado é F. Substitua a letra pelo valor apropriado em sua VM.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

    1. Certifique-se de que haja espaço suficiente no disco para alocar a mesma quantidade de memória que a RAM, o que depende do tamanho que você está selecionando para essa VM.
    2. Se não houver espaço suficiente ou se a VM for grande (série G, GS ou E), você poderá alterar o local em que o arquivo será criado e fazer a referência para qualquer outro disco de dados anexado à VM. Para fazer isso, você precisará alterar a seguinte chave:

            reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

            REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
            REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

            reg unload HKLM\BROKENSYSTEM

3. [Desanexe o disco do SO e, em seguida, anexe-o novamente na VM afetada](../windows/troubleshoot-recovery-disks-portal.md).
4. Inicie a VM para reproduzir o problema e, em seguida, um arquivo de despejo será gerado.
5. Anexe o disco do SO a uma VM de recuperação, colete o arquivo de despejo e, em seguida [envie um tíquete de suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) com o arquivo de despejo.


