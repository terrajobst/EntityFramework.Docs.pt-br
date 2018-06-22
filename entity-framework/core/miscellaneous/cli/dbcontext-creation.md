---
title: Criação de DbContext de tempo de design - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202478"
---
<a name="design-time-dbcontext-creation"></a>Criação de tempo de design DbContext
==============================
Alguns dos comandos EF principais ferramentas (por exemplo, o [migrações] [ 1] comandos) exigem um derivado `DbContext` instância a ser criada em tempo de design para obter detalhes sobre o aplicativo tipos de entidade e como eles serão mapeados para um esquema de banco de dados. Na maioria dos casos, é desejável que o `DbContext` criado, portanto, está configurado de maneira semelhante a como seria [configurado em tempo de execução][2].

Há várias maneiras que as ferramentas de tentarem criar o `DbContext`:

<a name="from-application-services"></a>De serviços de aplicativo
-------------------------
Se seu projeto de inicialização é um aplicativo do ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo.

A ferramenta primeiro tenta obter o provedor de serviços invocando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade.

> [!NOTE]
> Quando você cria um novo aplicativo ASP.NET Core 2.0, esse gancho está incluído por padrão. Em versões anteriores do EF Core e ASP.NET Core, as ferramentas tentam invocar `Startup.ConfigureServices` diretamente para obter o provedor de serviços do aplicativo, mas esse padrão não funciona corretamente em aplicativos ASP.NET 2.0 de núcleo. Se você estiver atualizando um aplicativo ASP.NET Core 1. x 2.0, você pode [modificar sua `Program` classe seguir o padrão de novos][3].

O `DbContext` em si e as dependências em seu construtor precisam ser registrados como serviços no provedor de serviços do aplicativo. Isso pode ser facilmente conseguido tendo [um construtor no `DbContext` que usa uma instância do `DbContextOptions<TContext>` como um argumento] [ 4] e usando o [ `AddDbContext<TContext>` método] [5].

<a name="using-a-constructor-with-no-parameters"></a>Usar um construtor sem parâmetros
--------------------------------------
Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas procure derivada `DbContext` tipo dentro do projeto. Em seguida, tente criar uma instância usando um construtor sem parâmetros. Isso pode ser o construtor padrão se o `DbContext` é configurado usando o [ `OnConfiguring` ] [ 6] método.

<a name="from-a-design-time-factory"></a>Uma fábrica de tempo de design
--------------------------
Você também pode informar as ferramentas como criar seu DbContext Implementando o `IDesignTimeDbContextFactory<TContext>` interface: se uma classe que implemente essa interface é encontrada no mesmo projeto como derivada `DbContext` ou no projeto de inicialização do aplicativo, as ferramentas de ignoram outras maneiras de criar o DbContext e usar a fábrica de tempo de design em vez disso.

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
> O `args` parâmetro é usado no momento. Não há [um problema] [ 7] a capacidade de especificar argumentos de tempo de design de ferramentas de controle.

Uma fábrica de tempo de design pode ser especialmente útil se você precisa configurar o DbContext diferente para tempo de design que em tempo de execução, se o `DbContext` leva construtor parâmetros adicionais não estão registrados no DI, se você não estiver usando DI em todos os, ou se alguns motivo você preferir não tem um `BuildWebHost` método no seu aplicativo do ASP.NET Core  
`Main` classe.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
