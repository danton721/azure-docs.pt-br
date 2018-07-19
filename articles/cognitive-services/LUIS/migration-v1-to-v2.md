---
title: Guia de Migração de API de v1 para v2 | Microsoft Docs
titleSuffix: Azure
description: Saiba como migrar para o conjunto de APIs mais recente.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 060baa6578f8a31234a1a667e2d591b92c39a06f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264277"
---
# <a name="api-v2-migration-guide"></a>Guia de migração da API v2
O [ponto de extremidade](https://aka.ms/v1-endpoint-api-docs) versão 1 e as APIs de [criação](https://aka.ms/v1-authoring-api-docs) serão preteridos. Use este guia para entender como migrar para o [ponto de extremidade](https://aka.ms/luis-endpoint-apis) versão 2 e para as APIs de [criação](https://aka.ms/luis-authoring-apis). 

## <a name="new-azure-regions"></a>Novas regiões do Azure
O LUIS tem novas [regiões](https://aka.ms/LUIS-regions) fornecidas para as APIs do LUIS. O LUIS fornece um site diferente para grupos de regiões. O aplicativo deve ser criado na mesma região que você espera consultar. Os aplicativos não migram regiões automaticamente. Exporte o aplicativo de uma região e importe para outra para que ele fique disponível em uma nova região.

## <a name="authoring-route-changes"></a>Criação de alterações de rota
A rota da API de criação mudou de usar a rota **prog** para usar a rota **api**.


| version | rota |
|--|--|
|1|/luis/v1.0/**prog**/apps|
|2|/luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Alterações de rota de ponto de extremidade
A API de ponto de extremidade tem novos parâmetros querystring, bem como uma resposta diferente. Se o sinalizador detalhado for true, todas as intenções, independentemente da pontuação, serão retornadas em uma matriz denominada intenções, além de topScoringIntent.

| version | Rota GET |
|--|--|
|1|/luis/v1/application?ID={appId}&q={q}|
|2|/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]|


Resposta de êxito do ponto de extremidade v1:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

Resposta de êxito do ponto de extremidade v2:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>O gerenciamento de chaves não está mais na API
As APIs da chave de assinatura são preteridas, retornando 410 GONE.

| version | rota |
|--|--|
|1|/luis/v1.0/prog/subscriptions|
|1|/luis/v1.0/prog/subscriptions/{subscriptionKey}|

As [chaves de assinatura](luis-how-to-azure-subscription.md) do Azure são geradas no portal do Azure. Atribua a chave a um aplicativo LUIS na página **[Publicar](manage-keys.md)**. Não é necessário saber o valor real da chave. O LUIS usa o nome da assinatura para fazer a atribuição. 

## <a name="new-versioning-route"></a>Nova rota de controle de versão
Agora o modelo v2 está contido em uma [versão](luis-how-to-manage-versions.md). Um nome de versão tem 10 caracteres na rota. A versão padrão é "0,1".

| version | rota |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities|

## <a name="metadata-renamed"></a>Metadados renomeados
Várias APIs que retornam metadados do LUIS têm novos nomes.

| Nome da rota v1 | Nome da rota v2 |
|--|--|
|PersonalAssistantApps |assistants|
|applicationcultures|cultures|
|applicationdomains|domains|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Exemplo" renomeado para "sugestão"
O LUIS sugere declarações com base em [declarações de ponto de extremidade](label-suggested-utterances.md) existentes que podem aprimorar o modelo. Na versão anterior, isso era chamado de **exemplo**. Na nova versão, o nome é alterado de exemplo para **sugestão**. Isso é chamado de **[Examinar declarações de ponto de extremidade](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** no site do LUIS.

| version | rota |
|--|--|
|1|/luis/v1.0/**prog**/apps/{appId}/entities/{entityId}/**sample**|
|1|/luis/v1.0/**prog**/apps/{appId}/intents/{intentId}/**sample**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/entities/{entityId}/**suggest**|
|2|/luis/**api**/v2.0/apps/{appId}/**versions**/{versionId}/intents/{intentId}/**suggest**|


## <a name="create-app-from-prebuilt-domains"></a>Criar aplicativo de domínios predefinidos
[Domínios predefinidos](luis-how-to-use-prebuilt-domains.md) fornecem um modelo de domínio predefinido. Os domínios predefinidos permitem que você desenvolva rapidamente seu aplicativo LUIS para domínios comuns. Essa API permite que você crie um novo aplicativo com base em um domínio predefinido. A resposta é o novo appID.

|Rota v2|verbo|
|--|--|
|/luis/api/v2.0/apps/customprebuiltdomains  |obter, publicar|
|/luis/api/v2.0/apps/customprebuiltdomains/{culture}  |get|

## <a name="importing-1x-app-into-2x"></a>Importando o aplicativo 1.x para 2.x
O JSON do aplicativo 1.x exportado tem algumas áreas que você precisa alterar antes de importar para o [LUIS][LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Entidades predefinidas 
As [entidades predefinidas](Pre-builtEntities.md) mudaram. Certifique-se de estar usando as entidades predefinidas V2. Isso inclui usar [datetimeV2](pre-builtentities.md?#use-a-prebuilt-datetimev2-entity), em vez de datetime. 

### <a name="actions"></a>Ações
A propriedade de ações não é mais válida. Ela deve ser um vazio 

### <a name="labeled-utterances"></a>Declarações rotuladas
A V1 permitia que declarações rotuladas incluíssem espaços no início ou fim da palavra ou frase. Removeram os espaços. 

## <a name="common-reasons-for-http-response-status-codes"></a>Motivos comuns para códigos de status de resposta HTTP
Confira [LUIS API response codes](luis-reference-response-codes.md) (Códigos de resposta da API do LUIS).

## <a name="next-steps"></a>Próximas etapas

Use a documentação da API v2 atualizar as chamadas REST existentes ao [ponto de extremidade](https://aka.ms/luis-endpoint-apis) do LUIS e às APIs de [criação](https://aka.ms/luis-authoring-apis). 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions