---
title: 'Tutorial: criar um Banco de Dados do Azure para MariaDB usando o portal do Azure'
description: Este tutorial explica como criar e gerenciar o banco de dados e o servidor do Banco de Dados do Azure para MariaDB usando o portal do Azure.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: tutorial
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 2e1cd0d28c544b2e4c5dc86c7a6db39a5d20c1ed
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991004"
---
# <a name="tutorial-design-an-azure-database-for-mariadb-database-using-the-azure-portal"></a>Tutorial: criar um Banco de Dados do Azure para MariaDB usando o portal do Azure
O Banco de Dados do Azure para MariaDB é um serviço gerenciado que permite executar, gerenciar e dimensionar bancos de dados altamente disponíveis do MySQL na nuvem. Usando o Portal do Azure, você pode gerenciar facilmente seu servidor e projetar um banco de dados.

Neste tutorial, você usará o Portal do Azure para aprender a:

> [!div class="checklist"]
> * Criar um Banco de Dados do Azure para MariaDB
> * Configurar o firewall do servidor
> * Usar a ferramenta de linha de comando do MySQL para criar um banco de dados
> * Carregar dados de exemplo
> * Consultar dados
> * Atualizar dados
> * Restaurar dados

## <a name="sign-in-to-the-azure-portal"></a>Entre no Portal do Azure
Abra seu navegador da Web favorito e visite o [portal do Microsoft Azure](https://portal.azure.com/). Insira suas credenciais para entrar no portal. A exibição padrão é o painel de serviço.

## <a name="create-an-azure-database-for-mariadb-server"></a>Criar um servidor do Banco de Dados do Azure para MariaDB
Um servidor do Banco de Dados do Azure para MariaDB é criado com um conjunto definido de recursos de computação e armazenamento <!--[compute and storage resources](./concepts-compute-unit-and-storage.md)-->. O servidor é criado dentro de um [Grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Selecione o botão **Criar um recurso** (+) no canto superior esquerdo do portal.

2. Digite **Banco de Dados do Azure para MariaDB** na caixa de pesquisa para localizar rapidamente o serviço.
   
   ![Navegar até o MySQL](./media/tutorial-design-database-using-portal/1-Navigate-to-mariadb.png)

3. Clique no bloco **Banco de Dados do Azure para MariaDB** e, em seguida, clique em **Criar**. Preencha o formulário do Banco de Dados do Azure para MariaDB.
   
   ![Criar formulário](./media/tutorial-design-database-using-portal/2-create-form.png)

    **Configuração** | **Valor sugerido** | **Descrição do campo** 
    ---|---|---
    Nome do servidor | Nome de servidor exclusivo | Escolha um nome exclusivo que identifique o servidor de Banco de Dados do Azure para MariaDB. Por exemplo, mydemoserver. O nome de domínio *.mariadb.database.azure.com* é acrescentado ao nome do servidor fornecido. O nome do servidor pode conter apenas letras minúsculas, números e o caractere de hífen (-). Ele deve conter de 3 a 63 caracteres.
    Assinatura | Sua assinatura | Selecione a assinatura do Azure que você deseja usar para o servidor. Se você tem várias assinaturas, escolha a assinatura na qual recebe a cobrança do recurso.
    Grupo de recursos | *myresourcegroup* | Forneça um novo nome de um grupo de recursos ou um existente.    Grupo de recursos|*myresourcegroup*| Um novo nome do grupo de recursos ou um existente de sua assinatura.
    Selecionar a origem | *Em branco* | Selecione *Em branco* para criar um novo servidor do zero. (Selecione *Backup* se você estiver criando um servidor de um backup geográfico de um servidor de Banco de Dados do Azure para MariaDB existente).
    Logon de administrador do servidor | myadmin | Uma conta de logon a ser usada ao se conectar ao servidor. O nome de logon do administrador não pode ser **azure_superusuário**, **admin**, **administrador**, **raiz**, **convidado** ou **público**.
    Senha | *Sua escolha* | Forneça uma nova senha para a conta do administrador do servidor. Ela deve conter de 8 a 128 caracteres. A senha deve conter caracteres de três das seguintes categorias: letras maiúsculas, letras minúsculas, números (0-9) e caracteres não alfanuméricos (!, $, #, % e assim por diante).
    Confirmar senha | *Sua escolha*| Confirme a senha da conta do administrador.
    Local padrão | *A região mais próxima de seus usuários*| Escolha o local mais próximo para os usuários ou para outros aplicativos do Azure.
    Versão | *A versão mais recente*| A versão mais recente (a menos que você tenha requisitos específicos que exijam uma outra versão).
    Tipo de preço | **Uso Geral**, **Gen 5**, **2 vCores**, **5 GB**, **7 dias**, **Com redundância geográfica** | As configurações de computação, armazenamento e backup para o novo servidor. Selecione **Tipo de preço**. Em seguida, selecione a guia **Uso Geral**. *Gen 5*, *2 vCores*, *5 GB*, e *7 dias* são os valores padrão para **Geração de Computação**, **vCore**, **Armazenamento** e **Período de Retenção de Backup**. Você pode deixar esses controles deslizantes como estão. Para habilitar os backups do servidor em armazenamento com redundância geográfica, selecione **Redundância Geográfica** das **Opções de Redundância de Backup**. Para salvar a seleção desse tipo de preço, selecione **OK**. A captura de tela a seguir demonstra essas seleções.
    
   ![Tipo de preço](./media/tutorial-design-database-using-portal/3-pricing-tier.png)

4. Clique em **Criar**. Em um ou dois minutos, um novo servidor do Banco de Dados do Azure para MariaDB estará em execução na nuvem. Na barra de ferramentas, clique no botão **Notificações** para monitorar o processo de implantação.

## <a name="configure-firewall"></a>Configurar o firewall
Os Banco de Dados do Azure para MariaDB são protegidos por um firewall. Por padrão, todas as conexões com o servidor e com os bancos de dados dentro do servidor são rejeitadas. Antes de se conectar ao Banco de Dados do Azure para MariaDB pela primeira vez, configure o firewall para adicionar o endereço IP (ou o intervalo de endereços IP) da rede pública do computador cliente.

1. Clique em seu servidor recém-criado e depois clique em **Segurança de conexão**.
   
   ![Segurança da conexão](./media/tutorial-design-database-using-portal/1-Connection-security.png)
2. Você pode **Adicionar Meu IP** ou configurar regras de firewall aqui. Lembre-se de clicar em **Salvar** depois de criar as regras.
Agora você pode se conectar ao servidor usando a ferramenta de linha de comando do MySQL ou a ferramenta de GUI do MySQL Workbench.

> [!TIP]
> O servidor do Banco de Dados do Azure para MariaDB se comunica pela porta 3306. Se você estiver tentando conectar-se a partir de uma rede corporativa, o tráfego de saída pela porta 3306 poderá não ser permitido pelo firewall de sua rede. Se isto acontecer, você não poderá conectar o servidor do Banco de Dados do Azure para MariaDB, a menos que o departamento de TI abra a porta 3306.

## <a name="get-connection-information"></a>Obter informações de conexão
Obtenha o **Nome do servidor** e o **Nome de logon do administrador do servidor** totalmente qualificados para o servidor do Banco de Dados do Azure para MariaDB no portal do Azure. Use o nome do servidor totalmente qualificado para se conectar ao servidor usando a ferramenta de linha de comando do MySQL. 

1. No [portal do Azure](https://portal.azure.com/), clique em **Todos os recursos** no menu à esquerda, digite o nome e pesquise o servidor do Banco de Dados do Azure para MariaDB. Selecione o nome do servidor para exibir os detalhes.

2. Na página **Visão geral**, anote o **Nome do Servidor** e o **Nome de logon do administrador do servidor**. Clique no botão Copiar ao lado de cada campo para copiar para a área de transferência.
   ![4-2 Propriedades do servidor](./media/tutorial-design-database-using-portal/2-server-properties.png)

Neste exemplo, o nome do servidor é *mydemoserver.mariadb.database.azure.com* e o logon do administrador do servidor é *myadmin@mydemoserver*.

## <a name="connect-to-the-server-using-mysql"></a>Conectar-se ao servidor usando mysql
Use a [ferramenta de linha de comando do mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) para estabelecer uma conexão com seu servidor do Banco de Dados do Azure para MariaDB. Execute a ferramenta de linha de comando do MySQL no Azure Cloud Shell no navegador ou no próprio computador usando as ferramentas do MySQL instaladas localmente. Para iniciar o Azure Cloud Shell, clique no botão `Try It` em um bloco de código deste artigo ou visite o portal do Azure e clique no ícone `>_` na barra de ferramentas superior direita. 

Digite o comando para se conectar:
```azurecli-interactive
mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Criar um banco de dados vazio
Quando você estiver conectado ao servidor, crie um banco de dados em branco com o qual trabalhar.
```sql
CREATE DATABASE mysampledb;
```

No prompt, execute o seguinte comando para mudar a conexão para esse banco de dados recém-criado:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Criar tabelas no banco de dados
Agora que você sabe como se conectar ao Banco de Dados do Azure para MariaDB, é possível concluir algumas tarefas básicas:

Primeiro, crie uma tabela e carregue-a com alguns dados. Vamos criar uma tabela que armazena informações de inventário.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Carregar dados nas tabelas
Agora que você tem uma tabela, insira alguns dados nela. Na janela do prompt de comando aberta, execute a consulta a seguir para inserir algumas linhas de dados.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Agora, você tem duas linhas de dados de exemplo na tabela criada anteriormente.

## <a name="query-and-update-the-data-in-the-tables"></a>Consultar e atualizar os dados nas tabelas
Execute a seguinte consulta para recuperar as informações da tabela do banco de dados.
```sql
SELECT * FROM inventory;
```

Também é possível atualizar os dados nas tabelas.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

A linha é atualizada à medida que você recupera os dados.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Restaurar um banco de dados em um ponto anterior no tempo
Imagine que você excluiu acidentalmente uma tabela de banco de dados importante e não consegue recuperar os dados com facilidade. O Banco de Dados do Azure para MariaDB permite restaurar o servidor para um ponto no tempo, criando uma cópia dos bancos de dados no novo servidor. Use esse novo servidor para recuperar seus dados excluídos. As etapas a seguir restauram o servidor de exemplo para um ponto anterior à adição da tabela.

1. No portal do Azure, localize o Banco de Dados do Azure para MariaDB. Na página **Visão Geral**, clique em **Restaurar** na barra de ferramentas. A página Restaurar é aberta.

   ![10-1 restaurar um banco de dados](./media/tutorial-design-database-using-portal/1-restore-a-db.png)

2. Preencha o formulário **Restaurar** com as informações necessárias.
   
   ![10-2 formulário de restauração](./media/tutorial-design-database-using-portal/2-restore-form.png)
   
   - **Ponto de restauração**: selecione um ponto no tempo no qual você deseja restaurar, dentro do período listado. Lembre-se de converter o fuso horário local para UTC.
   - **Restaurar em novo servidor**: forneça um novo nome do servidor no qual você deseja restaurar.
   - **Localização**: a região é o mesma do servidor de origem e não pode ser alterada.
   - **Tipo de preço**: o tipo de preço é o mesmo do servidor de origem e não pode ser alterado.
   
3. Clique em **OK** para restaurar o servidor em um ponto no tempo <!--[restore to a point in time](./howto-restore-server-portal.md)--> anterior à exclusão da tabela. A restauração de um servidor cria uma nova cópia do servidor, a partir do ponto no tempo especificado. 

<!--## Next steps
In this tutorial, you use the Azure portal to learned how to:

> [!div class="checklist"]
> * Create an Azure Database for MariaDB
> * Configure the server firewall
> * Use mysql command-line tool to create a database
> * Load sample data
> * Query data
> * Update data
> * Restore data

><> [!div class="nextstepaction"]
> [How to connect applications to Azure Database for MariaDB](./howto-connection-string.md)-->