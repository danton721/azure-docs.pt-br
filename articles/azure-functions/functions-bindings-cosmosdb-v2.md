---
title: Associações do Azure Cosmos DB para Functions 2.x (Versão Prévia)
description: Entenda como usar gatilhos e associações do Azure Cosmos DB no Azure Functions.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: azure functions, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: tdykstra
ms.openlocfilehash: e40ba6bcfa1b6247f62849626d4a7803a76362a1
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234862"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x-preview"></a>Associações do Azure Cosmos DB para Azure Functions 2.x (Versão Prévia)

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versão 1 – já disponível](functions-bindings-cosmosdb.md)
> * [Versão 2 – Versão prévia](functions-bindings-cosmosdb-v2.md)

Este artigo explica como trabalhar com associações do [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) no Azure Functions 2.x. O Azure Functions dá suporte a associações de gatilho, entrada e saída para o Azure Cosmos DB.

> [!NOTE]
> Este artigo é para o [Azure Functions versão 2. x](functions-versions.md), que está na Versão Prévia.  Para obter informações sobre como usar essas associações em Functions 1. x, consulte [Associações do Azure Cosmos DB para Azure Functions 1. x](functions-bindings-cosmosdb.md).
>
> Essa associação era originalmente denominada DocumentDB. No Functions versão 2.x, o gatilho, associações e pacote são chamados de Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pacotes - Functions 2. x

