---
title: Histórico de versão do Azure AD Connect Health
description: Este documento descreve as versões do Azure AD Connect Health e o que foi incluído nessas versões.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/20/2019
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58de8de9a9fab67d743d4560ccda037d8f41301b
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66298996"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Histórico de Lançamento de Versão
A equipe do Active Directory do Azure atualiza regularmente o Azure AD Connect Health com novos recursos e funcionalidades. Este artigo lista as versões e os recursos que foram lançados.  

> [!NOTE]
> Conecte-se a integridade de agentes são atualizados automaticamente quando a nova versão for lançada. Verifique se as configurações de atualização automática está habilitada no portal do Azure. 
>

O Azure Active Directory Connect Health para sincronização é integrado com a instalação do Azure AD Connect. Leia mais sobre o [histórico de versão do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history) Para comentários sobre os recursos, vote em [canal Connnect Health User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/filters/new?category_id=165591)


## <a name="may-2019"></a>Maio de 2019
**Atualização do agente:** 
* Agente do Azure AD Connect Health para AD FS (versão 3.1.51.0) 
   1. Correção de bug para distinguir entre várias entradas que compartilham o mesmo client-request-id.
   2. Correção de bug para analisar erros de nome de usuário/senha inválidos no idioma localizado servidores.   

## <a name="april-2019"></a>Abril de 2019
**Atualização do agente:** 
* Agente do Azure AD Connect Health para AD FS (versão 3.1.46.0) 
   1. Corrigir o processo de alerta verificar SPN duplicado para ADFS

## <a name="march-2019"></a>Março de 2019
**Atualização do agente:** 
* Agente do Azure AD Connect Health para AD DS (versão 3.1.41.0)  
   1. Coleção de versão do .NET
   2. Aperfeiçoamento de coleta do contador de desempenho quando tem certas categorias
   3. Correção de bug em como evitar a geração de várias instâncias do agente de monitoramento

* Agente do Azure AD Connect Health para AD FS (versão 3.1.41.0) 
   1. Integrar e atualização de scripts de teste do AD FS usando ADFSToolBox
   2. Implementar a coleção de versão do .NET
   3. Aperfeiçoamento de coleta do contador de desempenho quando tem certas categorias
   4. Correção de bug em como evitar a geração de várias instâncias do agente de monitoramento


## <a name="november-2018"></a>Novembro de 2018
**Novos recursos de disponibilidade geral:** 
* Azure AD Connect Health para Sincronização - Diagnosticar e corrigir erros de sincronização de atributos duplicados do portal

**Atualização do agente:** 
* Agente do Azure AD Connect Health para AD DS (versão 3.1.24.0) 
   1. Conformidade e imposição da versão 1.2 do protocolo TLS
   2. Reduzir o ruído de alerta do Catálogo Global
   3. Correções de bug de registro de agente de integridade

* Agente do Azure AD Connect Health para AD FS (versão 3.1.24.0)  
   1. Conformidade e imposição da versão 1.2 do protocolo TLS
   2. Suporte do ADFSRequestToken de teste para o sistema operacional localizado
   3. Resolvido o problema de bloqueio do agente de diagnóstico EventHandler
   4. Correções de bug de registro de agente de integridade

