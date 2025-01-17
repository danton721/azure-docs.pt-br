---
title: Habilitando o SSL de ponta a ponta no Gateway de Aplicativo Azure AD
description: Esta artigo oferece uma visão geral do suporte a SSL de ponta a ponta do Gateway de Aplicativo.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: ee901fdcae9717cc6d03d7653bcaacc0c32518e0
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66254312"
---
# <a name="overview-of-ssl-termination-and-end-to-end-ssl-with-application-gateway"></a>Visão geral de terminação SSL e SSL de ponta a ponta com o Gateway de aplicativo

Seguro SSL (Sockets Layer) é a tecnologia de segurança padrão para estabelecer um link criptografado entre um servidor web e um navegador. Esse link garante que todos os dados passados entre o servidor web e os navegadores permanecem criptografados e privada. O gateway de aplicativo dá suporte a ambos os terminação de SSL no gateway, bem como criptografia de SSL de ponta a ponta.

## <a name="ssl-termination"></a>Terminação SSL

O Gateway de Aplicativo dá suporte a terminação SSL no gateway, pelo qual o tráfego flui geralmente descriptografado até os servidores de back-end. Há uma série de vantagens de fazer a terminação SSL no gateway de aplicativo:

- **Desempenho aprimorado** – o maior impacto ao fazer a descriptografia de SSL no desempenho é o handshake inicial. Para melhorar o desempenho, o servidor que está fazendo a descriptografia armazena em cache as IDs de sessão SSL e gerencia tíquetes de sessão TLS. Se isso for feito no gateway de aplicativo, todas as solicitações do mesmo cliente podem usar os valores do cache. Se ele for feito nos servidores de back-end, em seguida, cada vez que solicitações do cliente ir para um servidor diferente, o cliente deve re‑authenticate. O uso de tíquetes TLS pode ajudar a atenuar esse problema, mas eles não têm suporte por todos os clientes e podem ser difícil de configurar e gerenciar.
- **Melhor utilização dos servidores de back-end** – processamento de SSL/TLS é muito intensivo de CPU e está se tornando mais intensivo à medida que aumentam de tamanhos de chave. Removendo este trabalho de servidores back-end permite que eles se concentrem nas quais eles são mais eficientes no fornecimento de conteúdo.
- **Roteamento inteligente** – ao descriptografar o tráfego, o gateway de aplicativo tem acesso para o conteúdo da solicitação, como cabeçalhos, o URI e assim por diante e pode usar esses dados para rotear solicitações.
- **Gerenciamento de certificado** – certificados só precisam ser adquiridos e instalados no gateway de aplicativo e nem todos os servidores de back-end. Isso economiza tempo e dinheiro.

Para configurar a terminação SSL, um certificado SSL é necessário para ser adicionado ao ouvinte para habilitar o gateway de aplicativo derivar uma chave simétrica de acordo com a especificação do protocolo SSL. A chave simétrica, em seguida, é usada para criptografar e descriptografar o tráfego enviado para o gateway. O certificado SSL precisa estar no formato de troca de informações pessoais (PFX). Esse formato de arquivo permite exportar a chave privada que é exigida pelo gateway de aplicativo para realizar a criptografia e descriptografia do tráfego.

> [!NOTE] 
>
> O gateway de aplicativo não fornece qualquer capacidade de criar um novo certificado ou enviar uma solicitação de certificado para uma autoridade de certificação.

Para a conexão SSL funcionar, você precisa verificar se o certificado SSL atende às seguintes condições:

- Que a data e hora atuais está dentro do intervalo de datas "Válido até" no certificado e "Válido de".
- o "Common Name" (CN) do certificado corresponde ao cabeçalho de host na solicitação. Por exemplo, se o cliente estiver fazendo uma solicitação para `https://www.contoso.com/`, o CN deverá ser `www.contoso.com`.

### <a name="certificates-supported-for-ssl-termination"></a>Certificados com suporte para a terminação SSL

O gateway de aplicativo suporta os seguintes tipos de certificados:

