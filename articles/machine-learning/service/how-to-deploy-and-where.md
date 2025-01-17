---
title: Como e onde implantar modelos
titleSuffix: Azure Machine Learning service
description: 'Saiba como e onde implantar os modelos do serviço do Azure Machine Learning incluindo: Instâncias de Contêiner do Azure, Serviço de Kubernetes do Azure, Azure IoT Edge e matriz de porta programável no campo.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 05/31/2019
ms.custom: seoapril2019
ms.openlocfilehash: 89539509e759da7f041ce0216397b1a9c8ff1f16
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753104"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implantar modelos com o serviço do Azure Machine Learning

Saiba como implantar seu modelo de machine learning como um serviço web na nuvem do Azure ou em dispositivos IoT Edge. 

O fluxo de trabalho é semelhante independentemente do [onde você implanta](#target) seu modelo:

1. Registre o modelo.
1. Preparar para implantar (especificar ativos, uso, o destino de computação)
1. Implante o modelo para o destino de computação.
1. Teste o modelo implantado, também chamado de serviço web.

Para obter mais informações sobre os conceitos envolvidos no fluxo de trabalho de implantação, confira [Gerenciar, implantar e monitorar modelos com o Serviço do Azure Machine Learning](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Pré-requisitos

- Um modelo. Se você não tiver um modelo treinado, você pode usar o modelo e arquivos de dependência fornecida no [este tutorial](https://aka.ms/azml-deploy-cloud).

- O [extensão de CLI do Azure para o serviço de Machine Learning](reference-azure-machine-learning-cli.md), [SDK do Python do Azure Machine Learning](https://aka.ms/aml-sdk), ou o [extensão do Azure Machine Learning Visual Studio Code](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registrar seu modelo

Registre seus modelos de machine learning no seu espaço de trabalho do Azure Machine Learning. O modelo pode vir de Azure Machine Learning ou pode vir de algum outro lugar. Os exemplos a seguir demonstram como registrar um modelo de arquivo:

### <a name="register-a-model-from-an-experiment-run"></a>Registrar um modelo de uma execução de teste

+ **Exemplo de Scikit-Learn usando o SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```
+ **Usando a CLI**
  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```


+ **Usando o VS Code**

  Registrar modelos usando os arquivos de modelo ou pastas com o [VS Code](how-to-vscode-tools.md#deploy-and-manage-models) extensão.

### <a name="register-an-externally-created-model"></a>Registrar um modelo criado externamente

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Você pode registrar um modelo criado externamente, fornecendo uma **caminho local** ao modelo. Você pode fornecer uma pasta ou um único arquivo.

+ **Exemplo ONNX com o SDK do Python:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

+ **Usando a CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

**Tempo estimado**: Cerca de 10 segundos.

Para obter mais informações, consulte a documentação de referência da [classe do modelo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Escolha um destino de computação

Os seguintes destinos de computação ou recursos de computação, podem ser usados para hospedar a implantação do serviço web. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Preparar-se para implantar

Para implantar como um serviço web, você deve criar uma configuração de inferência de tipos (`InferenceConfig`) e uma configuração de implantação. Inferência de tipos ou modelo de pontuação, é a fase em que o modelo implantado é usado para previsão, mais comumente em dados de produção. A configuração de inferência de tipos, você pode especificar os scripts e as dependências necessárias para atender seu modelo. A configuração de implantação você deve especificar os detalhes de como fornecer o modelo de destino de computação.


### <a id="script"></a> 1. Definir o seu script de entrada & dependências

O script de entrada recebe os dados enviados para um serviço web implantado e passa-o para o modelo. Ele então envia de volta ao cliente a resposta retornada pelo modelo. **O script é específico para seu modelo**; ele deve compreender os dados que o modelo de espera e retorna.

O script contém duas funções que carregam e executam o modelo:

* `init()`: Normalmente, essa função carrega o modelo em um objeto global. Essa função é executada apenas uma vez quando o contêiner do Docker para o serviço web é iniciado.

* `run(input_data)`: Essa função usa o modelo para prever um valor com base nos dados de entrada. Entradas e saídas para a execução normalmente usam JSON para serialização e desserialização. Você também pode trabalhar com os dados binários brutos. Você pode transformar os dados antes de enviá-los para o modelo ou antes de retorná-los ao cliente.

#### <a name="optional-automatic-swagger-schema-generation"></a>(Opcional) Geração automática do esquema de Swagger

Para automaticamente gerar um esquema para o serviço web, fornecer um exemplo de entrada e/ou de saída no construtor para um dos objetos do tipo definido e o tipo e o exemplo são usados para criar automaticamente o esquema. Serviço de Machine Learning do Azure, em seguida, cria uma [OpenAPI](https://swagger.io/docs/specification/about/) (Swagger) a especificação para o serviço web durante a implantação.

Atualmente, há suporte para os seguintes tipos:

* `pandas`
* `numpy`
* `pyspark`
* objeto padrão do Python

Para usar a geração de esquema, inclua o `inference-schema` pacote em seu arquivo de ambiente do conda. O exemplo a seguir usa `[numpy-support]` , pois o script de entrada usa um tipo de parâmetro numpy: 

#### <a name="example-dependencies-file"></a>Exemplo de arquivo de dependências
O YAML a seguir está um exemplo de um arquivo de dependências de Conda para inferência de tipos.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Se você quiser usar a geração de esquema automático, o script de entrada **devem** importar o `inference-schema` pacotes. 

Definir a entrada e formatos de exemplo na saída de `input_sample` e `output_sample` variáveis que representam os formatos de solicitação e resposta para o serviço web. Usar esses exemplos na entrada e saída decoradores de função no `run()` função. Scikit-Saiba o exemplo a seguir usa a geração de esquema.

> [!TIP]
> Depois de implantar o serviço, use o `swagger_uri` propriedade para recuperar o documento de esquema JSON.

#### <a name="example-entry-script"></a>Script de exemplo de entrada

O exemplo a seguir demonstra como aceitar e retornar dados JSON:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType

def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
output_sample = np.array([3726.995])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Exemplo de script com a entrada do dicionário (consumo de suporte do Power BI)

O exemplo a seguir demonstra como definir dados de entrada como < chave: valor > dictionary, usando o Dataframe. Esse método tem suporte para o consumo de serviço web implantado do Power BI ([Saiba mais sobre como consumir o serviço web do Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType

def init():
    global model
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```
Para obter mais exemplos de script, consulte os exemplos a seguir:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Pontuação em relação aos dados binários: [Como consumir um serviço web](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Definir seu InferenceConfig

A configuração de inferência de tipos descreve como configurar o modelo para fazer previsões. O exemplo a seguir demonstra como criar uma configuração de inferência de tipos:

```python
inference_config = InferenceConfig(source_directory="C:/abc",
                                   runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Neste exemplo, a configuração contém os seguintes itens:

* Um diretório que contém os ativos necessários para inferência de tipos
* Esse modelo requer o Python
* O [script de entrada](#script), que é usado para manipular solicitações da web enviadas para o serviço implantado
* O arquivo conda que descreve os pacotes Python necessários para inferência de tipos

Para obter informações sobre a funcionalidade de InferenceConfig, consulte o [configuração avançada](#advanced-config) seção.

### <a name="3-define-your-deployment-configuration"></a>3. Definir sua configuração de implantação

Antes de implantar, você deve definir a configuração de implantação. A configuração de implantação é específica para o destino de computação que irá hospedar o serviço web. Por exemplo, ao implantar localmente deve especificar a porta em que o serviço aceita solicitações.

Você também precisará criar o recurso de computação. Por exemplo, se você fizer isso ainda não tiver um serviço de Kubernetes do Azure associado com seu espaço de trabalho.

A tabela a seguir fornece um exemplo de criação de uma configuração de implantação para cada destino de computação:

| Destino de computação | Exemplo de configuração de implantação |
| ----- | ----- |
| Local | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instance | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Serviço de Kubernetes do Azure | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

As seções a seguir demonstram como criar a configuração de implantação e, em seguida, usá-lo para implantar o serviço web.

## <a name="deploy-to-target"></a>Implantar no destino

### <a id="local"></a> Implantação local

Para implantar localmente, você precisa ter **Docker instalado** em seu computador local.

+ **Usando o SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

### <a id="aci"></a> Instâncias de contêiner do Azure (desenvolvimento/teste)

Use as Instâncias de Contêiner do Azure para implantar os modelos como um serviço Web, se uma ou mais das seguintes condições forem verdadeiras:
- Você precisa implantar rapidamente e validar o modelo.
- Você está testando um modelo ainda em desenvolvimento. 

Para ver a disponibilidade de região e de cota do ACI, consulte o [cotas e disponibilidade de região para instâncias de contêiner do Azure](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) artigo.

+ **Usando o SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```


+ **Usando o VS Code**

  Para [implantar seus modelos com o VS Code](how-to-vscode-tools.md#deploy-and-manage-models) você não precisa criar um contêiner ACI para testar com antecedência, porque os contêineres ACI são criados em tempo real.

Para obter mais informações, consulte a documentação de referência para as classes [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) e [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a>Serviço Kubernetes do Azure (desenvolvimento/teste e produção)

Você pode usar um cluster AKS existente ou criar outro usando o SDK do Azure Machine Learning, a CLI ou o portal do Azure.

<a id="deploy-aks"></a>

Se você já tiver um cluster do AKS anexado, você poderá implantar nele. Se você ainda não tiver criado ou anexado a um cluster do AKS, siga o processo para <a href="#create-attach-aks">criar um novo cluster do AKS</a>.

+ **Usando o SDK**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

+ **Usando o VS Code**

  Você também pode [implantar no AKS através da extensão do VS Code](how-to-vscode-tools.md#deploy-and-manage-models), mas você também precisará configurar clusters AKS com antecedência.

Saiba mais sobre implantação do AKS e dimensionamento automático na [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) referência.

#### Criar um novo cluster do AKS<a id="create-attach-aks"></a>
**Tempo estimado:** Aproximadamente 5 minutos.

Criando ou anexando a um cluster do AKS é um momento de um processo para seu espaço de trabalho. Você pode reutilizar esse cluster para várias implantações. Se você excluir o cluster ou o grupo de recursos que o contém, você deve criar um novo cluster na próxima vez que você precisa implantar. Você pode ter vários clusters AKS anexados ao seu espaço de trabalho.

Se você quiser criar um cluster do AKS para desenvolvimento, validação e teste, você definir `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` ao usar [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Um cluster criado com essa configuração só terá um nó.

> [!IMPORTANT]
> Definindo `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` cria um cluster do AKS que não é adequado para tratamento do tráfego de produção. Tempos de inferência de tipos podem ser maiores do que em um cluster criado para a produção. Tolerância a falhas também não é garantida para clusters de desenvolvimento/teste.
>
> É recomendável que os clusters criados para desenvolvimento/teste usam pelo menos duas CPUs virtuais.

O exemplo a seguir demonstra como criar um novo cluster do serviço Kubernetes do Azure:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Para obter mais informações sobre como criar um cluster do AKS fora do SDK do Azure Machine Learning, consulte os seguintes artigos:
* [Criar um cluster AKS](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Criar um cluster do AKS (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Para obter mais informações sobre o `cluster_purpose` parâmetro, consulte a [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referência.

> [!IMPORTANT]
> Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), se você escolher valores personalizados para agent_count e vm_size, será necessário certificar-se de que agent_count multiplicado por vm_size será maior ou igual a 12 CPUs virtuais. Por exemplo, se você usar um vm_size de "Standard_D3_v2", que tenha 4 CPUs virtuais, será necessário escolher um agent_count de 3 ou maior.

**Tempo estimado**: Aproximadamente 20 minutos.

#### <a name="attach-an-existing-aks-cluster"></a>Anexar a um cluster existente do AKS

Se você já tiver o cluster do AKS em sua assinatura do Azure, e é a versão 1.12. # #, você pode usá-lo para implantar sua imagem.

> [!WARNING]
> Ao anexar a um cluster do AKS para um espaço de trabalho, você pode definir como você usará o cluster, definindo o `cluster_purpose` parâmetro.
>
> Se você não definir a `cluster_purpose` parâmetro, ou conjunto `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, em seguida, o cluster deve ter pelo menos 12 CPUs virtuais disponíveis.
>
> Se você definir `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, em seguida, o cluster não precisa ter 12 CPUs virtuais. No entanto, um cluster que está configurado para desenvolvimento/teste não será adequado para o nível de tráfego de produção e pode aumentar o tempo de inferência de tipos.

O código a seguir demonstra como anexar um 1.12 existente do AKS. # # cluster ao espaço de trabalho:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Para obter mais informações sobre `attack_configuration()`, consulte o [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) referência.

Para obter mais informações sobre o `cluster_purpose` parâmetro, consulte a [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referência.

## <a name="consume-web-services"></a>Consumir serviços Web

Cada serviço web implantado fornece uma API REST, portanto, você pode criar aplicativos de cliente em uma variedade de linguagens de programação. Se você tiver habilitado a autenticação para seu serviço, você precisa fornecer uma chave de serviço como um token em seu cabeçalho de solicitação.

### <a name="request-response-consumption"></a>Consumo de solicitação-resposta

Aqui está um exemplo de como chamar o serviço no Python:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Para obter mais informações, confira [Criar aplicativos cliente para consumir serviços Web](how-to-consume-web-service.md).


### <a id="azuremlcompute"></a> Inferência de tipos de lote
Destinos de computação do Machine Learning do Azure são criados e gerenciados pelo serviço de Azure Machine Learning. Eles podem ser usados para previsão de lote de Pipelines de Machine Learning do Azure.

Para obter uma explicação de inferência de lote com a computação do Azure Machine Learning, leia as [como executar previsões de lote](how-to-run-batch-predictions.md) artigo.

### <a id="iotedge"></a> Inferência de tipos do IoT Edge
Suporte à implantação para a borda está em visualização. Para obter mais informações, consulte o [implantar o Azure Machine Learning como um módulo IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) artigo.


## <a id="update"></a> Atualize o web services

Quando você cria um novo modelo, você deve atualizar manualmente cada serviço que você deseja usar o novo modelo. Para atualizar o serviço Web, use o método `update`. O código a seguir demonstra como atualizar o serviço web para usar um novo modelo:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

<a id="advanced-config"></a>

## <a name="advanced-settings"></a>Configurações avançadas 

**<a id="customimage"></a> Usar uma imagem de base personalizada**

Internamente, InferenceConfig cria uma imagem do Docker que contém o modelo e outros ativos necessários para o serviço. Se não for especificado, uma imagem de base padrão é usada.

Ao criar uma imagem para usar com a sua configuração de inferência de tipos, a imagem deve atender aos seguintes requisitos:

* Ubuntu 16.04 ou posterior.
* 4.5 Conda. # ou maior.
* Python 3.5. # ou 3.6. #.

Para usar uma imagem personalizada, defina o `base_image` propriedade de inferência configuração para o endereço da imagem. O exemplo a seguir demonstra como usar uma imagem de um registro de contêiner do Azure públicas e privadas:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"

# or, use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

A imagem a seguir URIs são para imagens fornecidas pela Microsoft e pode ser usado sem fornecer um valor de nome ou a senha do usuário:

* `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7`
* `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03`

Para usar essas imagens, defina o `base_image` para o URI da lista acima. Defina `base_image_registry.address` como `mcr.microsoft.com`.

> [!IMPORTANT]
> Imagens da Microsoft que usam o CUDA ou o TensorRT devem ser usadas apenas nos serviços do Microsoft Azure.

Para obter mais informações sobre como carregar suas próprias imagens para um registro de contêiner do Azure, consulte [envie sua primeira imagem para um registro de contêiner do Docker privado](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).

Se seu modelo é treinado em computação do Azure Machine Learning, usando __versão 1.0.22 ou maior__ do SDK do Azure Machine Learning, uma imagem é criada durante o treinamento. O exemplo a seguir demonstra como usar esta imagem:

```python
# Use an image built during training with SDK 1.0.22 or greater
image_config.base_image = run.properties["AzureML.DerivedImageName"]
```

## <a name="clean-up-resources"></a>Limpar recursos
Para excluir um serviço Web implantado, use `service.delete()`.
Para excluir um modelo registrado, use `model.delete()`.

Para obter mais informações, consulte a documentação de referência [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), e [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Próximas etapas
* [Solução de problemas de implantação](how-to-troubleshoot-deployment.md)
* [Proteger serviços Web do Azure Machine Learning com SSL](how-to-secure-web-service.md)
* [Consumir um modelo de ML implantado como um serviço Web](how-to-consume-web-service.md)
* [Monitorar seus modelos do Azure Machine Learning com o Application Insights](how-to-enable-app-insights.md)
* [Coletar dados para modelos em produção](how-to-enable-data-collection.md)

