---
title: Propriedades da sombra – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197706"
---
# <a name="shadow-properties"></a>Propriedades da sombra

Propriedades de sombra são propriedades que não são definidas em sua classe de entidade do .NET, mas são definidas para esse tipo de entidade no modelo de EF Core. O valor e o estado dessas propriedades são mantidos puramente no controlador de alterações.

As propriedades de sombra são úteis quando há dados no banco que não devem ser expostos nos tipos de entidade mapeada. Eles são usados com mais frequência em Propriedades de chave estrangeira, em que a relação entre duas entidades é representada por um valor de chave estrangeira no banco de dados, mas a relação é gerenciada nos tipos de entidade usando propriedades de navegação entre os tipos de entidade.

Os valores de propriedade de sombra podem ser obtidos e `ChangeTracker` alterados por meio da API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

As propriedades de sombra podem ser referenciadas em consultas `EF.Property` LINQ por meio do método estático.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenções

As propriedades de sombra podem ser criadas por convenção quando uma relação é descoberta, mas nenhuma propriedade de chave estrangeira é encontrada na classe de entidade dependente. Nesse caso, uma propriedade de chave estrangeira de sombra será introduzida. A propriedade de chave estrangeira de sombra será `<navigation property name><principal key property name>` nomeada (a navegação na entidade dependente, que aponta para a entidade principal, é usada para a nomenclatura). Se o nome da propriedade da chave principal incluir o nome da propriedade de navegação, o nome será apenas `<principal key property name>`. Se não houver nenhuma propriedade de navegação na entidade dependente, o nome do tipo de entidade de segurança será usado em seu lugar.

Por exemplo, a listagem de código a seguir resultará `BlogId` em uma propriedade de sombra sendo `Post` introduzida na entidade.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Anotações de dados

Não é possível criar propriedades de sombra com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar propriedades de sombra. Depois de ter chamado a sobrecarga de cadeia `Property` de caracteres, você pode encadear qualquer uma das chamadas de configuração para outras propriedades.

Se o nome fornecido ao `Property` método corresponder ao nome de uma propriedade existente (uma propriedade de sombra ou uma definida na classe de entidade), o código configurará essa propriedade existente em vez de introduzir uma nova propriedade de sombra.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
