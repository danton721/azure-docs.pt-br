---
title: Tutorial para preparar o portal do Azure e o ambiente de datacenter para implantar o Azure Data Box Edge| Microsoft Docs
description: O primeiro tutorial sobre como implantar o Azure Data Box Edge envolve a preparação do portal do Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: d7e66970db3397531c798bc37bf7c1f346e999bf
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924764"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-edge"></a>Tutorial: Preparar para implantar o Azure Data Box Edge  


Este é o primeiro tutorial da série de tutoriais de implantação necessários para implantar completamente seu Azure Data Box Edge. Este tutorial descreve como preparar o portal do Azure para implantar um recurso do Data Box Edge.

Você precisará de privilégios de administrador para concluir o processo de instalação e configuração. A preparação do portal leva menos de 10 minutos.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obter a chave de ativação

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


### <a name="get-started"></a>Introdução

Veja os tutoriais a seguir na sequência indicada para implantar o Data Box Edge.

| **#** | **Nesta etapa** | **Use estes documentos** |
| --- | --- | --- | 
| 1. |**[Preparar o portal do Azure para o Data Box Edge](data-box-edge-deploy-prep.md)** |Crie e configure o recurso do Data Box Edge antes de instalar um dispositivo físico do Data Box Edge. |
| 2. |**[Instalar o Data Box Edge](data-box-edge-deploy-install.md)**|Desempacote, monte no rack e prepare o cabeamento do dispositivo físico do Data Box Edge.  |
| 3. |**[Conectar-se, configurar e ativar o Data Box Edge](data-box-edge-deploy-connect-setup-activate.md)** |Conectar-se a interface do usuário da Web local, conclua a configuração do dispositivo e ativar o dispositivo. O dispositivo está pronto para configurar compartilhamentos NFS ou SMB.  |
| 4. |**[Transferir dados com o Data Box Edge](data-box-edge-deploy-add-shares.md)** |Adicione compartilhamentos e conecte-se a ele por meio de SMB ou NFS. |
| 5. |**[Transformar dados com o Data Box Edge](data-box-edge-deploy-configure-compute.md)** |Configure módulos do Edge no dispositivo para transformar os dados à medida que eles se movem para o Azure. |

Agora, você pode começar a configurar o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

A seguir estão os pré-requisitos de configuração para o recurso do Data Box Edge, o dispositivo do Data Box Edge e a rede do datacenter.

### <a name="for-the-data-box-edge-resource"></a>Para o recurso do Data Box Edge

Antes de começar, verifique se:

