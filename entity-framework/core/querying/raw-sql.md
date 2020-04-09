---
title: Consultas SQL brutas – EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417666"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas

O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional. As consultas SQL brutas são úteis se a consulta que você quiser não puder ser expressa usando LINQ. As consultas SQL brutas também são usadas se o uso de uma consulta LINQ estiver resultando em uma consulta SQL ineficiente. As consultas SQL brutas podem retornar tipos de entidades regulares ou [tipos de entidades sem chave](xref:core/modeling/keyless-entity-types) que fazem parte do seu modelo.

> [!TIP]  
> Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) deste artigo no GitHub.

## <a name="basic-raw-sql-queries"></a>Consultas SQL brutas básicas

Você pode `FromSqlRaw` usar o método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta. `FromSqlRaw`só pode ser usado em raízes de `DbSet<>`consulta, que está diretamente no .

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Passando parâmetros

> [!WARNING]
> **Sempre use parametrização para consultas SQL brutas**
>
> Ao introduzir quaisquer valores fornecidos pelo usuário em uma consulta SQL bruta, deve-se tomar cuidado para evitar ataques de injeção SQL. Além de validar que tais valores não contêm caracteres inválidos, use sempre a parametrização que envia os valores separados do texto SQL.
>
> Em particular, nunca passe uma seqüência concatenada ou interpolada (`$""` `FromSqlRaw` ) `ExecuteSqlRaw`com valores fornecidos pelo usuário não validados em ou . Os `FromSqlInterpolated` `ExecuteSqlInterpolated` métodos permitem o uso da sintaxe de interpolação de cordas de forma a proteger contra ataques de injeção SQL.

O exemplo a seguir passa um único parâmetro para um procedimento armazenado, incluindo um espaço reservado de parâmetros na seqüência de consulta SQL e fornecendo um argumento adicional. Embora essa sintaxe `String.Format` possa parecer sintaxe, `DbParameter` o valor fornecido é embrulhado `{0}` em um nome de parâmetro gerado inserido onde o espaço reservado foi especificado.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`é semelhante, `FromSqlRaw` mas permite que você use sintaxe de interpolação de seqüência. Assim `FromSqlRaw`como `FromSqlInterpolated` , só pode ser usado em raízes de consulta. Como no exemplo anterior, o valor `DbParameter` é convertido em um e não é vulnerável à injeção SQL.

> [!NOTE]
> Antes da versão 3.0, `FromSqlRaw` havia `FromSqlInterpolated` `FromSql`duas sobrecargas nomeadas . Para obter mais informações, consulte a [seção versões anteriores](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro. Uma vez que um espaço reservado de parâmetro SQL `FromSqlRaw` regular é usado, em vez de um espaço reservado de cordas, pode ser usado com segurança:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`permite que você use parâmetros nomeados na seqüência de consultas SQL, o que é útil quando um procedimento armazenado tem parâmetros opcionais:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Como compor com o LINQ

Você pode compor em cima da consulta SQL bruta inicial usando operadores LINQ. O EF Core irá tratá-lo como subquery e compor sobre ele no banco de dados. O exemplo a seguir usa uma consulta SQL bruta que seleciona a partir de uma função de valor de tabela (TVF). E então compõe sobre ele usando LINQ para fazer filtragem e classificação.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

A consulta acima gera o seguinte SQL:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Como incluir dados relacionados

O método `Include` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

A composição com LINQ requer que sua consulta SQL bruta seja composta, já que a EF Core tratará o SQL fornecido como uma subquery. As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`. Além disso, o SQL aprovado não deve conter caracteres ou opções que não sejam válidas em uma subconsulta, tais como:

- Um ponto e vírgula em arrastão
- No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)
- No SQL Server, `ORDER BY` uma cláusula que `OFFSET 0` `TOP 100 PERCENT` não `SELECT` é usada com OR na cláusula

O SQL Server não permite compor sobre chamadas de procedimento armazenadas, portanto, qualquer tentativa de aplicar operadores de consulta adicionais a tal chamada resultará em SQL inválido. Use `AsEnumerable` `AsAsyncEnumerable` ou método `FromSqlRaw` `FromSqlInterpolated` logo após ou métodos para garantir que o EF Core não tente compor sobre um procedimento armazenado.

## <a name="change-tracking"></a>Controle de Alterações

As consultas que `FromSqlRaw` `FromSqlInterpolated` usam os métodos seguem exatamente as mesmas regras de rastreamento de alteração que qualquer outra consulta LINQ no EF Core. Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.

O exemplo a seguir usa uma consulta SQL bruta que seleciona a partir de uma `AsNoTracking`TVF (Table-Valued Function, função de valor de tabela) e desativa o rastreamento de alterações com a chamada para:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Limitações

Algumas limitações deve ser consideradas ao usar consultas SQL brutas:

- A consulta SQL deve retornar dados para todas as propriedades do tipo entidade.
- Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas. Note que esse comportamento é diferente do EF6. O EF6 ignorou o mapeamento de propriedades para colunas para consultas SQL brutas e os nomes das colunas de conjunto de resultados tinham que corresponder aos nomes da propriedade.
- A consulta SQL não pode conter dados relacionados. No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).

## <a name="previous-versions"></a>Versões anteriores

A versão 2.2 do EF Core e `FromSql`anteriormente tinha duas sobrecargas de `FromSqlRaw` método `FromSqlInterpolated`denominada, que se comportavam da mesma forma que as mais novas e . Foi fácil chamar acidentalmente o método de corda bruta quando a intenção era chamar o método de cordas interpolada, e o contrário. Chamar sobrecarga errada acidentalmente pode resultar em consultas não sendo parametrizadas quando deveriam ter sido.
