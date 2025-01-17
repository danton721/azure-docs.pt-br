---
title: Como escrever procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB
description: Saiba como definir procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/21/2019
ms.author: mjbrown
ms.openlocfilehash: 66e0a7e13df9eddcd722492c9c894721517af5f9
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968928"
---
# <a name="how-to-write-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Como escrever procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB

O Azure Cosmos DB oferece execução transacional e integrada de linguagem do JavaScript que permite escrever **procedimentos armazenados**, **gatilhos** e **UDFs (funções definidas pelo usuário)** . Ao usar a API do SQL no Azure Cosmos DB, é possível definir os procedimentos armazenados, gatilhos e UDFs na linguagem JavaScript. É possível escrever sua lógica em JavaScript e executá-la dentro do mecanismo de banco de dados. É possível criar e executar gatilhos, procedimentos armazenados e UDFs usando o [portal do Azure](https://portal.azure.com/), a [API de consulta integrada de linguagem JavaScript no Azure Cosmos DB](javascript-query-api.md) e os [SDKs de cliente da API do SQL do Cosmos DB](sql-api-dotnet-samples.md). 

Para chamar um procedimento armazenado, um gatilho e uma função definida pelo usuário, é necessário registrá-lo. Para saber mais, confira [Como trabalhar com procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md).

> [!NOTE]
> Para contêineres particionados, ao executar um procedimento armazenado, um valor de chave de partição deve ser fornecido nas opções de solicitação. O escopo dos procedimentos armazenados sempre é uma chave de partição. Itens que têm um valor de chave de partição diferente não estarão visíveis para o procedimento armazenado. Isso também aplica-se a gatilhos.

## <a id="stored-procedures"></a>Como escrever procedimentos armazenados

Procedimentos armazenados são escritos usando o JavaScript; eles podem criar, atualizar, ler, consultar e excluir itens dentro de um contêiner do Azure Cosmos. Os procedimentos armazenados são registrados por coleção e podem ser operados em qualquer documento ou ser um anexo presente na coleção.

**Exemplo**

Veja abaixo um procedimento armazenado simples que retorna uma resposta "Olá, Mundo".

```javascript
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

O objeto de contexto fornece acesso a todas as operações que podem ser realizadas no Azure Cosmos DB, bem como o acesso aos objetos de solicitação e resposta. Nesse caso, use o objeto de resposta para definir o corpo da resposta a ser enviada de volta para o cliente.

Uma vez escrito, o procedimento armazenado deve ser registrado com uma coleção. Para saber mais, confira o artigo [Como usar procedimentos armazenados no Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures).

### <a id="create-an-item"></a>Criar um item usando o procedimento armazenado

Quando você cria um item usando um procedimento armazenado, ele é inserido no contêiner do Azure Cosmos DB e uma ID para o item recém-criado é retornada. A criação de um item é uma operação assíncrona e depende das funções de retorno de chamada do JavaScript. A função de retorno de chamada tem dois parâmetros – um para o objeto de erro, caso a operação falhe, e outro para um valor retornado; nesse caso, o objeto criado. Dentro da chamada de retorno, é possível lidar com a exceção ou gerar um erro. Caso uma chamada de retorno não seja fornecida e haja um erro, o tempo de execução do Azure Cosmos DB gerará um erro. 

O procedimento armazenado também inclui um parâmetro para definir a descrição; é um valor booliano. Quando o parâmetro é definido como true e a descrição está ausente, o procedimento armazenado gerará uma exceção. Caso contrário, o restante do procedimento armazenado continuará em execução.

O exemplo de procedimento armazenado a seguir usa um novo item do Azure Cosmos DB como entrada, insere-o no contêiner do Azure Cosmos DB e retorna a ID do item recém-criado. Neste exemplo, estamos usando o exemplo ToDoList da [API do SQL .NET do Início Rápido](create-sql-api-dotnet.md)

```javascript
function createToDoItem(itemToCreate) {

    var context = getContext();
    var container = context.getCollection();

    var accepted = container.createDocument(container.getSelfLink(),
        itemToCreate,
        function (err, itemCreated) {
            if (err) throw new Error('Error' + err.message);
            context.getResponse().setBody(itemCreated.id)
        });
    if (!accepted) return;
}
```

### <a name="arrays-as-input-parameters-for-stored-procedures"></a>Matrizes como parâmetros de entrada para procedimentos armazenados 

Ao definir um procedimento armazenado no Portal do Azure, os parâmetros de entrada são sempre enviados como uma cadeia para o procedimento armazenado. Mesmo se você passar uma matriz de cadeias de caracteres como uma entrada, a matriz será convertida em cadeia de caracteres e enviada para o procedimento armazenado. Para contornar isso, é possível definir uma função no procedimento armazenado para analisar a cadeia de caracteres como uma matriz. O código a seguir mostra como analisar um parâmetro de entrada de cadeia de caracteres como uma matriz:

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);

    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

### <a id="transactions"></a>Transações em procedimentos armazenados

É possível implementar transações em itens dentro de um contêiner usando um procedimento armazenado. O exemplo a seguir usa transações dentro de um aplicativo de jogo de futebol de fantasia para trocar jogadores entre duas equipes em uma única operação. O procedimento armazenado tenta ler os dois itens do Azure Cosmos DB, cada um correspondendo às IDs de jogador transmitidas como um argumento. Se ambos os jogadores forem encontrados, então o procedimento armazenado atualizará os itens trocando suas equipes. Se forem encontrados erros pelo caminho, o procedimento armazenado gerará uma exceção JavaScript que aborta implicitamente a transação.

```javascript
// JavaScript source code
function tradePlayers(playerId1, playerId2) {
    var context = getContext();
    var container = context.getCollection();
    var response = context.getResponse();

    var player1Document, player2Document;

    // query for players
    var filterQuery = 
    {     
        'query' : 'SELECT * FROM Players p where p.id = @playerId1',
        'parameters' : [{'name':'@playerId1', 'value':playerId1}] 
    };
            
    var accept = container.queryDocuments(container.getSelfLink(), filterQuery, {},
        function (err, items, responseOptions) {
            if (err) throw new Error("Error" + err.message);

            if (items.length != 1) throw "Unable to find both names";
            player1Item = items[0];

            var filterQuery2 = 
            {     
                'query' : 'SELECT * FROM Players p where p.id = @playerId2',
                'parameters' : [{'name':'@playerId2', 'value':playerId2}] 
            };
            var accept2 = container.queryDocuments(container.getSelfLink(), filterQuery2, {},
                function (err2, items2, responseOptions2) {
                    if (err2) throw new Error("Error" + err2.message);
                    if (items2.length != 1) throw "Unable to find both names";
                    player2Item = items2[0];
                    swapTeams(player1Item, player2Item);
                    return;
                });
            if (!accept2) throw "Unable to read player details, abort ";
        });

    if (!accept) throw "Unable to read player details, abort ";

    // swap the two players’ teams
    function swapTeams(player1, player2) {
        var player2NewTeam = player1.team;
        player1.team = player2.team;
        player2.team = player2NewTeam;

        var accept = container.replaceDocument(player1._self, player1,
            function (err, itemReplaced) {
                if (err) throw "Unable to update player 1, abort ";

                var accept2 = container.replaceDocument(player2._self, player2,
                    function (err2, itemReplaced2) {
                        if (err) throw "Unable to update player 2, abort"
                    });

                if (!accept2) throw "Unable to update player 2, abort";
            });

        if (!accept) throw "Unable to update player 1, abort";
    }
}
```

### <a id="bounded-execution"></a>Execução vinculada em procedimentos armazenados

A seguir, há um exemplo de um procedimento armazenado que importa itens em massa em um contêiner do Azure Cosmos. O procedimento armazenado lida com a execução vinculada verificando o valor retornado booliano em `createDocument` e depois utiliza a contagem de itens inseridos em cada invocação do procedimento armazenado para rastrear e retomar o progresso nos lotes.

```javascript
function bulkImport(items) {
    var container = getContext().getCollection();
    var containerLink = container.getSelfLink();

    // The count of imported items, also used as current item index.
    var count = 0;

    // Validate input.
    if (!items) throw new Error("The array is undefined or null.");

    var itemsLength = items.length;
    if (itemsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create an item.
    tryCreate(items[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted.
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called items.length times.
    //    In this case all items were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(item, callback) {
        var isAccepted = container.createDocument(containerLink, item, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client,
        // which will call the script again with remaining set of items.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when container.createDocument is done in order to process the result.
    function callback(err, item, options) {
        if (err) throw err;

        // One more item has been inserted, increment the count.
        count++;

        if (count >= itemsLength) {
            // If we created all items, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(items[count], callback);
        }
    }
}
```

## <a id="triggers"></a>Como escrever gatilhos

O Azure Cosmos DB dá suporte a pré-gatilhos e pós-gatilhos. Os pré-gatilhos são executados antes de modificar um item de banco de dados e os pós-gatilhos são executados depois de modificar um item de banco de dados.

### <a id="pre-triggers"></a>Pré-gatilhos

O exemplo a seguir mostra como um pré-gatilho é usado para validar as propriedades de um item do Azure Cosmos DB que está sendo criado. Neste exemplo, estamos usando a amostra ToDoList da [API do SQL .NET de Início Rápido](create-sql-api-dotnet.md) para adicionar uma propriedade de carimbo de data/hora a um item recém-adicionado, se ele não contém uma.

```javascript
function validateToDoItemTimestamp() {
    var context = getContext();
    var request = context.getRequest();

    // item to be created in the current operation
    var itemToCreate = request.getBody();

    // validate properties
    if (!("timestamp" in itemToCreate)) {
        var ts = new Date();
        itemToCreate["timestamp"] = ts.getTime();
    }

    // update the item that will be created
    request.setBody(itemToCreate);
}
```

Pré-gatilhos não podem ter parâmetros de entrada. O objeto de solicitação no gatilho é usado para manipular a mensagem de solicitação associada à operação. No exemplo anterior, o pré-gatilho é executado ao criar um item do Azure Cosmos DB e o corpo da mensagem de solicitação contém o item a ser criado no formato JSON.

Quando os gatilhos são registrados, é possível especificar as operações com as quais eles podem ser executados. Esse gatilho deve ser criado com um valor `TriggerOperation` de `TriggerOperation.Create`, o que significa que não é permitido usar o gatilho em uma operação de substituição, conforme mostrado no código a seguir.

Para obter exemplos de como registrar e chamar um pré-gatilho, confira artigos de [pré-gatilhos](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) e [pós-gatilhos](how-to-use-stored-procedures-triggers-udfs.md#post-triggers). 

### <a id="post-triggers"></a>Pós-gatilhos

O exemplo a seguir mostra um pós-gatilho. Este gatilho consulta o item de metadados e o atualiza com detalhes sobre o item recém-criado.


```javascript
function updateMetadata() {
var context = getContext();
var container = context.getCollection();
var response = context.getResponse();

// item that was created
var createdItem = response.getBody();

// query for metadata document
var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
var accept = container.queryDocuments(container.getSelfLink(), filterQuery,
    updateMetadataCallback);
if(!accept) throw "Unable to update metadata, abort";

function updateMetadataCallback(err, items, responseOptions) {
    if(err) throw new Error("Error" + err.message);
        if(items.length != 1) throw 'Unable to find metadata document';

        var metadataItem = items[0];

        // update metadata
        metadataItem.createdItems += 1;
        metadataItem.createdNames += " " + createdItem.id;
        var accept = container.replaceDocument(metadataItem._self,
            metadataItem, function(err, itemReplaced) {
                    if(err) throw "Unable to update metadata, abort";
            });
        if(!accept) throw "Unable to update metadata, abort";
        return;
}
```

É importante observar a execução transacional de gatilhos no Azure Cosmos DB. O pós-gatilho é executado como parte da mesma transação para o próprio item subjacente. Uma exceção durante a execução pós-gatilho falhará a transação inteira. Tudo o que estiver confirmado será revertido, e uma exceção será retornada.

Para obter exemplos de como registrar e chamar um pré-gatilho, confira artigos de [pré-gatilhos](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) e [pós-gatilhos](how-to-use-stored-procedures-triggers-udfs.md#post-triggers). 

## <a id="udfs"></a>Como escrever funções definidas pelo usuário

O exemplo a seguir cria uma UDF para calcular o imposto de renda para várias faixas de renda. Essa função definida pelo usuário deve ser usada dentro de uma consulta. Para os fins deste exemplo, suponha que há um contêiner chamado “Rendas” com propriedades da seguinte maneira:

```json
{
   "name": "Satya Nadella",
   "country": "USA",
   "income": 70000
}
```

A seguir, há uma definição de função para calcular o imposto de renda para várias faixas de renda:

```javascript
function tax(income) {

        if(income == undefined)
            throw 'no input';

        if (income < 1000)
            return income * 0.1;
        else if (income < 10000)
            return income * 0.2;
        else
            return income * 0.4;
    }
```

Para obter exemplos de como registrar e usar uma função definida pelo usuário, confira o artigo [Como usar funções definidas pelo usuário no Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md#udfs).

## <a name="next-steps"></a>Próximas etapas

Aprenda mais conceitos e como escrever ou usar procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB:

* [Como registrar e usar procedimentos armazenados, gatilhos e funções definidas pelo usuário no Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md)

* [Como escrever procedimentos armazenados e gatilhos usando a API de Consulta do Javascript no Azure Cosmos DB](how-to-write-javascript-query-api.md)

* [Trabalhando com procedimentos armazenados, gatilhos e funções definidas pelo usuário do Azure Cosmos DB no Azure Cosmos DB](stored-procedures-triggers-udfs.md)

* [Trabalhando com a API de consulta integrada da linguagem JavaScript no Azure Cosmos DB](javascript-query-api.md)
