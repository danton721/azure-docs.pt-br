---
title: 'Tutorial de instalação: desempacotar, cabear e montar em rack um dispositivo físico do Azure Data Box Edge | Microsoft Docs'
description: O segundo tutorial sobre como instalar o Azure Data Box Edge envolve desempacotamento, montagem em rack e cabeamento do dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 7062e9d829242a62dfa4f661c21ce6c56b0db69c
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924532"
---
# <a name="tutorial-install-azure-data-box-edge"></a>Tutorial: Instalar o Azure Data Box Edge

Este tutorial descreve como instalar um dispositivo físico do Data Box Edge. O procedimento de instalação envolve desempacotamento, montagem em rack e cabeamento do dispositivo. 

A instalação poderá levar cerca de duas horas para ser concluída.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Desempacotar o dispositivo
> * Montar o dispositivo em rack
> * Cabear o dispositivo

## <a name="prerequisites"></a>Pré-requisitos

Os pré-requisitos de instalação de um dispositivo físico são os seguintes:

### <a name="for-the-data-box-edge-resource"></a>Para o recurso do Data Box Edge

Antes de começar, verifique se:

* Você concluiu todas as etapas descritas em [Preparar a implantação do Azure Data Box Edge](data-box-edge-deploy-prep.md).
    * Você criou um recurso do Data Box Edge para implantar seu dispositivo.
    * Você já gerou a chave de ativação para ativar seu dispositivo com o recurso do Data Box Edge.

 
### <a name="for-the-data-box-edge-physical-device"></a>Para o dispositivo físico do Data Box Edge

Antes de implantar um dispositivo:

- Coloque o dispositivo em uma superfície de trabalho plana, estável e nivelada.
- Verifique se o local em que você pretende configurar tem:
    - Alimentação CA padrão de uma fonte independente

        -OU-
    - Uma PDU (unidade de distribuição de energia) de rack com um no-break
    - Um slot de 1U disponível no rack em que você pretende montar o dispositivo

### <a name="for-the-network-in-the-datacenter"></a>Para a rede no datacenter

Antes de começar:

