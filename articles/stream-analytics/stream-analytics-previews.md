---
title: Recursos de visualização do Azure Stream Analytics
description: Este artigo lista os recursos do Azure Stream Analytics que estão atualmente na visualização.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: e63937cedf44b1642e091a4744d898a26422be10
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393684"
---
# <a name="azure-stream-analytics-preview-features"></a>Recursos de visualização do Azure Stream Analytics

Este artigo resume todos os recursos atualmente em visualização do Azure Stream Analytics. O uso de recursos de visualização em um ambiente de produção não é recomendado.

## <a name="public-previews"></a>Visualizações públicas

Os seguintes recursos estão em pré-visualização pública. Você pode aproveitar esses recursos hoje, mas não os use em seu ambiente de produção.

### <a name="visual-studio-code-for-azure-stream-analytics-released-may-2019"></a>Visual Studio Code para análise do Azure Stream (lançado em maio de 2019)

Os trabalhos do Azure Stream Analytics podem ser criados no Visual Studio Code. Consulte nosso [VS Code tutorial de Introdução](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).

### <a name="anomaly-detection"></a>Detecção de anomalias

O Azure Stream Analytics apresenta novos modelos de aprendizado de máquina com suporte para a detecção de *spike* e *dips*, além da detecção de tendências negativas bidirecionais, lentas positivas e lentas. Para obter mais informações, visite [detecção de anomalias no Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="integration-with-azure-machine-learning"></a>Integração com o Azure Machine Learning

Você pode dimensionar os trabalhos do Stream Analytics com as funções de Aprendizado de Máquina (ML). Para saber mais sobre como usar as funções do ML em seu trabalho do Stream Analytics, visite [Dimensionar seu trabalho do Stream Analytics com as funções do Azure Machine Learning](stream-analytics-scale-with-machine-learning-functions.md). Confira um cenário do mundo real com [Realizando análise de sentimentos usando o Azure Stream Analytics e o Aprendizado de Máquina do Azure](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>Agregado definido pelo usuário de JavaScript

O Azure Stream Analytics oferece suporte a agregados definidos pelo usuário (UDA) escritos em JavaScript, que permitem implementar lógica de negócios com informações de estado complexas. Aprenda a usar UDAs da documentação [agregados definidos pelo usuário do JavaScript do Azure Stream Analytics](stream-analytics-javascript-user-defined-aggregates.md). 

### <a name="live-data-testing-in-visual-studio"></a>Em tempo real de dados de teste no Visual Studio

As ferramentas do Visual Studio para o Azure Stream Analytics aprimoram o recurso de teste local que permite testar suas consultas em fluxos de eventos ao vivo de fontes de nuvem, como Hub de Eventos ou Hub de IoT. Saiba como [Testar dados ao vivo localmente usando as ferramentas do Azure Stream Analytics para o Visual Studio](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>Funções definidas pelo usuário .NET no IoT Edge

Com as funções definidas pelo usuário padrão do .NET, você pode executar o código do .NET Standard como parte de seu fluxo de pipeline. Você pode criar classes C# simples ou importar projetos e bibliotecas completos. Experiência completa de autoria e depuração é suportada no Visual Studio. Para obter mais informações, visite [Desenvolver funções definidas pelo usuário do .NET Standard para tarefas do Edge Analytics Broadcast do Azure](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Outras visualizações

Os recursos a seguir também estão disponíveis na visualização.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge-and-cloud-announced-may-2019"></a>C#desserializador personalizado para o Azure Stream Analytics no IoT Edge e nuvem (maio de 2019 Announced)

Os desenvolvedores podem implementar desserializadores personalizados no C# para desserializar os eventos recebidos pelo Azure Stream Analytics. Exemplos de formatos que podem ser desserializados incluem Parquet, Protobuf, XML ou qualquer formato binário. Inscreva-se para essa versão prévia [aqui](https://aka.ms/asapreview1).

### <a name="parquet-output-announced-may-2019"></a>Saída de parquet (anunciada de maio de 2019)
Parquet é um formato de coluna habilitando o processamento eficiente de big data. Enviando dados em formato Parquet em um data lake, você pode tirar proveito do Azure Stream Analytics para grande escala de energia streaming ETL e executar processamento em lotes, treinar os algoritmos de aprendizado de máquina ou executar consultas interativas em seus dados históricos. Inscreva-se para essa versão prévia [aqui](https://aka.ms/asapreview1).

### <a name="one-click-integration-with-event-hubs-announced-may-2019"></a>Integração de um clique com o Hubs de eventos (maio de 2019 Announced) 
Com essa integração, você agora poderá visualizar os dados de entrada e começar a escrever uma consulta do Stream Analytics com um clique do portal do Hub de eventos. Quando sua consulta estiver pronta, você poderá experimentamos-la em apenas alguns cliques e começar a obter insights em tempo real. Isso reduzirá significativamente o tempo e o custo de desenvolver soluções de análise em tempo real. Inscreva-se para essa versão prévia [aqui](https://aka.ms/asapreview1).

### <a name="support-for-azure-stack-announced-may-2019"></a>Suporte para o Azure Stack (anunciado de maio de 2019)
Esse recurso seja habilitado no tempo de execução do Azure IoT Edge aproveita os recursos do Azure Stack personalizados, como o suporte nativo para entradas locais e gera em execução no Azure Stack (por exemplo, os Hubs de eventos, IoT Hub, o armazenamento de BLOBs). Essa nova integração permite que você crie arquiteturas híbridas que podem analisar seus dados perto de onde ele foi gerado, reduzindo a latência e maximizar a insights.
Inscreva-se para essa versão prévia [aqui](https://aka.ms/asapreview1).

