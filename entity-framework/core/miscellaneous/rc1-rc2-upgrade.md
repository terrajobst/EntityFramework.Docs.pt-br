---
title: Atualização do EF Core 1.0 RC1 para RC2 – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996891"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Atualização do EF Core 1.0 RC1 para 1.0 RC2

Este artigo fornece diretrizes para mover um aplicativo compilado com os pacotes do RC1 para RC2.

## <a name="package-names-and-versions"></a>Versões e nomes de pacote

Entre RC1 e RC2, alteramos de "Entity Framework 7" para "Entity Framework Core". Você pode ler mais sobre os motivos para a alteração nos [esta postagem de Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Por causa dessa alteração, nossos nomes de pacote alterado de `EntityFramework.*` para `Microsoft.EntityFrameworkCore.*` e nossas versões do `7.0.0-rc1-final` à `1.0.0-rc2-final` (ou `1.0.0-preview1-final` para ferramentas).

**Você precisará remover completamente os pacotes do RC1 e, em seguida, instale o RC2 aqueles.** Aqui está o mapeamento para alguns pacotes comuns.

| Pacote do RC1                                               | Equivalente do RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Ainda não está disponível para o RC2                                            |
| EntityFramework 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Juntamente com os nomes de pacote, namespaces alterado de `Microsoft.Data.Entity.*` para `Microsoft.EntityFrameworkCore.*`. Você pode lidar com essa alteração com um localizar/substituir do `using Microsoft.Data.Entity` com `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Alterações de convenção de nomenclatura de tabela

Uma alteração significativa de funcional que realizamos no RC2 era usar o nome da `DbSet<TEntity>` propriedade para uma determinada entidade como o nome da tabela que ela mapeia para, em vez de apenas o nome de classe. Você pode ler mais sobre essa alteração no [o problema de lançamento relacionados](https://github.com/aspnet/Announcements/issues/167).

Para aplicativos existentes do RC1, é recomendável adicionar o código a seguir ao início do seu `OnModelCreating` método para manter a estratégia de nomeação do RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Se você deseja adotar a nova estratégia de nomenclatura, recomendamos com êxito renomeia concluir o restante das etapas de atualização e, em seguida, remover o código e criando uma migração para aplicar a tabela.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext Startup.cs alterações (somente para projetos ASP.NET Core)

No RC1, você precisava adicionar serviços do Entity Framework para o provedor de serviço de aplicativo - `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

No RC2, você pode remover as chamadas para `AddEntityFramework()`, `AddSqlServer()`, etc.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Você também precisará adicionar um construtor, para seu contexto derivado, que usa opções de contexto e os passa para o construtor base. Isso é necessário porque o removemos alguns da mágica assustadora que entrou furtivamente-los em segundo plano:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Passar um IServiceProvider

Se você tiver código RC1 que passa um `IServiceProvider` ao contexto, isso agora foi movido para `DbContextOptions`, em vez de ser um parâmetro de construtor separado. Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para definir o provedor de serviço.

### <a name="testing"></a>Testes

O cenário mais comum para isso era controlar o escopo de um banco de dados InMemory durante o teste. Consulte o documento atualizado [testes](testing/index.md) artigo para obter um exemplo de fazer isso com o RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Resolvendo serviços internos do provedor de serviço de aplicativo (somente para projetos ASP.NET Core)

Se você tiver um aplicativo ASP.NET Core e você deseja que o EF para resolver serviços internos do provedor de serviço de aplicativo, há uma sobrecarga de `AddDbContext` que permite que você configure isso:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> É recomendável, permitindo que o EF internamente gerenciar seus próprios serviços, a menos que você tenha um motivo para combinar os serviços internos do EF em seu provedor de serviço de aplicativo. O principal motivo que você talvez queira fazer isso é usar o provedor de serviços de aplicativo para substituir serviços que o EF usa internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Comandos DNX = > da CLI do .NET (somente para projetos ASP.NET Core)

Se você tiver usado o `dnx ef` comandos para projetos ASP.NET 5, eles agora foram movidas para `dotnet ef` comandos. A mesma sintaxe de comando ainda se aplica. Você pode usar `dotnet ef --help` para obter informações de sintaxe.

A maneira como os comandos são registrados foi alterado no RC2, devido a DNX sendo substituído pela CLI do .NET. Comandos agora são registrados em um `tools` seção `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Se você usar o Visual Studio, agora você pode usar o Console do Gerenciador de pacotes para executar comandos do EF para projetos do ASP.NET Core (isso não tinha suporte no RC1). Você ainda precisa registrar os comandos a `tools` seção `project.json` para fazer isso.

## <a name="package-manager-commands-require-powershell-5"></a>Comandos do Gerenciador de pacote exigem o PowerShell 5

Se você usar os comandos de Entity Framework no Console do Gerenciador de pacotes no Visual Studio, você precisará garantir que você tenha o PowerShell 5 instalado. Esse é um requisito temporário que será removido na próxima versão (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obter mais detalhes).

## <a name="using-imports-in-projectjson"></a>Usando "imports" no Project. JSON

Algumas das dependências do EF Core ainda não suporte a .NET Standard. O EF Core em projetos do .NET Standard e .NET Core talvez seja necessário adicionar "imports" ao Project. JSON como uma solução temporária.

Ao adicionar o EF, a restauração do NuGet exibirá essa mensagem de erro:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

A solução alternativa é importar manualmente o perfil portáteis "portable-net451 seria + win8". Isso força o NuGet para tratar esse binários que correspondem a isso forneça como uma estrutura compatível com o .NET Standard, mesmo que eles não são. Embora "portable-net451 seria + win8" não seja 100% compatível com o .NET Standard, é compatível para a transição de PCL para .NET Standard. Importações podem ser removidas quando dependências do EF, eventualmente, a atualização para o .NET Standard.

Várias estruturas podem ser adicionadas a "imports" na sintaxe da matriz. Outras importações podem ser necessárias se você adicionar as bibliotecas adicionais ao seu projeto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Ver [emitir #5176](https://github.com/aspnet/EntityFramework/issues/5176).
