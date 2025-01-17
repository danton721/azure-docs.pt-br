---
title: Criar uma Máquina Virtual de Ciência de Dados do Ubuntu Linux
titleSuffix: Azure
description: Configure e crie uma Máquina Virtual de Ciência de Dados para Linux (Ubuntu) no Azure para realizar a análise e o aprendizado de máquina.
services: machine-learning
documentationcenter: ''
author: gopitk
ms.author: gokuma
manager: cgronlun
ms.custom: seodec18
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.openlocfilehash: 5a9fdebc8db0c2a1acc20a894f80cfcc87fb89d5
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236484"
---
# <a name="provision-the-data-science-virtual-machine-for-linux-ubuntu"></a>Provisionar a Máquina Virtual de Ciência de Dados para Linux (Ubuntu)

A Máquina Virtual de Ciência de Dados para Linux é uma imagem de máquina virtual baseada no Ubuntu que facilita a introdução ao aprendizado de máquina, incluindo o aprendizado aprofundado, no Azure. As ferramentas de aprendizado aprofundado incluem:

* [Caffe](https://caffe.berkeleyvision.org/): Uma estrutura de aprendizado aprofundado criada para velocidade, expressividade e modularidade
* [Caffe2](https://github.com/caffe2/caffe2): Uma versão de plataforma cruzada do Caffe
* [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK): Um kit de ferramentas de software de aprendizado profundo da Microsoft Research
* [H2O](https://www.h2o.ai/): Uma plataforma de big data de software livre e interface gráfica do usuário
* [Keras](https://keras.io/): Uma API de rede neural de alto nível em Python para TensorFlow, Microsoft Cognitive Toolkit e Theano
* [MXNet](https://mxnet.io/): Uma biblioteca de aprendizado aprofundado flexível e eficiente com muitas associações de linguagem
* [NVIDIA DIGITS](https://developer.nvidia.com/digits): Um sistema gráfico que simplifica tarefas comuns de aprendizado profundo
* [PyTorch](https://pytorch.org/): Uma biblioteca Python de alto nível com suporte para redes dinâmicas
* [TensorFlow](https://www.tensorflow.org/): Uma biblioteca de software livre para inteligência de máquina do Google
* [Theano](http://deeplearning.net/software/theano/): Uma biblioteca do Python para definir, otimizar e avaliar com eficiência expressões matemáticas que envolvem matrizes multidimensionais
* [Torch](http://torch.ch/): Uma estrutura de computação científica com amplo suporte para algoritmos de aprendizado de máquina
* CUDA, cuDNN e o driver NVIDIA
* Muitos exemplos de Notebooks Jupyter

Todas as bibliotecas são as versões de GPU, embora também sejam executadas na CPU.

A Máquina Virtual de Ciência de Dados para Linux também contém ferramentas populares para atividades de desenvolvimento e ciência de dados, incluindo:

* O Microsoft R Server Developer Edition com Microsoft R Open
* Distribuição do Anaconda Python (versões 2.7 e 3.5), incluindo bibliotecas de análise de dados populares
* JuliaPro - uma distribuição curada da linguagem Julia, com bibliotecas populares de análise de dados e científicas
* Instância autônoma de Spark e Hadoop de nó único (HDFS, Yarn)
* JupyterHub - um servidor de notebook Jupyter multiusuário com suporte para kernels R, Python, PySpark, Julia
* Gerenciador de Armazenamento do Azure
* CLI (interface de linha de comando) do Azure para gerenciar recursos do Azure
* Ferramentas de Machine Learning
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Um sistema de aprendizado de máquina rápido com suporte a técnicas como online, hash, allreduce, reduções, learning2search, ativo e aprendizado interativo
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Uma ferramenta que fornece implementação de árvore aumentada rápida e precisa
  * [Rattle](https://togaware.com/rattle/): Uma ferramenta gráfica que facilita o uso de análise de dados e aprendizado de máquina em R
  * [LightGBM](https://github.com/Microsoft/LightGBM): Uma estrutura de gradient boosting rápida, distribuída e de alto desempenho
* SDK do Azure em Java, Python, node.js, Ruby, PHP
* Bibliotecas em R e Python para uso em Azure Machine Learning e outros serviços do Azure
* Ferramentas de desenvolvimento e editores (RStudio, PyCharm, IntelliJ, Emacs, vim)

Fazer a ciência de dados envolve a iteração em uma sequência de tarefas:

1. Localizar, carregar e pré-processar dados
1. Compilar e testar modelos
1. Implantar os modelos para consumo em aplicativos inteligentes

Cientistas de dados usam várias ferramentas para concluir essas tarefas. Pode ser muito demorado encontrar as versões apropriadas do software e depois baixar, compilar e instalar essas versões.

A Máquina Virtual de Ciência de Dados para Linux pode aliviar essa carga substancialmente. Use-a para iniciar rapidamente seu projeto de análise. Ela permite que você trabalhe nas tarefas em várias linguagens, incluindo R, Python, SQL, Java e C++. O SDK do Azure incluído na VM permite que você compile seus aplicativos usando vários serviços em Linux para a plataforma de nuvem da Microsoft. Além disso, você tem acesso a outras linguagens como Ruby, Perl, PHP e node.js, também pré-instaladas.

Não há encargos de software para esta imagem da VM de ciência de dados. Você paga apenas pelas taxas de uso de hardware do Azure, que são avaliadas com base no tamanho da máquina virtual que você provisiona. Encontre mais detalhes sobre as taxas de computação na [página de listagem de VMs do Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Outras versões da Máquina Virtual de Ciência de Dados

A imagem do [CentOS](linux-dsvm-intro.md) também está disponível, com muitas das mesmas ferramentas que a imagem do Ubuntu. Uma imagem do [Windows](provision-vm.md) também está disponível.

## <a name="prerequisites"></a>Pré-requisitos

Antes de criar uma Máquina Virtual de Ciência de Dados para Linux, você deve ter uma assinatura do Azure. Para obter uma, confira [Obter avaliação gratuita do Azure](https://azure.microsoft.com/free/).

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Criar sua Máquina Virtual de Ciência de Dados para Linux

Veja as etapas para criar uma instância da Máquina Virtual de Ciência de Dados para Linux:

1. Navegue até a listagem de máquinas virtuais no [Portal do Azure](https://portal.azure.com/#create/microsoft-dsvm.linux-data-science-vm-ubuntulinuxdsvmubuntu). Você pode ser solicitado a entrar na sua conta do Azure, caso ainda não esteja conectado. 
1. Clique em **Criar** (na parte inferior) para abrir o assistente. ![configure-data-science-vm](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
1. As seções a seguir fornecem as entradas para cada uma das etapas no assistente (enumeradas à direita da figura acima) que são usadas para criar a Máquina Virtual de Ciência de Dados da Microsoft. Aqui estão as entradas necessárias para configurar cada uma das seguintes etapas:

    a. **Noções básicas**:

   * **Nome**: O nome do servidor de ciência de dados que você está criando.
   * **Tipo de disco da VM**: Escolha **SSD Premium** se preferir uma unidade de estado sólido (SSD). Caso contrário, escolha **HDD Standard**. 
   * **Nome de usuário**: A primeira ID de entrada da conta.
   * **Senha**: Primeira senha da conta (você pode usar a chave pública SSH, em vez de senha).
   * **Assinatura**: Se você tiver mais de uma assinatura, selecione aquela em que o computador será criado e cobrado. Você deve ter privilégios de criação de recurso nessa assinatura.
   * **Grupo de recursos**: Você pode criar um grupo novo ou usar um grupo existente.
   * **Localização**: Selecione o data center mais apropriado. Normalmente, é o datacenter que contém a maioria dos seus dados ou que está mais próximo de sua localização física para o acesso mais rápido à rede.

   b. **Tamanho**:

   * Selecione um dos tipos de servidor que atenda aos seus requisitos funcionais e restrições de custo. Selecione uma VM de classe ND para instâncias de VM baseadas em GPU ou o NC. A página [Produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/) lista as regiões com GPUs.

   c. **Configurações**:

   * Na maioria dos casos, você pode simplesmente usar os valores padrão. Passe o ponteiro do mouse sobre o link informativo para obter ajuda sobre um campo específico, caso você queira considerar o uso de valores não padrão.

   d. **Resumo**:

   * Verifique se todas as informações inseridas estão corretas. Um link é fornecido para os termos de uso. A VM não tem encargos adicionais além dos de computação para o tamanho do servidor que você escolheu na etapa **Tamanho** . Para iniciar o provisionamento, clique em **Criar**. 

O provisionamento deve demorar cerca de 5 minutos. O status do provisionamento é exibido no Portal do Azure.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Como acessar uma Máquina Virtual de Ciência de Dados para Linux

É possível acessar a DSVM do Ubuntu usando três métodos:

1. SSH para sessões de terminal
1. X2Go para sessões gráficas
1. JupyterHub e JupyterLab para notebooks Jupyter

Você também pode anexar uma VM de ciência de dados para o Azure Notebooks para executar os notebooks Jupyter na VM e ignorar as limitações da camada de serviço gratuito. Para obter mais informações, consulte [gerenciar e configurar projetos de blocos de anotações - camada de computação](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

### <a name="ssh"></a>SSH

Após a criação da VM, você poderá entrar nela usando SSH. Use as credenciais da conta criada na seção **Noções básicas** da etapa 3 para a interface shell de texto. No Windows, você pode baixar uma ferramenta de cliente SSH como o [Putty](https://www.putty.org). Se você preferir uma área de trabalho gráfica (Sistema do Windows X), poderá usar o encaminhamento X11 no Putty ou instalar o cliente X2Go.

> [!NOTE]
> O cliente X2Go apresentou desempenho melhor do que o encaminhamento X11 em testes. Recomendamos o uso do cliente X2Go para uma interface gráfica de área de trabalho.

### <a name="x2go"></a>X2Go

A VM Linux já está provisionada com um servidor X2Go e pronta para aceitar conexões de cliente. Para se conectar à área de trabalho gráfica da VM do Linux, realize o seguinte procedimento em seu cliente:

1. Baixe e instale o cliente X2Go para sua plataforma de cliente [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. Execute o cliente X2Go e selecione **Nova Sessão**. Ele abrirá uma janela de configuração com várias guias. Insira os seguintes parâmetros de configuração:
   * **Guia Sessão**:
     * **Host**: O nome do host ou endereço IP da sua VM de Ciência de Dados Linux.
     * **Logon**: Nome de usuário na VM Linux.
     * **Porta SSH**: Deixe em 22, o valor padrão.
     * **Tipo de sessão**: Altere o valor para XFCE. No momento, a VM Linux dá suporte apenas à área de trabalho XFCE.
   * **Guia Mídia**: Você poderá desligar o suporte a som e impressão de cliente se não precisar usá-los.
   * **Pastas compartilhadas**: Caso você queira que os diretórios de seus computadores cliente sejam montados na VM Linux, adicione os diretórios de computador cliente que você deseja compartilhar com a VM nesta guia.

Após o logon na VM usando o cliente SSH ou a área de trabalho gráfica XFCE por meio do cliente X2Go, você estará pronto para começar a usar as ferramentas instaladas e configuradas na VM. No XFCE, você pode ver atalhos do menu de aplicativos e ícones da área de trabalho para muitas das ferramentas.

### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub e JupyterLab

A DSVM do Ubuntu executa o [JupyterHub](https://github.com/jupyterhub/jupyterhub), um servidor Jupyter multiusuário. Para se conectar, navegue até https:\// seu-vm-ip:8000 em seu laptop ou desktop, insira o nome de usuário e a senha que você usou para criar a VM e entrar. Muitos notebooks de exemplo estão disponíveis para você procurar e experimentar.

O JupyterLab, a próxima geração de notebooks Jupyter e JupyterHub, também está disponível. Para acessá-lo, entrar no JupyterHub e, em seguida, navegue até a URL https:\// seu-vm-ip:8000/usuário/your-nome de usuário/laboratório. Você pode definir o JupyterLab como o servidor de notebook padrão adicionando esta linha ao */etc/jupyterhub/jupyterhub_config.py*:

```python
c.Spawner.default_url = '/lab'
```

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Ferramentas Instaladas na Máquina Virtual de Ciência de Dados para Linux

### <a name="deep-learning-libraries"></a>Bibliotecas de aprendizado aprofundado

#### <a name="cntk"></a>CNTK

O Kit de Ferramentas Cognitivas da Microsoft é um kit de ferramentas de aprendizado profundo de software livre. As associações do Python estão disponíveis nos ambientes raiz e py35 do Conda. Ele também tem uma ferramenta de linha de comando (cntk) que já está em PATH.

Os blocos de anotações de amostra de Python estão disponíveis no JupyterHub. Para executar um exemplo básico na linha de comando, execute os seguintes comandos no shell:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Para saber mais, confira a seção sobre CNTK do [GitHub](https://github.com/Microsoft/CNTK) e o [wiki de CNTK](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe

Caffe é uma estrutura de aprendizado aprofundado da Berkeley Vision and Learning Center. Ele está disponível em /opt/caffe. Exemplos podem ser encontrados em /opt/caffe/examples.

#### <a name="caffe2"></a>Caffe2

Caffe2 é uma estrutura de aprendizado do Facebook que se baseia no Caffe. Ele está disponível no Python 2.7 no ambiente raiz do Conda. Para ativá-lo, execute o seguinte comando no shell:

```bash
source /anaconda/bin/activate root
```

Alguns blocos de anotações de amostra também estão disponíveis no JupyterHub.

#### <a name="h2o"></a>H2O

H2O é uma plataforma de análise preditiva e aprendizado de máquina rápido, na memória e distribuído. Um pacote do Python é instalado nos ambientes raiz e py35 do Anaconda. Um pacote R também é instalado. Para iniciar o H2O da linha de comando, execute `java -jar /dsvm/tools/h2o/current/h2o.jar`; existem várias [opções de linha de comando](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) que você pode configurar. A interface do usuário da Web do Flow pode ser acessada navegando até http://localhost:54321 para começar. Os blocos de anotações de amostra também estão disponíveis no JupyterHub.

#### <a name="keras"></a>Keras

Keras é uma API de rede neural de alto nível em Python capaz de ser executada em TensorFlow, Microsoft Cognitive Toolkit ou Theano. Ela está disponível nos ambientes raiz e py35 do Python.

#### <a name="mxnet"></a>MXNet

MXNet é uma estrutura de aprendizado profunda criada para eficiência e flexibilidade. Ela tem associações R e Python incluídas no DSVM. Os blocos de anotações de amostra estão incluído no JupyterHub e o código de exemplo está disponível em /dsvm/samples/mxnet.

#### <a name="nvidia-digits"></a>NVIDIA DIGITS

O NVIDIA Deep Learning GPU Training System, conhecido como DIGITS, é um sistema para simplificar tarefas comuns de aprendizado aprofundado, como gerenciamento de dados, design e treinamento de redes neurais em sistemas GPU e monitoramento de desempenho em tempo real com visualização avançada.

DIGITS está disponível como um serviço, chamado de dígitos. Inicie o serviço e navegue até http://localhost:5000 para começar.

DIGITS também é instalado como um módulo do Python no ambiente raiz Conda.

#### <a name="tensorflow"></a>TensorFlow

TensorFlow é a biblioteca de aprendizado aprofundado do Google. É uma biblioteca de software livre para computação numérica usando grafos de fluxo de dados. O TensorFlow está disponível no ambiente de py35 do Python e alguns blocos de anotações de amostra estão incluídos no JupyterHub.

#### <a name="theano"></a>Theano

Theano é uma biblioteca do Python para computação numérica eficiente. Ela está disponível nos ambientes raiz e py35 do Python. 

#### <a name="torch"></a>Torch

Tocha é uma estrutura de computação científica com amplo suporte para algoritmos de aprendizado de máquina. Ela está disponível em /dsvm/tools/torch e a enésima sessão interativa e o gerenciador de pacotes luarocks estão disponíveis na linha de comando. Os exemplos estão disponíveis em /dsvm/samples/torch.

PyTorch também está disponível no ambiente raiz do Anaconda. Os exemplos estão em /dsvm/samples/pytorch.

### <a name="microsoft-r-server"></a>Servidor R da Microsoft

R é uma das linguagens mais populares para análise de dados e aprendizado de máquina. Se você quiser usar o R para sua análise, a VM terá o MRS (Microsoft R Server) com o MRO (Microsoft R Open) e a MKL (Math Kernel Library). A MKL otimiza as operações matemáticas frequentes em algoritmos analíticos. O MRO é 100% compatível com CRAN-R e qualquer uma das bibliotecas R publicadas em CRAN pode ser instalada no MRO. O MRS fornece dimensionamento e operacionalização de modelos R em serviços Web. Edite seus programas R em um dos editores padrão como RStudio, vi ou Emacs. Se você preferir usar o editor de Emacs, ele estará pré-instalado. O pacote de Emacs ESS (Estatística de Fala Emacs) simplifica o trabalho com arquivos R no editor Emacs.

Para iniciar o console R, basta digitar **R** no shell. Esse comando leva você para um ambiente interativo. Para desenvolver seu programa R, você normalmente usa um editor como vi ou Emacs e, em seguida, executa os scripts no R. Com o RStudio, você tem um ambiente IDE gráfico completo para desenvolver o seu programa R.

Também há um script de R para você instalar os [20 melhores pacotes do R](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) , caso queira. Esse script pode ser executado quando você estiver na interface interativa de R, na qual você pode entrar (conforme mencionado) digitando **R** no shell.  

### <a name="python"></a>Python

O Anaconda Python é instalado com os ambientes Python 2.7 e 3.5. O ambiente 2.7 é chamado _raiz_ e o ambiente 3.5 é chamado _py35_. Essa distribuição contém o Python base com aproximadamente 300 dos mais populares pacotes de matemática, engenharia e análise de dados.

O ambiente py35 é o padrão. Para ativar o ambiente raiz (2.7):

```bash
source activate root
```

Para ativar o ambiente py35 novamente:

```bash
source activate py35
```

Para invocar a sessão interativa do Python, basta digitar **Python** no shell. 

Instale bibliotecas adicionais do Python usando ```conda``` ou ```pip```. Para pip, ative o ambiente correto primeiro, se não quiser o padrão:

```bash
source activate root
pip install <package>
```

Ou especifique o caminho completo até o pip:

```bash
/anaconda/bin/pip install <package>
```

Para conda, você sempre deve especificar o nome do ambiente (_py35_ ou _raiz_):

```bash
conda install <package> -n py35
```

Se estiver em uma interface gráfica ou tiver a configuração do encaminhamento X11, você poderá digitar o comando **pycharm** para iniciar o IDE do PyCharm Python. Você pode usar os editores de texto padrão. Além disso, você pode usar o Spyder, um IDE do Python que é fornecido com distribuições do Anaconda Python. O Spyder precisa de uma área de trabalho gráfica ou de encaminhamento X11. Um atalho para o Spyder é fornecido na área de trabalho gráfica.

### <a name="jupyter-notebook"></a>Notebook Jupyter

A distribuição do Anaconda também acompanha um notebook Jupyter, um ambiente de compartilhamento de código e de análise. O notebook Jupyter é acessado com o JupyterHub. Entre usando seu nome de usuário e senha locais do Linux.

O servidor do notebook Jupyter foi previamente configurado com os kernels do Python 2, do Python 3 e do R. Há um ícone de área de trabalho chamado "Bloco de anotações do Jupyter" para iniciar o navegador a fim de acessar o servidor do notebook. Se você estiver na VM via cliente SSH ou X2Go, também poderá visitar [https://localhost:8000/](https://localhost:8000/) para acessar o servidor do Notebook Jupyter.

> [!NOTE]
> Continue se você obtiver quaisquer avisos de certificado.

Você pode acessar o servidor de bloco de anotações do Jupyter por meio de qualquer host. Basta digitar *https://\<endereço IP ou nome DNS da VM\>:8000/*

> [!NOTE]
> A porta 8000 é aberta no firewall por padrão quando a VM é provisionada. 

Empacotamos exemplos de notebooks, um em Python em outro em R. Você pode ver o link para os exemplos na página inicial do bloco de anotações após a autenticação no notebook Jupyter usando a senha e o nome de usuário Linux locais. Você pode criar um novo notebook selecionando **Novo** e o kernel de linguagem apropriado. Caso não visualize o botão **Novo**, clique no ícone do **Jupyter** na parte esquerda superior para ir para a home page do servidor de notebook.

### <a name="apache-spark-standalone"></a>Apache Spark autônomo

Há uma instância autônoma do Apache Spark pré-instalada no DSVM Linux para ajudar você a desenvolver aplicativos Spark localmente, antes de testar e implantar em clusters maiores. Execute programas PySpark através do kernel de Jupyter. Ao abrir o Jupyter, clique no botão **Novo** e verá uma lista de kernels disponíveis. O “Spark – Python” é o kernel PySpark que permite a criação de aplicativos Spark usando a linguagem Python. Você também pode usar um IDE Python como PyCharm ou Spyder para compilar seu programa Spark. Nessa instância autônomo, a pilha de Spark é executado dentro do programa de cliente de chamada, que torna mais rápido e mais fácil solucionar problemas em comparação ao desenvolver em um cluster Spark.

Um exemplo de notebook PySpark é fornecido no Jupyter, e você pode encontrá-lo no diretório "SparkML" no diretório inicial do Jupyter ($HOME/notebooks/SparkML/pySpark). 

Se você estiver programando em R para Spark, você pode usar o Microsoft R Server, SparkR ou sparklyr. 

Antes de executar no contexto do Spark no Microsoft R Server, execute uma etapa de configuração única para habilitar uma instância local de HDFS Hadoop e Yarn de nó único. Por padrão, os serviços do Hadoop serão instalados, mas desabilitados no DSVM. Para habilitá-los, execute os seguintes comandos como raiz na primeira vez:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

Você pode interromper o Hadoop serviços relacionados quando não precisar deles, executando ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```

Um exemplo que demonstra como desenvolver e testar o MRS no contexto de Spark remoto (que é a instância de Spark autônoma no DSVM) é fornecido e disponibilizado na */dsvm/samples/MRS* directory.

### <a name="ides-and-editors"></a>IDEs e editores

Você tem a opção de vários editores de código, incluindo vi/VIM, Emacs, PyCharm, RStudio e IntelliJ. IntelliJ, RStudio e PyCharm são editores gráficos, e você precisa estar conectado a uma área de trabalho gráfica para usá-los. Há atalhos do menu do aplicativo e da área de trabalho para iniciar esses editores.

**VIM** e **Emacs** são editores baseados em texto. No Emacs, instalamos um pacote complementar chamado ESS (Emacs Speaks Statistics) que facilita o trabalho com R no editor Emacs. Mais informações podem ser encontradas em: [ESS](https://ess.r-project.org/).

**LaTex** é instalado por meio do pacote texlive, juntamente com um pacote complementar do Emacs chamado [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) , que simplifica a criação de seus documentos do LaTex no Emacs.  

### <a name="databases"></a>Bancos de dados

#### <a name="graphical-sql-client"></a>Cliente gráfico do SQL

**SQuirrel SQL**, um cliente gráfico do SQL, foi fornecido para conectar-se a bancos de dados diferentes (como o Microsoft SQL Server e MySQL) e executar consultas SQL. Você pode executar o SQuirrel SQL de uma sessão de área de trabalho gráfica (usando o cliente X2Go, por exemplo) usando um ícone da área de trabalho ou, usando o seguinte comando no shell:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

Antes do primeiro uso, configure os drivers e aliases de banco de dados. Os JDBC drivers estão localizados em:

*/usr/share/java/jdbcdrivers*

Para saber mais, confira [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Ferramentas de linha de comando para acessar o Microsoft SQL Server

O pacote de driver ODBC do SQL Server também vem com duas ferramentas de linha de comando:

**bcp**: O utilitário em massa bcp copia dados entre uma instância do Microsoft SQL Server e um arquivo de dados em um formato especificado pelo usuário. O utilitário bcp pode ser usado para importar grandes números de novas linhas para tabelas do SQL Server ou para exportar dados de tabelas para arquivos de dados. Para importar dados para uma tabela, você deve usar um arquivo de formato criado para essa tabela ou então entender a estrutura da tabela e os tipos de dados que são válidos para suas colunas.

Para saber mais, confira [Conectando-se com o bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: Você pode inserir instruções de Transact-SQL com o utilitário sqlcmd, bem como procedimentos do sistema e arquivos de script no prompt de comando. Esse utilitário usa o ODBC para executar lotes do Transact-SQL.

Para saber mais, confira [Conectando-se com o sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Há algumas diferenças nesse utilitário entre as plataformas Linux e Windows. Consulte a documentação para obter detalhes.

#### <a name="database-access-libraries"></a>Bibliotecas de acesso do banco de dados

Há bibliotecas disponíveis em R e Python para acessar bancos de dados.

* No R, o pacote **RODBC** ou pacote **dplyr** permite que você consulte ou execute instruções SQL no servidor de banco de dados.
* No Python, a biblioteca **pyodbc** fornece acesso ao banco de dados com o ODBC como a camada subjacente.  

### <a name="azure-tools"></a>Ferramentas do Azure

As ferramentas do Azure a seguir são instaladas na VM:

* **Interface de linha de comando do Azure**: A CLI do Azure permite criar e gerenciar recursos do Azure por meio de comandos do shell. Para invocar as ferramentas do Azure, digite apenas **azure help**. Para saber mais, confira a [página de documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Gerenciador de Armazenamento do Microsoft Azure**: O Gerenciador de Armazenamento do Microsoft Azure é uma ferramenta gráfica usada para navegar pelos objetos armazenados na sua Conta de Armazenamento do Azure e carregar e baixar os dados nos blobs do Azure. Você pode acessar o Gerenciador de Armazenamento do ícone de atalho da área de trabalho. Você pode invocá-lo de um prompt do shell digitando **StorageExplorer**. Você deve estar conectado em um cliente X2Go ou ter o encaminhamento do X11.
* **Bibliotecas do Azure**: Veja a seguir algumas das bibliotecas pré-instaladas.
  
  * **Python**: As bibliotecas relacionadas ao Azure no Python que estão instaladas são **azure**, **azureml**, **pydocumentdb** e **pyodbc**. Com as três primeiras bibliotecas, você pode acessar os serviços de armazenamento do Azure, o Azure Machine Learning e o Azure Cosmos DB (um banco de dados NoSQL no Azure). A quarta biblioteca, pyodbc (juntamente com o Microsoft ODBC Driver for SQL Server), habilita, do Python, o acesso ao SQL Server, ao Banco de Dados SQL do Azure e ao SQL Data Warehouse do Azure pelo uso de uma interface do ODBC. Insira **pip list** para ver todas as bibliotecas listadas. Certifique-se de executar este comando nos ambientes do Python 2.7 e 3.5.
  * **R**: As bibliotecas relacionadas ao Azure em R que estão instaladas são **AzureML** e **RODBC**.
  * **Java**: A lista de bibliotecas Java do Azure pode ser encontrada no diretório **/dsvm/sdk/AzureSDKJava** na VM. As bibliotecas principais são as APIs de armazenamento e gerenciamento do Azure, o Azure Cosmos DB e os drivers JDBC para SQL Server.  

Você pode acessar o [portal do Azure](https://portal.azure.com) do navegador Firefox previamente instalado. No portal do Azure, você pode criar, gerenciar e monitorar recursos do Azure.

### <a name="azure-machine-learning"></a>Azure Machine Learning

O Azure Machine Learning é um serviço de nuvem totalmente gerenciado que habilita você a compilar, implantar e compartilhar soluções de análise preditiva. Você compila seus modelos e experimentos do Azure Machine Learning Studio. Ele pode ser acessado de um navegador da Web na máquina virtual de ciência de dados visitando [Microsoft Azure Machine Learning](https://studio.azureml.net).

Depois de fazer logon no Azure Machine Learning Studio, você tem acesso a uma tela de experimentação em que você pode compilar um fluxo lógico para os algoritmos de aprendizado de máquina. Você também tem acesso a um Notebook do Jupyter hospedado no Azure Machine Learning e pode trabalhar perfeitamente com o Machine Learning Studio. Coloque em operação os modelos de aprendizado de máquina compilados encapsulando-os em uma interface de serviço Web. Operacionalização de modelos do machine learning permite que os clientes escritos em qualquer linguagem invocar as previsões desses modelos. Para saber mais, confira a [Documentação do Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

Você pode também criar seus modelos em R ou Python na VM e, em seguida, implantá-los em produção no Azure Machine Learning. Instalamos bibliotecas em R (**AzureML**) e Python (**azureml**) para habilitar essa funcionalidade.

Para saber mais sobre como implantar modelos em R e Python no Azure Machine Learning, confira a seção [Dez coisas que você pode fazer na Máquina Virtual de Ciência de Dados](vm-do-ten-things.md) (particularmente, a seção “Compilar os modelos usando R ou Python e operacionalizá-los usando o Azure Machine Learning”).

> [!NOTE]
> Essas instruções foram escritas para a versão do Windows da VM de Ciência de Dados. Mas as informações fornecidas sobre a implantação de modelos para o Azure Machine Learning são aplicáveis à VM Linux.

### <a name="machine-learning-tools"></a>Ferramentas do Machine Learning

A VM vem com algumas ferramentas e algoritmos de aprendizado de máquina que foram pré-compiladas e pré-instaladas localmente. Estão incluídos:

* **Vowpal Wabbit**: Um algoritmo de aprendizado rápido online.
* **xgboost**: Uma ferramenta que fornece algoritmos de árvore aumentados e otimizados.
* **Rattle**: Uma ferramenta gráfica baseada em R para facilitar a exploração de dados e a modelagem.
* **Python**: O Anaconda Python é fornecido com os algoritmos de aprendizado de máquina com bibliotecas como Scikit-learn. Você pode instalar outras bibliotecas usando o comando `pip install` .
* **LightGBM**: Uma estrutura de gradient boosting rápida, distribuída e de alto desempenho baseada em algoritmos de árvore de decisão.
* **R**: Uma vasta biblioteca de funções de aprendizado de máquina está disponível para R. Algumas das bibliotecas pré-instaladas são lm, glm, randomForest e rpart. Outras bibliotecas podem ser instaladas, executando:
  
        install.packages(<lib name>)

Veja algumas informações adicionais sobre as três primeiras ferramentas de aprendizado de máquina na lista.

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit é um sistema de aprendizado de máquina rápido que usa técnicas como online, hash, allreduce, reduções, learning2search, ativo e aprendizado interativo.

Para executar a ferramenta em um exemplo básico, use os seguintes comandos:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Há outras demonstrações maiores nesse diretório. Para saber mais sobre VW, confira [esta seção do GitHub](https://github.com/JohnLangford/vowpal_wabbit) e o [wiki do Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>XGBoost

Essa é uma biblioteca que é projetada e otimizada para algoritmos aumentados (de árvore). O objetivo dessa biblioteca é estender os limites de computação de máquinas para os extremos necessários de modo a fornecer aumento de árvore de grande escala escalonável, portátil e preciso.

Ele é fornecido como uma linha de comando, bem como uma biblioteca do R.

Para usar esta biblioteca em R, você pode iniciar a sessão interativa do R (basta digitar **R** no shell) e carregar a biblioteca.

Aqui está um exemplo simples, que você pode executar no prompt do R:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

Para executar a linha de comando do xgboost, aqui estão os comandos a executar no shell:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Um arquivo .model é gravado no diretório especificado. Mais informações sobre este exemplo de demonstração podem ser encontradas [no GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Para saber mais sobre o xgboost, confira a [página de documentação do xgboost](https://xgboost.readthedocs.org/en/latest/) e seu [repositório GitHub](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle

Rattle (the **R** **A**nalytical **T**ool **T**o **L**earn **E**asily – Ferramenta Analítica do R para Aprender com Facilidade) usa exploração e modelagem de dados com base em GUI. Ele apresenta resumos estatísticos e visuais dos dados, transforma os dados que podem ser modelados prontamente, compila modelos de dados supervisionados e sem supervisão, apresenta o desempenho dos modelos graficamente e calcula as pontuações de novos conjuntos de dados. Ele também gera código R, replicando as operações na interface do usuário que pode ser executado diretamente em R ou usado como ponto de partida para análise posterior.

Para executar o Rattle, você precisa estar em uma sessão de logon da área de trabalho gráfica. No terminal, digite ```R``` para entrar no ambiente R. No prompt do R, digite os seguintes comandos:

```R
library(rattle)
rattle()
```

Agora, uma interface gráfica é aberta com um conjunto de guias. Aqui estão as etapas do guia de início rápido no Rattle necessárias para usar um conjunto de dados de clima de exemplo e criar um modelo. Em algumas das etapas abaixo, você é solicitado a instalar e carregar automaticamente alguns pacotes do R que ainda não estão no sistema.

> [!NOTE]
> Se não tiver acesso para instalar o pacote no diretório do sistema (o padrão), você poderá ver uma solicitação na janela do console do R para instalar pacotes na sua biblioteca pessoal. Caso veja essas solicitações, responda *s* .

1. Clique em **Executar**.
1. Uma caixa de diálogo é exibida perguntando se você deseja usar o conjunto de dados meteorológicos de exemplo. Clique em **Sim** para carregar o exemplo.
1. Clique na guia **Modelo** .
1. Clique em **Executar** para compilar uma árvore de decisão.
1. Clique em **Desenhar** para exibir a árvore de decisão.
1. Clique no botão de opção **Floresta** e clique em **Executar** para compilar uma floresta aleatória.
1. Clique na guia **Avaliar** .
1. Clique no botão de opção **Risco** e em **Executar** para exibir duas gráficos de desempenho de Risco (Cumulativo).
1. Clique a guia **Log** para mostrar o código R gerado das operações anteriores.
   (Devido a um bug na versão atual do Rattle, você precisa inserir um caractere *#* na frente de *Exportar este log...* no texto do log.)
1. Clique no botão **Exportar** para salvar o script de R chamado *weather_script.R* na pasta base.

Você pode sair do Rattle e do R. Agora você pode modificar o script do R gerado ou usá-la como ele é para executá-lo em qualquer momento, para repetir tudo o que foi feito na interface do usuário do Rattle. Essa é uma maneira fácil, especialmente para iniciantes em R, de fazer análise e aprendizado de máquina rapidamente em uma interface gráfica e, ao mesmo tempo, gerar código em R automaticamente para modificar e/ou aprender.

## <a name="next-steps"></a>Próximas etapas

Veja como você pode continuar seu aprendizado e exploração:

* O passo a passo [Ciência de dados na Máquina Virtual da Ciência de Dados para Linux](linux-dsvm-walkthrough.md) mostra como executar várias tarefas comuns de ciência de dados com a VM de Ciência de Dados Linux provisionada aqui. 
* Explore as várias ferramentas de ciência de dados na VM de ciência de dados ao experimentar as ferramentas descritas neste artigo. Você também pode executar *dsvm-more-info* no shell contido na máquina virtual para uma introdução básica e ponteiros para obter mais informações sobre as ferramentas instaladas na VM.  
* Saiba como criar soluções completas de análise sistematicamente usando o [Processo de Ciência de Dados de Equipe](https://aka.ms/tdsp).
* Visite a [Galeria de IA do Azure](https://gallery.azure.ai/) para obter exemplos de análise de dados e aprendizado de máquina que usam os serviços de IA do Azure.
