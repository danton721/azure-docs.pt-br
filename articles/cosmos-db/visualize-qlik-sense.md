---
title: Conectar o Qlik Sense ao Azure Cosmos DB e visualizar seus dados | Microsoft Docs
description: Este artigo descreve as etapas necessárias para conectar o Azure Cosmos DB ao Qlik Sense e visualizar seus dados.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: sngun
ms.openlocfilehash: e804ddec5f7aebf58352e87e483bf2bdeca0fce0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988931"
---
# <a name="connect-qlik-sense-to-azure-cosmos-db-and-visualize-your-data"></a>Conectar o Qlik Sense ao Azure Cosmos DB e visualizar seus dados

Qlik Sense é uma ferramenta de visualização de dados que combina dados de fontes diferentes em uma única exibição. O Qlik Sense indexa cada relação possível em seus dados para que você possa obter insights imediatos sobre os dados. Visualize dados do Azure Cosmos DB usando o Qlik Sense. Este artigo descreve as etapas necessárias para conectar o Azure Cosmos DB ao Qlik Sense e visualizar seus dados. 

> [!NOTE]
> No momento, a conexão do Azure Cosmos DB ao Qlik Sense só tem suporte para contas de API de SQL do Azure Cosmos DB.

Você pode conectar o Qlik Sense ao Azure Cosmos DB com:

* API de SQL do Cosmos DB usando o conector ODBC.

* API do MongoDB do Cosmos DB usando o conector do MongoDB do Qlik Sense (atualmente em versão prévia).

* API do MongoDB e a API de SQL do Cosmos DB usando o conector API REST no Qlik Sense.

* API do Mongo DB do Cosmos DB usando o conector gRPC para Qlik Core.
Este artigo descreve os detalhes da conexão com a API de SQL do Cosmos DB usando o conector ODBC.

Este artigo descreve os detalhes da conexão com a API de SQL do Cosmos DB usando o conector ODBC.

## <a name="prerequisites"></a>Pré-requisitos

Antes de seguir as instruções deste artigo, verifique se você tem os seguintes recursos:

* Faça o download do [Qlik Sense Desktop](https://www.qlik.com/us/try-or-buy/download-qlik-sense) ou configure o Qlik Sense no Azure [instalando o item do marketplace Qlik Sense](https://azuremarketplace.microsoft.com/marketplace/apps/qlik.qlik-sense).

* Faça o download dos [dados de videogame](https://www.kaggle.com/gregorut/videogamesales), esses dados de exemplo estão no formato CSV. Você armazenará esses dados em uma conta do Cosmos DB e os visualizará no Qlik Sense.

* Crie uma conta de API SQL do Azure Cosmos DB usando as etapas descritas na seção [criar uma conta](create-sql-api-dotnet.md#create-a-database-account) do artigo de início rápido.

* [Criar um banco de dados e uma coleção](create-sql-api-dotnet.md#add-a-collection) – Você pode usar o valor de taxa de transferência da coleção para 1000 RU/s. 

* Carregue os exemplos de dados de vendas de videogame em sua conta do Cosmos DB. Você pode importar os dados usando a ferramenta de migração de dados do Azure Cosmos DB, com uma [importação sequencial](import-data.md#SQLSeqTarget) ou [em massa](import-data.md#SQLBulkTarget) dos dados. A importação dos dados para a conta do Cosmos DB demora cerca de 3 a 5 minutos.

* Baixe, instale e configure o driver ODBC usando as etapas no artigo [Conectar-se ao Cosmos DB com o driver ODBC](odbc-driver.md). Os dados de videogame são um conjunto de dados simples e você não precisa editar o esquema, basta usar o esquema de mapeamento de coleção padrão.

## <a name="connect-qlik-sense-to-cosmos-db"></a>Conectar o Qlik Sense ao Cosmos DB

1. Abra o Qlik Sense e selecione **Criar novo aplicativo**. Forneça um nome para seu aplicativo e selecione **Criar**.

   ![Criar um novo aplicativo Qlik Sense](./media/visualize-qlik-sense/create-new-qlik-sense-app.png)

2. Após a criação do novo aplicativo, selecione **Abrir aplicativo** e escolha **Adicionar dados de arquivos e outras fontes**. 

3. Nas fontes de dados, selecione **ODBC** para abrir a janela de configuração de nova conexão. 

4. Mude para **DSN de Usuário** e escolha a conexão do ODBC criada anteriormente. Forneça um nome para a conexão e selecione **Criar**. 

   ![Crie uma nova conexão](./media/visualize-qlik-sense/create-new-connection.png)

5. Após a criação da conexão, você pode escolher o banco de dados, a coleção onde os dados de videogame estão localizados e visualizá-la.

   ![Escolher o banco de dados e uma coleção](./media/visualize-qlik-sense/choose-database-and-collection.png) 

6. Em seguida, selecione **Adicionar dados** para carregar os dados no Qlik Sense. Depois de carregar os dados no Qlik Sense, você pode gerar insights e realizar análise nos dados. Você pode usar os insights ou criar seu próprio aplicativo explorando as vendas de videogames. A imagem a seguir mostra 

   ![Visualizar dados](./media/visualize-qlik-sense/visualize-data.png)

### <a name="limitations-when-connecting-with-odbc"></a>Limitações durante a conexão com o ODBC 

O Cosmos DB é um banco de dados distribuído sem esquema com drivers modelados com base nas necessidades do desenvolvedor. O driver ODBC exige um banco de dados com esquema para inferir colunas, seus tipos de dados e outras propriedades. A consulta SQL regular ou a sintaxe DML com recurso relacional não é aplicável à API de SQL do Cosmos DB, pois a API de SQL não é SQL ANSI. Por esse motivo, as instruções SQL emitidas por meio do driver ODBC são convertidas em sintaxe SQL específicas ao Cosmos DB, que não têm equivalentes para todas as construções. Para evitar esses problemas de conversão, aplique um esquema ao configurar a conexão ODBC. O artigo [conectar-se com o driver ODBC](odbc-driver.md) fornece sugestões e métodos para ajudar você a configurar o esquema. Crie esse mapeamento para cada coleção/banco de dados dentro da conta do Cosmos DB.

## <a name="next-steps"></a>Próximas etapas

Se você estiver usando uma ferramenta de visualização diferente, como o Power BI, conecte-se a ela usando as instruções no documento a seguir:

* [Visualizar dados do Cosmos DB usando o conector do Power BI](powerbi-visualize.md)