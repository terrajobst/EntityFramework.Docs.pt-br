---
title: Consultas SQL brutas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197763"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas

O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional. Isso pode ser útil se a consulta que você deseja executar não pode ser expressa usando LINQ ou se usar uma consulta LINQ resulta em uma consulta SQL ineficiente. Consultas SQL brutas podem retornar tipos de entidade regulares ou [tipos de entidade sem](xref:core/modeling/keyless-entity-types) formatação que fazem parte do seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) deste artigo no GitHub.

## <a name="basic-raw-sql-queries"></a>Consultas SQL brutas básicas

Você pode usar o `FromSqlRaw` método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passando parâmetros

> [!WARNING]
> **Sempre usar parametrização para consultas SQL brutas**
>
> Ao introduzir qualquer valor fornecido pelo usuário em uma consulta SQL bruta, deve-se ter cuidado para evitar ataques de injeção de SQL. Além de validar que esses valores não contêm caracteres inválidos, sempre use a parametrização que envia os valores separados do texto SQL.
>
> Em particular, nunca passe uma cadeia de caracteres concatenada ou interpolada (`$""`) com valores fornecidos pelo usuário não validados no ou `ExecuteSqlRaw`no `FromSqlRaw` . Os `FromSqlInterpolated` métodos `ExecuteSqlInterpolated` e permitem usar a sintaxe de interpolação de cadeia de caracteres de forma a proteger contra ataques de injeção de SQL.

O exemplo a seguir passa um único parâmetro para um procedimento armazenado, incluindo um espaço reservado de parâmetro na cadeia de caracteres de consulta SQL e fornecendo um argumento adicional. Embora isso possa parecer `String.Format` com a sintaxe, o valor fornecido é encapsulado em um `DbParameter` e o nome de parâmetro gerado `{0}` inserido onde o espaço reservado foi especificado.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Como alternativa ao `FromSqlRaw`, você pode usar `FromSqlInterpolated` o que permite o uso seguro da interpolação de cadeia de caracteres. Assim como no exemplo anterior, o valor é convertido em `DbParameter` a e, portanto, não é vulnerável à injeção de SQL:

> [!NOTE]
> Antes da versão 3,0, `FromSqlRaw` e `FromSqlInterpolated` foram duas sobrecargas nomeadas `FromSql`. Consulte a [seção versões anteriores](#previous-versions) para obter mais detalhes.


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro. Como um espaço reservado de parâmetro SQL regular é usado, em vez de um `FromSqlRaw` espaço reservado de cadeia de caracteres, pode ser usado com segurança:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Isso permite que você use parâmetros nomeados na cadeia de consulta SQL, o que é útil quando um procedimento armazenado tem parâmetros opcionais:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Como compor com o LINQ

Se a consulta SQL puder ser composta no banco de dados, é possível compor com base na consulta SQL bruta inicial usando os operadores LINQ. As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`.

O exemplo a seguir usa uma consulta SQL bruta que é selecionada de uma Função com Valor de Tabela (TVF) e compõe com base nela usando o LINQ para filtrar e classificar.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Isso produzirá a seguinte consulta SQL:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Controle de Alterações

As consultas que usam `FromSql` os métodos seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core. Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.

O exemplo a seguir usa uma consulta SQL bruta que seleciona de uma função com valor de tabela (TVF) e desabilita o controle de alterações com a chamada `AsNoTracking`para:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Como incluir dados relacionados

O método `Include` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Observe que isso requer que sua consulta SQL bruta seja combinável; em especial, não funcionará com chamadas de procedimento armazenado. Consulte as observações sobre a capacidade de composição em [limitações](#limitations)).

## <a name="limitations"></a>Limitações

Algumas limitações deve ser consideradas ao usar consultas SQL brutas:

* A consulta SQL deve retornar dados para todas as propriedades do tipo de entidade.

* Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas. Observe que isso é diferente do EF6, onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e os nomes das colunas do conjunto de resultados tinham que corresponder aos nomes das propriedades.

* A consulta SQL não pode conter dados relacionados. No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).

* Instruções `SELECT` aprovadas para este método devem normalmente ser combináveis: Se EF Core precisar avaliar operadores de consulta adicionais no servidor (por exemplo, para converter operadores LINQ aplicados após `FromSql` métodos), o SQL fornecido será tratado como uma subconsulta. Isso significa que o SQL aprovado não deve conter nenhum caractere ou opção não válida em uma subconsulta, como:
  * um ponto e vírgula à direita
  * No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)
  * No SQL Server, uma cláusula `ORDER BY` não é acompanhada de `OFFSET 0` OR `TOP 100 PERCENT` na cláusula `SELECT`

* Observe que SQL Server não permite a composição de chamadas de procedimento armazenado, portanto, qualquer tentativa de aplicar operadores de consulta adicionais a tal chamada resultará em um SQL inválido. Os operadores de consulta podem ser `AsEnumerable()` introduzidos depois para avaliação do cliente.

# <a name="previous-versions"></a>Versões anteriores

EF core versão 2,2 e anterior tinham duas sobrecargas chamadas `FromSql` que se compararam da mesma maneira que as mais `FromSqlRaw` recentes `FromSqlInterpolated`e. Isso tornou muito fácil chamar acidentalmente o método de cadeia de caracteres bruto quando a intenção era chamar o método de cadeia de caracteres interpolado e o contrário. Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.
