---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 592e1973344b231693077f8286a41dfd67a8d188
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66689110"
---
| Resource | Limite máximo/padrão | Observação |
| --- | --- | --- |
| Gateway de Aplicativo do Azure |1.000 por assinatura | |
| Configurações de IP de front-end |2 |1 pública e 1 privada |
| Portas de front-end |100<sup>1</sup> | |
| Pools de endereços de back-end |100<sup>1</sup> | |
| Servidores de back-end por pool |1.200 | |
| Ouvintes HTTP |100<sup>1</sup> | |
| Regras de balanceamento de carga HTTP |100<sup>1</sup> | |
| Configurações de HTTP de back-end |100<sup>1</sup> | |
| Instâncias por gateway |32 | |
| Certificados SSL |100<sup>1</sup> |1 por ouvintes HTTP |
| Tamanho máximo de certificado SSL |V1 SKU - 10 KB<br>V2 SKU - 25 KB| |
| Certificados de autenticação |100 | |
| Certificados raiz confiáveis |100 | |
| Tempo limite da solicitação mínimo |1 segundo | |
| Tempo limite da solicitação máxima |24 horas | |
| Número de sites |100<sup>1</sup> |1 por ouvintes HTTP |
| Mapas de URL por ouvinte |1 | |
| Regras com base em caminho máximo por mapa de URL|100||
| Configurações de redirecionamento |100<sup>1</sup>| |
| Conexões WebSocket Simultâneas |Gateways médio 20 mil<br> Gateways grandes 50 mil| |
| Tamanho máximo da URL|8.000||
| Tamanho de carregamento de arquivo máximo padrão |2 GB | |
| Tamanho de carregamento de arquivo máximo WAF |Gateways de WAF médio, 100 MB<br>Gateways de WAF grandes, 500 MB| |
| Limite de tamanho do corpo de WAF, sem os arquivos|128 KB||
|Regras personalizadas do máximo WAF|100||

<sup>1</sup> no caso de SKUs de WAF habilitado, é recomendável que você limite o número de recursos para 40 para otimizar o desempenho.
