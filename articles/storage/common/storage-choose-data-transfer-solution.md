---
title: Escolha uma solução do Azure para transferência de dados | Microsoft Docs
description: Saiba como escolher uma solução do Azure para transferência de dados com base em tamanhos de dados e largura de banda disponível em seu ambiente
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: 56470c08f0ac940dae42821ae61846f1c86d52eb
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479520"
---
# <a name="choose-an-azure-solution-for-data-transfer"></a>Escolha uma solução do Azure para transferência de dados

Este artigo fornece uma visão geral de algumas das soluções de transferência de dados comuns do Azure. O artigo também tem links para as opções recomendadas, dependendo da largura de banda de rede em seu ambiente e o tamanho dos dados que você pretende transferir.

## <a name="types-of-data-movement"></a>Tipos de movimentação de dados

A transferência de dados pode ser off-line ou sobre a conexão de rede. Escolha sua solução, dependendo de:

- **Tamanho dos dados** - tamanho dos dados que se destina para transferência,
- **Frequência da transferência** - ingestão de dados única ou periódica, e
- **Rede** – transferência de Largura de Banda disponível para transferência de dados de entrada em seu ambiente.

A movimentação de dados pode ser dos seguintes tipos:

- **Transferência offline usando os dispositivos enviáveis** - usar dispositivos físicos de envio quando você deseja fazer a transferência de dados em massa de uso único offline. A Microsoft envia um disco ou um dispositivo especializado seguro. Como alternativa, você pode comprar e enviar seus próprios discos. Você copia dados para o dispositivo e, em seguida, transfere-o para o Azure em que os dados são carregados.  As opções disponíveis para esse caso são Data Box Disk, Data Box, Data Box Heavy e Importação/Exportação (use seus próprios discos).

- **Transferência de Rede** - transferir seus dados para o Azure ao longo de sua conexão de rede. Isso pode ser feito de várias maneiras.

    - **Interface gráfica** - se você ocasionalmente transferir apenas alguns arquivos e não é necessário automatizar a transferência de dados, você pode escolher uma ferramenta de interface gráfica, como o Gerenciador de Armazenamento do Microsoft Azure ou uma ferramenta de exploração baseada na web no portal do Azure.
    - **Transferência programática ou com script** - você pode usar ferramentas de software otimizado fornecidas ou chamar nossas APIs REST/SDKs diretamente. Ferramentas programáveis disponíveis são AzCopy, Azure PowerShell e CLI do Azure. Para uma interface programática, use um dos SDKs para .NET, Java, Python, Node/JS, C++, Go, PHP ou Ruby.
    - **Dispositivos locais** - fornecemos um dispositivo físico ou virtual que reside em seu datacenter e otimiza a transferência de dados pela rede. Esses dispositivos também fornecem um cache local dos arquivos usados com frequência. O dispositivo físico é a borda da caixa de dados e o dispositivo virtual é o Azure Data Box Gateway. Ambos executados permanentemente em suas instalações e se conectam ao Azure pela rede.
    - **Pipeline de dados gerenciados** - você pode configurar um pipeline de nuvem para regularmente transferir arquivos entre vários serviços do Azure, no local ou uma combinação dos dois. Use o Azure Data Factory para configurar e gerenciar pipelines de dados e mover e transformar os dados para análise.

O visual a seguir ilustra as diretrizes para escolher a transferência de dados do Azure de várias ferramentas de acordo com a largura de banda de rede disponível para transferência, tamanho dos dados destinado a transferência e a frequência da transferência.

![Ferramentas de transferência de dados do Azure](media/storage-choose-data-transfer-solution/azure-data-transfer-options-3.png)

**Os limites superiores dos dispositivos de transferência off-line - Data Box Disk, Data Box e Data Box Heavy podem ser estendidos colocando várias ordens de um tipo de dispositivo.*

## <a name="selecting-a-data-transfer-solution"></a>Seleção de uma solução de transferência de dados

Responda às seguintes perguntas para ajudar a selecionar uma solução de transferência de dados:

- A sua largura de banda disponível limitada ou não existente está disponível e você deseja transferir grandes conjuntos de dados?
  
    Em caso afirmativo, consulte: [Cenário 1: Transferência de grandes conjuntos de dados sem nenhuma ou pouca largura de banda](storage-solution-large-dataset-low-network.md).
- Deseja transferir grandes conjuntos de dados sobre a rede e ter uma largura de banda de rede de moderada a alta?

    Em caso afirmativo, consulte: [Cenário 2: Transferência de grandes conjuntos de dados com largura de banda de rede moderada a alta](storage-solution-large-dataset-moderate-high-network.md).
- Deseja transferir, ocasionalmente, apenas alguns arquivos pela rede?

    Em caso positivo, consulte [Cenário 3: Transferir pequenos conjuntos de dados com largura de banda de rede limitada a moderada](storage-solution-small-dataset-low-moderate-network.md).
- Você está procurando a transferência de dados de point-in-time em intervalos regulares?

    Em caso afirmativo, use as opções de script/programação descritas no [Cenário 4: Transferência de dados periódica](storage-solution-periodic-data-transfer.md).
- Você está procurando a transferência de dados contínua em andamento?

    Em caso positivo, use as opções no [Cenário 4: Transferência de dados periódica](storage-solution-periodic-data-transfer.md).
 

## <a name="data-transfer-feature-in-azure-portal"></a>Recurso de transferência de dados no portal do Azure

Você também pode ir para sua conta de armazenamento do Azure no portal do Azure e selecione o **transferência de dados** recurso. Forneça a largura de banda de rede em seu ambiente, o tamanho dos dados que você deseja transferir e a frequência de transferência de dados. Você verá os dados ideais transferência soluções correspondente às informações que você forneceu. 

## <a name="next-steps"></a>Próximas etapas

- [Obter uma introdução ao Armazenamento do Microsoft Azure](https://azure.microsoft.com/resources/videos/introduction-to-microsoft-azure-storage-explorer/).
- [Leia uma visão geral do AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10).
- [Usar o Azure PowerShell com o Armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/common/storage-powershell-guide-full)
- [Uso da CLI do Azure com o Armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/common/storage-azure-cli)
- Saiba mais sobre:

    - [Azure Data Box, Azure Data Box Disk e Azure Data Box Heavy para transferências offline ](https://docs.microsoft.com/azure/databox/).
    - [Azure Data Box Gateway e Azure Data Box Edge para transferências online](https://docs.microsoft.com/azure/databox-online/).
- [Saiba o que é o Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview).
- Use as APIs REST para transferir dados

    - [In .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
    - [In Java](https://docs.microsoft.com/java/api/overview/azure/storage/client)
