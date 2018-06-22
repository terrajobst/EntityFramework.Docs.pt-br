---
title: A atualização do EF Core 1.0 RC1 para RC2 - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678621"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>A atualização do EF Core 1.0 RC1 para 1.0 RC2

Este artigo fornece diretrizes para mover um aplicativo compilado com os pacotes RC1 para RC2.

## <a name="package-names-and-versions"></a>Versões e nomes de pacote

Entre RC1 e RC2, é alterado de "Entity Framework 7" a "Entity Framework Core". Você pode ler mais sobre os motivos para que a alteração no [esta postagem por Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Devido a essa alteração, os nomes de pacote é alterado de `EntityFramework.*` para `Microsoft.EntityFrameworkCore.*` e nossas versões de `7.0.0-rc1-final` para `1.0.0-rc2-final` (ou `1.0.0-preview1-final` para ferramentas).

**Você precisará remover completamente os pacotes RC1 e, em seguida, instale o RC2 aqueles.** Aqui está o mapeamento para alguns pacotes comuns.

| Pacote do RC1                                               | RC2 equivalente                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Ainda não está disponível para RC2                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Juntamente com os nomes de pacote, namespaces alterado de `Microsoft.Data.Entity.*` para `Microsoft.EntityFrameworkCore.*`. Você pode lidar com essa alteração com um localizar/substituir de `using Microsoft.Data.Entity` com `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabela de alterações de convenção de nomenclatura

Uma alteração significativa de funcional que tomamos no RC2 era usar o nome do `DbSet<TEntity>` propriedade para uma determinada entidade como o nome da tabela que ele seja mapeado para, em vez de apenas o nome da classe. Você pode ler mais sobre essa alteração no [o problema de lançamento relacionados](https://github.com/aspnet/Announcements/issues/167).

Para aplicativos existentes do RC1, é recomendável adicionar o código a seguir para o início da sua `OnModelCreating` método para manter a estratégia de nomenclatura RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Se você desejar adotar a nova estratégia de nomenclatura, recomendamos com êxito renomeia concluir o restante das etapas de atualização e, em seguida, remover o código e a criação de uma migração para aplicar a tabela.

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

Você também precisará adicionar um construtor para o contexto derivado, que usa opções de contexto e os passa para o construtor base. Isso é necessário porque removemos alguns mágica assustador que entrou furtivamente-los em segundo plano:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Passar um IServiceProvider

Se você tiver código RC1 transmite um `IServiceProvider` para o contexto, isso agora foi movido para `DbContextOptions`, em vez de ser um parâmetro de construtor separado. Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para definir o provedor de serviço.

### <a name="testing"></a>Testes

O cenário mais comum para isso era controlar o escopo de um banco de dados InMemory durante o teste. Consulte a atualização [teste](testing/index.md) artigo para obter um exemplo de como fazer isso com RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Resolvendo serviços internos do provedor de serviços de aplicativo (somente para projetos ASP.NET Core)

Se você tiver um aplicativo ASP.NET Core e você desejar EF para resolver serviços internos do provedor de serviço de aplicativo, há uma sobrecarga de `AddDbContext` que permite que você configure isso:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> É recomendável permitir que o EF internamente gerenciar seus próprios serviços, a menos que você tenha um motivo para combinar os serviços EF internos em seu provedor de serviços de aplicativo. O principal motivo que talvez você queira fazer isso é usar o provedor de serviços de aplicativo para substituir os serviços que o EF usa internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Comandos do DNX = > .NET CLI (apenas para projetos ASP.NET Core)

Se você tiver usado o `dnx ef` comandos para projetos ASP.NET 5, eles agora foram movidas para `dotnet ef` comandos. A mesma sintaxe de comando ainda se aplica. Você pode usar `dotnet ef --help` para obter informações de sintaxe.

A maneira como os comandos são registrados foi alterado no RC2, devido a DNX está sendo substituída pela CLI do .NET. Comandos são registrados em um `tools` seção `project.json`:

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
> Se você usar o Visual Studio, agora você pode usar o Console do Gerenciador de pacotes para executar comandos EF para projetos ASP.NET Core (isso não era suportado no RC1). Você ainda precisará registrar os comandos de `tools` seção `project.json` para fazer isso.

## <a name="package-manager-commands-require-powershell-5"></a>Comandos do Gerenciador de pacote exigem o PowerShell 5

Se você usar os comandos do Entity Framework no Console do Gerenciador de pacotes no Visual Studio, você precisará ter 5 PowerShell instalado. Esse é um requisito temporário que será removido na próxima versão (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obter mais detalhes).

## <a name="using-imports-in-projectjson"></a>Usando "imports" no Project

Algumas das dependências do EF Core ainda não suporta .NET padrão. EF Core em projetos do .NET padrão e .NET Core talvez seja necessário adicionar "importar" para Project como uma solução temporária.

Ao adicionar EF, a restauração do NuGet exibirá essa mensagem de erro:

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

A solução alternativa é importar manualmente o perfil portátil "portátil net451 + win8". Isso força o NuGet para tratar esse binários que correspondem a esse fornece como uma estrutura compatível com .NET padrão, mesmo que eles não são. Embora "portátil net451 + win8" não é 100% compatível com .NET padrão, é compatível para a transição do PCL para .NET padrão. Importações podem ser removidas quando dependências do EF eventualmente atualizar para o .NET padrão.

Várias estruturas podem ser adicionadas para "imports" na sintaxe da matriz. Outras importações podem ser necessárias se você adicionar bibliotecas adicionais ao seu projeto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Consulte [emitir &#5176;](https://github.com/aspnet/EntityFramework/issues/5176).
