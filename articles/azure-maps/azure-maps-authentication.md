---
title: Autenticação com os Azure Mapas | Microsoft Docs
description: Autenticação para uso dos serviços dos Azure Mapas.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 22aba19e16e4349a5b495b307c9906f7ded5a636
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393670"
---
# <a name="authentication-with-azure-maps"></a>Autenticação com Azure Mapas

Os Azure Mapas dão suporte a duas maneiras de autenticar solicitações: Chave compartilhada e Azure AD (Azure Active Directory). Este artigo explica esses métodos de autenticação para ajudar a orientar sua implementação.

## <a name="shared-key-authentication"></a>Autenticação de Chave Compartilhada

A autenticação de Chave compartilhada passa as chaves geradas pela conta dos Azure Mapas com cada solicitação aos Azure Mapas.  Duas chaves são geradas quando sua conta dos Azure Mapas é criada. Cada solicitação aos serviços dos Azure Mapas exige que a chave de assinatura seja adicionada como um parâmetro à URL.

> [!Tip]
> Recomendamos a regeneração frequente das chaves. Você recebe duas chaves, de modo que possa manter conexões usando uma chave enquanto regenera a outra. Ao regenerar suas chaves, você precisará atualizar os aplicativos que acessam essa conta para que usem as novas chaves.

Para obter informações sobre como exibir suas chaves, consulte [Exibir detalhes de autenticação](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Autenticação com o Azure Active Directory (versão prévia)

Os Azure Mapas agora oferecem a integração do [Azure AD (Azure Active Directory)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) para a autenticação de solicitações de serviços dos Azure Mapas. O Azure AD fornece autenticação baseada em identificação, incluindo o [RBAC (controle de acesso baseado em função)](https://docs.microsoft.com/azure/role-based-access-control/overview) para permitir acesso no nível do usuário ou do aplicativo aos recursos dos Azure Mapas. As seções a seguir podem ajudá-lo a entender os conceitos e os componentes da integração entre o Azure AD e os Azure Mapas.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticação com tokens de acesso OAuth

Os Azure Mapas aceitam tokens de acesso **OAuth 2.0** para locatários do Azure AD associados à assinatura do Azure que contêm uma conta dos Azure Mapas. Os Azure Mapas aceitam tokens para:

* Usuários do Azure AD. 
* Aplicativos do parceiro que usam permissões delegadas pelos usuários.
* Identidades gerenciadas para recursos do Azure.

Azure Mapas gera um *identificador exclusivo (ID do cliente)* para cada conta do Azure Mapas. Quando você combina essa ID de cliente com parâmetros adicionais, você pode solicitar tokens do Azure AD, especificando o seguinte valor:

```
https://login.microsoftonline.com
```
Para obter mais informações sobre como configurar o Azure AD e solicitar tokens para os Azure Mapas, confira [Como gerenciar a autenticação nos Azure Mapas](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Para obter informações gerais sobre como solicitar tokens do Azure AD, consulte [O que é autenticação?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Solicite recursos dos Azure Mapas com tokens OAuth

Depois que um token é recebido do Azure AD, uma solicitação pode ser enviada para os Azure Mapas com os dois seguintes conjuntos de cabeçalhos de solicitação obrigatórios:

| Cabeçalho da solicitação    |    Value    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorização     | Portador eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` é o GUID baseado na conta dos Azure Mapas exibido na página de autenticação dos Azure Mapas.

Aqui está um exemplo de uma solicitação de rota do Azure Mapas que usa um token OAuth:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Para obter informações sobre como exibir a ID do cliente, consulte [Exibir detalhes de autenticação](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Controlar o acesso com RBAC

O Azure AD permite controlar o acesso a recursos protegidos por meio de RBAC. Depois que a sua conta dos Azure Mapas for criada e você registrar o aplicativo do Azure AD e do Azure Mapas em seu locatário do Azure AD, você poderá configurar o RBAC para um usuário, um aplicativo ou um recurso do Azure na página do portal de contas dos Azure Mapas.

Os Azure Mapas dão suporte ao controle de acesso de leitura para usuários individuais do Azure AD, aplicativos ou serviços do Azure por meio de identidades gerenciadas para recursos do Azure.

![Leitor de dados do Azure Mapas (versão prévia)](./media/azure-maps-authentication/concept.png)

Para obter informações sobre como exibir as configurações do RBAC, confira [Como configurar o RBAC para os Azure Mapas](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identidades gerenciadas para recursos do Azure e os Azure Mapas

As [identidades gerenciadas para recursos do Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) fornecem serviços do Azure (Serviço de Aplicativo do Azure, Azure Functions, Máquinas Virtuais etc.) com uma identidade gerenciada automaticamente que pode ser autorizada para acesso aos serviços dos Azure Mapas.  

## <a name="next-steps"></a>Próximas etapas

* Para saber mais sobre como autenticar um aplicativo com o Azure AD e os Azure Mapas, confira [Como gerenciar a autenticação no Azure Mapas](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Para saber mais sobre como autenticar os Azure Mapas, o Controle de Mapeamento e o Azure AD, confira [Use o Controle de Mapeamento dos Azure Mapas](https://aka.ms/amaadmc).