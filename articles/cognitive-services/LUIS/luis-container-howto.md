---
title: Contêineres do Docker
titleSuffix: Language Understanding - Azure Cognitive Services
description: O contêiner do LUIS carrega seu aplicativo treinado ou publicado para um contêiner do Docker e fornece acesso às previsões de consulta dos pontos de extremidade da API do contêiner.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: a6170d51e1a8756020b4f2caa733c388b2ce4060
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53013809"
---
# <a name="install-and-run-containers"></a>Instalar e executar contêineres
 
O contêiner do Serviço Inteligente de Reconhecimento Vocal (LUIS) carrega o modelo de reconhecimento vocal treinado ou publicado, também conhecido como um [aplicativo LUIS](https://www.luis.ai), em um contêiner do Docker e fornece acesso às previsões de consulta dos pontos de extremidade da API do contêiner. Você pode coletar logs de consulta do contêiner e carregá-los novamente no modelo do Serviço Inteligente de Reconhecimento Vocal do Azure para aumentar a precisão da previsão do aplicativo.

O vídeo a seguir demonstra como usar esse contêiner.

[![Demonstração de contêiner para os Serviços Cognitivos](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo)

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para executar o contêiner do LUIS, você precisará ter o seguinte: 

|Obrigatório|Finalidade|
|--|--|
|Mecanismo Docker| Para concluir esta versão prévia, você precisará ter um mecanismo Docker instalado em um [computador host](#the-host-computer). O Docker fornece pacotes que configuram o ambiente do Docker no [macOS](https://docs.docker.com/docker-for-mac/), no [Windows](https://docs.docker.com/docker-for-windows/) e no [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para instruções sobre conceitos básicos do Docker e de contêiner, consulte a [visão geral do Docker](https://docs.docker.com/engine/docker-overview/).<br><br> O Docker deve ser configurado para permitir que os contêineres conectem-se e enviem dados de cobrança para o Azure. <br><br> **No Windows**, o Docker também deve ser configurado para dar suporte a contêineres do Linux.<br><br>|
|Familiaridade com o Docker | É necessário ter uma compreensão básica de conceitos do Docker, como registros, repositórios, contêineres e imagens de contêiner, bem como conhecimento dos comandos básicos do `docker`.| 
|Recurso de Reconhecimento Vocal (LUIS) e aplicativo associado |Para usar o contêiner, você precisará ter:<br><br>* Um [recurso do Azure de _Reconhecimento Vocal_](luis-how-to-azure-subscription.md), junto com a chave do ponto de extremidade associada e o URI do ponto de extremidade (usado como o ponto de extremidade de cobrança).<br>* Um aplicativo publicado ou treinado empacotado como uma entrada montada para o contêiner com sua ID do aplicativo associado.<br>* A Chave de Criação para baixar o pacote do aplicativo, se você estiver fazendo isso a partir da API.<br><br>Esses requisitos são usados para transmitir argumentos de linha de comando para as seguintes variáveis:<br><br>**{AUTHORING_KEY}** : Essa chave é usada para obter o aplicativo empacotado do serviço de LUIS na nuvem e carregar os logs de consulta novamente na nuvem. O formato é `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APPLICATION_ID}** : Essa ID é usada para selecionar o aplicativo. O formato é `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{ENDPOINT_KEY}** : Essa chave é usada para iniciar o contêiner. Você pode encontrar a chave do ponto de extremidade em dois locais. O primeiro é o portal do Azure na lista de chaves do recurso _Reconhecimento Vocal_. A chave do ponto de extremidade também está disponível no portal do LUIS na página Configurações de chaves e de ponto de extremidade. Não use a chave de inicialização.<br><br>**{BILLING_ENDPOINT}** : O valor do ponto de extremidade de cobrança está disponível na página Visão Geral do Serviço Inteligente de Reconhecimento Vocal do portal do Azure. Um exemplo é: `https://westus.api.cognitive.microsoft.com/luis/v2.0`.<br><br>A [chave de criação e a chave do ponto de extremidade](luis-boundaries.md#key-limits) têm finalidades diferentes. Não use uma no lugar da outra. |

### <a name="the-host-computer"></a>O computador host

O **host** é o computador que executa o contêiner do Docker. Ele pode ser um computador local ou um serviço de hospedagem do Docker no Azure, incluindo:

* [Serviço de Kubernetes do Azure](/azure/aks/)
* [Instâncias de Contêiner do Azure](/azure/container-instances/)
* Cluster [Kubernetes](https://kubernetes.io/) implantado no [Azure Stack](/azure/azure-stack/). Para obter mais informações, consulte [Implantar Kubernetes no Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

### <a name="container-requirements-and-recommendations"></a>Recomendações e requisitos do contêiner

Esse contêiner dá suporte aos valores mínimos e recomendados para as configurações:

|Configuração| Mínimo | Recomendadas |
|-----------|---------|-------------|
|Núcleos<BR>`--cpus`|Um núcleo<BR>pelo menos 2,6 GHz (gigahertz) ou mais rápido|Um núcleo|
|Memória<BR>`--memory`|2 GB|4 GB|
|Transações por segundo<BR>(TPS)|20 TPS|40 TPS|

As configurações `--cpus` e `--memory` são usadas como parte do comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obter a imagem de contêiner com `docker pull`

Use o comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para fazer download de uma imagem de contêiner do repositório `mcr.microsoft.com/azure-cognitive-services/luis`:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Para obter uma descrição completa das marcas disponíveis, como as `latest` usadas no comando anterior, confira [LUIS](https://hub.docker.com/r/microsoft/azure-cognitive-services-luis/) no Hub do Docker.

> [!TIP]
> Você pode usar o comando [imagens do estivador](https://docs.docker.com/engine/reference/commandline/images/) para listar as imagens do contêiner transferidas por download. Por exemplo, o comando a seguir lista o ID, o repositório e a tag de cada imagem do contêiner transferida por download, formatada como uma tabela:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/luis                           latest
>  ``` 

## <a name="how-to-use-the-container"></a>Como usar o contêiner

Depois que o contêiner estiver no [computador host](#the-host-computer), use o processo a seguir para trabalhar com o contêiner.

![Processo para usar o contêiner do Serviço Inteligente de Reconhecimento Vocal (LUIS)](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. [Exporte o pacote](#export-packaged-app-from-luis) para o contêiner no portal do LUIS ou nas APIs de LUIS.
1. Mova o arquivo de pacote para o diretório de **entrada** exigido no [computador host](#the-host-computer). Não renomeie, altere ou descompacte o arquivo de pacote do LUIS.
1. [Execute o contêiner](##run-the-container-with-docker-run), com a _montagem de entrada_ e as configurações de cobrança necessárias. Há outros [exemplos](luis-container-configuration.md#example-docker-run-commands) do comando `docker run` disponíveis. 
1. [Consultar o ponto de extremidade de previsão do contêiner](#query-the-containers-prediction-endpoint). 
1. Quando você terminar de usar o contêiner [importe os logs do ponto de extremidade](#import-the-endpoint-logs-for-active-learning) da montagem de saída no portal do LUIS e [pare](#stop-the-container) o contêiner.
1. Use o [aprendizado ativo](luis-how-to-review-endoint-utt.md) do portal do LUIS na página **Examinar declarações do ponto de extremidade** para aprimorar o aplicativo.

O aplicativo em execução no contêiner não pode ser alterado. Para alterar o aplicativo no contêiner, você precisará alterar o aplicativo no serviço de LUIS usando o portal do [LUIS](https://www.luis.ai) ou usando as [APIs de criação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) do LUIS. Treine e/ou publique e, em seguida, baixe um novo pacote e execute novamente o contêiner.

O aplicativo LUIS dentro do contêiner não pode ser exportado novamente para o serviço de LUIS. Somente os logs de consulta podem ser carregados. 

## <a name="export-packaged-app-from-luis"></a>Exportar aplicativo empacotado do LUIS

O contêiner de LUIS requer um aplicativo LUIS treinado ou publicado para responder a consultas de previsão das declarações dos usuários. Para obter o aplicativo LUIS, use a API do pacote publicado ou treinado. 

O local padrão é o subdiretório `input` do local em que você executa o comando `docker run`.  

Coloque o arquivo de pacote em um diretório e faça referência a esse diretório como a montagem de entrada quando você executar o contêiner do Docker. 

### <a name="package-types"></a>Tipos de pacote

O diretório de montagem de entrada pode conter as versões de **Produção**, **Preparo** e **Treinada** do aplicativo simultaneamente. Todos os pacotes são montados. 

|Tipo de pacote|API do ponto de extremidade de consulta|Disponibilidade de consulta|Formato do nome de arquivo do pacote|
|--|--|--|--|
|Treinado|Get, Post|Somente contêiner|`{APPLICATION_ID}_v{APPLICATION_VERSION}.gz`|
|Staging|Get, Post|Azure e contêiner|`{APPLICATION_ID}_STAGING.gz`|
|Produção|Get, Post|Azure e contêiner|`{APPLICATION_ID}_PRODUCTION.gz`|

>**Importante:** Não renomeie, altere ou descompacte os arquivos de pacote do LUIS.

### <a name="packaging-prerequisites"></a>Pré-requisitos de empacotamento

Antes de empacotar um aplicativo LUIS, você deverá ter o seguinte:

|Requisitos de empacotamento|Detalhes|
|--|--|
|Instância do recurso _Reconhecimento Vocal_ do Azure|As regiões com suporte incluem<br><br>Oeste dos EUA (```westus```)<br>Europa Ocidental (```westeurope```)<br>Leste da Austrália (```australiaeast```)|
|Aplicativo LUIS treinado ou publicado|Sem [dependências sem suporte](#unsupported-dependencies). |
|Acesso ao sistema de arquivos do [computador host](#the-host-computer) |O computador host deve permitir uma [montagem de entrada](luis-container-configuration.md#mount-settings).|
  
### <a name="export-app-package-from-luis-portal"></a>Exportar pacote de aplicativo do portal do LUIS

O [portal](https://www.luis.ai) do LUIS permite exportar o pacote do aplicativo publicado ou treinado. 

### <a name="export-published-apps-package-from-luis-portal"></a>Exportar o pacote do aplicativo publicado do portal do LUIS

O pacote do aplicativo publicado está disponível na página da lista **Meus aplicativos**. 

1. Entre no portal do [LUIS](https://www.luis.ai).
1. Marque a caixa de seleção à esquerda do nome do aplicativo na lista. 
1. Selecione o item **Exportar** na barra de ferramentas contextual acima da lista.
1. Selecione **Exportar para o contêiner (GZIP)**.
1. Selecione o ambiente do **Slot de produção** ou do **Slot de preparo**.
1. O download do pacote é feito do navegador.

![Exportar o pacote publicado para o contêiner a partir do menu Exportar da página do aplicativo](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-trained-apps-package-from-luis-portal"></a>Exportar o pacote do aplicativo treinado do portal do LUIS

O pacote do aplicativo treinado está disponível na página da lista **Versões**. 

1. Entre no portal do [LUIS](https://www.luis.ai).
1. Selecione o aplicativo na lista. 
1. Selecione **Gerenciar** na barra de navegação do aplicativo.
1. Selecione **Versões** na barra de navegação à esquerda.
1. Marque a caixa de seleção à esquerda do nome da versão na lista.
1. Selecione o item **Exportar** na barra de ferramentas contextual acima da lista.
1. Selecione **Exportar para o contêiner (GZIP)**.
1. O download do pacote é feito do navegador.

![Exportar o pacote treinado para o contêiner a partir do menu Exportar da página Versões](./media/luis-container-how-to/export-trained-package-for-container.png)


### <a name="export-published-apps-package-from-api"></a>Exportar o pacote do aplicativo publicado da API

Use o método de API REST a seguir para empacotar um aplicativo LUIS que você já [publicou](luis-how-to-publish-app.md). Substitua os espaços reservados por seus próprios valores apropriados na chamada à API, usando a tabela abaixo da especificação de HTTP.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Placeholder | Valor |
|-------------|-------|
|{APPLICATION_ID} | A ID de aplicativo do aplicativo LUIS publicado. |
|{APPLICATION_ENVIRONMENT} | O ambiente do aplicativo LUIS publicado. Use um dos seguintes valores:<br/>```PRODUCTION```<br/>```STAGING``` |
|{AUTHORING_KEY} | A chave de criação da conta do LUIS para o aplicativo LUIS publicado.<br/>Você pode obter sua chave de criação na página **Configurações do Usuário** no portal do LUIS. |
|{AZURE_REGION} | A região do Azure apropriada:<br/><br/>```westus``` – Oeste dos EUA<br/>```westeurope``` – Europa Ocidental<br/>```australiaeast``` – Leste da Austrália |

Use o comando CURL abaixo para baixar o pacote publicado, substituindo por seus próprios valores:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_{APPLICATION_ENVIRONMENT}.gz
```

Se for bem-sucedido, a resposta será um arquivo de pacote do LUIS. Salve o arquivo no local de armazenamento especificado para a montagem de entrada do contêiner. 

### <a name="export-trained-apps-package-from-api"></a>Exportar o pacote do aplicativo treinado da API

Use o método de API REST a seguir para empacotar um aplicativo LUIS que você já [treinou](luis-how-to-train.md). Substitua os espaços reservados por seus próprios valores apropriados na chamada à API, usando a tabela abaixo da especificação de HTTP.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Placeholder | Valor |
|-------------|-------|
|{APPLICATION_ID} | A ID de aplicativo do aplicativo LUIS treinado. |
|{APPLICATION_VERSION} | A versão de aplicativo do aplicativo LUIS treinado. |
|{AUTHORING_KEY} | A chave de criação da conta do LUIS para o aplicativo LUIS publicado.<br/>Você pode obter sua chave de criação na página **Configurações do Usuário** no portal do LUIS.  |
|{AZURE_REGION} | A região do Azure apropriada:<br/><br/>```westus``` – Oeste dos EUA<br/>```westeurope``` – Europa Ocidental<br/>```australiaeast``` – Leste da Austrália |

Use o seguinte comando CURL para baixar o pacote treinado:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_v{APPLICATION_VERSION}.gz
```

Se for bem-sucedido, a resposta será um arquivo de pacote do LUIS. Salve o arquivo no local de armazenamento especificado para a montagem de entrada do contêiner. 

## <a name="run-the-container-with-docker-run"></a>Executar o contêiner com `docker run`

Use o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para executar o contêiner. O comando usa os seguintes parâmetros:

| Placeholder | Valor |
|-------------|-------|
|{ENDPOINT_KEY} | Essa chave é usada para iniciar o contêiner. Não use a chave de inicialização. |
|{BILLING_ENDPOINT} | O valor do ponto de extremidade de cobrança está disponível na página Visão Geral do Serviço Inteligente de Reconhecimento Vocal do portal do Azure.|

Substitua esses parâmetros por seus próprios valores no comando `docker run` de exemplo a seguir.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> O comando anterior usa o diretório fora da unidade `c:` para evitar conflitos de permissão no Windows. Se você precisar usar um diretório específico como o diretório de entrada, talvez seja necessário conceder ao Docker permissão de serviço. O comando Docker anterior usa a barra invertida, `\`, como um caractere de continuação de linha. Substitua ou remova a barra invertida com base nos requisitos do sistema operacional de seu [computador host](#the-host-computer). Não altere a ordem dos argumentos, a menos que você esteja familiarizado com contêineres do Docker.


Esse comando:

* Executa um contêiner da imagem de contêiner do LUIS
* Carrega o aplicativo LUIS da montagem de entrada em c:\input, localizada no host do contêiner
* Aloca dois núcleos de CPU e 4 GB (gigabytes) de memória
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner
* Salva os logs de LUIS e contêiner para gerar a montagem de saída em c:\output, localizada no host do contêiner
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host. 

Há outros [exemplos](luis-container-configuration.md#example-docker-run-commands) do comando `docker run` disponíveis. 

> [!IMPORTANT]
> As opções `Eula`, `Billing` e `ApiKey` devem ser especificadas para executar o contêiner; caso contrário, o contêiner não será iniciado.  Para mais informações, consulte [Faturamento](#billing).
> O valor de ApiKey é a **Chave** da página Chaves e Pontos de extremidade no portal do LUIS e também está disponível na página de chaves do recurso de Reconhecimento Vocal do Azure.  

## <a name="query-the-containers-prediction-endpoint"></a>Consultar o ponto de extremidade de previsão do contêiner

O contêiner fornece APIs de ponto de extremidade de previsão de consulta com base em REST. Os pontos de extremidade para aplicativos publicados (preparo ou produção) têm uma rota _diferente_ da dos pontos de extremidade para aplicativos treinados. 

Use o host, https://localhost:5000, para APIs de contêiner. 

|Tipo de pacote|Método|Rota|Parâmetros de consulta|
|--|--|--|--|
|Publicado|[Get](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [Post](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|/luis/v2.0/apps/{appId}?|q={q}<br>&staging<br>[&timezoneOffset]<br>[&verbose]<br>[&log]<br>|
|Treinado|Get, Post|/luis/v2.0/apps/{appId}/versions/{versionId}?|q={q}<br>[&timezoneOffset]<br>[&verbose]<br>[&log]|

Os parâmetros de consulta configuram como e o que é retornado na resposta da consulta:

|Parâmetro de consulta|Tipo|Finalidade|
|--|--|--|
|`q`|string|A declaração do usuário.|
|`timezoneOffset`|número|O timezoneOffset permite [alterar o fuso horário](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) usado pela datetimeV2 predefinida da entidade.|
|`verbose`|booleano|Retorna todas as intenções e suas pontuações quando definido como true. O padrão é false, o que retorna apenas a intenção principal.|
|`staging`|booleano|Retorna a consulta dos resultados do ambiente de preparo quando definido como true. |
|`log`|booleano|Registra as consultas, que podem ser usadas posteriormente no [aprendizado ativo](luis-how-to-review-endoint-utt.md). O padrão é true.|

### <a name="query-published-app"></a>Consultar aplicativo publicado

Um exemplo de comando CURL para consultar o contêiner em relação a um aplicativo publicado é:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Para fazer consultas no ambiente de **Preparo**, altere o valor de parâmetro da cadeia de caracteres de consulta **staging** para true: 

`staging=true`

### <a name="query-trained-app"></a>Consultar aplicativo treinado

Um exemplo de comando CURL para consultar o contêiner em relação a um aplicativo treinado é: 

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}/versions/{APPLICATION_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
O nome da versão tem no máximo 10 caracteres e contém apenas caracteres permitidos em uma URL. 

## <a name="import-the-endpoint-logs-for-active-learning"></a>Importar os logs de ponto de extremidade para aprendizado ativo

Se uma montagem de saída for especificada para o contêiner do LUIS, os arquivos de log de consulta de aplicativo serão salvos no diretório de saída, em que {INSTANCE_ID} é a ID do contêiner. O log de consulta do aplicativo contém a consulta, a resposta e os carimbos de data/hora para cada consulta de previsão enviada para o contêiner de LUIS. 

O local a seguir mostra a estrutura de diretórios aninhados para os arquivos de log do contêiner.
`
/output/luis/{INSTANCE_ID}/
`
 
No portal do LUIS, selecione seu aplicativo e selecione **Importar logs de ponto de extremidade** para carregá-los. 

![Importar arquivos de log do contêiner para aprendizado ativo](./media/luis-container-how-to/upload-endpoint-log-files.png)

Depois que o log é carregado, [examine as declarações do ponto de extremidade](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances) no portal do LUIS.

## <a name="stop-the-container"></a>Parar o contêiner

Para desligar o contêiner, no ambiente de linha de comando em que o contêiner estiver em execução, pressione **Ctrl+C**.

## <a name="troubleshooting"></a>solução de problemas

Se você executar o contêiner com uma [montagem](luis-container-configuration.md#mount-settings) de saída e o registro em log habilitado, o contêiner gerará arquivos de log que são úteis para solucionar problemas que ocorrem durante a inicialização ou execução do contêiner. 

## <a name="containers-api-documentation"></a>Documentação da API do contêiner

O contêiner fornece um conjunto completo de documentação para os pontos de extremidade, bem como um recurso `Try it now`. Esse recurso permite que você insira suas configurações em um formulário HTML baseado na Web e faça a consulta sem precisar escrever nenhum código. Depois que a consulta é retornada, um exemplo de comando CURL é fornecido para demonstrar o formato do corpo e dos cabeçalhos HTTP exigidos. 

> [!TIP]
> Leia a [Especificação de OpenAPI](https://swagger.io/docs/specification/about/), que descreve as operações de API com suporte pelo contêiner, no URI relativo do `/swagger`. Por exemplo: 
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Cobrança

O contêiner do LUIS envia informações de cobrança ao Azure usando um recurso _Reconhecimento Vocal_ em sua conta do Azure. 

Os contêineres dos Serviços Cognitivos não estão licenciados para execução sem estarem conectados ao Azure para medição. Os clientes precisam ativar os contêineres para comunicar informações de cobrança com o serviço de medição em todos os momentos. Os contêineres dos Serviços Cognitivos não enviam dados do cliente (a declaração) para a Microsoft. 

O `docker run` usa os seguintes argumentos para fins de cobrança:

| Opção | DESCRIÇÃO |
|--------|-------------|
| `ApiKey` | A chave de API do recurso _Reconhecimento Vocal_ usada para rastrear informações de cobrança.<br/>O valor dessa opção deve ser definido como uma chave de API para o recurso LUIS do Azure provisionado especificado em `Billing`. |
| `Billing` | O ponto de extremidade do recurso _Reconhecimento Vocal_ usado para rastrear informações de cobrança.<br/>O valor dessa opção deve ser definido como o URI do terminal de um recurso LUIS do Azure provisionado.|
| `Eula` | Indica que você aceitou a licença para o contêiner.<br/>O valor dessa opção deve ser definido como `accept`. |

> [!IMPORTANT]
> Todas as três opções devem ser especificadas com valores válidos ou o contêiner não será iniciado.

Para obter mais informações sobre essas opções, consulte [Configurar contêineres](luis-container-configuration.md).

## <a name="unsupported-dependencies"></a>Dependências sem suporte

Você pode usar um aplicativo LUIS se ele **não inclui** nenhuma das seguintes dependências:

Configurações de aplicativo sem suporte|Detalhes|
|--|--|
|Culturas de contêiner sem suporte| Alemão (de-DE)<br>Holandês (nl-NL)<br>Japonês (ja-JP)<br>|
|Domínios sem suporte|Domínios predefinidos, incluindo as entidades e intenções de domínio predefinidas|
|Entidades sem suporte para todas as culturas|Entidade predefinida [KeyPhrase](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-keyphrase) para todas as culturas|
|Entidades sem suporte para a cultura Inglês (en-US)|Entidades predefinidas [GeographyV2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-geographyv2)|
|Preparação da fala|Não há suporte para dependências externas no contêiner.|
|Análise de sentimento|Não há suporte para dependências externas no contêiner.|
|Verificação Ortográfica do Bing|Não há suporte para dependências externas no contêiner.|

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu conceitos e fluxo de trabalho para baixar, instalar e executar os contêineres do Serviço Inteligente de Reconhecimento Vocal (LUIS). Em resumo:

* O Serviço Inteligente de Reconhecimento Vocal (LUIS) oferece um contêiner do Linux para Docker que fornece previsões de declarações de consultas a pontos de extremidade.
* Imagens de contêiner são baixadas do Registro de Contêiner da Microsoft (MCR).
* Imagens de contêiner são executadas no Docker.
* Você pode usar a API REST para consultar os pontos de extremidade do contêiner especificando o URI de host do contêiner.
* Você deve especificar informações de faturamento ao instanciar um contêiner.

> [!IMPORTANT]
> Os contêineres dos Serviços Cognitivos não estão licenciados para execução sem estarem conectados ao Azure para medição. Os clientes precisam ativar os contêineres para comunicar informações de cobrança com o serviço de medição em todos os momentos. Os contêineres de Serviços Cognitivos não enviam dados do cliente (por exemplo, a imagem ou o texto que está sendo analisado) para a Microsoft.

## <a name="next-steps"></a>Próximas etapas

* Revise [Configurar contêineres](luis-container-configuration.md) para configurações
* Consulte [Perguntas frequentes (FAQ)](luis-resources-faq.md) para resolver problemas relacionados à funcionalidade LUIS.