---
title: Habilidade de pesquisa cognitiva do Text Merge (Azure Search) | Microsoft Docs
description: Mescle o texto de uma coleção de campos em um campo consolidado. Use essa habilidade cognitiva em um pipeline de enriquecimento do Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: e288748d7433f4b3c7da7db1ab1ef2ee487318df
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33786735"
---
#    <a name="text-merge-cognitive-skill"></a>Habilidade de percepção do Text Merge

A habilidade do **Text Merge** consolida o texto de uma coleção de campos em um único campo. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.TextMerger

## <a name="skill-parameters"></a>Parâmetros de habilidades

Os parâmetros diferenciam maiúsculas de minúsculas.

| Nome do parâmetro     | DESCRIÇÃO |
|--------------------|-------------|
| insertPreTag  | Cadeia de caracteres a serem incluídas antes de cada inserção. O valor padrão é `" "`. Para omitir o espaço, defina o valor como `""`.  |
| insertPostTag | Cadeia de caracteres a ser incluída antes de cada inserção. O valor padrão é `" "`. Para omitir o espaço, defina o valor como `""`.  |


##  <a name="sample-input"></a>Entrada de exemplo
Um documento JSON fornece entrada utilizável para esta habilidade JSON poderia ser:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Saída de exemplo
Este exemplo mostra a saída da entrada anterior, supondo que o *insertPreTag* seja definido como `" "`, e *insertPostTag* seja definido como `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Definição do conjunto de habilidades de exemplo estendido

Um cenário comum para o Text Merge é a capacidade de mesclar a representação textual de imagens (texto de uma habilidade OCR ou a legenda de uma imagem) no campo de conteúdo de um documento. 

O conjunto de qualificações do exemplo a seguir usa a habilidade de OCR para extrair o texto de imagens inseridas no documento. Em seguida, cria um campo *merged_text* que contém texto original e OCRed de cada imagem. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Util.TextMerger",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```
O exemplo acima presume que um campo de imagens normalizado existe. Para gerar esse campo, defina a configuração *imageAction* na definição do indexador para *generateNormalizedImages* conforme mostrado abaixo:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="see-also"></a>Consulte também

+ [Habilidades predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)