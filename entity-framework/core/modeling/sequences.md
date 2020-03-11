---
title: Sequências-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417436"
---
# <a name="sequences"></a>Sequências

> [!NOTE]  
> As sequências são um recurso normalmente suportado apenas por bancos de dados relacionais. Se você estiver usando um banco de dados não relacional, como Cosmos, verifique a documentação do banco de dados ao gerar valores exclusivos.

Uma sequência gera valores numéricos únicos e sequenciais no banco de dados. As sequências não são associadas a uma tabela específica, e várias tabelas podem ser configuradas para desenhar valores da mesma sequência.

## <a name="basic-usage"></a>Uso básico

Você pode configurar uma sequência no modelo e, em seguida, usá-la para gerar valores para propriedades:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

Observe que o SQL específico usado para gerar um valor de uma sequência é específico ao banco de dados; o exemplo acima funciona em SQL Server, mas falhará em outros bancos de dados. Consulte a documentação do banco de dados específico para obter mais informações.

## <a name="configuring-sequence-settings"></a>Definindo configurações de sequência

Você também pode configurar aspectos adicionais da sequência, como seu esquema, valor inicial, incremento, etc.:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
