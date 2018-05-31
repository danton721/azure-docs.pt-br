---
title: Arquivo de inclusão
description: Arquivo de inclusão
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 404e238e51b7ac8b799f413965560a8d42ccc5df
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371109"
---
Até agora, você executou o código do aplicativo como se fosse o único desenvolvedor trabalhando no aplicativo. Nesta seção, você aprenderá como o Azure Dev Spaces simplifica o desenvolvimento em equipe:
* Permita que uma equipe de desenvolvedores trabalhe no mesmo ambiente de desenvolvimento.
* Dá suporte a cada desenvolvedor iterando em seu código em isolamento e sem o receio de interromper os outros.
* Teste o código de ponta a ponta, antes da confirmação, sem a necessidade de criar simulações ou simular dependências.

## <a name="challenges-with-developing-microservices"></a>Desafios com o desenvolvimento de microsserviços
O aplicativo de exemplo não é muito complexo no momento. Porém, no desenvolvimento do mundo real, os desafios logo surgem à medida que você adiciona mais serviços e a equipe de desenvolvimento cresce.

Imagine-se trabalhando em um serviço que interage com dezenas de outros serviços.

- A execução de tudo localmente para o desenvolvimento pode se tornar irrealista. Seu computador de desenvolvimento pode não ter recursos suficientes para executar o aplicativo inteiro. Ou talvez o aplicativo tenha pontos de extremidade que precisam ser acessíveis publicamente (por exemplo, o aplicativo responde a um webhook por meio de um aplicativo SaaS).

- Você pode tentar executar somente os serviços dos quais depende, mas isso significa que você precisará saber o fechamento completo de dependências (por exemplo, as dependências de dependências). Ou é uma questão de não saber com facilidade como criar e executar as dependências porque você não trabalhou nelas.
- Alguns desenvolvedores recorrem à simulação de muitas de suas dependências de serviço. Essa abordagem pode ser útil às vezes, mas o gerenciamento dessas simulações logo pode competir com seu próprio esforço de desenvolvimento. Além disso, essa abordagem leva a grandes diferenças entre o ambiente de desenvolvimento e o de produção, e bugs sutis podem afetá-lo.
- Entende-se que fazer qualquer tipo de teste de ponta a ponta se torna difícil. Os testes de integração só podem ocorrer de maneira realista após a confirmação, o que significa que você verá problemas mais tarde no ciclo de desenvolvimento.

![](../media/common/microservices-challenges.png)


## <a name="work-in-a-shared-development-environment"></a>Trabalhar em um ambiente de desenvolvimento compartilhado
Com o Azure Dev Spaces, você pode configurar um ambiente de desenvolvimento *compartilhado* no Azure. Cada desenvolvedor pode se concentrar exatamente em sua parte do aplicativo e pode desenvolver iterativamente um *código de pré-confirmação* em um ambiente que já contém todos os outros serviços e recursos de nuvem dos quais seus cenários dependem. As dependências estão sempre atualizadas e os desenvolvedores trabalham de uma forma que reflete a produção.

## <a name="work-in-your-own-space"></a>Trabalhar em seu próprio espaço
À medida que você desenvolve o código para seu serviço e antes de estar pronto para fazer check-in dele, em geral, o código não estará em um bom estado. Você ainda o está moldando iterativamente, testando e experimentando soluções. O Azure Dev Spaces apresenta o conceito de um **espaço**, permitindo que você trabalhe em isolamento e sem o receio de interromper os membros de sua equipe.

> [!Note]
> Antes de continuar, feche todas as janelas do VS Code de ambos os serviços e, em seguida, execute `azds up -d` em cada uma das pastas raiz do serviço. (Essa é uma limitação da Versão Prévia.)

Vamos examinar mais detalhadamente em que local os serviços estão em execução no momento. Execute o comando `azds list` e você verá uma saída semelhante à seguinte:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     http://webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

A coluna Espaço mostra que ambos os serviços estão em execução em um espaço chamado `default`. Qualquer pessoa que abre a URL pública e navega para o aplicativo Web invocará o caminho do código escrito anteriormente que percorre ambos os serviços. Agora suponha que você deseje continuar desenvolvendo `mywebapi`. Como fazer alterações de código e testá-las e não interromper os outros desenvolvedores que estão usando o ambiente de desenvolvimento? Para fazer isso, você configurará seu próprio espaço.

## <a name="create-a-space"></a>Criar um espaço
Para executar sua própria versão de `mywebapi` em um espaço que não seja `default`, crie seu próprio espaço usando o seguinte comando:

``` 
azds space create --name scott
```

No exemplo acima, usei meu nome para o novo espaço, de modo que meus colegas o identifiquem como o espaço em que estou trabalhando, mas você pode chamá-lo como desejar e ser flexível sobre o que ele significa, como 'sprint4' ou 'demo'.

Execute o comando `azds space list` para ver uma lista de todos os espaços no ambiente de desenvolvimento. Um asterisco (*) é exibido ao lado do espaço atualmente selecionado. Em seu caso, o espaço chamado 'scott' foi selecionado automaticamente quando foi criado. Você pode selecionar outro espaço a qualquer momento com o comando `azds space select`.