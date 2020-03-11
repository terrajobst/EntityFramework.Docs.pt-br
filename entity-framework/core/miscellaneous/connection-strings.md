---
title: Cadeias de conexão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416585"
---
# <a name="connection-strings"></a>Cadeias de Conexão

A maioria dos provedores de banco de dados requer alguma forma de cadeia de conexão para se conectar ao banco de dados. Às vezes, essa cadeia de conexão contém informações confidenciais que precisam ser protegidas. Talvez você também precise alterar a cadeia de conexão ao mover seu aplicativo entre ambientes, como desenvolvimento, teste e produção.

## <a name="winforms--wpf-applications"></a>WinForms & aplicativos do WPF

Os aplicativos WinForms, WPF e ASP.NET 4 têm um padrão de cadeia de conexão testado e testado. A cadeia de conexão deve ser adicionada ao arquivo app. config do seu aplicativo (Web. config se você estiver usando ASP.NET). Se a cadeia de conexão contiver informações confidenciais, como nome de usuário e senha, você poderá proteger o conteúdo do arquivo de configuração usando a [ferramenta Gerenciador de segredo](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> A configuração de `providerName` não é necessária em cadeias de conexão EF Core armazenadas em app. config porque o provedor de banco de dados está configurado por meio de código.

Em seguida, você pode ler a cadeia de conexão usando a API `ConfigurationManager` no método `OnConfiguring` do seu contexto. Talvez seja necessário adicionar uma referência ao assembly do `System.Configuration` Framework para poder usar essa API.

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

Cadeias de conexão em um aplicativo UWP normalmente são uma conexão do SQLite que especifica apenas um nome de arquivo local. Normalmente, eles não contêm informações confidenciais e não precisam ser alterados à medida que um aplicativo é implantado. Como tal, essas cadeias de conexão normalmente são boas para serem deixadas no código, como mostrado abaixo. Se você quiser movê-los para fora do código, o UWP dá suporte ao conceito de configurações, consulte a [seção Configurações do aplicativo da documentação do UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obter detalhes.

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

Em ASP.NET Core o sistema de configuração é muito flexível, e a cadeia de conexão pode ser armazenada em `appsettings.json`, uma variável de ambiente, o armazenamento de segredo do usuário ou outra fonte de configuração. Consulte a [seção configuração da documentação do ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obter mais detalhes. O exemplo a seguir mostra a cadeia de conexão armazenada em `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

O contexto é normalmente configurado em `Startup.cs` com a cadeia de conexão sendo lida da configuração. Observe que o método `GetConnectionString()` procura um valor de configuração cuja chave é `ConnectionStrings:<connection string name>`. Você precisa importar o namespace [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) para usar esse método de extensão.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
