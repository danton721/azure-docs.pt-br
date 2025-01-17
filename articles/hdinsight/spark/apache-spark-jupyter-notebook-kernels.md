---
title: Kernels para o bloco de anotações do Jupyter em clusters do Spark no Azure HDInsight
description: Saiba mais sobre os kernels PySpark, PySpark3 e Spark para o notebook do Jupyter disponíveis com clusters do Spark no Azure HDInsight.
keywords: bloco de anotações do jupyter no spark, jupyter spark
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: hrasheed
ms.openlocfilehash: b2ae24c0449b009db6fcecdd8a1366ea5154629a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257813"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Kernels para o bloco de anotações do Jupyter em clusters do Apache Spark no Azure HDInsight 

Os clusters do HDInsight Spark fornecem kernels que você pode usar com o notebook Jupyter do [Apache Spark](https://spark.apache.org/) para testar seus aplicativos. Um kernel é um programa que é executado e que interpreta seu código. Os três kernels são:

- **PySpark** - para aplicativos escritos em Python2.
- **PySpark3** - para aplicativos escritos em Python3.
- **Spark** - para aplicativos escritos em Scala.

Neste artigo, você aprenderá como usar esses kernels e os benefícios de usá-los.

## <a name="prerequisites"></a>Pré-requisitos

Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte o artigo sobre como [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Criar um bloco de anotações do Jupyter no Spark HDInsight

1. Dos [portal do Azure](https://portal.azure.com/), selecione seu cluster Spark.  Consulte [lista e mostrar clusters](../hdinsight-administer-use-portal-linux.md#showClusters) para obter instruções. O **visão geral** exibir é aberta.

2. Dos **visão geral** exibir, o **painéis do Cluster** caixa, selecione **notebook Jupyter**. Se você receber uma solicitação, insira as credenciais de administrador para o cluster.

    ![Bloco de anotações do Jupyter no Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Bloco de anotações do Jupyter no Spark") 
  
   > [!NOTE]  
   > Você também pode acessar o bloco de anotações do Jupyter de seu cluster do Spark abrindo o seguinte URL no navegador. Substitua **CLUSTERNAME** pelo nome do cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Selecione **New**e, em seguida, selecione **Pyspark**, **PySpark3**, ou **Spark** para criar um bloco de anotações. Use o kernel Spark para aplicativos em Scala, o kernel PySpark para aplicativos em Python2 e o kernel PySpark3 para aplicativos em Python3.
   
    ![Kernels para bloco de anotações do Jupyter no Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Kernels para bloco de anotações do Jupyter no Spark") 

4. Um notebook é aberto com o kernel selecionado.

## <a name="benefits-of-using-the-kernels"></a>Benefícios de usar os kernels

Estes são alguns dos benefícios de usar os novos kernels com o bloco de anotações do Jupyter nos clusters do Spark HDInsight.

- **Contextos de predefinição**. Com os kernels **PySpark**, **PySpark3** ou **Spark**, não é necessário definir os contextos Spark ou Hive explicitamente antes de começar a trabalhar com seus aplicativos. Eles estão disponíveis para você por padrão. Esses contextos são:
   
  * **sc** - para o contexto do Spark
  * **sqlContext** : para o contexto Hive
   
    Portanto, você não precisa executar instruções como as seguintes para definir os contextos:
   
         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)
   
    Em vez disso, pode usar os contextos predefinidos diretamente em seu aplicativo.

- **A mágica da célula**. O kernel de PySpark fornece algumas "mágicas"” predefinidas, que são comandos especiais que podem ser chamados com `%%` (por exemplo, `%%MAGIC` `<args>`). O comando mágico deve ser a primeira palavra em uma célula do código e de permitir várias linhas de conteúdo. A palavra mágica deve ser a primeira palavra na célula. Adicionar algo antes da palavra mágica, até mesmo comentários, causa um erro.     Para saber mais sobre palavras mágicas, clique [aqui](https://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    A tabela a seguir lista as diferentes palavras mágicas disponíveis por meio dos kernels.

   | Mágica | Exemplo | DESCRIÇÃO |
   | --- | --- | --- |
   | ajuda |`%%help` |Gera uma tabela de todos os comandos mágicos disponíveis com exemplo e descrição |
   | informações |`%%info` |Envia informações de sessão para o ponto de extremidade Livy atual |
   | CONFIGURAR |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Configura os parâmetros para a criação de uma sessão. O sinalizador force (-f) será obrigatório se uma sessão já tiver sido criada, o que garante que a sessão será descartada e recriada. Veja o [Corpo da Solicitação POST /sessions da Livy](https://github.com/cloudera/livy#request-body) para obter uma lista de parâmetros válidos. Os parâmetros devem ser passados como uma cadeia de caracteres JSON e devem estar na linha seguinte, logo após a mágica, conforme mostrado na coluna de exemplo. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Executa uma consulta do Hive no dqlContext. Se o parâmetro `-o` for passado, o resultado da consulta será persistido no contexto %%local do Python como um dataframe do [Pandas](https://pandas.pydata.org/) . |
   | local |`%%local`<br>`a=1` |Todo o código nas linhas subsequentes é executado localmente. O código deve ser um código Python2 válido, independentemente do kernel que você está usando. Portanto, mesmo se você selecionou **PySpark3** ou **Spark** kernels ao criar o bloco de anotações, se você usar o `%%local` mágica em uma célula, essa célula só deve ter um código Python2 válido. |
   | logs |`%%logs` |Gera os logs da sessão atual do Livy. |
   | excluir |`%%delete -f -s <session number>` |Exclui uma sessão específica do ponto de extremidade atual do Livy. Você não pode excluir a sessão iniciada para o próprio kernel. |
   | limpeza |`%%cleanup -f` |Exclui todas as sessões do ponto de extremidade atual do Livy, incluindo a sessão deste notebook. O sinalizador de força -f é obrigatório. |

   > [!NOTE]  
   > Além das mágicas adicionados pelo kernel PySpark, você também pode usar as [mágicas internas do IPython](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), incluindo `%%sh`. Você pode usar a mágica `%%sh` para executar scripts e bloco de código no nó principal do cluster.

- **Visualização automática**. O kernel Pyspark visualiza automaticamente a saída das consultas de Hive e SQL. Escolha entre vários tipos diferentes de visualização, incluindo Tabela, Pizza, Linha, Área, Barra.

## <a name="parameters-supported-with-the-sql-magic"></a>Parâmetros compatíveis com a mágica de %%sql
A palavra mágica `%%sql` é compatível com diversos parâmetros que podem ser usados para controlar o tipo de saída que você recebe ao executar consultas. A tabela a seguir lista as saídas.

| Parâmetro | Exemplo | DESCRIÇÃO |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Use esse parâmetro para manter o resultado da consulta, no contexto Python %%local, como um dataframe [Pandas](https://pandas.pydata.org/) . O nome da variável dataframe é o nome da variável que você especificar. |
| -q |`-q` |Use esta opção para desativar visualizações da célula. Se você não deseja que o conteúdo de uma célula para autovisualize e deseja apenas capturá-los como um dataframe, use `-q -o <VARIABLE>`. Se desejar desativar as visualizações sem capturar os resultados (por exemplo, para executar uma consulta SQL, como uma instrução `CREATE TABLE`), use `-q` sem especificar um argumento `-o`. |
| -m |`-m <METHOD>` |Onde **METHOD** é **take** ou **sample** (o padrão é **take**). Se o método for **take**, o kernel selecionará elementos da parte superior do conjunto de dados de resultados especificado por MAXROWS (descrito posteriormente nesta tabela). Se o método for **sample**, o kernel experimentará aleatoriamente os elementos do conjunto de dados segundo o parâmetro `-r`, descrito a seguir nesta tabela. |
| -r |`-r <FRACTION>` |Aqui **FRACTION** é um número de ponto flutuante entre 0.0 e 1.0. Se o método de amostragem para a consulta SQL for `sample`, o kernel experimentará aleatoriamente a fração especificada dos elementos do conjunto de resultados. Por exemplo, se você executar uma consulta SQL com os argumentos `-m sample -r 0.01`, 1% das linhas resultantes serão amostradas aleatoriamente. |
| -n |`-n <MAXROWS>` |**MAXROWS** é um valor inteiro. O kernel limita o número de linhas de saída para **MAXROWS**. Se **MAXROWS** for um número negativo como **-1**, o número de linhas no conjunto de resultados não será limitado. |

**Exemplo:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

A instrução acima faz o seguinte:

* Seleciona todos os registros de **hivesampletable**.
* Como usamos - q, ele desativa autovisualization.
* Como usamos `-m sample -r 0.1 -n 500` , ele recolhe um exemplo de 10% das linhas na hivesampletable aleatoriamente e limita o tamanho do conjunto de resultados a 500 linhas.
* Por fim, como usamos `-o query2` , ele também salva a saída em um dataframe chamado **query2**.

## <a name="considerations-while-using-the-new-kernels"></a>Considerações ao usar os novos kernels

Seja qual for o kernel usado, deixar os notebooks em execução consumirá os recursos de cluster.  Com esses kernels, como os contextos são predefinidos, simplesmente sair dos notebooks não elimina o contexto e, portanto, os recursos do cluster continuam em uso. Uma prática recomendada é usar a opção **Fechar e Interromper** do menu **Arquivo** do notebook quando você terminar de usar o notebook, o que elimina o contexto e sai do notebook.

## <a name="where-are-the-notebooks-stored"></a>Onde os blocos de anotações são armazenados?

Se o cluster usa o armazenamento do Azure como a conta de armazenamento padrão, os blocos de anotações do Jupyter são salvos para a conta de armazenamento na pasta **/HdiNotebooks**.  Os notebooks, arquivos de texto e pastas que você cria no Jupyter podem ser acessados na conta de armazenamento.  Por exemplo, se você usar o Jupyter para criar uma pasta **myfolder** e um notebook **myfolder/mynotebook.ipynb**, poderá acessar esse notebook em `/HdiNotebooks/myfolder/mynotebook.ipynb` dentro da conta de armazenamento.  O inverso também é possível, ou seja, se você carregar um notebook diretamente em sua conta de armazenamento em `/HdiNotebooks/mynotebook1.ipynb`, ele também ficará visível no Jupyter.  Os logs são mantidos na conta de armazenamento mesmo após a exclusão do cluster.

> [!NOTE]  
> Os clusters HDInsight com o Azure Data Lake Storage como armazenamento padrão não armazenam notebooks no armazenamento associado.

A forma como os blocos de anotações são salvos na conta de armazenamento é compatível com [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Portanto, se você se conectar por SSH ao cluster, poderá usar comandos de gerenciamento de arquivos, como mostra o snippet a seguir:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                   # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter

Independentemente se o cluster usa o armazenamento do Azure ou o Azure Data Lake Storage como a conta de armazenamento padrão, os notebooks também são salvos em um nó principal do cluster em `/var/lib/jupyter`.

## <a name="supported-browser"></a>Navegador com suporte

Os blocos de anotações do Jupyter em clusters do Spark HDInsight só têm suporte no Google Chrome.

## <a name="feedback"></a>Comentários

Os kernels novos estão evoluindo e amadurecerão com o tempo. Isso também pode significar que as APIs podem mudar à medida que esses kernels amadurecem. Agradecemos o envio quaisquer comentários que você tenha ao usar esses novos kernels. Isso é muito útil na formação da versão final desses kernels. Você pode deixar seus comentários/feedback sob o **comentários** seção na parte inferior deste artigo.

## <a name="seealso"></a>Consulte também
* [Visão geral: Apache Spark no Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Cenários
* [Apache Spark com BI: Executar análise de dados interativa usando o Spark no HDInsight com ferramentas de BI](apache-spark-use-bi-tools.md)
* [Apache Spark com Machine Learning: Usar o Spark no HDInsight para analisar a temperatura de prédios usando dados do sistema de HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark com Machine Learning: Usar o Spark no HDInsight para prever resultados da inspeção de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de log do site usando o Apache Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Criar e executar aplicativos
* [Criar um aplicativo autônomo usando Scala](apache-spark-create-standalone-application.md)
* [Execute trabalhos remotamente em um cluster do Apache Spark usando o Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Use o Plug-in de Ferramentas do HDInsight para IntelliJ IDEA para criar e enviar aplicativos Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use o Plugin do HDInsight Tools para o IntelliJ IDEA para depurar os aplicativos do Apache Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use os blocos de anotações do Apache Zeppelin com um cluster do Apache Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Usar pacotes externos com blocos de notas Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gerenciar recursos
* [Gerenciar os recursos de cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
* [Rastrear e depurar trabalhos em execução em um cluster do Apache Spark no HDInsight](apache-spark-job-debugging.md)
