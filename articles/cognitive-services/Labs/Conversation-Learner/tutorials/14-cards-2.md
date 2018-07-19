---
title: Como usar cartões com um aplicativo de Aprendizado de Conversa, parte 2 - Serviços Cognitivos da Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como usar cartões com um aplicativo de Aprendizado de Conversa.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 254f0953fd3e281a35857e69d9795e3decebf45d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35364212"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Como usar cartões (parte 1 de 2)
Este tutorial mostra como adicionar um cartão de formulário preenchível ao seu bot. Ele mostrará como os campos de formulário se movem para as entidades.

O Aprendizado de Conversa espera que os arquivos de definição de cartão estejam localizados em um diretório chamado “cartões” que está presente no diretório onde o bot é iniciado.

## <a name="requirements"></a>Requisitos
Este tutorial exige que o bot de tutorial geral esteja em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Os cartões são elementos de interface do usuário que permitem ao usuário selecionar uma opção na conversa. 

### <a name="open-the-demo"></a>Abrir a demonstração

Na lista de aplicativos da interface do usuário da Web, clique no Tutorial-14-Cartões-2. 

### <a name="the-card"></a>O cartão

A definição de cartão está no seguinte local: c:\<caminhoinstalado\>\src\cards\shippingAddress.json.

Esse cartão coleta três campos de endereço para entrega: cidade, rua e estado.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Ações

Nós criamos três ações. Como você pode ver abaixo, a primeira ação é um cartão.

![](../media/tutorial14_actions.PNG)

Vamos ver como o tipo de ação do cartão foi criado:

- Observe o Endereço-Rua, onde o tipo é Input.text e sua ID.
- Da mesma forma, há Endereço-Cidade e uma lista suspensa com a ID de Endereço-Estado.

As Ids são importantes, como quando os campos são preenchidos e enviados, elas serão os nomes de entidade que receberão esses valores no bot.

## <a name="entities"></a>Entidades
Definimos três entidades correspondentes ao cartão, conforme vimos acima.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Ações

Definimos duas ações.

![](../media/tutorial14_actions.PNG)

- O primeiro é o cartão de endereço de envio em que o Tipo de Ação é CARTÃO e Modelo é selecionado na lista suspensa como endereçoDeEntrega.
- O segundo é uma ação simples para ler o endereço de envio.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Diálogo de Treinamento

Vamos examinar uma caixa de diálogo de ensino.

1. Clique em Diálogos de Treinamento e, em seguida, em Novo Diálogo de Treinamento.
1. Digite ‘oi’.
2. Clique em Ação de Pontuação.
3. Clique para Selecionar ‘Endereço de Entrega’.
4. Preencha o cartão e envie.
    - Observe que esses valores agora foram movidos para a memória de entidade. Nenhuma análise é necessária, pois o formulário já particionou as entradas.
5. Clique em Ações de Pontuação.
3. Clique para Selecionar 'Enviando para $Address...".
4. Clique em Teste Concluído.

![](../media/tutorial14_train_dialog.PNG)

Agora você viu como obter valores do cartão com campos preenchíveis e menus suspensos, como capturá-los e reuni-los em entidades de bot.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Ramificação e desfazer](./15-branching-and-undo.md)