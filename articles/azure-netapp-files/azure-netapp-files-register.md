---
title: Registrar-se no Azure NetApp Files | Microsoft Docs
description: Descreve como registrar-se para usar Azure Files do NetApp.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: b-juche
ms.openlocfilehash: 6f5d84dea2e835fd12a062b628181354295ed9f6
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299222"
---
# <a name="register-for-azure-netapp-files"></a>Registro no Azure NetApp Files

> [!IMPORTANT] 
> Antes de registrar o provedor de recursos de arquivos do Azure NetApp, você deve ter recebido um email da equipe do Azure NetApp arquivos confirmando que você recebeu acesso ao serviço. 

Neste artigo, saiba como se registrar para arquivos do Azure NetApp para que você pode começar a usar o serviço.

## <a name="waitlist"></a>Enviar uma solicitação de uma lista de espera para acessar o serviço

1. Enviar uma solicitação de uma lista de espera para acessar o serviço de arquivos do NetApp do Azure por meio de [página de envio de uma lista de espera de arquivos do Azure NetApp](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u). 

    Inscrição de uma lista de espera não garante o acesso imediato do serviço. 

2. Aguarde um email de confirmação oficial da equipe do Azure NetApp arquivos antes de prosseguir com outras tarefas. 

## <a name="resource-provider"></a>Registrar o provedor de recursos do NetApp

Para usar o serviço, é necessário registrar o Provedor de recursos do Azure no Azure NetApp Files.

> [!NOTE] 
> Você poderá registrar com êxito o provedor de recursos do NetApp, mesmo sem receber acesso para o serviço. No entanto, sem a autorização de acesso, qualquer portal do Azure ou a solicitação de API para criar uma conta do NetApp ou qualquer outro recurso de arquivos do Azure NetApp será rejeitada com o seguinte erro:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`


1. No portal do Azure, clique no ícone do Azure Cloud Shell no canto superior direito:

      ![ícone do Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Se você tiver várias assinaturas em sua conta do Azure, selecione a que foi incluída na lista de permissões para o Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. No console do Azure Cloud Shell, insira o seguinte comando para verificar se sua assinatura foi incluída na lista de permissões:
    
        az feature list | grep NetApp

   A saída do comando é exibida da seguinte maneira:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/ANFGA",  
       "name": "Microsoft.NetApp/ANFGA" 
       
   `<SubID>` é a ID da assinatura.

    Se você não vir o nome do recurso `Microsoft.NetApp/ANFGA`, você não tem acesso ao serviço. Pare nesta etapa. Siga as instruções em [enviar uma solicitação de uma lista de espera para acessar o serviço](#waitlist) para solicitar acesso de serviço antes de continuar. 

4. No console do Azure Cloud Shell, insira o seguinte comando para registrar o Provedor de recursos do Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait

   O parâmetro `--wait` instrui o console a aguardar a conclusão do registro. O processo de registro pode levar algum tempo para ser concluído.

5. No console do Azure Cloud Shell, insira o seguinte comando para verificar se o Provedor de recursos do Azure foi registrado: 
    
        az provider show --namespace Microsoft.NetApp

   A saída do comando é exibida da seguinte maneira:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   `<SubID>` é a ID da assinatura.  O valor do parâmetro `state` indica `Registered`.

6. No portal do Azure, clique na folha **Assinaturas**.
7. Na folha Assinaturas, clique na ID de assinatura. 
8. Nas configurações da assinatura, clique em **Provedores de recursos** para verificar se o Provedor do Microsoft.NetApp indica o status Registrado: 

      ![Microsoft.NetApp registrado](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Próximas etapas

[Criar uma conta do NetApp](azure-netapp-files-create-netapp-account.md)