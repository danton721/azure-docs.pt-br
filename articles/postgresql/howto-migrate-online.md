---
title: Migração com tempo de inatividade mínimo para banco de dados do Azure para PostgreSQL – servidor único
description: Este artigo descreve como executar uma migração de tempo de inatividade mínimo de um banco de dados PostgreSQL para o banco de dados do Azure para PostgreSQL – servidor único usando o serviço de migração de banco de dados do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 93cd390889c023adf1c30a8470e1c2298598439e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067521"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Migração com tempo de inatividade mínimo para banco de dados do Azure para PostgreSQL – servidor único
É possível realizar migrações do PostgreSQL para o Banco de Dados do Azure para PostgreSQL com tempo de inatividade mínimo usando o **recurso de sincronização contínua** introduzido recentemente para [DMS](https://aka.ms/get-dms) (Serviço de Migração de Banco de Dados do Azure). Essa funcionalidade limita o tempo de inatividade incorrido pelo aplicativo.

## <a name="overview"></a>Visão geral
O DMS do Azure executa uma carga inicial do local no Banco de Dados do Azure para PostgreSQL e, em seguida, sincroniza continuamente quaisquer novas transações no Azure enquanto o aplicativo permanece em execução. Depois que os dados são atualizados no lado do Azure de destino, você interrompe o aplicativo por um breve momento (tempo de inatividade mínimo), aguarde o último lote de dados (desde o momento em que você para o aplicativo até que o aplicativo esteja efetivamente indisponível para receber qualquer novo tráfego) para atualizar no destino e, em seguida, atualize a cadeia de conexão para apontar para o Azure. Quando você terminar, o aplicativo estará ativo no Azure!

![Sincronização contínua com o Serviço de Migração de Banco de Dados do Azure](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Próximas etapas
- Exibir o vídeo [Modernização de aplicativos com o Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), que contém uma demonstração mostrando como migrar aplicativos do PostgreSQL para o Banco de Dados do Azure para PostgreSQL.
- Consulte o tutorial [Migrar o PostgreSQL para o Banco de Dados do Azure para PostgreSQL on-line usando o DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
