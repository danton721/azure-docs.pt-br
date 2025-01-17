---
title: Tutorial – Rotear o tráfego para melhorar a resposta do site usando o Gerenciador de Tráfego do Azure
description: Este artigo de tutorial descreve como criar um perfil do Gerenciador de Tráfego para criar aplicativos web altamente disponíveis.
services: traffic-manager
author: asudbring
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: allensu
ms.openlocfilehash: 304beeae02da5836ba88a56d7166fc681e263501
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258351"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Tutorial: Melhorar a resposta de site usando o Gerenciador de Tráfego

Este tutorial descreve como usar o Gerenciador de Tráfego para criar um site da seb altamente responsivo, direcionando o tráfego do usuário para o site com a menor latência. Normalmente, o data center com a menor latência corresponde ao mais próximo em termos de distância geográfica.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Crie duas VMs em execução de um site da Web básica no IIS
> * Crie duas VMs para exibir o Gerenciador de Tráfego em ação de teste
> * Configurar nome DNS para as VMs que executam IIS
> * Crie um perfil do Gerenciador de Tráfego para desempenho aprimorado de site
> * Criar pontos de extremidade no perfil do Gerenciador de Tráfego
> * Ver o Gerenciador de Tráfego em ação

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para ver o Gerenciador de Tráfego em ação, este tutorial exige que você implante o seguinte:

- duas instâncias de sites básicos em execução em diferentes regiões do Azure - **Leste dos EUA** e **Europa Ocidental**.
- duas VMs de teste para testar o Gerenciador de tráfego - uma VM no **Leste dos EUA** e a segunda VM na **Europa Ocidental**. As VMs de teste são usadas para ilustrar como o Gerenciador de Tráfego roteia o tráfego de usuário para o site que está executando na mesma região, pois ele fornece a menor latência.

### <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em https://portal.azure.com.

### <a name="create-websites"></a>Criar sites

Nesta seção, você criará duas instâncias de site que fornecem os dois pontos de extremidade para o perfil do Gerenciador de Tráfego em duas regiões do Azure. Criar dois sites inclui as seguintes etapas:

1. Criar duas VMs para executar um site básico - uma no **Leste dos EUA**e o outro na **Europa Ocidental**.
2. Instale o servidor IIS em cada VM e atualize a página de site padrão que descreve o nome da VM que um usuário está conectado ao visitar o site.

#### <a name="create-vms-for-running-websites"></a>Criar VMs para a execução de sites

Nesta seção, você criará duas VMs *myIISVMEastUS* e *myIISVMWestEurope* no **Leste dos EUA** e **Europa Ocidental** regiões do Azure.

1. No canto superior esquerda do portal do Azure, selecione **criar um recurso** > **computação** > **Windows Server Datacenter de 2019**.
2. Em **Criar uma máquina virtual**, insira ou selecione os seguintes valores na guia **Configurações básicas**:

   - **Assinatura** > **Grupo de Recursos**: Selecione **criar novo** e, em seguida, digite **myResourceGroupTM1**.
   - **Detalhes da Instância** > **Nome da máquina virtual**: Tipo de *myIISVMEastUS*.
   - **Detalhes da instância** > **região**:  Selecione **Leste dos EUA**.
   - **Conta de administrador** > **nome de usuário**:  Insira um nome de usuário de sua escolha.
   - **Conta de administrador** > **senha**:  Insira uma senha de sua escolha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
   - **Regras de porta de entrada** > **portas de entrada públicas**: Selecione **Permitir portas selecionadas**.
   - **Regras de porta de entrada** > **selecione as portas de entrada**: Selecione **RDP** e **HTTP** na caixa suspenso.