As associações do Azure Cosmos DB para Functions versão 2.x são fornecidas no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.CosmosDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB), versão 3.x. O código-fonte para a associação está no repositório GitHub [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Gatilho

O Gatilho do Azure Cosmos DB usa o [Feed de Alterações do Azure Cosmos DB](../cosmos-db/change-feed.md) para atender as inserções e atualizações nas partições. O feed de alteração publica inserções e atualizações, não exclusões.

## <a name="trigger---example"></a>Gatilho - exemplo

Consulte o exemplo específico a um idioma:

* [C#](#trigger---c-example)
* [Script do C# (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Ignorar exemplos de gatilho](#trigger---attributes)

### <a name="trigger---c-example"></a>Gatilho - exemplo C#

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que é chamada quando há inserções ou atualizações na coleção e no banco de dados especificados.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Ignorar exemplos de gatilho](#trigger---attributes)

### <a name="trigger---c-script-example"></a>Gatilho - exemplo de script C#

O exemplo a seguir mostra uma associação de gatilho do Cosmos DB em um arquivo *function.json* e uma [função script C#](functions-reference-csharp.md) que usa a associação. A função grava mensagens de log quando registros do Cosmos DB são modificados.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Aqui está o código de script do C#:
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
      log.Verbose("Documents modified " + documents.Count);
      log.Verbose("First document Id " + documents[0].Id);
    }
```

[Ignorar exemplos de gatilho](#trigger---attributes)

### <a name="trigger---javascript-example"></a>Gatilho - exemplo de JavaScript

O exemplo a seguir mostra uma associação de gatilho do Cosmos DB em um arquivo *function.json* e uma [função JavaScript](functions-reference-node.md) que usa a associação. A função grava mensagens de log quando registros do Cosmos DB são modificados.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Aqui está o código JavaScript:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

## <a name="trigger---attributes"></a>Gatilho – atributos

Em [bibliotecas de classes de C#](functions-dotnet-class-library.md), utilize o atributo [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs).

O construtor do atributo toma o nome do banco de dados e o nome da coleção. Para obter informações sobre essas configurações e outras propriedades que podem ser configuradas, consulte [Gatilho – configurações](#trigger---configuration). Aqui está um exemplo de atributo `CosmosDBTrigger` em uma assinatura de método:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Para ver um exemplo completo, consulte [Gatilho – exemplo de C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Gatilho – configuração

A tabela a seguir explica as propriedades de configuração de associação que você define no arquivo *function.json* e no atributo `CosmosDBTrigger`.

|Propriedade function.json | Propriedade de atributo |DESCRIÇÃO|
|---------|---------|----------------------|
|**tipo** || Deve ser definido como `cosmosDBTrigger`. |
|**direction** || Deve ser definido como `in`. Esse parâmetro é definido automaticamente quando você cria o gatilho no portal do Azure. |
|**name** || O nome da variável usado no código de função que representa a lista de documentos com alterações. | 
|**connectionStringSetting**|**ConnectionStringSetting** | O nome de uma configuração de aplicativo que contém a cadeia de conexão usada para conectar-se à conta do Azure Cosmos DB que está sendo monitorada. |
|**databaseName**|**DatabaseName**  | O nome do banco de dados do Azure Cosmos DB com a coleção que está sendo monitorada. |
|**collectionName** |**CollectionName** | O nome da coleção que está sendo monitorada. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Opcional) O nome de uma configuração de aplicativo que contém a cadeia de conexão para o serviço com a coleção de concessão. Quando não definido, o valor `connectionStringSetting` é usado. Esse parâmetro é definido automaticamente quando a associação é criada no portal. A cadeia de conexão da coleção de concessões deve ter permissões de gravação.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Opcional) O nome do banco de dados que contém a coleção usada para armazenar as concessões. Quando não definido, o valor da configuração `databaseName` é usado. Esse parâmetro é definido automaticamente quando a associação é criada no portal. |
|**leaseCollectionName** | **LeaseCollectionName** | (Opcional) O nome da coleção usada para armazenar as concessões. Quando não definido, o valor `leases` é usado. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Opcional) Quando definido como `true`, a coleção de concessões é criada automaticamente quando ela ainda não existe. O valor padrão é `false`. |
|**leasesCollectionThroughput**| **leasesCollectionThroughput**| (Opcional) Define a quantidade de Unidades de Solicitação a atribuir quando a coleção de concessões for criada. Essa configuração é usada apenas quando `createLeaseCollectionIfNotExists` é definido como `true`. Esse parâmetro é definido automaticamente quando a associação é criada usando o portal.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| (Opcional) Quando definido, ele adiciona um prefixo às concessões criadas na coleção de Concessão para esta Função, permitindo efetivamente duas funções do Azure Functions separadas para compartilhar a mesma coleção de Concessão usando prefixos diferentes.
|**feedPollDelay**| **FeedPollDelay**| (Opcional) Quando definido, ele define, em milissegundos, o atraso entre a sondagem de uma partição quanto a novas alterações no feed, depois que todas as alterações atuais forem descarregadas. O padrão é 5000 (5 segundos).
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (Opcional) Quando definido, ele define, em milissegundos, o intervalo para disparar uma tarefa para computar se as partições são distribuídas uniformemente entre as instâncias de host conhecidas. O padrão é 13000 (13 segundos).
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (Opcional), Quando definido, ele define, em milissegundos, o intervalo para o qual a concessão é tomada em uma concessão que representa uma partição. Se a concessão não for renovada dentro deste intervalo, ela será expirada e a propriedade da partição será movida para outra instância. O padrão é 60000 (60 segundos).
|**leaseRenewInterval**| **LeaseRenewInterval**| (Opcional) Quando definido, ele define, em milissegundos, o intervalo de renovação para todas as concessões para partições atualmente mantidas por uma instância. O padrão é 17000 (17 segundos).
|**checkpointFrequency**| **CheckpointFrequency**| (Opcional) Quando definido, ele define, em milissegundos, o intervalo entre os pontos de verificação de concessão. O padrão é sempre após uma chamada de Função com êxito.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Opcional) Quando definido, ele personaliza a quantidade máxima de itens recebidos por chamada de Função.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Gatilho - uso

O gatilho requer uma segunda coleção que ele usa para armazenar _concessões_ sobre as partições. Tanto a coleção que está sendo monitorada quanto a coleção que contém as concessões devem estar disponíveis para o gatilho funcionar.

>[!IMPORTANT]
> Se várias funções estiverem configuradas para usar um gatilho do Cosmos DB para a mesma coleção, cada uma das funções deverá usar uma coleção de concessões dedicada ou especificar uma diferente `LeaseCollectionPrefix` para cada função. Caso contrário, apenas uma das funções será disparada. Para obter informações sobre o prefixo, consulte a [seção de Configuração](#trigger---configuration).

O gatilho não indica se um documento foi atualizado ou inserido, ele fornece apenas o documento em si. Se você precisa lidar com inserções e atualizações de forma diferente, você pode fazer isso com a implementação de campos de carimbo de hora de inserção ou atualização.

## <a name="input"></a>Entrada

A associação de dados de entrada do Azure Cosmos DB recupera um ou mais documentos de banco de dados do Azure Cosmos e os passa para o parâmetro de entrada da função. A ID do documento ou os parâmetros de consulta podem ser determinados com base no gatilho que invoca a função. 

>[!NOTE]
> Não use associação de entrada ou saída do Azure Cosmos DB se você estiver usando a API do MongoDB em uma conta da Cosmos DB. É possível que ocorra corrupção de dados.

## <a name="input---examples"></a>Entrada - exemplos

Consulte os exemplos específicos da linguagem que lê um documento especificando um valor de ID:

* [C#](#input---c-examples)
* [Script do C# (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Ignorar exemplos de entrada](#input---attributes)

### <a name="input---c-examples"></a>Entrada - exemplos em C#

Esta seção contém os seguintes exemplos:

* [Gatilho da fila, pesquisar ID no JSON](#queue-trigger-look-up-id-from-json-c)
* [Gatilho HTTP, pesquisar ID na cadeia de caracteres de consulta](#http-trigger-look-up-id-from-query-string-c)
* [Gatilho HTTP, pesquisar ID nos dados da rota](#http-trigger-look-up-id-from-route-data-c)
* [Gatilho HTTP, pesquisar ID nos dados da rota, usando SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [Gatilho HTTP, obter vários documentos, usando SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [Gatilho HTTP, obter vários documentos, usando DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c)

Os exemplos se referem a um tipo `ToDoItem` simples:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Gatilho da fila, pesquisar ID no JSON (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera um único documento. A função é disparada por uma mensagem da fila que contém um objeto JSON. O gatilho da fila analisa o JSON em um objeto chamado `ToDoItemLookup`, que contém a ID a pesquisar. Essa ID é usada para recuperar um documento `ToDoItem` no banco de dados e na coleção especificados.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>Gatilho HTTP, pesquisar ID na cadeia de caracteres de consulta (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera um único documento. A função é disparada por uma solicitação HTTP que usa uma cadeia de caracteres de consulta para especificar a ID a pesquisar. Essa ID é usada para recuperar um documento `ToDoItem` no banco de dados e na coleção especificados.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>Gatilho HTTP, pesquisar ID nos dados da rota (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera um único documento. A função é disparada por uma solicitação HTTP que usa os dados da rota para especificar a ID a pesquisar. Essa ID é usada para recuperar um documento `ToDoItem` no banco de dados e na coleção especificados.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>Gatilho HTTP, pesquisar ID nos dados da rota, usando SqlQuery (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera um único documento. A função é disparada por uma solicitação HTTP que usa os dados da rota para especificar a ID a pesquisar. Essa ID é usada para recuperar um documento `ToDoItem` no banco de dados e na coleção especificados. 

O exemplo mostra como usar uma expressão de associação no parâmetro `SqlQuery`. Você pode passar os dados da rota para o parâmetro `SqlQuery` conforme mostrado, mas atualmente [não é possível passar os valores da cadeia de caracteres de consulta](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items", 
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>Gatilho HTTP, obter vários documentos, usando SqlQuery (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera uma lista de documentos. A função é disparada por uma solicitação HTTP. A consulta é especificada na propriedade do atributo `SqlQuery`.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>Gatilho HTTP, obter vários documentos, usando DocumentClient (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que recupera uma lista de documentos. A função é disparada por uma solicitação HTTP. O código usa uma instância `DocumentClient` fornecida pela associação do Azure Cosmos DB para ler uma lista de documentos. A instância `DocumentClient` também pode ser usada para as operações de gravação.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.Info($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Ignorar exemplos de entrada](#input---attributes)

### <a name="input---c-script-examples"></a>Entrada - exemplos de script C#

Esta seção contém os seguintes exemplos que ler um documento especificando um valor de ID de várias fontes:

* Gatilho da fila, pesquisar ID na mensagem da fila
* Gatilho da fila, pesquisar ID na mensagem da fila, usando SqlQuery

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-c-script"></a>Gatilho da fila, pesquisar ID na mensagem da fila (script C#)

O exemplo a seguir mostra uma associação de entrada do Cosmos DB em um arquivo *function.json* e uma [função script C#](functions-reference-csharp.md) que usa a associação. A função lê um documento único e atualiza o valor de texto do documento.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
A seção [configuração](#input---configuration) explica essas propriedades.

Aqui está o código de script do C#:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding 
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-queue-message-using-sqlquery-c-script"></a>Gatilho da fila, pesquisar ID na mensagem da fila, usando SqlQuery (script C#)

O exemplo a seguir mostra uma associação de dados de entrada do Cosmos DB em um arquivo *function.json* e uma [função script C#](functions-reference-csharp.md) que usa a associação de dados. A função recupera vários documentos especificados por uma consulta SQL usando um gatilho de fila para personalizar os parâmetros de consulta.

O gatilho de consulta fornece um parâmetro `departmentId`. Uma mensagem de consulta de `{ "departmentId" : "Finance" }` retornará todos os registros ao departamento financeiro. 

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

A seção [configuração](#input---configuration) explica essas propriedades.

Aqui está o código de script do C#:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Ignorar exemplos de entrada](#input---attributes)

### <a name="input---javascript-examples"></a>Entrada - exemplos de JavaScript

Esta seção contém os seguintes exemplos que ler um documento especificando um valor de ID de várias fontes:

* Gatilho da fila, pesquisar ID na mensagem da fila
* Gatilho da fila, pesquisar ID na mensagem da fila, usando SqlQuery

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-javascript"></a>Gatilho da fila, pesquisar ID na mensagem da fila (JavaScript)

O exemplo a seguir mostra uma associação de entrada do Cosmos DB em um arquivo *function.json* e uma [função JavaScript](functions-reference-node.md) que usa a associação. A função lê um documento único e atualiza o valor de texto do documento.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
A seção [configuração](#input---configuration) explica essas propriedades.

Aqui está o código JavaScript:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Ignorar exemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-javascript"></a>Gatilho da fila, pesquisar ID na mensagem da fila, usando SqlQuery (JavaScript)

O exemplo a seguir mostra uma associação de dados de entrada do Cosmos DB em um arquivo *function.json* e uma [função script C#](functions-reference-node.md) que usa a associação de dados. A função recupera vários documentos especificados por uma consulta SQL usando um gatilho de fila para personalizar os parâmetros de consulta.

O gatilho de consulta fornece um parâmetro `departmentId`. Uma mensagem de consulta de `{ "departmentId" : "Finance" }` retornará todos os registros ao departamento financeiro. 

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

A seção [configuração](#input---configuration) explica essas propriedades.

Aqui está o código JavaScript:

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[Ignorar exemplos de entrada](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Entrada - exemplos de F#

O exemplo a seguir mostra uma associação de entrada do Cosmos DB em um arquivo *function.json* e uma [função F#](functions-reference-fsharp.md) que usa a associação. A função lê um documento único e atualiza o valor de texto do documento.

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

A seção [configuração](#input---configuration) explica essas propriedades.

O código F# é o seguinte:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Esse exemplo requer um arquivo `project.json` que especifique as dependências `FSharp.Interop.Dynamic` e `Dynamitey` do NuGet:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Para adicionar um arquivo do `project.json`, veja [Gerenciamento de pacotes do F#](functions-reference-fsharp.md#package).

## <a name="input---attributes"></a>Entrada – atributos

Em [Bibliotecas de classes C#](functions-dotnet-class-library.md), utilize o atributo [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

O construtor do atributo toma o nome do banco de dados e o nome da coleção. Para obter informações sobre essas configurações e outras propriedades que podem ser configuradas, consulte [a seção de configuração a seguir](#input---configuration). 

## <a name="input---configuration"></a>Entrada - configuração

A tabela a seguir explica as propriedades de configuração de associação que você definir no arquivo *function.json* e o `CosmosDB` atributo.

|Propriedade function.json | Propriedade de atributo |DESCRIÇÃO|
|---------|---------|----------------------|
|**tipo**     || Deve ser definido como `cosmosDB`.        |
|**direction**     || Deve ser definido como `in`.         |
|**name**     || Nome do parâmetro de associação que representa o documento na função.  |
|**databaseName** |**DatabaseName** |O banco de dados que contém o documento.        |
|**collectionName** |**CollectionName** | O nome da coleção que contém o documento. |
|**ID**    | **Id** | A ID do documento a ser recuperado. Essa propriedade dá suporte a [expressões de associação](functions-triggers-bindings.md#binding-expressions-and-patterns). Não defina ambas as propriedades **id** e **sqlQuery**. Se você não definir uma ou outra, toda a coleção é recuperada. |
|**sqlQuery**  |**SqlQuery**  | Uma consulta SQL do Azure Cosmos DB usada para recuperar vários documentos. A propriedade dá suporte a associações de tempo de execução, como neste exemplo: `SELECT * FROM c where c.departmentId = {departmentId}`. Não defina ambas as propriedades **id** e **sqlQuery**. Se você não definir uma ou outra, toda a coleção é recuperada.|
|**connectionStringSetting**     |**ConnectionStringSetting**|O nome da configuração do aplicativo que contém a cadeia de conexão do Azure Cosmos DB.        |
|**partitionKey**|**PartitionKey**|Especifica o valor da chave de partição para a pesquisa. Pode incluir parâmetros de associação.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Entrada - uso

Em funções C# e F#, todas as alterações feitas no documento de entrada por parâmetros de entrada nomeados são persistidas automaticamente. 

Funções de JavaScript, as atualizações não são feitas automaticamente após a saída da função. Em vez disso, use `context.bindings.<documentName>In` e `context.bindings.<documentName>Out` para fazer atualizações. Consulte o [exemplo JavaScript](#input---javascript-example).

## <a name="output"></a>Saída

A saída do Azure Cosmos DB permite que você grave um novo documento para um banco de dados do Azure Cosmos DB. 

>[!NOTE]
> Não use associação de entrada ou saída do Azure Cosmos DB se você estiver usando a API do MongoDB em uma conta da Cosmos DB. É possível que ocorra corrupção de dados.

## <a name="output---example"></a>Saída - exemplo

Consulte o exemplo específico a um idioma:

* [C#](#output---c-examples)
* [Script do C# (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Consulte também o [exemplo de entrada](#input---c-examples) que usa `DocumentClient`.

[Ignorar exemplos de saída](#output---attributes)

### <a name="ouput---c-examples"></a>Saída - exemplos de C#

Esta seção contém os seguintes exemplos:

* Gatilho da fila, gravar um documento
* Gatilho da fila, gravar documentos usando IAsyncCollector

Os exemplos se referem a um tipo `ToDoItem` simples:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Ignorar exemplos de saída](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Gatilho da fila, gravar um documento (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que adiciona um documento a um banco de dados, usando os dados fornecidos na mensagem do armazenamento de fila.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Ignorar exemplos de saída](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Gatilho da fila, gravar documentos usando IAsyncCollector (C#)

O exemplo a seguir mostra uma [função C#](functions-dotnet-class-library.md) que adiciona uma coleção de documentos a um banco de dados, usando os dados fornecidos em um JSON de mensagem da fila.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Ignorar exemplos de saída](#output---attributes)

### <a name="output---c-script-examples"></a>Saída - exemplos de script C#

O exemplo a seguir mostra uma associação de saída do Azure Cosmos DB em um arquivo *function.json* e uma [função de script C#](functions-reference-csharp.md) que usa a associação. A função usa uma associação de entrada de fila para uma fila que recebe o JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos do Azure Cosmos DB no formato a seguir para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

A seção [configuração](#output---configuration) explica essas propriedades.

Aqui está o código de script do C#:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
      log.Info($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

Para criar vários documentos, você também pode associar a `ICollector<T>` ou `IAsyncCollector<T>`, sendo `T` um dos tipos com suporte.

[Ignorar exemplos de saída](#output---attributes)

### <a name="output---javascript-examples"></a>Saída - exemplos de JavaScript

O exemplo a seguir mostra uma associação de saída do Azure Cosmos DB em um arquivo *function.json* e uma [função de script C#](functions-reference-node.md) que usa a associação. A função usa uma associação de entrada de fila para uma fila que recebe o JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos do Azure Cosmos DB no formato a seguir para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

A seção [configuração](#output---configuration) explica essas propriedades.

Aqui está o código JavaScript:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({ 
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Ignorar exemplos de saída](#output---attributes)

### <a name="output---f-examples"></a>Saída - Exemplos de F#

O exemplo a seguir mostra uma associação de saída do Azure Cosmos DB em um arquivo *function.json* e uma [função de script C#](functions-reference-fsharp.md) que usa a associação. A função usa uma associação de entrada de fila para uma fila que recebe o JSON no seguinte formato:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

A função cria documentos do Azure Cosmos DB no formato a seguir para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Aqui estão os dados de associação no arquivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
A seção [configuração](#output---configuration) explica essas propriedades.

O código F# é o seguinte:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
      log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

Esse exemplo requer um arquivo `project.json` que especifique as dependências `FSharp.Interop.Dynamic` e `Dynamitey` do NuGet:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Para adicionar um arquivo do `project.json`, veja [Gerenciamento de pacotes do F#](functions-reference-fsharp.md#package).

## <a name="output---attributes"></a>Saída - atributos

Em [Bibliotecas de classes C#](functions-dotnet-class-library.md), utilize o atributo [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

O construtor do atributo toma o nome do banco de dados e o nome da coleção. Para obter informações sobre essas configurações e outras propriedades que podem ser configuradas, consulte [Saída – configurações](#output---configuration). Aqui está um exemplo de atributo `CosmosDB` em uma assinatura de método:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Para ver um exemplo completo, consulte [Saída – exemplo de C#](#output---c-example).

## <a name="output---configuration"></a>Saída - configuração

A tabela a seguir explica as propriedades de configuração de associação que você define no arquivo *function.json* e no `CosmosDB` atributo.

|Propriedade function.json | Propriedade de atributo |DESCRIÇÃO|
|---------|---------|----------------------|
|**tipo**     || Deve ser definido como `cosmosDB`.        |
|**direction**     || Deve ser definido como `out`.         |
|**name**     || Nome do parâmetro de associação que representa o documento na função.  |
|**databaseName** | **DatabaseName**|O banco de dados que contém a coleção na qual o documento será criado.     |
|**collectionName** |**CollectionName**  | O nome da coleção na qual o documento será criado. |
|**createIfNotExists**  |**CreateIfNotExists**    | É um valor booliano para indicar se a coleção será criada quando não existir. O padrão é *false* porque as novas coleções são criadas com a taxa de transferência reservada, o que tem implicações de preço. Para saber mais, confira a [página de preço](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |Quando `CreateIfNotExists` for true, define o caminho da chave de partição para a coleção criada.|
|**CollectionThroughput**|**CollectionThroughput**| Quando `CreateIfNotExists` for true, define a [taxa de transferência](../cosmos-db/set-throughput.md) da coleção criada.|
|**connectionStringSetting**    |**ConnectionStringSetting** |O nome da configuração do aplicativo que contém a cadeia de conexão do Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - uso

Por padrão, quando você grava no parâmetro de saída em sua função, um documento é criado no banco de dados. Este documento contém um GUID gerado automaticamente como a ID do documento. Você pode especificar a ID do documento de saída especificando a propriedade `id` no objeto JSON enviado ao parâmetro de saída. 

> [!Note]  
> Ao especificar a ID de um documento existente, ela é substituída pelo novo documento de saída. 

## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Associação | Referência |
|---|---|
| CosmosDB | [Códigos de erro CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Vá para um guia de início rápido que use um gatilho do Cosmos DB](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre a computação de banco de dados sem servidor com o Cosmos DB](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Aprenda mais sobre gatilhos e de associações do Azure Functions](functions-triggers-bindings.md)