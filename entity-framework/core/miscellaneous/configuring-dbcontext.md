---
title: Configurando um DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: e04c1b65df96819f3493e0ed34ccf26609511f6a
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445916"
---
# <a name="configuring-a-dbcontext"></a>Configurar um DbContext

Este artigo mostra padrões básicos para configurar um `DbContext` por meio de um `DbContextOptions` para se conectar a um banco de dados usando um provedor de EF Core específico e comportamentos opcionais.

## <a name="design-time-dbcontext-configuration"></a>Configuração de DbContext de tempo de design

EF Core ferramentas de tempo de design, como as [migrações](xref:core/managing-schemas/migrations/index) , precisam ser capazes de descobrir e criar uma instância de trabalho de um tipo de `DbContext` para coletar detalhes sobre os tipos de entidade do aplicativo e como eles são mapeados para um esquema de banco de dados. Esse processo pode ser automático, contanto que a ferramenta possa criar facilmente o `DbContext` de forma que ele seja configurado de forma semelhante à forma como ele seria configurado em tempo de execução.

Embora qualquer padrão que forneça as informações de configuração necessárias para o `DbContext` possa funcionar em tempo de execução, as ferramentas que exigem o uso de um `DbContext` em tempo de design só podem funcionar com um número limitado de padrões. Eles são abordados em mais detalhes na seção [criação de contexto em tempo de design](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Configurando o DbContextOptions

`DbContext` deve ter uma instância de `DbContextOptions` para executar qualquer trabalho. A instância `DbContextOptions` transporta informações de configuração, como:

- O provedor de banco de dados a ser usado, normalmente selecionado invocando um método como `UseSqlServer` ou `UseSqlite`. Esses métodos de extensão exigem o pacote de provedor correspondente, como `Microsoft.EntityFrameworkCore.SqlServer` ou `Microsoft.EntityFrameworkCore.Sqlite`. Os métodos são definidos no namespace `Microsoft.EntityFrameworkCore`.
- Qualquer cadeia de conexão ou identificador de instância de banco de dados necessário, normalmente passado como um argumento para o método de seleção de provedor mencionado acima
- Qualquer seletor de comportamento opcional em nível de provedor, normalmente também encadeado dentro da chamada para o método de seleção de provedor
- Qualquer seletor geral de EF Core de comportamento, normalmente encadeado após ou antes do método de seletor de provedor

O exemplo a seguir configura o `DbContextOptions` para usar o provedor de SQL Server, uma conexão contida na variável `connectionString`, um tempo limite de comando no nível do provedor e um seletor de comportamento de EF Core que faz todas as consultas executadas no `DbContext` [sem rastreamento](xref:core/querying/tracking#no-tracking-queries) Por padrão:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Os métodos de seletor de provedor e outros métodos de seletor de comportamento mencionados acima são métodos de extensão em `DbContextOptions` ou classes de opção específicas do provedor. Para ter acesso a esses métodos de extensão, talvez seja necessário ter um namespace (normalmente `Microsoft.EntityFrameworkCore`) no escopo e incluir dependências de pacote adicionais no projeto.

O `DbContextOptions` pode ser fornecido para o `DbContext` substituindo o método `OnConfiguring` ou externamente por meio de um argumento de construtor.

Se ambos forem usados, `OnConfiguring` será aplicado por último e poderá substituir as opções fornecidas para o argumento do construtor.

### <a name="constructor-argument"></a>Argumento do Construtor

O construtor pode simplesmente aceitar um `DbContextOptions` da seguinte maneira:

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
> O construtor base de DbContext também aceita a versão não genérica do `DbContextOptions`, mas o uso da versão não genérica não é recomendado para aplicativos com vários tipos de contexto.

Seu aplicativo agora pode passar o `DbContextOptions` ao instanciar um contexto, da seguinte maneira:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Onconfiguring

Você também pode inicializar o `DbContextOptions` no próprio contexto. Embora você possa usar essa técnica para a configuração básica, normalmente ainda precisará obter determinados detalhes de configuração de fora, por exemplo, uma cadeia de conexão de banco de dados. Isso pode ser feito com uma API de configuração ou qualquer outro meio.

Para inicializar `DbContextOptions` no contexto, substitua o método `OnConfiguring` e chame os métodos no `DbContextOptionsBuilder` fornecido:

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

Um aplicativo pode simplesmente instanciar tal contexto sem passar nada para seu construtor:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Essa abordagem não se presta ao teste, a menos que os testes tenham como destino o banco de dados completo.

### <a name="using-dbcontext-with-dependency-injection"></a>Usando DbContext com injeção de dependência

EF Core dá suporte ao uso de `DbContext` com um contêiner de injeção de dependência. O tipo DbContext pode ser adicionado ao contêiner de serviço usando o método `AddDbContext<TContext>`.

`AddDbContext<TContext>` fará com que o tipo DbContext, `TContext` e o `DbContextOptions<TContext>` correspondente estejam disponíveis para injeção no contêiner de serviço.

Veja [mais leitura](#more-reading) abaixo para obter informações adicionais sobre injeção de dependência.

Adicionando o `DbContext` à injeção de dependência:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Isso requer a adição de um [argumento de Construtor](#constructor-argument) ao tipo DbContext que aceita `DbContextOptions<TContext>`.

Código de contexto:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Código do aplicativo (em ASP.NET Core):

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

Código do aplicativo (usando ServiceProvider diretamente, menos comum):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Evitando problemas de threading de DbContext

Entity Framework Core não dá suporte a várias operações paralelas sendo executadas na mesma instância de `DbContext`. Isso inclui a execução paralela de consultas assíncronas e qualquer uso simultâneo explícito de vários threads. Portanto, sempre `await` as chamadas assíncronas imediatamente ou usar instâncias `DbContext` separadas para operações executadas em paralelo.

Quando EF Core detectar uma tentativa de usar uma instância de `DbContext` simultaneamente, você verá um `InvalidOperationException` com uma mensagem como esta: 

> Uma segunda operação foi iniciada neste contexto antes da conclusão de uma operação anterior. Isso geralmente é causado por threads diferentes usando a mesma instância de DbContext, no entanto, não há garantia de que os membros de instância sejam thread-safe.

Quando o acesso simultâneo deixa de ser detectado, ele pode resultar em comportamento indefinido, falhas do aplicativo e corrupção de dados.

Há erros comuns que podem causar inadvertidamente acesso simultâneo na mesma instância `DbContext`:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Esquecendo de aguardar a conclusão de uma operação assíncrona antes de iniciar qualquer outra operação no mesmo DbContext

Os métodos assíncronos permitem que EF Core iniciem operações que acessam o banco de dados de uma maneira sem bloqueio. Mas se um chamador não aguardar a conclusão de um desses métodos e continuar a executar outras operações no `DbContext`, o estado do `DbContext` poderá ser (e provavelmente será) corrompido. 

Sempre Await EF Core métodos assíncronos imediatamente.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Compartilhando instâncias DbContext implicitamente em vários threads por meio de injeção de dependência

O método de extensão [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra os tipos `DbContext` com um [tempo de vida com escopo](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) definido por padrão. 

Isso é seguro contra problemas de acesso simultâneos em aplicativos ASP.NET Core porque há apenas um thread executando cada solicitação de cliente em um determinado momento, e como cada solicitação Obtém um escopo de injeção de dependência separado (e, portanto, uma instância do `DbContext` separada).

No entanto, qualquer código que execute explicitamente vários threads em paralelo deve garantir que as instâncias `DbContext` nunca sejam acessadas simultaneamente.

Usando a injeção de dependência, isso pode ser feito registrando o contexto como escopo e criando escopos (usando `IServiceScopeFactory`) para cada thread ou registrando o `DbContext` como transitório (usando a sobrecarga de `AddDbContext` que usa um parâmetro `ServiceLifetime`).

## <a name="more-reading"></a>Mais leitura

* Leia [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para saber mais sobre como usar di.
* Leia [teste](testing/index.md) para obter mais informações.
