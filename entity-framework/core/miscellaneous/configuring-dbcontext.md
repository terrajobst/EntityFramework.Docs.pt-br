---
title: Configurando um DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Configurando um DbContext

Este artigo mostra os padrões para configurar um `DbContext` com `DbContextOptions`. Opções são usadas principalmente para selecionar e configurar o repositório de dados.

## <a name="configuring-dbcontextoptions"></a>Configurando DbContextOptions

`DbContext`deve ter uma instância de `DbContextOptions` para executar. Isso pode ser configurado por meio da substituição `OnConfiguring`, ou externamente fornecido por meio de um argumento de construtor.

Se ambos forem usadas, `OnConfiguring` é executado nas opções fornecidas, que significa que é aditivo e pode substituir opções fornecidas para o argumento de construtor.

### <a name="constructor-argument"></a>Argumento de construtor

Código de contexto com o construtor

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> O construtor base do DbContext também aceita a versão não genérica de `DbContextOptions`. Não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.

Código do aplicativo para inicializar a partir do argumento de construtor

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`ocorre por último e pode substituir opções de DI ou o construtor. Essa abordagem não se prestam ao teste (a menos que o banco de dados completo de destino).

Código de contexto com `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Código do aplicativo ao inicializar com `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Usando DbContext com injeção de dependência

EF oferece suporte ao uso `DbContext` com um contêiner de injeção de dependência. O tipo DbContext pode ser adicionado ao contêiner de serviço usando `AddDbContext<TContext>`.

`AddDbContext`fará com que os dois o tipo DbContext, `TContext`, e `DbContextOptions<TContext>` disponíveis para a injeção do contêiner de serviços.

Consulte [leitura mais](#more-reading) abaixo para obter informações sobre injeção de dependência.

Adicionando dbcontext a injeção de dependência

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo DbContext que aceita `DbContextOptions`.

Código do contexto:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Código do aplicativo (no ASP.NET Core):

``` csharp
public MyController(BloggingContext context)
```

Código do aplicativo (usando o provedor de serviços diretamente, menos comuns):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Usando o`IDesignTimeDbContextFactory<TContext>`

Como alternativa para as opções acima, você também pode fornecer uma implementação de `IDesignTimeDbContextFactory<TContext>`. Ferramentas EF podem usar esta fábrica para criar uma instância do seu DbContext. Isso pode ser necessário para habilitar experiências de tempo de design específicas, como as migrações.

Implemente essa interface para habilitar os serviços de tempo de design para tipos de contexto que não têm um construtor padrão público. Serviços de tempo de design descobrirá automaticamente implementações dessa interface que estão no mesmo assembly como o contexto de derivada.

Exemplo:

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a>Ler mais

* Leitura [para iniciar o ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.
* Leitura [injeção de dependência](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) para saber mais sobre como usar a DI.
* Leitura [teste](testing/index.md) para obter mais informações.
