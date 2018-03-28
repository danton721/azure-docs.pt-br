---
title: Adicionando um Azure Active Directory usando Serviços Conectados no Visual Studio | Microsoft Docs
description: Adicionar um Active Directory do Azure usando a caixa de diálogo Adicionar Serviços Conectados do Visual Studio
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: ''
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: kraigb
ms.openlocfilehash: b21761b6fc166ecbb2fec9c13e5e207481fa9a39
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/16/2018
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Adicionando um Active Directory do Azure usando Serviços Conectados no Visual Studio

Ao usar o Azure AD (Azure Active Directory), você pode oferecer suporte ao SSO (Logon Único) para aplicativos Web MVC ASP.NET ou Autenticação do Active Directory nos serviços da API Web. Com a Autenticação do Microsoft Azure Active Directory, os usuários podem usar suas contas no Azure Active Directory para se conectar aos aplicativos Web. As vantagens da Autenticação AD do Azure com a API Web incluem segurança aprimorada de dados ao expor uma API de um aplicativo Web. Com o AD do Azure, você não precisa gerenciar um sistema de autenticação separado com a própria conta e gerenciamento de usuários.

Este artigo e os artigos complementares fornecem detalhes sobre como usar o recurso de Serviço Conectado do Visual Studio para o Active Directory. O recurso está disponível no Visual Studio de 2017 e no Visual Studio 2015.

No momento, o serviço conectado do Active Directory não dá suporte a aplicativos ASP.NET Core.

## <a name="prerequisites"></a>pré-requisitos

- Conta do Azure: se não tiver uma conta do Azure, você poderá [inscrever-se para uma avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ou [ativar seus benefícios de assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Conectar-se ao Azure Active Directory usando a caixa de diálogo Serviços Conectados

1. No Visual Studio, crie ou abra um projeto ASP.NET MVC ou um projeto de API Web ASP.NET. Você pode usar MVC, API da Web, aplicativo de página única, aplicativo de API do Azure, aplicativo móvel do Azure e modelos de serviço móvel do Azure.

1. Selecione o comando do menu **Projeto > Adicionar serviço conectado...**  ou clique duas vezes no nó **Serviços conectados** encontrado no projeto em Gerenciador de Soluções.

1. Na página **Serviços Conectados**, selecione **Autenticação com o Azure Active Directory**.

    ![Página Serviços Conectados](./media/vs-azure-active-directory/connected-services-add-active-directory.png)

1. Na página **Introdução**, selecione **Avançar**. Se você vir erros nessa página, consulte [Diagnosticando erros com o Serviço Conectado do Azure Active Directory](vs-active-directory-error.md).

    ![Página Introdução](./media/vs-azure-active-directory/configure-azure-ad-wizard-1.png)

1. Na página **Logon único**, selecione um domínio na lista suspensa **Domínio**. A lista contém todos os domínios que podem ser acessados por contas listadas na caixa de diálogo Configurações de Conta do Visual Studio (**Arquivo > Configurações de conta...**). Como alternativa, você poderá inserir um nome de domínio se não encontrar o que estiver procurando, por exemplo `mydomain.onmicrosoft.com`. Você pode escolher a opção para criar um novo aplicativo do Azure Active Directory ou usar as configurações de um aplicativo do Azure Active Directory existente. Quando terminar, escolha **Avançar**.

    ![Página Logon único](./media/vs-azure-active-directory/configure-azure-ad-wizard-2.png)

1. Na página **Acesso ao diretório**, selecione a opção **Ler dados do diretório** conforme desejado. Os desenvolvedores geralmente incluem essa opção.

    ![Página Acesso ao diretório](./media/vs-azure-active-directory/configure-azure-ad-wizard-3.png)

1. Selecione **Concluir** para iniciar as modificações em seu projeto para habilitar a autenticação do Microsoft Azure Active Directory. Durante esse tempo, o Visual Studio mostra progresso:

    ![Progresso do serviço conectado do Active Directory](./media/vs-azure-active-directory/active-directory-connected-service-output.png)

1. Quando o processo for concluído, o Visual Studio abrirá seu navegador em um dos seguintes artigos, conforme apropriado para o tipo de projeto:

    - [Introdução aos projetos do .NET MVC](vs-active-directory-dotnet-getting-started.md)
    - [Introdução aos projetos da WebAPI](vs-active-directory-webapi-getting-started.md)

1. Você também pode ver o domínio do Active Directory no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

## <a name="how-your-project-is-modified"></a>Como o projeto é modificado

Quando você adiciona o serviço conectado ao assistente, o Visual Studio adiciona o Azure Active Directory e referências associadas a seu projeto. Os arquivos de configuração e os arquivos de código em seu projeto também são modificados para adicionar suporte ao AD do Azure. As modificações específicas que o Visual Studio faz dependem do tipo de projeto. Confira os artigos a seguir para obter detalhes:

- [O que aconteceu com meu projeto do .NET MVC?](vs-active-directory-dotnet-what-happened.md)
- [O que aconteceu com meu projeto da API Web?](vs-active-directory-webapi-what-happened.md)

## <a name="next-steps"></a>Próximas etapas

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar entrada com a Microsoft para um aplicativo Web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)