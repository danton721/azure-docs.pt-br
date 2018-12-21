---
title: Operadores úteis em consultas do Azure Log Analytics | Microsoft Docs
description: Funções comuns a serem usadas para diferentes cenários em consultas do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 060b1e469a31c335f062ccd332157d13e64f9318
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183975"
---
# <a name="useful-operators-in-log-analytics-queries"></a>Operadores úteis em consultas do Log Analytics

A tabela abaixo fornece algumas funções comuns a serem usadas para diferentes cenários em consultas do Log Analytics.

## <a name="useful-operators"></a>Operadores úteis

Categoria                                |Função de Análise Relevante
----------------------------------------|----------------------------------------
Aliases de Seleção e Coluna            |`project`, `project-away`, `extend`
Constantes e tabelas temporárias          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Operadores de Comparação e Cadeia de Caracteres         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Funções de cadeia de caracteres comuns                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Funções matemáticas comuns                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Análise de texto                            |`extract()`, `extractjson()`, `parse`, `split()`
Limitação de saída                         |`take`, `limit`, `top`, `sample`
Funções de data                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Agrupamento e agregação                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Junções e Uniões                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Classificação, ordem                             |`sort`, `order` 
Objeto dinâmico (JSON e matriz)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Operadores lógicos                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Aprendizado de máquina                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Próximas etapas

- Percorra uma lição em [escrevendo consultas no Log Analytics](get-started-queries.md).