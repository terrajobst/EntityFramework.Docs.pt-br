---
title: Criação de DbContext no tempo de design - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993712"
---
<a name="design-time-dbcontext-creation"></a>Criação de DbContext no tempo de design
==============================
Alguns dos comandos de ferramentas do EF Core (por exemplo, o [migrações][1] comandos) exigem um derivada `DbContext` instância a ser criada em tempo de design para obter detalhes sobre o aplicativo tipos de entidade e como eles são mapeados para um esquema de banco de dados. Na maioria dos casos, é desejável que o `DbContext` criado, portanto, está configurado de forma semelhante a como ele seria [configurado no tempo de execução][2].

Há várias maneiras, as ferramentas de tentam criar o `DbContext`:

<a name="from-application-services"></a>Dos serviços de aplicativo
-------------------------
Se seu projeto de inicialização é um aplicativo ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo.

As ferramentas primeiro tentam obter o provedor de serviço invocando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade.

> [!NOTE]
> Quando você cria um novo aplicativo ASP.NET Core 2.0, esse gancho é incluído por padrão. Nas versões anteriores do EF Core e ASP.NET Core, as ferramentas de tentarem invocar `Startup.ConfigureServices` diretamente para obter o provedor de serviços do aplicativo, mas esse padrão não funciona corretamente em aplicativos ASP.NET Core 2.0. Se você estiver atualizando um aplicativo ASP.NET Core 1.x 2.0, você poderá [modificar sua `Program` classe seguir o padrão de novos][3].

O `DbContext` em si e todas as dependências em seu construtor precisam ser registrados como serviços no provedor de serviços do aplicativo. Isso pode ser feito facilmente tendo [um construtor na `DbContext` que usa uma instância do `DbContextOptions<TContext>` como um argumento][4] e usando o [`AddDbContext<TContext>` método][5].

<a name="using-a-constructor-with-no-parameters"></a>Usando um construtor sem parâmetros
--------------------------------------
Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas procure derivado `DbContext` tipo dentro do projeto. Em seguida, tente criar uma instância usando um construtor sem parâmetros. Isso pode ser o construtor padrão se o `DbContext` é configurado usando o [`OnConfiguring`][6] método.

<a name="from-a-design-time-factory"></a>De uma fábrica de tempo de design
--------------------------
Você também pode informar as ferramentas de como criar o DbContext, Implementando a `IDesignTimeDbContextFactory<TContext>` interface: se uma classe que implementa essa interface for encontrada no mesmo projeto derivado `DbContext` ou no projeto de inicialização do aplicativo, as ferramentas de ignoram outras maneiras de criar o DbContext e o uso a fábrica de tempo de design em vez disso.

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
> O `args` parâmetro está sendo utilizado. Não há [um problema][7] a capacidade de especificar argumentos de tempo de design das ferramentas de acompanhamento.

Uma fábrica de tempo de design pode ser especialmente útil se você precisar configurar o DbContext de forma diferente para o tempo de design que em tempo de execução, se o `DbContext` leva construtor parâmetros adicionais não são registrados na DI, se você não estiver usando injeção de dependência em todos os ou se para alguns motivo você prefere não ter uma `BuildWebHost` método do seu aplicativo ASP.NET Core `Main` classe.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
