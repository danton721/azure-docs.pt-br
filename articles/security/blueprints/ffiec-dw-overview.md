---
title: Blueprint de segurança e conformidade do Azure – data warehouse para o FFIEC
description: Blueprint de segurança e conformidade do Azure – data warehouse para o FFIEC
services: security
author: meladie
ms.assetid: eb841702-057b-4e48-8bc8-2873e155d10e
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: meladie
ms.openlocfilehash: 3ee164b013eff6664e423f3356b64744a1fa6611
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906362"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-ffiec-financial-services"></a>Blueprint de segurança e conformidade do Azure – data warehouse para serviços financeiros do FFIEC

## <a name="overview"></a>Visão geral

Este Blueprint de segurança e conformidade do Azure fornece as diretrizes para a implantação de uma arquitetura de data warehouse no Azure adequada para a coleta, o armazenamento e a recuperação de dados financeiros, regulamentada pelo FFIEC (Federal Financial Institution Examination Council).

Essa arquitetura de referência, o guia de implementação e o modelo de risco fornecem uma base para que os clientes fiquem em conformidade com os requisitos do FFIEC. Essa solução fornece uma linha de base para ajudar os clientes a implantar cargas de trabalho no Azure em conformidade com o FFIEC. No entanto, essa solução não deve ser usada no estado em que se encontra em um ambiente de produção porque é necessário realizar uma configuração adicional.

Alcançar a conformidade com FFIEC exige que auditores qualificados certifiquem uma solução de cliente de produção. As auditorias são supervisionadas por examinadores de agências membros do FFIEC, incluindo o Comitê de Administradores dos órgãos FRB (Federal Reserve System), FDIC (Federal Deposit Insurance Corporation), NCUA (National Credit Union Administration), OCC (Office of the Comptroller of the Currency) e CFPB (Consumer Financial Protection Bureau). Esses examinadores certificam que as auditorias sejam concluídas por avaliadores independentes da instituição auditada. Os clientes são responsáveis por realizar as devidas avaliações de segurança e conformidade de qualquer solução criada usando essa arquitetura, uma vez que os requisitos podem variar com base nas particularidades da implementação de cada cliente.

## <a name="architecture-diagram-and-components"></a>Diagrama e componentes da arquitetura

Essa solução fornece uma arquitetura de referência que implementa um data warehouse de nuvem seguro e de alto desempenho. Há duas camadas de dados separadas nesta arquitetura: uma em que os dados são importados, armazenados e preparados em um ambiente SQL clusterizado e outra para o SQL Data Warehouse do Azure em que os dados são carregados usando uma ferramenta de extração, transformação e carregamento (por exemplo, consultas T-SQL do [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)) para processamento. Após os dados serem armazenados no SQL Data Warehouse do Azure, você poderá executar análises em grande escala.

O Azure oferece uma variedade de serviços de relatórios e análises para o cliente. Essa solução inclui o SSRS (SQL Server Reporting Services) para a criação rápida de relatórios do SQL Data Warehouse do Azure. Todo o tráfego SQL é criptografado com SSL por meio da inclusão de certificados autoassinados. Como melhor prática, o Azure recomenda o uso de uma autoridade de certificação confiável para aumentar a segurança.

No SQL Data Warehouse do Azure, os dados são armazenados em tabelas relacionais com armazenamento em colunas, um formato que reduz significativamente os custos de armazenamento de dados enquanto melhora o desempenho das consultas. Dependendo dos requisitos de uso, os recursos de computação do SQL Data Warehouse do Azure poderão ser escalados ou reduzidos verticalmente ou desligados completamente se não houver processos ativos que demandem recursos de computação.

Um balanceador de carga SQL gerencia o tráfego SQL, garantindo o alto desempenho. Todas as máquinas virtuais nessa arquitetura de referência são implantadas em um conjunto de disponibilidade com instâncias do SQL Server configuradas em um grupo de disponibilidade Always On para funcionalidades de alta disponibilidade e de recuperação de desastre.

Essa arquitetura de referência de data warehouse também inclui uma camada do Active Directory para o gerenciamento de recursos dentro da arquitetura. A sub-rede do Active Directory permite a fácil adoção em uma estrutura maior de floresta do Active Directory, permitindo a operação contínua do ambiente mesmo quando o acesso à floresta maior não está disponível. Todas as máquinas virtuais são ingressadas no domínio na camada do Active Directory e usam as políticas de grupo do Active Directory para impor configurações de conformidade e segurança no nível do sistema operacional.

