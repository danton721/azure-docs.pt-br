---
title: RelyingParty – Azure Active Directory B2C | Microsoft Docs
description: Especifica o elemento RelyingParty de uma política personalizada no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2b5e9bfe6eaa9b84e259d941760792635a2994f4
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512849"
---
# <a name="relyingparty"></a>RelyingParty

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

O elemento **RelyingParty** especifica o percurso do usuário para reforçar para a solicitação atual para o Azure AD (Azure Active Directory) B2C. Também especifica a lista de declarações de que o aplicativo de RP (terceira parte confiável) precisa como parte do token emitido. Um aplicativo de RP, como um aplicativo Web, móvel ou da área de trabalho, chama o arquivo de política de RP. O arquivo de política de RP executa uma tarefa específica, como entrar, redefinir senha ou editar perfil. Vários aplicativos podem usar a mesma política RP e um único aplicativo pode usar várias políticas. Todos os aplicativos de RP recebem o mesmo token com declarações e o usuário passa pelo mesmo percurso do usuário.

A exemplo a seguir mostra um elemento **RelyingParty** no arquivo de política *B2C_1A_signup_signin*:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="your-tenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://your-tenant.onmicrosoft.com/B2C_1A_signup_signin">

  <BasePolicy>
    <TenantId>your-tenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
      <SingleSignOn Scope="TrustFramework" KeepAliveInDays="7"/>
      <SessionExpiryType>Rolling</SessionExpiryType>
      <SessionExpiryInSeconds>300</SessionExpiryInSeconds>
      <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="your-application-insights-key" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
      </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Description>The policy profile</Description> 
      <Protocol Name="OpenIdConnect" />
      <Metadata>collection of key/value pairs of data</Metadata>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ...
```

O elemento **RelyingParty** opcional contém os seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| DefaultUserJourney | 1:1 | O percurso do usuário padrão para o aplicativo de RP. |
| UserJourneyBehaviors | 0:1 | O escopo dos comportamentos do percurso do usuário. |
| TechnicalProfile | 1:1 | Um perfil técnico é compatível com o aplicativo de RP. O perfil técnico fornece um contrato para o aplicativo de RP contatar o Azure AD B2C. |

## <a name="defaultuserjourney"></a>DefaultUserJourney

O elemento `DefaultUserJourney` especifica uma referência ao identificador do percurso do usuário que normalmente é definido na política de Base ou Extensões. Os exemplos a seguir mostram o percurso do usuário de inscrição ou entrada especificado no elemento **RelyingParty**:

Política *B2C_1A_signup_signin*:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

*B2C_1A_TrustFrameWorkBase* ou *B2C_1A_TrustFrameworkExtensionPolicy*:

```XML
<UserJourneys>
  <UserJourney Id="SignUpOrSignIn">
  ...
