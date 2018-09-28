---
title: Criar uma conta de API do Cassandra do Azure Cosmos DB, um banco de dados e uma tabela usando um aplicativo Java
description: Este artigo mostra como criar uma conta de API do Cassandra, adicionar um banco de dados (também conhecido como um keyspace) e uma tabela para essa conta usando um aplicativo java.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: af25c52d1300a2daed3542c5cb1db080c505f715
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953116"
---
# <a name="create-an-azure-cosmos-db-cassandra-api-account-database-and-a-table-by-using-a-java-application"></a>Criar uma conta de API do Cassandra do Azure Cosmos DB, um banco de dados e uma tabela usando um aplicativo Java

Este tutorial descreve como usar um aplicativo Java para criar uma conta de API do Cassandra no Azure Cosmos DB, adicionar um banco de dados (também conhecido como um keyspace) e adicionar uma tabela. O aplicativo Java usa o [driver Datastax Java](https://github.com/datastax/java-driver) para criar um banco de dados do usuário que contém detalhes como ID de usuário, nome de usuário, cidade do usuário.  

Este tutorial cobre as seguintes tarefas:

> [!div class="checklist"]
> * Criar uma conta de banco de dados do Cassandra
> * Obter uma cadeia de conexão da conta
> * Como criar dependências e projeto Maven
> * Adicionar um banco de dados e uma tabela
> * Execute o aplicativo

## <a name="prerequisites"></a>Pré-requisitos 

* Se você não tiver uma assinatura do Azure, crie uma  [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  antes de começar. Como alternativa, você pode  [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/)  sem uma assinatura do Azure, sem ônus e sem compromisso. 

* Obter a versão mais recente do [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 

* [Baixar](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um armazenamento binário [Maven](http://maven.apache.org/) 
  - No Ubuntu, você pode executar `apt-get install maven` para instalar o Maven. 

## <a name="create-a-database-account"></a>Criar uma conta de banco de dados 

1. Faça login no  [ portal do Azure ](https://portal.azure.com/). 

2. Selecione  **Criar um recurso** > **Bancos de dados** > **Azure Cosmos DB**. 

3. No painel  **Nova conta** , insira as configurações da nova conta do Azure Cosmos DB. 

   |Configuração   |Valor sugerido  |DESCRIÇÃO  |
   |---------|---------|---------|
   |ID   |   Insira um nome exclusivo    | Insira um nome exclusivo para identificar essa conta do Azure Cosmos DB. <br/><br/>Como cassandra.cosmosdb.azure.com é acrescentado à ID fornecida para criar seu ponto de contato, use uma ID exclusiva mas identificável.         |
   |API    |  Cassandra   |  A API determina o tipo de conta a ser criada. <br/> Selecione **Cassandra**, pois neste artigo, você criará um banco de dados de toda a coluna que pode ser consultado usando a sintaxe CQL.  |
   |Assinatura    |  Sua assinatura        |  Selecione a assinatura do Azure que você deseja usar para essa conta do Azure Cosmos DB.        |
   |Grupo de recursos   | Insira um nome    |  Selecione  **Criar Novo**, em seguida, insira um novo nome de grupo de recursos para sua conta. Para simplificar, você pode usar um nome igual à sua ID.    |
   |Local padrão    |  Selecione a região mais próxima de seus usuários    |  Selecione a localização geográfica na qual hospedar a sua conta do Azure Cosmos DB. Use o local mais próximo dos usuários para fornecer a eles acesso mais rápido aos dados.    |

   ![Criar conta com o portal](./media/create-cassandra-api-account-java/create-account.png)

4. Em seguida, selecione **Criar**. <br/>A criação da conta leva alguns minutos. Depois que o recurso for criado, você pode ver a notificação **Implantação bem-sucedida** no canto superior direito do portal.

## <a name="get-the-connection-details-of-your-account"></a>Obter os detalhes de conexão da sua conta  

Obter as informações de cadeia de conexão do portal do Azure e copiá-lo para o arquivo de configuração de Java. Isso permite que seu aplicativo se comunique com o banco de dados hospedado. 

1. No  [Portal do Azure](http://portal.azure.com/), navegue até sua conta do Azure Cosmos DB. 

2. Abra o painel  **cadeia de conexão**.  

3. Copie os valores de **PONTO DE CONTATO**, **PORTA**, **NOME DE USUÁRIO**, e **SENHA PRIMÁRIA** a serem usados nas próximas etapas.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Criar projeto, dependências e classes de utilitário do Maven 

O projeto de exemplo de Java que você usa neste artigo é hospedado no GitHub. Você pode baixá-lo do repositório [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started). 

Depois de baixar os arquivos, atualize as informações de cadeia de conexão dentro do arquivo `java-examples\src\main\resources\config.properties` e execute-o.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_host = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Como alternativa, você também pode criar o exemplo do zero.  

1. No terminal ou prompt de comando, crie um novo projeto Maven chamado Cassandra-demo. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. Localize a pasta `cassandra-demo`. Usando um editor de texto, abra o arquivo `pom.xml` que foi gerado. 

   Adicione as dependências do datastax Cassandra e compile os plug-ins exigidos pelo seu projeto, conforme mostrado no arquivo [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml).  

3. Na pasta `cassandra-demo\src\main`, crie uma nova pasta chamada `resources`.  Na pasta de recursos, adicione os arquivos config.properties e log4j.properties:

   - O arquivo [config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) armazena os valores da chave e ponto de extremidade da conexão de API do Cassandra do Azure Cosmos DB. 
   
   - O arquivo [log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) define o nível de log necessário ao interagir com a API do Cassandra.  

4. Em seguida, navegue até a pasta `src/main/java/com/azure/cosmosdb/cassandra/`. Dentro da pasta de cassandra, crie outra pasta chamada `utils`. A nova pasta armazena as classes de utilitário necessárias para se conectar à conta de API do Cassandra. 

   Adicione a classe [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) para criar o cluster e para abrir e fechar as sessões de Cassandra. O cluster se conecta à API do Cassandra do Azure Cosmos DB e retorna uma sessão para acessar. Use a classe [Configurações](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) para ler informações de cadeia de conexão do arquivo config.properties. 

5. O exemplo de Java cria um banco de dados com informações do usuário, como nome de usuário, ID de usuário, cidade do usuário. Você precisa definir get e métodos para acessar os detalhes do usuário na função principal.
 
   Crie uma classe [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) na pasta `src/main/java/com/azure/cosmosdb/cassandra/` com get e defina métodos. 

## <a name="add-a-database-and-a-table"></a>Adicionar um banco de dados e uma tabela  

Esta seção descreve como adicionar um banco de dados (keyspace) e uma tabela, usando a linguagem de consulta do Cassandra (CQL). Para saber mais sobre a sintaxe CQL para esses comandos, consulte a sintaxe de consulta [criar um keyspace](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) e [criar tabela](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable). 

1. Na pasta `src\main\java\com\azure\cosmosdb\cassandra`, crie uma nova pasta chamada `repository`. 

2. Em seguida, crie a classe de Java `UserRepository` e adicione o seguinte código nele: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. Localize a pasta `src\main\java\com\azure\cosmosdb\cassandra` e cria uma nova subpasta chamada `examples`.

4. Em seguida, crie a classe de Java `UserProfile`. Essa classe contém o método principal que chama os métodos createKeyspace e createTable que você definiu anteriormente: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Execute o aplicativo 

1. Abra um prompt de comando ou uma janela de terminal. Cole o seguinte bloco de código. 

   Esse código altera o diretório (cd) para o caminho da pasta onde você criou o projeto. Em seguida, ele executa o comando `mvn clean install` para gerar o arquivo `cosmosdb-cassandra-examples.jar` dentro da pasta de destino. Por fim, ele executa o aplicativo Java.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   A janela do terminal exibe notificações de que o keyspace e a tabela foram criados. 
   
2. Agora, no portal do Azure, abra o **Data Explorer** para confirmar que o keyspace e a tabela foram criadas.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a criar uma conta de API do Cassandra do Azure Cosmos DB, um banco de dados e uma tabela usando um aplicativo Java. Você pode prosseguir para o próximo artigo:

> [!div class="nextstepaction"]
> [carregar dados de exemplo para a tabela de API do Cassandra](cassandra-api-load-data.md).