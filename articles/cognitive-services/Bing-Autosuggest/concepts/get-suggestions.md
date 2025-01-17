---
title: Sugerindo termos de pesquisa com a API de Sugestão Automática do Bing
titlesuffix: Azure Cognitive Services
description: Saiba como usar a API de Sugestão Automática do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 7cd81a937a3fe24e16e3f77e38686378aa28e10a
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382692"
---
# <a name="suggesting-query-terms"></a>Sugestão de termos de consulta

Normalmente, você chamará a API de Sugestão Automática do Bing sempre que um usuário digitar um novo caractere na caixa de pesquisa do aplicativo. A integridade da cadeia de caracteres de consulta afeta a relevância dos termos de consulta sugeridos que a API retorna. Quanto mais completa a cadeia de caracteres de consulta, mais relevante é a lista de termos de consulta sugeridos. Por exemplo, as sugestões que a API poderá retornar para `s` provavelmente são menos relevantes do que as consultas retornadas para `sailing dinghies`.

## <a name="example-request"></a>Solicitação de exemplo

O exemplo a seguir mostra uma solicitação que retorna as cadeias de caracteres de consulta sugeridas para *navegar*. Lembre-se de codificar a URL do termo de consulta parcial do usuário quando você definir o parâmetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#query). Por exemplo, se o usuário inserir *sailing les*, defina `q` como `sailing+les` ou `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

A resposta a seguir contém uma lista de objetos de [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#searchaction) que contêm os termos de consulta sugeridos.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

## <a name="using-suggested-query-terms"></a>Usando os termos de consulta sugeridos

Cada sugestão inclui um campo `displayText`, `query` e `url`. O campo `displayText` contém a consulta sugerida que você usa para preencher a lista suspensa de sua caixa de pesquisa. Exiba todas as sugestões incluídas na resposta e na ordem determinada.

O exemplo a seguir mostra uma caixa de pesquisa suspensa com os termos de consulta sugeridos pela API de Sugestão Automática do Bing.

![Lista suspensa da caixa de pesquisa com sugestão automática](../media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Se o usuário selecionar uma consulta sugerida na lista suspensa, você poderá usar o termo de consulta no campo `query` para chamar a [API de Pesquisa na Web do Bing](../../bing-web-search/search-the-web.md) e exibir os resultados por conta própria. Ou você pode usar a URL no campo `url` para enviar o usuário à página de resultados da pesquisa do Bing.

## <a name="next-steps"></a>Próximas etapas

* [O que é a API de Sugestão Automática do Bing?](../get-suggested-search-terms.md)