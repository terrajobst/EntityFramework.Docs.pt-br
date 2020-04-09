---
title: Rastreamento vs. Consultas sem rastreamento - EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417645"
---
# <a name="tracking-vs-no-tracking-queries"></a>Rastreamento vs. Consultas sem rastreamento

Rastreamento de controles de comportamento se o Entity Framework Core mantiver informações sobre uma instância de entidade em seu rastreador de alterações. Se uma entidade é acompanhada, qualquer alteração detectada na entidade será mantida no banco de dados durante a `SaveChanges()`. O EF Core também corrigirá as propriedades de navegação entre as entidades em um resultado de consulta de rastreamento e as entidades que estão no rastreador de alterações.

> [!NOTE]
> [Os tipos de entidades sem chave](xref:core/modeling/keyless-entity-types) nunca são rastreados. Onde quer que este artigo mencione tipos de entidades, refere-se a tipos de entidades que têm uma chave definida.

> [!TIP]  
> Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="tracking-queries"></a>Consultas com acompanhamento

Por padrão, as consultas que retornam tipos de entidade são de acompanhamento. O que significa que você pode fazer alterações `SaveChanges()`nessas instâncias de entidade e ter essas mudanças persistidas por . No exemplo a seguir, a alteração para a classificação de blogs será detectada e persistida no banco de dados durante a `SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Consultas sem controle

As consulta sem acompanhamento são úteis quando os resultados são usados em um cenário de somente leitura. Eles são mais rápidos para executar porque não há necessidade de configurar as informações de rastreamento de alteração. Se você não precisar atualizar as entidades recuperadas do banco de dados, então uma consulta sem rastreamento deve ser usada. Você pode trocar uma consulta individual para não rastrear.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Você também pode alterar o comportamento de acompanhamento padrão no nível de instância do contexto:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Resolução de identidade

Uma vez que uma consulta de rastreamento usa o rastreador de alterações, o EF Core fará a resolução de identidade em uma consulta de rastreamento. Ao materializar uma entidade, o EF Core retornará a mesma instância da entidade do rastreador de alterações se ele já estiver sendo rastreado. Se o resultado contiver a mesma entidade várias vezes, você terá de volta a mesma instância para cada ocorrência. Consultas sem rastreamento não usam o rastreador de alterações e não fazem resolução de identidade. Assim, você recebe de volta nova instância de entidade mesmo quando a mesma entidade está contida no resultado várias vezes. Esse comportamento foi diferente nas versões anteriores ao EF Core 3.0, veja [versões anteriores](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Rastreamento e projeções personalizadas

Mesmo que o tipo de resultado da consulta não seja um tipo de entidade, o EF Core ainda rastreará os tipos de entidade contidos no resultado por padrão. Na consulta a seguir, que retorna um tipo anônimo, as instâncias do `Blog` no conjunto de resultados será rastreado.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Se o conjunto de resultados contiver tipos de entidade saem da composição LINQ, o EF Core irá rastreá-los.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Se o conjunto de resultados não contiver nenhum tipo de entidade, então nenhum rastreamento será feito. Na consulta a seguir, retornamos um tipo anônimo com alguns dos valores da entidade (mas sem instâncias do tipo entidade real). Não há entidades rastreadas saindo da consulta.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 O EF Core suporta fazer avaliação de clientes na projeção de nível superior. Se o EF Core materializar uma instância de entidade para avaliação do cliente, ela será rastreada. Aqui, já que `blog` estamos passando entidades `StandardizeURL`para o método do cliente, o EF Core também acompanhará as instâncias do blog.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

O EF Core não rastreia as instâncias de entidade sem chave contidas no resultado. Mas o EF Core rastreia todas as outras instâncias de tipos de entidades com chave de acordo com as regras acima.

Algumas das regras acima funcionaram de forma diferente antes do EF Core 3.0. Para obter mais informações, consulte [versões anteriores](#previous-versions).

## <a name="previous-versions"></a>Versões anteriores

Antes da versão 3.0, o EF Core tinha algumas diferenças na forma como o rastreamento era feito. Diferenças notáveis são as seguintes:

- Conforme explicado na página [Client vs Server Evaluation,](xref:core/querying/client-eval) o EF Core suportava a avaliação do cliente em qualquer parte da consulta antes da versão 3.0. A avaliação do cliente causou a materialização das entidades, que não fizeram parte do resultado. Então a EF Core analisou o resultado para detectar o que rastrear. Este design tinha certas diferenças da seguinte forma:
  - A avaliação do cliente na projeção, que causou a materialização, mas não retornou a instância materializada da entidade, não foi rastreada. O exemplo a seguir `blog` não rastreou entidades.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - A EF Core não rastreou os objetos que saem da composição linq em certos casos. O exemplo a seguir `Post`não foi rastreado.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Sempre que os resultados da consulta continham tipos de entidades sem chave, toda a consulta era feita sem rastreamento. Isso significa que os tipos de entidadecom chaves, que são resultado, não estavam sendo rastreados também.
- O EF Core fez resolução de identidade em consulta sem rastreamento. Utilizou referências fracas para acompanhar as entidades que já haviam sido devolvidas. Então, se um conjunto de resultados contivesse a mesma entidade várias vezes, você obteria a mesma instância para cada ocorrência. Embora se um resultado anterior com a mesma identidade saísse do escopo e coletasse lixo, a EF Core retornou uma nova instância.
