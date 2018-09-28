---
title: Tutorial – criar e gerenciar dados exportados do Gerenciamento de Custos do Azure | Microsoft Docs
description: Este artigo mostra como você pode criar e gerenciar dados exportados do Gerenciamento de Custos do Azure para que você possa usá-los em sistemas externos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/13/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b1c7323dc108f5ee7a8bf6075abfe2a3e2fb9810
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953133"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: criar e gerenciar dados exportados

Se leu o tutorial de análise de custo, você está familiarizado com o download manual dos dados do Gerenciamento de Custos. No entanto, você pode criar uma tarefa recorrente diária, que exporta automaticamente seus dados do Gerenciamento de Custos com essa frequência para o Armazenamento do Azure. Os dados exportados estão no formato CSV e contém todas as informações que são coletadas pelo Gerenciamento de Custos. Em seguida, você pode usar os dados exportados no Armazenamento do Azure com sistemas externos e combiná-los com seus próprios dados personalizados. Você também pode usar os dados exportados em um sistema externo, tal como um painel ou outro sistema financeiro.

Os exemplos neste tutorial orientam você ao exportar os dados do Gerenciamento de Custos e, em seguida, verificar se eles foram exportados com êxito.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar uma exportação diária
> * Verificar se os dados são coletados

## <a name="prerequisites"></a>Pré-requisitos

- Conta de Contrato Enterprise do Azure
- Conta de Armazenamento do Azure configurada para armazenamento de blobs ou de arquivos

## <a name="sign-in-to-azure"></a>Entrar no Azure
Entre no Portal do Azure em [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Criar uma exportação diária

Gerenciamento de Custos + Cobrança &gt; selecione uma assinatura ou um grupo de recursos em uma assinatura &gt; Exportar &gt; **Adicionar**.

Digite um nome para a exportação e especifique a assinatura, a conta de armazenamento do Azure, o contêiner e o diretório de armazenamento de arquivos/contêiner de blobs e, em seguida, clique em **Criar**.

![Nova exportação](./media/tutorial-export-acm-data/new-export01.png)

Sua nova exportação aparece na lista de exportações. Por padrão, as novas exportações estão habilitadas e são executadas diariamente. Se você quiser desabilitar ou excluir uma exportação agendada, clique em qualquer item na lista e, em seguida, clique em **Desabilitar** ou **Excluir**.

Inicialmente, pode levar uma ou duas horas até que a exportação seja executada. No entanto, pode levar até quatro horas para que os dados sejam mostrados em arquivos exportados.

## <a name="verify-that-data-is-collected"></a>Verificar se os dados são coletados

Você pode verificar facilmente se os dados do Gerenciamento de Custos estão sendo coletados e exibir o arquivo CSV exportado usando o Gerenciador de Armazenamento do Azure.

Na lista de exportação, clique no nome da conta de armazenamento. Na página da conta de armazenamento, clique em Abrir no Gerenciador. Se você vir uma caixa de confirmação, clique em **Sim** para abrir o arquivo no Gerenciador de Armazenamento do Azure.

![Página da conta de armazenamento](./media/tutorial-export-acm-data/storage-account-page.png)

No Gerenciador de Armazenamento, navegue até o contêiner que você deseja abrir e selecione a pasta que corresponde ao mês atual. É mostrada uma lista de arquivos CSV. Selecione um deles e clique em **Abrir**.

![Gerenciador de Armazenamento](./media/tutorial-export-acm-data/storage-explorer.png)

O arquivo é aberto com o programa ou aplicativo que está configurado para abrir as extensões de arquivo CSV. Aqui está um exemplo no Excel.

![Dados de exportação de exemplo](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Acessar dados exportados de outros sistemas

Uma das finalidades de exportar os dados do Gerenciamento de Custos é acessar os dados de sistemas externos. Você pode usar um sistema de painéis ou outro sistema financeiro. Tais sistemas variam amplamente, portanto, mostrar um exemplo não seria conveniente.  No entanto, você pode obter uma introdução sobre como acessar os dados dos aplicativos na [Introdução ao Armazenamento do Azure](../storage/common/storage-introduction.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar uma exportação diária
> * Verificar se os dados são coletados

Avance para o próximo tutorial para otimizar e melhorar a eficiência, identificando recursos ociosos e subutilizados.

> [!div class="nextstepaction"]
> [Examinar recomendações de otimização e agir com base nelas](tutorial-acm-opt-recommendations.md)