---
title: Exemplos e as definições de logs de auditoria no Azure Active Directory B2C | Microsoft Docs
description: Guia e exemplos de como acessar os logs de auditoria do Azure AD B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 27c91185bacea839ec73a3f4bd06f5df43bd4edf
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509650"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Acessando os logs de auditoria do Microsoft Azure Active Directory B2C

O Azure Active Directory B2C (Azure AD B2C) emite os logs de auditoria que contém informações de atividade sobre os recursos B2C, tokens emitidos e acesso de administrador. Este artigo fornece uma visão geral das informações disponíveis por meio de logs de auditoria e instruções sobre como acessar esses dados para seu locatário do Azure AD B2C.

> [!IMPORTANT]
> Os logs de auditoria são retidos por sete dias. Planeje realizar o download e armazenar seus logs usando um dos métodos abaixo se precisar de um período de retenção mais longo.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Visão geral das atividades disponíveis na categoria B2C dos logs de auditoria
A categoria **B2C** nos logs de auditoria contém os seguintes tipos de atividades:

|Tipo de atividade |DESCRIÇÃO  |
|---------|---------|
|Autorização |Atividades relacionadas à autorização de um usuário para acessar os recursos B2C (por exemplo, um administrador ao acessar uma lista de políticas de B2C)         |
|Diretório |Atividades relacionadas aos atributos de diretório recuperados quando um administrador faz logon usando o Portal do Azure |
|Aplicativo | Operações CRUD nos aplicativos B2C |
|Chave |Operações CRUD nas chaves armazenadas no contêiner de chave B2C |
|Resource |Operações CRUD nos recursos B2C (por exemplo, políticas e provedores de identidade)
|Authentication |Validação de credenciais de usuário e emissão de token|

> [!NOTE]
> Para atividades CRUD do objeto de usuário, consulte a categoria **Diretório Principal**.

## <a name="example-activity"></a>Atividade de exemplo
O exemplo a seguir mostra os dados capturados quando um usuário entra com um provedor de identidade externa: ![Logs de Auditoria – Exemplo](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Acessar os logs de auditoria por meio do Portal do Azure
1. Vá para o [Portal do Azure](https://portal.azure.com). Verifique se você está no diretório do B2C.
2. Clique no **Azure Active Directory** na barra de favoritas à esquerda
    
    ![Logs de auditoria - botão AAD](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Em **Atividade**, clique em **Logs de Auditoria**

    ![Logs de Auditoria - seção Logs](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. No dropox da **Categoria**, selecione **B2C**
3. Clique em **Aplicar**

    ![Logs de Auditoria - Categoria](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Você verá uma lista de atividades registradas nos últimos sete dias.
- Use o menu suspenso **Tipo de Recurso de Atividade** para filtrar os tipos de atividade descritos acima
- Use o menu suspenso **Intervalo de Datas** para filtrar o intervalo de datas das atividades mostradas
- Se você clicar em uma linha específica na lista, uma caixa contextual à direita mostra atributos adicionais associados à atividade
- Clique em **Download** para baixar as atividades como um arquivo csv

## <a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Acessar os logs de auditoria por meio da API de relatórios do Microsoft Azure Active Directory
Os logs de auditoria são publicados para o mesmo pipeline como outras atividades do Azure Active Directory, para que possam ser acessados por meio da [API de relatórios do Microsoft Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference).

### <a name="prerequisites"></a>Pré-requisitos
Para autenticar à API de relatórios do Microsoft Azure Active Directory, você precisa registrar um aplicativo. Antes de começar, você precisa concluir as etapas em [Pré-requisitos para acessar as APIs de relatórios do Microsoft Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

### <a name="accessing-the-api"></a>Como acessar a API
Para realizar o download dos logs de auditoria do Microsoft Azure Active Directory B2C por meio da API, você desejará filtrar os registros para a categoria **B2C**. Para filtrar por categoria, use o parâmetro de cadeia de caracteres de consulta ao chamar o ponto de extremidade da API de relatórios do Microsoft Azure Active Directory, conforme exibido abaixo:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

### <a name="powershell-script"></a>Script do PowerShell
O script a seguir fornece um exemplo de uso do PowerShell para consultar a API de relatórios do Microsoft Azure Active Directory e armazenar os resultados como um arquivo JSON:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago

    # loop through each query page (1 through n)
    Do {
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```
