---
title: 'Início Rápido: Traduzir texto, C# – Tradução de Texto'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você traduzirá o texto de um idioma para outro usando a API de Tradução de Texto com C#.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: e59e634b04a55a0c7a0fd555b09404545bd26c60
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514940"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-using-c"></a>Início Rápido: Usar a API de Tradução de Texto para converter uma cadeia de caracteres usando C#

Neste início rápido, você aprenderá a converter uma cadeia de texto de inglês para italiano e alemão usando .NET Core e a API REST de Tradução de Texto.

Este início rápido requer uma [Conta dos Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com um recurso de Tradução de Texto. Se não tiver uma conta, você poderá usar a [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/) para obter uma chave de assinatura.

## <a name="prerequisites"></a>Pré-requisitos

* [SDK .NET](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Pacote NuGet do Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) ou seu editor de texto favorito
* Uma chave de assinatura do Azure para a Tradução de Texto

## <a name="create-a-net-core-project"></a>Criar projeto do .NET Core

Abra um novo prompt de comando (ou a sessão de terminal) e execute estes comandos:

```console
dotnet new console -o translate-sample
cd translate-sample
```

O primeiro comando faz duas coisas. Ele cria um novo aplicativo de console .NET e cria um diretório chamado `translate-sample`. O segundo comando é alterado para o diretório do seu projeto.

Em seguida, você precisará instalar o Json.Net. No diretório do projeto, execute:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Adicionar namespaces necessários ao seu projeto

O comando `dotnet new console` que você executou anteriormente criou um projeto, incluindo `Program.cs`. Esse arquivo é onde você colocará o código do aplicativo. Abra `Program.cs` e substitua o existente usando as instruções existentes. Essas instruções garantem que você tenha acesso a todos os tipos necessários para compilar e executar o aplicativo de exemplo.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-translate-text"></a>Criar uma função para traduzir texto

Na classe `Program`, crie uma função chamada `TranslateText`. Essa classe encapsula o código usado para chamar o recurso Traduzir e imprime o resultado no console.

```csharp
static void TranslateText()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Definir a chave de assinatura, o nome do host e o caminho

Adicione essas linhas à função `TranslateText`. Você observará que, juntamente com a `api-version`, dois parâmetros adicionais foram anexados à `route`. Esses parâmetros são usados para definir as saídas da tradução. Neste exemplo, está definido como alemão (`de`) e italiano (`it`). Verifique se você atualizou o valor de chave de assinatura.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/translate?api-version=3.0&to=de&to=it";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Em seguida, precisaremos criar e serializar o objeto JSON que inclui o texto a ser traduzido. Tenha em mente que você pode transmitir mais de um objeto na matriz `body`.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Hello world!" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Instanciar o cliente e fazer uma solicitação

Essas linhas instanciam `HttpClient` e `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Construir a solicitação e imprimir a resposta

Dentro de `HttpRequestMessage`, você vai:

* Declarar o método HTTP
* Construir o URI de solicitação
* Inserir o corpo da solicitação (objeto JSON serializado)
* Adicionar os cabeçalhos necessários
* Fazer uma solicitação assíncrona
* Imprima a resposta

Adicione este código a `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Colocar tudo isso junto

A última etapa é chamar `TranslateText()` na função `Main`. Localize `static void Main(string[] args)` e adicione estas linhas:

```csharp
TranslateText();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Executar o aplicativo de exemplo

E, pronto, você já pode executar seu aplicativo de exemplo. Na linha de comando (ou sessão de terminal), navegue até o diretório do projeto e execute:

```console
dotnet run
```

## <a name="sample-response"></a>Resposta de exemplo

Localize a abreviação do país/região nesta [lista de idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="clean-up-resources"></a>Limpar recursos

Remova todas as informações confidenciais do código-fonte do seu aplicativo de exemplo, como as chaves de assinatura.

## <a name="next-steps"></a>Próximas etapas

Explore o exemplo de código para este início rápido e outros, incluindo identificação de transliteração e idioma, bem como outros projetos de Tradução de Texto de exemplo no GitHub.

> [!div class="nextstepaction"]
> [Explore exemplos de C# no GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Consulte também

* [Transliteração de texto](quickstart-csharp-transliterate.md)
* [Identificar a linguagem pela entrada](quickstart-csharp-detect.md)
* [Obter traduções alternativas](quickstart-csharp-dictionary.md)
* [Obter uma lista de idiomas com suporte](quickstart-csharp-languages.md)
* [Determinar o comprimento da frase em uma entrada](quickstart-csharp-sentences.md)
