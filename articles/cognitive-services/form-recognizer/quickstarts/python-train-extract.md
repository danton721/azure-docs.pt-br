---
title: 'Início Rápido: treinar em um modelo e extrair dados de formulário usando a API REST com o Python – Reconhecimento de Formulários'
titleSuffix: Azure Cognitive Services
description: Neste Início Rápido, você usará a API REST do Reconhecimento de Formulários com o Python para treinar em um modelo e extrair dados de formulários.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 04/24/2019
ms.author: pafarley
ms.openlocfilehash: 139c0c29033dc45d07fd0987c2eee92308512329
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65906974"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-python"></a>Início Rápido: treinar em um modelo do Reconhecimento de Formulários e extrair dados de formulário usando a API REST com o Python

Neste Início Rápido, você usará a API REST do Reconhecimento de Formulários do Azure com o Python para treinar e pontuar formulários a fim de extrair pares chave-valor e tabelas.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este início rápido, é necessário ter:
- Acesso à versão prévia de acesso limitado do Reconhecimento de Formulários. Para obter acesso à versão prévia, preencha e envie o formulário de [Solicitação de acesso ao Reconhecimento de Formulários](https://aka.ms/FormRecognizerRequestAccess).
- [Python](https://www.python.org/downloads/) instalado (se quiser executar o exemplo localmente).
- Um conjunto com pelo menos cinco formulários do mesmo tipo. Você pode usar um [conjunto de dados de exemplo](https://go.microsoft.com/fwlink/?linkid=2090451) para este Início Rápido.

## <a name="create-a-form-recognizer-resource"></a>Criar um recurso do Reconhecimento de Formulários

Quando o acesso para usar o Reconhecimento de Formulários for concedido, você receberá um email de boas-vindas com vários links e recursos. Use o link do "portal do Azure" na mensagem para abrir o portal do Azure e criar um recurso do Reconhecimento de Formulários. No painel **Criar**, forneça as informações a seguir:

|    |    |
|--|--|
| **Nome** | Um nome descritivo para o seu recurso. É recomendável usar um nome descritivo, como, por exemplo, *MyNameFormRecognizer*. |
| **Assinatura** | Selecione a assinatura do Azure à qual o acesso foi concedido. |
| **Localidade** | A localização da sua instância de serviço cognitivo. Locais diferentes podem introduzir latência, mas não têm impacto sobre a disponibilidade de tempo de execução do seu recurso. |
| **Tipo de preços** | O custo do recurso depende de seu uso e do tipo de preço que você escolheu. Para obter mais informações, consulte a API [detalhes de preços](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupo de recursos** | O [grupo de recursos do Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que conterá o seu recurso. Você pode criar um grupo ou adicioná-lo a um grupo pré-existente. |

> [!IMPORTANT]
> Normalmente, ao criar um recurso de Serviço Cognitivo no portal do Azure, você tem a opção de criar uma chave de assinatura para vários serviços (usada em vários serviços cognitivos) ou uma chave de assinatura de serviço único (usada somente com um serviço cognitivo específico). No entanto, como o Reconhecimento de Formulários é uma versão prévia, ele não está incluído na assinatura vários serviços e você não pode criar a assinatura de serviço único, a menos que você use o link fornecido no email de boas-vindas.

Quando o recurso de Reconhecimento de Formulários concluir a implantação, localize-o e selecione-o na lista **Todos os recursos** do portal. Em seguida, selecione a guia **Chaves** para exibir suas chaves de assinatura. A chave fornecerá ao aplicativo o acesso ao recurso. Copie o valor de **CHAVE 1**. Você o usará na próxima seção.

## <a name="create-and-run-the-sample"></a>Criar e executar o exemplo

Para criar e executar a amostra, faça as seguintes alterações no snippet de código a seguir:
1. Substitua `<Endpoint>` pela URL do ponto de extremidade do recurso do Reconhecimento de Formulários na região do Azure em que você obteve suas chaves de assinatura.
1. Substitua `<SAS URL>` pela URL de SAS (Assinatura de Acesso Compartilhado) de um contêiner do Armazenamento de Blob do Azure no qual os dados de treinamento estão localizados.  
1. Substitua `<Subscription key>` pela chave de assinatura que você copiou na etapa anterior.
    ```python
    ########### Python Form Recognizer Train #############
    from requests import post as http_post

    # Endpoint URL
    base_url = r"<Endpoint>" + "/formrecognizer/v1.0-preview/custom"
    source = r"<SAS URL>"
    headers = {
        # Request headers
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': '<Subscription Key>',
    }
    url = base_url + "/train" 
    body = {"source": source}
    try:
        resp = http_post(url = url, json = body, headers = headers)
        print("Response status code: %d" % resp.status_code)
        print("Response body: %s" % resp.json())
    except Exception as e:
        print(str(e))
    ```
1. Salve o código como um arquivo com uma extensão .py. Por exemplo, *form-recognize-train.py*.
1. Abra una janela de prompt de comando.
1. No prompt, use o comando `python` para executar o exemplo. Por exemplo, `python form-recognize-train.py`.

Você receberá a resposta `200 (Success)` com a seguinte saída JSON:

```json
{
  "modelId": "59e2185e-ab80-4640-aebc-f3653442617b",
  "trainingDocuments": [
    {
      "documentName": "Invoice_1.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_2.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_3.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_4.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_5.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    }
  ],
  "errors": []
}
```

Observe o valor de `"modelId"`. Ele será necessário nas etapas a seguir.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Extrair pares chave-valor e tabelas de formulários

Em seguida, você analisará um documento e extrairá pares chave-valor e tabelas dele. Chame a API **Modelar – Analisar** executando o comando do Python a seguir. Antes de executar o comando, faça essas alterações:

1. Substitua `<Endpoint>` pelo ponto de extremidade que você obteve com a chave de assinatura do Reconhecimento de Formulários. Encontre-o na guia **Visão geral** de recursos do Reconhecimento de Formulários.
1. Substitua `<File Path>` pelo caminho do arquivo ou a URL do local do formulário cujos dados serão extraídos.
1. Substitua `<modelID>` pela ID do modelo recebida na seção anterior.
1. Substitua `<file type>` pelo tipo do arquivo. Tipos com suporte: pdf, imagem/jpeg, imagem/png.
1. Substitua `<subscription key>` por sua chave de assinatura.

    ```python
    ########### Python Form Recognizer Analyze #############
    from requests import post as http_post
    
    # Endpoint URL
    base_url = r"<Endpoint>" + "/formrecognizer/v1.0-preview/custom"
    file_path = r"<File Path>"
    model_id = "<modelID>"
    headers = {
        # Request headers
        'Content-Type': 'application/<file type>',
        'Ocp-Apim-Subscription-Key': '<subscription key>',
    }

    try:
        url = base_url + "/models/" + model_id + "/analyze" 
        with open(file_path, "rb") as f:
            data_bytes = f.read()  
        resp = http_post(url = url, data = data_bytes, headers = headers)
        print("Response status code: %d" % resp.status_code)    
        print("Response body:\n%s" % resp.json())   
    except Exception as e:
        print(str(e))
    ```

1. Salve o código como um arquivo com uma extensão .py. Por exemplo, *form-recognize-analyze.py*.
1. Abra una janela de prompt de comando.
1. No prompt, use o comando `python` para executar o exemplo. Por exemplo, `python form-recognize-analyze.py`.

### <a name="examine-the-response"></a>Examinar a resposta

Uma resposta com êxito é retornada em JSON. Ela representa os pares chave-valor e as tabelas extraídos do formulário:

```bash
{
  "status": "success",
  "pages": [
    {
      "number": 1,
      "height": 792,
      "width": 612,
      "clusterId": 0,
      "keyValuePairs": [
        {
          "key": [
            {
              "text": "Address:",
              "boundingBox": [
                57.4,
                683.1,
                100.5,
                683.1,
                100.5,
                673.7,
                57.4,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "1 Redmond way Suite",
              "boundingBox": [
                57.4,
                671.3,
                154.8,
                671.3,
                154.8,
                659.2,
                57.4,
                659.2
              ],
              "confidence": 0.86
            },
            {
              "text": "6000 Redmond, WA",
              "boundingBox": [
                57.4,
                657.1,
                146.9,
                657.1,
                146.9,
                645.5,
                57.4,
                645.5
              ],
              "confidence": 0.86
            },
            {
              "text": "99243",
              "boundingBox": [
                57.4,
                643.4,
                85,
                643.4,
                85,
                632.3,
                57.4,
                632.3
              ],
              "confidence": 0.86
            }
          ]
        },
        {
          "key": [
            {
              "text": "Invoice For:",
              "boundingBox": [
                316.1,
                683.1,
                368.2,
                683.1,
                368.2,
                673.7,
                316.1,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "Microsoft",
              "boundingBox": [
                374,
                687.9,
                418.8,
                687.9,
                418.8,
                673.7,
                374,
                673.7
              ],
              "confidence": 1
            },
            {
              "text": "1020 Enterprise Way",
              "boundingBox": [
                373.9,
                673.5,
                471.3,
                673.5,
                471.3,
                659.2,
                373.9,
                659.2
              ],
              "confidence": 1
            },
            {
              "text": "Sunnayvale, CA 87659",
              "boundingBox": [
                373.8,
                659,
                479.4,
                659,
                479.4,
                645.5,
                373.8,
                645.5
              ],
              "confidence": 1
            }
          ]
        }
      ],
      "tables": [
        {
          "id": "table_0",
          "columns": [
            {
              "header": [
                {
                  "text": "Invoice Number",
                  "boundingBox": [
                    38.5,
                    585.2,
                    113.4,
                    585.2,
                    113.4,
                    575.8,
                    38.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "34278587",
                    "boundingBox": [
                      38.5,
                      547.3,
                      82.8,
                      547.3,
                      82.8,
                      537,
                      38.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Date",
                  "boundingBox": [
                    139.7,
                    585.2,
                    198.5,
                    585.2,
                    198.5,
                    575.8,
                    139.7,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/18/2017",
                    "boundingBox": [
                      139.7,
                      546.8,
                      184,
                      546.8,
                      184,
                      537,
                      139.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Due Date",
                  "boundingBox": [
                    240.5,
                    585.2,
                    321,
                    585.2,
                    321,
                    575.8,
                    240.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/24/2017",
                    "boundingBox": [
                      240.5,
                      546.8,
                      284.8,
                      546.8,
                      284.8,
                      537,
                      240.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Charges",
                  "boundingBox": [
                    341.3,
                    585.2,
                    381.2,
                    585.2,
                    381.2,
                    575.8,
                    341.3,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "$56,651.49",
                    "boundingBox": [
                      387.6,
                      546.4,
                      437.5,
                      546.4,
                      437.5,
                      537,
                      387.6,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "VAT ID",
                  "boundingBox": [
                    442.1,
                    590,
                    474.8,
                    590,
                    474.8,
                    575.8,
                    442.1,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "PT",
                    "boundingBox": [
                      447.7,
                      550.6,
                      460.4,
                      550.6,
                      460.4,
                      537,
                      447.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            }
          ]
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="next-steps"></a>Próximas etapas

Neste guia de início rápido, você usou a API REST do Reconhecimento de Formulários com o Python para treinar em um modelo e executá-lo em um caso de exemplo. Em seguida, confira a documentação de referência para explorar a API de Reconhecimento de Formulários de forma mais aprofundada.

> [!div class="nextstepaction"]
> [Documentação de referência da API REST](https://aka.ms/form-recognizer/api)
