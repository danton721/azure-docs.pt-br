---
title: 'Início Rápido: Converter script de texto, Go – API de Tradução de Texto'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você converterá texto em um idioma de um script para outro usando a API de Tradução de Texto com Go.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: 748754bdeec34d864efd05297664b935a06f1a06
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515205"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-go"></a>Início Rápido: Usar a API de Tradução de Texto para transliterar texto usando Go

Neste início rápido, você converter texto em um idioma de um script para outro usando a API de Tradução de Texto.

Este início rápido requer uma [Conta dos Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com um recurso de Tradução de Texto. Se não tiver uma conta, você poderá usar a [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/) para obter uma chave de assinatura.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido requer:

* [Go](https://golang.org/doc/install)
* Uma chave de assinatura do Azure para a Tradução de Texto

## <a name="create-a-project-and-import-required-modules"></a>Criar um projeto e importar os módulos necessários

Crie um novo projeto Go usando seu IDE ou editor favorito. Em seguida, copie esse trecho de código para seu projeto em um arquivo denominado `transliterate-text.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Criar a função principal

Este exemplo tentará ler a chave da sua assinatura de Tradução de Texto da variável de ambiente `TRANSLATOR_TEXT_KEY`. Se você não estiver familiarizado com as variáveis de ambiente,poderá definir `subscriptionKey` como uma cadeia de caracteres e comentar a instrução condicional.

Copie este código em seu projeto:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables. If so, be sure to delete the "os" import.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our transliterate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    transliterate(subscriptionKey)
}
```

## <a name="create-a-function-to-transliterate-text"></a>Criar uma função para transliterar texto

Vamos criar uma função para transliterar texto. Esta função usará um único argumento, sua chave de assinatura de Tradução de Texto.

```go
func transliterate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

Em seguida, vamos construir a URL. A URL é criada usando os métodos `Parse()` e `Query()`. Você observará que os parâmetros são adicionados com o método `Add()`. Neste exemplo, estamos transliterando do japonês para o alfabeto latino.

Copie este código para a função `transliterate`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0")
q := u.Query()
q.Add("language", "ja")
q.Add("fromScript", "jpan")
q.Add("toScript", "latn")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Para saber mais sobre pontos de extremidade, rotas e parâmetros de solicitação, confira [API de Tradução de Texto 3.0: Transliterar](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

## <a name="create-a-struct-for-your-request-body"></a>Criar um struct para o corpo da solicitação

Em seguida, crie uma estrutura anônima para o corpo da solicitação e codifique-o como JSON com `json.Marshal()`. Adicione este código à função `transliterate`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "こんにちは"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Criar a solicitação

Agora que você codificou o corpo da solicitação como JSON, pode criar sua solicitação POST e chamar a API de Tradução de Texto.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Processar e imprimir a resposta

Adicione este código à função `transliterate` para decodificar a resposta JSON e, em seguida, formate e imprima o resultado.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Colocar tudo isso junto

É isso; você montou um programa simples que chamará a API de Tradução de Texto e retornará uma resposta JSON. Agora é hora de executar o programa:

```console
go run transliterate-text.go
```

Se você quiser comparar seu código com o nosso, o exemplo completo está disponível no [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Resposta de exemplo

```json
[
  {
    "script": "latn",
    "text": "konnichiwa"
  }
]
```

## <a name="next-steps"></a>Próximas etapas

Explore pacotes Go para APIs de Serviços Cognitivos do [SDK do Azure para linguagem Go](https://github.com/Azure/azure-sdk-for-go) no GitHub.

> [!div class="nextstepaction"]
> [Explore pacotes Go no GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)

## <a name="see-also"></a>Consulte também

Saiba como usar a API de Tradução de Texto para:

* [Traduzir o texto](quickstart-go-translate.md)
* [Identificar a linguagem pela entrada](quickstart-go-detect.md)
* [Obter traduções alternativas](quickstart-go-dictionary.md)
* [Obter uma lista de idiomas com suporte](quickstart-go-languages.md)
* [Determinar o comprimento da frase em uma entrada](quickstart-go-sentences.md)
