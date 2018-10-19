---
title: Guia do desenvolvedor de R para Azure – programação em R | Microsoft Docs
description: Este artigo apresenta uma visão geral das várias maneiras como os cientistas de dados podem aproveitar suas habilidades existentes com a linguagem de programação R no Azure. O Azure oferece muitos serviços que os desenvolvedores do R podem aproveitar para estender suas cargas de trabalho de ciência de dados para a nuvem.
services: machine-learning
documentationcenter: ''
author: AnalyticJeremy
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: R
ms.topic: article
ms.date: 09/12/2018
ms.author: jepeach
ms.openlocfilehash: bc00bd3b61398355c663d133c0c9a66c2a52aa8d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47045584"
---
# <a name="r-developers-guide-to-azure"></a>Guia do desenvolvedor de R para Azure
<img src="media/r-developers-guide/logo_r.svg" alt="R logo" align="right" width="200" />

Muitos cientistas de dados que lidam com volumes crescentes de dados estão procurando maneiras de aproveitar o poder da computação em nuvem para suas análises.  Este artigo apresenta uma visão geral das várias maneiras como os cientistas de dados podem aproveitar suas habilidades existentes com a [linguagem de programação R](https://www.r-project.org) no Azure.

A Microsoft adotou totalmente a linguagem de programação R como uma ferramenta de primeira classe para cientistas de dados.  Fornecendo várias opções diferentes para os desenvolvedores do R executarem o código no Azure, a empresa está habilitando os cientistas de dados a estender suas cargas de trabalho de ciência de dados para a nuvem ao lidar com projetos em larga escala.

Vamos examinar as várias opções e os cenários mais atraentes para cada um deles.

## <a name="azure-services-with-r-language-support"></a>Serviços do Azure com suporte à linguagem R
Este artigo aborda os seguintes serviços do Azure que dão suporte à linguagem R:

|Serviço                                                          |DESCRIÇÃO                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Máquina Virtual de Ciência de Dados](#data-science-virtual-machine)    |uma VM personalizada para usar como uma estação de trabalho de ciência de dados ou como um destino de computação personalizado|
|[ML Services no HDInsight](#ml-services-on-hdinsight)            |sistema com base em cluster para executar análises de R em grandes conjuntos de dados em vários nós   |
|[Azure Databricks](#azure-databricks)                            |ambiente colaborativo do Spark que dá suporte a R e outras linguagens               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |executar scripts R personalizados em experimentos de aprendizado de máquina do Azure                      |
|[Lote do Azure](#azure-batch)                                      |oferece uma variedade de opções para executar o código R economicamente em vários nós em um cluster|
|[Azure Notebooks](#azure-notebooks)                              |uma versão baseada em nuvem sem custos (mas limitada) de Jupyter Notebooks                  |
|[Banco de Dados SQL do Azure](#azure-sql-database)                        |executar scripts do R dentro do mecanismo de banco de dados do SQL Server                            |

## <a name="data-science-virtual-machine"></a>Máquina Virtual de Ciência de Dados
A [DSVM (Máquina Virtual de Ciência de Dados)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) é uma imagem de VM personalizada plataforma de nuvem do Microsoft Azure especificamente criada para ciência de dados. Ela tem muitas ferramentas de ciência de dados populares, incluindo:
* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

A DSVM pode ser provisionada com Windows ou Linux como o sistema operacional.  Você pode usar a DSVM de duas maneiras diferentes: como uma estação de trabalho interativa ou como uma plataforma de computação para um cluster personalizado.

### <a name="as-a-workstation"></a>Como uma estação de trabalho
Se você quer começar a usar o R na nuvem de modo rápido e fácil, essa é a melhor opção.  O ambiente será familiar a qualquer pessoa que já tenha trabalhado com R em uma estação de trabalho local.  No entanto, em vez de usar recursos locais, o ambiente do R é executado em uma VM na nuvem.  Se seus dados já estiverem armazenados no Azure, haverá o benefício adicional de permitir que os scripts do R sejam executados "mais próximos dos dados". Em vez de transferir os dados pela Internet, os dados podem ser acessados pela rede interna do Azure, que fornece tempos de acesso muito mais rápidos.

A DSVM pode ser particularmente útil para pequenas equipes de desenvolvedores do R.  Em vez de investir em estações de trabalho potentes para cada desenvolvedor e exigir que os membros da equipe sincronizem as versões dos vários pacotes de software que eles usarão, cada desenvolvedor pode criar uma instância da DSVM sempre que necessário.

### <a name="as-a-compute-platform"></a>Como uma plataforma de computação
Além de ser usado como uma estação de trabalho, a DSVM também é usada como uma plataforma de computação escalonável elasticamente para projetos em R.  Usando o pacote do R <code>[AzureDSVM](https://github.com/Azure/AzureDSVM)</code>, você pode controlar programaticamente a criação e a exclusão de instâncias da DSVM.  Você pode formar as instâncias em um cluster e implantar uma análise distribuída a ser realizada na nuvem.  Todo esse processo pode ser controlado pelo código de R em execução na sua estação de trabalho local.

Para saber mais sobre a DSVM, veja ["Introdução à Máquina Virtual de Ciência de Dados do Azure para Linux e Windows"](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview).

## <a name="ml-services-on-hdinsight"></a>ML Services no HDInsight
O [Serviços do Microsoft ML](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) fornece aos cientistas de dados, estatísticos e programadores de R acesso sob demanda a métodos escalonáveis e distribuídos de análise no HDInsight.  Essa solução fornece os recursos mais recentes para análise de R em conjuntos de dados de praticamente qualquer tamanho carregados no Blob do Azure ou no Data Lake Storage.

Essa é uma solução de nível empresarial que permite que você dimensione seu código R em um cluster.  Ao aproveitar as funções no pacote
<code>[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</code> da Microsoft, os scripts de R no HDInsight podem executar funções de processamento de dados em paralelo em vários nós em um cluster.  Isso permite ao R processar dados em uma escala muito maior do que seria possível com o R de thread único em execução em uma estação de trabalho.

Essa capacidade de dimensionar torna os Serviços de ML no HDInsight uma ótima opção para os desenvolvedores do R com grandes conjuntos de dados.  Fornece uma plataforma flexível e escalonável para executar os scripts do R na nuvem.

Para obter instruções passo a passo sobre como criar um cluster de Serviços de ML, leia o artigo ["Introdução aos serviços de ML no Azure HDInsight"](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started).

## <a name="azure-databricks"></a>Azure Databricks
O [Azure Databricks](https://azure.microsoft.com/services/databricks/) é uma plataforma de análise baseada no Apache Spark otimizada para a plataforma de serviços de nuvem do Microsoft Azure.  Projetado com os fundadores do Apache Spark, O Databricks é integrado com o Azure para fornecer instalação com um clique, fluxos de trabalho simplificados e um espaço de trabalho interativo que permite a colaboração entre os cientistas de dados, os engenheiros de dados e os analistas de negócios.

A colaboração no Databricks é habilitada pelo sistema de bloco de anotações da plataforma.  Os usuários podem criar, compartilhar e editar blocos de anotações com outros usuários dos sistemas.  Esses blocos de anotações permitem aos usuários escrever um código executado em clusters do Spark gerenciados no ambiente do Databricks.  Esses blocos de anotações dão suporte total a R e fornecem aos usuários acesso ao Spark por meio dos pacotes `SparkR` e `sparklyr`.

Uma vez que o Databricks se baseia no Spark e tem um forte foco na colaboração, a plataforma é frequentemente usada pelas equipes de cientistas de dados que trabalham juntas em análises complexas de grandes conjuntos de dados.  Como os blocos de anotações no Databricks dão suporte a outras linguagens além do R, é especialmente útil para as equipes em que os analistas usam diferentes idiomas para seu trabalho principal.

O artigo ["O que é o Azure Databricks?"](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)
pode fornecer mais detalhes sobre a plataforma e ajudá-lo a começar.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
O [Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) é uma ferramenta colaborativa do tipo "arrastar e soltar", que você pode usar para criar, testar e implantar soluções de análise preditiva na nuvem.  Ele permite a cientistas de dados emergentes criar e implantar modelos de machine learning sem necessidade de escrever muito código.

O ML Studio dá suporte a R e Python.  Você pode usar o R com o ML Studio de duas maneiras.

### <a name="custom-r-scripts-in-your-experiments"></a>Scripts do R personalizados em seus experimentos
Primeiro, você pode estender a manipulação de dados e as funcionalidades de aprendizado de máquina do ML Studio escrevendo scripts em R personalizados.
Embora o ML Studio inclua uma ampla variedade de módulos para preparação e análise de dados, ele não consegue se equiparar às funcionalidades de uma linguagem madura como R. Portanto, o serviço foi projetado para permitir que você apresente seus próprios scripts personalizados em R nos casos em que os módulos fornecidos não atendam às suas necessidades.

Para aproveitar essa funcionalidade, arraste e solte um módulo "Executar Script em R" em seu teste.  Em seguida, use o editor de código no painel "Propriedades" para gravar um novo script em R ou colar um script existente.  No script, você pode fazer referência a pacotes do R externos.  Você pode usar o script para manipular os dados ou treinar modelos complexos de ML que não fazem parte da biblioteca padrão do modelo do ML Studio.

Para obter uma introdução detalhada sobre como usar o R em experimentos do ML Studio, confira o ["Tutorial de Início Rápido para a linguagem de programação em R para o Azure Machine Learning".](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Criar, gerenciar e implantar experimentos de seu ambiente local do R
A outra maneira de usar o R com o ML Studio é usar o
pacote <code>[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)</code> para monitorar e controlar o processo de experimentação com o ambiente de programação R.  Esse pacote, mantido pela Microsoft, permite carregar e baixar os conjuntos de dados para e do Azure ML, interrogar experimentos, publicar as funções do R como serviços Web do Azure ML e executar dados do R por meio de serviços Web existentes e recuperar a saída.

Este pacote torna muito mais fácil usar o Azure ML como uma plataforma de implantação escalonável para o código R.  Em vez de clicar e arrastar na interface do usuário, você pode automatizar todo o processo de implantação usando ferramentas que você já conhece.

## <a name="azure-batch"></a>Lote do Azure
Para trabalhos de R em grande escala, você pode usar o [Lote do Azure](https://azure.microsoft.com/services/batch/).  Esse serviço fornece gerenciamento de computação e agendamento de trabalhos de escala de nuvem, portanto, você pode dimensionar sua carga de trabalho do R em dezenas, centenas ou milhares de máquinas virtuais.  Como é uma plataforma de computação generalizada, há algumas opções para a execução de trabalhos do R no Lote do Azure.

Uma opção é usar o pacote <code>[doAzureParallel](https://github.com/Azure/doAzureParallel)</code> da Microsoft.  Este pacote do R é um back-end paralelo para o pacote `foreach`.  Ele permite que cada iteração do loop `foreach` seja executado em paralelo em um nó no cluster do Lote do Azure.  Para obter uma introdução ao pacote, leia a postagem no blog ["doAzureParallel: aproveite a computação flexível do Azure diretamente na sessão do R"](https://azure.microsoft.com/blog/doazureparallel/).

Outra opção para executar um script R no Lote do Azure é empacotar seu código com "RScript.exe" como um Aplicativo de Lote no portal do Azure.  Para obter instruções detalhadas, confira ["Carga de trabalho em R no Lote do Azure."](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)

Uma terceira opção é usar o AZTK ([Kit de Ferramentas de Engenharia de Dados Distribuídos do Azure](https://github.com/Azure/aztk)), que permite provisionar clusters do Spark sob demanda usando contêineres do Docker no Lote do Azure.  Isso fornece uma maneira econômica de executar trabalhos do Spark no Azure.  Usando [SparklyR com AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), seus scripts R podem ser expandidos na nuvem com facilidade e economia.

## <a name="azure-notebooks"></a>Azure Notebooks
O [Azure Notebooks](https://notebooks.azure.com) é um método de baixo custo e baixo atrito para desenvolvedores do R que preferem trabalhar com blocos de anotações para levar o código para o Azure.  É um serviço gratuito para qualquer um desenvolver e executar código no navegador usando [Jupyter](https://jupyter.org/), que é um projeto de software livre que permite combinar texto de markdown, código executável e elementos gráficos em uma única tela.

Embora o Azure Notebooks seja uma opção viável para projetos em pequena escala, ele tem algumas limitações que o tornam inadequado para projetos de ciência de dados em grande escala.  No momento, o serviço limita o processo do cada bloco de anotações a 4 GB de memória, e os conjuntos de dados só podem ter 1 GB.  No entanto, para a publicação de análises menores, essa é uma opção fácil e sem custos.

## <a name="azure-sql-database"></a>Banco de Dados SQL do Azure
O [Banco de Dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) é o serviço de banco de dados relacional de nuvem inteligente totalmente gerenciado da Microsoft.  Ele permite usar todo o poder do SQL Server sem nenhuma complicação para configurar a infraestrutura.  Isso inclui [Serviços de Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), uma das adições mais recentes ao serviço SQL.

Esse recurso oferece um mecanismo inserido de análise preditiva e ciência de dados que pode executar código R dentro de um banco de dados do SQL Server como procedimentos armazenados, como scripts T-SQL que contém instruções em R ou código R que contém T-SQL.  Em vez de extrair dados do banco de dados e carregá-los no ambiente de R, você carrega seu código R diretamente no banco de dados e deixa-o em execução junto com os dados.

Embora os Serviços do Machine Learning façam parte do SQL Server local desde 2016, é relativamente novo ao Banco de Dados SQL do Azure.  Ele está atualmente em [versão prévia limitada](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap), mas continuará a evoluir.


### <a name="next-steps"></a>Próximas etapas
* [Como executar seu código R no Azure com mrsdeploy](http://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [Machine Learning Server na Nuvem](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [Recursos Adicionais do Machine Learning Server e do Microsoft R](https://docs.microsoft.com/machine-learning-server/resources-more)
* [R no Azure](https://github.com/yueguoguo/r-on-azure) – uma visão geral de pacotes, ferramentas e estudos de caso do uso do R com o Azure

---

<sub>O logotipo do R é &copy; 2016 The R Foundation, e é usado sob os termos da [licença Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/).</sub>