---
title: Conversão de dados
titleSuffix: Language Understanding - Azure Cognitive Services
description: Saiba como as expressões podem ser alteradas antes das previsões no Reconhecimento vocal (LUIS)
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: a148c849d0935978f049e01dd254c4c18800ee3b
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496996"
---
# <a name="convert-data-format-of-utterances"></a>Converter o formato de dados de declarações
O LUIS usa o serviço de Fala dos Serviços Cognitivos para converter enunciados falados em enunciados de texto antes da previsão. 

## <a name="speech-to-intent-conversion-concepts"></a>Conceitos de conversão de fala em intenção
Conversão de fala em texto no LUIS permite que você envie expressões faladas a um ponto de extremidade e receba uma resposta de previsão LUIS. O processo é uma integração entre o serviço de [Fala](https://docs.microsoft.com/azure/cognitive-services/Speech) com LUIS. Saiba mais sobre a conversão de fala em intenção com um [tutorial](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Requisitos de chave
Você não precisa criar uma chave de **API de Fala do Bing** para esta integração. Uma chave de **Reconhecimento vocal** criada no portal do Azure funciona para este exercício. Não use a chave de inicialização do LUIS.

### <a name="pricing-tier"></a>Camada de preços
Essa integração usa um modelo de [preço](luis-boundaries.md#key-limits) diferente dos tipos de preço comuns do Reconhecimento vocal. 

### <a name="quota-usage"></a>Uso da cota
Consulte [limites de chave](luis-boundaries.md#key-limits) para obter informações. 

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Extração de dados](luis-concept-data-extraction.md)

