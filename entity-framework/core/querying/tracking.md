---
title: Acompanhamento versus consultas sem controle-EF Core
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417645"
---
# <a name="tracking-vs-no-tracking-queries"></a>Acompanhamento versus consultas sem controle

Controlando controles de comportamento se Entity Framework Core manterá informações sobre uma instância de entidade em seu rastreador de alterações. Se uma entidade é acompanhada, qualquer alteração detectada na entidade será mantida no banco de dados durante a `SaveChanges()`. EF Core também corrigirá as propriedades de navegação entre as entidades em um resultado de consulta de rastreamento e as entidades que estão no controlador de alterações.

> [!NOTE]
> Os [tipos de entidade de subunidade](xref:core/modeling/keyless-entity-types) nunca são rastreados. Sempre que este artigo menciona tipos de entidade, ele se refere a tipos de entidade que têm uma chave definida.

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="tracking-queries"></a>Consultas com acompanhamento

Por padrão, as consultas que retornam tipos de entidade são de acompanhamento. Isso significa que você pode fazer alterações nessas instâncias de entidade e fazer com que essas alterações persistiram por `SaveChanges()`. No exemplo a seguir, a alteração para a classificação de blogs será detectada e persistida no banco de dados durante a `SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Consultas sem controle

As consulta sem acompanhamento são úteis quando os resultados são usados em um cenário de somente leitura. Eles são mais rápidos de executar porque não há necessidade de configurar as informações de controle de alterações. Se você não precisar atualizar as entidades recuperadas do banco de dados, uma consulta sem rastreamento deverá ser usada. Você pode trocar uma consulta individual como sem rastreamento.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

Você também pode alterar o comportamento de acompanhamento padrão no nível de instância do contexto:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Resolução de identidade

Como uma consulta de rastreamento usa o rastreador de alterações, EF Core fará a resolução de identidade em uma consulta de rastreamento. Ao materializar uma entidade, EF Core retornará a mesma instância de entidade do controlador de alteração se ela já estiver sendo acompanhada. Se o resultado contiver a mesma entidade várias vezes, você obterá a mesma instância para cada ocorrência. As consultas sem controle não usam o controlador de alterações e não fazem a resolução de identidade. Assim, você obtém uma nova instância da entidade, mesmo quando a mesma entidade está contida no resultado várias vezes. Esse comportamento era diferente nas versões anteriores ao EF Core 3,0, consulte [versões antigas](#previous-versions).

## <a name="tracking-and-custom-projections"></a>Acompanhamento e projeções personalizadas

Mesmo que o tipo de resultado da consulta não seja um tipo de entidade, EF Core ainda acompanhará os tipos de entidade contidos no resultado por padrão. Na consulta a seguir, que retorna um tipo anônimo, as instâncias do `Blog` no conjunto de resultados será rastreado.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Se o conjunto de resultados contiver tipos de entidade provenientes da composição do LINQ, EF Core os controlará.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Se o conjunto de resultados não contiver nenhum tipo de entidade, nenhum controle será feito. Na consulta a seguir, retornamos um tipo anônimo com alguns dos valores da entidade (mas nenhuma instância do tipo de entidade real). Não há nenhuma entidade rastreada saindo da consulta.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 O EF Core dá suporte à avaliação do cliente na projeção de nível superior. Se EF Core materializar uma instância de entidade para avaliação do cliente, ela será controlada. Aqui, como estamos passando `blog` entidades para o método de cliente `StandardizeURL`, EF Core também acompanhará as instâncias de blog.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

O EF Core não rastreia as instâncias de entidade sem o subconjunto contido no resultado. Mas EF Core rastreia todas as outras instâncias de tipos de entidade com chave de acordo com as regras acima.

Algumas das regras acima funcionaram de maneira diferente antes de EF Core 3,0. Para obter mais informações, consulte [versões anteriores](#previous-versions).

## <a name="previous-versions"></a>Versões anteriores

Antes da versão 3,0, EF Core tinha algumas diferenças em como o controle foi feito. As diferenças notáveis são as seguintes:

- Conforme explicado na página [avaliação do cliente vs Server](xref:core/querying/client-eval) , EF Core avaliação de cliente com suporte em qualquer parte da consulta antes da versão 3,0. A avaliação do cliente causou a materialização das entidades, que não faziam parte do resultado. Portanto EF Core analisado o resultado para detectar o que rastrear. Esse design tinha determinadas diferenças, da seguinte maneira:
  - Avaliação do cliente na projeção, que causou a materialização, mas não retornou que a instância de entidade materializada não foi rastreada. O exemplo a seguir não acompanha `blog` entidades.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core não rastreie os objetos provenientes da composição do LINQ em determinados casos. O exemplo a seguir não acompanha `Post`.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Sempre que os resultados da consulta contiverem tipos de entidade sem nenhum tipo, a consulta inteira foi feita sem rastreamento. Isso significa que os tipos de entidade com chaves, que, no resultado, não estavam sendo acompanhados.
- EF Core a resolução de identidade na consulta sem rastreamento. Ele usou referências fracas para manter o controle das entidades que já foram retornadas. Portanto, se um conjunto de resultados contiver a mesma entidade várias vezes, você obterá a mesma instância para cada ocorrência. No entanto, se um resultado anterior com a mesma identidade saiu do escopo e obtiver o lixo coletado, EF Core retornou uma nova instância.
