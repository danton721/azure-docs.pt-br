---
title: Consultas de pesquisa no Log Analytics | Microsoft Docs
description: Este artigo fornece um tutorial para começar a escrever consultas de pesquisa no Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 7c6bade68e6190ec52cbc136c057906be6d9d37c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631795"
---
# <a name="search-queries-in-log-analytics"></a>Consultas de pesquisa no Log Analytics

> [!NOTE]
> Você deve concluir [Introdução às consultas no Log Analytics](get-started-queries.md) antes de concluir este tutorial.

Consultas de Log Analytics do Azure podem começar com um nome de tabela ou um comando de pesquisa. Este tutorial abrange consultas baseadas em pesquisa. Há vantagens em cada método.

As consultas baseadas em tabela começam com o escopo da consulta e, portanto, tendem a ser mais eficientes do que as consultas de pesquisa. As consultas de pesquisa são menos estruturadas, o que as torna a melhor opção ao pesquisar um valor específico entre colunas ou tabelas. A **pesquisa** pode verificar todas as colunas em uma determinada tabela, ou em todas as tabelas, para o valor especificado. A quantidade de dados sendo processada pode ser enorme, e é por isso que essas consultas demoram mais para serem concluídas e podem retornar conjuntos de resultados muito grandes.

## <a name="search-a-term"></a>Um termo de pesquisa
O **pesquisa** comando normalmente é usado para pesquisar um termo específico. No exemplo a seguir, todas as colunas em todas as tabelas são verificadas para o termo "erro":

```OQL
search "error"
| take 100
```

Enquanto eles são fáceis de usar, consultas sem escopo como aquele mostrado acima não são eficientes e têm probabilidade de retornar vários resultados irrelevantes. Uma prática melhor seria pesquisar na tabela relevante ou até mesmo em uma coluna específica.

### <a name="table-scoping"></a>Tabela de escopo
Para pesquisar um termo em uma tabela específica, adicione `in (table-name)` logo após o operador **search**:

```OQL
search in (Event) "error"
| take 100
```

ou em várias tabelas:
```OQL
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tabela e coluna de escopo
Por padrão, a **pesquisa** avaliará todas as colunas no conjunto de dados. Para pesquisar somente uma coluna específica, use a seguinte sintaxe:

```OQL
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Se você usar `==` em vez de `:`, os resultados seriam incluir registros em que o *origem* coluna tem o valor exato "error" e nesse caso exato. O uso de ':' não incluirá registros em que *Fonte* tenha valores como "código de erro 404" ou "Erro".

## <a name="case-sensitivity"></a>Diferenciar maiusculas de minúsculas
Por padrão, pesquisa de termo diferencia maiusculas de minúsculas, portanto, pesquisar "dns" pode produzir resultados como "DNS", "dns" ou "Dns". Para tornar a pesquisa diferencia maiusculas de minúsculas, use o `kind` opção:

```OQL
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Usar curingas
O comando de **pesquisa** suporta curingas, no início, final ou meio de um termo.

Para pesquisar termos que começam com "win":
```OQL
search in (Event) "win*"
| take 100
```

Para pesquisar termos que terminem com ".com":
```OQL
search in (Event) "*.com"
| take 100
```

Para pesquisar termos que contenham "www":
```OQL
search in (Event) "*www*"
| take 100
```

Para pesquisar termos que começam com "corp" e terminam em ".com", como "corp.mydomain.com" "

```OQL
search in (Event) "corp*.com"
| take 100
```

Você também pode obter tudo em uma tabela usando apenas um curinga: `search in (Event) *`, mas isso seria o mesmo que escrever apenas `Event`.

> [!TIP]
> Embora você possa usar `search *` para obter todas as colunas de todas as tabelas, é recomendável que você sempre definir o escopo de suas consultas em tabelas específicas. Fora do escopo de consultas pode demorar um pouco para ser concluído e pode retornar um número excessivo de resultados.

## <a name="add-and--or-to-search-queries"></a>Adicione *e* / *ou* para pesquisar consultas
Use **e** para procurar registros que contêm vários termos:

```OQL
search in (Event) "error" and "register"
| take 100
```

Use **ou** para obter registros que contenham pelo menos um dos termos:

```OQL
search in (Event) "error" or "register"
| take 100
```

Se você tiver várias condições de pesquisa, você pode combiná-los na mesma consulta usando parênteses:

```OQL
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Os resultados deste exemplo seriam registros que contêm o termo "erro" e também contêm "Registrar" ou algo que começa com "empacotar".

## <a name="pipe-search-queries"></a>Redirecionar consultas de pesquisa
Assim como qualquer outro comando, a **pesquisa** pode ser canalizada para que os resultados da pesquisa possam ser filtrados, classificados e agregados. Por exemplo, para obter o número de *evento* registros que contêm "win":

```OQL
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Próximas etapas

- Ver mais tutoriais sobre o [site de linguagem de consulta do Log Analytics](http://http://docs.loganalytics.io)