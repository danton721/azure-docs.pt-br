---
title: Exemplos de modelo do Azure Resource Manager para rede virtual | Microsoft Docs
description: Saiba mais sobre modelos diferentes do Azure Resource Manager disponíveis para implantação de redes virtuais do Azure.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: c4d0c65cfae0bb323978f4143b0058071562ddb6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682516"
---
# <a name="azure-resource-manager-template-samples-for-virtual-network"></a>Exemplos de modelo do Azure Resource Manager para rede virtual

A tabela a seguir contém links para exemplos de modelos do Azure Resource Manager. Você pode implantar os modelos usando o [Portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), a [CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) do Azure ou o Azure [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Para aprender a criar seus próprios modelos, consulte [Criar seu primeiro modelo](../azure-resource-manager/resource-manager-create-first-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e [Entender a estrutura e a sintaxe de modelos do Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Para obter a sintaxe JSON e as propriedades a serem usadas em modelos, confira [Tipos de recurso de Microsoft.Network](/azure/templates/microsoft.network/allversions).


| | |
|----|----|
|[Criar uma rede virtual com duas sub-redes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets)| Crie uma rede virtual com duas sub-redes.|
|[Rotear o tráfego por meio de uma solução de virtualização de rede](https://github.com/Azure/azure-quickstart-templates/tree/master/201-userdefined-routes-appliance)| Crie uma rede virtual com três sub-redes. Implanta uma máquina virtual em cada uma das sub-redes. Cria uma tabela de rota que contém rotas para direcionar o tráfego de uma sub-rede para outra, por meio da máquina virtual na terceira sub-rede. Associe a tabela de rotas a uma das sub-redes.|
|[Criar um ponto de extremidade de serviço de rede virtual para o Armazenamento do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-2subnets-service-endpoints-storage-integration)|Crie uma nova rede virtual com duas sub-redes e uma interface de rede em cada sub-rede. Habilita um ponto de extremidade de serviço do Armazenamento do Azure para uma das sub-redes e protege uma nova conta de armazenamento para essa sub-rede.|
|[Conectar duas redes virtuais](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vnet-to-vnet-peering)| Cria duas redes virtuais e um emparelhamento de rede virtual entre elas.|
|[Criar uma máquina virtual com vários endereços IP](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-ipconfig)| Crie uma VM Windows ou Linux com vários endereços IP.|
|[Configurar rede virtual de pilha dual de IPv4 + IPv6](https://github.com/Azure/azure-quickstart-templates/tree/master/ipv6-in-vnet)|Implanta a rede virtual de pilha dual (IPv4 + IPv6) com duas VMs e um Azure Load Balancer básico com endereços IP públicos do IPv4 e IPv6. |
