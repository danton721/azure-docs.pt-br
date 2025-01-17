---
title: Ferramentas de exploração e visualização de dados – Azure | Microsoft Docs
description: Ferramentas de exploração e visualização de dados para a Máquina Virtual de Ciência de Dados.
keywords: ferramentas de ciência de dados, máquina virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 693be80e493a0ba259d147f432dc9d6c07ba876d
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427522"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Ferramentas de exploração e visualização de dados para a Máquina Virtual de Ciência de Dados

Uma etapa importante na ciência de dados é entender os dados. Ferramentas de visualização e exploração de dados ajudam a acelerar a compreensão dos dados. Aqui estão algumas ferramentas oferecidas na DSVM que facilitam essa etapa fundamental. 

## <a name="apache-drill"></a>Análise do Apache
|    |           |
| ------------- | ------------- |
| O que é?   | Mecanismo de consulta SQL de software livre em Big Data    |
| Versões do DSVM com suporte      | Windows, Linux  |
| Como é configurado/instalado no DSVM?      |  Instalado em `/dsvm/tools/drill*` somente no modo inserido   |
| Usos típicos      |  Exploração de Dados in-situ sem necessidade de ETL. Consultar diferentes formatos e fontes de dados, incluindo CSV, JSON, tabelas relacionais, Hadoop     |
| Como usar/executar?      | Atalho da Área de Trabalho  <br/> [Introdução a Análise em 10 minutos](https://drill.apache.org/docs/drill-in-10-minutes/)  |
| Ferramentas relacionadas ao DSVM      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| O que é?   |  Weka é uma coleção de algoritmos de aprendizado de máquina para tarefas de mineração de dados. Os algoritmos podem ser aplicados diretamente a um conjunto de dados ou chamados do seu próprio código Java. Weka contém ferramentas para o pré-processamento, classificação, regressão, clustering, regras de associação e visualização de dados. |
| Edições do DSVM com suporte     | Windows, Linux     |
| Usos típicos      | Ferramenta de ML geral     |
| Como usar/executar?      | No Windows, pesquise Weka no Menu Iniciar. No Linux, entrar com X2Go e, em seguida, navegue até aplicativos -> desenvolvimento -> Weka. |
| Links para exemplos      | [Exemplos de Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Ferramentas relacionadas ao DSVM      |LightGBM, Rattle, Xgboost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| O que é?   |   Uma Interface Gráfica do Usuário para Mineração de Dados usando R   |
| Edições do DSVM com suporte     | Windows, Linux     |
| Usos típicos      | Ferramenta de Mineração de Dados da Interface do Usuário Geral para R    |
| Como usar/executar?      | Ferramenta da interface do usuário. No Windows, inicie um Prompt de Comando, execute R e, em seguida, dentro de R, execute `rattle()`. No Linux, conecte-se ao X2Go, inicie um terminal, execute R e, em seguida, dentro de R, execute `rattle()`. |
| Links para exemplos      | [Rattle](https://togaware.com/onepager/) |
| Ferramentas relacionadas ao DSVM      |LightGBM, Weka, Xgboost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| O que é?   | Visualização de Dados Interativa e Ferramenta de BI    |
| Versões do DSVM com suporte      | Windows  |
| Usos típicos      |  Visualização de Dados e criação de Painéis   |
| Como usar/executar?      | Atalho da Área de Trabalho (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Ferramentas relacionadas ao DSVM      |   2019 do Visual Studio, Visual Studio Code, Juno      |

