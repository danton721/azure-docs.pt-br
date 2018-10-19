---
title: Condição de uso em modelos do Azure Resource Manager | Microsoft Docs
description: Saiba como implantar recursos do Azure com base em condições.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/02/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 216e474f519e57352b017dc3e6bcdd74d48b03de
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238639"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Tutorial: condição de uso em modelos do Azure Resource Manager

Saiba como implantar recursos do Azure com base em condições. 

O cenário usado neste tutorial é semelhante àquele usado no [Tutorial: criar modelos do Azure Resource Manager com recursos dependentes](./resource-manager-tutorial-create-templates-with-dependent-resources.md). Neste tutorial, você criará uma conta de armazenamento, uma máquina virtual, uma rede virtual e alguns outros recursos dependentes. Em vez de criar uma nova conta de armazenamento, você permitirá que as pessoas escolham entre criar uma nova conta de armazenamento e usar uma conta de armazenamento existente. Para isso, você definirá um parâmetro adicional. Se o valor do parâmetro for “new”, uma nova conta de armazenamento será criada.

Este tutorial cobre as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de início rápido
> * Modificar o modelo
> * Implantar o modelo
> * Limpar recursos

Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, você precisa do seguinte:

* [Visual Studio Code](https://code.visualstudio.com/) com a [extensão de Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Modelos de Início Rápido do Azure é um repositório de modelos do Gerenciador de Recursos. Em vez de criar um modelo do zero, você pode encontrar um exemplo de modelo e personalizá-lo. O modelo usado neste tutorial é chamado [Implantar uma VM Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, escolha **Arquivo**>**Abrir Arquivo**.
2. Em **Nome do arquivo**, cole a seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Escolha **Abrir** para abrir o arquivo.
4. Selecione **Arquivo**>**Salvar como** para salvar uma cópia do arquivo no computador local com o nome **azuredeploy.json**.

## <a name="modify-the-template"></a>Modificar o modelo

Faça duas alterações no modelo existente:

* Adicione um parâmetro usado para fornecer um nome de conta de armazenamento. Esse parâmetro concede ao usuário a opção de especificar um nome de conta de armazenamento existente. Ele também pode ser usado como o novo nome da conta de armazenamento.
* Adicione um novo parâmetro chamado **newOrExisting**. A implantação usa esse parâmetro para determinar se uma nova conta de armazenamento será criada ou uma conta de armazenamento existente será usada.

1. Abra **azuredeploy.json** no Visual Studio Code.
2. Substitua **variables('storageAccountName')** por **parameters('storageAccountName')** em todo o modelo.  **variables('storageAccountName')** aparece três vezes.
3. Remova as declarações de variável a seguir:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Adicione os dois parâmetros a seguir ao modelo:

    ```json
    "newOrExisting": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    ```
    A definição dos parâmetros atualizados ficará assim:

    ![Condição de uso do Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Adicione a seguinte linha no início da definição da conta de armazenamento.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'yes')]",
    ```

    A condição verifica o valor de um parâmetro chamado **newOrExisting**. Se o valor do parâmetro for **new**, a implantação criará a conta de armazenamento.

    A definição da conta de armazenamento atualizada será assim:

    ![Condição de uso do Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)

6. Salve as alterações.

## <a name="deploy-the-template"></a>Implantar o modelo

Siga as instruções em [Implantar o modelo](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para implantar o modelo.

Quando você implanta o modelo usando o Azure PowerShell, é necessário especificar um parâmetro adicional:

```powershell
$resourceGroupName = "<Enter the resource group name>"
$storageAccountName = "Enter the storage account name>"
$location = "<Enter the Azure location>"
$vmAdmin = "<Enter the admin username>"
$vmPassword = "<Enter the password>"
$dnsLabelPrefix = "<Enter the prefix>"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
$vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
    -dnsLabelPrefix $dnsLabelPrefix -storageAccountName $storageAccountName -newOrExisting "new"
```

> [!NOTE]
> A implantação falhará se **newOrExisting** for **new**, mas a conta de armazenamento com o nome da conta de armazenamento especificado já existir.

Tente criar outra implantação com **newOrExisting** definido como “existing” e especifique uma conta de armazenamento existente. Para criar uma conta de armazenamento com antecedência, confira [Criar uma conta de armazenamento](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implantados excluindo o grupo de recursos.

1. No portal do Azure, escolha **Grupos de recursos** do menu à esquerda.
2. No campo **Filtrar por nome**, insira o nome do grupo de recursos.
3. Escolha o nome do grupo de recursos.  Você deverá ver um total de seis recursos no grupo de recursos.
4. Escolha **Excluir grupo de recursos** no menu superior.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você desenvolverá um modelo que permite aos usuários escolher entre criar uma nova conta de armazenamento e usar uma conta de armazenamento existente. A máquina virtual criada neste tutorial requer um nome de usuário de administrador e uma senha. Em vez de transmitir a senha durante a implantação, é possível armazenar previamente a senha usando o Azure Key Vault e recuperá-la durante a implantação. Para saber como recuperar segredos do Azure Key Vault e usá-los na implantação de modelo, confira:

> [!div class="nextstepaction"]
> [Integrar o Key Vault à implantação de modelo](./resource-manager-tutorial-use-key-vault.md)