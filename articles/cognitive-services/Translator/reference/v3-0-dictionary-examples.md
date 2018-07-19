---
title: Método Dictionary Examples da API de Tradução de Texto da Microsoft | Microsoft Docs
description: Use o método Dictionary Examples da API de Tradução de Texto da Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 9960f3be42090edaec1df935d70e4c1a0d25b691
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35364550"
---
# <a name="text-api-30-dictionary-examples"></a>API de Texto 3.0: Dictionary Examples

Fornece exemplos que mostram como os termos no dicionário são usados no contexto. Esta operação é usada em conjunto com a [Pesquisa no dicionário](.\v3-0-dictionary-lookup.md).

## <a name="request-url"></a>URL de Solicitação

Envie uma solicitação `POST` para:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>Parâmetros da solicitação

Os parâmetros de solicitação passados na cadeia de caracteres de consulta são:

<table width="100%">
  <th width="20%">Parâmetro de consulta</th>
  <th>DESCRIÇÃO</th>
  <tr>
    <td>api-version</td>
    <td>*Parâmetro necessário*.<br/>Versão da API solicitada pelo cliente. O valor precisa ser `3.0`.</td>
  </tr>
  <tr>
    <td>de</td>
    <td>*Parâmetro necessário*.<br/>Especifica o idioma do texto de entrada. O idioma de origem deve ser um dos [idiomas compatíveis](.\v3-0-languages.md) incluídos no escopo de `dictionary`.</td>
  </tr>
  <tr>
    <td>para</td>
    <td>*Parâmetro necessário*.<br/>Especifica o idioma do texto de saída. O idioma de destino deve ser um dos [idiomas compatíveis](.\v3-0-languages.md) incluídos no escopo de `dictionary`.</td>
  </tr>
</table>

Os cabeçalhos da solicitação incluem:

<table width="100%">
  <th width="20%">Cabeçalhos</th>
  <th>DESCRIÇÃO</th>
  <tr>
    <td>_Uma autorização_<br/>_cabeçalho_</td>
    <td>*Cabeçalho de solicitação obrigatório*.<br/>Veja [Opções disponíveis para autenticação](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Tipo de conteúdo</td>
    <td>*Cabeçalho de solicitação obrigatório*.<br/>Especifica o tipo de conteúdo da carga. Os valores possíveis são: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Cabeçalho de solicitação obrigatório*.<br/>O tamanho do corpo da solicitação.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Opcional*.<br/>Um GUID gerado pelo cliente para identificar exclusivamente a solicitação. Você pode omitir esse cabeçalho se incluir a ID de rastreamento na cadeia de caracteres de consulta usando um parâmetro de consulta chamado `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação é uma matriz JSON. Cada elemento da matriz é um objeto JSON com as seguintes propriedades:

  * `Text`: uma cadeia de caracteres especificando o termo da pesquisa. Deve ser o valor de um campo `normalizedText` de das traduções reversas da solicitação de [Pesquisa no dicionário](.\v3-0-dictionary-lookup.md) anterior. Também pode ser o valor do campo `normalizedSource`.

  * `Translation`: uma cadeia de caracteres especificando o texto traduzido retornado anteriormente pela operação [Pesquisa no dicionário](.\v3-0-dictionary-lookup.md). Deve ser o valor do campo `normalizedTarget` na lista `translations` da resposta [Pesquisa no dicionário](.\v3-0-dictionary-lookup.md). O serviço retornará exemplos para o par de palavras de origem-destino específico.

Um exemplo é:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

As seguintes limitações se aplicam:

* A matriz pode ter no máximo 10 elementos.
* O valor de texto de um elemento de matriz não pode exceder 100 caracteres incluindo espaços.

## <a name="response-body"></a>Corpo da resposta

Uma resposta bem-sucedida é uma matriz JSON com um resultado para cada cadeia de caracteres na matriz de entrada. Um objeto de resultado inclui as seguintes propriedades:

  * `normalizedSource`: uma cadeia de caracteres fornecendo o formulário normalizado do termo de origem. Em geral, isso deve ser idêntico ao valor do campo `Text` no índice da lista correspondente no corpo da solicitação.
    
  * `normalizedTarget`: uma cadeia de caracteres fornecendo o formulário normalizado do termo de destino. Em geral, isso deve ser idêntico ao valor do campo `Translation` no índice da lista correspondente no corpo da solicitação.
  
  * `examples`: uma lista de exemplos para o par (termo de origem, termo de destino). Cada elemento da lista é um objeto com as seguintes propriedades:

    * `sourcePrefix`: a cadeia de caracteres para concatenar _antes_ do valor de `sourceTerm` para formar um exemplo completo. Não adicione um caractere de espaço, pois ele já estará lá quando precisar estar. Esse valor pode ser uma cadeia de caracteres vazia.

    * `sourceTerm`: uma cadeia de caracteres igual ao termo real é pesquisada. A cadeia de caracteres é adicionada com `sourcePrefix` e `sourceSuffix` para formar o exemplo completo. Seu valor é separado para que possa ser marcado em uma interface do usuário, por exemplo, colocando-o em negrito.

    * `sourceSuffix`: a cadeia de caracteres para concatenar _após_ o valor de `sourceTerm` para formar um exemplo completo. Não adicione um caractere de espaço, pois ele já estará lá quando precisar estar. Esse valor pode ser uma cadeia de caracteres vazia.

    * `targetPrefix`: uma cadeia de caracteres semelhante a `sourcePrefix`, mas para o destino.

    * `targetTerm`: uma cadeia de caracteres semelhante a `sourceTerm`, mas para o destino.

    * `targetSuffix`: uma cadeia de caracteres semelhante a `sourceSuffix`, mas para o destino.

    > [!NOTE]
    > Se não houver nenhum exemplo no dicionário, a resposta será 200 (OK), mas a lista `examples` será uma lista vazia.

## <a name="examples"></a>Exemplos

Este exemplo mostra como pesquisar exemplos para o par composto pelo termo em inglês `fly` e sua tradução em espanhol `volar`.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

O corpo da resposta (abreviado para maior clareza) é:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```