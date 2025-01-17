---
title: Políticas de indexação no Azure Cosmos DB
description: Saiba como configurar e alterar a política para a indexação automática e melhor desempenho no Azure Cosmos DB de indexação padrão.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: thweiss
ms.openlocfilehash: 4206fba8297672a1a24415169cfd19ff89344038
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431190"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Políticas de indexação no Azure Cosmos DB

No Azure Cosmos DB, cada contêiner tem uma política de indexação que determina como os itens de contêiner devem ser indexados. O padrão de política para a indexação recentemente criados índices de contêineres todas as propriedades de cada item, imposição de índices de intervalo para qualquer cadeia de caracteres ou número, e índices espaciais para qualquer objeto GeoJSON de tipo de ponto. Isso permite que você obtenha o alto desempenho de consulta sem precisar pensar sobre a indexação e o gerenciamento de índice com antecedência.

Em algumas situações, talvez você queira substituir esse comportamento automático para atender melhor às suas necessidades. Você pode personalizar a política de indexação de um contêiner, definindo sua *modo de indexação*e inclua ou exclua *caminhos da propriedade*.

## <a name="indexing-mode"></a>Modo de indexação

O Azure Cosmos DB dá suporte a dois modos de indexação:

- **Consistentee**: Se a política de indexação de um contêiner é definida para consistente, o índice é atualizado de forma síncrona como criar, atualizar ou excluir itens. Isso significa que a consistência de consultas de leitura será o [consistência configurada para a conta](consistency-levels.md).

- **Nenhum**: Se a política de indexação de um contêiner é definida como None, a indexação efetivamente está desabilitada no contêiner. Isso é comumente usado quando um contêiner é usado como um repositório de chave-valor puro sem a necessidade de índices secundários. Ele também pode ajudar a acelerar a massa operações de inserção.

## <a name="including-and-excluding-property-paths"></a>Incluindo e excluindo os caminhos de propriedade

