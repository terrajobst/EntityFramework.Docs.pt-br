---
title: "Criação de DbContext de tempo de design - Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Criação de tempo de design DbContext
==============================
Algumas das ferramentas EF comandos requerem uma instância de DbContext a ser criado no design de tempo (por exemplo, ao executar comandos de migrações). Há várias maneiras que de ferramentas tentam criá-la.

<a name="from-application-services"></a>De serviços de aplicativo
-------------------------
Se seu projeto de inicialização é um aplicativo do ASP.NET Core, as ferramentas de tentar obter o objeto DbContext do provedor de serviços do aplicativo. Eles obtiverem-lo chamando `Program.BuildWebHost()` e acessar o `IWebHost.Services` propriedade. Qualquer DbContext registrado usando `IServiceCollection.AddDbContext<TContext>()` pode ser encontrado e criado dessa maneira. Esse padrão foi [introduzido no ASP.NET 2.0 de núcleo][1]

<a name="using-the-default-constructor"></a>Usando o construtor padrão
-----------------------------
Se o DbContext não pode ser obtido do provedor de serviço de aplicativo, as ferramentas de aparência para o tipo DbContext dentro do projeto. Tente criá-la usando o construtor padrão.

<a name="from-a-design-time-factory"></a>Uma fábrica de tempo de design
--------------------------
Você também pode informar as ferramentas como criar seu DbContext implementando `IDesignTimeDbContextFactory`. Se uma classe que implemente essa interface foi encontrada no seu projeto, as ferramentas de ignoram outras maneiras de criar o DbContext.
Eles sempre usam a fábrica em tempo de design. Uma fábrica é especialmente útil se você precisa configurar o DbContext diferente para tempo de design que em tempo de execução.

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

> [!NOTE]
> O `args` parâmetro é usado no momento. Não há [um problema] [ 2] a capacidade de especificar argumentos de tempo de design de ferramentas de controle.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