- Sua assinatura do Microsoft Azure está habilitada para o recurso do Data Box Edge. Não há suporte para as assinaturas pagas conforme o uso.
- Você tem acesso de colaborador ou proprietário à sua assinatura.
- Você tem acesso de usuário ou administrador à API do Azure Active Directory Graph. Para obter mais informações, confira [API do Azure Active Directory Graph](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Você tem sua conta de armazenamento do Microsoft Azure com credenciais de acesso.

### <a name="for-the-data-box-edge-device"></a>Para o dispositivo do Data Box Edge

Antes de implantar um dispositivo físico, verifique se:

- Você examinou as informações de segurança que foram incluídas no pacote de remessa.
- Você tem um slot de 1 U disponível em um rack padrão de 19" no seu datacenter para montar o dispositivo em rack. 
- Você tem acesso a uma superfície de trabalho plana, estável e nivelada na qual o dispositivo pode apoiar-se com segurança.
- O local em que você pretende instalar o dispositivo tem alimentação CA padrão de uma fonte independente ou uma PDU (unidade de distribuição de energia) no rack com um no-break.
- Você tem acesso a um dispositivo físico.


### <a name="for-the-datacenter-network"></a>Para a rede de datacenter

Antes de começar, verifique se:

- A rede no seu datacenter é configurada de acordo com os requisitos de rede do seu dispositivo de Data Box Edge. Para obter mais informações, confira [Requisitos do sistema do Data Box Edge](data-box-edge-system-requirements.md).

- Para as condições normais de operação do Data Box Edge, você tem:

    - Uma largura de banda de download de, no mínimo, 10 Mbps para garantir que o dispositivo permaneça atualizado.
    - Uma largura de banda dedicada de download e upload de, no mínimo, 20 Mbps para transferir arquivos.

## <a name="create-a-new-resource"></a>Criar um novo recurso

Se você tiver um recurso do Data Box Edge existente para gerenciar seu dispositivo físico, ignore esta etapa e vá para [Obter a chave de ativação](#get-the-activation-key).

Para criar um recurso do Data Box Edge, execute as seguintes etapas no portal do Azure.

1. Use suas credenciais do Microsoft Azure para entrar 
    
    - No portal do Azure nesta URL: [https://portal.azure.com](https://portal.azure.com).
    - Ou no portal do Azure Governamental nesta URL: [https://portal.azure.us](https://portal.azure.us). Para obter mais detalhes, acesse [Conectar-se ao Azure Governamental usando o portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

2. No painel esquerdo, selecione **+ Criar um recurso**. Pesquise **Data Box Edge / Data Box Gateway**. Selecione **Data Box Edge / Data Box Gateway**. Selecione **Criar**.
3. Escolha a assinatura que deseja usar para o dispositivo do Data Box Edge. Selecione a região em que você deseja implantar o recurso do Data Box Edge. Para esta versão, o Leste dos EUA, o Sudeste Asiático e o Oeste da Europa estão disponíveis. 

    Escolha um local mais próximo da região geográfica em que você deseja implantar seu dispositivo. A região armazena apenas os metadados para o gerenciamento de dispositivos. Os dados reais podem ser armazenados em qualquer conta de armazenamento. 
    
    Na opção **Data Box Edge**, selecione **Criar**.

    ![Pesquisar o serviço do Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-sku.png)

3. Na guia **Informações Básicas**, insira ou selecione os **Detalhes do projeto** a seguir.
    
    |Configuração  |Valor  |
    |---------|---------|
    |Assinatura    |Isso é preenchido automaticamente de acordo com a seleção anterior. A assinatura está vinculada à sua conta de faturamento. |
    |Grupo de recursos  |Selecione um grupo existente ou crie um novo grupo.<br>Saiba mais sobre [grupos de recursos do Azure](../azure-resource-manager/resource-group-overview.md).     |

4. Insira ou selecione os **Detalhes da instância** a seguir.

    |Configuração  |Valor  |
    |---------|---------|
    |NOME   | Um nome amigável para identificar o recurso.<br>O nome tem entre 2 e 50 caracteres contendo letras, números e hifens.<br> Nome começa e termina com uma letra ou um número.        |
    |Região     |Para esta versão, Leste dos EUA, Sudeste da Ásia e Oeste da Europa estão disponíveis para implantação do recurso. Se você estiver usando o Azure Governamental, todas as regiões do governo estarão disponíveis, conforme mostrado nas [regiões do Azure](https://azure.microsoft.com/global-infrastructure/regions/).<br> Escolha um local mais próximo da região geográfica em que você deseja implantar seu dispositivo.|

    ![Detalhes do projeto e da instância](media/data-box-edge-deploy-prep/data-box-edge-resource.png)

5. Selecione **Avançar: Endereço para entrega**.

    - Caso já tenha um dispositivo, marque a caixa de combinação **Tenho um dispositivo do Data Box Edge**.
    - Se esse for o novo dispositivo solicitado, insira o nome de contato, a empresa, o endereço para entrega do dispositivo e informações de contato.

    ![Endereço para entrega do novo dispositivo](media/data-box-edge-deploy-prep/data-box-edge-resource1.png)

6. Selecione **Avançar: Revisar + criar**.

7. Na guia **Examinar + criar**, examine os **Detalhes de preço**, os **Termos de uso** e os detalhes do recurso. Marque a caixa de combinação **Li os termos de privacidade**.

    ![Examinar os detalhes do recurso do Data Box Edge e os termos de privacidade](media/data-box-edge-deploy-prep/data-box-edge-resource2.png)

8. Selecione **Criar**.

A criação do recurso leva alguns minutos. Depois que o recurso for criado com êxito e implantado, você será notificado. Selecione **Ir para o recurso**.

![Ir para o recurso do Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-resource3.png)

Depois que o pedido for feito, a Microsoft analisará o pedido e o contatará (por email) com os detalhes da entrega.

![Notificação para análise do pedido do Data Box Edge](media/data-box-edge-deploy-prep/data-box-edge-resource4.png)

## <a name="get-the-activation-key"></a>Obter a chave de ativação

Depois que o recurso do Data Box Edge estiver em execução, será necessário obter a chave de ativação. Essa chave é usada para ativar e conectar seu dispositivo do Data Box Edge ao recurso. Você pode obter essa chave agora enquanto estiver no portal do Azure.

1. Selecione o recurso criado. Selecione **Visão geral** e, em seguida, **Configuração do dispositivo**.

    ![Selecionar Configuração do dispositivo](media/data-box-edge-deploy-prep/data-box-edge-select-devicesetup.png)

2. No bloco **Ativar**, selecione **Gerar chave** para criar uma chave de ativação. Selecione o ícone de copiar para copiar a chave e salvá-la para uso posterior.

    ![Obter a chave de ativação](media/data-box-edge-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - A chave de ativação expira três dias depois de ser gerada.
> - Se a chave expirou, gere uma nova. A chave antiga não é válida.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu sobre os tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Criar um novo recurso
> * Obter a chave de ativação

Prossiga para o próximo tutorial para saber como instalar o Data Box Edge.

> [!div class="nextstepaction"]
> [Instalar o Data Box Edge](./data-box-edge-deploy-install.md)



