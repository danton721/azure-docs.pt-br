---
title: Como proibir o uso de senhas no AD do Azure – Azure Active Directory
description: Proibir senhas fracas do seu ambiente com senhas proibidas dinamicamente pelo Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28201e09a4025c0c8820abc6836a5923e48eb885
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742302"
---
# <a name="configuring-the-custom-banned-password-list"></a>Configurar a lista de senhas proibidas personalizada

Muitas organizações descobrem que seus usuários criam senhas usando palavras locais comuns, como uma escola, uma equipe esportiva ou uma pessoa famosa, deixando-as fáceis de adivinhar. A lista de senhas proibidas personalizada da Microsoft permite que as organizações adicionem cadeia de caracteres para avaliar e bloquear, além da lista global de senhas proibidas, quando usuários e administradores tentam alterar ou redefinir uma senha.

## <a name="add-to-the-custom-list"></a>Adicionar à lista personalizada

A configuração da lista de senhas proibidas personalizada exige uma licença do Azure Active Directory Premium P1 ou P2. Para obter mais informações sobre o licenciamento do Active Directory do Azure, consulte a [do Azure Active Directory, página de preços](https://azure.microsoft.com/pricing/details/active-directory/).

1. Entrar para o [portal do Azure](https://portal.azure.com) e navegue até **Azure Active Directory**, **métodos de autenticação**, em seguida, **proteção por senha**.
1. Defina a opção **Impor lista personalizada** para **Sim**.
1. Adicione cadeias de caracteres à **Lista de senhas proibidas personalizada**, uma cadeia de caracteres por linha
   * A lista de senhas proibidas personalizada pode conter até 1.000 palavras.
   * A lista de senhas proibidas personalizada não diferencia maiúsculas de minúsculas.
   * A lista de senhas proibidas personalizada considera a substituição de caracteres comuns.
      * Exemplo: "o" e "0" ou "a" e "\@"
   * O comprimento mínimo da cadeia de caracteres é de quatro caracteres e o máximo é de 16 caracteres.
1. Depois de adicionar todas as cadeia de caracteres, clique em **Salvar**.

> [!NOTE]
> É possível que demore várias horas até que as atualizações da lista de senhas proibidas sejam aplicadas.

![Modificar a lista de senhas proibidas personalizada em Métodos de Autenticação no portal do Azure](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Como ele funciona

Cada vez que um usuário ou administrador redefinir ou alterar uma senha do Azure AD, ela passará pelas listas de senhas proibidas para confirmar se não está em uma lista. Essa verificação está incluída em todas as senhas definidas ou alteradas usando o Azure AD.

## <a name="what-do-users-see"></a>O que os usuários veem

Quando um usuário tentar redefinir uma senha para alguma que seria proibida, a seguinte mensagem de erro será exibida:

Infelizmente, sua senha contém uma palavra, uma frase ou um padrão que pode ser facilmente adivinhado. Tente novamente com uma senha diferente.

## <a name="next-steps"></a>Próximas etapas

[Visão geral conceitual das listas de senhas proibidas](concept-password-ban-bad.md)

[Visão geral conceitual de proteção por senha do Azure AD](concept-password-ban-bad-on-premises.md)

[Habilitar integração local com as listas de senhas proibidas](howto-password-ban-bad-on-premises.md)
