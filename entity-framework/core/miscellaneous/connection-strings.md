---
title: Cadeias de caracteres de Conexão – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191336"
---
# <a name="connection-strings"></a>Cadeias de caracteres de conexão

A maioria dos provedores de banco de dados exigem alguma forma de cadeia de conexão para se conectar ao banco de dados. Às vezes, essa cadeia de caracteres de conexão contém informações confidenciais que precisam ser protegidos. Você também precisará alterar a cadeia de caracteres de conexão ao seu aplicativo mudar entre ambientes, como desenvolvimento, teste e produção.

## <a name="net-framework-applications"></a>Aplicativos do .NET framework

Aplicativos do .NET framework, como WinForms, WPF, Console e ASP.NET 4, têm um padrão de cadeia de caracteres de conexão testada e. A cadeia de caracteres de conexão deve ser adicionada ao seu arquivo App. config de aplicativos (Web. config se você estiver usando ASP.NET). Se a cadeia de conexão contém informações confidenciais, como nome de usuário e senha, você pode proteger o conteúdo do arquivo de configuração usando [configuração protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> O `providerName` configuração não é necessário em cadeias de caracteres de conexão do EF Core armazenadas em App. config porque o provedor de banco de dados está configurado por meio de código.

Você pode ler a cadeia de conexão usando o `ConfigurationManager` API em seu contexto `OnConfiguring` método. Talvez você precise adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.

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

Cadeias de caracteres de Conexão em um aplicativo da UWP são normalmente uma conexão de SQLite que especifica apenas um nome de arquivo local. Eles normalmente não contêm informações confidenciais e não precisa ser alterado como um aplicativo é implantado. Dessa forma, essas cadeias de caracteres de conexão são normalmente bem a ser deixado no código, conforme mostrado abaixo. Se você quiser movê-los fora do código, em seguida, UWP é compatível com o conceito de configurações, consulte o [seção de configurações do aplicativo da documentação do UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obter detalhes.

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

No ASP.NET Core, o sistema de configuração é muito flexível e a cadeia de caracteres de conexão pode ser armazenada em `appsettings.json`, uma variável de ambiente, o repositório de segredos do usuário ou outra fonte de configuração. Consulte a [seção de configuração da documentação do ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obter mais detalhes. O exemplo a seguir mostra a cadeia de conexão armazenada no `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

O contexto é normalmente configurado no `Startup.cs` com a cadeia de caracteres de conexão que está sendo lida da configuração. Observe a `GetConnectionString()` método procura um valor de configuração cuja chave é `ConnectionStrings:<connection string name>`. Você precisa importar o [Extensions](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace para usar esse método de extensão.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