Uma política de indexação personalizada pode especificar caminhos de propriedade explicitamente incluídos ou excluídos da indexação. Otimizando o número de caminhos que são indexados, você pode reduzir a quantidade de armazenamento usada por seu contêiner e melhorar a latência de operações de gravação. Esses caminhos são definidos seguindo [o método descrito na seção de visão geral de indexação](index-overview.md#from-trees-to-property-paths) com as seguintes adições:

- um caminho que leva a um valor escalar (cadeia de caracteres ou número) termina com `/?`
- elementos de uma matriz são endereçados juntos por meio de `/[]` notação (em vez de `/0`, `/1` etc.)
- o `/*` curinga pode ser usado para corresponder a todos os elementos abaixo do nó

Levando o mesmo exemplo novamente:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

- o `headquarters`do `employees` caminho é `/headquarters/employees/?`
- o `locations`' `country` caminho é `/locations/[]/country/?`
- o caminho para qualquer coisa em `headquarters` é `/headquarters/*`

Quando um caminho for explicitamente incluído na política de indexação, ela também precisa definir quais tipos de índice devem ser aplicados para o caminho e para cada tipo de índice, o tipo de dados deste índice aplica-se a:

| Tipo de índice | Tipos de dados de destino permitidos |
| --- | --- |
| Intervalo | Cadeia de caracteres ou número |
| Espacial | Ponto, LineString ou um polígono |

Por exemplo, podemos pode incluir a `/headquarters/employees/?` caminho e especificar que um `Range` índice deve ser aplicado nesse caminho para ambos `String` e `Number` valores.

### <a name="includeexclude-strategy"></a>Incluir/excluir estratégia

Qualquer política de indexação deve incluir o caminho raiz `/*` como um incluído ou um caminho excluído.

- Inclua o caminho raiz para excluir seletivamente os caminhos que não precisam ser indexados. Isso é a abordagem recomendada, já que permite que o Azure Cosmos DB proativamente qualquer nova propriedade que pode ser adicionada ao seu modelo de índice.
- Exclua o caminho raiz para seletivamente incluir caminhos que precisam ser indexados.

- Para caminhos com caracteres normais que incluem: alfanuméricos e _ (sublinhado), você não precisa de escape a cadeia de caracteres de caminho em torno de aspas duplas (por exemplo, "/ caminho /?"). Para caminhos com outros caracteres especiais, você precisará escapar a cadeia de caracteres de caminho em torno de aspas duplas (por exemplo, "/\"caminho-abc\"/?"). Se você espera que os caracteres especiais em seu caminho, você pode escapar todos os caminhos para segurança. Funcionalmente não faz nenhuma diferença se você fazer o escape de todos os caminhos Vs apenas aqueles que têm caracteres especiais.

Ver [esta seção](how-to-manage-indexing-policy.md#indexing-policy-examples) para exemplos de política de indexação.

## <a name="composite-indexes"></a>Índices compostos

Consultas que `ORDER BY` duas ou mais propriedades exigem um índice composto. Atualmente, os índices compostos somente são utilizados pelo Multi `ORDER BY` consultas. Por padrão, não há índices compostos são definidos para que você deve [adicionar índices compostos](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) conforme necessário.

Ao definir um índice composto, especifique:

- Dois ou mais caminhos de propriedade. A sequência na qual propriedade caminhos são definidos é importante.
- A ordem (crescente ou decrescente).

As considerações a seguir são usadas ao usar índices compostos:

- Se os caminhos de índice composto não corresponderem a sequência das propriedades na cláusula ORDER BY, em seguida, o índice composto não oferece suporte a consulta

- A ordem dos caminhos de índice composto (em ordem crescente ou decrescente) também deve corresponder à ordem na cláusula ORDER BY.

- O índice composto também dá suporte a uma cláusula ORDER BY com ordem oposta em todos os caminhos.

Considere o exemplo a seguir onde um índice composto é definido nas propriedades de a, b e c:

| **Índice composto**     | **Exemplo `ORDER BY` consulta**      | **Suporte ao índice?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, b asc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

Você deve personalizar sua política de indexação para que você possa servir todas as mídias necessárias `ORDER BY` consultas.

## <a name="modifying-the-indexing-policy"></a>Modificando a política de indexação

Política de indexação do contêiner pode ser atualizada a qualquer momento [usando o portal do Azure ou um dos SDKs com suporte](how-to-manage-indexing-policy.md). Uma atualização da política de indexação aciona uma transformação do índice antigo para o novo, que é executada online e localmente (de modo que nenhum espaço de armazenamento adicional é consumido durante a operação). Índice da política antiga é transformado com eficiência para a nova política sem afetar a disponibilidade de gravação ou a taxa de transferência provisionada no contêiner. Transformação de índice é uma operação assíncrona, e o tempo necessário para concluir depende a taxa de transferência provisionada, o número de itens e seu tamanho. 

> [!NOTE]
> Enquanto a reindexação está em andamento, consultas não podem retornar todos os resultados correspondentes e fará isso sem retornar erros. Isso significa que os resultados da consulta podem não ser consistentes até que a transformação do índice seja concluída. É possível acompanhar o andamento da transformação do índice [usando um dos SDKs](how-to-manage-indexing-policy.md).

Se o modo da política de indexação nova é definido para consistente, nenhuma outra alteração de política indexação pode ser aplicada enquanto a transformação do índice está em andamento. Uma transformação de índice em execução pode ser cancelada, definindo o modo da política de indexação como Nenhum (que o descartará imediatamente o índice).

## <a name="indexing-policies-and-ttl"></a>As políticas de indexação e o TTL

O [recurso Time-to-Live (TTL)](time-to-live.md) exige a indexação para ser o Active Directory no contêiner que ele seja ativado. Isso significa que:

- não é possível ativar a TTL em um contêiner em que o modo de indexação é definido como None,
- não é possível definir o modo de indexação como nenhum em um contêiner em que o TTL é ativado.

Para cenários em que nenhum caminho de propriedade precisa ser indexados, mas o TTL é necessária, você pode usar uma política de indexação com:

- um modo de indexação definido para consistente, e
- Nenhum caminho incluído, e
- `/*` como o único caminho excluído.

## <a name="obsolete-attributes"></a>Atributos obsoletos

Ao trabalhar com as políticas de indexação, você pode encontrar os seguintes atributos agora estão obsoletos:

- `automatic` um valor booliano é definido na raiz de uma política de indexação. Ele agora é ignorado e pode ser definido como `true`, quando a ferramenta que você está usando requer a ele.
- `precision` um número é definido no nível do índice para caminhos incluídos. Ele agora é ignorado e pode ser definido como `-1`, quando a ferramenta que você está usando requer a ele.
- `hash` é um tipo de índice que está sendo substituído pelo tipo de intervalo.

## <a name="next-steps"></a>Próximas etapas

Leia mais sobre indexação nos artigos a seguir:

- [Visão geral de indexação](index-overview.md)
- [Como gerenciar a política de indexação](how-to-manage-indexing-policy.md)
