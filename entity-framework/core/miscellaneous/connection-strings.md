---
title: Cadeias de conexão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149107"
---
# <a name="connection-strings"></a><span data-ttu-id="88a66-102">Cadeias de caracteres de conexão</span><span class="sxs-lookup"><span data-stu-id="88a66-102">Connection Strings</span></span>

<span data-ttu-id="88a66-103">A maioria dos provedores de banco de dados requer alguma forma de cadeia de conexão para se conectar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="88a66-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="88a66-104">Às vezes, essa cadeia de conexão contém informações confidenciais que precisam ser protegidas.</span><span class="sxs-lookup"><span data-stu-id="88a66-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="88a66-105">Talvez você também precise alterar a cadeia de conexão ao mover seu aplicativo entre ambientes, como desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="88a66-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="88a66-106">WinForms & aplicativos do WPF</span><span class="sxs-lookup"><span data-stu-id="88a66-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="88a66-107">Os aplicativos WinForms, WPF e ASP.NET 4 têm um padrão de cadeia de conexão testado e testado.</span><span class="sxs-lookup"><span data-stu-id="88a66-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="88a66-108">A cadeia de conexão deve ser adicionada ao arquivo app. config do seu aplicativo (Web. config se você estiver usando ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="88a66-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="88a66-109">Se a cadeia de conexão contiver informações confidenciais, como nome de usuário e senha, você poderá proteger o conteúdo do arquivo de configuração usando a [ferramenta Gerenciador de segredo](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="88a66-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="88a66-110">A `providerName` configuração não é necessária em EF Core cadeias de conexão armazenadas em app. config porque o provedor de banco de dados está configurado por meio de código.</span><span class="sxs-lookup"><span data-stu-id="88a66-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="88a66-111">Em seguida, você pode ler a cadeia de `ConfigurationManager` conexão usando a API no `OnConfiguring` método do contexto.</span><span class="sxs-lookup"><span data-stu-id="88a66-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="88a66-112">Talvez seja necessário adicionar uma referência ao assembly da `System.Configuration` estrutura para poder usar essa API.</span><span class="sxs-lookup"><span data-stu-id="88a66-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="88a66-113">UWP (Plataforma Universal do Windows)</span><span class="sxs-lookup"><span data-stu-id="88a66-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="88a66-114">Cadeias de conexão em um aplicativo UWP normalmente são uma conexão do SQLite que especifica apenas um nome de arquivo local.</span><span class="sxs-lookup"><span data-stu-id="88a66-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="88a66-115">Normalmente, eles não contêm informações confidenciais e não precisam ser alterados à medida que um aplicativo é implantado.</span><span class="sxs-lookup"><span data-stu-id="88a66-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="88a66-116">Como tal, essas cadeias de conexão normalmente são boas para serem deixadas no código, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="88a66-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="88a66-117">Se você quiser movê-los para fora do código, o UWP dá suporte ao conceito de configurações, consulte a [seção Configurações do aplicativo da documentação do UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="88a66-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="88a66-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88a66-118">ASP.NET Core</span></span>

<span data-ttu-id="88a66-119">Em ASP.NET Core o sistema de configuração é muito flexível, e a cadeia de conexão pode ser `appsettings.json`armazenada no, uma variável de ambiente, o armazenamento de segredo do usuário ou outra fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="88a66-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="88a66-120">Consulte a [seção configuração da documentação do ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="88a66-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="88a66-121">O exemplo a seguir mostra a cadeia de conexão `appsettings.json`armazenada em.</span><span class="sxs-lookup"><span data-stu-id="88a66-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="88a66-122">O contexto é normalmente configurado em `Startup.cs` com a cadeia de conexão que está sendo lida da configuração.</span><span class="sxs-lookup"><span data-stu-id="88a66-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="88a66-123">Observe que `GetConnectionString()` o método procura um valor de configuração cuja chave `ConnectionStrings:<connection string name>`seja.</span><span class="sxs-lookup"><span data-stu-id="88a66-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="88a66-124">Você precisa importar o namespace [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) para usar esse método de extensão.</span><span class="sxs-lookup"><span data-stu-id="88a66-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
