---
title: Visão geral de pré-requisitos para usar o Serviço de Migração de Banco de Dados do Azure | Microsoft Docs
description: Saiba mais sobre uma visão geral dos pré-requisitos para usar o Serviço de Migração de Banco de Dados do Azure para executar migrações de banco de dados.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 05/29/2019
ms.openlocfilehash: 4e21014f7b4ed86846a100ed9a2b1cd4b0400974
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304268"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Visão geral de pré-requisitos para usar o Serviço de Migração de Banco de Dados do Azure

Existem vários pré-requisitos necessários para garantir que o serviço de migração de banco de dados do Azure é executado sem problemas ao executar migrações de banco de dados. Alguns dos pré-requisitos se aplicam em todos os cenários (pares de origem e destino) com suporte do serviço, enquanto outros pré-requisitos são exclusivos para um cenário específico.

Os pré-requisitos associados a usar o Serviço de Migração de Banco de Dados do Azure estão listados nas seções a seguir.

## <a name="prerequisites-common-across-migration-scenarios"></a>Pré-requisitos comuns em cenários de migração

Os pré-requisitos do Serviço de Migração de Banco de Dados do Azure que são comuns a todos os cenários de migração compatíveis incluem a necessidade de:

* Criar uma VNET (Rede Virtual) do Azure para o Serviço de Migração de Banco de Dados do Azure usando o modelo de implantação do Azure Resource Manager, que fornece conectividade site a site aos servidores de origem locais usando o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou a [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Certifique-se de que suas regras de grupo de segurança da rede (NSG) da rede virtual não bloqueiam as seguintes portas de comunicação 443, 53, 9354, 445 e 12000. Veja mais detalhes sobre a filtragem de tráfego do NSG da VNet do Azure no artigo [Filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Ao usar um dispositivo de firewall na frente de seus bancos de dados de origem, talvez seja necessário adicionar regras de firewall para permitir que o Serviço de Migração de Banco de Dados do Azure acesse os bancos de dados de origem para migração.
* Configurar o [Firewall do Windows para acesso ao mecanismo de banco de dados](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Habilitar o protocolo TCP/IP, que está desabilitado por padrão durante a instalação do SQL Server Express, seguindo as instruções no artigo [Habilitar ou desabilitar um protocolo de rede de servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).

    > [!IMPORTANT]
    > Criando uma instância do serviço de migração de banco de dados do Azure requer acesso às configurações de rede virtual que normalmente não estão dentro do mesmo grupo de recursos. Como resultado, o usuário criando uma instância do DMS requer a permissão no nível de assinatura. Para criar as funções necessárias, o que você pode atribuir conforme necessário, execute o seguinte script:
    >
    > ```
    >
    > $readerActions = `
    > "Microsoft.DataMigration/services/*/read", `
    > "Microsoft.Network/networkInterfaces/ipConfigurations/read"
    >
    > $writerActions = `
    > "Microsoft.DataMigration/services/*/write", `
    > "Microsoft.DataMigration/services/*/delete", `
    > "Microsoft.DataMigration/services/*/action"
    >
    > $writerActions += $readerActions
    >
    > # TODO: replace with actual subscription IDs
    > $subScopes = ,"/subscriptions/00000000-0000-0000-0000-000000000000/","/subscriptions/11111111-1111-1111-1111-111111111111/"
    >
    > function New-DmsReaderRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Reader"
    > $aRole.Description = "Lets you perform read only actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    >
    > $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    >
    > function New-DmsContributorRole() {
    > $aRole = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
    > $aRole.Name = "Azure Database Migration Contributor"
    > $aRole.Description = "Lets you perform CRUD actions on DMS service/project/tasks."
    > $aRole.IsCustom = $true
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    >
    >   $aRole.AssignableScopes = $subScopes
    > #Create the role
    > New-AzRoleDefinition -Role $aRole
    > }
    > 
    > function Update-DmsReaderRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Reader"
    > $aRole.Actions = $readerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > function Update-DmsConributorRole() {
    > $aRole = Get-AzRoleDefinition "Azure Database Migration Contributor"
    > $aRole.Actions = $writerActions
    > $aRole.NotActions = @()
    > Set-AzRoleDefinition -Role $aRole
    > }
    >
    > # Invoke above functions
    > New-DmsReaderRole
    > New-DmsContributorRole
    > Update-DmsReaderRole
    > Update-DmsConributorRole
    > ```

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Pré-requisitos para migração do SQL Server para o Banco de Dados SQL do Azure

Além dos pré-requisitos do Serviço de Migração de Banco de Dados do Azure que são comuns a todos os cenários de migração, também há pré-requisitos que se aplicam especificamente para um cenário ou outro.

Ao usar o Serviço de Migração de Banco de Dados do Azure para executar migrações do SQL Server para Banco de Dados SQL do Azure, além dos pré-requisitos que são comuns a todos os cenários de migração, certifique-se de atender aos seguintes pré-requisitos adicionais:

* Criar uma instância do Banco de Dados SQL do Azure seguindo os detalhes no artigo [Criar um banco de dados SQL do Azure no portal do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
* Fazer download e instalar o [Assistente de migração de dados](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 ou posterior.
* Abra o Firewall do Windows para permitir que o Serviço de Migração de Banco de Dados do Azure acesse o SQL Server de origem, que por padrão é porta TCP 1433.
* Se você estiver executando vários usando portas dinâmicas de instâncias nomeadas do SQL Server, talvez você queira habilitar o serviço navegador do SQL e permitir o acesso à porta UDP 1434 por meio de seus firewalls para que o Serviço de Migração do Banco de Dados do Azure possa se conectar a uma instância nomeada em seu código-fonte servidor.
* Crie uma [regra de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) no nível de servidor para o servidor do Banco de Dados SQL do Azure para permitir o acesso do Serviço de Migração de Banco de Dados do Azure aos bancos de dados de destino. Forneça o intervalo de sub-redes da VNET usado para o Serviço de Migração de Banco de Dados do Azure.
* Garantir que as credenciais usadas para se conectar à instância de origem do SQL Server tenham as permissões [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql).
* Garantir que as credenciais usadas para se conectar para direcionar a instância do Banco de Dados SQL do Azure tenham a permissão CONTROL DATABASE nos bancos de dados de destino do SQL do Azure.

   > [!NOTE]
   > Para obter uma lista completa de pré-requisitos necessários para usar o Serviço de Migração de Banco de Dados do Azure para executar migrações do SQL Server para o Banco de Dados do SQL Azure, consulte o tutorial [Migrar o SQL Server para o Banco de Dados SQL do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-an-azure-sql-database-managed-instance"></a>Pré-requisitos para migração do SQL Server para uma instância gerenciada do banco de dados SQL

* Criar uma instância gerenciada do banco de dados de SQL do Azure seguindo os detalhes no artigo [criar um banco de dados de instância gerenciada do SQL no portal do Azure](https://aka.ms/sqldbmi).
* Abra os firewalls para permitir o tráfego SMB na porta 445 para o intervalo de sub-rede ou endereço IP do Serviço de Migração de Banco de Dados do Azure.
* Abra o Firewall do Windows para permitir que o Serviço de Migração de Banco de Dados do Azure acesse o SQL Server de origem, que por padrão é porta TCP 1433.
* Se você estiver executando vários usando portas dinâmicas de instâncias nomeadas do SQL Server, talvez você queira habilitar o serviço navegador do SQL e permitir o acesso à porta UDP 1434 por meio de seus firewalls para que o Serviço de Migração do Banco de Dados do Azure possa se conectar a uma instância nomeada em seu código-fonte servidor.
* Verificar se os logons usados para conectar o SQL Server de origem e a Instância Gerenciada de destino são membros da função de servidor sysadmin.
* Criar um compartilhamento de rede que pode ser usado pelo Serviço de Migração de Banco de Dados do Azure para fazer backup do banco de dados de origem.
* Certifique-se de que a conta de serviço que executa a instância do SQL Server de origem tem privilégios de gravação no compartilhamento de rede que você criou e que a conta de computador do servidor de origem tem acesso de leitura/gravação para o mesmo compartilhamento.
* Anote um usuário do Windows (e a senha) que tem privilégios de controle total no compartilhamento de rede criado anteriormente. O Serviço de Migração de Banco de Dados do Azure representa a credencial do usuário para carregar os arquivos de backup no contêiner de armazenamento do Azure para a operação de restauração.
* Crie um contêiner de blobs e recupere seu URI SAS usando as etapas descritas no artigo [Gerenciar os recursos do Armazenamento de Blobs do Azure com o Gerenciador de Armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Selecione todas as permissões (Leitura, Gravação, Exclusão, Lista) na janela da política ao criar o URI SAS.

   > [!NOTE]
   > Para obter uma listagem completa dos pré-requisitos necessários para usar o Serviço de Migração de Banco de Dados do Azure para executar migrações do SQL Server para a Instância Gerenciada do Banco de Dados SQL do Azure, consulte o tutorial [Migrar o SQL Server para a Instância Gerenciada do Banco de Dados SQL do Azure](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Próximas etapas

Para obter uma visão geral do Serviço de Migração de Banco de Dados do Azure e da disponibilidade regional, consulte o artigo [ O que é o Serviço de Migração de Banco de Dados do Azure ](dms-overview.md).