- Examine os requisitos de rede para implantar o Data Box Edge e configure a rede de datacenter de acordo com os requisitos. Para saber mais, veja [Requisitos de rede do Data Box Edge](data-box-edge-system-requirements.md#networking-port-requirements).

- Garanta que a largura de banda mínima da Internet seja de 20 Mbps para o funcionamento ideal do dispositivo.


## <a name="unpack-the-device"></a>Desempacotar o dispositivo

Este dispositivo é fornecido em uma única caixa. Conclua as etapas a seguir para desempacotar seu dispositivo. 

1. Coloque a caixa em uma superfície plana e nivelada.
2. Inspecione a caixa e a espuma da embalagem para verificar se não há amassados, cortes, danos por água ou qualquer outro dano visível. Se a caixa ou a embalagem estiver gravemente danificada, não a abra. Contate o Suporte da Microsoft para receber ajuda e saber se o dispositivo está em boas condições de funcionamento.
3. Abra a caixa. Depois de abrir a caixa, verifique se você tem estes itens:
    - Um dispositivo do Edge de compartimento único
    - Dois cabos de alimentação
    - Um conjunto de kit de trilho
    - Um livreto Informações sobre Regulamentação, Segurança e Meio-ambiente

Caso não tenha recebido todos os itens listados aqui, contate o suporte do Data Box Edge. A próxima etapa é montar o dispositivo em rack.


## <a name="rack-the-device"></a>Montar o dispositivo em rack

O dispositivo precisa ser instalado em um rack padrão de 19 polegadas. Use o procedimento a seguir para montar o dispositivo em um rack padrão de 19 polegadas.

> [!IMPORTANT]
> Os dispositivos Data Box Edge devem ser montados em rack para a operação apropriada.


### <a name="prerequisites"></a>Pré-requisitos

- Antes de começar, leia as instruções de segurança no livreto Informações sobre Regulamentação, Segurança e Meio-ambiente. Esse livreto foi enviado com o dispositivo.
- Comece a instalar os trilhos no espaço alocado mais próximo à parte inferior do compartimento de rack.
- Para a configuração de montagem do trilho com ferramentas:
    -  Você precisa fornecer oito parafusos: 10-32, 12-24, M5 ou M6. O diâmetro da cabeça dos parafusos precisa ser inferior a 10 mm (0,4").
    -  Você precisa de uma chave de fenda de ponta chata.

### <a name="identify-the-rail-kit-contents"></a>Identificar o conteúdo do kit de trilho

Localize os componentes para instalação do conjunto do kit de trilho:
1. Dois conjuntos de trilho deslizante A7 Dell ReadyRails II
2. Duas tiras com fixador de contato

![Identificar o conteúdo do kit de trilho](./media/data-box-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Instale e remova os trilhos sem ferramentas (racks de orifício quadrado ou redondo)

> [!TIP]
> Essa opção é sem ferramenta porque não exige ferramentas para instalar e remover os trilhos no quadrado não rosqueado ou furos redondos no rack.

1. Posicione as extremidades esquerda e direita do trilho rotuladas **FRENTE** voltadas para dentro e vire cada extremidade até encaixá-las nos orifícios no lado dianteiro das bordas verticais do rack.
2. Alinhe cada extremidade nos orifícios inferior e superior dos espaços em U desejados.
3. Encaixe a extremidade traseira do trilho até que ela se ajuste totalmente na borda vertical do rack e a trava se encaixe no lugar. Repita essas etapas para posicionar e encaixar a extremidade dianteira da borda vertical do rack.
4. Para remover os trilhos, aperte o botão de liberação da trava no ponto médio da extremidade e remova cada trilho.

![Instalar e remover os trilhos sem ferramentas](./media/data-box-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Instalar e remover os trilhos com ferramentas (racks com orifício rosqueado)

> [!TIP]
> Essa opção é com ferramentas porque requer uma ferramenta (_uma chave de fenda de ponta chata_) para instalar e remover os trilhos redondo rosqueados nos racks.

1. Remova os pinos dos suportes de montagem dianteiro e traseiro usando uma chave de fenda com ponta achatada.
2. Puxe e gire os subconjuntos de trava de trilho para removê-los dos suportes de montagem.
3. Fixe os trilhos de montagem esquerdo e direito nas bordas verticais dianteiras do rack usando dois pares de parafusos.
4. Deslize os suportes traseiros esquerdo e direito para frente em direção às bordas verticais traseiras do rack e fixe-os usando dois pares de parafusos.

![Instalar e remover trilhos com ferramentas](./media/data-box-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Instalar o sistema em um rack

1. Puxe os trilhos deslizantes internos para fora do rack até eles fiquem travados.
2. Localize o suporte do trilho traseiro em cada lado do sistema e abaixe-os até os slots J traseiros sobre os conjuntos deslizantes. Gire o sistema para baixo até todos os suportes do trilho serem encaixados nos slots J.
3. Empurre o sistema para dentro até as alavancas de trava se encaixarem.
4. Pressione os botões de trava de liberação deslizante em ambos os trilhos e deslize o sistema no rack.

![Instalar o sistema em um rack](./media/data-box-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Remover o sistema do rack

1. Localize as alavancas de trava nas laterais dos trilhos internos.
2. Destrave cada alavanca girando-a até a posição de liberação.
3. Segure as laterais do sistema com firmeza e puxe-o para frente até os suportes do trilho ficarem na frente dos slots J. Levante o sistema para cima e longe do rack e coloque-o em uma superfície nivelada.

![Remover o sistema do rack](./media/data-box-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Encaixar e liberar a trava do tipo slam

> [!NOTE]
> Para sistemas não equipados com travas do tipo slam, fixe o sistema usando parafusos, conforme descrito na etapa 3 deste procedimento.

1. Na parte dianteira, localize a trava do tipo slam em uma das laterais do sistema.
2. As travas fecham automaticamente conforme o sistema é empurrado para o rack e são liberadas quando puxadas para cima.
3. Para fixar o sistema de remessa no rack ou para outros ambientes instáveis, localize o parafuso de montagem rígida sob cada trava e aperte cada parafuso com uma chave Phillips nº 2.

![Encaixar e liberar a trava do tipo slam](./media/data-box-edge-deploy-install/engaging-releasing-slam-latch.png)

### <a name="route-the-cables"></a>Rotear os cabos

> [!NOTE]
>  Se você não solicitou o CMA (Braço de Gerenciamento de Cabos) opcional, use as duas tiras com fixador de contato fornecidas no kit de trilho para rotear os cabos na parte traseira do sistema.

1. Localize os suportes externos do CMA nas laterais interiores de ambas as bordas do rack.
2. Agrupe os cabos com cuidado, afastando-os dos conectores do sistema nas laterais esquerda e direita.
3. Passe as tiras com fixador de contato pelos slots nos suportes externos do CMA em cada lateral do sistema para fixar os conjuntos de cabos.


![Rotear os cabos](./media/data-box-edge-deploy-install/routing-cables.png)

## <a name="cable-the-device"></a>Cabear o dispositivo

Os procedimentos a seguir explicam como cabear o dispositivo do Edge para energia e rede.

Antes de começar o cabeamento do dispositivo, você precisará do seguinte:

- Seu dispositivo físico do Edge, descompactado e montado no rack.
- Dois cabos de alimentação. 
- Pelo menos um cabo de rede RJ-45 de 1 GbE para conectar-se à interface de gerenciamento. Há duas interfaces de rede de 1 GbE, uma de gerenciamento e uma de dados, no dispositivo.
- Um cabo de cobre SFP+ de 25 GbE para cada interface de rede de dados a ser configurado. Pelo menos um adaptador de rede de dados da PORTA 2, PORTA 3, PORTA 4, PORTA 5 ou PORTA 6 precisa estar conectado à Internet (com conectividade com o Azure).  
- Acesso a duas unidades de distribuição de energia (recomendado).

> [!NOTE]
> - Se você está conectando somente um adaptador de rede de dados, recomendamos que você use um adaptador de rede de 25/10 GbE, como a PORTA 3, a PORTA 4, a PORTA 5 ou a PORTA 6, para enviar dados para o Azure. 
> - Para melhor desempenho e para lidar com grandes volumes de dados, considere a possibilidade de se conectar a todas as portas de dados.
> - O dispositivo do Edge deve estar conectado à rede de datacenter de modo que possa receber dados de servidores de fonte de dados.

O dispositivo do Edge tem 8 SSDs NVMe. O painel frontal também tem botões de energia e LEDs de status. O dispositivo inclui PSUs (unidades de alimentação) redundantes na parte traseira. Seu dispositivo tem seis adaptadores de rede:

- Duas interfaces de 1 Gbps
- Quatro interfaces de 25 Gbps que também podem servir como interfaces de 10 Gbps.
- Um controlador BMC. 

Identifique as várias portas de dados no backplane do dispositivo.
 
  ![Backplane de um dispositivo cabeado](./media/data-box-edge-deploy-install/backplane-cabled.png)

O dispositivo tem duas placas de rede correspondentes às 6 portas: 

 - QLogic FastLinQ 41264
 - QLogic FastLinQ 41262

Para obter uma lista completa de cabos, comutadores e transceptores compatíveis com essas placas de rede, acesse a [Matriz de interoperabilidade da Cavium FastlinQ série 41000](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Execute as etapas a seguir para cabear o dispositivo para obter energia e rede.

1. Conecte os cabos de energia a cada uma das PSUs no compartimento. Para garantir a alta disponibilidade, instale e conecte as duas PSUs a diferentes fontes de alimentação.

2. Ligue os cabos de alimentação às PDUs (unidades de distribuição de energia) do rack. Verifique se as duas PSUs usam fontes de energia separadas.

3. Conecte a PORTA 1 do adaptador de rede de 1 GbE ao computador usado para configurar o dispositivo físico. A PORTA 1 é o adaptador de gerenciamento dedicado.

4. Conecte uma ou mais das portas (PORTA 2, PORTA 3, PORTA 4, PORTA 5 ou PORTA 6) ao rede do datacenter/Internet. Se estiver se conectando na PORTA 2, use o cabo de rede RJ-45. Para os adaptadores de rede de 10/25 GbE, use os cabos de cobre SFP+.  

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu sobre tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Desempacotar o dispositivo
> * Montar o dispositivo em rack
> * Cabear o dispositivo

Vá para o próximo tutorial para aprender a conectar, configurar e ativar seu dispositivo.

> [!div class="nextstepaction"]
> [Conectar e configurar o Data Box Edge](./data-box-edge-deploy-connect-setup-activate.md)


