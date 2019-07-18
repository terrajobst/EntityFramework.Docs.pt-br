---
title: Atualizando do EF Core 1,0 RC1 para RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5300fe459ec2b8ab9bb573c7284b009249071d65
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306463"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Atualizando do EF Core 1,0 RC1 para 1,0 RC2

Este artigo fornece diretrizes para mover um aplicativo criado com os pacotes RC1 para RC2.

## <a name="package-names-and-versions"></a>Nomes e versões de pacote

Entre o RC1 e o RC2, alteramos de "Entity Framework 7" para "Entity Framework Core". Você pode ler mais sobre os motivos da alteração nesta postagem de [Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Por causa dessa alteração, nossos nomes de pacote mudaram `Microsoft.EntityFrameworkCore.*` de `EntityFramework.*` para e nossas `7.0.0-rc1-final` versões `1.0.0-rc2-final` de para `1.0.0-preview1-final` (ou para ferramentas).

**Será necessário remover completamente os pacotes RC1 e, em seguida, instalar os RC2.** Aqui está o mapeamento para alguns pacotes comuns.

| Pacote RC1                                               | RC2 equivalente                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Ainda não disponível para RC2                                            |
| EntityFramework. Commands 7.0.0-RC1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Juntamente com nomes de pacote, os namespaces `Microsoft.Data.Entity.*` foram `Microsoft.EntityFrameworkCore.*`alterados de para. Você pode lidar com essa alteração com uma localização/substituição `using Microsoft.Data.Entity` de `using Microsoft.EntityFrameworkCore`com.

## <a name="table-naming-convention-changes"></a>Alterações na Convenção de nomenclatura de tabela

Uma alteração funcional significativa que fizemos no RC2 era usar o nome da `DbSet<TEntity>` propriedade para uma determinada entidade como o nome da tabela que ele mapeia, em vez de apenas o nome da classe. Você pode ler mais sobre essa alteração no [problema relacionado do comunicado](https://github.com/aspnet/Announcements/issues/167).

Para aplicativos RC1 existentes, recomendamos adicionar o seguinte código ao início do `OnModelCreating` método para manter a estratégia de nomenclatura do RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Se você quiser adotar a nova estratégia de nomenclatura, recomendamos que você conclua com êxito o restante das etapas de atualização e, em seguida, removesse o código e criando uma migração para aplicar os renomeações de tabela.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>Alterações de AddDbContext/Startup.cs (somente ASP.NET Core projetos)

No RC1, você precisava adicionar serviços de Entity Framework ao provedor `Startup.ConfigureServices(...)`de serviços de aplicativo:

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

Você também precisa adicionar um construtor ao seu contexto derivado, que usa opções de contexto e os passa para o construtor base. Isso é necessário porque removemos algumas das mágicas assustadoras que as recorri em segundo plano:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Passando um IServiceProvider

Se você tiver um código RC1 que passe `IServiceProvider` um para o contexto, ele agora será movido `DbContextOptions`para, em vez de ser um parâmetro de Construtor separado. Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para definir o provedor de serviços.

### <a name="testing"></a>Testes

O cenário mais comum para fazer isso era controlar o escopo de um banco de dados de inmemory durante o teste. Consulte o artigo [teste](testing/index.md) atualizado para obter um exemplo de como fazer isso com o RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Resolvendo serviços internos do provedor de serviços de aplicativo (somente ASP.NET Core projetos)

Se você tiver um aplicativo ASP.NET Core e quiser que o EF resolva serviços internos do provedor de serviços de aplicativo, há uma sobrecarga do `AddDbContext` que permite que você configure isso:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> É recomendável permitir que o EF gerencie internamente seus próprios serviços, a menos que você tenha um motivo para combinar os serviços internos do EF em seu provedor de serviços de aplicativo. O principal motivo para que você queira fazer isso é usar o provedor de serviços de aplicativo para substituir os serviços que o EF usa internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Comandos DNX = > CLI .NET (somente projetos ASP.NET Core)

Se você usou anteriormente os `dnx ef` comandos para projetos ASP.NET 5, eles agora foram movidos `dotnet ef` para comandos. A mesma sintaxe de comando ainda se aplica. Você pode usar `dotnet ef --help` para obter informações de sintaxe.

A maneira como os comandos são registrados foi alterada no RC2, devido ao DNX sendo substituído pela CLI do .NET. Os comandos agora são registrados em `tools` uma seção `project.json`em:

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
> Se você usar o Visual Studio, agora poderá usar o console do Gerenciador de pacotes para executar comandos do EF para projetos de ASP.NET Core (não há suporte para isso no RC1). Você ainda precisará registrar os comandos na `tools` seção de `project.json` para fazer isso.

## <a name="package-manager-commands-require-powershell-5"></a>Os comandos do Gerenciador de pacotes exigem o PowerShell 5

Se você usar os comandos de Entity Framework no console do Gerenciador de pacotes no Visual Studio, será necessário garantir que o PowerShell 5 esteja instalado. Esse é um requisito temporário que será removido na próxima versão (consulte o [problema #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obter mais detalhes).

## <a name="using-imports-in-projectjson"></a>Usando "Imports" em Project. JSON

Algumas das dependências do EF Core ainda não dão suporte ao .NET Standard. EF Core em projetos do .NET Standard e do .NET Core podem exigir a adição de "Imports" a Project. JSON como uma solução alternativa temporária.

Ao adicionar o EF, a restauração do NuGet exibirá esta mensagem de erro:

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

A solução alternativa é importar manualmente o perfil portátil "Portable-net451 + win8". Isso forçará o NuGet a tratar esses binários que correspondam a esse fornecimento como uma estrutura compatível com .NET Standard, embora não sejam. Embora "Portable-net451 + win8" não seja 100% compatível com .NET Standard, ele é compatível o suficiente para a transição de PCL para .NET Standard. As importações podem ser removidas quando as dependências do EF eventualmente são atualizadas para .NET Standard.

Várias estruturas podem ser adicionadas a "Imports" na sintaxe de matriz. Outras importações poderão ser necessárias se você adicionar mais bibliotecas ao seu projeto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Consulte o [problema #5176](https://github.com/aspnet/EntityFramework/issues/5176).
