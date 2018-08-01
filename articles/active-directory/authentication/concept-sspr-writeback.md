---
title: Integração de write-back de senha local com a SSPR do Azure AD
description: Obter write-back de senhas da nuvem para infraestrutura do AD local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 78c0864a8edd8380d30cbf0fa2284e47f3217b01
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163435"
---
# <a name="what-is-password-writeback"></a>O que é write-back de senha?

Ter um utilitário de redefinição de senha baseado em nuvem é ótimo, mas a maioria das empresas ainda tem um diretório local em que os usuários existem. Como o suporte da Microsoft mantém o tradicional AD (Active Directory) local em sincronia com alterações de senha na nuvem? Write-back de senha é um recurso habilitado com [Azure AD Connect](./../connect/active-directory-aadconnect.md) que permite que as alterações de senha na nuvem possam efetuar write-back para um diretório local existente em tempo real.

Há suporte de write-back de senha em ambientes que usam:

* [Serviços de Federação do Active Directory (AD FS)](../connect/active-directory-aadconnect-federation-management.md)
* [Sincronização de hash de senha](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
* [Autenticação de passagem](/../connect/active-directory-aadconnect-pass-through-authentication.md)

O Write-back de senha fornece:

* **Imposição de políticas de senha do Active Directory local**: quando um usuário redefine a senha, será verificado se essa senha atende à política do Active Directory local antes de confirmá-la nesse diretório. Essa revisão inclui a verificação do histórico, da complexidade, do tempo, dos filtros de senha e de qualquer outra restrição de senha que você tenha definido no Active Directory local.
* **Comentários sem atraso** – o write-back de senha é uma operação síncrona. Os usuários serão notificados imediatamente se suas senhas não atenderem à política ou se não puderem ser redefinidas nem alteradas por qualquer motivo.
* **Dá suporte a alterações de senhas no painel de acesso e no Office 365**: quando usuários federados sincronizados com hash de senha alteram suas senhas expiradas ou não expiradas, o write-back dessas senhas é efetuado no ambiente do Active Directory local.
* **Dá suporte a write-back de senha quando um administrador as redefine no portal do Azure**: sempre que um administrador redefine uma senha de usuário no [portal do Azure](https://portal.azure.com), se esse usuário for federado ou sincronizado com hash de senha, o write-back de senha será efetuado no local. Essa funcionalidade não é compatível atualmente com o Portal de Administração do Office.
* **Não exige nenhuma regra de firewall de entrada**: o write-back de senha usa uma retransmissão do Barramento de Serviço do Azure como um canal de comunicação subjacente. Toda a comunicação é de saída pela porta 443.

> [!Note]
> Contas de usuários existentes em grupos protegidos no Active Directory local não podem ser usadas com o write-back de senha. Para obter mais informações sobre grupos protegidos, consulte [Contas e grupos protegidos do Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="licensing-requirements-for-password-writeback"></a>Requisitos de licenciamento do write-back de senha

**Redefinição/alteração/desbloqueio de senha de autoatendimento com write-back local é um recurso Premium do Azure AD**. Para obter mais informações sobre licenciamento, consulte o [site de preços do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

Para usar o write-back de senha, você deve ter uma das licenças a seguir atribuídas no locatário:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 ou A3
* Enterprise Mobility + Security E5 ou A5
* Microsoft 365 E3 ou A3
* Microsoft 365 E5 ou A5
* Microsoft 365 F1

> [!WARNING]
> Os planos de licenciamento do Standalone Office 365 *não dão suporte ao write-back de senha* e exigem que você tenha um dos planos anteriores para que essa funcionalidade funcione.
>

## <a name="how-password-writeback-works"></a>Como funciona o write-back de senha

Quando um usuário federado ou sincronizado com hash de senha tenta redefinir ou alterar a senha na nuvem, ocorrem as seguintes ações:

1. Uma verificação é realizada para ver qual tipo de senha o usuário tem. Se a senha do usuário for gerenciada no local:
   * Uma verificação será executada para ver se o serviço de write-back está ativo e em execução. Se estiver, o usuário poderá prosseguir.
   * Se o serviço de write-back estiver inativo, o usuário será informado de que a senha não poderá ser redefinida agora.
2. Em seguida, o usuário passa pelas entradas de autenticação pertinentes e alcança a página de **redefinição de senha**.
3. O usuário seleciona uma nova senha e a confirma.
4. Quando o usuário seleciona **Enviar**, a senha de texto não criptografado é criptografada com uma chave simétrica criada durante o processo de configuração de write-backback.
5. A senha criptografada é incluída em uma carga que é enviada por um canal HTTPS para a retransmissão de barramento de serviço específica do locatário (que é configurada para você durante o processo de configuração de write-back). Esta retransmissão é protegida por uma senha gerada aleatoriamente que somente a sua instalação local sabe.
6. Depois que a mensagem chega ao barramento de serviço, o ponto de extremidade de redefinição de senha é automaticamente reativado e vê que há uma solicitação de redefinição pendente.
7. O serviço procurará o usuário em questão usando o atributo de âncora de nuvem. Para que essa pesquisa funcione:

   * O objeto de usuário deve existir no espaço conector do Active Directory.
   * O objeto de usuário deve estar vinculado ao objeto de MV (metaverso) correspondente.
   * O objeto de usuário deve estar vinculado ao objeto de do conector do Azure Active Directory correspondente.
   * O vínculo do objeto de conector do Active Directory com o MV deve ter a regra de sincronização `Microsoft.InfromADUserAccountEnabled.xxx` no vínculo. <br> <br>
   Quando a chamada vem da nuvem, o mecanismo de sincronização usa o atributo **cloudAnchor** para procurar o objeto de espaço do conector do Azure Active Directory. Ele então segue o vínculo de volta para o objeto de MV e segue o vínculo de volta para o objeto do Active Directory. Como pode haver vários objetos do Active Directory (várias florestas) para o mesmo usuário, o mecanismo de sincronização depende do vínculo `Microsoft.InfromADUserAccountEnabled.xxx` para escolher o correto.

   > [!Note]
   > Como resultado dessa lógica, o Azure AD Connect deve conseguir se comunicar com o Emulador PDC (controlador de domínio primário). Se você precisar habilitar isso manualmente, poderá conectar o Azure AD Connect ao emulador PDC. Clique com o botão direito do mouse nas **propriedades** do conector de sincronização do Active Directory, em seguida, selecione **configurar partições de diretório**. Em seguida, procure a seção **configurações de conexão do controlador de domínio** e marque a caixa **usar somente controladores de domínio preferenciais**. Mesmo se o controlador de domínio preferencial não for um emulador PDC, o Azure AD Connect tentará se conectar ao PDC para fazer o write-back de senha.

8. Depois que a conta do usuário é localizada, será feita uma tentativa para redefinir a senha diretamente na floresta apropriada do Active Directory.
9. Se a operação de definição de senha for realizada com êxito, o usuário será informado de que a senha foi alterada.
   > [!NOTE]
   > Se o hash de senha do usuário for sincronizado com o Azure AD usando a sincronização de hash de senha, haverá uma chance da política de senha local ser mais fraca que a política de senha na nuvem. Nesse caso, a política local é imposta. Essa política garante que a política local seja imposta na nuvem, independentemente de você usar federação ou sincronização de hash de senha para fornecer logon único.

10. Se a operação de definição de senha falhar, um erro solicitará que o usuário tente novamente. A operação poderá falhar porque:
   * O serviço foi desativado.
   * A senha selecionada não atendeu às políticas da organização.
   * Não é possível localizar o usuário no Active Directory local.

    As mensagens de erro fornecem diretrizes aos usuários para que possam tentar resolver sem intervenção do administrador.

## <a name="password-writeback-security"></a>Segurança de write-back de senha

O write-back de senha é um serviço altamente seguro. Para garantir que as informações sejam protegidas, um modelo de segurança em quatro camadas é habilitado, conforme descrito a seguir:

* **Retransmissão do barramento de serviço específica ao locatário**
   * Quando você configura o serviço, é configurada uma retransmissão de barramento de serviço específica de locatário protegida por uma senha forte gerada aleatoriamente à qual a Microsoft nunca tem acesso.
* **Chave de criptografia de senha criptograficamente forte e bloqueada**
   * Após a criação da retransmissão do barramento de serviço, é criada uma chave simétrica forte utilizada para criptografar a senha à medida que ela é transmitida. Essa chave existe apenas no repositório secreto de sua empresa na nuvem, que é amplamente bloqueado e auditado, assim como qualquer outra senha no diretório.
* **Segurança de camada de transporte (TLS) padrão do setor**
   1. Quando ocorre uma redefinição de senha ou operação de alteração na nuvem, a senha de texto sem formatação é criptografada com a chave pública.
   2. A senha criptografada é colocada em uma mensagem HTTPS que é enviada por um canal criptografado usando certificados SSL da Microsoft para a retransmissão do barramento de serviço.
   3. Depois que a mensagem chega ao barramento de serviço, o agente local é ativado e se autentica no barramento de serviço usando a senha forte gerada anteriormente.
   4. O agente local recebe a mensagem criptografada e a descriptografa usando a chave privada.
   5. O agente local tenta definir a senha por meio da API SetPassword do AD DS. Essa etapa é o que permite a imposição da diretiva de senha local do Active Directory (como complexidade, idade, histórico e filtros) na nuvem.
* **Políticas de expiração de mensagem**
   * Caso a mensagem fique no barramento de serviço devido ao serviço local estar desativado, ela atingirá o tempo limite e será removida após alguns minutos. O tempo limite e a remoção da mensagem aumenta ainda mais a segurança.

### <a name="password-writeback-encryption-details"></a>Detalhes de criptografia de write-back de senha

Depois que um usuário envia uma redefinição de senha, a solicitação de redefinição passa por várias etapas de criptografia antes de chegar no seu ambiente local. Essas etapas de criptografia garantem a máxima segurança e confiabilidade do serviço. Elas são descritas da seguinte maneira:

* **Etapa 1: criptografia de senha com chave RSA de 2048 bits**: depois que um usuário envia uma senha para write back local, a senha enviada em si é criptografada com uma chave RSA de 2048 bits.
* **Etapa 2: criptografia em nível de pacote com AES-GCM**: todo o pacote (senha + metadados necessários) é criptografado usando AES-GCM. Essa criptografia impede que qualquer pessoa com acesso direto ao canal do barramento de serviço subjacente exiba ou viole o conteúdo.
* **Etapa 3: toda comunicação ocorre via TLS/SSL**: toda comunicação com o Barramento de Serviço ocorre em um canal SSL/TLS. Essa criptografia protege o conteúdo de terceiros não autorizados.
* **Substituição de chave automática a cada seis meses**: todas as chaves serão substituídas a cada seis meses ou sempre que o write-back de senha for desabilitado e, em seguida, habilitado novamente no Azure AD Connect, para garantir a máxima segurança e segurança do serviço.

### <a name="password-writeback-bandwidth-usage"></a>Uso de largura de banda de write-back de senha

O write-back de senha é um serviço de largura de banda baixa que envia solicitações de volta para o agente local somente nas seguintes circunstâncias:

* Duas mensagens são enviadas quando o recurso for habilitado ou desabilitado por meio do Azure AD Connect.
* Uma mensagem é enviada a cada cinco minutos como uma pulsação de serviço enquanto o serviço está em execução.
* Duas mensagens são enviadas sempre que uma nova senha é enviada:
   * A primeira mensagem é uma solicitação para executar a operação.
   * A segunda mensagem contém o resultado da operação e é enviada nas seguintes circunstâncias:
      * Sempre que uma nova senha é enviada durante uma redefinição de senha de autoatendimento do usuário.
      * Sempre que uma nova senha é enviada durante uma operação de alteração de senha do usuário.
      * Sempre que uma nova senha é enviada durante uma redefinição de senha do usuário iniciada pelo administrador (somente nos portais de administração do Azure).

#### <a name="message-size-and-bandwidth-considerations"></a>Considerações sobre a largura de banda e o tamanho da mensagem

O tamanho de cada mensagem descrita anteriormente normalmente é inferior a 1 KB. Mesmo sob cargas extremas, o próprio serviço de write-back de senha consome alguns quilobits por segundo de largura de banda. Como cada mensagem é enviada em tempo real, somente quando for solicitado por uma operação de atualização de senha, e como o tamanho da mensagem é bem pequeno, o uso da largura de banda da capacidade de write-back será efetivamente muito pequeno para ter qualquer impacto real mensurável.

## <a name="supported-writeback-operations"></a>Operações de write-back com suporte

É feito o write-back das senhas em todas as seguintes situações:

* **Operações do usuário final com suporte**
   * Qualquer operação de alteração de senha voluntária de autoatendimento do usuário final
   * Qualquer operação para forçar o autoatendimento de alteração de senha do usuário final, por exemplo, expiração de senha
   * Qualquer redefinição de senha de autoatendimento do usuário final originada do [portal de redefinição de senha](https://passwordreset.microsoftonline.com)
* **Operações do administrador com suporte**
   * Qualquer operação de alteração de senha voluntária de autoatendimento do administrador
   * Qualquer operação para forçar o autoatendimento de alteração de senha do administrador, por exemplo, expiração de senha
   * Qualquer redefinição de senha de autoatendimento do administrador originada do [portal de redefinição de senha](https://passwordreset.microsoftonline.com)
   * Qualquer redefinição de senha do usuário final iniciada pelo administrador no [portal do Azure](https://portal.azure.com)

## <a name="unsupported-writeback-operations"></a>Operações de write-back sem suporte

*Não* é feito o write-back das senhas em nenhuma das seguintes situações:

* **Operações do usuário final sem suporte**
   * Qualquer usuário final que redefine sua própria senha usando o PowerShell versão 1, versão 2 ou a API do Graph do Azure AD
* **Operações do administrador sem suporte**
   * Qualquer redefinição de senha do usuário final iniciada pelo administrador no [portal de gerenciamento do Office](https://portal.office.com)
   * Qualquer redefinição de senha do usuário final iniciada pelo administrador do PowerShell versão 1, versão 2 ou da API do Graph do Azure AD

## <a name="next-steps"></a>Próximas etapas

Habilitar write-back de senha usando o Tutorial: [Habilitar write-back de senha](tutorial-enable-writeback.md)