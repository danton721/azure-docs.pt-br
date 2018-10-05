---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 6a64d85cc476c7494a1730959b96e9480115cd90
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47045677"
---
Galeria de Imagens Compartilhadas é um serviço que ajuda você a criar a estrutura e a organização em torno de suas imagens de VM personalizadas. A Galeria de Imagens Compartilhadas fornece três propostas de valor principais
- Gerenciamento simples
- Dimensionar suas imagens do cliente
- Compartilhar suas imagens – compartilhe suas imagens com diferentes usuários, entidades de serviço ou grupos do AD na sua organização, bem como em diferentes regiões, usando a replicação de várias regiões

Uma imagem gerenciada é uma cópia de uma VM completa (incluindo quaisquer discos de dados anexados) ou apenas o disco do SO, dependendo de como você cria a imagem. Quando você cria uma VM da imagem, a cópia dos VHDs na imagem é usada para criar os discos para a nova VM. A imagem gerenciada permanece no armazenamento e pode ser usada repetidamente para criar novas VMs.

Se você tiver um grande número de imagens gerenciadas que precise manter e gostaria de disponibilizá-los em toda a empresa, poderá usar uma galeria de imagem compartilhada como um repositório que facilite a atualização e o compartilhamento das suas imagens. Os encargos para usar uma galeria de imagens compartilhadas são apenas os custos para o armazenamento usado pelas imagens, além de quaisquer custos de egresso de rede para replicar imagens da região de origem para as regiões publicadas.

O recurso Galeria de Imagens Compartilhadas tem vários tipos de recursos:

| Recurso | DESCRIÇÃO|
|----------|------------|
| **Imagem gerenciada** | Esta é uma imagem de linha de base que pode ser usada sozinha ou para criar diversas **versões de imagem compartilhada** em uma galeria de imagens.|
| **Galeria de imagens** | Como o Azure Marketplace, uma **galeria de imagens** é um repositório para gerenciar e compartilhar imagens, mas você controla quem tem acesso. |
| **Imagem da galeria** | As imagens são definidas dentro de uma galeria e transportam informações sobre a imagem e os requisitos para usá-la internamente. Isso inclui se a imagem é Windows ou Linux, notas sobre a versão e requisitos mínimos e máximos de memória. Esse tipo de imagem é um recurso dentro do modelo de implantação do Resource Manager, mas não é usado diretamente para a criação de VMs. É uma definição de um tipo de imagem. |
| **Versão de imagem compartilhada** | Uma **versão da imagem** é usada para criar uma VM ao usar uma galeria. Você pode ter diversas versões de uma imagem conforme necessário para seu ambiente. Como uma imagem gerenciada, quando você usa uma **versão da imagem** para criar uma VM, a versão da imagem é usada para criar novos discos para a VM. Versões de imagem podem ser usadas várias vezes. |

<br>


![Gráfico mostrando como você pode ter várias versões de uma imagem na sua galeria](./media/shared-image-galleries/shared-image-gallery.png)

### <a name="regional-support"></a>Suporte regional

O suporte regional para galerias de imagens compartilhadas é limitado, mas será expandido ao longo do tempo. Para a versão prévia, aqui estão as listas de locais em que você pode criar galerias e regiões em que se pode replicar qualquer galeria: 

| Criar Galeria em  | Replicar Versão para |
|--------------------|----------------------|
| Centro-Oeste dos EUA    |Centro-Sul dos Estados Unidos|
| Leste dos EUA 2          |Leste dos EUA|
| Centro-Sul dos Estados Unidos   |Leste dos EUA 2|
| Sudeste Asiático     |Oeste dos EUA|
| Europa Ocidental        |Oeste dos EUA 2|
|                    |Centro dos EUA|
|                    |Centro-Norte dos EUA|
|                    |Canadá Central|
|                    |Leste do Canadá|
|                    |Norte da Europa|
|                    |Europa Ocidental|
|                    |Sul da Índia|
|                    |Sudeste Asiático|



