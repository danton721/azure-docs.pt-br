---
title: Limites de tamanho de solicitação e listas de exclusões de firewall do aplicativo Web no Gateway de Aplicativo do Azure – Portal do Azure | Microsoft Docs
description: Este artigo fornece informações sobre limites de tamanho de solicitação e listas de exclusões de firewall de aplicativo Web no Gateway de Aplicativo com o Portal do Azure.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: victorh
ms.openlocfilehash: 995e003422d5a94fe57174dc9733c870e4e003aa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965477"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists-public-preview"></a>Limites de tamanho de solicitação e listas de exclusões de firewall do aplicativo Web (versão prévia pública)

O WAF (firewall de aplicativo Web) do Gateway de Aplicativo do Azure fornece proteção para aplicativos Web. Este artigo descreve a configuração dos limites de tamanho de solicitação e listas de exclusões de WAF.

> [!IMPORTANT]
> A configuração de limites de tamanho de solicitação e listas de exclusões de WAF está atualmente em versão prévia pública. Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Veja os [Termos de Uso Adicionais para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter detalhes.

## <a name="waf-request-size-limits"></a>Limites de tamanho de solicitação de WAF
O firewall do aplicativo Web permite aos usuários configurar limites de tamanho de solicitação dentro de limites inferior e superior. As duas configurações de limites de tamanho a seguir estão disponíveis:
- O campo de tamanho de corpo de solicitação máximo é especificado em KBs e controla o limite de tamanho de solicitação geral excluindo qualquer carregamento de arquivo. Este campo pode variar de valor mínimo de 1 KB a um máximo de 128 KB. O valor padrão para o tamanho do corpo da solicitação é 128 KB.
- O campo de limite de carregamento de arquivo é especificado em MB e controla o tamanho máximo de carregamento de arquivos permitido. Esse campo pode ter um valor mínimo de 1 MB e um máximo de 500 MB. O valor padrão para o limite de carregamento de arquivos é 100 MB.

O WAF também oferece um botão configurável para ativar ou desativar a inspeção de corpo da solicitação. Por padrão, a inspeção de corpo da solicitação está habilitada. Se a inspeção de corpo da solicitação estiver desativada, o WAF não avalia o conteúdo do corpo da mensagem HTTP. Nesses casos, o WAF continua a impor regras de WAF no URI, cookies e cabeçalhos. Se a inspeção de corpo de solicitação estiver desativada, o campo de tamanho máximo do corpo da solicitação não é aplicável e não pode ser definido. Desativar a inspeção de corpo da solicitação permite que mensagens maiores que 128 KB sejam enviadas ao WAF. No entanto, o corpo da mensagem não será inspecionado quanto a vulnerabilidades.

## <a name="waf-exclusion-lists"></a>Listas de exclusão de WAF

As listas de exclusão de WAF permitem aos usuários omitir certos atributos de solicitação de uma avaliação de WAF. Um exemplo comum são os tokens inseridos do Active Directory que são usados para autenticação ou campos de senha. Esses atributos são propensos a conter os caracteres especiais que podem disparar um falso positivo das regras de WAF. Depois que um atributo é adicionado à lista de exclusão de WAF, ele não é levado em consideração por qualquer regra de WAF configurada e ativa. As listas de exclusão são globais em escopo.
Você pode adicionar cabeçalhos de solicitação, cookies de solicitação ou argumentos de cadeia de caracteres de consulta de solicitação para as listas de exclusão de WAF. Você pode especificar um cabeçalho de solicitação, cookie ou correspondência de atributo de cadeia de caracteres de consulta exatos, ou, opcionalmente, é possível especificar correspondências parciais. A seguir estão os operadores de critérios de correspondência com suporte: 
- **Equals**: esse operador é usado para uma correspondência exata. Por exemplo, para a seleção de cabeçalho chamado **bearerToken**"** use o operador equals com seletor definido como **bearerToken**. 
- **Começa com**: esse operador corresponde com todos os campos que começam com o valor do seletor especificado. 
- **Termina com**: esse operador corresponde com todos os campos de solicitação que terminam com o valor do seletor especificado. 
- **Contém**: esse operador corresponde com todos os campos de solicitação que contenham o valor do seletor especificado.

Em todos os casos, a correspondência diferencia maiúsculas de minúsculas e a expressão regular não é permitida como seletor.

## <a name="next-steps"></a>Próximas etapas

Depois de configurar as configurações de WAF, você pode aprender como exibir os logs de WAF. Para obter mais informações, consulte [Diagnósticos do Gateway de Aplicativo](application-gateway-diagnostics.md#diagnostic-logging).