A solução usa contas de Armazenamento do Azure, que os clientes podem configurar para usar a Criptografia do Serviço de Armazenamento para manter a confidencialidade dos dados em repouso. O Azure armazena três cópias dos dados dentro do datacenter selecionado do cliente para garantir a resiliência. O armazenamento com redundância geográfica garante que os dados sejam replicados para um datacenter secundário centenas de quilômetros de distância e armazenados novamente como três cópias nesse datacenter, impedindo que um evento prejudicial resulte em perda de dados no data center principal do cliente.

Para maior segurança, todos os recursos nessa solução são gerenciados como um grupo de recursos por meio do Azure Resource Manager. O controle de acesso baseado em função do Azure Active Directory é usado para controlar o acesso aos recursos implantados, incluindo as chaves no Azure Key Vault. A integridade do sistema é monitorada por meio da Central de Segurança do Azure e do Azure Monitor. Os clientes configuram os dois serviços de monitoramento para capturar logs e exibir a integridade do sistema em um único painel facilmente navegável.

Uma máquina virtual atua como host bastião de gerenciamento, fornecendo uma conexão segura para os administradores acessarem recursos implantados. Os dados são carregados nessa área de preparo por meio do host bastião de gerenciamento. **A Microsoft recomenda configurar uma conexão VPN ou do Azure ExpressRoute para gerenciamento e importação de dados para a sub-rede da arquitetura de referência.**

![Diagrama da arquitetura de referência de data warehouse para o FFIEC](images/ffiec-dw-architecture.png "Diagrama da arquitetura de referência de data warehouse para o FFIEC")