```

O elemento **DefaultUserJourney** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| ReferenceId | Sim | Um identificador de percurso do usuário na política. Para obter mais informações, confira [percursos do usuário](userjourneys.md) |

## <a name="userjourneybehaviors"></a>UserJourneyBehaviors

O elemento **UserJourneyBehaviors** contém os seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| SingleSignOn | 0:1 | O escopo do comportamento da sessão de SSO (logon único) de um percurso do usuário. |
| SessionExpiryType |0:1 | O comportamento de autenticação da sessão. Valores possíveis: `Rolling` ou `Absolute`. O valor `Rolling` (padrão) indica que o usuário permanece conectado desde que o usuário esteja ativo continuamente no aplicativo. O valor `Absolute` indica que o usuário será forçado a autenticar-se novamente após o período especificado por um tempo de vida de sessão de aplicativo. |
| SessionExpiryInSeconds | 0:1 | O tempo de vida do cookie da sessão do Azure AD B2C especificado como um inteiro armazenado no navegador do usuário mediante uma autenticação bem-sucedida. |
| JourneyInsights | 0:1 | A chave de instrumentação do Azure Application Insights a ser usada. |
| ContentDefinitionParameters | 0:1 | A lista de pares chave-valor a serem acrescentados ao URI da carga de definição de conteúdo. |

### <a name="singlesignon"></a>SingleSignOn

O elemento **SingleSignOn** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| Scope | Sim | O escopo do comportamento de logon único. Valores possíveis: `Suppressed`, `Tenant`, `Application` ou `Policy`. O valor `Suppressed` indica que o comportamento será suprimido. Por exemplo, no caso de uma única sessão de logon único, nenhuma sessão será mantida para o usuário e o usuário sempre será solicitado a fazer uma seleção de provedor de identidade. O valor `TrustFramework` indica que o comportamento é aplicado a todas as políticas na estrutura de confiança. Por exemplo, um usuário navegando por dois percursos de política para uma estrutura de confiança não é solicitado a fazer uma seleção de provedor de identidade. O valor `Tenant` indica que o comportamento é aplicado a todas as políticas no locatário. Por exemplo, um usuário navegando por dois percursos de política para um locatário não é solicitado a fazer uma seleção de provedor de identidade. O valor `Application` indica que o comportamento é aplicado a todas as políticas para o aplicativo que está fazendo a solicitação. Por exemplo, um usuário navegando por dois percursos de política para um aplicativo não é solicitado a fazer uma seleção de provedor de identidade. O valor `Policy` indica que o comportamento se aplica somente a uma política. Por exemplo, um usuário que navega pelas dois percursos de política para uma estrutura de confiança é solicitado a informar uma seleção de provedor de identidade ao alternar entre políticas. |
| KeepAliveInDays | Sim | Controla por quanto tempo o usuário permanece conectado. A definição do valor como 0 desliga a funcionalidade KMSI. Para obter mais informações, confira [Manter-me conectado](active-directory-b2c-reference-kmsi-custom.md). |

## <a name="journeyinsights"></a>JourneyInsights

O elemento **JourneyInsights** contém os seguintes atributos:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| TelemetryEngine | Sim | O valor deve ser `ApplicationInsights`. | 
| InstrumentationKey | Sim | A cadeia de caracteres que contém a chave de instrumentação para o elemento de insights de aplicativo. |
| DeveloperMode | Sim | Valores possíveis: `true` ou `false`. Se `true`, o Application Insights acelerará a telemetria pelo pipeline de processamento. Essa configuração é boa para desenvolvimento, mas restrita em grandes volumes. Os logs de atividade detalhados destinam-se somente a ajudar no desenvolvimento de políticas personalizadas. Não use o modo de desenvolvimento em produção. Os logs coletam todas as declarações enviadas entre os provedores de identidade durante o desenvolvimento. Se for usado em produção, o desenvolvedor assumirá a responsabilidade pela PII (Informações de identificação particular) coletadas no log do App Insights que ele possui. Esses logs detalhados são coletados apenas quando esse valor é definido como `true`.|
| ClientEnabled | Sim | Valores possíveis: `true` ou `false`. Se `true`, enviará o script do lado do cliente do Application Insights para acompanhar erros de exibição de página e do lado do cliente. | 
| ServerEnabled | Sim | Valores possíveis: `true` ou `false`. Se `true`, enviará o JSON UserJourneyRecorder existente como um evento personalizado para o Application Insights. | 
| TelemetryVersion | Sim | O valor deve ser `1.0.0`. | 

Para obter mais informações, confira [Coleta de Logs](active-directory-b2c-troubleshoot-custom.md)

## <a name="contentdefinitionparameters"></a>ContentDefinitionParameters

Usando políticas personalizadas no Azure AD B2C, você pode enviar um parâmetro em uma cadeia de consulta. Passando o parâmetro para seu ponto de extremidade HTML, é possível alterar dinamicamente o conteúdo da página. Por exemplo, é possível alterar a imagem de tela de fundo na página de inscrição ou de entrada do Azure AD B2C, com base em um parâmetro passado do seu aplicativo Web ou móvel. O Azure AD B2C passa os parâmetros de cadeia de consulta ao seu arquivo HTML dinâmico, como um arquivo aspx. 

O exemplo a seguir passa um parâmetro denominado `campaignId` com um valor de `hawaii` na cadeia de consulta:

`https://login.microsoft.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?pB2C_1A_signup_signin&client_id=a415078a-0402-4ce3-a9c6-ec1947fcfb3f&nonce=defaultNonce&redirect_uri=http%3A%2F%2Fjwt.io%2F&scope=openid&response_type=id_token&prompt=login&campaignId=hawaii`

