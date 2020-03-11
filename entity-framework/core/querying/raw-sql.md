---
title: Consultas SQL brutas – EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417666"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas

O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional. Consultas SQL brutas são úteis se a consulta que você deseja não pode ser expressa usando LINQ. As consultas SQL não processadas também são usadas se o uso de uma consulta LINQ resultar em uma consulta SQL ineficiente. Consultas SQL brutas podem retornar tipos de entidade regulares ou [tipos de entidade sem](xref:core/modeling/keyless-entity-types) formatação que fazem parte do seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) deste artigo no GitHub.

## <a name="basic-raw-sql-queries"></a>Consultas SQL brutas básicas

Você pode usar o método de extensão `FromSqlRaw` para iniciar uma consulta LINQ com base em uma consulta SQL bruta. `FromSqlRaw` só pode ser usada em raízes de consulta, que está diretamente no `DbSet<>`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Passagem de parâmetros

> [!WARNING]
> **Sempre usar parametrização para consultas SQL brutas**
>
> Ao introduzir qualquer valor fornecido pelo usuário em uma consulta SQL bruta, deve-se ter cuidado para evitar ataques de injeção de SQL. Além de validar que esses valores não contêm caracteres inválidos, sempre use a parametrização que envia os valores separados do texto SQL.
>
> Em particular, nunca passe uma cadeia de caracteres concatenada ou interpolada (`$""`) com valores fornecidos pelo usuário não validados em `FromSqlRaw` ou `ExecuteSqlRaw`. Os métodos `FromSqlInterpolated` e `ExecuteSqlInterpolated` permitem usar a sintaxe de interpolação de cadeia de caracteres de forma a proteger contra ataques de injeção de SQL.

O exemplo a seguir passa um único parâmetro para um procedimento armazenado, incluindo um espaço reservado de parâmetro na cadeia de caracteres de consulta SQL e fornecendo um argumento adicional. Embora essa sintaxe possa parecer com `String.Format` sintaxe, o valor fornecido é encapsulado em um `DbParameter` e o nome de parâmetro gerado inserido onde o espaço reservado `{0}` foi especificado.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated` é semelhante a `FromSqlRaw` mas permite que você use a sintaxe de interpolação de cadeia de caracteres. Assim como `FromSqlRaw`, `FromSqlInterpolated` só pode ser usada em raízes de consulta. Assim como no exemplo anterior, o valor é convertido em um `DbParameter` e não é vulnerável à injeção de SQL.

> [!NOTE]
> Antes da versão 3,0, `FromSqlRaw` e `FromSqlInterpolated` eram duas sobrecargas nomeadas `FromSql`. Para obter mais informações, consulte a [seção versões anteriores](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro. Como um espaço reservado de parâmetro SQL regular é usado, em vez de um espaço reservado de cadeia de caracteres, `FromSqlRaw` pode ser usada com segurança:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw` permite que você use parâmetros nomeados na cadeia de caracteres de consulta SQL, o que é útil quando um procedimento armazenado tem parâmetros opcionais:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Como compor com o LINQ

Você pode compor na parte superior da consulta SQL bruta inicial usando operadores LINQ. EF Core irá tratá-lo como subconsulta e redigir sobre ele no banco de dados. O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF). E, em seguida, compõe-o usando o LINQ para fazer filtragem e classificação.

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

A composição com o LINQ exige que sua consulta SQL bruta seja combinável, pois EF Core tratará o SQL fornecido como uma subconsulta. As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`. Além disso, o SQL Passed não deve conter nenhum caractere ou opção que não seja válida em uma subconsulta, como:

- Um ponto e vírgula à direita
- No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)
- No SQL Server, uma cláusula `ORDER BY` que não é usada com `OFFSET 0` ou `TOP 100 PERCENT` na cláusula `SELECT`

SQL Server não permite a composição de chamadas de procedimento armazenado, portanto, qualquer tentativa de aplicar operadores de consulta adicionais a tal chamada resultará em um SQL inválido. Use o método `AsEnumerable` ou `AsAsyncEnumerable` logo após `FromSqlRaw` métodos ou `FromSqlInterpolated` para garantir que o EF Core não tente compor um procedimento armazenado.

## <a name="change-tracking"></a>Controle de Alterações

As consultas que usam os métodos `FromSqlRaw` ou `FromSqlInterpolated` seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core. Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.

O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF) e desabilita o controle de alterações com a chamada para `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Limitações

Algumas limitações deve ser consideradas ao usar consultas SQL brutas:

- A consulta SQL deve retornar dados para todas as propriedades do tipo de entidade.
- Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas. Observe que esse comportamento é diferente de EF6. EF6 ignorou a propriedade para o mapeamento de coluna para consultas SQL brutas e nomes de coluna de conjunto de resultados tinham que corresponder aos nomes de propriedade.
- A consulta SQL não pode conter dados relacionados. No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).

## <a name="previous-versions"></a>Versões anteriores

EF Core versão 2,2 e anterior tinham duas sobrecargas de método chamada `FromSql`, que se compararam da mesma maneira que as `FromSqlRaw` e `FromSqlInterpolated`mais recentes. Era fácil chamar acidentalmente o método de cadeia de caracteres bruto quando a intenção era chamar o método de cadeia de caracteres interpolado e o contrário. Chamar a sobrecarga errada acidentalmente pode resultar em consultas que não estão sendo parametrizadas quando deveriam ter sido.
