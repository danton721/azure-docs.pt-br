---
title: Visão geral do Microsoft Azure Data Box Disk | Microsoft Docs em dados
description: Descreve o Azure Data Box Disk, uma solução de nuvem que permite a transferência de grandes quantidades de dados para o Azure
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/17/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: 22211f52f6b9a16f785cacb9364ebe5b74617453
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126619"
---
# <a name="what-is-azure-data-box-disk-preview"></a>O que é o Azure Data Box Disk? (Visualização)

A solução do Microsoft Azure Data Box Disk permite que você envie terabytes de dados locais para o Azure de forma rápida, barata e confiável. A transferência de dados segura é acelerada por meio do envio de uma a cinco SSDs (unidades de estado sólido). Esses discos de 8 TB criptografados são enviados ao seu datacenter por meio de uma operadora regional. 

Você pode configurar, conectar e desbloquear rapidamente os discos por meio do serviço Data Box no Portal do Azure. Copie seus dados em discos e envie os discos de volta para o Azure. No datacenter do Azure, seus dados são carregados automaticamente das unidades para a nuvem usando um link de carregamento rápido de rede privada.


> [!IMPORTANT]
> - O Data Box Disk está em versão prévia. Examine os [termos de serviço do Azure para a versão prévia](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implantar essa solução. 
> - Você precisa se inscrever nesse serviço. Para se inscrever, vá até o [Portal de versão prévia](http://aka.ms/azuredataboxfromdiskdocs).
> - Durante a versão prévia, o Data Box Disk pode ser entregue aos clientes nos EUA e na União Europeia. Para saber mais, acesse [Disponibilidade por região](#region-availability).

## <a name="use-cases"></a>Casos de uso

Use i Data Box Disk para transferir TBs de dados em cenários sem conectividade de rede limitada. A movimentação de dados pode ser única, periódica ou uma transferência de dados em massa inicial seguida por transferências periódicas. 

- **Migração única** - quando uma grande quantidade de dados local é transferida para o Azure. Por exemplo, movimentar dados de fitas offline para dados de arquivamento no armazenamento esporádico do Azure.
- **Transferência incremental** - quando uma transferência em massa inicial é feita usando o Data Box Disk (semente) seguida por transferências incrementais pela rede. Por exemplo, Commvault e Data Box Disk são usados para mover cópias de backup para o Azure. Essa migração é seguida por meio da cópia de dados incrementais por meio da rede para o armazenamento do Azure. 
- **Carregamentos periódicos** - quando grandes quantidades de dados são geradas periodicamente e precisam ser movidas para o Azure. Por exemplo, na exploração de energia, em que o conteúdo de vídeo é gerado em plataformas de petróleo e em produções de energia eólica.

## <a name="the-workflow"></a>O fluxo de trabalho

Um fluxo de trabalho típico inclui as seguintes etapas:

1. **Ordem** - Crie um pedido no Portal do Azure, forneça informações de envio e a conta de armazenamento de destino do Azure para seus dados. Se houver discos disponíveis, o Azure os criptografará, preparará e enviará com uma ID de acompanhamento de remessa.

2. **Receber** - Após a entrega dos discos, descompacte e conecte os discos ao computador que tem os dados a serem copiados. Desbloqueie os discos.
    
3. **Copiar dados** - Arraste e solte para copiar os dados nos discos.

4. **Retornar** - Preparar e enviar os discos de volta para o datacenter do Azure.

5. **Carregar** - Os dados são copiados automaticamente dos discos para o Azure. Os discos são apagados com segurança, de acordo com as diretrizes do NIST (Instituto Nacional de Padrões e Tecnologia).

Ao longo desse processo, você receberá uma notificação por email sobre todas as alterações de status. Para saber mais sobre o fluxo detalhado, acesse [Implantar Data Box Disk no Portal do Azure](data-box-disk-quickstart-portal.md).


## <a name="benefits"></a>Benefícios

O Data Box Disk foi projetado para mover grandes quantidades de dados para o Azure sem afetar a rede. A solução oferece as seguintes vantagens:

- **Velocidade** - O Data Box Disk usa uma conexão USB 3.0 para mover até 35 TB de dados para o Azure em menos de uma semana.   

- **Fácil de usar** - O Data Box é uma solução fácil de usar.

    - Os discos usam a conectividade USB sem exigir quase nenhum tempo de configuração.
    - Os discos têm um fator forma compacto que os torna fáceis de manusear.
    - Os discos não têm requisitos de alimentação externa.
    - Os discos podem ser usados com um laptop, desktop ou um servidor de datacenter.
    - A solução fornece o acompanhamento ponta a ponta por meio do Portal do Azure.    

- **Seguro** - O Data Box Disk tem proteções de segurança internas para os discos, dados e serviço. 
    - Os discos são resistente a adulterações e oferecem suporte à atualização segura. 
    - Os dados nos discos são sempre protegidos por uma criptografia AES de 128 bits. 
    - Os discos só podem ser desbloqueados com uma chave fornecida no Portal do Azure. 
    - O serviço é protegido pelos recursos de segurança do Azure. 
    - Após o carregamento dos seus dados no Azure, os discos são limpos, conforme os padrões NIST 800-88r1.  
    
Para saber mais, acesse [Segurança e proteção de dados do Azure Data Box Disk](data-box-disk-security.md).


## <a name="features-and-specifications"></a>Recursos e especificações


| Especificações                                          | DESCRIÇÃO              |
|---------------------------------------------------------|--------------------------|
| Peso                                                  | < 0,90 kg. por caixa. Até cinco discos na caixa                |
| Dimensões                                              | Disco - SSD de 2,5" |            
| Cabos                                                  | 1 cabo USB 3.1 por disco|
| Capacidade de armazenamento por pedido                              | 40 TB (~ 35 TB utilizáveis)|
| Capacidade de armazenamento de disco                                   | 8 TB (~ 7 TB utilizáveis)|
| Interface de dados                                          | USB   |
| Segurança                                                | Previamente criptografado usando o BitLocker e atualização segura <br> Discos protegidos por chave de acesso <br> Dados sempre criptografados  |
| Taxa de transferência de dados                                      | até 430 MBps, dependendo do tamanho do arquivo      |
|Gerenciamento                                               | Portal do Azure |


## <a name="region-availability"></a>Disponibilidade de região

Durante a versão prévia, o Data Box Disk pode transferir dados para as seguintes regiões do Azure:


|Região do Azure  |Região do Azure  |
|---------|---------|
|Centro-Oeste dos EUA     |Centro-Norte dos EUA         |        
|Oeste dos EUA 2     | Leste dos EUA          |     
|Oeste dos EUA     | Leste dos EUA 2           |      
|Centro-Sul dos Estados Unidos   | Europa Ocidental       |         
|Centro dos EUA     | Norte da Europa |



## <a name="pricing"></a>Preços

Durante a versão prévia, o Data Box Disk fica disponível gratuitamente. Isso mudará quando o Data Box Disk estiver disponível para todos.

## <a name="next-steps"></a>Próximas etapas

- Examine os [requisitos do Data Box Disk](data-box-disk-system-requirements.md).
- Entenda os [limites do Data Box Disk](data-box-disk-limits.md).
- Implante rapidamente o [Azure Data Box Disk](data-box-disk-quickstart-portal.md) no Portal do Azure.