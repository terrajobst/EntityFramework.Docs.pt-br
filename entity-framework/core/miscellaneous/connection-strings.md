---
title: Cadeias de caracteres de Conexão – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 52a8527170845d3e73ebcec518713ade3f3844f0
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333849"
---
# <a name="connection-strings"></a><span data-ttu-id="fdf13-102">Cadeias de caracteres de conexão</span><span class="sxs-lookup"><span data-stu-id="fdf13-102">Connection Strings</span></span>

<span data-ttu-id="fdf13-103">A maioria dos provedores de banco de dados exigem alguma forma de cadeia de conexão para se conectar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fdf13-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="fdf13-104">Às vezes, essa cadeia de caracteres de conexão contém informações confidenciais que precisam ser protegidos.</span><span class="sxs-lookup"><span data-stu-id="fdf13-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="fdf13-105">Você também precisará alterar a cadeia de caracteres de conexão ao seu aplicativo mudar entre ambientes, como desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="fdf13-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="fdf13-106">Aplicativos do .NET framework</span><span class="sxs-lookup"><span data-stu-id="fdf13-106">.NET Framework Applications</span></span>

<span data-ttu-id="fdf13-107">Aplicativos do .NET framework, como WinForms, WPF, Console e ASP.NET 4, têm um padrão de cadeia de caracteres de conexão testada e.</span><span class="sxs-lookup"><span data-stu-id="fdf13-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="fdf13-108">A cadeia de caracteres de conexão deve ser adicionada ao seu arquivo App. config de aplicativos (Web. config se você estiver usando ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="fdf13-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="fdf13-109">Se a cadeia de conexão contém informações confidenciais, como nome de usuário e senha, você pode proteger o conteúdo do arquivo de configuração usando [configuração protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="fdf13-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="fdf13-110">O `providerName` configuração não é necessário em cadeias de caracteres de conexão do EF Core armazenadas em App. config porque o provedor de banco de dados está configurado por meio de código.</span><span class="sxs-lookup"><span data-stu-id="fdf13-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="fdf13-111">Você pode ler a cadeia de conexão usando o `ConfigurationManager` API em seu contexto `OnConfiguring` método.</span><span class="sxs-lookup"><span data-stu-id="fdf13-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="fdf13-112">Talvez você precise adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.</span><span class="sxs-lookup"><span data-stu-id="fdf13-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="fdf13-113">UWP (Plataforma Universal do Windows)</span><span class="sxs-lookup"><span data-stu-id="fdf13-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="fdf13-114">Cadeias de caracteres de Conexão em um aplicativo da UWP são normalmente uma conexão de SQLite que especifica apenas um nome de arquivo local.</span><span class="sxs-lookup"><span data-stu-id="fdf13-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="fdf13-115">Eles normalmente não contêm informações confidenciais e não precisa ser alterado como um aplicativo é implantado.</span><span class="sxs-lookup"><span data-stu-id="fdf13-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="fdf13-116">Dessa forma, essas cadeias de caracteres de conexão são normalmente bem a ser deixado no código, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="fdf13-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="fdf13-117">Se você quiser movê-los fora do código, em seguida, UWP é compatível com o conceito de configurações, consulte o [seção de configurações do aplicativo da documentação do UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="fdf13-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="fdf13-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdf13-118">ASP.NET Core</span></span>

<span data-ttu-id="fdf13-119">No ASP.NET Core, o sistema de configuração é muito flexível e a cadeia de caracteres de conexão pode ser armazenada em `appsettings.json`, uma variável de ambiente, o repositório de segredos do usuário ou outra fonte de configuração.</span><span class="sxs-lookup"><span data-stu-id="fdf13-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="fdf13-120">Consulte a [seção de configuração da documentação do ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="fdf13-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="fdf13-121">O exemplo a seguir mostra a cadeia de conexão armazenada no `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="fdf13-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="fdf13-122">O contexto é normalmente configurado no `Startup.cs` com a cadeia de caracteres de conexão que está sendo lida da configuração.</span><span class="sxs-lookup"><span data-stu-id="fdf13-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="fdf13-123">Observe a `GetConnectionString()` método procura um valor de configuração cuja chave é `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="fdf13-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="fdf13-124">Você precisa importar o [Extensions](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace para usar esse método de extensão.</span><span class="sxs-lookup"><span data-stu-id="fdf13-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