A solução usa os serviços do Azure a seguir. Há detalhes da arquitetura de implantação na seção [Arquitetura de implantação](#deployment-architecture).

- Conjuntos de Disponibilidade
    - Controladores de Domínio do Active Directory
    - Testemunha e nós de cluster SQL
- Azure Active Directory
- Catálogo de Dados do Azure
- Cofre da Chave do Azure
- Azure Monitor
- Central de Segurança do Azure
- Azure Load Balancer
- Armazenamento do Azure
- Azure Log Analytics
- Máquinas Virtuais do Azure
    - (1) Host bastião
    - (2) Controlador de domínio do Active Directory
    - (2) Nó de cluster do SQL Server
    - (1) Testemunha do SQL Server
- Rede Virtual do Azure
    - (1) /16 rede
    - (4) /24 redes
    - (4) Grupos de segurança de rede
- Cofre dos Serviços de Recuperação
- SQL Data Warehouse
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Arquitetura de implantação

A seção a seguir fornece detalhes sobre os elementos de implantação e implementação.

**SQL Data Warehouse**: o [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) é um EDW (Enterprise Data Warehouse) que aproveita o MPP (processamento paralelo massivo) para executar rapidamente consultas complexas em petabytes de dados, permitindo que os usuários identifiquem dados financeiros de forma eficiente. Os usuários podem usar consultas T-SQL PolyBase simples para importar Big Data no SQL Data Warehouse e utilizar o poder do MPP para executar análises de alto desempenho.

**SSRS (SQL Server Reporting Services)**: o [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) fornece a criação rápida de relatórios com tabelas, gráficos, mapas, medidores e matrizes, entre outros elementos, para o SQL Data Warehouse do Azure.

**Catálogo de Dados**: o [Catálogo de Dados](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) torna fontes de dados facilmente identificáveis e compreensíveis para os usuários que gerenciam os dados. Fontes de dados comum podem ser registradas, marcadas e pesquisadas em busca de dados financeiros. Os dados permanecem no local existente, mas uma cópia de seus metadados é adicionada ao Catálogo de Dados, juntamente com uma referência ao local da fonte de dados. Os metadados também são indexados para tornar cada fonte de dados fácil de descobrir por meio de pesquisa e compreensível para os usuários que os descobrirem.

**Host bastião**: o host bastião é o único ponto de entrada que permite que os usuários acessem os recursos implantados nesse ambiente. O host bastião fornece uma conexão segura com os recursos implantados, permitindo apenas tráfego remoto de endereços IP públicos em uma lista de segurança. Para permitir o tráfego de RDP (Área de Trabalho Remota), a origem do tráfego precisa ser definida no grupo de segurança de rede.

Essa solução cria uma máquina virtual como um host bastião ingressado em domínio com as seguintes configurações:
-   [Extensão de antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensão de Diagnóstico do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) usando o Azure Key Vault
-   Uma [política de desligamento automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reduzir o consumo de recursos de máquina virtual quando eles não estiverem em uso
-   O [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) habilitado, para que as credenciais e outros segredos sejam executadas em um ambiente protegido isolado do sistema operacional em execução.

### <a name="virtual-network"></a>Rede virtual

Esta arquitetura de referência define uma rede virtual privada com um espaço de endereço de 10.0.0.0/16.

**Grupos de segurança de rede**: esses grupos contêm listas de controle de acesso que permitem ou negam tráfego dentro de uma rede virtual. Os grupos de segurança de rede podem ser usados para proteger o tráfego no nível da sub-rede ou da máquina virtual individual. Existem os seguintes grupos de segurança de rede:

  - Um grupo de segurança de rede para a camada de dados (Clusters do SQL Server, Testemunha do SQL Server e Load Balancer do SQL)
  - Um grupo de segurança de rede do host bastião de gerenciamento
  - Um grupo de segurança de rede para o Active Directory
  - Um grupo de segurança de rede para o SQL Server Reporting Services

Cada um dos grupos de segurança de rede têm portas e protocolos específicos abertos para que a solução possa funcionar corretamente e com segurança. Além disso, as configurações a seguir são habilitadas para cada grupo de segurança de rede:

- [Eventos e logs de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) são habilitados e armazenados em uma conta de armazenamento
- O Log Analytics está conectado aos [logs de diagnóstico do grupo de segurança de rede](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Sub-redes**: cada sub-rede está associada a seu grupo de segurança de rede correspondente.

### <a name="data-at-rest"></a>Dados em repouso

A arquitetura protege dados em repouso com diversas medidas, incluindo criptografia e auditoria de banco de dados.

**Armazenamento do Azure**: para atender aos requisitos de criptografia de dados em repouso, todo o [Armazenamento do Azure](https://azure.microsoft.com/services/storage/) usa [Criptografia do Serviço de Armazenamento](https://docs.microsoft.com/azure/storage/storage-service-encryption). Isso ajuda a proteger e defender os dados para dar suporte aos compromissos de segurança organizacional e aos requisitos de conformidade definidos pelo FFIEC.

**Azure Disk Encryption**: o [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aproveita o recurso BitLocker do Windows para fornecer criptografia de volume para discos de dados. A solução é integrada ao Azure Key Vault para ajudar a controlar e gerenciar as chaves de criptografia de disco.

**Banco de Dados SQL do Azure**: a instância do Banco de Dados SQL do Azure usa as seguintes medidas de segurança de banco de dados:

- A [autenticação e a autorização do Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permitem o gerenciamento de identidades dos usuários de banco de dados e de outros serviços da Microsoft em uma única localização central.
- A [auditoria do banco de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) controla os eventos de banco de dados e grava-os em um log de auditoria em uma conta de armazenamento do Azure.
- O Banco de Dados SQL do Azure está configurado para usar a [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que executa criptografia e descriptografia em tempo real do banco de dados, dos backups associados e dos arquivos de log de transações para proteger as informações em repouso. A Transparent Data Encryption oferece a garantia de que os dados armazenados não fiquem sujeitos a acesso não autorizado.
- [Regras de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impedem o acesso aos servidores do banco de dados até que as permissões apropriadas sejam concedidas. O firewall concede acesso aos bancos de dados com base no endereço IP de origem de cada solicitação.
- A [Detecção de Ameaças SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar e responder a possíveis ameaças conforme elas ocorrem, fornecendo alertas de segurança sobre atividades suspeitas no banco de dados, vulnerabilidades potenciais, ataques de injeção de SQL e padrões anormais de acesso ao banco de dados.
- As [Colunas Criptografadas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantem que os dados confidenciais nunca sejam exibidos como texto não criptografado no sistema do banco de dados. Após a habilitação da criptografia de dados, somente aplicativos cliente ou servidores de aplicativo com acesso às chaves poderão acessar dados de texto não criptografado.
- As [Propriedades Estendidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) podem ser usadas para interromper o processamento de titulares dos dados, pois permitem que os usuários adicionem propriedades personalizadas em objetos de banco de dados e marquem os dados como "Descontinuados" para dar suporte à lógica de aplicativo e evitar o processamento de dados financeiros associados.
- A [Segurança em nível de linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite aos usuários definir políticas para restringir o acesso aos dados para interromper o processamento.
- A [Máscara de Dados Dinâmicos do Banco de Dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita a exposição de dados confidenciais mascarando os dados para usuários ou aplicativos não privilegiados. A Máscara de Dados Dinâmicos pode descobrir automaticamente dados possivelmente confidenciais e sugerir as máscaras apropriadas a serem aplicadas. Isso ajuda a identificar e reduzir o acesso aos dados, para que eles não saiam do banco de dados por meio de acesso não autorizado. Os clientes são responsáveis por ajustar as configurações da Máscara de Dados Dinâmicos para seguir seus respectivos esquemas de banco de dados.

### <a name="identity-management"></a>Gerenciamento de identidades

As seguintes tecnologias oferecem funcionalidades para gerenciar o acesso a dados no ambiente do Azure:

- O [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) é o serviço de gerenciamento de identidades e diretório multilocatário baseado em nuvem da Microsoft. Todos os usuários dessa solução são criados no Azure Active Directory, incluindo os usuários que acessam o Banco de Dados SQL do Azure.
- A autenticação no aplicativo é executada usando o Azure Active Directory. Para obter mais informações, confira [Integrando aplicativos com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Além disso, a criptografia da coluna do banco de dados utiliza o Azure Active Directory para autenticar o aplicativo no Banco de Dados SQL do Microsoft Azure. Para obter mais informações, veja como [proteger dados confidenciais no Banco de Dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- O [controle de acesso baseado em função do Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite que os administradores definam permissões de acesso refinadas para conceder apenas a quantidade de acesso de que os usuários precisam para realizar seus trabalhos. Em vez de conceder permissão irrestrita aos recursos do Azure a todos os usuários, os administradores podem permitir que apenas determinadas ações acessem os dados. O acesso à assinatura é limitado ao administrador da assinatura.
- O [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite que os clientes minimizem o número de usuários que tenham acesso a determinadas informações. Os administradores podem usar o Azure Active Directory Privileged Identity Management para descobrir, restringir e monitorar identidades privilegiadas e seu acesso aos recursos. Essa funcionalidade também pode ser usada para impor o acesso administrativo sob demanda Just-In-Time quando necessário.
- O [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta possíveis vulnerabilidades que afetam as identidades de uma organização, configura respostas automatizadas para ações suspeitas detectadas relacionadas a essas identidades e investiga incidentes suspeitos para tomar as medidas apropriadas para resolvê-los.

### <a name="security"></a>Segurança

**Gerenciamento de segredos**: a solução usa o [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para gerenciar chaves e segredos. O Cofre da Chave do Azure ajuda a proteger chaves criptográficas e segredos usados por aplicativos e serviços em nuvem. As seguintes funcionalidades do Azure Key Vault ajudam os clientes a proteger e acessar esses dados:

- Políticas de acesso avançadas são configuradas com base na necessidade.
- As políticas de acesso do Key Vault são definidas com o mínimo de permissões necessárias para chaves e segredos.
- Todas as chaves e segredos no Key Vault têm datas de validade.
- Todas as chaves no Key Vault são protegidas por módulos de segurança de hardware especializados. O tipo de chave é uma Chave RSA de 2048 bits protegida por HSM.
- Todos os usuários e identidades recebem permissões mínimas necessárias usando o controle de acesso baseado em função.
- Os Logs de diagnóstico para Key Vault são habilitados com um período de retenção de pelo menos 365 dias.
- As operações criptográficas permitidas para chaves são restritas às necessárias.

**Gerenciamento de patches**: máquinas virtuais do Windows implantadas como parte dessa arquitetura de referência são configuradas por padrão para receber atualizações automáticas do serviço Windows Update. Essa solução também inclui o serviço [Automação do Azure](https://docs.microsoft.com/azure/automation/automation-intro) por meio do qual as implantações atualizadas podem ser criadas para aplicar patch em máquinas virtuais quando necessário.

**Proteção contra malware**: o [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para máquinas virtuais oferece funcionalidade de proteção em tempo real que ajuda a identificar e remover vírus, spyware e outros softwares mal-intencionados, com alertas configuráveis quando ocorrem tentativas de instalar ou executar softwares mal-intencionados ou indesejados conhecidos em máquinas virtuais protegidas.

**Central de Segurança do Azure**: com a [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro), os clientes podem aplicar e gerenciar políticas de segurança de maneira centralizada nas cargas de trabalho, limitar a exposição a ameaças, além de detectar ataques e responder a eles. Além disso, a Central de Segurança do Azure acessa as configurações existentes de serviços do Azure para fornecer recomendações de serviço e configuração, a fim de ajudar a melhorar a postura de segurança e proteger os dados.

A Central de Segurança do Azure usa uma variedade de funcionalidades de detecção para alertar os clientes de ataques potenciais direcionados a seus ambientes. Esses alertas contêm informações valiosas sobre o que disparou o alerta, os recursos de destino e a origem do ataque. A Central de Segurança do Azure tem um conjunto de [alertas de segurança predefinidos](https://docs.microsoft.com/en-us/azure/security-center/security-center-alerts-type), que são disparados em caso de ameaça ou atividade suspeita. As [regras de alerta personalizadas](https://docs.microsoft.com/en-us/azure/security-center/security-center-custom-alert) na Central de Segurança do Azure permitem que os clientes definam novos alertas de segurança com base nos dados já coletados do ambiente.

A Central de Segurança do Azure fornece alertas e incidentes de segurança priorizados, simplificando a descoberta e a resolução por parte dos clientes de possíveis problemas de segurança. Um [relatório de inteligência contra ameaças](https://docs.microsoft.com/azure/security-center/security-center-threat-report) é gerado para cada ameaça detectada, a fim de ajudar as equipes de resposta a incidentes a investigar e corrigir as ameaças.

### <a name="business-continuity"></a>Continuidade dos negócios

**Alta disponibilidade**: as cargas de trabalho de servidor são agrupadas em um [Conjunto de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ajudar a garantir a alta disponibilidade das máquinas virtuais no Azure. Pelo menos uma máquina virtual fica disponível durante um evento de manutenção planejada ou não planejada, atendendo ao SLA do Azure de 99,95%.

**Cofre dos Serviços de Recuperação**: o [Cofre dos Serviços de Recuperação](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) armazena dados de backup e protege todas as configurações das máquinas virtuais do Azure nessa arquitetura. Com um Cofre dos Serviços de Recuperação, os clientes podem restaurar arquivos e pastas por meio de uma máquina virtual de IaaS sem restaurar a máquina virtual inteira, acelerando os tempos de restauração.

### <a name="logging-and-auditing"></a>Registro em log e auditoria

Os serviços do Azure registram em log de forma extensiva as atividades do sistema e do usuário, bem como a integridade do sistema:
- **Logs de atividades:** os [Logs de atividades](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem insights sobre as operações executadas nos recursos de uma assinatura. Os logs de atividade podem ajudar a determinar o iniciador, o horário da ocorrência e o status de uma operação.
- **Logs de diagnóstico**: os [Logs de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluem todos os logs emitidos por todos os recursos. Esses logs são logs do sistema de eventos do Windows, logs de Armazenamento do Azure, logs de auditoria do Key Vault e logs de acesso e firewall do Gateway de Aplicativo. Todos os logs de diagnóstico são gravados em uma conta de armazenamento do Azure centralizada e criptografada para arquivamento. A retenção é configurável pelo usuário, de até 730 dias, para atender aos requisitos de retenção específicos da organização.

**Log Analytics**: esses logs são consolidados no [Log Analytics](https://azure.microsoft.com/services/log-analytics/) para processamento, armazenamento e relatórios de painéis. Depois de coletados, os dados são organizados em tabelas separadas para cada tipo de dados nos espaços de trabalho do Operations Management Suite, o que permite que todos os dados sejam analisados em conjunto, independentemente da fonte original. Além disso, a Central de Segurança do Azure é integrada ao Log Analytics, permitindo que os clientes usem as consultas do Log Analytics para acessar seus dados de eventos de segurança e combiná-los com os dados de outros serviços.

As seguintes [soluções de gerenciamento](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) do Log Analytics são incluídas como parte dessa arquitetura:
-   [Avaliação do Active Directory](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): a solução Verificação de Integridade do Active Directory avalia o risco e a integridade dos ambientes de servidor em intervalos regulares e fornece uma lista priorizada de recomendações específicas da infraestrutura de servidor implantada.
- [Avaliação do SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): a solução de Verificação de Integridade do SQL avalia o risco e a integridade dos ambientes de servidor em intervalos regulares e fornece aos clientes uma lista priorizada de recomendações específicas da infraestrutura do servidor implantado.
- [Integridade do Agente](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-solution-agenthealth): a solução de Integridade do Agente informa quantos agentes estão implantados e sua distribuição geográfica, bem como quantos agentes não estão respondendo e o número de agentes que estão enviando dados operacionais.
-   [Análise do Log de Atividades](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): a solução Análise do Log de Atividades ajuda com a análise dos logs de atividades do Azure em todas as assinaturas do Azure de um cliente.

**Automação do Azure**: a solução [Automação do Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) armazena, executa e gerencia runbooks. Nessa solução, os runbooks ajudam a coletar logs do Banco de Dados SQL do Azure. A solução [Controle de Alterações](https://docs.microsoft.com/azure/automation/automation-change-tracking) da Automação permite que os clientes identifiquem com facilidade as alterações no ambiente.

**Azure Monitor**: o [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ajuda os usuários a acompanhar o desempenho, manter a segurança e identificar tendências, permitindo que as organizações auditem, criem alertas e arquivem dados, incluindo o acompanhamento de chamadas à API em seus recursos do Azure.

## <a name="threat-model"></a>Modelo de ameaça

O diagrama de fluxo de dados dessa arquitetura de referência está disponível para [download](https://aka.ms/ffiec-dw-tm/) ou pode ser encontrado abaixo. Esse modelo pode ajudar os clientes a entenderem os pontos de risco em potencial na infraestrutura do sistema ao fazer modificações.

![Modelo de risco de data warehouse para o FFIEC](images/ffiec-dw-threat-model.png "Modelo de risco de data warehouse para o FFIEC")

## <a name="compliance-documentation"></a>Documentação de conformidade

O [Blueprint de segurança e conformidade do Azure – matriz de responsabilidades do cliente do FFIEC](https://aka.ms/ffiec-crm) lista todos os objetivos exigidos pelo FFIEC. Essa matriz detalha se a implementação de cada objetivo é de responsabilidade da Microsoft, do cliente ou é compartilhada pelos dois.

O [Blueprint de segurança e conformidade do Azure – matriz de implementação de data warehouse do FFIEC](https://aka.ms/ffiec-dw-cim) fornece informações sobre quais objetivos do FFIEC são atendidos pela arquitetura de data warehouse, incluindo descrições detalhadas de como a implementação atende aos requisitos de cada objetivo abordado.

## <a name="guidance-and-recommendations"></a>Diretrizes e recomendações

### <a name="vpn-and-expressroute"></a>VPN e ExpressRoute

Um túnel VPN seguro ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) precisa ser configurado para estabelecer uma conexão de forma segura com os recursos implantados como parte dessa arquitetura de referência de data warehouse. Configurando adequadamente o ExpressRoute ou uma VPN, os clientes podem adicionar uma camada de proteção para os dados em trânsito.

Implementando um túnel de VPN seguro com o Azure, é possível criar uma conexão privada virtual entre uma rede local e uma Rede Virtual do Azure. Essa conexão ocorre na Internet e permite que os clientes enviem informações por &quot;túnel&quot; com segurança usando um link criptografado entre a rede do cliente e o Azure. A VPN site a site é uma tecnologia segura e madura implantada por empresas de todos os portes há décadas. O [modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) é usado nessa opção como um mecanismo de criptografia.

Como o tráfego do túnel de VPN passa pela Internet com uma VPN site a site, a Microsoft oferece outra opção de conexão ainda mais segura. O Azure ExpressRoute é um link de WAN dedicado entre o Azure e a instalação local ou um provedor de hospedagem do Exchange. Como as conexões do ExpressRoute não ocorrem pela Internet, elas oferecem mais confiabilidade, mais velocidade, latências mais baixas e maior segurança que as conexões típicas pela Internet. Além disso, como essa é uma conexão direta do provedor de telecomunicações do cliente, os dados não passam pela Internet e, portanto, não são expostos a ela.

Estão [disponíveis](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) práticas recomendadas para a implementação de uma rede híbrida segura que estende uma rede local para o Azure.

### <a name="extract-transform-load-process"></a>Processo de extração, transformação e carregamento

O [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) pode carregar dados no SQL Data Warehouse do Azure sem precisar de uma ferramenta separada de extração, transformação e carregamento ou de importação. O PolyBase permite acessar dados por meio de consultas T-SQL. É possível usar a pilha de análise de business intelligence da Microsoft com o PolyBase, bem como ferramentas de terceiros compatíveis com o SQL Server.

### <a name="azure-active-directory-setup"></a>Configuração do Azure Active Directory

O [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é essencial para gerenciar a implantação e provisionar acesso para pessoas que interagem com o ambiente. Um Windows Server Active Directory existente pode ser integrado ao Azure Active Directory com [quatro cliques](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Os clientes também podem associar a infraestrutura implantada (os controladores de domínio) do Active Directory a um Azure Active Directory existente fazendo da infraestrutura do Active Directory implantada um subdomínio de uma floresta do Azure Active Directory.

### <a name="optional-services"></a>Serviços opcionais

O Azure oferece uma variedade de serviços para ajudar com o armazenamento e a preparação de dados formatados e não formatados. Os serviços a seguir podem ser adicionados a esta arquitetura de referência dependendo dos requisitos do cliente:
-   O [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) é um serviço de nuvem gerenciado criado para projetos híbridos complexos de extração, transformação e carregamento e de integração de dados. O Azure Data Factory tem funcionalidades para ajudar a rastrear e localizar dados financeiros, incluindo ferramentas de visualização e monitoramento para identificar quando os dados chegaram e de onde vieram. Usando o Azure Data Factory, os clientes podem criar e agendar fluxos de trabalho controlados por dados, chamados de pipelines, que ingerem dados de diferentes armazenamentos de dados. Esses pipelines permitem que os clientes a ingiram dados de fontes internas e externas. Em seguida, os clientes podem processar e transformar os dados para saída em armazenamentos de dados, como o SQL Data Warehouse do Azure.
- Os clientes podem arranjar os dados não estruturados no [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), que permite capturar dados com qualquer tamanho, tipo e velocidade de ingestão em um único lugar para análises operacionais e exploratórias.  O Azure Data Lake tem recursos que permitem a extração e a conversão de dados. O Azure Data Lake Store é compatível com a maioria dos componentes de software livre no ecossistema do Hadoop e se integra bem com outros serviços do Azure, como o SQL Data Warehouse do Azure.

## <a name="disclaimer"></a>Isenção de responsabilidade

 - Este documento serve apenas para fins informativos. A MICROSOFT NÃO FORNECE NENHUMA GARANTIA, EXPRESSA, IMPLÍCITA OU REGULAMENTAR, QUANTO ÀS INFORMAÇÕES PRESENTES NESTE DOCUMENTO. Este documento é fornecido "no estado em que se encontra". As informações e opiniões expressadas neste documento, incluindo URLs e outras referências a sites da Internet, podem ser alteradas sem aviso prévio. Os clientes que estão lendo este documento arcarão com o risco de usá-lo.
 - Este documento não fornece aos clientes nenhum direito legal a qualquer propriedade intelectual de qualquer produto ou solução da Microsoft.
 - Os clientes podem copiar e usar este documento para fins de consulta interna.
 - Determinadas recomendações neste documento podem resultar em maior uso de recursos de computação, rede ou dados no Azure e podem aumentar os custos de licença ou assinatura do cliente.
 - Essa arquitetura é destinada a servir como base para os clientes se ajustarem a seus requisitos específicos e não deve ser usada no estado em que se encontra em um ambiente de produção.
 - Este documento foi desenvolvido como uma referência e não deve ser usado para definir todos os meios pelos quais um cliente pode atender aos regulamentos e requisitos de conformidade específicos. Os clientes devem procurar suporte jurídico de suas organizações em implementações do cliente aprovadas.