---
title: Como usar o Azure Search de um aplicativo .NET – Azure Search
description: Saiba como usar o Azure Search em um aplicativo .NET usando o C# e o SDK do .NET. As tarefas baseadas em códigos incluem a conexão com o serviço, conteúdo de índice e consulta a um índice.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.openlocfilehash: d0921761b565d9e61374bf340f812af4d43f192a
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66426746"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Como usar o Azure Search de um aplicativo .NET

> [!Important]
> Esse conteúdo ainda está em construção. Versão 9.0 do SDK do .NET do Azure Search está disponível no NuGet. Estamos trabalhando para atualizar este guia de migração para explicar como atualizar para a nova versão. Fique atento.
>

Este artigo é um passo a passo para ajudar você a usar o [SDK do .NET do Azure Search](https://aka.ms/search-sdk). Você pode usar o SDK do .NET para implementar uma experiência de pesquisa avançada em seu aplicativo usando o Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Novidades no SDK do Azure Search
O SDK consiste em algumas bibliotecas de clientes que permitem que você gerencie seus índices, fontes de dados, indexadores e mapas de sinônimos, carregue e gerencie documentos e execute consultas, sem a necessidade de lidar com os detalhes de HTTP e JSON. Essas bibliotecas de cliente são distribuídas como pacotes NuGet.

O principal pacote NuGet é `Microsoft.Azure.Search`, o que é um pacote meta que inclui todos os outros pacotes, como dependências. Use esse pacote se você estiver apenas começando ou se você souber que seu aplicativo precisará de todos os recursos do Azure Search.

Os outros pacotes NuGet no SDK são:
 
  - `Microsoft.Azure.Search.Data`: Use esse pacote se estiver desenvolvendo um aplicativo .NET usando o Azure Search e só precisa consultar ou atualizar documentos nos índices. Se você também precisa criar ou atualizar índices, mapas de sinônimo ou outros recursos de nível de serviço, use o `Microsoft.Azure.Search` pacote em vez disso.
  - `Microsoft.Azure.Search.Service`: Use esse pacote se estiver desenvolvendo a automação no .NET para gerenciar índices, mapas de sinônimos, indexadores, fontes de dados ou outros recursos no nível do serviço do Azure Search. Se você precisar apenas consultar ou atualizar em seus índices, use o `Microsoft.Azure.Search.Data` pacote em vez disso. Se você precisar de toda a funcionalidade do Azure Search, use o `Microsoft.Azure.Search` pacote em vez disso.
  - `Microsoft.Azure.Search.Common`: Tipos comuns necessários para as bibliotecas .NET do Azure Search. Você não precisa usar esse pacote diretamente em seu aplicativo. Só deve ser usado como uma dependência.

A diversas bibliotecas de clientes definem classes como `Index`, `Field` e `Document`, e operações como `Indexes.Create` e `Documents.Search` nas classes `SearchServiceClient` e `SearchIndexClient`. Essas classes são organizadas nos namespaces a seguir:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

A versão atual do SDK .Net do Azure Search está disponível para o público geral. Se você gostaria de fornecer comentários para que possamos incorporar na próxima versão, consulte nosso [página de comentários](https://feedback.azure.com/forums/263029-azure-search/).

O SDK do .NET dá suporte à versão `2017-11-11` da [API REST da Azure Search](https://docs.microsoft.com/rest/api/searchservice/). Essa versão agora inclui suporte para sinônimos, bem como aprimoramentos incrementais para indexadores. 

Esse SDK não oferece suporte a [Operações de Gerenciamento](https://docs.microsoft.com/rest/api/searchmanagement/), como criar e dimensionar os serviços do Search e gerenciar chaves de API. Se você precisar gerenciar seus recursos de Pesquisa a partir de um aplicativo .NET, é possível usar o [SDK de Gerenciamento do .NET da Azure Search](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Atualizando para a última versão do SDK
Se você já estiver usando uma versão mais antiga do SDK do .NET do Azure Search e quiser atualizar para a versão disponível para o público geral, [este artigo](search-dotnet-sdk-migration-version-5.md) explicará como fazer isso.

## <a name="requirements-for-the-sdk"></a>Requisitos para o SDK
1. Visual Studio 2017 ou posterior.
2. Seu próprio serviço de Azure Search. Para usar o SDK, você precisará do nome do serviço e de uma ou mais chaves de API. [Criar um serviço no portal](search-create-service-portal.md) ajudará você com estas etapas.
3. Baixe o [pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.Search) do SDK do .NET do Azure Search usando "Gerenciar pacotes NuGet" no Visual Studio. Basta procurar o nome do pacote `Microsoft.Azure.Search` em NuGet.org (ou um dos outros nomes de pacote acima se você precisar somente de um subconjunto da funcionalidade).

O SDK do .NET do Azure Search oferece suporte a aplicativos voltados para o .NET Framework 4.5.2 e superior, além do .NET Core.

## <a name="core-scenarios"></a>Principais cenários
Há várias coisas que você precisará fazer em seu aplicativo de pesquisa. Neste tutorial, abordaremos os principais cenários:

* Criando um índice
* Preenchimento do índice com documentos
* Procura por documentos usando filtros e pesquisa de texto completo

O código de exemplo a seguir ilustra cada um desses cenários. Fique à vontade para usar os snippets de código em seu próprio aplicativo.

### <a name="overview"></a>Visão geral
O exemplo de aplicativo que vamos explorar cria um novo índice chamado "hotéis", preenche-o com alguns documentos e, em seguida, executa algumas consultas de pesquisa. Este é o programa principal, mostrando o fluxo geral:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> Você pode encontrar o código-fonte completo do aplicativo de exemplo usado neste passo a passo no [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Vamos examinar isso passo a passo. Primeiro, é necessário criar um novo `SearchServiceClient`. Esse objeto permite o gerenciamento de índices. Para construir um, você precisa fornecer o nome do serviço do Azure Search, bem como uma chave de API de administração. Você pode inserir essas informações no arquivo `appsettings.json` do [aplicativo de exemplo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Se você fornecer uma chave incorreta (por exemplo, uma chave de consulta quando era necessário fornecer uma chave de administrador), o `SearchServiceClient` lançará uma `CloudException` com a mensagem de erro "Forbidden" na primeira vez que você chamar um método de operação, como `Indexes.Create`. Se isso ocorrer, verifique novamente a chave da API.
> 
> 

As próximas linhas chamam métodos para criação de um índice chamado "hotéis," excluindo-o primeiro caso ele já exista. Abordaremos esses métodos um pouco mais tarde.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Em seguida, o índice precisa ser preenchido. Para popular o índice, será necessário um `SearchIndexClient`. Há duas maneiras de obter um: construindo ou chamando `Indexes.GetClient` no `SearchServiceClient`. Usaremos o último por conveniência.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> Em um aplicativo típico de pesquisa, o gerenciamento e o preenchimento do índice é tratado por um componente separado das consultas de pesquisa. `Indexes.GetClient` é conveniente para preencher um índice porque poupa o trabalho de fornecer outras `SearchCredentials`. Ele faz isso passando a chave de administrador que você usou para criar o `SearchServiceClient` ao novo `SearchIndexClient`. No entanto, na parte do aplicativo que executa consultas, é melhor criar o `SearchIndexClient` diretamente para que você possa passar uma chave de consulta em vez de uma chave de administrador. Isso está de acordo com o princípio de privilégios mínimos e ajuda a tornar seu aplicativo mais seguro. Você pode saber mais sobre chaves de administração e chaves de consulta [aqui](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Agora que temos um `SearchIndexClient`, podemos preencher o índice. População do índice é feita por outro método que abordaremos mais tarde.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Por fim, executamos algumas consultas de pesquisa e exibimos os resultados. Dessa vez, usamos outro `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Posteriormente, faremos uma análise mais detalhada do método `RunQueries`. Este é o código para criar o novo `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Dessa vez, usaremos uma chave de consulta, já que não precisamos de acesso de gravação para o índice. Você pode inserir essas informações no arquivo `appsettings.json` do [aplicativo de exemplo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Se você executar esse aplicativo com um nome de serviço válido e chaves de API, a saída deve ser semelhante a este exemplo:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

O código-fonte completo do aplicativo é fornecido ao final deste artigo.

Em seguida, analisaremos com mais detalhes cada um dos métodos chamados pelo `Main`.

### <a name="creating-an-index"></a>Criando um índice
Depois de criar uma `SearchServiceClient`, `Main` exclui o índice "hotéis", caso ele já exista. Essa exclusão é feito pelo método a seguir:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Esse método usa o `SearchServiceClient` fornecido para verificar se o índice existe e, nesse caso, excluí-lo.

> [!NOTE]
> O exemplo código deste artigo usa os métodos síncronos do SDK do .NET do Azure Search para manter a simplicidade. Recomendamos que você use os métodos assíncronos em seus próprios aplicativos para mantê-los escalonáveis e responsivos. Por exemplo, no método acima você pode usar `ExistsAsync` e `DeleteAsync` em vez de `Exists` e `Delete`.
> 
> 

Em seguida, `Main` cria um novo índice "hotéis" chamando este método:

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

Esse método cria um novo objeto `Index` com uma lista de objetos `Field` que definem o esquema do novo índice. Cada campo tem um nome, tipo de dados e vários atributos que definem seu comportamento de pesquisa. A classe `FieldBuilder` usa a reflexão para criar uma lista de objetos `Field` para o índice, examinando os atributos e propriedades públicas da classe do modelo `Hotel` determinada. Posteriormente, faremos uma análise mais detalhada da classe `Hotel`.

> [!NOTE]
> Você pode criar quando desejar a lista de objetos `Field` diretamente, em vez de usar o `FieldBuilder` se necessário. Por exemplo, você pode não querer usar uma classe de modelo ou pode precisar usar uma classe de modelo existente que não deseja modificar adicionando atributos.
>
> 

Além dos campos, você também pode adicionar perfis de pontuação, sugestões ou opções de CORS para o índice (esses parâmetros são omitidos do exemplo para fins de brevidade). Você pode saber mais sobre o objeto Index e suas partes na [referência do SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), bem como na [Referência da API REST do Azure Search](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>Preenchendo o índice
A próxima etapa no `Main` é popular o índice recém-criado. A população do índice é feita no método a seguir:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        { 
            HotelId = "1", 
            BaseRate = 199.0, 
            Description = "Best hotel in town",
            DescriptionFr = "Meilleur hôtel en ville",
            HotelName = "Fancy Stay",
            Category = "Luxury", 
            Tags = new[] { "pool", "view", "wifi", "concierge" },
            ParkingIncluded = false, 
            SmokingAllowed = false,
            LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
            Rating = 5, 
            Location = GeographyPoint.Create(47.678581, -122.131577)
        },
        new Hotel()
        { 
            HotelId = "2", 
            BaseRate = 79.99,
            Description = "Cheapest hotel in town",
            DescriptionFr = "Hôtel le moins cher en ville",
            HotelName = "Roach Motel",
            Category = "Budget",
            Tags = new[] { "motel", "budget" },
            ParkingIncluded = true,
            SmokingAllowed = true,
            LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
            Rating = 1,
            Location = GeographyPoint.Create(49.678581, -122.131577)
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

Este método tem quatro partes. O primeiro cria uma matriz de objetos `Hotel` que servirá como nossos dados de entrada para carregamento do índice. Esses dados são codificados para manter a simplicidade. Em seu próprio aplicativo, provavelmente seus dados virão de uma fonte de dados externa, como um banco de dados SQL.

A segunda parte cria um `IndexBatch` com os documentos. Especifique a operação que você quer aplicar ao lote no momento da criação dele, nesse caso, chamando `IndexBatch.Upload`. O lote é então carregado para o índice do Azure Search pelo método `Documents.Index` .

> [!NOTE]
> Neste exemplo, estamos carregando apenas documentos. Se você quiser mesclar alterações em documentos existentes ou excluir documentos, poderá criar lotes chamando `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` ou `IndexBatch.Delete`. Também é possível combinar diferentes operações em um único lote chamando `IndexBatch.New`, que usa uma coleção de objetos `IndexAction`, com cada um deles informando ao Azure Search para executar uma operação específica em um documento. Você pode criar cada `IndexAction` com sua própria operação chamando o método correspondente como `IndexAction.Merge`, `IndexAction.Upload` e assim por diante.
> 
> 

A terceira parte desse método é um bloco catch que trata um caso de erro importante para indexação. Se o serviço de Azure Search não indexar alguns documentos no lote, uma `IndexBatchException` será lançada por `Documents.Index`. Essa exceção poderá ocorrer se você estiver indexando documentos enquanto o serviço estiver sob carga pesada. **É altamente recomendável a manipulação explícita desse caso em seu código.** Você pode atrasar e repetir a indexação de documentos que falharam, ou você pode registrar em log e continuar, como faz o exemplo, ou pode alguma outra coisa, dependendo dos requisitos de consistência de dados do aplicativo.

> [!NOTE]
> Você pode usar o método [`FindFailedActionsToRetry`](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) para construir um novo lote contendo apenas as ações que falharam em uma chamada anterior para `Index`. Há uma discussão sobre como usá-lo corretamente no [StackOverflow](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Por fim, o método `UploadDocuments` atrasa por dois segundos. A indexação ocorre de maneira assíncrona em seu serviço de Azure Search, portanto, o exemplo de aplicativo precisa aguardar alguns instantes para garantir que os documentos estejam disponíveis para pesquisa. Normalmente, atrasos como esses só são necessários em demonstrações, testes e exemplos de aplicativos.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>Como o SDK do .NET lida com documentos
Você pode estar se perguntando como o SDK do .NET do Azure Search é capaz de carregar instâncias de uma classe definida pelo usuário, como `Hotel` , no índice. Para ajudar a responder a essa pergunta, vamos examinar a classe `Hotel` :

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

A primeira coisa a observar é que cada propriedade pública de `Hotel` corresponde a um campo na definição do índice, mas com uma diferença fundamental: o nome de cada campo começa com uma letra minúscula ("minúsculas concatenadas"), enquanto o nome de cada propriedade pública de `Hotel` começa com uma letra maiúscula ("maiúsculas concatenadas"). Esse cenário é comum em aplicativos .NET que executam associação de dados em que o esquema de destino está fora do controle do desenvolvedor do aplicativo. Em vez de violar as diretrizes de nomenclatura do .NET, usando minúscula para os nomes de propriedade, você pode informar ao SDK para mapear automaticamente os nomes de propriedade como minúscula com o atributo `[SerializePropertyNamesAsCamelCase]` .

> [!NOTE]
> O SDK do .NET de Azure Search usa a biblioteca [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) para serializar e desserializar os objetos de modelo personalizados para e a partir do JSON. Se necessário, você pode personalizar essa serialização. Para obter mais informações, consulte [serialização personalizada com JSON.NET](#JsonDotNet).
> 
> 

A segunda coisa a observar é os atributos que decoram cada propriedade pública (como `IsFilterable`, `IsSearchable`, `Key`, e `Analyzer`). Esses atributos são mapeados diretamente para os [atributos correspondentes do índice da Azure Search](https://docs.microsoft.com/rest/api/searchservice/create-index#request). O `FieldBuilder` classe usa essas propriedades para criar definições de campo para o índice.

O terceiro fator importante sobre o `Hotel` classe é os tipos de dados das propriedades públicas. Os tipos .NET dessas propriedades estão mapeados para seus tipos de campo equivalentes na definição do índice. Por exemplo, a propriedade de cadeia de caracteres `Category` mapeia para o campo `category`, que é do tipo `Edm.String`. Há mapeamentos de tipo semelhantes entre `bool?` e `Edm.Boolean`, `DateTimeOffset?` e `Edm.DateTimeOffset` etc. As regras específicas para o mapeamento de tipos estão documentadas com o método `Documents.Get` na [referência do SDK do .NET do Azure Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get). A classe `FieldBuilder` cuida desse mapeamento para você, mas ainda pode ser útil para entender caso você precise solucionar problemas de serialização.

Essa capacidade de usar suas próprias classes como documentos funciona em ambas as direções; Você também pode recuperar os resultados da pesquisa e fazer com que o SDK os desserialize automaticamente para um tipo de sua escolha, como veremos na próxima seção.

> [!NOTE]
> O SDK do .NET do Azure Search também oferece suporte a documentos do tipo dinâmico usando a classe `Document`, que é um mapeamento de chave/valor de nomes de campo para valores de campo. Isso é útil em cenários nos quais você não conhece o esquema de índice no momento do design, ou nos quais seria inconveniente associar a classes de modelo específico. Todos os métodos no SDK que lidam com documentos têm sobrecargas que funcionam com a classe `Document` , bem como sobrecargas fortemente tipadas que utilizam um parâmetro de tipo genérico. Somente as últimas são usadas no exemplo de código deste tutorial. A [`Document` classe](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) herda de `Dictionary<string, object>`.
> 
>

**Por que você deve usar tipos de dados anuláveis**

Ao criar suas próprias classes de modelo para mapear para um índice de Azure Search, sugerimos declarar as propriedades dos tipos de valor como `bool` e `int` para serem anuláveis (por exemplo, `bool?` em vez de `bool`). Se você usar uma propriedade não anulável, será preciso **assegurar** que nenhum documento no índice contenha um valor nulo para o campo correspondente. Nem o SDK, nem o serviço Azure Search ajudarão você a impor isso.

Isso não é apenas uma preocupação hipotética: imagine um cenário em que você adiciona um novo campo a um índice existente do tipo `Edm.Int32`. Depois de atualizar a definição de índice, todos os documentos terão um valor nulo para esse novo campo (já que todos os tipos são anuláveis no Azure Search). Ao usar uma classe de modelo com uma propriedade não anulável `int` para esse campo, você obterá uma `JsonSerializationException` como esta ao tentar recuperar os documentos:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Por esse motivo, sugerimos que você use tipos anuláveis nas suas classes de modelo como uma prática recomendada.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Serialização personalizada com JSON.NET
O SDK usa JSON.NET para serializar e desserializar documentos. Você pode personalizar a serialização e desserialização, se necessário, definindo seu próprio `JsonConverter` ou `IContractResolver`. Para obter mais informações, consulte o [documentação do JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm). Isso pode ser útil quando você quer adaptar uma classe de modelo existente de seu aplicativo para usar com o Azure Search e outros cenários mais avançados. Por exemplo, com a serialização personalizada, você pode:

* Incluir ou excluir determinadas propriedades da sua classe de modelo para serem armazenadas como campos de documento.
* Mapear os nomes de propriedade no código aos nomes de campo no índice.
* Crie atributos personalizados que podem ser usados para mapear propriedades para campos do documento.

Você pode encontrar exemplos de implementação de serialização personalizada nos testes de unidade para o SDK .NET do Azure Search no GitHub. Um bom ponto de partida é [esta pasta](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Ela contém classes que são usadas pelos testes de serialização personalizada.

### <a name="searching-for-documents-in-the-index"></a>Pesquisando documentos no índice
A última etapa no aplicativo de exemplo é procurar por documentos no índice:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "lastRenovationDate desc" },
            Select = new[] { "hotelName", "lastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

Cada vez que ele executa uma consulta, esse método cria, primeiro, um novo objeto `SearchParameters`. Esse objeto é usado para especificar opções adicionais para a consulta como classificação, filtragem, paginação e facetas. Nesse método, estamos definindo as propriedades de `Filter`, `Select`, `OrderBy` e `Top` para consultas diferentes. Todas as propriedades de `SearchParameters` são documentadas [aqui](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

A próxima etapa é executar a consulta de pesquisa. Executando a pesquisa é feita usando o `Documents.Search` método. Para cada consulta, passamos o texto de pesquisa a ser usado como uma cadeia de caracteres (ou `"*"` se não houver um texto de pesquisa) e também os parâmetros de pesquisa criados anteriormente. Também podemos especificar `Hotel` como o parâmetro de tipo para `Documents.Search`, que informa ao SDK para desserializar documentos nos resultados da pesquisa em objetos do tipo `Hotel`.

> [!NOTE]
> Você pode saber mais sobre a sintaxe de expressão da consulta de pesquisa [aqui](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Por fim, depois de cada consulta, esse método percorre todas as correspondências nos resultados da pesquisa, imprimindo cada documento no console:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Vejamos cada uma das consultas com mais detalhes. Este é o código para executar a primeira consulta:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

Neste caso, estamos pesquisando hotéis que correspondam à palavra "budget" e queremos nos resultados apenas os nomes dos hotéis, conforme especificado pelo parâmetro `Select`. Estes são os resultados:

    Name: Roach Motel

Em seguida, queremos encontrar hotéis com uma diária inferior a US$ 150 e obter resultados somente com a ID e descrição dos hotéis:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Esta consulta usa uma expressão `$filter` do OData, `baseRate lt 150`, para filtrar os documentos no índice. Você pode saber mais sobre a sintaxe do OData com suporte do Azure Search [aqui](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Estes são alguns dos resultados da consulta:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

Em seguida, queremos localizar os dois principais hotéis reformados mais recentemente e mostrar o nome dos hotéis e a data da última reforma. Eis o código: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Neste caso, usaremos novamente a sintaxe do OData para especificar o parâmetro `OrderBy` como `lastRenovationDate desc`. Também vamos definir `Top` como 2 para garantir que só iremos receber os dois principais documentos. Como antes, definimos `Select` para especificar quais campos devem ser retornados.

Estes são os resultados:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Por fim, queremos encontrar todos os hotéis que correspondam à palavra "motel":

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Estes são os resultados, que incluem todos os campos uma vez que não especificamos a propriedade `Select`:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Essa etapa conclui o tutorial, mas não pare aqui. * * Próximas etapas fornecem recursos adicionais para aprender mais sobre o Azure Search.

## <a name="next-steps"></a>Próximas etapas
* Procure as referências para o [SDK do .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) e a [API REST](https://docs.microsoft.com/rest/api/searchservice/).
* Revise as [convenções de nomenclatura](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) para saber as regras de nomeação de vários objetos.
* Examine os [tipos de dados com suporte](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) no Azure Search.
