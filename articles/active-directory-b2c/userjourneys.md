---
title: UserJourneys | Microsoft Docs
description: Especifica o elemento UserJourneys de uma política personalizada no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d2e6ad9aa9692efa4ea5633dff78b262bb1917be
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512024"
---
# <a name="userjourneys"></a>UserJourneys

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Percursos do usuário especificam caminhos explícitos por meio dos quais uma política permite que um aplicativo de terceira parte confiável obtenhas as declarações desejadas para um usuário. O usuário passa por esses caminhos para recuperar as declarações que devem ser apresentadas para a terceira parte confiável. Em outras palavras, os percursos do usuário definem a lógica de negócios pela qual o usuário final passa conforme o Framework de Experiência de Identidade do Azure AD B2C processa a solicitação.

Esses percursos do usuário podem ser considerados como modelos disponíveis para satisfazer a necessidade de núcleo de várias partes confiáveis da comunidade de interesse. Percursos do usuário facilitam a definição da parte de uma política de terceiros terceira parte confiável. Uma política pode definir vários percursos do usuário. Cada percurso do usuário é uma sequência de etapas de orquestração.

Para definir os percursos do usuário com suporte da política, um elemento **UserJourneys** é adicionado sob o elemento de nível superior do arquivo de política. 

O elemento **UserJourneys** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| UserJourney | 1:n | Um percurso do usuário que define todos os constructos necessários para um fluxo completo do usuário. | 

O elemento **UserJourney** contém o seguinte atributo:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| ID | Sim | Um identificador de um percurso do usuário que pode ser usado para referenciá-lo de outros elementos na política. O elemento **DefaultUserJourney** da [política de terceira parte confiável](relyingparty.md) aponta para esse atributo. |

O elemento **UserJourney** contém os seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| OrchestrationSteps | 1:n | Uma sequência de orquestração que deve ser seguida para ter uma transação bem-sucedida. Cada percurso do usuário consiste de uma lista ordenada de etapas de orquestração que são executadas em sequência. Se alguma delas falhar, a transação falhará. |

## <a name="orchestrationsteps"></a>OrchestrationSteps

Um percurso do usuário é representado como uma sequência de orquestração que deve ser seguida para ter uma transação bem-sucedida. Se alguma delas falhar, a transação falhará. Essas etapas de orquestração fazem referência aos blocos de construção e aos provedores de declarações permitidos no arquivo de política. Qualquer etapa de orquestração responsável por mostrar ou renderizar uma experiência do usuário também tem uma referência ao identificador de definição do conteúdo correspondente.

Etapas de orquestração podem ser executadas condicionalmente com base em pré-condições definidas no elemento de etapa de orquestração. Por exemplo, você pode verificar para executar uma etapa de orquestração somente se não houver um declarações específicas, ou se uma declaração for igual ou não ao valor especificado. 

Para especificar a lista ordenada de etapas de orquestração, um elemento **OrchestrationSteps** é adicionado como parte da política. Este elemento é obrigatório.

O elemento **OrchestrationSteps** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| OrchestrationStep | 1:n | Uma etapa de orquestração ordenada. | 

O elemento **OrchestrationStep** contém os seguintes atributos:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| `Order` | Sim | A ordem das etapas de orquestração. | 
| `Type` | Sim | O tipo da etapa de orquestração. Valores possíveis: <ul><li>**ClaimsProviderSelection** – indica que a etapa de orquestração apresenta vários provedores de declarações para o usuário selecionar um.</li><li>**CombinedSignInAndSignUp** – indica que a etapa de orquestração apresenta uma conexão de provedor social combinada e uma página de entrada de conta local.</li><li>**ClaimsExchange** – indica que a etapa de orquestração troca declarações com um provedor de declarações.</li><li>**SendClaims** – indica que a etapa de orquestração envia as declarações à terceira parte confiável com um token emitido por um emissor de declarações.</li></ul> | 
| ContentDefinitionReferenceId | Não | O identificador da [definição de conteúdo](contentdefinitions.md) associada com esta etapa de orquestração. Geralmente, o identificador de referência da definição de conteúdo é definido no perfil técnico autodeclarado. No entanto, há alguns casos em que o Azure AD B2C precisa exibir algo sem um perfil técnico. Há dois exemplos - se o tipo da etapa de orquestração é um dos seguintes: `ClaimsProviderSelection` ou `CombinedSignInAndSignUp`, Azure AD B2C precisa para exibir a seleção de provedor de identidade sem a necessidade de um perfil técnico. | 
| CpimIssuerTechnicalProfileReferenceId | Não | O tipo da etapa de orquestração é `SendClaims`. Esta propriedade define o identificador do perfil técnico do provedor de declarações que emite o token para a terceira parte confiável.  Se ausente, nenhum token de terceira parte confiável será criado. |


O elemento **OrchestrationStep** pode conter um dos seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- | 
| Pré-condições | 0:n | Uma lista de pré-condições que devem ser atendidas para que a etapa de orquestração seja executada. | 
| ClaimsProviderSelections | 0:n | Uma lista de seleções do provedor de declarações para a etapa de orquestração. | 
| ClaimsExchanges | 0:n | Uma lista de trocas de declarações para a etapa de orquestração. | 

### <a name="preconditions"></a>Pré-condições

O elemento **Preconditions** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- | 
| Pré-condição | 0:n | Dependendo do perfil técnico usado, redireciona o cliente de acordo com a seleção do provedor de declarações ou faz uma chamada de servidor para trocar declarações. | 


