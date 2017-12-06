---
title: Consultas SQL bruto - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a>Consultas SQL bruto

Entity Framework Core permite que a lista suspensa brutas consultas SQL ao trabalhar com um banco de dados relacional. Isso pode ser útil se a consulta que você deseja executar não pode ser expressada usando LINQ, ou se usando uma consulta LINQ está resultando em ineficiente SQL sendo enviado para o banco de dados.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) no GitHub.

## <a name="limitations"></a>Limitações

Há algumas limitações a serem consideradas ao usar consultas SQL brutas:
* Consultas SQL só podem ser usadas para retornar os tipos de entidade que fazem parte do seu modelo. Há um aprimoramento na nossa lista de pendências para [habilitar retornando tipos de ad hoc de consultas SQL brutas](https://github.com/aspnet/EntityFramework/issues/1862).

* A consulta SQL deve retornar dados de todas as propriedades do tipo de entidade.

* Os nomes de coluna no conjunto de resultados devem corresponder aos nomes de coluna mapeados para propriedades. Observe que isso é diferente de EF6 onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e tinham nomes que correspondam aos nomes de propriedade de coluna do conjunto de resultados.

* A consulta SQL não pode conter dados relacionados. No entanto, em muitos casos você pode compor sobre a consulta usando o `Include` operador para retornar dados relacionados (consulte [incluindo dados relacionados](#including-related-data)).

## <a name="basic-raw-sql-queries"></a>Consultas SQL brutas básicas

Você pode usar o *FromSql* método de extensão para iniciar uma consulta LINQ com base em uma consulta SQL bruta.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Consultas SQL brutas podem ser usadas para executar um procedimento armazenado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passando parâmetros

Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL. Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e, em seguida, fornece valores de parâmetros como argumentos adicionais. Quaisquer valores de parâmetro que você fornecer serão automaticamente convertidos para um `DbParameter`.

O exemplo a seguir passa um único parâmetro para um procedimento armazenado. Enquanto isso pode parecer como `String.Format` sintaxe, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado inserida onde o `{0}` espaço reservado foi especificado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Essa é a mesma consulta, mas usando a sintaxe de interpolação de cadeia de caracteres, que tem suporte no EF Core 2.0 e posterior:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Você também pode criar um DbParameter e fornecê-lo como um valor de parâmetro. Isso permite que você use parâmetros nomeados na cadeia de caracteres de consulta SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Compor com LINQ

Se a consulta SQL pode ser composta no banco de dados, você pode compor sobre a consulta SQL bruta inicial usando operadores LINQ. Consultas SQL que podem ser compostas em ser com o `SELECT` palavra-chave.

O exemplo a seguir usa uma consulta SQL bruta que seleciona a partir de uma função com valor de tabela (TVF) e, em seguida, compõe usando LINQ para executar a filtragem e classificação.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Incluindo dados relacionados

Compondo operadores LINQ pode ser usado para incluir dados relacionados na consulta.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Sempre use a parametrização para consultas SQL brutas:** APIs que aceitam um SQL bruto da cadeia de caracteres como `FromSql` e `ExecuteSqlCommand` permitir valores facilmente ser passados como parâmetros. Além de validar a entrada do usuário, sempre use parametrização para os valores usados em um consulta SQL bruto/comando. Se você estiver usando a concatenação de cadeia de caracteres para criar dinamicamente a qualquer parte da cadeia de caracteres de consulta, você será responsável por validar qualquer entrada para proteger contra ataques de injeção de SQL.