## <a name="scaling"></a>Dimensionamento
Galeria de Imagens Compartilhadas permite que você especifique o número de réplicas que deseja que o Azure mantenha para as imagens. Isso ajuda em cenários de implantação de várias VMs, uma vez que as implantações de VM podem ser distribuídas para diferentes réplicas, reduzindo a chance de o processo de criação de instância ser limitado devido à sobrecarga de uma única réplica.

![Gráfico mostrando como você pode dimensionar imagens](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Replicação
A Galeria de Imagens Compartilhadas também permite replicar imagens para outras regiões do Azure automaticamente. Cada versão da imagem compartilhado pode ser replicada para regiões diferentes, dependendo do que faz sentido para a sua organização. Um exemplo é sempre replicar a imagem mais recente em várias regiões, enquanto todas as versões mais antigas só estão disponíveis em uma região. Isso pode ajudar a economizar nos custos de armazenamento para as versões de imagem Compartilhada. Uma versão da imagem Compartilhada replicada pode ser atualizada após o momento da criação. O tempo necessário para replicar para diferentes regiões depende da quantidade de dados sendo copiados e do número de regiões para as quais a versão é replica. Isso pode levar algumas horas em alguns casos. Enquanto a replicação está em andamento, você pode exibir o status da replicação por região. Depois que a replicação de imagem está concluída em uma região, você pode implantar uma VM ou VMSS usando essa versão na região.

![Gráfico mostrando como você pode replicar imagens](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access
Uma vez que galeria de imagens compartilhadas, imagem compartilhada e versão de imagem compartilhada são recursos, podem ser compartilhadas usando controles do Azure RBAC nativos internos. Usando RBAC, você pode compartilhar esses recursos com outros usuários, entidades de serviço e grupos em sua organização. O escopo de compartilhamento desses recursos está dentro do mesmo locatário do AD. Depois que um usuário tiver acessado a versão de imagem Compartilhada, ele poderá implantar uma VM ou um conjunto de dimensionamento de máquinas virtuais em qualquer uma das assinaturas às quais eles tenham acesso dentro do mesmo locatário do AD que a versão de imagem Compartilhada.  Aqui está a matriz de compartilhamento que ajuda a entender ao que o usuário obtém acesso:

| Compartilhado com o usuário     | Galeria de imagens compartilhadas | Imagem Compartilhada | Versão de imagem compartilhada |
|----------------------|----------------------|--------------|----------------------|
| Galeria de imagens compartilhadas | SIM                  | sim          | SIM                  |
| Imagem Compartilhada         | Não                    | sim          | SIM                  |
| Versão de imagem compartilhada | Não                    | Não            | SIM                  |



## <a name="billing"></a>Cobrança
Não há custo adicional para usar o serviço de Galeria de Imagens Compartilhadas. Você será cobrado pelos seguintes recursos:
- Custos de armazenamento para armazenar as versões da imagem Compartilhada. Depende do número de réplicas da versão e o número de regiões em para as quais a versão é replicada.
- Encargos de saída de rede para replicação da região de origem da versão para as regiões replicadas.

## <a name="frequently-asked-questions"></a>Perguntas frequentes 

**P.** Como faço para me inscrever para a Versão Prévia Pública da Galeria de Imagens Compartilhadas?
 
 a. Para se inscrever para a versão prévia pública da Galeria de Imagens Compartilhadas, você precisa se registrar para o recurso executando os comandos a seguir em cada uma das assinaturas em que pretende criar uma galeria de imagens compartilhadas, definição de imagem ou recursos de versão de imagem, e também em que pretende implantar Máquinas Virtuais usando as versões de imagem.

**CLI**: 

```bash 
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

**PowerShell**: 

```powershell
Register-AzureRmProviderFeature -FeatureName GalleryPreview -ProviderNamespace Microsoft.Compute
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

**P.** Como posso listar todos os recursos da Galeria de Imagens Compartilhadas entre assinaturas? 
 
 a. Para listar todos os recursos da Galeria de Imagens Compartilhadas entre assinaturas às quais você tem acesso no portal do Azure, siga as etapas abaixo:

 1. Abra o [Portal do Azure](https://portal.azure.com).
 1. Vá para **Todos os recursos**.
 1. Selecione todas as assinaturas sob as quais você gostaria de listar todos os recursos.
 1. Procure recursos do tipo **Galeria privada**.
 
 Para ver as definições de imagem e as versões da imagem, você também deve selecionar **Mostrar tipos ocultos**.
 
 Para listar todos os recursos da Galeria de Imagens Compartilhadas entre as assinaturas para as quais você tem permissões, use o seguinte comando na CLI do Azure:

 ```bash
 az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
 ```


**P.** Como faço para compartilhar minhas imagens entre assinaturas?
 
 a. Você pode compartilhar imagens entre assinaturas usando o RBAC (Controle de Acesso Baseado em Função). Qualquer usuário que tenha permissões de leitura para uma versão de imagem, mesmo entre assinaturas, poderá implantar uma máquina Virtual usando a versão da imagem.


**P.** Posso mover minha imagem existente para a galeria de imagens compartilhadas?
 
 a. Sim. Há três cenários com base nos tipos de imagens que você pode ter.

 Cenário 1: se você tiver uma imagem gerenciada, poderá criar uma definição de imagem e a versão da imagem usando essa definição.

 Cenário 2: se você tiver uma imagem generalizada não gerenciada, poderá criar uma imagem gerenciada com base nela e então criar uma definição de imagem e a versão da imagem com base nessa definição. 

 Cenário 3: se você tiver um VHD em seu sistema de arquivos local, precisará carregar o VHD, criar uma imagem gerenciada e então criar a definição da imagem e a versão da imagem com base nela. 
    - Se o VHD for de uma VM do Windows, veja [Carregar um VHD generalizado](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
    - Se o VHD for para uma VM do Linux, veja [Carregar um VHD](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**P.** Posso criar uma versão da imagem de um disco especializado?

 a. Não, no momento não damos suporte para discos especializados como imagens. Se você tiver um disco especializado, precisará [criar uma VM do VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) anexando o specializeddisk a uma nova VM. Depois de ter uma VM em execução, precisará seguir as instruções para criar uma imagem gerenciada usando a [VM do Windows] (https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-custom-images) ou a [VM do Linux](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-custom-images). Quando você tiver uma imagem gerenciada generalizada, poderá iniciar o processo para criar uma descrição da imagem compartilhada e a versão da imagem.


**P.** Posso criar uma galeria de imagens compartilhadas, definição de imagem e versão da imagem no portal do Azure?

 a. Não, no momento não damos suporte para a criação de nenhum dos recursos de Galeria de Imagens Compartilhadas por meio do portal do Azure. No entanto, nós oferecemos suporte para a criação de recursos da Galeria de Imagens Compartilhadas por meio da CLI, de Modelos e de SDKs. O PowerShell também será lançado em breve.

 
**P.** Depois de criada, posso atualizar a definição da imagem ou a versão da imagem? Que tipo de detalhes posso modificar?

 a. Os detalhes que podem ser atualizados em cada um dos recursos são mencionados a seguir:
 
Galeria de imagens compartilhadas:
- DESCRIÇÃO

definição de imagem:
- vCPUs recomendadas
- Memória
- DESCRIÇÃO
- Data de fim da vida útil

Versão da imagem:
- Contagem de réplicas regionais
- Regiões de destino
- Exclusão da mais recente
- Data de fim da vida útil


**P.** Depois de criada, posso mover o recurso de Galeria de Imagens Compartilhadas para uma assinatura diferente?

 a. Não, não é possível mover o recurso de Galeria de Imagens Compartilhadas para uma assinatura diferente. No entanto, você poderá replicar as versões da imagem na galeria para outras regiões conforme necessário.

**P.** É possível replicar minhas versões de imagem entre nuvens – 21Vianet do Azure China, Azure Alemanha e Nuvem do Azure Governamental? 

 a. Não, você não pode replicar as versões de imagem entre nuvens.

**P.** Posso replicar minhas versões de imagem entre assinaturas? 

 a. Não, você não pode replicar as versões de imagem entre regiões em uma assinatura e usá-las em outras assinaturas por meio de RBAC.

**P.** Posso compartilhar versões de imagem entre locatários do AD? 

 a. Não, no momento a galeria de imagens compartilhadas não oferece suporte para compartilhamento de versões de imagem entre locatários do AD. No entanto, você pode usar o recurso de ofertas privadas no Azure Marketplace para fazer isso.


**P.** Quanto tempo leva para replicar as versões de imagem entre todas as regiões de destino?

 a. O tempo de replicação de versão de imagem depende inteiramente do tamanho da imagem e do número de regiões para as quais ela está sendo replicada. No entanto, como uma melhor prática, é recomendável manter a imagem pequena e as regiões de origem e destino perto para melhores resultados. Você pode verificar o status da replicação usando o sinalizador -ReplicationStatus.


**P.** Quantas galerias de imagens compartilhadas eu posso criar em uma assinatura?

 a. A cota padrão é 
- 10 galerias de imagens compartilhada por assinatura por região
- 200 definições de imagem por assinatura por região
- 2.000 versões de imagem por assinatura por região


**P.** Qual é a diferença entre região de origem e região de destino?

 a. Região de origem é a região em que sua versão da imagem será criada e regiões de destino são as regiões em que uma cópia da sua versão da imagem será armazenada. Para cada versão de imagem, você pode ter apenas uma região de origem. Além disso, passe o local da região de origem como uma das regiões de destino ao criar uma versão da imagem.  


**P.** Como faço para especificar a região de origem ao criar a versão da imagem?

 a. Durante a criação de uma versão de imagem, você pode usar a marca **--location** na CLI e a marca **-Location** no PowerShell para especificar a região de origem. Verifique se a imagem gerenciada que você está usando como imagem base para criar a versão da imagem está no local em que você pretende criar a versão da imagem. Além disso, passe o local da região de origem como uma das regiões de destino ao criar uma versão da imagem.  


**P.** Como faço para especificar o número de réplicas de versão da imagem a serem criadas em cada região?

 a. Há duas maneiras de especificar o número de réplicas de versão da imagem a serem criadas em cada região:
 
1. A contagem de réplica regionais que especifica o número de réplicas que você deseja criar por região. 
2. A contagem de réplicas comuns, que é a contagem padrão por região caso a contagem de réplicas regionais não seja especificada. 

Para especificar a contagem de réplicas regionais, passe o local junto com o número de réplicas que você deseja criar nessa região desta maneira: "Centro-Sul dos EUA=2". 

Se a contagem de réplicas regionais não for especificada com cada local, o número de réplicas padrão será a contagem de réplicas comuns que você especificou. 

Para especificar a contagem de réplicas comuns na CLI, use o argumento **--replica-count** no comando `az sig image-version create`.


**P.** Posso criar a Galeria de Imagens Compartilhadas em um local diferente daquele em que desejo criar a definição de imagem e a versão da imagem?

 a. Sim, isso é possível. Porém, como uma melhor prática, incentivamos manter o grupo de recursos, a Galeria de Imagens Compartilhadas, a definição da imagem e a versão da imagem no mesmo local.


**P.** Quais são os encargos para usar a Galeria de Imagens Compartilhadas?

 a. Não há encargos para usar o serviço Galeria de Imagens Compartilhadas, exceto pelos encargos de armazenamento para armazenar as versões de imagem e os encargos de saída de rede para replicar as versões de imagem da região de origem para as regiões de destino.

**P.** Que versão de API devo usar para criar a Galeria de Imagens Compartilhadas, a definição de imagem, a versão da imagem e a VM/VMSS com base na Versão da Imagem?

 a. Para implantações de conjunto de dimensionamento de máquinas virtuais e VM usando uma versão de imagem, recomendamos usar a API versão 2018-04-01 ou superior. Para trabalhar com galerias de imagens compartilhadas, definições de imagem e versões de imagem, é recomendável usar a API versão 2018-06-01. 