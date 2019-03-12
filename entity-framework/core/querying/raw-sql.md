---
title: Consultas SQL brutas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ad7ac3099cfd4c49b88acfbbff61f2af9294b6ec
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463237"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas

O Entity Framework Core permite acessar uma lista suspensa de consultas SQL brutas ao trabalhar com um banco de dados relacional. Isso pode ser útil quando a consulta que você deseja realizar não pode ser expressa usando o LINQ ou quando o uso de uma consulta do LINQ resulta em consultas SQL ineficientes. Consultas SQL brutas podem retornar tipos de entidade ou, a partir do EF Core 2.1, [tipos de consulta](xref:core/modeling/query-types) que fazem parte do seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="basic-raw-sql-queries"></a>Consultas SQL brutas básicas

Você pode usar o método de extensão *FromSql* para iniciar uma consulta LINQ com base em uma consulta SQL bruta.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

As consultas SQL brutas podem ser usadas para executar um procedimento armazenado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passando parâmetros

Assim como acontece com qualquer API que aceita SQL, é importante parametrizar qualquer entrada do usuário para proteger contra um ataque de injeção de SQL. Você pode incluir espaços reservados de parâmetros na cadeia de caracteres de consulta SQL e fornecer os valores de parâmetros como argumentos adicionais. Qualquer valor de parâmetro fornecido será automaticamente convertido em um `DbParameter`.

O exemplo a seguir aprova um único parâmetro para um procedimento armazenado. Embora isso possa parecer a sintaxe `String.Format`, o valor fornecido é encapsulado em um parâmetro e o nome de parâmetro gerado é inserido onde o espaço reservado `{0}` foi especificado.

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

Você também pode construir um DbParameter e fornecê-lo como valor de parâmetro. Isso permite usar parâmetros nomeados na cadeia de caracteres de consulta SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Como compor com o LINQ

Se a consulta SQL puder ser composta no banco de dados, é possível compor com base na consulta SQL bruta inicial usando os operadores LINQ. As consultas SQL que podem ser compostas começam com a palavra-chave `SELECT`.

O exemplo a seguir usa uma consulta SQL bruta que é selecionada de uma Função com Valor de Tabela (TVF) e compõe com base nela usando o LINQ para filtrar e classificar.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Controle de Alterações

As consultas que usam o `FromSql()` seguem exatamente as mesmas regras de controle de alterações que qualquer outra consulta LINQ no EF Core. Por exemplo, se a consulta projetar tipos de entidade, os resultados serão controlados por padrão.  

O exemplo a seguir usa uma consulta SQL bruta que seleciona em uma TVF (função com valor de tabela) e, em seguida, desabilita o controle de alterações com a chamada para .AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Como incluir dados relacionados

O método `Include()` pode ser usado para incluir dados relacionados, assim como em qualquer outra consulta LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Limitações

Algumas limitações deve ser consideradas ao usar consultas SQL brutas:

* A consulta SQL deve retornar dados para todas as propriedades da entidade ou tipo de consulta.

* Os nomes das colunas no conjunto de resultados devem corresponder aos nomes das colunas para as quais as propriedades são mapeadas. Observe que isso é diferente do EF6, onde o mapeamento de coluna/propriedade foi ignorado para consultas SQL brutas e os nomes das colunas do conjunto de resultados tinham que corresponder aos nomes das propriedades.

* A consulta SQL não pode conter dados relacionados. No entanto, em muitos casos é possível combinar com base na consulta usando o operador `Include` para retornar dados relacionados (confira [Como incluir dados relacionados](#including-related-data)).

* Instruções `SELECT` aprovadas para este método devem normalmente ser combináveis: se o EF Core precisar avaliar operadores de consulta adicionais no servidor (por exemplo, para traduzir operadores LINQ aplicados após `FromSql`), o SQL fornecido será tratado como uma consulta aninhada. Isso significa que o SQL aprovado não deve conter nenhum caractere ou opção não válida em uma subconsulta, como:
  * um ponto-e-vírgula à direita
  * No SQL Server, uma dica a nível de consulta à direita (por exemplo, `OPTION (HASH JOIN)`)
  * No SQL Server, um cláusula `ORDER BY` não é acompanhada de `TOP 100 PERCENT` na cláusula `SELECT`

* As instruções SQL, diferentes de `SELECT`, são reconhecidas automaticamente como não combináveis. Como consequência, os resultados completos de procedimentos armazenados são sempre retornados ao cliente e os operadores LINQ aplicados após `FromSql` são avaliados na memória.

> [!WARNING]  
> **Sempre use a parametrização para consultas SQL brutas:** Além de validar a entrada do usuário, sempre use a parametrização para os valores usados em um comando/consulta SQL bruta. as APIs que aceitam uma cadeia de caracteres SQL, como `FromSql` e `ExecuteSqlCommand`, permitem que os valores sejam facilmente aprovados como parâmetros. Sobrecargas de `FromSql` e `ExecuteSqlCommand` que aceitam FormattableString também permitem usar syntaxt de interpolação da cadeia de caracteres maneira que ajude na proteção contra ataques de injeção de SQL. 
> 
> Se você estiver usando a concatenação de cadeia de caracteres ou de interpolação para criar dinamicamente qualquer parte da cadeia de caracteres de consulta ou passando a entrada do usuário para instruções ou procedimentos armazenados que podem executar essas entradas como SQL dinâmico, você será responsável por validar qualquer entrada para proteger-se contra ataques de injeção de SQL.