- Certificado de AC (autoridade de certificação): Um certificado de autoridade de certificação é um certificado digital emitido por uma autoridade de certificação (CA)
- Certificado EV (validação estendida): Um certificado EV é um diretrizes de certificado padrão do setor. Isso deixar a barra de localizador do navegador verde e publicar o nome da empresa também.
- Certificado curinga: Esse certificado dá suporte a qualquer número de subdomínios com base em *. site.com, onde seu subdomínio substituiria o *. No entanto, não o tiver, dar suporte site.com, portanto, no caso dos usuários estão acessando seu site sem precisar digitar o entrelinhamento "www", o certificado curinga não abrange que.
- Os certificados autoassinados: Navegadores de cliente não confiar nesses certificados e avisará o usuário que o certificado do serviço virtual não é parte de uma cadeia de confiança. Os certificados autoassinados são bons para testes ou ambientes em que os administradores controlam os clientes e podem ignorar com segurança os alertas de segurança do navegador. Cargas de trabalho de produção nunca devem usar certificados autoassinados.

Para obter mais informações, consulte [configura a terminação SSL com o gateway de aplicativo](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="size-of-the-certificate"></a>Tamanho do certificado
Verifique a [limita o Gateway de aplicativo](https://docs.microsoft.com/azure/azure-subscription-service-limits#application-gateway-limits) seção para saber o SSL máximo tamanho com suporte de certificado.

## <a name="end-to-end-ssl-encryption"></a>Criptografia SSL de ponta a ponta

Alguns clientes talvez não desejam comunicação não criptografada para os servidores de back-end. Isso pode ocorrer devido a requisitos de segurança, requisitos de conformidade ou o aplicativo pode aceitar apenas uma conexão segura. Para tais aplicativos, o gateway de aplicativo dá suporte à criptografia SSL de ponta a ponta.

O SSL de ponta a ponta permite transmitir com segurança dados confidenciais para o back-end criptografado aproveitando as vantagens dos recursos de balanceamento de carga da Camada 7 que o gateway de aplicativo fornece. Alguns desses recursos têm uma afinidade de sessão baseada em cookies, um roteamento baseado em URL, suporte para o roteamento com base em sites ou a capacidade de injetar cabeçalhos X-Forwarded-*.

Quando configurado com o modo de comunicação SSL de ponta a ponta, o gateway de aplicativo encerra as sessões SSL no gateway e descriptografa o tráfego do usuário. Ele aplica as regras configuradas para selecionar uma instância de pool de back-end apropriada para rotear o tráfego. Depois, o gateway de aplicativo inicia uma nova conexão SSL com o servidor de back-end e criptografa novamente os dados usando o certificado de chave pública do servidor de back-end antes de transmitir a solicitação ao back-end. Qualquer resposta do servidor Web passa pelo mesmo processo de volta para o usuário final. SSL de ponta a ponta é habilitada, definindo a configuração de protocolo na [configuração de HTTP de back-end](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) para HTTPS, que é então aplicado a um pool de back-end.

A política SSL se aplica ao tráfego de front-end e back-end. No front-end, o Gateway de aplicativo atua como o servidor e impõe a política. No back-end, o Gateway de aplicativo atua como o cliente e envia as informações de protocolo/codificação como a preferência durante o handshake SSL.

O gateway de aplicativo se comunique somente com essas instâncias de back-end que têm na lista de permissões seus certificados com o gateway de aplicativo ou cujos certificados assinados por autoridades de autoridade de certificação bem conhecidas em que o CN do certificado corresponde ao nome de host no HTTP configurações de back-end. Isso inclui os serviços do Azure confiáveis, como aplicativos web do serviço de aplicativo do Azure e o gerenciamento de API do Azure.

Se os certificados dos membros no pool de back-end não são assinados por autoridades de autoridade de certificação bem conhecidas, em seguida, cada instância no pool de back-end com SSL de ponta a ponta habilitado deve ser configurada com um certificado para permitir a comunicação segura. Adicionar o certificado garante que o gateway de aplicativo se comunique somente com instâncias de back-end conhecidas. Isso protege ainda mais a comunicação de ponta a ponta.

> [!NOTE] 
>
> Configuração do certificado de autenticação não é necessária para serviços do Azure confiáveis, como aplicativos web do serviço de aplicativo do Azure e o gerenciamento de API do Azure.

> [!NOTE] 
>
> O certificado adicionado ao **configuração de HTTP de back-end** para autenticar o back-end, os servidores podem ser o mesmo que o certificado adicionado para o **ouvinte** para a terminação SSL no gateway de aplicativo ou diferentes para segurança aprimorada.

![cenário do ssl de ponta a ponta][1]

Neste exemplo, as solicitações que usam o TLS1.2 são roteadas para os servidores de back-end no Pool1 usando o SSL de ponta a ponta.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>SSL de ponta a ponta e lista de exceções de certificados

O gateway de aplicativo se comunica somente com as instâncias de back-end conhecidas que têm o certificado na lista de exceções do gateway. Para habilitar a lista de exceções dos certificados, você precisa carregar a chave pública dos certificados do servidor de back-end no gateway de aplicativo (não no certificado-raiz). Somente as conexões com os back-ends conhecidos e na lista de exceções são permitidas. O back-ends restantes resultam em um erro de gateway. Os certificados autoassinados servem somente para teste e não são recomendados para cargas de trabalho de produção. Esses certificados devem estar na lista de exceções do gateway de aplicativo, conforme descrito nas etapas acima, antes de poder ser usados.

> [!NOTE]
> A configuração do certificado de autenticação não é necessária para serviços do Azure confiáveis, como aplicativos Serviço de Aplicativo do Azure.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>SSL de ponta a ponta com a v2 do SKU

Os Certificados de Autenticação foram reprovados e substituídos por Certificados raiz confiáveis no SKU do Gateway de Aplicativo v2. Eles funcionam da mesma forma para Certificados de Autenticação com algumas diferenças importantes:

- Os certificados assinados por autoridades de certificação conhecidas cujo CN corresponde ao nome do host nas configurações de back-end de HTTP não exigem etapas adicionais para que o SSL de ponta a ponta funcione. 

   Por exemplo, se os certificados de back-end forem emitidos por uma autoridade de certificação bem conhecida e tiverem um CN da contoso.com, e o campo de host da configuração http de back-end também estiver definido como contoso.com, nenhuma etapa adicional será necessária. Você pode definir o protocolo de configuração http de back-end como HTTPS e, tanto a investigação de integridade quanto o caminho de dados, serão habilitados para SSL. Se você estiver usando o serviço de aplicativo do Azure ou outros serviços da web do Azure como seu back-end, em seguida, eles também são implicitamente confiáveis e nenhuma etapa adicional é necessária para SSL de ponta a ponta.
- Se o certificado for autoassinado ou assinado por intermediários desconhecidos, para ativar o SSL de ponta a ponta na SKU v2, um certificado raiz confiável deverá ser definido. O Gateway de Aplicativo só se comunicará com back-ends cujo certificado raiz do certificado do servidor corresponda a um da lista de certificados raiz confiáveis na configuração http de back-end associada ao pool.
- Além da correspondência do certificado raiz, o Gateway de Aplicativo também validará se a configuração do Host especificada na configuração http de back-end corresponde à configuração do nome comum (CN) apresentado pelo certificado SSL do servidor de back-end. Ao tentar estabelecer uma conexão SSL com o back-end, o Gateway de Aplicativo definirá a extensão de Indicação de Nome de Servidor (SNI) para o Host especificado na configuração http de back-end.
- Se **escolher o nome do host do endereço de back-end** for escolhido em vez do campo Host na configuração http de back-end, o cabeçalho SNI será sempre definido como o FQDN do pool de back-end e o CN no certificado SSL do servidor de back-end deverá corresponder ao FQDN. Membros do pool de back-end com IPs não são suportados neste cenário.
- O certificado raiz é um certificado raiz codificado em base64 a partir dos certificados do servidor de back-end.

## <a name="next-steps"></a>Próximas etapas

Depois de aprender sobre SSL de ponta a ponta, vá para [Configurar um SSL de ponta a ponta usando o gateway de aplicativo com o PowerShell](application-gateway-end-to-end-ssl-powershell.md) para criar um Gateway de Aplicativo usando o SSL de ponta a ponta.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
