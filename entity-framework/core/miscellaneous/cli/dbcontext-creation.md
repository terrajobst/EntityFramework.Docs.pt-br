---
title: Criação de DbContext de tempo de design-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812025"
---
# <a name="design-time-dbcontext-creation"></a>Criação de DbContext no tempo de design

Alguns dos comandos de ferramentas de EF Core (por exemplo, os comandos de [migração][1] ) exigem que uma instância de `DbContext` derivada seja criada em tempo de design para coletar detalhes sobre os tipos de entidade do aplicativo e como eles são mapeados para um esquema de banco de dados. Na maioria dos casos, é desejável que a `DbContext` criada assim seja configurada de forma semelhante a como ela seria [configurada em tempo de execução][2].

Há várias maneiras pelas quais as ferramentas tentam criar o `DbContext`:

## <a name="from-application-services"></a>Dos serviços de aplicativos

Se o seu projeto de inicialização usar o host do [ASP.NET Core Web][3] ou o [host genérico do .NET Core][4], as ferramentas tentarão obter o objeto DbContext do provedor de serviços do aplicativo.

As ferramentas primeiro tentam obter o provedor de serviços invocando `Program.CreateHostBuilder()`, chamando `Build()`e acessando a propriedade `Services`.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Quando você cria um novo aplicativo ASP.NET Core, esse gancho é incluído por padrão.

O `DbContext` em si e quaisquer dependências em seu Construtor precisam ser registrados como serviços no provedor de serviços do aplicativo. Isso pode ser facilmente obtido com [um construtor na `DbContext` que usa uma instância de `DbContextOptions<TContext>` como um argumento][5] e usando o [método`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Usando um construtor sem parâmetros

Se DbContext não puder ser obtido do provedor de serviços de aplicativo, as ferramentas procurarão o tipo de `DbContext` derivado dentro do projeto. Em seguida, eles tentam criar uma instância usando um construtor sem parâmetros. Esse pode ser o construtor padrão se a `DbContext` for configurada usando o método [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>De uma fábrica de tempo de design

Você também pode informar às ferramentas como criar seu DbContext implementando a interface `IDesignTimeDbContextFactory<TContext>`: se uma classe que implementa essa interface for encontrada no mesmo projeto que o `DbContext` derivado ou no projeto de inicialização do aplicativo, as ferramentas ignorarão a outra maneiras de criar o DbContext e usar a fábrica de tempo de design em vez disso.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> O parâmetro `args` não é usado no momento. Há [um problema][8] que controla a capacidade de especificar argumentos de tempo de design a partir das ferramentas.

Uma fábrica de tempo de design pode ser especialmente útil se você precisar configurar o DbContext de forma diferente para o tempo de design do que no tempo de execução, se o construtor de `DbContext` usar parâmetros adicionais não estiver registrado em DI, se você não estiver usando DI, ou se por algum motivo você prefira não ter um método `BuildWebHost` na classe `Main` do aplicativo ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