O elemento **ContentDefinitionParameters** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| ContentDefinitionParameter | 0:n | Uma cadeia de caracteres que contém o par chave-valor acrescentado à cadeia de consulta de uma carga de definição de conteúdo URI. |

O elemento **ContentDefinitionParameter** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| Name | Sim | O nome do par chave-valor. |

Para obter mais informações, confira [Configurar a interface do usuário com conteúdo dinâmico usando políticas personalizadas](active-directory-b2c-ui-customization-custom-dynamic.md)

## <a name="technicalprofile"></a>TechnicalProfile

O elemento **TechnicalProfile** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- | 
| ID | Sim | O valor deve ser `PolicyProfile`. |

O **TechnicalProfile** contém os seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| DisplayName | 0:1 | A cadeia de caracteres que contém o nome do perfil técnico exibido aos usuários. |
| DESCRIÇÃO | 0:1 | A cadeia de caracteres que contém a descrição do perfil técnico exibido aos usuários. |
| Protocol | 1:1 | O protocolo usado para federação. |
| Metadados | 0:1 | A coleção de *Item* de pares chave-valor utilizados pelo protocolo de comunicação com o ponto de extremidade no decorrer de uma transação para configurar a interação entre a terceira parte confiável e outros participantes da comunidade. |
| OutputClaims | 0:1 | Uma lista de tipos de declaração obtidos como saída no perfil técnico. Cada um desses elementos contém referência a um **ClaimType** já definido na seção **ClaimsSchema** ou em uma política da qual este arquivo de política herda. |
| SubjectNamingInfo | 0:1 | O nome da entidade usado nos tokens. |

O elemento **Protocol** contém os seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| Name | Sim | O nome de um protocolo válido com suporte no Azure AD B2C que é usado como parte do perfil técnico. Valores possíveis: `OpenIdConnect` ou `SAML2`. O valor `OpenIdConnect` representa o padrão de protocolo do OpenID Connect 1.0 de acordo com a especificação OpenID Foundation. O representa `SAML2` o padrão de protocolo SAML 2.0 de acordo com a especificação OASIS. Não use um token SAML em produção. |

## <a name="outputclaims"></a>OutputClaims

O elemento **OutputClaims** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| OutputClaim | 0:n | O nome de um tipo de declaração esperado na lista com suporte para a política que a terceira parte confiável assina. Essa declaração serve como uma saída para o perfil técnico. |

O elemento **OutputClaim** contém os seguintes atributos:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Sim | Uma referência a um **ClaimType** já definido na seção **ClaimsSchema** no arquivo de política. |
| DefaultValue | Não | Um valor padrão que poderá ser usado se o valor da declaração estiver vazio. |
| PartnerClaimType | Não | Envia a declaração em um nome diferente, conforme configurado na definição ClaimType. |

### <a name="subjectnaminginfo"></a>SubjectNamingInfo

Com o elemento **SubjectNameingInfo**, você controla o valor da entidade do token:
- **Token JWT** – o `sub` de declaração. Essa é uma entidade de segurança sobre a qual o token declara informações, como o usuário de um aplicativo. Esse valor é imutável e não pode ser reatribuído nem reutilizado. Ele pode ser usado para executar verificações de autorização seguras, por exemplo, quando o token é usado para acessar um recurso. Por padrão, a declaração de entidade é preenchida com a ID de objeto do usuário no diretório. Para obter mais informações, confira [Token, sessão e configuração de logon único](active-directory-b2c-token-session-sso.md).
- **Token SAML** – o elemento `<Subject><NameID>` que identifica o elemento de assunto.

O elemento **SubjectNamingInfo** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| ClaimType | Sim | Uma referência a **PartnerClaimType** da declaração de saída. As declarações de saída devem ser definidas na política de terceira parte confiável da coleção **OutputClaims**. |

O exemplo a seguir mostra como definir uma terceira parte confiável OpenId Connect. As informações de nome da entidade são configuradas como a `objectId`:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```
O token JWT inclui a declaração `sub` com a objectId do usuário:

```JSON
{
  ...
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  ...
}
```


