---
title: Configurando um DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152384"
---
# <a name="configuring-a-dbcontext"></a>Configurando um DbContext

Este artigo mostra padrões básicos para configurar um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor EF Core e comportamentos opcionais.

## <a name="design-time-dbcontext-configuration"></a>Configuração do tempo de design DbContext

Tempo de design EF principais ferramentas como [migrações](xref:core/managing-schemas/migrations/index) precisa ser capaz de detectar e criar uma instância de trabalho de um `DbContext` tipo para obter detalhes sobre os tipos de entidade e como eles serão mapeados para um esquema de banco de dados do aplicativo. Esse processo pode ser automático enquanto a ferramenta pode criar facilmente o `DbContext` de forma que ele será configurado da mesma forma como seria configurado em tempo de execução.

Enquanto qualquer padrão que fornece as informações de configuração necessárias para o `DbContext` pode trabalhar em tempo de execução, ferramentas que exigem o uso de um `DbContext` em tempo de design só pode trabalhar com um número limitado de padrões. Eles são abordados mais detalhadamente o [criação do contexto de tempo de Design](xref:core/miscellaneous/cli/dbcontext-creation) seção.

## <a name="configuring-dbcontextoptions"></a>Configurando DbContextOptions

`DbContext` deve ter uma instância de `DbContextOptions` para executar qualquer trabalho. O `DbContextOptions` instância contém informações de configuração, como:

- O provedor de banco de dados a ser usado, normalmente selecionada invocando um método como `UseSqlServer` ou `UseSqlite`
- Qualquer cadeia de caracteres de conexão necessárias ou o identificador da instância de banco de dados, normalmente passado como um argumento para o método de seleção de provedor mencionado acima
- Quaisquer seletores de comportamento opcional de nível de provedor, normalmente também encadeados dentro da chamada para o método de seleção de provedor
- Quaisquer seletores de comportamento EF Core gerais, normalmente encadeados depois ou antes que o método de seletor de provedor

O exemplo a seguir configura o `DbContextOptions` para usar o provedor do SQL Server, uma conexão contidas a `connectionString` variável, um tempo limite de comando de nível de provedor e um seletor de comportamento de EF principal que faz com que todas as consultas executadas no `DbContext` [nenhum controle](xref:core/querying/tracking#no-tracking-queries) por padrão:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são os métodos de extensão em `DbContextOptions` ou classes de opção específica do provedor. Para ter acesso a esses métodos de extensão, talvez seja necessário ter um namespace (geralmente `Microsoft.EntityFrameworkCore`) no escopo e incluir as dependências do pacote adicional no projeto.

O `DbContextOptions` podem ser fornecidos para o `DbContext` , substituindo o `OnConfiguring` método ou externamente por meio de um argumento de construtor.

Se ambos forem usadas, `OnConfiguring` é aplicada por último e podem substituir as opções fornecidas para o argumento de construtor.

### <a name="constructor-argument"></a>Argumento de construtor

Código de contexto com o construtor:

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
> O construtor base do DbContext também aceita a versão não genérica de `DbContextOptions`, mas não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.

Código do aplicativo para inicializar a partir do argumento de construtor:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

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

Código do aplicativo para inicializar um `DbContext` que usa `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Essa abordagem não se prestam para teste, a menos que os testes de banco de dados completo de destino.

### <a name="using-dbcontext-with-dependency-injection"></a>Usando DbContext com injeção de dependência

EF Core dá suporte ao uso `DbContext` com um contêiner de injeção de dependência. O tipo DbContext pode ser adicionado ao contêiner de serviço usando o `AddDbContext<TContext>` método.

`AddDbContext<TContext>` fará com que os dois o tipo DbContext, `TContext`e o correspondente `DbContextOptions<TContext>` disponíveis para a injeção do contêiner de serviços.

Consulte [leitura mais](#more-reading) abaixo para obter informações adicionais sobre injeção de dependência.

Adicionando o `Dbcontext` a injeção de dependência:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo DbContext que aceita `DbContextOptions<TContext>`.

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
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Código do aplicativo (usando o provedor de serviços diretamente, menos comuns):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Ler mais

* Leitura [para iniciar o ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.
* Leitura [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar a DI.
* Leitura [teste](testing/index.md) para obter mais informações.