#### <a name="precondition"></a>Pré-condição

O **pré-condição** elemento contém os seguintes atributos:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| `Type` | Sim | O tipo de verificação ou consulta ser executada para essa pré-condição. O valor poderá ser **ClaimsExist**, que especifica que as ações deverão ser executadas se as declarações especificadas existirem no conjunto de declarações do usuário atual, ou **ClaimEquals**, que especifica que as ações deverão ser executadas se a declaração especificada existir e seu valor for igual ao valor especificado. |
| `ExecuteActionsIf` | Sim | Use um teste de verdadeiro ou falso para decidir se as ações na pré-condição devem ser executadas. | 

O elemento **Precondition** contém os seguintes elementos:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| Value | 1:n | Um ClaimTypeReferenceId a ser consultado. Outro elemento de valor contém o valor a ser verificado.</li></ul>|
| Ação | 1:1 | A ação que deverá ser executada se a verificação de pré-condição dentro de uma etapa de orquestração for verdadeira. Se o valor de `Action` estiver definido como `SkipThisOrchestrationStep`, o `OrchestrationStep` associado não deverá ser executado. | 

#### <a name="preconditions-examples"></a>Exemplos de pré-condições

As pré-condições a seguir verificam se o objectId do usuário existe. No percurso do usuário, o usuário optou por entrar usando a conta local. Se a objectId existir, ignore esta etapa de orquestração.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

As pré-condições a seguir verificam se o usuário se conectou usando uma conta social. É feita uma tentativa de encontrar a conta do usuário no diretório. Se o usuário faz logon ou se inscreve com uma conta local, ignore esta etapa de orquestração.

```XML
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>authenticationSource</Value>
      <Value>localAccountAuthentication</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="AADUserReadUsingAlternativeSecurityId" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId-NoError" />
  </ClaimsExchanges>
</OrchestrationStep>
```

As pré-condições podem verificar várias pré-condições. O exemplo a seguir verifica se 'objectId' ou 'email' existe. Se a primeira condição for true, a jornada pulará para a próxima etapa de orquestração.

```XML
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <Preconditions>
  <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>email</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>      
  <ClaimsExchanges>
    <ClaimsExchange Id="SelfAsserted-SocialEmail" TechnicalProfileReferenceId="SelfAsserted-SocialEmail" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsproviderselection"></a>ClaimsProviderSelection

Uma etapa de orquestração do tipo `ClaimsProviderSelection` ou `CombinedSignInAndSignUp` pode conter uma lista de provedores de declarações com que um usuário pode se conectar. A ordem dos elementos dentro dos elementos `ClaimsProviderSelections` controla a ordem dos provedores de identidade apresentados ao usuário.

O elemento **ClaimsProviderSelection** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| ClaimsProviderSelection | 0:n | Fornece a lista de provedores de declarações que podem ser selecionados.|

O elemento **ClaimsProviderSelection** contém os seguintes atributos: 

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| TargetClaimsExchangeId | Não | O identificador da troca de declarações, que é executado na próxima etapa de orquestração da seleção do provedor de declarações. Esse atributo ou o atributo ValidationClaimsExchangeId deve ser especificado, mas não ambos. | 
| ValidationClaimsExchangeId | Não | O identificador da troca de declarações, que é executado na etapa de orquestração atual para validar a seleção do provedor de declarações. Esse atributo ou o atributo TargetClaimsExchangeId deve ser especificado, mas não ambos. |

### <a name="claimsproviderselection-example"></a>Exemplo de ClaimsProviderSelection

Na etapa de orquestração a seguir, o usuário pode optar por entrar com o LinkedIn, Facebook, Twitter, Google ou uma conta local. Se o usuário selecionar um dos provedores de identidade social, a segunda etapa de orquestração será executada com a troca de declaração selecionada especificada no atributo `TargetClaimsExchangeId`. A segunda etapa de orquestração redireciona o usuário para o provedor de identidade social para concluir o processo de conexão. Se o usuário optar por se conectar com a conta local, o Azure AD B2C permanecerá na mesma etapa de orquestração (a mesma página de conexão ou de entrada) e ignorará a segunda etapa de orquestração.

```XML
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
    <ClaimsProviderSelections>
    <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    <ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
    <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
    </ClaimsProviderSelections>
    <ClaimsExchanges>
    <ClaimsExchange Id="LocalAccountSigninEmailExchange" 
                    TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
    </ClaimsExchanges>
</OrchestrationStep>


<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## <a name="claimsexchanges"></a>ClaimsExchanges

O elemento **ClaimsExchanges** contém o seguinte elemento:

| Elemento | Ocorrências | DESCRIÇÃO |
| ------- | ----------- | ----------- |
| ClaimsExchange | 0:n | Dependendo do perfil técnico usado, redireciona o cliente de acordo com a seleção do ClaimsProviderSelection ou faz uma chamada de servidor para trocar declarações. | 

O elemento **ClaimsExchange** contém os seguintes atributos:

| Atributo | Obrigatório | DESCRIÇÃO |
| --------- | -------- | ----------- |
| ID | Sim | Um identificador da etapa de troca de declarações. O identificador é usado para referenciar a troca de declarações para uma etapa da seleção do provedor de declarações na política. | 
| TechnicalProfileReferenceId | Sim | O identificador do perfil técnico que deve ser executado. |
