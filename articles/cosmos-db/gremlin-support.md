---
title: Suporte do Azure Cosmos DB para Gremlin
description: Saiba mais sobre a linguagem Gremlin do Apache TinkerPop. Saiba quais etapas e recursos estão disponíveis no Azure Cosmos DB
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 05/21/2019
ms.author: lbosq
ms.openlocfilehash: b36c041c24a07f89701e78aea4d08270342b8d22
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978945"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Suporte do Azure Cosmos DB para grafo do Gremlin
Azure Cosmos DB suporta a linguagem transversal de gráficos [Apache Tinkerpop](https://tinkerpop.apache.org), conhecida como gráfica [Gremlin](https://tinkerpop.apache.org/docs/3.3.2/reference/#graph-traversal-steps). É possível usar a linguagem Gremlin para criar entidades de grafo (vértices e bordas), modificar propriedades dentro dessas entidades, executar consultas e passagens e excluir entidades. 

O Azure Cosmos DB traz recursos prontos para empresas para bancos de dados de grafo. Esses recursos incluem distribuição global, dimensionamento independente do armazenamento e da taxa de transferência, latências de milissegundos de dígito único previsíveis, indexação automática e SLAs, leitura disponível para contas de bancos de dados abrangendo duas ou mais regiões do Azure. Como o Azure Cosmos DB dá suporte a TinkerPop/Gremlin, você pode migrar com facilidade aplicativos escritos usando outro banco de dados de gráfico compatível. Além disso, devido ao suporte para Gremlin, o Azure Cosmos DB integra-se perfeitamente com estruturas de análise habilitadas para TinkerPop, como o [Apache Spark GraphX](https://spark.apache.org/graphx/). 

Neste artigo, fornecemos instruções passo a passo rápidas do Gremlin e enumeramos os recursos do Gremlin compatíveis com a API do Gremlin.

## <a name="gremlin-by-example"></a>Gremlin pelo exemplo
Vamos usar um grafo de exemplo para entender como as consultas podem ser expressas no Gremlin. A figura a seguir mostra um aplicativo de negócios que gerencia dados sobre usuários, interesses e dispositivos na forma de um grafo.  

![Banco de dados de exemplo mostrando interesses, dispositivos e pessoas](./media/gremlin-support/sample-graph.png) 

Este grafo tem os seguintes tipos de vértice (chamados de "rótulo" no Gremlin):

- Pessoas: O grafo tem três pessoas; Robin, Thomas e Ben
- Interesses: Os interesses dessas pessoas, que neste exemplo é o jogo de futebol
- Dispositivos: Os dispositivos que as pessoas usam
- Sistemas operacionais: Os sistemas operacionais em que os dispositivos executam

Nós representamos as relações entre essas entidades usando os seguintes tipos/rótulos de borda:

- Conhece: Por exemplo, "Thomas conhece Robin"
- Interessado: Para representar os interesses das pessoas em nosso grafo, por exemplo, "Ben está interessado em futebol"
- RunsOS: O laptop executa o sistema operacional Windows
- Usa: Para representar qual dispositivo uma pessoa usa. Por exemplo, Robin usa um telefone Motorola com o número de série 77

Vamos executar algumas operações nesse grafo usando o [Console do Gremlin](https://tinkerpop.apache.org/docs/3.3.2/reference/#gremlin-console). Você também pode executar essas operações usando drivers do Gremlin na plataforma de sua escolha (Java, Node.js, Python ou .NET).  Antes de examinarmos o que tem suporte no BD Cosmos do Azure, vejamos alguns exemplos para nos familiarizarmos com a sintaxe.

Primeiro, vamos ver o CRUD. A seguinte instrução do Gremlin insere o vértice "Thomas" no grafo:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Em seguida, a seguinte instrução do Gremlin insere uma borda do tipo "conhece" entre Thomas e Robin.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

A consulta a seguir retorna os vértices do tipo "pessoa" na ordem decrescente de seus nomes:
```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Os grafos se destacam em situações em que você precisa responder perguntas como "Quais sistemas operacionais os amigos de Thomas usam?". Você pode executar esse transversal do Gremlin para obter informações do gráfico:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Agora, vejamos o que o BD Cosmos do Azure oferece para desenvolvedores de Gremlin.

## <a name="gremlin-features"></a>Recursos do Gremlin
O TinkerPop é um padrão que abrange uma ampla variedade de tecnologias de grafo. Portanto, ele tem uma terminologia padrão para descrever quais recursos são fornecidos por um provedor de grafo. O Azure Cosmos DB fornece um banco de dados de grafo gravável, persistente e de alta simultaneidade que pode ser particionado em vários servidores ou clusters. 

A tabela a seguir lista os recursos do TinkerPop que são implementados pelo BD Cosmos do Azure: 

| Categoria | Implementação do BD Cosmos do Azure |  Observações | 
| --- | --- | --- |
| Recursos de grafo | Fornece persistência e ConcurrentAccess. Projetado para dar suporte a Transactions | Métodos de computador podem ser implementados por meio do conector Spark. |
| Recursos variáveis | Dá suporte a Boolean, Integer, Byte, Double, Float, Integer, Long, String | Dá suporte a tipos primitivos, é compatível com tipos complexos por meio do modelo de dados |
| Recursos do vértice | Dá suporte a RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty  | Dá suporte à criação, modificação e exclusão de vértices |
| Recursos de propriedade do vértice | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Dá suporte à criação, modificação e exclusão de propriedades do vértice |
| Recursos da borda | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Dá suporte à criação, modificação e exclusão de bordas |
| Recursos de propriedade da borda | Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Dá suporte à criação, modificação e exclusão de propriedades da borda |

## <a name="gremlin-wire-format-graphson"></a>Formato de transmissão do Gremlin: GraphSON

O Azure Cosmos DB usa o [formato GraphSON](https://tinkerpop.apache.org/docs/3.3.2/reference/#graphson-reader-writer) ao retornar resultados de operações Gremlin. GraphSON é o formato padrão do Gremlin para representar vértices, bordas e propriedades (propriedades com um ou vários valores) usando JSON. 

Por exemplo, o snippet a seguir mostra uma representação em GraphSON de um vértice *retornado ao cliente* no Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

As propriedades usadas por GraphSON para vértices são descritas a seguir:

| Propriedade | DESCRIÇÃO | 
| --- | --- | --- |
| `id` | A ID do vértice. Deve ser exclusiva (em combinação com o valor de `_partition`, se aplicável). Se nenhum valor for fornecido, ele será automaticamente fornecido com um GUID | 
| `label` | O rótulo do vértice. É usado para descrever o tipo de entidade. |
| `type` | Usado para distinguir vértices de documentos que não são grafos |
| `properties` | Recipiente de propriedades definidas pelo usuário associadas ao vértice. Cada propriedade pode ter vários valores. |
| `_partition` | A chave de partição do vértice. Usado para [particionamento de gráfico](graph-partitioning.md). |
| `outE` | Essa propriedade contém uma lista de bordas externas de um vértice. Armazenar as informações de adjacência com o vértice permite a execução rápida de passagens. As bordas são agrupadas com base em seus rótulos. |

E a borda contém as seguintes informações para ajudar com a navegação para outras partes do grafo.

| Propriedade | DESCRIÇÃO |
| --- | --- |
| `id` | A ID da borda. Deve ser exclusiva (em combinação com o valor de `_partition`, se aplicável) |
| `label` | O rótulo da borda. Esta propriedade é opcional e é usada para descrever o tipo de relacionamento. |
| `inV` | Ela contém uma lista nos vértices de uma borda. Armazenar as informações de adjacência com a borda permite a execução rápida das passagens. Os vértices são agrupados com base em seus rótulos. |
| `properties` | Recipiente de propriedades definidas pelo usuário associadas à borda. Cada propriedade pode ter vários valores. |

Cada propriedade pode armazenar diversos valores em uma matriz. 

| Propriedade | DESCRIÇÃO |
| --- | --- |
| `value` | O valor da propriedade

## <a name="gremlin-steps"></a>Etapas do Gremlin
Agora, vejamos as etapas do Gremlin com suporte do BD Cosmos do Azure. Para obter uma referência completa sobre o Gremlin, consulte [Referência do TinkerPop](https://tinkerpop.apache.org/docs/3.3.2/reference).

| Etapa | DESCRIÇÃO | Documentação do TinkerPop 3.2 |
| --- | --- | --- |
| `addE` | Adiciona uma borda entre dois vértices | [Etapa addE](https://tinkerpop.apache.org/docs/3.3.2/reference/#addedge-step) |
| `addV` | Adiciona um vértice ao grafo | [Etapa addV](https://tinkerpop.apache.org/docs/3.3.2/reference/#addvertex-step) |
| `and` | Garante que todas as passagens retornem um valor | [e uma etapa](https://tinkerpop.apache.org/docs/3.3.2/reference/#and-step) |
| `as` | Um modulador de etapa para atribuir uma variável à saída de uma etapa | [Etapa as](https://tinkerpop.apache.org/docs/3.3.2/reference/#as-step) |
| `by` | Um modulador de etapa usado com `group` e `order` | [Etapa by](https://tinkerpop.apache.org/docs/3.3.2/reference/#by-step) |
| `coalesce` | Retorna a primeira passagem que retorna um resultado | [Etapa coalesce](https://tinkerpop.apache.org/docs/3.3.2/reference/#coalesce-step) |
| `constant` | Retorna um valor constante. Usada com `coalesce`| [Etapa constant](https://tinkerpop.apache.org/docs/3.3.2/reference/#constant-step) |
| `count` | Retorna a contagem da passagem | [Etapa count](https://tinkerpop.apache.org/docs/3.3.2/reference/#count-step) |
| `dedup` | Retorna os valores com as duplicatas removidas | [Etapa dedup](https://tinkerpop.apache.org/docs/3.3.2/reference/#dedup-step) |
| `drop` | Remove os valores (vértice/borda) | [Etapa drop](https://tinkerpop.apache.org/docs/3.3.2/reference/#drop-step) |
| `executionProfile` | Cria uma descrição de todas as operações gerada pela etapa executada Gremlin | [etapa executionProfile](graph-execution-profile.md) |
| `fold` | Atua como uma barreira que calcula o valor agregado dos resultados| [Etapa fold](https://tinkerpop.apache.org/docs/3.3.2/reference/#fold-step) |
| `group` | Agrupa os valores com base nos rótulos especificados| [Etapa group](https://tinkerpop.apache.org/docs/3.3.2/reference/#group-step) |
| `has` | Usada para filtrar propriedades, vértices e bordas. Dá suporte às variantes `hasLabel`, `hasId`, `hasNot` e `has`. | [Etapa has](https://tinkerpop.apache.org/docs/3.3.2/reference/#has-step) |
| `inject` | Insere valores em um fluxo| [Etapa inject](https://tinkerpop.apache.org/docs/3.3.2/reference/#inject-step) |
| `is` | Usada para executar um filtro usando uma expressão booliana | [Etapa is](https://tinkerpop.apache.org/docs/3.3.2/reference/#is-step) |
| `limit` | Usada para limitar o número de itens na passagem| [Etapa limit](https://tinkerpop.apache.org/docs/3.3.2/reference/#limit-step) |
| `local` | A etapa local encapsula uma seção de uma passagem, de forma semelhante a uma subconsulta | [Etapa local](https://tinkerpop.apache.org/docs/3.3.2/reference/#local-step) |
| `not` | Usada para produzir a negação de um filtro | [Etapa not](https://tinkerpop.apache.org/docs/3.3.2/reference/#not-step) |
| `optional` | Retorna o resultado da passagem especificada se ela produzir um resultado. Caso contrário, retorna o elemento de chamada | [Etapa optional](https://tinkerpop.apache.org/docs/3.3.2/reference/#optional-step) |
| `or` | Garante que pelo menos uma das passagens retorne um valor | [Etapa or](https://tinkerpop.apache.org/docs/3.3.2/reference/#or-step) |
| `order` | Retorna os resultados na ordem de classificação especificada | [Etapa order](https://tinkerpop.apache.org/docs/3.3.2/reference/#order-step) |
| `path` | Retorna o caminho completo da passagem | [Etapa path](https://tinkerpop.apache.org/docs/3.3.2/reference/#path-step) |
| `project` | Projeta as propriedades como um mapa | [Etapa project](https://tinkerpop.apache.org/docs/3.3.2/reference/#project-step) |
| `properties` | Retorna as propriedades para os rótulos especificados | [Etapa properties](https://tinkerpop.apache.org/docs/3.3.2/reference/#_properties_step) |
| `range` | Filtra para o intervalo de valores especificado| [Etapa range](https://tinkerpop.apache.org/docs/3.3.2/reference/#range-step) |
| `repeat` | Repete a etapa o número de vezes especificado. Usada para efetuar loop | [Etapa repeat](https://tinkerpop.apache.org/docs/3.3.2/reference/#repeat-step) |
| `sample` | Usada para fazer a amostragem dos resultados da passagem | [Etapa sample](https://tinkerpop.apache.org/docs/3.3.2/reference/#sample-step) |
| `select` | Usada para projetar resultados da passagem |  [Etapa select](https://tinkerpop.apache.org/docs/3.3.2/reference/#select-step) |
| `store` | Usada para agregações sem bloqueio da passagem | [Etapa store](https://tinkerpop.apache.org/docs/3.3.2/reference/#store-step) |
| `TextP.startingWith(string)` | Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade com o início de uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.endingWith(string)` |  Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade com o final de uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.containing(string)` | Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade com o conteúdo de uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notStartingWith(string)` | Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade que não começa com uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notEndingWith(string)` | Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade que não termina com uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notContaining(string)` | Função de filtragem de cadeia de caracteres. Essa função é usada como um predicado para a etapa `has()` para corresponder uma propriedade que não contém uma determinada cadeia de caracteres | [Predicados TextP](http://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `tree` | Agrega os caminhos de um vértice em uma árvore | [Etapa tree](https://tinkerpop.apache.org/docs/3.3.2/reference/#tree-step) |
| `unfold` | Desenrola um iterador como uma etapa| [Etapa unfold](https://tinkerpop.apache.org/docs/3.3.2/reference/#unfold-step) |
| `union` | Mescla resultados de várias passagens| [Etapa union](https://tinkerpop.apache.org/docs/3.3.2/reference/#union-step) |
| `V` | Inclui as etapas necessárias para passagens entre vértices e bordas `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV` e `otherV` para | [Etapa vertex](https://tinkerpop.apache.org/docs/3.3.2/reference/#vertex-steps) |
| `where` | Usada para filtrar os resultados da passagem. Dá suporte aos operadores `eq`, `neq`, `lt`, `lte`, `gt`, `gte` e `between`  | [Etapa where](https://tinkerpop.apache.org/docs/3.3.2/reference/#where-step) |

O mecanismo otimizado para gravação do Azure Cosmos DB dá suporte à indexação automática de todas as propriedades dentro dos vértices e bordas por padrão. Sendo assim, consultas com filtros, consultas de intervalo, classificações ou agregações de qualquer propriedade são processadas no índice e atendidas de modo eficiente. Para obter mais informações sobre como a indexação funciona no BD Cosmos do Azure, consulte nosso artigo sobre [Indexação independente do esquema](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

## <a name="next-steps"></a>Próximas etapas
* Comece a compilar um aplicativo de grafo [usando nossos SDKs](create-graph-dotnet.md) 
* Para saber mais sobre o [suporte para grafo](graph-introduction.md) no Azure Cosmos DB
