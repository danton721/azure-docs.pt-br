---
title: Diagnosticar um problema de roteamento de rede de máquina virtual - CLI do Azure | Microsoft Docs
description: Neste artigo, você aprenderá a diagnosticar um problema de roteamento da rede de máquina virtual com a funcionalidade de próximo salto do Observador de Rede do Azure.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: fcb7ec2e40b5c0e8794d2f4d70395dcbecca019c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182156"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Diagnosticar um problema de roteamento de rede de máquina virtual - CLI do Azure

Neste artigo, você implanta uma VM (máquina virtual ) e, em seguida, verifica a comunicação com um endereço IP e uma URL. Você determina a causa de uma falha de comunicação e como resolvê-la.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Caso opte por instalar e usar a CLI localmente, este artigo exigirá que seja executada a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão instalada, execute `az --version`. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Depois de verificar a versão do CLI, execute `az login` para criar uma conexão com o Azure. Os comandos CLI neste artigo são formatados para serem executados em um shell Bash.

## <a name="create-a-vm"></a>Criar uma máquina virtual

Antes de criar uma VM, você deve criar um grupo de recursos para conter a VM. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo a seguir cria um grupo de recursos chamado *myResourceGroup* na localização *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Crie uma VM com [az vm create](/cli/azure/vm#az_vm_create). Se as chaves SSH ainda não existirem em uma localização de chave padrão, o comando criará. Para usar um conjunto específico de chaves, use a opção `--ssh-key-value`. O exemplo a seguir cria uma VM chamada *myVm*:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

A VM demora alguns minutos para criar. Não continue com as etapas restantes até que a VM seja criada e a CLI retorne a saída.

## <a name="test-network-communication"></a>Testar comunicação de rede

Para testar a comunicação de rede com o Observador de Rede, primeiro habilite um observador de rede na região onde está localizada a VM que deseja testar e, em seguida, use a funcionalidade de próximo salto do Observador de Rede para testar a comunicação.

### <a name="enable-network-watcher"></a>Habilitar o observador de rede

Se você já tiver um observador de rede habilitado na região Este dos EUA, vá para [Usar próximo salto](#use-next-hop). Use o comando [az network watcher configure](/cli/azure/network/watcher#az-network-watcher-configure) para criar um observador de rede na região East US:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Usar próximo salto

O Azure cria automaticamente as rotas para destinos padrão. Você pode criar rotas personalizadas que substituem as rotas padrão. Às vezes, as rotas personalizadas podem causar falha na comunicação. Para testar o roteamento de uma VM, use [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) para determinar o próximo salto de roteamento quando o tráfego for destinado a um endereço específico.

Teste a comunicação de saída da VM para um dos endereços IP para www.bing.com:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Após alguns segundos, a saída informa que o **nextHopType** é **Internet** e que a **routeTableId** é **Rota do Sistema**. Esse resultado permite que você saiba que há uma rota válida para o destino.

Teste a comunicação de saída da VM em 172.31.0.100:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

A saída retornada informa que **Nenhum** é o **nextHopType** e que a **routeTableId** também é **Rota do Sistema**. Esse resultado permite que você saiba que, embora haja uma rota do sistema válida para o destino, não há nenhum próximo salto para encaminhar o tráfego para o destino.

## <a name="view-details-of-a-route"></a>Exibir detalhes de uma rota

Para analisar o roteamento adicional, examine as rotas reais para a interface de rede com o comando [az network nic show-effective-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table):

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

O texto a seguir está incluído na saída retornada:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Quando você usou o comando `az network watcher show-next-hop` para testar a comunicação de saída a 13.107.21.200 em [Usar próximo salto](#use-next-hop), a rota com o **addressPrefix** 0.0.0.0/0** foi usada para encaminhar o tráfego para o endereço, pois nenhuma outra rota na saída inclui o endereço. Por padrão, todos os endereços não especificados dentro do prefixo de endereço de outra rota são encaminhados para a Internet.

Quando você usou o comando `az network watcher show-next-hop` para testar a comunicação de saída 172.31.0.100, no entanto, o resultado informou que não houve nenhum tipo de próximo salto. O texto a seguir também está na saída retornada:

```azurecli
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

Como você pode ver na saída do comando `az network watcher nic show-effective-route-table`, embora haja uma rota padrão para o prefixo 172.16.0.0/12, que inclui o endereço 172.31.0.100, o **nextHopType** é **Nenhum**. O Azure cria uma rota padrão para 172.16.0.0/12, mas não especifica um tipo de próximo salto até que haja um motivo. Se, por exemplo, você adicionou o intervalo de endereços 172.16.0.0/12 ao espaço de endereço da rede virtual, o Azure alterará o **nextHopType** para **Rede virtual** da rota. Em seguida, uma verificação mostra a **Rede virtual** como o **nextHopType**.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não for mais necessário, você poderá usar [az group delete](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos que ele contém:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você criou uma VM e o roteamento de rede diagnosticado da VM. Você aprendeu que o Azure cria várias rotas padrão e testou o roteamento para dois destinos diferentes. Saiba mais sobre o [roteamento no Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e como [criar rotas personalizadas](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Para conexões de VM de saída, determine também a latência e o tráfego de rede permitido e negado entre a VM e um ponto de extremidade, usando a funcionalidade [solução de problemas de conexão](network-watcher-connectivity-cli.md) do Observador de Rede. Você pode monitorar a comunicação entre uma VM e um ponto de extremidade, como um endereço IP ou uma URL, ao longo do tempo usando a funcionalidade de monitor de conexão do Observador de Rede. Para saber como, consulte [Monitorar uma conexão de rede](connection-monitor.md).