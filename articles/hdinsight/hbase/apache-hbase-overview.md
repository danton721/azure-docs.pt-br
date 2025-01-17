---
title: O que é o Apache HBase no HDInsight do Azure?
description: Uma introdução ao Apache HBase no HDInsight, uma compilação de banco de dados NoSQL no Hadoop. Aprenda sobre casos de uso e compare HBase com outros clusters Hadoop.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: hrasheed
ms.openlocfilehash: 358b835b42862bd0ceb1a5c4a48ba4b18a567f4d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235905"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>O que é o Apache HBase no HDInsight do Azure

O [Apache HBase](https://hbase.apache.org/) é um banco de dados NoSQL de software livre, que se baseia no [Apache Hadoop](https://hadoop.apache.org/) e é modelado com base no [Google BigTable](https://cloud.google.com/bigtable/). O HBase fornece acesso aleatório e uma coerência forte para grandes quantidades de dados não estruturados e semiestruturados em um banco de dados sem esquema, organizado por famílias de colunas.

Da perspectiva do usuário, o HBase é semelhante a um banco de dados. Os dados são armazenados nas linhas e colunas de uma tabela e os dados em uma linha são agrupados por família de colunas. O HBase é um banco de dados sem esquema, no aspecto de que nem as colunas nem os tipos de dados armazenados nelas precisam ser definidos antes de serem utilizados. O código-fonte aberto é dimensionado linearmente para lidar com petabytes de dados em milhares de nós. Ele pode fazer uso da redundância de dados, do processamento em lote e de outros recursos que são fornecidos por aplicativos distribuídos no ecossistema do Hadoop.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Como o Apache HBase é implementado no Azure HDInsight?

O HBase do HDInsight é oferecido como um cluster gerenciado integrado ao ambiente do Azure. Os clusters são configurados para armazenar dados diretamente no [Armazenamento do Azure](./../hdinsight-hadoop-use-blob-storage.md), que fornece baixa latência e maior elasticidade nas escolhas de desempenho e custo. Isso permite que os clientes criem sites interativos que trabalham com grandes conjuntos de dados, de modo a criar serviços que armazenam dados de sensor e telemetria de milhões de pontos de extremidade e analisar esses dados por meio de trabalhos do Hadoop. O HBase e o Hadoop são bons pontos iniciais para projetos de big data no Azure e, especificamente, podem possibilitar que aplicativos em tempo real trabalhem com grandes conjuntos de dados.

A implementação do HDInsight utiliza a arquitetura de expansão do HBase para fornecer o compartilhamento automático de tabelas, uma sólida consistência para leituras e gravações e failover automático. O desempenho é aprimorado pelo cache na memória para leituras e streaming de alta produtividade para gravações. O cluster do HBase pode ser criado dentro da rede virtual. Para obter detalhes, consulte [Criar clusters do HDInsight na Rede Virtual do Azure](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Como os dados no HBase do HDInsight são gerenciados?
Os dados podem ser gerenciados no HBase usando os comandos `create`, `get`, `put` e `scan` do shell do HBase. Os dados são gravados no banco de dados usando `put` e lidos usando `get`. O comando `scan` é utilizado para obter dados de múltiplas linhas em uma tabela. Os dados também podem ser gerenciados utilizando a API C# do HBase, que oferece uma biblioteca de cliente sobre a API REST do HBase. Um banco de dados HBase também pode ser consultado usando o [Apache Hive](https://hive.apache.org/). Para ver uma introdução a esses modelos de programação, consulte [Introdução ao uso do Apache HBase com Apache Hadoop no HDInsight](./apache-hbase-tutorial-get-started-linux.md). Coprocessadores também estão disponíveis, que permitem que os nós de processamento de dados que hospedam o banco de dados.

> [!NOTE]  
> Não há suporte para thrift pelo HBase no HDInsight.

## <a name="scenarios-use-cases-for-apache-hbase"></a>Cenários: Casos de uso para o Apache HBase
O caso de uso canônico para o qual o BigTable (e por extensão, o HBase) foi criado a partir da pesquisa na web. Os mecanismos de pesquisa criam índices que mapeiam termos nas páginas da web que os contêm. Mas há muitos outros casos de uso aos quais o HBase se ajusta, muitos dos quais são descritos nesta seção.

* Repositório de valor-chave
  
    O HBase pode ser usado como um repositório de valor-chave e é adequado para gerenciar sistemas de mensagens. O Facebook utiliza o HBase para seu sistema de mensagens, e ele é ideal para armazenar e gerenciar comunicações pela Internet. O WebTable utiliza o HBase para pesquisar e gerenciar tabelas extraídas de páginas da Web.
* Dados de sensor
  
    O HBase é útil para capturar dados que são coletados gradativamente de várias fontes. Isso inclui análise de dados sociais, séries temporais, manter painéis interativos atualizados com tendências e contadores e gerenciar sistemas de logs de auditorias. Os exemplos incluem o terminal de comerciantes Bloomberg e o OpenTSDB (Open Time Series Database), que armazena e oferece acesso a métricas coletadas sobre a integridade dos sistemas de servidores.
* Consulta em tempo real
  
    [Apache Phoenix](https://phoenix.apache.org/) é um mecanismo de consulta SQL para o HBase no Apache. Ele é acessado como um driver JDBC e permite consultar e gerenciar tabelas do HBase utilizando o SQL.
* HBase como uma plataforma
  
    Os aplicativos podem ser executados sobre o HBase utilizando-o como um armazenamento de dados. Exemplos incluem Phoenix, [OpenTSDB](http://opentsdb.net/), Kiji e Titan. Os aplicativos também podem ser integrados ao HBase. Os exemplos incluem [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](https://lucene.apache.org/solr/), [Apache Storm](https://storm.apache.org/), [Apache Flume](https://flume.apache.org/), [ Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/) , [Ganglia](http://ganglia.info/), e [Apache Drill](https://drill.apache.org/).

## <a name="next-steps"></a>Próximas etapas
* [Introdução ao uso do Apache HBase com o Apache Hadoop no HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Criar clusters do HDInsight na Rede Virtual do Azure](./apache-hbase-provision-vnet.md)
* [Configurar a replicação do Apache HBase no HDInsight](apache-hbase-replication.md)
* [Utilizar o Apache Maven para compilar aplicativos Java que usam o HBase com o Apache HDInsight (Hadoop)](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Consulte também
* [HBase no Apache](https://hbase.apache.org/)
* [Guia de referência do HBase do Apache](https://hbase.apache.org/book.html)
* [Bigtable: Um sistema de armazenamento distribuído para dados estruturados](https://research.google.com/archive/bigtable.html)