## <a name="august-2018"></a>Agosto de 2018 
*  Agente do Azure AD Connect Health para Sincronização (versão 3.1.7.0) lançado com o Azure AD Connect versão 1.1.880.0    
   1. Hotfix para o problema de [CPU alta de monitoramento de agente com as versões do .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>Junho de 2018 
**Novos recursos de visualização:** 
* Azure AD Connect Health para Sincronização - Diagnosticar e corrigir erros de sincronização de atributos duplicados do portal 

**Atualização do agente:** 
* Agente do Azure AD Connect Health para AD DS (versão 3.1.7.0)    
  1. Hotfix para o problema de [CPU alta de monitoramento de agente com as versões do .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
* Agente do Azure AD Connect Health para AD FS (versão 3.1.7.0)  
  1. Hotfix para o problema de [CPU alta de monitoramento de agente com as versões do .NET Framework KB](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
  2. Correções de resultados de teste no servidor secundário do ADFS Server 2016
   
* Agente do Azure AD Connect Health para AD FS (versão 3.1.2.0)  
  1. Hotfix para o gerenciamento de memória do agente e alertas relacionados especificamente para a versão 3.0.244.0


## <a name="may-2018"></a>Maio de 2018
**Atualização do agente:**
* Agente do Azure AD Connect Health para AD DS (versão 3.0.244.0)
  1. Melhoria da privacidade do agente  
  2. Correções de bugs e aprimoramentos gerais

* Agente do Azure AD Connect Health para for AD FS (versão 3.0.244.0)
  1. Serviço de diagnóstico do agente e melhorias do módulo do PowerShell relacionadas
  2. Melhoria da privacidade do agente  
  3. Correções de bugs e aprimoramentos gerais

* Agente do Azure AD Connect Health para Sincronização (versão 3.0.164.0) lançado com o Azure AD Connect versão 1.1.819.0 
  1. Melhoria da privacidade do agente  
  2. Correções de bugs e aprimoramentos gerais


## <a name="march-2018"></a>Março de 2018
**Novos recursos de visualização:**
* Azure Active Directory Connect Health para AD FS - Alerta e relatório de IP arriscado.

**Atualização do agente:**

* Agente do Azure AD Connect Health para AD DS (versão 3.0.176.0)
  1. Melhorias de disponibilidade do agente 
  2. Correções de bugs e aprimoramentos gerais
* Agente do Azure AD Connect Health para AD FS (versão 3.0.176.0)
  1. Melhorias de disponibilidade do agente 
  2. Correções de bugs e aprimoramentos gerais
* Agente do Azure AD Connect Health para Sincronização (versão 3.0.129.0) lançado com o Azure AD Connect versão 1.1.750.0  
  1. Melhorias de disponibilidade do agente 
  2. Correções de bugs e aprimoramentos gerais

## <a name="december-2017"></a>Dezembro de 2017
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD DS (versão 3.0.145.0)
  1. Melhorias de disponibilidade do agente 
  2. Novos comandos de solução de problemas de agente foram adicionados
  3. Correções de bugs e aprimoramentos gerais
* Agente do Azure AD Connect Health para AD FS (versão 3.0.145.0)
  1. Novos comandos de solução de problemas de agente foram adicionados
  2. Melhorias de disponibilidade do agente 
  3. Correções de bugs e aprimoramentos gerais
  
## <a name="october-2017"></a>Outubro de 2017
**Atualização do agente:**

 * O agente do Azure AD Connect Health for Sync (versão 3.0.129.0) lançado com a versão 1.1.649.0 do Azure AD Connect
<br></br> Corrigido um problema de compatibilidade entre o Azure AD Connect e o agente do Azure AD Connect Health para Sincronização. Esse problema afeta os clientes que estão executando a atualização in-loco do Azure AD Connect para a versão 1.1.647.0, mas, atualmente, têm o Agente de Integridade versão 3.0.127.0. Após a atualização, o Agente de Integridade não poderá mais enviar dados de integridade do Azure AD Connect Synchronization Service ao Azure AD Health Service. Com essa correção, o Agente de Integridade versão 3.0.129.0 será instalado durante a atualização in-loco do Azure AD Connect. O Agente de Integridade versão 3.0.129.0 não tem problema de compatibilidade com o Azure AD Connect versão 1.1.649.0.

## <a name="july-2017"></a>Julho de 2017
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD DS (versão 3.0.68.0)
  1. Correções de bugs e aprimoramentos gerais
  2. Suporte à nuvem soberana
* Agente do Azure AD Connect Health para AD FS (versão 3.0.68.0)
  1. Correções de bugs e aprimoramentos gerais
  2. Suporte à nuvem soberana
* O agente do Azure AD Connect Health for Sync (versão 3.0.68.0) lançado com a versão 1.1.614.0 do Azure AD Connect
  1. Suporte para a Nuvem do Microsoft Azure Governamental e Microsoft Cloud Germany

## <a name="april-2017"></a>Abril de 2017      
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 3.0.12.0)
  1. Correções de bugs e aprimoramentos gerais
* Agente do Azure AD Connect Health para AD DS (versão 3.0.12.0)
  1. Aprimoramentos de upload dos contadores de desempenho
  2. Correções de bugs e aprimoramentos gerais

## <a name="october-2016"></a>Outubro de 2016
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 2.6.408.0)
* Aprimoramento da detecção de endereços IP do cliente nas solicitações de autenticação
* Correções de bugs relacionados aos alertas
* Agente do Azure AD Connect Health para AD DS (versão 2.6.408.0)
* Correções de bugs relacionadas aos alertas.
* O agente do Azure AD Connect Health for Sync (versão 2.6.353.0) lançado com a versão 1.1.281.0 do Azure AD Connect
* Fornecer os dados necessários para os relatórios de erros de sincronização
* Correções de bugs relacionados aos alertas

**Novos recursos de visualização:**

* Relatórios de erros de sincronização do Azure AD Connect

**Novos recursos:**

* Azure AD Connect Health para AD FS - o campo de endereço IP está disponível no relatório sobre os 50 principais usuários com o nome de usuário/senha inválidos.

## <a name="july-2016"></a>Julho de 2016
**Novos recursos de visualização:**

* [Azure AD Connect Health para AD DS](how-to-connect-health-adds.md).

## <a name="january-2016"></a>Janeiro de 2016
**Atualização do agente:**

* Agente do Azure AD Connect Health para AD FS (versão 2.6.91.1512)

**Novos recursos:**

* [Ferramenta de Teste de Conectividade para agentes do Azure AD Connect Health](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Novembro de 2015
**Novos recursos:**

* Suporte ao [Controle de Acesso Baseado em Função](how-to-connect-health-operations.md#manage-access-with-role-based-access-control)

**Novos recursos de visualização:**

* [Azure AD Connect Health para sincronização](how-to-connect-health-sync.md).

**Problemas corrigidos:**

* Correções de bug para erros exibidos durante os registros de agente.

## <a name="september-2015"></a>Setembro de 2015
**Novos recursos:**

* Relatório de senha de nome de usuário incorreto para o AD FS
* Suporte para configurar o Proxy HTTP Não Autenticado
* Suporte para configurar o agente no núcleo do Servidor
* Aprimoramentos em Alertas para o AD FS
* Aprimoramentos no Agente do Azure AD Connect Health Agent para AD FS em conectividade e no upload de dados.

**Problemas corrigidos:**

* Correções de bugs em informações de uso para tipos de Erro do AD FS.

## <a name="june-2015"></a>Junho de 2015
**Versão inicial do Azure AD Connect Health para AD FS e para o Proxy do AD FS.**

**Novos recursos:**

* Alertas para o monitoramento de servidores do AD FS e do Proxy do AD FS com notificações por email.
* Acesso fácil à topologia do AD FS e a padrões nos Contadores de Desempenho do AD FS.
* A tendência em solicitações bem-sucedidas de token em servidores do AD FS agrupadas por Aplicativos, Métodos de Autenticação, Local de Rede da Solicitação etc.
* Tendências em solicitações com falha em servidores do AD FS agrupadas por Aplicativos, Tipos de Erro etc.
* Implantação mais simples de agente usando credenciais de Administrador Global do AD do Azure.  

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [Monitorar a infraestrutura de identidade local e os serviços de sincronização na nuvem](whatis-hybrid-identity-health.md).

