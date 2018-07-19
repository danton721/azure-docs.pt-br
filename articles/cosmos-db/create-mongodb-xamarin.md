---
title: 'Azure Cosmos DB: compilar um aplicativo Xamarin.Forms com o .NET e com a API do MongoDB | Microsoft Docs'
description: Apresenta um exemplo de código Xamarin que pode ser usado para se conectar à API do MongoDB do Azure Cosmos DB e consultá-la
services: cosmos-db
author: codemillmatt
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quickstart, xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: masoucou
ms.openlocfilehash: 42e2a7f57cbe40dbef8469e4e9ca020f4346a897
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309888"
---
# <a name="quickstart-build-a-mongodb-api-xamarinforms-app-with-net-and-the-azure-portal"></a>Início rápido: compilar um aplicativo Xamarin.Forms da API do MongoDB com o .NET e o Portal do Azure

O Azure Cosmos DB é o serviço de banco de dados multimodelo distribuído globalmente da Microsoft. É possível criar e consultar rapidamente documentos, chave/valor e bancos de dados do grafo. Todos se beneficiam de recursos de escala horizontal e distribuição global no núcleo do Azure Cosmos DB.

Este início rápido demonstra como criar uma conta de [API do MongoDB](mongodb-introduction.md) do Azure Cosmos DB, um banco de dados de documento e uma coleção usando o Portal do Azure. Você criará um aplicativo de tarefas pendentes Xamarin.Forms usando o [driver .NET do MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Pré-requisitos para executar o aplicativo de exemplo

Para executar o exemplo, você precisará do [Visual Studio](https://www.visualstudio.com/downloads/) ou do [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/) e uma conta válida do Azure CosmosDB.

Caso ainda não tenha o Visual Studio, baixe o [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) com a carga de trabalho **Desenvolvimento móvel com .NET** inserida na instalação.

Se você preferir trabalhar em um Mac, baixe o [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/) e execute a instalação.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>

## <a name="create-a-database-account"></a>Criar uma conta de banco de dados

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

O exemplo descrito neste artigo é compatível com MongoDB.Driver versão 2.6.1.

## <a name="clone-the-sample-app"></a>Clonar o aplicativo de exemplo

Primeiro, baixe o aplicativo de API de exemplo do MongoDB do GitHub. Ele implementa uma aplicativo de tarefas com o modelo de armazenamento de documentos do MongoDB.

1. Abra um prompt de comando, crie uma nova pasta chamada exemplos de git e feche o prompt de comando.

    ```bash
    md "C:\git-samples"
    ```

2. Abra uma janela de terminal de git, como git bash, e use o comando `cd` para alterar para a nova pasta para instalar o aplicativo de exemplo.

    ```bash
    cd "C:\git-samples"
    ```

3. Execute o comando a seguir para clonar o repositório de exemplo. Este comando cria uma cópia do aplicativo de exemplo no seu computador.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started.git
    ```

Se não quiser usar o git, você também pode [baixar o projeto como um arquivo ZIP](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started/archive/master.zip)

## <a name="review-the-code"></a>Examine o código

Esta etapa é opcional. Se você estiver interessado em aprender como os recursos de banco de dados são criados no código, poderá examinar os trechos de código a seguir. Caso contrário, você poderá pular para [Atualizar sua cadeia de conexão](#update-your-connection-string).

Os trechos de código a seguir foram todos obtidos da classe `MongoService`, encontrada no seguinte caminho: src/TaskList.Core/Services/MongoService.cs.

* Inicialize o cliente Mongo.
    ```cs
    MongoClientSettings settings = MongoClientSettings.FromUrl(
        new MongoUrl(APIKeys.ConnectionString)
    );

    settings.SslSettings =
        new SslSettings() { EnabledSslProtocols = SslProtocols.Tls12 };

    MongoClient mongoClient = new MongoClient(settings);
    ```

* Recupere uma referência ao banco de dados e á coleção. O SDK .NET do MongoDB criará automaticamente o banco de dados e a coleção, se eles ainda não existirem.
    ```cs
    string dbName = "MyTasks";
    string collectionName = "TaskList";

    var db = mongoClient.GetDatabase(dbName);

    var collectionSettings = new MongoCollectionSettings {
        ReadPreference = ReadPreference.Nearest
    };

    tasksCollection = db.GetCollection<MyTask>(collectionName, collectionSettings);
    ```
* Recupere todos os documentos como uma lista.
    ```cs
    var allTasks = await TasksCollection
                    .Find(new BsonDocument())
                    .ToListAsync();
    ```

* Consulte documentos específicos.
    ```cs
    public async Task<List<MyTask>> GetIncompleteTasksDueBefore(DateTime date)
    {
        var tasks = await TasksCollection
                        .AsQueryable()
                        .Where(t => t.Complete == false)
                        .Where(t => t.DueDate < date)
                        .ToListAsync();

        return tasks;
    }
    ```

* Crie uma tarefa e a insira na coleção do MongoDB.
    ```cs
    public async Task CreateTask(MyTask task)
    {
        await TasksCollection.InsertOneAsync(task);
    }
    ```

* Atualize uma tarefa em uma coleção do MongoDB.
    ```cs
    public async Task UpdateTask(MyTask task)
    {
        await TasksCollection.ReplaceOneAsync(t => t.Id.Equals(task.Id), task);
    }
    ```

* Exclua uma tarefa de uma coleção do MongoDB.
    ```cs
    public async Task DeleteTask(MyTask task)
    {
        await TasksCollection.DeleteOneAsync(t => t.Id.Equals(task.Id));
    }
    ```

<a id="update-your-connection-string"></a>

## <a name="update-your-connection-string"></a>Atualizar sua cadeia de conexão

Agora, volte ao Portal do Azure para obter informações sobre a cadeia de conexão e copiá-las para o aplicativo.

1. No [Portal do Azure](http://portal.azure.com/), na sua conta do Azure Cosmos DB, no painel de navegação esquerdo, clique em **Cadeia de Conexão** e, em seguida, clique em **Chaves de leitura/gravação**. Você usará os botões de cópia no lado direito da tela para copiar a Cadeia de Conexão Primária nas próximas etapas.

2. Abra o arquivo **APIKeys.cs** no diretório **Auxiliares** do projeto **TaskList.Core**.

3. Copie o valor da sua **cadeia de conexão primária** no portal (usando o botão de cópia) e transforme-o no valor do campo **ConnectionString** em seu arquivo **APIKeys.cs**.

Agora, você atualizou o aplicativo com todas as informações necessárias para se comunicar com o Azure Cosmos DB.

## <a name="run-the-app"></a>Execute o aplicativo

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e clique em **Gerenciar Pacotes NuGet**.
2. Clique em **Restaurar todos os pacotes NuGet**.
3. Clique com botão direito em **TaskList.Android** e selecione **Definir como projeto de inicialização**.
4. Pressione F5 para iniciar a depuração do aplicativo.
5. Se você quiser executar no iOS, primeiro seu computador está conectado a um Mac (confira estas [instruções](https://docs.microsoft.com/en-us/xamarin/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio) sobre como fazer isso).
6. Clique com botão direito no projeto **TaskList.Android** e selecione **Definir como projeto de inicialização**.
7. Clique em F5 para começar a depuração do aplicativo.

### <a name="visual-studio-for-mac"></a>Visual Studio para Mac

1. Na lista suspensa de plataformas, selecione TaskList.iOS ou TaskList.Android, dependendo da plataforma na qual você queira executar.
2. Pressione cmd+Enter para começar a depuração do aplicativo.

## <a name="review-slas-in-the-azure-portal"></a>Examinar SLAs no Portal do Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu como criar uma conta do Azure Cosmos DB e executar um aplicativo Xamarin.Forms usando a API para MongoDB. Agora, é possível importar outros dados para sua conta do Cosmos DB.

> [!div class="nextstepaction"]
> [Importar dados no Azure Cosmos DB para a API do MongoDB](mongodb-migrate.md)