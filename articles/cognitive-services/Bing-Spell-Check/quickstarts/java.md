---
title: 'Início Rápido: API de Verificação Ortográfica do Bing, Java'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para ajudá-lo a começar a usar rapidamente a API de Verificação Ortográfica do Bing.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: v-jaswel
ms.openlocfilehash: 00e0b7db5bfc8b763d9b16524bd783601d1ec4d8
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48800988"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Início Rápido para a API de Verificação Ortográfica do Bing com Java 

Este artigo mostra como usar a [API de Verificação Ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/) com Java. A API de Verificação Ortográfica retorna uma lista de palavras que ela não reconhece, juntamente com as substituições sugeridas. Normalmente, você enviaria texto para essa API e faria as substituições sugeridas no texto, ou então as mostraria para o usuário do seu aplicativo para que ele pudesse decidir realizar ou não as substituições. Este artigo mostra como enviar uma solicitação que contém o texto "Omá, mndo!". As substituições sugeridas serão "Olá" e "mundo".

## <a name="prerequisites"></a>Pré-requisitos

É necessário ter o [JDK 7 ou 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) para compilar e executar esse código. Você poderá usar um Java IDE se tiver um favorito, mas um editor de texto é suficiente.

É necessário ter uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de Verificação Ortográfica do Bing v7**. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/#lang) é suficiente para esse início rápido. É necessário ter a chave de acesso fornecida ao ativar a avaliação gratuita ou você poderá usar uma chave de assinatura paga no painel do Azure.

## <a name="get-spell-check-results"></a>Obter resultados da Verificação Ortográfica

1. Crie um projeto Java em seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua assinatura.
4. Execute o programa.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Resposta**

Uma resposta com êxito é retornada em JSON, conforme mostrado no seguinte exemplo: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutorial de Verificação Ortográfica do Bing](../tutorials/spellcheck.md)

## <a name="see-also"></a>Consulte também

- [Visão geral da Verificação Ortográfica do Bing](../proof-text.md)
- [Referência de API de Verificação Ortográfica do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)