---
title: Glossário de desenvolvedor do Microsoft identity platform | Azure
description: Uma lista de termos de recursos e conceitos de desenvolvedor do Microsoft identity platform comumente usadas.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 551512df-46fb-4219-a14b-9c9fc23998ba
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/21/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jmprieur, saeeda, jesakowi, nacanuma
ms.collection: M365-identity-device-management
ms.openlocfilehash: 314d7a8e8cf6837e2b22446ba23fee03d539bf35
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235346"
---
# <a name="microsoft-identity-platform-developer-glossary"></a>Glossário de desenvolvedor do Microsoft identity platform

Este artigo contém as definições para alguns dos conceitos de desenvolvedor e terminologia, que são úteis ao aprender sobre desenvolvimento de aplicativos usando a plataforma de identidade da Microsoft.

## <a name="access-token"></a>o token de acesso

Um tipo de [token de segurança](#security-token) emitido por um [servidor de autorização](#authorization-server) e usado por um [aplicativo cliente](#client-application) para acessar um [servidor de recursos protegido](#resource-server). Normalmente na forma de um [JWT (Token Web JSON)][JWT], o token consiste na autorização concedida ao cliente pelo [proprietário do recurso](#resource-owner) para um nível de acesso solicitado. O token contém todas as [declarações](#claim) aplicáveis sobre a entidade, habilitando o aplicativo cliente a usá-lo como uma forma de credenciais ao acessar um recurso específico. Isso também elimina a necessidade do proprietário do recurso de expor as credenciais para o cliente.

Os tokens de acesso às vezes são chamados de "Usuário+Aplicativo" ou "Somente Aplicativo", dependendo das credenciais que estão sendo representadas. Por exemplo, quando um aplicativo cliente usa:

* [Concessão de autorização de "código de autorização"](#authorization-grant), o usuário final é autenticado primeiro como o proprietário do recurso, delegando a autorização ao cliente para acessar o recurso. O cliente é autenticado depois, ao obter o token de acesso. Às vezes, o token pode ser chamado mais especificamente de token de "Usuário+Aplicativo", que representa o usuário que autorizou o aplicativo cliente e o aplicativo.
* [Concessão de autorização de "credenciais de cliente"](#authorization-grant), o cliente fornece a única autenticação, funcionando sem a autenticação/autorização do proprietário do recurso. Portanto, às vezes o token pode ser chamado de token "Somente de Aplicativo".

Ver [plataforma de identidade da Microsoft referência de Token] [ AAD-Tokens-Claims] para obter mais detalhes.

## <a name="application-id-client-id"></a>ID do aplicativo (ID do cliente)

Os problemas de identificador exclusivo do Azure AD para um registro de aplicativo que identifica um aplicativo específico e as configurações associadas. Essa ID do aplicativo ([ID do cliente](https://tools.ietf.org/html/rfc6749#page-15)) é usado ao executar a autenticação de solicitações e é fornecida para as bibliotecas de autenticação em tempo de desenvolvimento. A ID do aplicativo (ID do cliente) não é um segredo.

## <a name="application-manifest"></a>manifesto do aplicativo

Um recurso fornecido pelo [Portal do Azure][AZURE-portal], que produz uma representação JSON da configuração de identidade do aplicativo, usada como um mecanismo para atualizar suas entidades de [Aplicativo][AAD-Graph-App-Entity] e [ServicePrincipal][AAD-Graph-Sp-Entity] associadas. Veja [Noções básicas sobre o manifesto de aplicativo do Azure Active Directory][AAD-App-Manifest] para saber mais.

## <a name="application-object"></a>objeto de aplicativo

Quando você registra/atualiza um aplicativo no [Portal do Azure][AZURE-portal], o portal cria/atualiza um objeto de aplicativo e um [objeto de entidade de serviço](#service-principal-object) correspondente para esse locatário. O objeto de aplicativo *define* a configuração de identidade do aplicativo globalmente (em todos os locatários aos quais ele tem acesso), fornecendo um modelo do qual seus objetos de entidade de serviço correspondentes são *derivados* para uso localmente em tempo de execução (em um locatário específico).

Para obter mais informações, consulte [Objetos de entidade de serviço e aplicativo][AAD-App-SP-Objects].

## <a name="application-registration"></a>registro de aplicativo

Para permitir que um aplicativo se integre e delegue funções de Gerenciamento de Acesso e Identidade ao Azure AD, ele deve ser registrado em um [locatário](#tenant)do Azure AD. Ao registrar seu aplicativo no Azure AD, você fornece uma configuração de identidade para o aplicativo, permitindo que ele se integre ao Azure AD e use recursos como:

* Gerenciamento robusto de Logon Único usando o Gerenciamento de Identidade do Azure AD e a implementação de protocolo [OpenID Connect][OpenIDConnect]
* Acesso agenciado a [recursos protegidos](#resource-server) pela [aplicativos cliente](#client-application), por meio do OAuth 2.0 [servidor de autorização](#authorization-server)
* [Estrutura de consentimento](#consent) para gerenciar o acesso do cliente a recursos protegidos, com base na autorização do proprietário do recurso.

Veja [Integrando aplicativos com o Azure Active Directory][AAD-Integrating-Apps] para saber mais.

## <a name="authentication"></a>Autenticação

O ato de desafiar uma parte para o fornecimento de credenciais legítimas, fornecendo a base para a criação de uma entidade de segurança a ser usada para controle de identidade e acesso. Durante uma [concessão de autorização OAuth2](#authorization-grant), por exemplo, a parte que está realizando a autenticação está desempenhando a função de [proprietário do recurso](#resource-owner) ou [aplicativo cliente](#client-application), dependendo da concessão usada.

## <a name="authorization"></a>autorização

O ato de conceder a uma entidade de segurança autenticada permissão para fazer algo. Há dois casos de uso principais no modelo de programação do Azure AD:

* Durante um fluxo de [concessão de autorização OAuth2](#authorization-grant): quando o [proprietário do recurso](#resource-owner) concede autorização para o [aplicativo cliente](#client-application), permitindo que o cliente acesse recursos do proprietário do recurso.
* Durante o acesso a recursos pelo cliente: conforme implementado pelo [servidor de recursos](#resource-server), usando os valores de [declaração](#claim) presentes no [token de acesso](#access-token) para tomar decisões de controle de acesso com base neles.

## <a name="authorization-code"></a>código de autorização

Um "token" de curta duração fornecido a um [aplicativo cliente](#client-application) pelo [ponto de extremidade de autorização](#authorization-endpoint), como parte do fluxo de "código de autorização", uma das quatro [concessões de autorização](#authorization-grant) OAuth2. O código é retornado para o aplicativo cliente em resposta à autenticação de um [proprietário do recurso](#resource-owner), indicando que o proprietário do recurso delegou autorização para acessar os recursos solicitados. Como parte do fluxo, o código é resgatado posteriormente para um [token de acesso](#access-token).

## <a name="authorization-endpoint"></a>ponto de extremidade de autorização

Um dos pontos de extremidade implementados pelo [servidor de autorização](#authorization-server), usado para interagir com o [proprietário do recurso](#resource-owner) para fornecer uma [concessão de autorização](#authorization-grant) durante um fluxo de concessão de autorização OAuth2. Dependendo do fluxo de concessão de autorização usado, a concessão real fornecida pode variar, incluindo um [código de autorização](#authorization-code) ou um [token de segurança](#security-token).

Veja as seções sobre [tipos de concessão de autorização][OAuth2-AuthZ-Grant-Types] e [ponto de extremidade de autorização][OAuth2-AuthZ-Endpoint] da especificação OAuth2 e a [especificação OpenIDConnect][OpenIDConnect-AuthZ-Endpoint] para saber mais.

## <a name="authorization-grant"></a>concessão de autorização

Uma credencial que representa a [autorização](#resource-owner) [do proprietário do recurso](#authorization) para acessar seus recursos protegidos, concedida a um [aplicativo cliente](#client-application). Um aplicativo cliente pode usar um dos [quatro tipos de concessão definidos pela Estrutura de Autorização OAuth2][OAuth2-AuthZ-Grant-Types] para obter uma concessão, dependendo do tipo/requisitos do cliente: "concessão de código de autorização", "concessão de credenciais de cliente", "concessão implícita" e "concessão de credenciais de senha do proprietário do recurso". A credencial retornada ao cliente é um [token de acesso](#access-token) ou um [código de autorização](#authorization-code) (posteriormente trocado por um token de acesso), dependendo do tipo de concessão de autorização usado.

## <a name="authorization-server"></a>servidor de autorização

Conforme definido pela [Estrutura de Autorização OAuth2][OAuth2-Role-Def], o servidor é responsável pela emissão de tokens de acesso ao [cliente](#client-application) depois de autenticar com êxito o [proprietário do recurso](#resource-owner) e obter sua autorização. Um [aplicativo cliente](#client-application) interage com o servidor de autorização em tempo de execução por meio de seus pontos de extremidade de [autorização](#authorization-endpoint) e [token](#token-endpoint), de acordo com as [concessões de autorização](#authorization-grant) definidas pelo OAuth2.

No caso de integração de aplicativos da plataforma Microsoft identity, plataforma de identidade Microsoft implementa a função de servidor de autorização para aplicativos do Azure AD e o serviço da Microsoft APIs, por exemplo [APIs do Microsoft Graph] [Microsoft-Graph].

## <a name="claim"></a>declaração

Um [token de segurança](#security-token) contêm declarações, que fornecem asserções sobre uma entidade (como um [aplicativo cliente](#client-application) ou um [proprietário de recursos](#resource-owner)) para outra entidade (como o [servidor de recursos](#resource-server)). As declarações são pares de nome/valor que transmitem fatos sobre a entidade do token (por exemplo, a entidade de segurança que foi autenticada pelo [servidor de autorização](#authorization-server)). As declarações presentes em um token específico dependem de diversas variáveis, incluindo o tipo de token, o tipo de credencial usado para autenticar a entidade, a configuração de aplicativo etc.

Ver [referência de token do Microsoft identity platform] [ AAD-Tokens-Claims] para obter mais detalhes.

## <a name="client-application"></a>aplicativo cliente

Conforme definido pela [Estrutura de Autorização OAuth2][OAuth2-Role-Def], um aplicativo que faz solicitações de recursos protegidas em nome do [proprietário do recurso](#resource-owner). O termo "cliente" não implica características de implementação de hardware específicas (por exemplo, se o aplicativo é executado em um servidor, na área de trabalho ou em outros dispositivos).

Um aplicativo cliente solicita [autorização](#authorization) de um proprietário de recurso para participar de um fluxo de [concessão de autorização OAuth2](#authorization-grant) e pode acessar APIs/dados em nome do proprietário do recurso. A estrutura de autorização OAuth2 [define dois tipos de clientes][OAuth2-Client-Types], "confidencial" e "público", com base na capacidade do cliente de manter a confidencialidade de suas credenciais. Os aplicativos podem implementar um [cliente Web (confidencial)](#web-client) que é executado em um servidor Web, um [cliente nativo (público)](#native-client) instalado em um dispositivo ou um [cliente baseado em agente de usuário (público)](#user-agent-based-client) que é executado no navegador do dispositivo.

## <a name="consent"></a>consentimento

O processo para que um [proprietário do recurso](#resource-owner) conceda autorização a um [aplicativo cliente](#client-application), para acessar recursos protegidos em [permissões](#permissions) específicas, em nome do proprietário do recurso. Dependendo das permissões solicitadas pelo cliente, um administrador ou usuário deverá consentir o acesso a seus dados da empresa/individuais, respectivamente. Observe que, em um cenário de [multilocatário](#multi-tenant-application), a [entidade de serviço](#service-principal-object) do aplicativo também é registrada no locatário do usuário isso que fornece o consentimento.

Veja [estrutura de consentimento](consent-framework.md) para obter mais informações.

## <a name="id-token"></a>token de ID

Um [token de segurança](#security-token) [OpenID Connect][OpenIDConnect-ID-Token] fornecido por um [ponto de extremidade de autorização](#authorization-endpoint) do [servidor de autorização](#authorization-server), que contém [declarações](#claim) referentes à autenticação de um [proprietário de recurso](#resource-owner) de usuário final. Assim como um token de acesso, os tokens de ID também são representados como um [JWT (Token Web JSON)][JWT] assinado digitalmente. Diferentemente de um token de acesso, as declarações de um token de ID não são usadas para fins relacionados ao acesso a recursos e ao controle de acesso especificamente.

Ver [referência de token do Microsoft identity platform] [ AAD-Tokens-Claims] para obter mais detalhes.

## <a name="microsoft-identity-platform"></a>Plataforma de identidade da Microsoft

A plataforma de identidade da Microsoft é uma evolução da plataforma de desenvolvedor e serviço de identidade do Azure AD (Azure Active Directory). Ela permite que os desenvolvedores criem aplicativos que se conectam a todas as identidades da Microsoft e obtenham tokens para chamar o Microsoft Graph, outras APIs da Microsoft ou APIs que os desenvolvedores criaram. Trata-se de uma plataforma completa que consiste em um serviço de autenticação, bibliotecas, registro de aplicativo e configuração, documentação completa do desenvolvedor, exemplos de código e outros conteúdos do desenvolvedor. A plataforma de identidade da Microsoft dá suporte a protocolos padrão do setor, como OAuth 2.0 e OpenID Connect. Veja [Sobre a plataforma de identidade da Microsoft](about-microsoft-identity-platform.md) para obter mais detalhes.

## <a name="multi-tenant-application"></a>Aplicativos multilocatários

Uma classe de aplicativo que permite se conectar e [consentir](#consent) por usuários provisionados em qualquer [locatário](#tenant) do Azure AD, incluindo locatários diferentes daquele em que o cliente está registrado. Aplicativos de [cliente nativo](#native-client) são multilocatários por padrão, enquanto aplicativos de [cliente Web](#web-client) e [recurso da Web/API](#resource-server) têm a capacidade de selecionar locatário único ou multilocatário. Por outro lado, um aplicativo Web registrado como locatário único só permitirá conexões de contas de usuário provisionadas no mesmo locatário que aquele em que o aplicativo está registrado.

Veja [Como conectar qualquer usuário do Azure AD usando o padrão de aplicativo multilocatário][AAD-Multi-Tenant-Overview] para saber mais.

## <a name="native-client"></a>cliente nativo

Um tipo de [aplicativo cliente](#client-application) que é instalado de forma nativa em um dispositivo. Como todo o código é executado em um dispositivo, ele é considerado um cliente "público" devido à sua incapacidade de armazenar credenciais de forma particular/confidencial. Veja [Perfis e tipos de cliente OAuth2][OAuth2-Client-Types] para saber mais.

## <a name="permissions"></a>permissões

Um [aplicativo cliente](#client-application) obtém acesso a um [servidor de recursos](#resource-server) declarando solicitações de permissão. Dois tipos estão disponíveis:

* Permissões “delegadas”, que especificam o acesso [com base no escopo](#scopes) usando a autorização delegada do [proprietário do recurso](#resource-owner) conectado, são apresentadas para o recurso em tempo de execução como [declarações “scp”](#claim) no [token de acesso](#access-token) do cliente.
* Permissões de “aplicativo”, que especificam o acesso [baseado em função](#roles) usando as credenciais/identidade do aplicativo cliente, são apresentadas para o recurso em tempo de execução como [declarações de “funções”](#claim) no token de acesso do cliente.

Também surgem durante o processo de [consentimento](#consent) , oferecendo ao administrador ou ao proprietário do recurso a oportunidade de conceder/negar ao cliente o acesso aos recursos em seu locatário.

Solicitações de permissão são configuradas na **permissões de API** página de um aplicativo na [portal do Azure][AZURE-portal], selecionando o desejado "permissões delegadas" e " Permissões de aplicativo"(a segunda opção requer associação na função de Administrador Global). Como um [cliente público](#client-application) não pode manter credenciais com segurança, ele só pode solicitar permissões delegadas, enquanto um [cliente confidencial](#client-application) tem a capacidade de solicitar permissões delegadas e de aplicativo. O [objeto de aplicativo](#application-object) do cliente armazena as permissões declaradas em sua [propriedade requiredResourceAccess][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>proprietário do recurso

Conforme definido pela [Estrutura de Autorização OAuth2][OAuth2-Role-Def], uma entidade com a capacidade de conceder acesso a um recurso protegido. Quando o proprietário do recurso é uma pessoa, é chamado de usuário final. Por exemplo, quando um [aplicativo cliente](#client-application) quer acessar a caixa de correio do usuário por meio da [API do Microsoft Graph][Microsoft-Graph], requer a permissão do proprietário do recurso da caixa de correio.

## <a name="resource-server"></a>servidor de recursos

Conforme definido pela [Estrutura de Autorização OAuth2][OAuth2-Role-Def], um servidor que hospeda recursos protegidos, capaz de aceitar e responder a solicitações de recursos protegidos de [aplicativos cliente](#client-application) que apresentam um [token de acesso](#access-token). Também conhecido como um servidor de recursos protegidos ou aplicativo de recurso.

Um servidor de recursos expõe APIs e impõe o acesso a seus recursos protegidos por meio de [escopos](#scopes) e [funções](#roles), usando a Estrutura de Autorização OAuth 2.0. Alguns exemplos são a API do Graph do Azure AD, que fornece acesso aos dados de locatário do Azure AD, e APIs do Office 365, que fornecem acesso a dados como email e calendário. Ambos também são acessíveis por meio da [API do Microsoft Graph][Microsoft-Graph].

Assim como um aplicativo cliente, a configuração de identidade do aplicativo de recurso é estabelecida via [registro](#application-registration) em um locatário do Azure AD, fornecendo o objeto de entidade de serviço e de aplicativo. Algumas APIs fornecidas pela Microsoft, como a API do Graph do Azure AD, têm entidades de serviço previamente registradas disponíveis em todos os locatários durante o provisionamento.

## <a name="roles"></a>roles

Assim como os [escopos](#scopes), as funções fornecem uma maneira para que um [servidor de recursos](#resource-server) governe o acesso a seus recursos protegidos. Há dois tipos: uma função de "usuário" implementa o controle de acesso baseado em função para usuários/grupos que exigem acesso ao recurso, enquanto uma função de "aplicativo" implementa o mesmo para [aplicativos cliente](#client-application) que requerem acesso.

As funções são cadeias de caracteres definidas por recursos (por exemplo, "Aprovador de despesas", "Somente leitura", "Directory.ReadWrite.All"), gerenciadas no [Portal do Azure][AZURE-portal] por meio do [manifesto do aplicativo](#application-manifest) do recurso e armazenadas na [propriedade appRoles][AAD-Graph-Sp-Entity] do recurso. O Portal do Azure também é usado para atribuir usuários às funções de "usuário" e configurar [permissões de aplicativo](#permissions) do cliente para acessar uma função de "aplicativo".

Para ver uma discussão detalhada sobre as funções de aplicativo expostas pela API do Graph do Azure AD, veja [Escopos de permissão da API do Graph][AAD-Graph-Perm-Scopes]. Para um exemplo de implementação passo a passo, consulte [Gerenciar acesso usando RBAC e o portal do Azure][AAD-RBAC].

## <a name="scopes"></a>escopos

Assim como as [funções](#roles), os escopos fornecem uma maneira para que um [servidor de recursos](#resource-server) governe o acesso a seus recursos protegidos. Os escopos são usados para implementar o controle de acesso [baseado em escopo][OAuth2-Access-Token-Scopes] em um [aplicativo cliente](#client-application) que recebeu acesso delegado ao recurso de seu proprietário.

Os escopos são cadeias de caracteres definidas por recursos (por exemplo "Mail.Read", "Directory.ReadWrite.All"), gerenciados no [Portal do Azure][AZURE-portal] por meio do [manifesto do aplicativo](#application-manifest) do recurso e armazenados na [propriedade oauth2Permissions][AAD-Graph-Sp-Entity] do recurso. O Portal do Azure também é usado para configurar [permissões delegadas](#permissions) do aplicativo cliente para acessar um escopo.

Uma prática recomendada para a convenção de nomenclatura é usar um formato "resource.operation.constraint". Para obter uma discussão detalhada sobre os escopos expostos pela API do Graph do Azure AD, veja [Escopos de permissão da API do Graph][AAD-Graph-Perm-Scopes]. Para escopos expostos por serviços do Office 365, veja [Referência de permissões de API do Office 365][O365-Perm-Ref].

## <a name="security-token"></a>token de segurança

Um documento assinado que contém declarações, como um token OAuth2 ou uma asserção SAML 2.0. Uma [concessão de autorização](#authorization-grant) OAuth2, um [token de acesso](#access-token) (OAuth2) e um [Token de ID](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) são tipos de tokens de segurança, que são implementados como um [JWT (Token Web JSON)][JWT].

## <a name="service-principal-object"></a>objeto de entidade de serviço

Quando você registra/atualiza um aplicativo no [Portal do Azure][AZURE-portal], o portal cria/atualiza um [objeto de aplicativo](#application-object) e um objeto de entidade de serviço correspondente para esse locatário. O objeto de aplicativo *define* a configuração de identidade do aplicativo globalmente (em todos os locatários em que o aplicativo associado recebeu acesso) e é o modelo do qual seus objetos de entidade de serviço correspondentes são *derivados* para uso localmente em tempo de execução (em um locatário específico).

Para obter mais informações, consulte [Objetos de entidade de serviço e aplicativo][AAD-App-SP-Objects].

## <a name="sign-in"></a>entrada

O processo para que um [aplicativo cliente](#client-application) inicie a autenticação do usuário final e capture o estado relacionado, para adquirir um [token de segurança](#security-token) e obter o escopo da sessão do aplicativo para esse estado. O estado pode incluir artefatos, como informações de perfil de usuário, e informações derivadas de declarações de token.

A função de entrada de um aplicativo normalmente é usada para implementar o SSO (logon único). Ela também pode ser precedida por uma função de "inscrição", como o ponto de entrada para um usuário final obter acesso a um aplicativo (após a primeira entrada). A função de inscrição é usada para coletar e persistir o estado adicional específico do usuário e pode exigir o [consentimento do usuário](#consent).

## <a name="sign-out"></a>saída

O processo de cancelamento de autenticação de um usuário final, desanexando o estado do usuário associado à sessão do [aplicativo cliente](#client-application) durante a [entrada](#sign-in)

## <a name="tenant"></a>locatário

Uma instância de um diretório do Azure AD é chamada de locatário do Azure AD. Ele fornece vários recursos, incluindo:

* um serviço de registro para aplicativos integrados
* autenticação de contas de usuário e aplicativos registrados
* Pontos de extremidade REST necessários para dar suporte a vários protocolos, incluindo OAuth2 e SAML, incluindo [ponto de extremidade de autorização](#authorization-endpoint), [ponto de extremidade de token](#token-endpoint) e ponto de extremidade "comum" usado por [aplicativos de multilocação](#multi-tenant-application).

Locatários do Azure AD são criados/associados com assinaturas do Azure e o Office 365 durante a inscrição, fornecendo recursos de Gerenciamento de Identidades e Acesso para a assinatura. Os administradores de assinatura do Azure também podem criar locatários do Azure AD adicionais por meio do Portal do Azure. Veja [Como obter um locatário do Azure Active Directory][AAD-How-To-Tenant] para saber mais sobre as várias maneiras de obter acesso a um locatário. Veja [Como as assinaturas do Azure são associadas ao Azure Active Directory][AAD-How-Subscriptions-Assoc] para saber mais sobre a relação entre um locatário do Azure AD e assinaturas.

## <a name="token-endpoint"></a>ponto de extremidade de token

Um dos pontos de extremidade implementados pelo [servidor de autorização](#authorization-server) para dar suporte a [concessões de autorização](#authorization-grant) OAuth2. Dependendo da concessão, ele pode ser usado para adquirir um [token de acesso](#access-token) (e um token de "atualização" relacionado) para um [cliente](#client-application) ou um [token de ID](#id-token) quando usado com o protocolo [OpenID Connect][OpenIDConnect].

## <a name="user-agent-based-client"></a>Cliente com base em agente de usuário

Um tipo de [aplicativo cliente](#client-application) que baixa código de um servidor Web e é executado em um agente de usuário (por exemplo, um navegador da Web), como um SPA (aplicativo de página única). Como todo o código é executado em um dispositivo, ele é considerado um cliente "público" devido à sua incapacidade de armazenar credenciais de forma particular/confidencial. Para obter mais informações, consulte [Perfis e tipos de cliente OAuth2s][OAuth2-Client-Types].

## <a name="user-principal"></a>entidade de usuário

Da mesma forma como um objeto de entidade de serviço é usado para representar uma instância de aplicativo, um objeto de entidade de usuário é outro tipo de entidade de segurança, que representa um usuário. A [entidade de usuário][AAD-Graph-User-Entity] do Azure AD Graph define o esquema de um objeto de usuário, incluindo propriedades relacionadas ao usuário, como nome e sobrenome, nome UPN, associação à função de diretório, etc. Isso fornece a configuração de identidade de usuário do Azure AD para estabelecer um nome UPN no tempo de execução. A entidade de usuário é usada para representar um usuário autenticado para Logon Único, gravando a delegação de [consentimento](#consent), tomando decisões de controle de acesso etc.

## <a name="web-client"></a>cliente Web

Um tipo de [aplicativo cliente](#client-application) que executa todo o código em um servidor Web e pode funcionar como um cliente "confidencial" armazenando com segurança suas credenciais no servidor. Para obter mais informações, consulte [Perfis e tipos de cliente OAuth2s][OAuth2-Client-Types].

## <a name="next-steps"></a>Próximas etapas

O [guia do desenvolvedor do Microsoft identity platform] [ AAD-Dev-Guide] é a página inicial a ser usado para todos os Microsoft identity platform relacionados ao desenvolvimento tópicos, incluindo uma visão geral de [aplicativo integração] [ AAD-How-To-Integrate] e os fundamentos do [autenticação de identidade da plataforma Microsoft e cenários de autenticação com suporte][AAD-Auth-Scenarios]. Você também pode encontrar exemplos de código e tutoriais sobre como iniciar rapidamente o [GitHub](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=).

Use a seção de comentários a seguir para fornecer comentários e ajudar a refinar e moldar esse conteúdo, incluindo solicitações de novas definições ou atualizando as existentes!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]:reference-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]:../fundamentals/active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]:quickstart-create-new-tenant.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Multi-Tenant-Overview]:howto-convert-app-to-be-multi-tenant.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AZURE-portal]: https://portal.azure.com
[AAD-RBAC]: ../../role-based-access-control/role-assignments-portal.md
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://developer.microsoft.com/graph
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
