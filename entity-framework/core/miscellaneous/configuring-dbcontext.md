---
title: Configurar um DbContext – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 47e0a66a136e743a815d099d2bee5f0da1a068c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363931"
---
# <a name="configuring-a-dbcontext"></a>Configurar um DbContext

Este artigo mostra os padrões básicos para a configuração de um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor específico do EF Core e comportamentos opcionais.

## <a name="design-time-dbcontext-configuration"></a>Configuração do tempo de design DbContext

Ferramentas de tempo de design do EF Core, como [migrações](xref:core/managing-schemas/migrations/index) precisa ser capaz de detectar e criar uma instância de trabalho de um `DbContext` tipo para obter detalhes sobre os tipos de entidade e como eles são mapeados para um esquema de banco de dados do aplicativo. Esse processo pode ser automático, desde que a ferramenta pode criar facilmente o `DbContext` de tal forma que ele será configurado da mesma forma como deve ser configurado em tempo de execução.

Embora qualquer padrão que fornece informações de configuração necessárias para o `DbContext` pode funcionar em tempo de execução, as ferramentas que exigem o uso de um `DbContext` em tempo de design só pode trabalhar com um número limitado de padrões. Isso é abordado em mais detalhes os [criação de contexto de tempo de Design](xref:core/miscellaneous/cli/dbcontext-creation) seção.

## <a name="configuring-dbcontextoptions"></a>Configurando DbContextOptions

`DbContext` deve ter uma instância de `DbContextOptions` para realizar qualquer trabalho. O `DbContextOptions` instância carrega as informações de configuração, como:

- O provedor de banco de dados a ser usado, normalmente selecionado ao invocar um método como `UseSqlServer` ou `UseSqlite`. Esses métodos de extensão exigem o pacote correspondente do provedor, como `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`. Os métodos são definidos na `Microsoft.EntityFrameworkCore` namespace.
- Qualquer cadeia de caracteres de conexão necessárias ou o identificador da instância do banco de dados, normalmente passado como um argumento para o método de seleção de provedor mencionado acima
- Seletores qualquer comportamento opcional do nível de provedor, normalmente também encadeadas dentro da chamada para o método de seleção de provedor
- Qualquer gerais seletores de comportamento do EF Core, encadeados normalmente depois ou antes que o método de seletor de provedor

O exemplo a seguir configura a `DbContextOptions` para usar o provedor do SQL Server, uma conexão contidos em de `connectionString` variável, um tempo limite de comando de nível de provedor e um seletor de comportamento do EF Core que faz com que todas as consultas executadas no `DbContext` [sem controle](xref:core/querying/tracking#no-tracking-queries) por padrão:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são métodos de extensão em `DbContextOptions` ou classes de opção específico do provedor. Para ter acesso a esses métodos de extensão que você talvez precise ter um namespace (normalmente `Microsoft.EntityFrameworkCore`) no escopo e incluir as dependências de pacote adicionais no projeto.

O `DbContextOptions` pode ser fornecido para o `DbContext` , substituindo o `OnConfiguring` método ou externamente por meio de um argumento do construtor.

Se ambos forem usadas, `OnConfiguring` é aplicada por último e podem substituir as opções fornecidas para o argumento do construtor.

### <a name="constructor-argument"></a>Argumento de construtor

Código do contexto com o construtor:

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
> O construtor de base do DbContext também aceita a versão não genérica de `DbContextOptions`, mas não é recomendável usar a versão não genérica para aplicativos com vários tipos de contexto.

Código do aplicativo para inicializar a partir do argumento do construtor:

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

Código do aplicativo para inicializar uma `DbContext` que usa `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Essa abordagem não se prestam para teste, a menos que os testes de banco de dados completo de destino.

### <a name="using-dbcontext-with-dependency-injection"></a>Usando o DbContext com injeção de dependência

O EF Core dá suporte ao uso `DbContext` com um contêiner de injeção de dependência. O tipo DbContext pode ser adicionado ao contêiner de serviço usando o `AddDbContext<TContext>` método.

`AddDbContext<TContext>` fará os dois é o tipo DbContext, `TContext`e o correspondente `DbContextOptions<TContext>` disponível para injeção de contêiner de serviço.

Ver [ler mais](#more-reading) abaixo para obter mais informações sobre injeção de dependência.

Adicionando o `Dbcontext` a injeção de dependência:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Isso requer a adição de um [argumento do construtor](#constructor-argument) para o tipo de DbContext que aceita `DbContextOptions<TContext>`.

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

Código do aplicativo (usando o provedor de serviços diretamente, menos comum):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Evitando problemas de threading de DbContext

O Entity Framework Core não oferece suporte a várias operações simultâneas que estão sendo executadas no mesmo `DbContext` instância. Acesso simultâneo pode resultar em comportamento indefinido, panes de aplicativos e dados corrompidos. Por isso é importante usar sempre separar `DbContext` instâncias para operações executadas em paralelo. 

Há erros comuns que podem fazer o inadvernetly access simultâneas na mesma `DbContext` instância:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Esquecer de aguardar a conclusão de uma operação assíncrona antes de iniciar qualquer outra operação no mesmo DbContext

Métodos assíncronos permitem que o EF Core iniciar as operações que acessam o banco de dados de uma maneira sem bloqueio. Mas, se um chamador não aguardar a conclusão de um desses métodos e continua a executar outras operações de `DbContext`, o estado do `DbContext` pode ser (e será muito provável) corrompido. 

Sempre aguardar os métodos assíncronos do EF Core imediatamente.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Instâncias de DbContext implicitamente de compartilhamento entre vários threads por meio da injeção de dependência

O [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra o método de extensão `DbContext` tipos com uma [tempo de vida com escopo](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) por padrão. 

Isso é seguro contra problemas de acesso simultâneo em aplicativos ASP.NET Core porque há apenas um thread de execução de cada solicitação de cliente em um determinado momento, e como cada solicitação recebe um escopo de injeção de dependência separados (e, portanto, um separado `DbContext` instância).

No entanto a qualquer código que executa explicitamente vários threads em paralelo deve garantir que `DbContext` instâncias não são nunca accesed simultaneamente.

Usando a injeção de dependência, isso pode ser obtido ao registrar o contexto como escopos de criação e escopo (usando `IServiceScopeFactory`) para cada thread ou registrando-se a `DbContext` como transitório (usando a sobrecarga de `AddDbContext` que utiliza um `ServiceLifetime` parâmetro).

## <a name="more-reading"></a>Ler mais

* Leia [Introdução ao ASP.NET Core](../get-started/aspnetcore/index.md) para obter mais informações sobre como usar o EF com ASP.NET Core.
* Leia [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar a DI.
* Leia [testes](testing/index.md) para obter mais informações.
