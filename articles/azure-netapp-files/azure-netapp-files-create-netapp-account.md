---
title: Criar uma conta do NetApp para acesso do Azure NetApp Files | Microsoft Docs
description: Descreve como acessar Azure NetApp Files e criar uma conta do NetApp para que você pode configurar um pool de capacidade e criar um volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010337"
---
# <a name="create-a-netapp-account"></a>Criar uma conta do NetApp
Criar uma conta do NetApp permite que você configure um pool de capacidade e, subsequentemente, crie um volume. Você pode usar a folha de Azure NetApp Files para criar uma nova conta do NetApp.

## <a name="before-you-begin"></a>Antes de começar
Você deve ter sido na lista de permissões para acessar o Provedor de Recursos Azure Microsoft.NetApp e configurado para usar o serviço Azure NetApp Files.  

[Página de inscrição na versão prévia pública do Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Etapas 

1. Localize a URL da versão prévia do portal do Azure do convite de versão prévia e entrar no portal. 
2.  Acesse a folha Azure NetApp Files usando um dos seguintes métodos:  
  * Procure os **Azure NetApp Files** na caixa de pesquisa do portal do Azure.  
  * Clique em **Todos os serviços** na navegação e, em seguida, filtre para o Azure NetApp Files.  

  Você pode "favoritar" a folha Azure NetApp Files clicando no ícone de estrela ao lado dele. 

3. Clique em **+ Adicionar** para criar uma nova conta do NetApp.  
  A janela Nova conta do NetApp é exibida.  

4. Forneça as seguintes informações para sua conta do NetApp: 
  * **Nome da conta**  
    Especifique um nome exclusivo para a assinatura.
  *  **Assinatura**  
    Selecione uma assinatura de suas assinaturas existentes.
  * **Grupo de recursos**   
    Use um Grupo de Recursos existente ou crie um novo.
  * **Localidade**  
    Selecione a região onde deseja que a conta e seus recursos filho a ser localizado.  
    Atualmente, o serviço Azure NetApp Files tem suporte somente na região Leste dos EUA.  

    ![Nova conta do NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Clique em **Criar**.     
  A conta do NetApp que você criou agora aparece na folha Azure NetApp Files. 

## <a name="next-steps"></a>Próximas etapas  

1. [Configurar um pool de capacidade](azure-netapp-files-set-up-capacity-pool.md)
2. [Criar um volume do Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Configurar a política de exportação para um volume (opcional)](azure-netapp-files-configure-export-policy.md)