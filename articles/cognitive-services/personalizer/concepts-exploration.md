---
title: Exploração – Personalizador
titleSuffix: Azure Cognitive Services
description: Com a exploração, o Personalizador é capaz de continuar fornecendo bons resultados, mesmo que haja alterações no comportamento do usuário. Escolher uma configuração de exploração é uma decisão de negócios sobre a proporção de interações do usuário com a qual explorar, a fim de melhorar o modelo.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/13/2019
ms.author: edjez
ms.openlocfilehash: 0e11dd962f4cc1ff9b1938b309a1b405dbf20232
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606989"
---
# <a name="exploration-and-exploitation"></a>Exploration e exploitation

Com a exploração, o Personalizador é capaz de continuar fornecendo bons resultados, mesmo que haja alterações no comportamento do usuário.

Quando o Personalizador recebe uma chamada de classificação, ele retorna um RewardActionID que:
* Usa a exploitation para fazer a correspondência com o comportamento mais provável do usuário com base no modelo de machine learning atual.
* Usa a exploration, que não faz a correspondência com a ação que tem a probabilidade mais alta na classificação.

<!--
Returning the most probable action is called *exploit* behavior. Returning a different action is called *exploration*.
-->
Atualmente, o Personalizador usa um algoritmo chamado *epsilon greedy* para explorar. 

## <a name="choosing-an-exploration-setting"></a>Como escolher a configuração de exploração

Configure a porcentagem de tráfego a ser usada para exploração na página **Configurações** do portal do Azure para o Personalizador. Essa configuração determina a porcentagem de chamadas de classificação que realizam exploração. 

O Personalizador determina se fará a exploration ou exploitation com essa probabilidade em cada chamada de classificação. Isso é diferente do comportamento de algumas estruturas A/B que bloqueiam um tratamento em IDs de usuário específicas.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Melhores práticas para escolher a configuração de exploração

<!--
@edjez - you say what not to do, but make no recommendations of what **to** do. 
-->

Escolher uma configuração de exploração é uma decisão de negócios sobre a proporção de interações do usuário com a qual explorar, a fim de melhorar o modelo. 

Uma configuração zero anula muitos dos benefícios do Personalizador. Com essa configuração, o Personalizador não usa nenhuma interação do usuário para descobrir as melhores interações. Isso leva à estagnação do modelo, descompasso e, por fim, menor desempenho.

Uma configuração muito alta anula os benefícios de aprendizado do comportamento do usuário. Defini-la como 100% implica uma aleatoriedade constante, e qualquer comportamento aprendido com os usuários não influenciaria o resultado.

É importante não alterar o comportamento do aplicativo com base no fato de o Personalizador estar fazendo exploration ou exploitation. Isso levaria a desvios de aprendizado que acabariam diminuindo o desempenho potencial.

## <a name="next-steps"></a>Próximas etapas

[Aprendizado de reforço](concepts-reinforcement-learning.md) 