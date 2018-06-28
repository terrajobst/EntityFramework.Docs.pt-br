---
"Cadeias de conexão - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052526"
---
# <a name="connection-strings"></a>Cadeias de conexão

A maioria dos provedores de banco de dados requer alguma forma de cadeia de conexão para se conectar ao banco de dados. Às vezes, essa cadeia de conexão contém informações confidenciais que precisam ser protegidas. Talvez você precise alterar a cadeia de conexão ao mover seu aplicativo entre ambientes, como desenvolvimento, teste e produção.

## <a name="net-framework-applications"></a>Aplicativos do .NET framework

Aplicativos do .NET framework, como WinForms, o WPF, o Console e o ASP.NET 4, com um padrão de cadeia de conexão testada e. A cadeia de conexão deve ser adicionada ao seu arquivo App. config de aplicativos (Web. config se você estiver usando o ASP.NET). Se a cadeia contém informações confidenciais, como nome de usuário e senha, você pode proteger o conteúdo do arquivo de configuração usando [configuração protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> O `providerName` configuração não é necessário em cadeias de caracteres de conexão principal EF armazenadas em App. config porque o provedor de banco de dados está configurado por meio de código.

Em seguida, você pode ler a cadeia de conexão usando o `ConfigurationManager` API em seu contexto `OnConfiguring` método. Talvez seja necessário adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>UWP (Plataforma Universal do Windows)

Cadeias de Conexão em um aplicativo UWP são, normalmente, conexões do SQLite que especificam apenas o nome de arquivo local. Normalmente, elas não contêm informações confidenciais e não precisam ser alteradas quando um aplicativo é implantado. Assim, essas cadeias de conexão podem permanecer no código na maioria das vezes, conforme mostrado abaixo. Se deseja retirá-las do código, o UWP oferece suporte ao conceito de configurações, consulte o [seção configurações do aplicativo da documentação UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obter detalhes.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

No núcleo do ASP.NET, o sistema de configuração é muito flexível e a cadeia de conexão pode ser armazenada no 'appsettings.json', em uma variável de ambiente, no armazenamento de segredo do usuário ou em outra fonte de configuração. Consulte a [seção de configuração da documentação do ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obter mais detalhes. O exemplo a seguir mostra a cadeia de conexão armazenada no `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

O contexto é normalmente configurado em `Startup.cs` com a cadeia de conexão que está sendo lida da configuração. Observe que o método `GetConnectionString()` procura por um valor de configuração cuja chave seja `ConnectionStrings:<connection string name>`.
``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
