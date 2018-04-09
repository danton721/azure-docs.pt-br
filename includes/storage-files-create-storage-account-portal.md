---
title: storage-files-create-storage-account-portal
description: Como criar uma nova conta de armazenamento para arquivos do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 82e141eb4941dbd58be95e19dae186acd18caf94
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/29/2018
---
Uma conta de armazenamento é um pool compartilhado de armazenamento no qual você pode implantar um compartilhamento de arquivos do Azure ou outros recursos de armazenamento como blobs ou filas. Uma conta de armazenamento pode conter um número ilimitado de compartilhamentos, e um compartilhamento pode conter um número ilimitado de arquivos, até os limites de capacidade da conta de armazenamento.

Para criar uma conta de armazenamento:

1. No menu à esquerda, clique em **+** para criar um recurso.
2. Digite **conta de armazenamento** na caixa de pesquisa (1) e selecione **Conta de armazenamento - blob, arquivo, tabela, fila** (2), depois clique em **Criar**.
    ![Uma captura de tela de como a entrada da conta de armazenamento se parecer na caixa de diálogo de pesquisa de recursos](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. Em **Nome**, digite *mystorageacct*, seguido por alguns números aleatórios até aparecer a marca de seleção verde, indicando que é um nome exclusivo. Um nome de conta de armazenamento deve conter apenas letras minúsculas e ser globalmente exclusivo. Anote o nome da sua conta de armazenamento porque você vai usá-lo mais tarde. 
4. Em **Modelo de implantação**, deixe o valor padrão de **Gerenciador de recursos**. Para saber mais sobre as diferenças entre o Azure Resource Manager e a implantação clássica, consulte [Entender os modelos de implantação e o estado de seus recursos](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. Em **Tipo de conta**, selecione **StorageV2**. Para saber mais sobre os diferentes tipos de contas de armazenamento, consulte [Noções básicas de contas de armazenamento do Azure](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. Em **Desempenho**, mantenha o valor padrão de **Armazenamento padrão**. Os arquivos do Azure atualmente oferecem suporte apenas ao armazenamento padrão. Mesmo que você selecione o armazenamento premium, o compartilhamento de arquivos será armazenado no armazenamento padrão.
7. Em **Replicação**, selecione *Armazenamento com redundância local (LRS)*. 
8. Em **Transferência segura necessária**, é recomendável sempre selecionar **Habilitado**. Para saber mais sobre essa opção, consulte [Noções básicas sobre criptografia em trânsito](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. Em **Assinatura**, selecione a assinatura na qual criar a conta de armazenamento. Se tiver apenas uma assinatura, ela deve ser a padrão.
10. Em **Grupo de recursos**, selecione **Criar novo** e digite *myResourceGroup* como o nome.
11. Em **Localização**, selecione **Leste dos EUA**.
12. Em **Redes virtuais**, deixe a opção padrão como *Desabilitado*. 
13. Selecione **Fixar no painel** para tornar o armazenamento de conta mais fácil de ser encontrado.
14. Ao terminar, clique em **Criar** para iniciar a implantação.