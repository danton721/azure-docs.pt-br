---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: d965b89659a9e3dc01035221a729f094c336eb5b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244584"
---
| Entidade                                       | Convenções   |
|----------------------------------------------|-----------------------------------------------------|
| Nomes de contêiner de blob de blocos e blob de páginas | Precisa ser um nome DNS válido com tamanho de 3 a 63 caracteres. <br>  Deve começar com uma letra ou número. <br> Pode conter apenas letras minúsculas, números e o hífen (-). <br> Cada hífen (-) precisa ser imediatamente precedido e seguido por uma letra ou um número. <br> Hifens consecutivos não são permitidos em nomes. |
| Nomes de compartilhamento de arquivos do Azure                  | O mesmo que o descrito acima                                          |
| Nomes de arquivo e diretório para arquivos do Azure     |<li> Uso de maiúsculas e minúsculas preservado, sem diferenciação. Não deve exceder 255 caracteres. </li><li> Não pode terminar com barra (/). </li><li>Se fornecido, será removido automaticamente. </li><li> Caracteres a seguir não são permitidos: <code>" \\ / : \| < > * ?</code></li><li> Os caracteres reservados de URL precisam ser escapados corretamente. </li><li> Caracteres de caminho de URL ilegal não são permitidas. Pontos de código como \\uE000 não são caracteres Unicode válidos. Alguns caracteres ASCII ou Unicode, como caracteres de controle (0x00 a 0x1F, \\u0081, etc.), também não são permitidos. Para regras que regem cadeia de caracteres Unicode no HTTP/1.1, consulte RFC 2616, Seção 2.2: Regras Básicas e RFC 3987. </li><li> Nomes de arquivo seguinte não são permitidos: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, caractere de ponto (.) e dois caracteres de ponto (..).</li>|
| Nomes de blob para blobs de blocos e blobs de página      | </li><li>Nomes de blob diferenciam maiúsculas de minúsculas e podem conter qualquer combinação de caracteres. </li><li>O nome do blob deve ter entre 1 e 1.024 caracteres. </li><li>Os caracteres reservados de URL precisam ser escapados corretamente. </li><li>O número de segmentos de caminho que incluem o nome do blob não pode exceder 254. Um segmento de caminho é a cadeia de caracteres entre caracteres delimitadores consecutivos (por exemplo, a barra '/') que correspondem ao nome de um diretório virtual.</li> |
