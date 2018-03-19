---
title: Organizar seus recursos com Grupos de Gerenciamento do Azure | Microsoft Docs
description: "Saiba mais sobre os grupos de gerenciamento e como usá-los."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: rithorn
ms.openlocfilehash: 1264bf77b6d922f5beb22177d1ac63efa9386ef2
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizar seus recursos com grupos de gerenciamento do Azure 

Se você tiver várias assinaturas, poderá organizá-las em contêineres chamados "grupos de gerenciamento" para ajudá-lo a gerenciar o acesso, a política e a conformidade em suas assinaturas. Os grupos de gerenciamento fornecem gerenciamento de nível empresarial em larga escala, independentemente do tipo de assinaturas que você possa ter.  

O recurso do grupo de gerenciamento está disponível em uma visualização pública. Para começar a usar os grupos de gerenciamento, faça logo no [Portal do Azure](https://portal.azure.com) e procure **Grupos de Gerenciamento** na seção **Todos os serviços**. 

O suporte ao Azure Policy para grupos de gerenciamento ainda não está disponível na Visualização Pública, porém, a disponibilidade está prevista para as próximas semanas.  

Como exemplo, é possível aplicar políticas a um grupo de gerenciamento que limita as regiões disponíveis para a criação de VM (máquina virtual). Essa política seria aplicada a todos os grupos de gerenciamento, assinaturas e recursos nesse grupo de gerenciamento, permitindo que as VMs fossem criadas nessa região.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarquia de grupos de gerenciamento e assinaturas 

É possível compilar uma estrutura flexível de grupos de gerenciamento e assinaturas para organizar seus recursos em uma hierarquia para políticas unificadas e gerenciamento de acesso. O diagrama a seguir mostra uma hierarquia de exemplo que consiste em grupos de gerenciamento e assinaturas organizados por departamentos.    

![hierarquia](media/management-groups/MG_overview.png)

Ao criar uma hierarquia agrupada por departamentos, é possível atribuir funções de [RBAC (Controle de Acesso Baseado em Função do Azure)](../active-directory/role-based-access-control-what-is.md) que *herdam* dos departamentos sob esse grupo de gerenciamento. Ao usar grupos de gerenciamento, você pode reduzir sua carga de trabalho e reduzir o risco de erro, apenas tendo que atribuir a função uma vez. 

### <a name="important-facts-about-management-groups"></a>Fatos importantes sobre os grupos de gerenciamento
- 10.000 grupos de gerenciamento podem ter suporte em um único diretório. 
- Uma árvore do grupo de gerenciamento pode dar suporte a até seis níveis de profundidade.
    - Esse limite não inclui o nível Raiz ou o nível de assinatura.
- Cada grupo de gerenciamento somente pode dar suporte a um pai.
- Cada grupo de gerenciamento pode ter vários elementos filhos. 

## <a name="root-management-group-for-each-directory"></a>Grupo de gerenciamento raiz para cada diretório

Cada diretório recebe um único grupo de gerenciamento de nível superior chamado grupo de gerenciamento "Raiz". Esse grupo de gerenciamento raiz é compilado na hierarquia para que todos os grupos de gerenciamento e assinaturas sejam dobrados nele. Esse grupo de gerenciamento raiz permite que políticas globais e atribuições de RBAC sejam aplicadas no nível de diretório. O Administrador de Diretório [precisa elevar-se](../active-directory/role-based-access-control-tenant-admin-access.md) para ser o proprietário desse grupo raiz inicialmente. Quando o administrador for o proprietário do grupo, ele poderá atribuir qualquer função de RBAC a outros usuários ou grupos do diretório para gerenciar a hierarquia.  

### <a name="important-facts-about-the-root-management-group"></a>Fatos importantes sobre o grupo de gerenciamento raiz
- A ID e o nome do grupo de gerenciamento raiz recebem a ID do Azure Active Directory por padrão. O nome de exibição pode ser atualizado a qualquer momento para mostrar diferentes no Portal do Azure. 
- Todos os grupos de gerenciamento e assinaturas podem dobrar para o único grupo de gerenciamento raiz dentro do diretório.  
    - É recomendável que todos os itens do diretório sejam dobrados para o grupo de gerenciamento Raiz para o gerenciamento global.  
    - Durante a Visualização Pública, as assinaturas no diretório não são criadas automaticamente como filhos de raiz.   
    - Durante a Visualização Pública, novas assinaturas não são padronizadas automaticamente para o grupo de gerenciamento Raiz. 
- O grupo de gerenciamento raiz não pode ser movido ou excluído, ao contrário de outros grupos de gerenciamento. 
  
## <a name="management-group-access"></a>Acesso do Grupo de Gerenciamento

Os Grupos de Gerenciamento do Azure fornecem suporte para [RBAC (Controle de Acesso Baseado em Função do Azure)](../active-directory/role-based-access-control-what-is.md) a todos os acessos de recursos e definições de função. Essas permissões são herdadas de recursos filho existentes na hierarquia.   

Embora qualquer [função de RBAC interna](../active-directory/role-based-access-control-what-is.md#built-in-roles) possa ser atribuída a um grupo de gerenciamento, existem quatro funções normalmente utilizadas: 
- **proprietário** tem acesso total a todos os recursos, inclusive o direito de delegar acesso a outros usuários. 
- **Colaborador** pode criar e gerenciar todos os tipos de recursos do Azure, mas não pode conceder acesso a outras pessoas.
- **Colaborador da Política de Recursos** pode criar e gerenciar políticas no diretório nos recursos.     
- **leitor** pode exibir os recursos existentes do Azure. 


## <a name="next-steps"></a>Próximas etapas 
Para saber mais sobre grupos de gerenciamento, consulte: 
- [Criar grupos de gerenciamento para organizar recursos do Azure](management-groups-create.md)
- [Como alterar, excluir ou gerenciar grupos de gerenciamento](management-groups-manage.md)
- [Instalar o módulo Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Revisar as especificações API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Instalar a extensão CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
