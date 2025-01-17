---
title: Conceitos de arquitetura no Azure IoT Central | Microsoft Docs
description: Este artigo apresenta os principais conceitos relacionados à arquitetura do Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 4bc9a79576c3165585a4a2c897bd41bfb77c080c
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693139"
---
# <a name="azure-iot-central-architecture"></a>Arquitetura do Azure IoT Central

Este artigo fornece uma visão geral da arquitetura do Microsoft Azure IoT Central.

![Arquitetura de nível superior](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Dispositivos

Os dispositivos trocam dados com o aplicativo Azure IoT Central. Um dispositivo pode:

- Enviar medidas, como telemetria.
- Sincronizar as configurações com o aplicativo.

No Azure IoT Central, os dados que um dispositivo pode trocar com o aplicativo são especificados em um modelo de dispositivo. Para obter mais informações sobre modelos de dispositivos, consulte [Gerenciamento de metadados](#metadata-management).

Para saber mais sobre como os dispositivos conectam ao aplicativo do Azure IoT Central, consulte [Conectividade de Dispositivo](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Gateway de nuvem

O Azure IoT Central usa o Hub IoT como um gateway de nuvem que permite a conectividade de dispositivo. O Hub IoT permite:

- Ingestão de dados em escala na nuvem.
- Gerenciamento de dispositivo.
- Conectividade de dispositivo segura.

Para saber mais sobre Hub IoT, consulte [Hub IoT](https://docs.microsoft.com/azure/iot-hub/).

Para saber mais sobre a conectividade de dispositivos no Azure IoT Central, consulte [Conectividade de dispositivo](concepts-connectivity.md).

## <a name="data-stores"></a>Armazenamentos de dados

O Azure IoT Central armazena dados de aplicativos na nuvem. Os dados armazenados de aplicativo armazenados incluem:

- Modelos de dispositivos.
- Identidades de dispositivos.
- Metadados do dispositivo.
- Dados de usuário e função.

O Azure IoT Central usa um armazenamento de série temporal para os dados de medida enviados dos dispositivos. Os dados da série temporal dos dispositivos usados pelo serviço analítico.

## <a name="analytics"></a>Análise

O serviço analítico é responsável por gerar os dados de relatório personalizados que o aplicativo exibe. Um operador pode [personalizar a análise](howto-create-analytics.md) exibida no aplicativo. O serviço analítico é compilado no [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) e processa os dados de medida enviados a partir dos dispositivos.

## <a name="rules-and-actions"></a>Regras e ações

[Regras e ações](howto-create-telemetry-rules.md) trabalham em conjunto para automatizar tarefas no aplicativo. Um construtor pode definir regras baseadas na telemetria de dispositivo, como a temperatura que excede um limite definido. O Azure IoT Central usa um processador de fluxo para determinar quando as condições da regra são atendidas. Quando uma condição de regra é atendida, ela dispara uma ação definida pelo construtor. Por exemplo, uma ação pode enviar um email para notificar um engenheiro de que a temperatura em um dispositivo está muito alta.

## <a name="metadata-management"></a>Gerenciamento de metadados

Em um aplicativo do Azure IoT Central, os modelos de dispositivo definem o comportamento e a capacidade dos tipos de dispositivo. Por exemplo, um modelo de dispositivo de refrigerador especifica a telemetria que um refrigerador envia ao aplicativo.

![Arquitetura de modelo](media/concepts-architecture/template_architecture.png)

Em um modelo de dispositivo:

- As **Medidas** especificam a telemetria que o dispositivo envia ao aplicativo.
- As **Configurações** especificam as configurações que um operador pode definir.
- As **Propriedades** especificam os metadados que um operador pode definir.
- As **Regras** automatizam o comportamento no aplicativo com base nos dados enviados de um dispositivo.
- Os **Painéis** são exibições personalizáveis de um dispositivo no aplicativo.

Um aplicativo pode ter um ou mais dispositivos simulados e reais com base em cada modelo de dispositivo.

## <a name="data-export"></a>Exportação de dados

Em um aplicativo do Azure IoT Central, você pode [exportação contínua de seus dados](howto-export-data-event-hubs-service-bus.md) para seus próprios Hubs de eventos do Azure e instâncias de barramento de serviço do Azure. Periodicamente, você pode exportar seus dados para sua conta de armazenamento de BLOBs do Azure. IoT Central pode exportar as medidas, dispositivos e modelos de dispositivo.

## <a name="batch-device-updates"></a>Atualizações de dispositivo do lote

Em um aplicativo do Azure IoT Central, você pode [criar e executar trabalhos](howto-run-a-job.md) para gerenciar dispositivos conectados. Esses trabalhos permitem que você ou em massa atualizações de propriedades do dispositivo ou configurações, execute comandos. Por exemplo, você pode criar um trabalho para aumentar a velocidade para várias máquinas de vendas refrigerated.

## <a name="role-based-access-control-rbac"></a>RBAC (Controle de Acesso Baseado em Função)

Um administrador pode [definir regras de acesso](howto-administer.md) para um aplicativo Azure IoT Central usando as funções predefinidas. Um administrador pode atribuir usuários a funções que determinam quais áreas do aplicativo o usuário tem acesso.

## <a name="security"></a>Segurança

Os recursos de segurança do Azure IoT Central incluem:

- Os dados são criptografados em trânsito e em repouso.
- A autenticação é fornecida pelo Azure Active Directory ou pela Conta Microsoft. A autenticação de dois fatores tem suporte.
- Isolamento de locatário completo.
- Segurança em nível de dispositivo.

## <a name="ui-shell"></a>Shell de interface do usuário

O shell de interface do usuário é um aplicativo dinâmico e moderno com base no navegador HTML5.
Um administrador pode personalizar a interface do usuário do aplicativo aplicando temas personalizados e modificar os links de ajuda para apontar para seus próprios recursos de ajuda personalizada. Para saber mais sobre a personalização da interface do usuário, consulte [personalizar o Azure IoT Central da interface do usuário](howto-customize-ui.md) artigo.

Um operador pode criar painéis de aplicativo personalizado. Você pode ter vários painéis que exibam dados diferentes e alternar entre elas.

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu sobre a arquitetura do Azure IoT Central, a próxima etapa sugerida é saber mais sobre [conectividade do dispositivo](concepts-connectivity.md) no Azure IoT Central.