---
title: Melhores práticas para escolher uma ID do Time Series na Versão Prévia do Azure Time Series Insights | Microsoft Docs
description: Noções básicas sobre as práticas recomendadas quando você escolhe uma ID do Time Series na Versão prévia do Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/08/2019
ms.custom: seodec18
ms.openlocfilehash: af540267e4afc1b248b66b1c6f4989b832c38b58
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237572"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Práticas recomendadas para escolher uma ID do Time Series

Este artigo aborda a chave de partição da Versão prévia do Azure Time Series Insights, a ID do Time Series e as práticas recomendadas para escolher uma.

## <a name="choose-a-time-series-id"></a>Escolha uma ID do Time Series

Escolher uma ID do Time Series é como escolher uma chave de partição para um banco de dados. É uma decisão importante que deve ser tomada em tempo de design. Não é possível atualizar um ambiente existente da Versão prévia do Time Series Insights para usar uma ID do Time Series diferente. Em outras palavras, quando um ambiente é criado com uma ID do Time Series, a política é uma propriedade imutável que não pode ser alterado.

> [!IMPORTANT]
> A ID do Time Series é imutável e diferencia maiúsculas de minúsculas (não pode ser alterada depois de definida).

Tendo isso em mente, selecionar a ID do Time Series adequada é essencial. Quando você seleciona uma ID do Time Series, considere estas práticas recomendadas a seguir:

* Escolha um nome de propriedade que tenha uma ampla gama de valores e tenha padrões de acesso uniformes. É uma prática recomendada ter uma chave de partição com muitos valores distintos (por exemplo, centenas ou milhares). Para muitos clientes, isso será algo como DeviceID ou SensorID no JSON.
* A ID do Time Series deve ser exclusiva no nível do nó folha do seu [Modelo do Time Series](./time-series-insights-update-tsm.md).
* Uma cadeia de caracteres de nome de propriedade de ID do Time Series pode ter até 128 caracteres e valores de propriedade de ID do Time Series que podem ter até 1024 caracteres.
* Se estiverem faltando alguns valores de propriedade de ID do Time Series exclusivo, eles são tratados como valores nulos, que fazem parte da restrição de exclusividade.

Além disso, você pode selecionar até *três* (3) propriedades de chave como sua ID do Time Series.

  > [!NOTE]
  > Suas *três* (3) propriedades de chave devem ser cadeias de caracteres.

Os cenários a seguir descrevem como selecionar mais de uma propriedade de chave como sua ID do Time Series:  

### <a name="scenario-one"></a>Cenário um

* Você tem frotas herdadas de ativos, cada uma com uma chave exclusiva.
* Por exemplo, uma frota é identificada exclusivamente pela propriedade *deviceId* e outra em que a propriedade exclusiva é *objectId*. Nenhuma das frotas contém a propriedade exclusiva da outra frota. Neste exemplo, você seleciona duas chaves, deviceId e objectId, como chaves exclusivas.
* Podemos aceitar valores nulos e a ausência de uma propriedade na carga do evento é contado como um valor `null`. Essa também é a maneira apropriada para lidar com o envio de dados para duas fontes de eventos diferentes em que os dados em cada fonte de eventos têm ID do Time Series exclusiva.

### <a name="scenario-two"></a>Cenário dois

* Você precisa que várias propriedades sejam exclusivas dentro do mesma frota de ativos. 
* Por exemplo, digamos que você é um construtor civil inteligente e implanta sensores em cada sala. Em cada sala, normalmente você têm os mesmos valores para *sensorId*, como *sensor1*, *sensor2* e *sensor3*.
* Além disso, o prédio tem números de andar e salas iguais nos locais da propriedade *flrRm*, que têm valores como *1a*, *2b*, *3a* , e assim por diante.
* Finalmente, você tem uma propriedade, *location*, que contém valores como *Redmond*, *Barcelona* e *Tóquio*. Para criar a exclusividade, você criaria as três propriedades a seguir como suas chaves de ID do Time Series: *sensorId*, *flrRm* e *location*.

## <a name="next-steps"></a>Próximas etapas

* Leia mais sobre [Modelagem de dados](./time-series-insights-update-tsm.md).

* Planeje o [ambiente do Azure Time Series Insights (Versão prévia)](./time-series-insights-update-plan.md).