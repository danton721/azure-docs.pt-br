---
title: Criar e gerenciar regras de firewall no banco de dados do Azure para PostgreSQL – único servidor usando a CLI do Azure
description: Este artigo descreve como criar e gerenciar regras de firewall no banco de dados do Azure para PostgreSQL – único servidor usando a linha de comando da CLI do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 03b1c215994e4089ad0aed4eac3868b05c564c4c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067527"
---
# <a name="create-and-manage-firewall-rules-in-azure-database-for-postgresql---single-server-using-azure-cli"></a>Criar e gerenciar regras de firewall no banco de dados do Azure para PostgreSQL – único servidor usando a CLI do Azure
Regras de firewall de nível de servidor podem ser usadas para gerenciar o acesso a um banco de dados do Azure para servidor PostgreSQL de um endereço IP específico ou intervalo de endereços IP. Usando comandos convenientes da CLI do Azure, você pode criar, atualizar, excluir, listar e mostrar as regras de firewall para gerenciar o servidor. Para uma visão geral do banco de dados PostgreSQL para regras de firewall, consulte [banco de dados do Azure para regras de firewall de servidor PostgreSQL](concepts-firewall-rules.md).

Regras de VNet (rede) virtuais também podem ser usadas para proteger o acesso ao seu servidor. Saiba mais sobre [criando e gerenciando a rede Virtual regras usando a CLI do Azure e pontos de extremidade de serviço](howto-manage-vnet-using-cli.md).

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de instruções, você precisa:
- Instalar o utilitário de linha de comando [CLI do Azure](/cli/azure/install-azure-cli) ou usar o Azure Cloud Shell no navegador.
- Um [banco de dados e servidor do Banco de Dados do Azure para PostgreSQL](quickstart-create-server-database-azure-cli.md).

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Configurar regras de firewall para o Banco de Dados do Azure para PostgreSQL
Os comandos de [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) são usados para configurar regras de firewall.

## <a name="list-firewall-rules"></a>Listar regras de firewall 
Para listar as regras de firewall existentes no servidor, execute o comando [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule).
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver
```
A saída listará as regras de firewall, se houver, no formato JSON por padrão. Você pode usar a opção `--output table` para obter um formato de tabela mais legível como saída.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mydemoserver --output table
```
## <a name="create-firewall-rule"></a>Criar uma regra de firewall
Crie uma regra de firewall no nível de servidor do PostgreSQL do Azure com o comando [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule). 

```
To allow access to a singular IP address, provide the same address in the `--start-ip-address` and `--end-ip-address`, as in this example, replacing the IP shown here with your specific IP.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Para permitir que aplicativos dos endereços IP do Azure conectem-se ao seu Banco de Dados do Azure para servidor PostgreSQL, forneça o endereço IP 0.0.0.0 como IP Inicial e IP Final, como neste exemplo.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mydemoserver --name AllowAllAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Esta opção configura o firewall para permitir todas as conexões do Azure, incluindo as conexões das assinaturas de outros clientes. Ao selecionar essa opção, verifique se as permissões de logon e de usuário limitam o acesso somente a usuários autorizados.
> 

Após o êxito, a saída do comando listará os detalhes da regra do firewall que você criou, em formato JSON por padrão. Em caso de falha, a saída mostrará uma mensagem de erro.

## <a name="update-firewall-rule"></a>Atualizar uma regra de firewall 
Atualize uma regra de firewall existente no servidor usando o comando [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule). Forneça o nome da regra de firewall existente como entrada, e os atributos IP inicial e IP final para atualizar.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.0
```
Após o êxito, a saída do comando listará os detalhes da regra do firewall que você atualizou, em formato JSON por padrão. Em caso de falha, a saída mostrará uma mensagem de erro.
> [!NOTE]
> Se a regra de firewall não existir, ela será criada pelo comando de atualização.

## <a name="show-firewall-rule-details"></a>Mostrar detalhes da regra de firewall
Também é possível exibir os detalhes de uma regra de firewall existente no nível do servidor executando o comando [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule).
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Após o êxito, a saída do comando listará os detalhes da regra do firewall que você especificou, em formato JSON por padrão. Em caso de falha, a saída mostrará uma mensagem de erro.

## <a name="delete-firewall-rule"></a>Excluir regra de firewall
Para revogar o acesso para um intervalo IP para o servidor, exclua uma regra de firewall existente executando o comando [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule). Forneça o nome da regra de firewall existente.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mydemoserver --name AllowIpRange
```
Após o êxito, não haverá saída. Em caso de falha, o texto da mensagem de erro retornará.

## <a name="next-steps"></a>Próximas etapas
- Da mesma forma, é possível usar um navegador da Web para [Criar e gerenciar as regras de firewall do Banco de Dados do Azure para PostgreSQL usando o Portal do Azure](howto-manage-firewall-using-portal.md).
- Entenda mais sobre as [Regras de firewall do servidor de Banco de Dados do Azure para PostgreSQL](concepts-firewall-rules.md).
- Proteger ainda mais o acesso ao seu servidor pelo [criando e gerenciando a rede Virtual regras usando a CLI do Azure e pontos de extremidade de serviço](howto-manage-vnet-using-cli.md).
- Para obter ajuda com a conexão com um servidor do Banco de Dados do Azure para PostgreSQL, consulte [Bibliotecas de conexão para o Banco de Dados do Azure para PostgreSQL](concepts-connection-libraries.md).