3. Selecione o **Management** , ou selecione **Avançar: Discos**, em seguida, **Avançar: Sistema de rede**, em seguida, **Avançar: Gerenciamento**. Em **Monitoramento**, defina **Diagnóstico de inicialização** como **Desativado**.
4. Selecione **Examinar + criar**.
5. Examine as configurações e, em seguida, clique em **criar**.  
6. Siga as etapas para criar uma segunda VM denominada *myIISVMWestEurope*, com um **grupo de recursos** nome do *myResourceGroupTM2*, um **local**dos *Europa Ocidental*e todas as outras configurações igual *myIISVMEastUS*.
7. As VMs podem levar alguns minutos para serem criadas. Não continue com as etapas restantes até que ambas as VMs sejam criadas.

   ![Criar uma máquina virtual](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Instalar o IIS e personalizar a página da Web padrão

Nesta seção, você deve instalar o servidor IIS em duas VMs *myIISVMEastUS* e *myIISVMWestEurope*e, em seguida, atualize a página padrão do site. A página do site personalizada mostra o nome da VM que você está se conectando ao visitar o site em um navegador da web.

1. Clique em **Todos os recursos** no menu esquerdo e, em seguida, na lista de recursos, clique em *myIISVMEastUS*, que está localizado no grupo de recursos *myResourceGroupTM1*.
2. Na página **Visão geral**, clique em **Conectar**e, em seguida, em **Conectar-se a máquina virtual**, selecione **Fazer download do arquivo RDP**.
3. Abra o arquivo rdp baixado. Se solicitado, selecione **Conectar**. Insira o nome de usuário e senha que você especificou ao criar a VM. Talvez seja necessário selecionar **Mais escolhas** e, em seguida, **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM.
4. Selecione **OK**.
5. Você pode receber um aviso do certificado durante o processo de logon. Se você receber o aviso, selecione **Sim** ou **Continuar**, para prosseguir com a conexão.
6. Na área de trabalho do servidor, navegue até **Ferramentas Administrativas do Windows**>**Gerenciador do Servidor**.
7. Inicie o Windows PowerShell na VM1 e usando os comandos a seguir para instalar o servidor do IIS e atualizar o arquivo htm padrão.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Instalar o IIS e personalizar a página da web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. Feche a conexão RDP com *myIISVMEastUS*.
9. Repita as etapas de 1 a 8 com, criando uma conexão de RDP com a VM *myIISVMWestEurope* dentro de *myResourceGroupTM2* grupo de recursos para instalar o IIS e personalizar sua página da web padrão.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurar nome DNS para as VMs que executam IIS

O Gerenciador de Tráfego roteia o tráfego de usuário com base no nome DNS dos pontos de extremidade de serviço. Nesta seção, você configura os nomes DNS para os servidores IIS - *myIISVMEastUS* e *myIISVMWestEurope*.

1. Clique em **Todos os Recursos** no menu esquerdo e, em seguida, na lista de recursos, selecione *myIISVMEastUS*, que está localizado no grupo de recursos *myResourceGroupTM1*.
2. Na página **Visão Geral**, em **Nome DNS**, selecione **Configurar**.
3. Na página **Configuração**, em rótulo de nome DNS, adicione um nome exclusivo e, em seguida, selecione **Salvar**.
4. Repita as etapas de 1 a 3 para a VM denominada *myIISVMWestEurope* que está localizado na *myResourceGroupTM2* grupo de recursos.

### <a name="create-test-vms"></a>Criar VMs de teste

Nesta seção, você pode criar uma VM (*myVMEastUS* e *myVMWestEurope*) em cada região do Azure (**Leste dos EUA** e **Europa Ocidental**). Você usará essas máquinas virtuais para testar como o Gerenciador de Tráfego roteia o tráfego para o servidor mais próximo do IIS ao navegar até o site.

1. No canto superior esquerda do portal do Azure, selecione **criar um recurso** > **computação** > **Windows Server Datacenter de 2019**.
2. Em **Criar uma máquina virtual**, insira ou selecione os seguintes valores na guia **Configurações básicas**:

   - **Assinatura** > **Grupo de Recursos**: Selecione **myResourceGroupTM1**.
   - **Detalhes da Instância** > **Nome da máquina virtual**: Tipo de *myVMEastUS*.
   - **Detalhes da instância** > **região**:  Selecione **Leste dos EUA**.
   - **Conta de administrador** > **nome de usuário**:  Insira um nome de usuário de sua escolha.
   - **Conta de administrador** > **senha**:  Insira uma senha de sua escolha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
   - **Regras de porta de entrada** > **portas de entrada públicas**: Selecione **Permitir portas selecionadas**.
   - **Regras de porta de entrada** > **selecione as portas de entrada**: Selecione **RDP** na caixa suspenso.

3. Selecione o **Management** , ou selecione **Avançar: Discos**, em seguida, **Avançar: Sistema de rede**, em seguida, **Avançar: Gerenciamento**. Em **Monitoramento**, defina **Diagnóstico de inicialização** como **Desativado**.
4. Selecione **Examinar + criar**.
5. Examine as configurações e, em seguida, clique em **criar**.  
6. Siga as etapas para criar uma segunda VM denominada *myVMWestEurope*, com um **grupo de recursos** nome do *myResourceGroupTM2*, um **local** dos *Europa Ocidental*e todas as outras configurações igual *myVMEastUS*.
7. As VMs podem levar alguns minutos para serem criadas. Não continue com as etapas restantes até que ambas as VMs sejam criadas.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gerenciador de Tráfego

Crie um perfil do Gerenciador de Tráfego que direciona o tráfego do usuário enviando-os para o ponto de extremidade com a menor latência.

1. No canto superior esquerdo da tela, selecione **Criar um recurso** > **Rede** > **Perfil do Gerenciador de Tráfego**  >  **Criar**.
2. No **perfil Criar Gerenciador de Tráfego**, insira ou selecione as informações a seguir, aceite os padrões para as configurações restantes e selecione **Criar**:

    | Configuração                 | Valor                                              |
    | ---                     | ---                                                |
    | NOME                   | Esse nome deve ser exclusivo na zona trafficmanager.net e resulta no nome DNS, trafficmanager.net, que é usado para acessar o seu perfil do Gerenciador de Tráfego.                                   |
    | Método de roteamento          | Selecione o método de roteamento **Desempenho**.                                       |
    | Assinatura            | Selecione sua assinatura.                          |
    | Grupo de recursos          | Selecione o grupo de recursos *myResourceGroupTM1*. |
    | Local padrão                | Selecione **Leste dos EUA**. Essa configuração refere-se ao local do grupo de recursos e não tem impacto no perfil do Gerenciador de Tráfego que será implantado globalmente.                              |
    |

    ![Criar um perfil do Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Adicionar pontos de extremidade do Gerenciador de Tráfego

Adicione as duas VMs executando o IIS servidores - *myIISVMEastUS* & *myIISVMWestEurope* para rotear o tráfego de usuário para o ponto de extremidade mais próximo ao usuário.

1. Na barra de pesquisa do portal, pesquise o nome do Perfil do Gerenciador de Tráfego que você criou na seção anterior e selecione o perfil do gerenciador de tráfego nos resultados que são exibidos.
2. Em **Perfil do Gerenciador de Tráfego**, na seção **Configurações**, clique em **Pontos de Extremidade** e clique em **Adicionar**.
3. Insira, ou selecione, as informações a seguir, aceite os padrões para as configurações restantes e, em seguida, selecione **OK**:

    | Configuração                 | Value                                              |
    | ---                     | ---                                                |
    | Type                    | Ponto de extremidade do Azure                                   |
    | NOME           | myEastUSEndpoint                                        |
    | Tipo de recurso de destino           | Endereço IP público                          |
    | Recurso de destino          | **Escolha um endereço IP Público** para mostrar a lista de recursos com endereços IP públicos na mesma assinatura. Em **Recursos**, selecione o endereço IP público denominado *myIISVMEastUS-ip*. Isso é o endereço IP público do servidor IIS VM no Leste dos EUA.|
    |        |           |

4. Repita as etapas 2 e 3 para adicionar outro ponto de extremidade chamado *myWestEuropeEndpoint* para o endereço IP público *ip myIISVMWestEurope* que está associado com o servidor IIS VM denominada  *myIISVMWestEurope*.
5. Quando a adição de ambos os pontos de extremidade estiver concluída, eles serão exibidos em **Perfil do Gerenciador de Tráfego** com seu status de monitoramento como **Online**.

    ![Adicionar um ponto de extremidade do Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)

## <a name="test-traffic-manager-profile"></a>Testar perfil de Gerenciador de Tráfego

Nesta seção, você testa como o Gerenciador de Tráfego do Microsoft Azure roteia o tráfego de usuário para as VMs mais próximas executando o site para fornecer latência mínima. Para exibir o Gerenciador de Tráfego em ação, conclua as seguintes etapas:

1. Determine o nome DNS do seu perfil do Gerenciador de Tráfego.
2. Visualizar o Gerenciador de Tráfego em ação conforme a seguir:
    - Na VM de teste (*myVMEastUS*) que está localizada na região do **Leste dos EUA**, em um navegador da web, navegue até o nome DNS do seu perfil do Gerenciador de Tráfego.
    - Na VM de teste (*myVMWestEurope*) que está localizado na **Europa Ocidental** região, em um navegador da web, navegue até o nome DNS do seu perfil do Gerenciador de tráfego.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Determinar o nome DNS do Perfil do Gerenciador de Tráfego

Neste tutorial, para manter a simplicidade, você usará o nome DNS do perfil do Gerenciador de Tráfego do Microsoft Azure para visitar os sites da Web.

Você pode determinar o nome DNS do seu perfil do Gerenciador de Tráfego conforme a seguir:

1. Na barra de pesquisa do portal, procure o nome do **Perfil do Gerenciador de Tráfego** criado na seção anterior. Nos resultados exibidos, clique no perfil do gerenciador de tráfego.
2. Clique em **Visão Geral**.
3. O **Perfil do Gerenciador de Tráfego** exibe o nome DNS do perfil do Gerenciador de Tráfego criado recentemente. Nas implantações de produção, você configura um nome de domínio intuitivo para apontar para o nome de domínio do Gerenciador de Tráfego, usando um registro DNS CNAME.

   ![Nome DNS do Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Ver o Gerenciador de Tráfego em ação

Nesta seção, você pode ver que o Gerenciador de Tráfego é a ação.

1. Selecione **Todos os recursos** no menu esquerdo e, em seguida, na lista de recursos, clique em *myIISVMEastUS*, que está localizado no grupo de recursos *myResourceGroupTM1*.
2. Na página **Visão geral**, clique em **Conectar**e, em seguida, em **Conectar-se a máquina virtual**, selecione **Fazer download do arquivo RDP**.
3. Abra o arquivo rdp baixado. Se solicitado, selecione **Conectar**. Insira o nome de usuário e senha que você especificou ao criar a VM. Talvez seja necessário selecionar **Mais escolhas** e, em seguida, **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM.
4. Selecione **OK**.
5. Você pode receber um aviso do certificado durante o processo de logon. Se você receber o aviso, selecione **Sim** ou **Continuar**, para prosseguir com a conexão.
1. Em um navegador da Web na VM *myVMEastUS*, digite o nome do DNS do seu perfil do Gerenciador de Tráfego para visualizar seu site. Uma vez que a máquina virtual está localizada no **Leste dos EUA**, você será encaminhado para o site mais próximo hospedado no servidor mais próximo do IIS *myIISVMEastUS* que está localizado no **Leste dos EUA**.

   ![Testar perfil de Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Em seguida, conecte-se à VM *myVMWestEurope* localizada na **Europa Ocidental** usando as etapas 1 a 5 e navegue para o nome de domínio de perfil do Gerenciador de Tráfego do Microsoft Azure dessa VM. Como a máquina virtual localizada na **Europa Ocidental**, você agora é roteadas para o site hospedado em mais próximo do servidor do IIS *myIISVMWestEurope* que está localizado no **Europa Ocidental**.

   ![Testar perfil de Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)

## <a name="delete-the-traffic-manager-profile"></a>Excluir o perfil do Gerenciador de Tráfego

Quando não for mais necessário, exclua os grupos de recursos (**ResourceGroupTM1** e **ResourceGroupTM2**). Para fazer isso, selecione o grupo de recursos (**ResourceGroupTM1** ou **ResourceGroupTM2**) e, em seguida, selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Distribuir tráfego para um conjunto de pontos de extremidade](traffic-manager-configure-weighted-routing-method.md)
