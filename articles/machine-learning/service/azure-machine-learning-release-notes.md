---
title: Novidades na versão
titleSuffix: Azure Machine Learning service
description: Saiba mais sobre as atualizações mais recentes para o Serviço do Azure Machine Learning e o aprendizado de máquina e SDKs de Python de preparação de dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: larryfr
author: Blackmist
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 2dd397e879dd76cabd119a3cbedff34041be2d13
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298480"
---
# <a name="azure-machine-learning-service-release-notes"></a>Notas de versão do serviço de aprendizado de máquina do Azure

Neste artigo, conheça os lançamentos de serviços do Aprendizado de Máquina do Azure.  Para obter uma descrição completa de cada SDK, visite os documentos de referência para:
+ O Azure Machine Learning [ **principal SDK para Python**](https://aka.ms/aml-sdk)
+ O [**SDK de Preparação de Dados do Azure Machine Learning**](https://aka.ms/data-prep-sdk)

Veja [a lista de problemas conhecidos](resource-known-issues.md) para aprender sobre erros e soluções conhecidas.

## <a name="2019-05-28"></a>2019-05-28

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>SDK v1.1.4 de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  + Agora você pode usar as seguintes funções de linguagem de expressão para extrair e analisar os valores de data e hora em novas colunas.
    + `RegEx.extract_record()` extrai elementos de data e hora em uma nova coluna.
    + `create_datetime()` cria objetos de data e hora de elementos de data e hora separadas.
  + Ao chamar `get_profile()`, agora você pode ver que o quantil colunas são rotuladas como (EST.) para indicar claramente os valores são aproximações.
  + Agora você pode usar * * glob durante a leitura do armazenamento de BLOBs do Azure.
    + Por exemplo: `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Correções de bug**
  + Corrigido um bug relacionado à leitura de um arquivo Parquet de uma fonte remota (Blob do Azure).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine Learning SDK for Python v1.0.39
+ **Alterações**
  + Opção de configuração de execução de auto_prepare_environment está sendo preterida, com auto preparar se tornando o padrão.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>SDK v1.1.3 de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  + Adicionado suporte para ler de um banco de dados PostgresSQL, chamar read_postgresql ou usando um repositório de dados.
    + Consulte os exemplos em guias de instruções:
      + [Bloco de anotações de ingestão de dados](https://aka.ms/aml-data-prep-ingestion-nb)
      + [Bloco de anotações do repositório de dados](https://aka.ms/aml-data-prep-datastore-nb)

+ **Correções de bugs e melhorias**
  + Correção de problemas com a conversão de tipo de coluna:
  + Agora corretamente converte uma coluna numérica ou booliana em uma coluna booleana.
  + Agora não falhará ao tentar definir uma coluna de data para ser do tipo Data.
  + Aprimoradas JoinType tipos e documentação de referência que o acompanha. Ao unir dois fluxos de dados, agora você pode especificar um desses tipos de junção:
    + NENHUM, CORRESPONDER, INTERNA, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, COMPLETO.
  + Inferência a reconhecer mais formatos de data de tipo de dados aprimorados.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Portal do Azure

No portal do Azure, agora você pode:
+ Crie e execute testes automatizados de ML 
+ Criar uma VM do bloco de anotações para experimentar os notebooks do Jupyter de exemplo ou o seu próprio.
+ Nova seção de criação (visualização) no espaço de trabalho de serviço Machine Learning, que inclui automatizada de aprendizado de máquina, Interface Visual e hospedado VMs de bloco de anotações
    + Criar automaticamente um modelo usando o aprendizado de máquina automatizada 
    + Use uma operação de arrastar e soltar Interface Visual para executar experimentos
    + Crie uma VM de bloco de anotações para explorar dados, criar modelos e implantar serviços.
+ Ao vivo atualizando em executar relatórios de métrica e de gráfico e executar as páginas de detalhes
+ Visualizador do arquivo atualizado para instantâneos nas páginas de detalhes de execução, saídas e logs.
+ Experiência de criação de relatórios novos e aprimorados no guia experimentos. 
+ Capacidade adicional para baixar o arquivo Config XML da página de visão geral do espaço de trabalho do serviço de Azure Machine Learning.
+ Dá suporte à criação de espaço de trabalho de serviço de Machine Learning do espaço de trabalho do Azure Databricks 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine Learning SDK for Python v1.0.33
+ **Novos recursos**
  + O _Workspace.create_ método agora aceita as configurações de cluster padrão para clusters de CPU e GPU.
  + Se a falha na criação do espaço de trabalho, os recursos dependentes são limpos.
  + SKU de registro de contêiner padrão do Azure foi alternado para básico.
  + O registro de contêiner do Azure é criado lentamente, quando necessário para a criação de imagem ou de execução.
  + Suporte para ambientes para execuções de treinamento.

### <a name="notebook-virtual-machine"></a>Máquina Virtual de bloco de anotações 

Use uma VM do bloco de anotações como um ambiente seguro e pronto para empresas de hospedagem para notebooks Jupyter em que você pode programar experimentos de aprendizado de máquina, implante modelos como pontos de extremidade da web e realizar todas as outras operações com suporte pelo SDK de aprendizado de máquina de Azure usando o Python. Ele fornece vários recursos:
+ [Crie rapidamente um bloco de anotações VM pré-configurada](quickstart-run-cloud-notebook.md) que tem a versão mais recente do SDK do Azure Machine Learning e os pacotes relacionados.
+ O acesso é protegido por meio de tecnologias comprovadas, como HTTPS, autorização e autenticação do Active Directory do Azure.
+ Armazenamento em nuvem confiável de blocos de anotações e o código em sua conta de armazenamento de BLOBs do espaço de trabalho do Azure Machine Learning. Você pode excluir com segurança sua VM notebook sem perder seu trabalho.
+ Pré-instalado notebooks de exemplo para explorar e experimentar os recursos do serviço de Azure Machine Learning.
+ Recursos de personalização completa de VMs do Azure, qualquer tipo VM, todos os pacotes, todos os drivers. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>SDK de aprendizado de máquina do Azure para v1.0.33 Python lançado.

+ Modelos do Azure ML Hardware Accelerated na [FPGAs](concept-accelerate-with-fpgas.md) está disponível.
  + Agora você pode [usar o pacote de modelos de azureml da aceleração](how-to-deploy-fpga-web-service.md) para:
    + Treinar os pesos de uma rede neural profunda com suporte (ResNet 50, ResNet 152, DenseNet 121, VGG-16 e VGG SSD)
    + Usar o aprendizado com a DNN com suporte de transferência
    + Registre o modelo com o serviço de gerenciamento de modelos e colocar o modelo em um contêiner
    + Implantar o modelo em uma VM do Azure com um FPGA em um cluster do serviço de Kubernetes do Azure (AKS)
  + Implantar o contêiner para um [caixa de dados do Azure Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) dispositivo de servidor
  + Classificar seus dados com o ponto de extremidade com essa gRPC [exemplo](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automatizado de Machine Learning

+ Recurso de varredura para habilitar featurizers adicionando dinamicamente para otimizar o desempenho. Novo featurizers: funcionam incorporações, peso de evidência, alvo codificações, codificação de destino do texto, distância do cluster
+ CV inteligente para lidar com train/válido se divide dentro de ML automatizado
+ Algumas alterações de otimização de memória e melhoria de desempenho de tempo de execução
+ Melhoria de desempenho na explicação de modelo
+ Conversão de modelo ONNX para execução local
+ Adicionado suporte Subsampling
+ Inteligente parar quando não há critérios de saída definidos
+ Conjuntos empilhados

+ Previsão de série temporal
  + Prever a nova função de previsão   
  + Agora você pode usar validação cruzada de origem sem interrupção em dados de série temporal
  + Nova funcionalidade adicionada ao configurar a série defasagem de hora 
  + Nova funcionalidade adicionada para oferecer suporte a recursos de agregação de janela sem interrupção
  + Nova detecção de feriado e featurizer quando o código do país é definido no experimento configurações

+ Azure Databricks
  + Habilitada a previsão de série temporal e funcionalidade explainabilty/possibilidade de interpretação de modelo
  + Agora você pode cancelar e continuar (continue) ML automatizado experimentos
  + Adicionado suporte para processamento de vários núcleos

### <a name="mlops"></a>MLOps
+ **Depuração de contêineres de pontuação e implantação de local**<br/> Agora você pode implantar um modelo de ML localmente e iterar rapidamente em seu arquivo de pontuação e as dependências para garantir que eles se comportam como esperado.

+ **Introduced InferenceConfig & Model.deploy()**<br/> Modelo de implantação agora dá suporte à especificação de uma pasta de origem com um script de entrada, o mesmo que um RunConfig.  Além disso, implantação de modelo foi simplificada para um único comando.

+ **Acompanhamento de referência do Git**<br/> Os clientes vêm solicitando recursos básicos de integração de Git para algum tempo como ele ajuda a manter uma trilha de auditoria de ponta a ponta. Implementamos o rastreamento em entidades principais no AM do Azure para os metadados relacionados ao Git (repositório, confirmação, estado limpo). Essas informações serão coletadas automaticamente pelo SDK e da CLI.

+ **Serviço de criação de perfil de & validação de modelo**<br/> Os clientes frequentemente reclamarem da dificuldade para dimensionar corretamente a computação associada ao seu serviço de inferência de tipos. Com nosso modelo de serviço de criação de perfil, o cliente pode fornecer entradas de exemplo e podemos criará o perfil em outra CPU 16 / configurações de memória para determinar ideal de dimensionamento para a implantação.

+ **Traga sua própria imagem base para inferência de tipos**<br/> Outra reclamação comum foi a dificuldade de movimentação de experimentação para dependências de compartilhamento de RE de inferência de tipos. Com nossa imagem base novo recurso de compartilhamento, agora você pode reutilizar suas imagens de base de experimentação, dependências e todos, inferência de tipos. Isso deverá acelerar as implantações e reduzir a lacuna de interno para o loop externo.

+ **Experiência aprimorada de geração de esquema de Swagger**<br/> Nossa geração de swagger anterior método era impossível para automatizar e propensa a erros. Temos uma nova maneira de na linha de gerar esquemas de swagger de qualquer função de Python por meio de decoradores. Podemos ter livre esse código e nossa protocolo de geração de esquema não está ligado à plataforma do Azure ML.

+ **CLI do Azure ML já está disponível (GA)**<br/> Modelos agora podem ser implantados com um único comando CLI. Temos os comentários dos clientes comuns que ninguém implanta um modelo de AM de um bloco de anotações do Jupyter. O [ **documentação de referência da CLI** ](https://aka.ms/azmlcli) foi atualizado.


## <a name="2019-04-22"></a>2019-04-22

SDK de aprendizado de máquina do Azure para v1.0.30 Python lançado.

O [ `PipelineEndpoint` ](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py) foi introduzir para adicionar uma nova versão de um pipeline publicado, mantendo o mesmo ponto de extremidade.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>SDK v1.1.2 de preparação de dados do Azure Machine Learning

Observação: SDK do Python de preparação de dados não instalará `numpy` e `pandas` pacotes. Ver [atualizadas as instruções de instalação](https://aka.ms/aml-data-prep-installation).

+ **Novos recursos**
  + Agora você pode usar a transformação dinâmica.
    + Guia de instruções: [Bloco de anotações de Pivot](https://aka.ms/aml-data-prep-pivot-nb)
  + Agora você pode usar expressões regulares em funções nativas.
    + Exemplos:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Agora você pode usar `to_upper`  e `to_lower`  funções na linguagem de expressão.
  + Agora você pode ver o número de valores exclusivos de cada coluna em um perfil de dados.
  + Para algumas das etapas leitor comumente usadas, você pode agora passar o `infer_column_types` argumento. Se ele for definido como `True`, preparação de dados tentará detectar e converter automaticamente os tipos de coluna.
    + `inference_arguments` agora foi preterida.
  + Agora você pode chamar `Dataflow.shape`.

+ **Correções de bugs e melhorias**
  + `keep_columns` agora aceita um argumento opcional adicional `validate_column_exists`, que verifica se o resultado de `keep_columns` conterá todas as colunas.
  + Todas as etapas de leitor (que leiam de um arquivo) agora aceita um argumento opcional adicional `verify_exists`.
  + Melhorar o desempenho de leitura de dados do pandas e obtendo os perfis de dados.
  + Corrigido um bug em que a divisão de uma única etapa de um fluxo de dados falhou com um único índice.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Portal do Azure
  + Agora você pode reenviar um Script existente que execute em um cluster de computação remota existente. 
  + Agora você pode executar um pipeline publicado com novos parâmetros na guia Pipelines. 
  + Detalhes de execução agora dá suporte a um novo Visualizador de arquivo de instantâneo. Quando você tiver enviado uma execução específica, você pode exibir um instantâneo do diretório. Você também pode baixar o notebook que foi enviado para iniciar a execução.
  + Agora você pode cancelar as execuções de pai do portal do Azure.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine Learning SDK for Python v1.0.23

+ **Novos recursos**
  + O SDK do Azure Machine Learning agora dá suporte a Python 3.7.
  + Avaliadores de DNN de aprendizado de máquina do Azure agora fornecem suporte a várias versões interno. Por exemplo, `TensorFlow`  estimador agora aceita um `framework_version` parâmetro e os usuários podem especificar versão '1.10' ou '1.12'. Para obter uma lista das versões compatíveis com sua versão atual do SDK, chame `get_supported_versions()` na classe de estrutura desejada (por exemplo, `TensorFlow.get_supported_versions()`).
  Para obter uma lista das versões compatíveis com a versão mais recente do SDK, consulte o [documentação do avaliador de DNN](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>1.1.1 do SDK de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  + Você pode ler várias fontes de caminho de dados/armazenamento de dados/DataReference usando transformações read_ *.
  + Você pode executar as seguintes operações em colunas para criar uma nova coluna: divisão, floor, módulo, energia, comprimento.
  + Preparação de dados agora é parte do pacote de diagnóstico do Azure ML e registrará em log informações de diagnóstico por padrão.
    + Para desativar isso, defina essa variável de ambiente como true: DISABLE_DPREP_LOGGER

+ **Correções de bugs e melhorias**
  + Documentação de código aprimorada para funções e classes comumente usadas.
  + Corrigido um bug no auto_read_file que falha ao ler os arquivos do Excel.
  + Adicionada a opção substituir a pasta em read_pandas_dataframe.
  + Desempenho aprimorado de instalação da dependência dotnetcore2 e suporte adicionado para Fedora 27/28 e 1804 do Ubuntu.
  + Melhoria no desempenho de leitura de Blobs do Azure.
  + Detecção de tipo de coluna agora dá suporte a colunas do tipo Long.
  + Corrigido um bug em que alguns valores de data foram sendo exibidos como carimbos de hora em vez de objetos de data e hora do Python.
  + Corrigido um bug em que as contagens de algum tipo foram sendo exibidas como duplas, em vez de números inteiros.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine Learning SDK for Python v1.0.21

+ **Novos recursos**
  + O *azureml.core.Run.create_children* método permite a criação de baixa latência de várias filho é executado com uma única chamada.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>V 1.1.0 do SDK de preparação de dados do Azure Machine Learning

+ **Alterações da falha**
  + O conceito do pacote de preparação de dados foi preterido e não é mais suportado. Em vez de manter vários fluxos de dados em um único pacote, você pode manter fluxos de dados individualmente.
    + Guia de instruções: [Bloco de anotações de abertura e salvando fluxos de dados](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Novos recursos**
  + Preparação de dados agora pode reconhecer a colunas que correspondem a um tipo específico de semântica e dividir adequadamente. Os STypes atualmente com suporte incluem: endereço de email, as coordenadas geográficas (latitude e longitude), endereços IPv4 e IPv6, o número de telefone dos EUA e CEP dos Estados Unidos.
    + Guia de instruções: [Bloco de anotações de tipos semântico](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Preparação de dados agora suporta as seguintes operações para gerar uma coluna resultante de duas colunas numéricas: subtrair, multiplicar, dividir e de módulo.
  + Você pode chamar `verify_has_data()` em um fluxo de dados para verificar se o fluxo de dados pode produzir registros se executado.

+ **Correções de bugs e melhorias**
  + Agora você pode especificar o número de compartimentos para usar em um histograma para perfis de coluna numérica.
  + O `read_pandas_dataframe` transformação agora requer o DataFrame ter - cadeia de caracteres ou bytes - nomes de coluna de tipo.
  + Corrigido um bug no `fill_nulls` transformação, em que valores não foram corretamente preenchidos se a coluna estava ausente.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine Learning SDK for Python v1.0.18

 + **Alterações**
   + O pacote azureml tensorboard substitui azureml-contrib-tensorboard.
   + Com esta versão, você pode configurar uma conta de usuário em seu cluster de computação gerenciada (amlcompute), ao criá-lo. Isso pode ser feito passando essas propriedades na configuração de provisionamento. Você pode encontrar mais detalhes na [documentação de referência SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>SDK v1.0.17 de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  + Agora dá suporte à adição de duas colunas numéricas para gerar uma coluna resultante usando a linguagem de expressão.

+ **Correções de bugs e melhorias**
  + Aprimorada a documentação e os parâmetros de verificação para random_split.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>SDK v1.0.16 de preparação de dados do Azure Machine Learning

+ **Correção de bug**
  + Correção de uma entidade de serviço problema de autenticação que foi causado por uma alteração na API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine Learning SDK for Python v1.0.17

+ **Novos recursos**

  + O Azure Machine Learning agora fornece suporte de primeira classe para estrutura popular de DNN Chainer. Usando o [ `Chainer` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) usuários da classe podem facilmente treinar e implantar modelos do Encadeador.
    + Saiba como [executar treinamento distribuído com ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Saiba como [executar o ajuste de hiperparâmetro com Chainer usando HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Pipelines de Machine Learning do Azure adicionada uma execução de Pipeline com base em modificações de repositório de dados de gatilho de capacidade. O pipeline [notebook agenda](https://aka.ms/pl-schedule) é atualizado para demonstrar esse recurso.

+ **Correções de bugs e melhorias**
  + Adicionamos suporte a Pipelines de Machine Learning do Azure para definir a propriedade source_directory_data_store para um repositório de dados desejado (como um armazenamento de BLOBs) em [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) que são fornecidos para o [ PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). Por padrão etapas usam o armazenamento de arquivos do Azure como o repositório de dados de backup, que podem ser executados em problemas de limitação quando um grande número de etapas é executado simultaneamente.

### <a name="azure-portal"></a>Portal do Azure

+ **Novos recursos**
  + Experiência do editor para relatórios de tabela novo arrastar e soltar. Os usuários podem arrastar uma coluna do poço para a área de tabela em que uma visualização da tabela será exibida. As colunas podem ser reorganizadas.
  + Novo Visualizador do arquivo de Logs
  + Links para o experimento é executado, computação, modelos, imagens e implantações do guia de atividades

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>SDK v1.0.15 de preparação de dados do Azure Machine Learning

+ **Novos recursos**
  + Agora oferece suporte à gravação de arquivo fluxos a partir de um fluxo de dados de preparação de dados. Também fornece a capacidade de manipular os nomes de fluxo de arquivo para criar novos nomes de arquivo.
    + Guia de instruções: [Bloco de anotações de trabalhar com fluxos de arquivos](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Correções de bugs e melhorias**
  + Desempenho aprimorado de t-Digest em grandes conjuntos de dados.
  + Preparação de dados agora dá suporte a leitura de dados de um caminho de dados.
  + Uma codificação ativa agora funciona em colunas de Boolianas e numéricas.
  + Outras diversas correções de bugs.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>SDK do Azure Machine Learning para Python v1.0.15

+ **Novos recursos**
  + Pipelines de Machine Learning do Azure adicionada AzureBatchStep ([notebook](https://aka.ms/pl-azbatch)), HyperDriveStep (bloco de notas) e a funcionalidade de agendamento com base no tempo ([notebook](https://aka.ms/pl-schedule)).
  +  DataTranferStep atualizado para funcionar com o Azure SQL Server e banco de dados do Azure para PostgreSQL ([notebook](https://aka.ms/pl-data-trans)).

+ **Alterações**
  + `PublishedPipeline.get_published_pipeline` preterido em favor de `PublishedPipeline.get`.
  + `Schedule.get_schedule` preterido em favor de `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.12

+ **Novos recursos**
  + A preparação de dados agora oferece suporte à leitura de um banco de dados SQL do Azure usando o armazenamento de dados.
 
+ **Alterações**
  + Melhoria no desempenho de memória de determinadas operações em dados grandes.
  + `read_pandas_dataframe()` agora exige que `temp_folder` seja especificado.
  + A propriedade `name` em `ColumnProfile` foi preterida - usar `column_name` em vez disso.

## <a name="2019-01-28"></a>28/01/2019

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>SDK do Azure Machine Learning para Python v1.0.10

+ **Alterações**: 
  + O SDK do Azure Machine Learning não tem mais pacotes azure-cli como dependência. Especificamente, as dependências azure-cli-core e azure-cli-profile foram removidas de azureml-core. Estas são alterações que afetam o usuário:
    + Se você estiver executando "az login" e, em seguida, usando o sdk do azureml, o SDK fará o log de código do navegador ou dispositivo no mais uma vez. Ele não usará nenhum estado de credencial criado por "az login".
    + Para autenticação de CLI do Azure, assim como usando "az login", use a classe _azureml.core.authentication.AzureCliAuthentication_. Para a autenticação de CLI do Azure, execute _pip install azure-cli_ no ambiente do Python em que você instalou o azureml-sdk.
    + Se você estiver usando "az login" e usando uma entidade de serviço para a automação, é recomendável usar a classe _azureml.core.authentication.ServicePrincipalAuthentication_, pois o azureml-sdk não usará o estado de credenciais criado pela CLI do Azure. 

+ **Correções de bug**: Esta versão contém principalmente correções de bugs secundários

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.8

+ **Correções de bug**
  + Melhoria no desempenho de obtenção de perfis de dados.
  + Bugs secundários relacionados ao relatório de erros foram corrigidos.
  
### <a name="azure-portal-new-features"></a>Portal do Azure: novos recursos
+ Nova experiência de criação de gráficos de arrastar e soltar para relatórios. Os usuários podem arrastar uma coluna ou um atributo do poço à área do gráfico, na qual o sistema selecionará automaticamente um tipo de gráfico apropriado para o usuário com base no tipo de dados. Os usuários podem alterar o tipo de gráfico para outros tipos aplicáveis ou adicionar outros atributos.

    Tipos de gráfico compatíveis:
    - Gráfico de linhas
    - Histograma
    - Gráfico de barras empilhadas
    - Gráfico de caixa
    - Gráfico de Dispersão
    - Gráfico de bolhas
+ O portal agora gera relatórios dinamicamente para experimentos. Quando um usuário envia uma execução a um experimento, um relatório será gerado automaticamente com métricas registradas em log e gráficos para permitir a comparação entre diferentes execuções. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>SDK do Azure Machine Learning para Python v1.0.8

+ **Correções de bug**: Esta versão contém principalmente correções de bugs secundários

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.7

+ **Novos recursos**
  + Aprimoramentos do repositório de dados (documentado em [Guia de instruções do repositório de dados](https://aka.ms/aml-data-prep-datastore-nb))
    + Capacidade adicional de ler e gravar no compartilhamento de Arquivo do Azure e em Repositórios de Dados do ADLS em expansão.
    + Ao usar repositórios de dados, a preparação de dados agora dá suporte ao uso à autenticação de entidade de serviço em vez da autenticação interativa.
    + Adicionado suporte para URLs de wasb e wasbs.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>SDK de Preparação de Dados do Azure Machine Learning v1.0.6

+ **Correções de bug**
  + Corrigido o bug com a leitura de contêineres de Blob do Azure legíveis públicos no Spark

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>SDK do Azure Machine Learning para Python v1.0.6
+ **Correções de bug**: Esta versão contém principalmente correções de bugs secundários

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>SDK de preparação de dados do Azure Machine Learning v1.0.4

+ **Novos recursos**
  + A função `to_bool` agora permite que os valores incompatíveis sejam convertidos em valores de erro. Esse é o novo comportamento de incompatibilidade de padrão para `to_bool` e `set_column_types`, enquanto o comportamento padrão anterior era para converter valores incompatíveis como False.
  + Ao chamar `to_pandas_dataframe`, há uma nova opção para interpretar valores ausente/nulos em colunas numéricas como NaN.
  + Capacidade adicional para verificar o tipo de retorno de algumas expressões para garantir a consistência de tipo e falha antecipada.
  + Agora você pode chamar `parse_json` para analisar valores em uma coluna como objetos JSON e expandi-las em várias colunas.

+ **Correções de bug**
  + Corrigido um bug que travou `set_column_types` no Python 3.5.2.
  + Corrigido um bug que falhou ao se conectar ao armazenamento de dados usando uma imagem do AML.

+ **Atualizações**
  * [Notebooks de exemplo](https://aka.ms/aml-data-prep-notebooks) para obter tutoriais de Introdução, estudos de caso e guias de instruções.

## <a name="2018-12-04-general-availability"></a>04/12/2018: Disponibilidade geral

O Serviço do Azure Machine Learning já está disponível ao público em geral.

### <a name="azure-machine-learning-compute"></a>Computação do Azure Machine Learning
Com esta versão, estamos anunciando uma nova experiência de computação gerenciada por meio da [Computação do Machine Learning](how-to-set-up-training-targets.md#amlcompute). Esse destino de computação substitui a computação do IA do Lote do Azure para o Azure Machine Learning. 

O destino de computação:
+ É usado para treinamento e lote inferência/pontuação de modelo
+ É único - para computação de vários nós
+ Realiza o gerenciamento de cluster e o plano de trabalho para o usuário
+ Dimensionado automaticamente por padrão
+ Suporte para recursos de CPU e GPU 
+ Permite o uso de VMs de baixa prioridade para redução de custos

A Computação do Azure Machine Learning pode ser criada no Python, pelo portal do Azure ou pela CLI. Precisa ser criada na região do seu workspace e não pode ser anexada a outro workspace. Esse destino de computação usa um contêiner do Docker para execução e empacota as dependências para replicar o mesmo ambiente em todos os nós.

> [!Warning]
> É recomendável criar um novo workspace para usar a Computação do Azure Machine Learning. Há uma possibilidade remota de que os usuários que tentarem criar a Computação do Azure Machine Learning em um workspace existente recebam um erro. A computação existente no workspace deve continuar a funcionar sem problemas.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>SDK do Azure Machine Learning para Python v1.0.2
+ **Alterações da falha**
  + Nesta versão, removemos o suporte para a criação de uma VM do Azure Machine Learning. Ainda é possível anexar uma VM em nuvem existente ou um servidor local remoto. 
  + Também removemos o suporte para o IA do Lote, agora, o suporte para todos ocorrerá por meio da Computação do Azure Machine Learning.

+ **Novo**
  + Para pipelines do aprendizado de máquina:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Updated**
  + Para pipelines do aprendizado de máquina:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) aceita runconfig agora
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) agora copia de e para uma fonte de dados SQL
    + Agendar a funcionalidade no SDK para criar e atualizar agendas e executar pipelines publicados

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>SDK de preparação de dados do Azure Machine Learning v0.5.2
+ **Alterações da falha** 
  * `SummaryFunction.N` foi renomeado para `SummaryFunction.Count`.
  
+ **Correções de bug**
  * Use o Token de Execução do AML mais recente ao ler e gravar em armazenamentos de dados ou execuções remotas. Anteriormente, se o Token de Execução do AML fosse atualizado no Python, o tempo de execução de preparação de dados não seria atualizado com o novo Token de Execução do AML.
  * Mensagens de erro mais claras adicionais
  * to_spark_dataframe() não falhará mais quando o Spark usar a serialização `Kryo`
  * Agora, o Inspetor de Contagem de Valor pode mostrar mais de mil valores únicos
  * Não haverá mais falha na Divisão Aleatória se o Fluxo de Dados original não tiver um nome  

+ **Mais informações**
  * [SDK de preparação de dados do Azure Machine Learning](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Documentos e notebooks
+ Pipelines de ML
  + Notebooks novos e atualizados para começar a ver exemplos de pipelines, escopo de lote e transferência de estilo: https://aka.ms/aml-pipeline-notebooks
  + Saiba como [criar seu primeiro pipeline](how-to-create-your-first-pipeline.md)
  + Saiba como [executar previsões em lotes usando pipelines](how-to-run-batch-predictions.md)
+ Computação de destino do Azure Machine Learning
  + [Notebooks de exemplo](https://aka.ms/aml-notebooks) agora são atualizados para usar essa nova computação gerenciada.
  + [Saiba mais sobre essa computação](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Portal do Azure: novos recursos
+ Crie e gerencie tipos de [Computação do Azure Machine Learning](how-to-set-up-training-targets.md#amlcompute) no portal.
+ Monitore o uso de cota e a [cota de solicitações](how-to-manage-quotas.md) da Computação do Azure Machine Learning.
+ Veja o status do cluster da Computação do Machine Learning em tempo real.
+ Foi adicionado suporte à rede virtual para a criação de Computação do Azure Machine Learning e do Serviço de Kubernetes do Azure.
+ Execute novamente os pipelines publicados com parâmetros existentes.
+ Novos [gráficos de aprendizado de máquina automatizado](how-to-track-experiments.md#auto) para modelos de classificação (gráfico de importância de recursos de comparação de precisão e calibragem com explicabilidade de modelo) e modelos de regressão (resíduos e gráfico de importância de recursos com explicabilidade de modelo). 
+ Os pipelines podem ser exibidos no portal do Azure




## <a name="2018-11-20"></a>20-11-2018

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>SDK do Azure Machine Learning para Python v0.1.80

+ **Alterações da falha** 
  * Namespace *azureml.Train.widgets* foi movido para *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* substitui as seguintes classes - *azureml.core.compute.BatchAICompute* e *azureml.core.compute.DSVMCompute*. A última classe será removida nas versões subsequentes. A classe AmlCompute tem agora uma definição mais fácil e simplesmente precisa de um vm_size e do max_nodes e dimensionará automaticamente a seu cluster de 0 para o max_nodes quando um trabalho for enviado. Nossos [notebooks de exemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) foram atualizados com essas informações e devem fornecer exemplos de uso. Esperamos que você goste dessa simplificação e dos muitos dos recursos mais interessantes para ficar em uma versão posterior!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>SDK de preparação de dados do AML v0.5.1 

Saiba mais sobre o SDK de preparação de dados lendo [docs de referência](https://aka.ms/data-prep-sdk).
+ **Novos recursos**
   * Criada uma nova CLI DataPrep para executar pacotes DataPrep e exibir o perfil de dados para um conjunto de dados ou o fluxo de dados
   * API de SetColumnType reprojetado para melhorar a usabilidade
   * Smart_read_file renomeado para auto_read_file
   * Agora inclui distorção e curtose no Perfil de Dados
   * Pode criar amostra com amostragem estratificada
   * Pode ler de arquivos zip que contenham arquivos CSV
   * Pode dividir conjuntos de dados por linha com a divisão aleatória (por exemplo, nos conjuntos de teste-treinamento)
   * Pode obter os tipos de dados de coluna de um fluxo de dados ou um perfil de dados chamando `.dtypes`
   * Pode obter a contagem de linhas de um fluxo de dados ou um perfil de dados chamando `.row_count`

+ **Correções de bug**
   * Corrigidos para conversão dupla 
   * Ativo corrigido após qualquer adição de coluna 
   * Corrigido um problema com FuzzyGrouping, onde ele não detectaria grupos em alguns casos
   * Função de classificação corrigida para respeitar a ordem de classificação de várias colunas
   * Fixos e/ou expressões para ser semelhante a como `pandas` lida com isso
   * Leitura corrigida do caminho dbfs
   * Mensagens de erro tornadas mais compreensíveis 
   * Agora não há mais falhar durante a leitura no destino de computação remota usando o token do AML
   * Agora não falhará na DSVM do Linux
   * Agora não falha quando os valores de cadeia de caracteres não são em predicados de cadeia de caracteres
   * Agora manipula erros de asserção ao fluxo de dados deve falhar corretamente
   * Agora dá suporte a locais de armazenamento dbutils montados no Azure Databricks

## <a name="2018-11-05"></a>05-11-2018

### <a name="azure-portal"></a>Portal do Azure 
O portal do Azure para o serviço de AML tem as seguintes atualizações:
  * Uma nova **Pipelines** guia para pipelines publicados.
  * Adicionado suporte para anexar a um cluster HDInsight existente como um destino de computação.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>SDK de aprendizado de máquina do AML para Python v0.1.74

+ **Alterações da falha** 
  * Workspace.compute_targets, datastores, experimentos, imagens, modelos e *webservices* são propriedades em vez de métodos. Por exemplo, substitua *Workspace.compute_targets()* com *Workspace.compute_targets*.
  * *Run.get_context* pretere *Run.get_submitted_run*. O último método será removido em versões subsequentes.
  * A classe *PipelineData* agora espera um objeto de armazenamento de dados como um parâmetro em vez de datastore_name. Da mesma forma, o *Pipeline* aceita default_datastore em vez de default_datastore_name.

+ **Novos recursos**
  * O caderno de amostras de [amostra do AML](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) agora usa etapas de MPI.
  * O widget RunDetails para notebooks Jupyter é atualizado para mostrar uma visualização do pipeline.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>SDK de preparação de dados do AML v0.4.0 
 
+ **Novos recursos**
  * Tipo de contagem adicionada ao perfil de dados 
  * Contagem de valores e histograma estão agora disponíveis
  * Mais percentis no perfil de dados
  * Mediana está disponível em Summarize
  * Agora há suporte para Python 3.7
  * Quando você salva um fluxo de dados que contém datastores em um pacote DataPrep, as informações do armazenamento de dados serão mantidas como parte do pacote DataPrep
  * A gravação no armazenamento de dados agora é suportada 
        
+ **Erro corrigido**
  * Extensões de inteiro não assinado de 64 bits agora são tratadas corretamente no Linux
  * Etiqueta de texto incorreta corrigida para arquivos de texto simples em smart_read
  * O tipo de coluna de cadeia de caracteres agora aparece na visualização de métricas
  * A contagem de tipos agora é fixada para mostrar ValueKinds mapeados para um único FieldType em vez de um individual
  * Write_to_csv não falha quando o caminho é fornecido como uma cadeia de caracteres
  * Ao usar Replace, deixar "localizar" em branco não falhará mais 

## <a name="2018-10-12"></a>12-10-2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>SDK de aprendizado de máquina do Azure para Python v0.1.68

+ **Novos recursos**
  * Suporte a vários locatários ao criar novo espaço de trabalho.

+ **Bugs corrigidos**
  * Você não precisa fixar a versão da biblioteca pynacl ao implantar o serviço web.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>SDK de preparação de dados de aprendizado de máquina do Azure v0.3.0

+ **Novos recursos**
  * Adicionado método transform_partition_with_file (script_path), que permite aos usuários passar no caminho de um arquivo Python para executar

## <a name="2018-10-01"></a>01-10-2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>SDK de aprendizado de máquina do Azure para Python v0.1.65
A [versão 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) inclui novos recursos, mais documentação, correções de bugs e mais [exemplos de blocos de notas](https://aka.ms/aml-notebooks).

Veja [a lista de problemas conhecidos](resource-known-issues.md) para aprender sobre erros e soluções conhecidas.

+ **Alterações da falha**
  * Workspace.Experiments, Workspace.models, Workspace.compute_targets, Workspace.images, dicionário de retorno Workspace.web_services, retornado anteriormente de lista. Veja a documentação da API [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py).

  * Automatizados de aprendizado de máquina removido erro normalizado quadrada da média das métricas principais.

+ **HyperDrive**
  * Várias correções de bugs do HyperDrive para Bayesian, melhorias de desempenho para obter chamadas de métricas. 
  * Atualização de Tensorflow 1.10 do 1.9 
  * Otimização de imagem do Docker para inicialização a frio. 
  * O trabalho agora reporta o status correto, mesmo se ele sair com um código de erro diferente de 0. 
  * Validação de atributo RunConfig no SDK. 
  * O objeto de execução HyperDrive suporta cancelamentos semelhantes a uma execução normal: não é necessário passar nenhum parâmetro. 
  * Melhorias de widget para manter o estado dos valores suspensos para execuções distribuídas e execuções do HyperDrive. 
  * TensorBoard e outros arquivos de log suportam fixos para o servidor Parameter. 
  * Suporte ao Intel (R) MPI no lado do serviço. 
  * Bugfix ao ajuste de parâmetro para correção de execução distribuída durante a validação no BatchAI. 
  * O Context Manager agora identifica a instância primária. 

+ **Experiência do portal do Azure**
  * log_table () e log_row () são suportados em detalhes da execução. 
  * Crie automaticamente gráficos para tabelas e linhas com 1, 2 ou 3 colunas numéricas e uma coluna categórica opcional.

+ **Automatizado de Machine Learning**
  * Melhor tratamento e documentação de erros 
  * Corrigidos problemas de desempenho de recuperação de propriedades de execução. 
  * Fixo continuar a execução do problema. 
  * Correção de ensembling problemas de iteração.
  * Bug deslocada de treinamento fixo no MAC OS.
  * Curva média de solicitação de pull/ROC no cenário de validação personalizada da macro da resolução.
  * Removida a lógica extra de índice.
  * Removemos o filtro de get_output API.

+ **Pipelines**
  * Adicionado um método Pipeline.publish () para publicar um pipeline diretamente, sem precisar executar uma execução primeiro.   
  * Adicionado um método PipelineRun.get_pipeline_runs () para buscar as execuções de pipeline que foram geradas a partir de um pipeline publicado.

+ **Project Brainwave**
  * Suporte atualizado para novos modelos de IA disponíveis em FPGAs.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>SDK de preparação de dados de aprendizado de máquina do Azure v0.2.0
[Versão 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) inclui recursos e correções de bug a seguir:

+ **Novos recursos**
  * Suporte para codificação one-hot
  * Suporte para a transformação de quantil
   
+ **Bug corrigido:**
  * Funciona com qualquer versão do Tornado, sem necessidade de rebaixar sua versão do Tornado
  * O valor conta para todos os valores, não apenas os três primeiros

## <a name="2018-09-public-preview-refresh"></a>09-2018 (atualização da visualização pública)

Uma nova versão atualizada do Azure Machine Learning: Leia mais sobre essa versão: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Próximas etapas

Leia a visão geral do [Serviço do Azure Machine Learning](../service/overview-what-is-azure-ml.md).
