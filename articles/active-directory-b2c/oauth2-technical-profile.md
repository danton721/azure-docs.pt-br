---
title: Definir um perfil de técnico de OAuth2 em uma política personalizada no Azure Active Directory B2C | Microsoft Docs
description: Defina um perfil de técnico de OAuth2 em uma política personalizada no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 80b196b34e8eee99ed77c3c8a914f89fa68d87b8
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512948"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definir um perfil de técnico de OAuth2 em uma política personalizada do Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O Azure AD (Azure Active Directory) B2C oferece suporte para o provedor de identidade do protocolo OAuth2. OAuth2 é o principal protocolo de autorização e autenticação delegada. Para obter mais informações, confira [RFC 6749 O Framework de Autorização OAuth 2.0](https://tools.ietf.org/html/rfc6749). Com um perfil técnico de OAuth2, você pode federar com um provedor de identidade baseada no OAuth2, como o Facebook. Federação com um provedor de identidade permite que os usuários entram com suas redes sociais existentes ou identidades corporativas.

## <a name="protocol"></a>Protocol

O atributo **Name** do elemento **Protocol** precisa ser definido como `OAuth2`. Por exemplo, o protocolo para o perfil técnico **Facebook-OAUTH** é `OAuth2`:

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...    
```

## <a name="input-claims"></a>Declarações de entrada

Os elementos **InputClaims** e **InputClaimsTransformations** não são necessários. Mas talvez você queira enviar parâmetros adicionais para seu provedor de identidade. O exemplo a seguir adiciona o parâmetro de cadeia de caracteres de consulta **domain_hint** com o valor de `contoso.com` à solicitação de autorização.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>Declarações de saída

O elemento **OutputClaims** contém uma lista de declarações retornadas pelo provedor de identidade OAuth2. Talvez seja necessário mapear o nome da declaração definida em sua política para o nome definido no provedor de identidade. Você também pode incluir declarações que não são retornadas pelo provedor de identidade, desde que você defina o atributo `DefaultValue`.

O elemento **OutputClaimsTransformations** pode conter uma coleção de elementos **OutputClaimsTransformation** usados para modificar as declarações de saída ou gerar novas declarações.

O exemplo a seguir mostra as declarações retornadas pelo provedor de identidade do Facebook:

- A declaração **first_name** é mapeada para a declaração **givenName**.
- A declaração **last_name** é mapeada para a declaração **surname**.
- O **displayName** sem mapeamento de nome de declaração.
- A declaração **email** sem mapeamento de nome.

O perfil técnico também retorna declarações que não são retornadas pelo provedor de identidade: 

- A declaração **identityProvider** que contém o nome do provedor de identidade.
- A declaração **authenticationSource** com um valor padrão de **socialIdpAuthentication**.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metadados

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| client_id | Sim | O identificador do aplicativo do provedor de identidade. |
| IdTokenAudience | Não | O público-alvo do id_token. Se for especificado, o Azure AD B2C verificará se o token está em uma declaração retornada pelo provedor de identidade e é igual ao especificado. |
| authorization_endpoint | Sim | A URL do ponto de extremidade da autorização, de acordo com RFC 6749. |
| AccessTokenEndpoint | Sim | A URL do ponto de extremidade do token, de acordo com RFC 6749. |  
| ClaimsEndpoint | Sim | A URL do ponto de extremidade de informações do usuário, de acordo com RFC 6749. | 
| AccessTokenResponseFormat | Não | O formato da chamada de ponto de extremidade do token de acesso. Por exemplo, o Facebook requer um método HTTP GET, mas a resposta do token de acesso está no formato JSON. |
| AdditionalRequestQueryParameters | Não | Parâmetros de consulta de solicitação adicionais. Por exemplo, talvez você queira enviar parâmetros adicionais para seu provedor de identidade. Você pode incluir vários parâmetros usando o delimitador de vírgula. | 
| ClaimsEndpointAccessTokenName | Não | O nome do que o parâmetro de cadeia de caracteres de consulta do token de acesso. Os pontos de extremidade de declaração de alguns provedores de identidade dão suporte a solicitação HTTP GET. Nesse caso, o token de portador é enviado usando um parâmetro de cadeia de caracteres de consulta em vez do cabeçalho de autorização. |
| ClaimsEndpointFormatName | Não | O nome do que o parâmetro de cadeia de caracteres de consulta do formato. Por exemplo, você pode definir o nome como `format` neste ponto de extremidade de declarações do LinkedIn `https://api.linkedin.com/v1/people/~?format=json`. | 
| ClaimsEndpointFormat | Não | O valor do que o parâmetro de cadeia de caracteres de consulta do formato. Por exemplo, você pode definir o valor como `json` neste ponto de extremidade de declarações do LinkedIn `https://api.linkedin.com/v1/people/~?format=json`. | 
| ProviderName | Não | O nome do provedor de identidade. |
| response_mode | Não | O método que o provedor de identidade usa para enviar o resultado de volta ao Azure AD B2C. Valores possíveis: `query`, `form_post` (padrão) ou `fragment`. |
| scope | Não | O escopo da solicitação que é definido de acordo com a especificação do provedor de identidade OAuth2. Como `openid`, `profile` e `email`. |
| HttpBinding | Não | A associação HTTP esperada para o token de acesso e pontos de extremidade do token de declarações. Os valores possíveis são `GET` ou `POST`.  |
| ResponseErrorCodeParamName | Não | O nome do parâmetro que contém a mensagem de erro retornada por HTTP 200 (OK). |
| ExtraParamsInAccessTokenEndpointResponse | Não | Contém os parâmetros extra que podem ser retornados na resposta de **AccessTokenEndpoint** por alguns provedores de identidade. Por exemplo, a resposta de **AccessTokenEndpoint** contém um parâmetro extra, como `openid`, que é um parâmetro obrigatório, além de access_token em uma cadeia de caracteres de consulta de solicitação **ClaimsEndpoint**. Vários nomes de parâmetro devem ter um escape e ser separados pelo delimitador de vírgula ','. |
| ExtraParamsInClaimsEndpointRequest | Não | Contém os parâmetros extra que podem ser retornados na solicitação **ClaimsEndpoint** por alguns provedores de identidade. Vários nomes de parâmetro devem ter um escape e ser separados pelo delimitador de vírgula ','. |

## <a name="cryptographic-keys"></a>Chaves de criptografia

O elemento **CryptographicKeys** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| client_secret | Sim | O segredo do cliente do aplicativo do provedor de identidade. A chave de criptografia será necessária apenas se os metadados **response_types** estiverem definidos como `code`. Nesse caso, o Azure AD B2C faz outra chamada para trocar o código de autorização para um token de acesso. Se os metadados são definidos `id_token`, você pode omitir a chave de criptografia. |  

## <a name="redirect-uri"></a>URI de redirecionamento

Ao configurar a URL de redirecionamento do seu provedor de identidade, insira `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`. Certifique-se de substituir **tenant** pelo nome do locatário (por exemplo, contosob2c.onmicrosoft.com) e **policyId** pelo identificador da sua política (por exemplo, b2c_1a_policy). O URI de redirecionamento deve ser todo em letras minúsculas.

Se você estiver usando o domínio **b2clogin.com** em vez de **login.microsoftonline.com**, use b2clogin.com, em vez de login.microsoftonline.com.

Exemplos:

- [Adicionar Google+ como um provedor de identidade OAuth2 usando políticas personalizadas](active-directory-b2c-custom-setup-goog-idp.md)













