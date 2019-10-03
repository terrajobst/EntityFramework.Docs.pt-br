---
title: Atualizando de versões anteriores para EF Core 2-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 42e59b47f569ef6fcf72fc5bd5f94d3e9d807a24
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813565"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Atualizando aplicativos de versões anteriores para EF Core 2,0

Nós tiramos a oportunidade de refinar significativamente nossas APIs e comportamentos existentes em 2,0. Há algumas melhorias que podem exigir a modificação do código em seu aplicativo existente, embora nós acreditamos que, para a maioria dos aplicativos o impacto será baixo, na maioria dos casos que exigem apenas recompilação e mínimas alterações guiadas para substituir as APIs obsoletas.

A atualização de um aplicativo existente para o EF Core 2,0 pode exigir:

1. Atualizando a implementação .NET de destino do aplicativo para um que dê suporte a .NET Standard 2,0. Consulte [implementações do .NET com suporte](../platforms/index.md) para obter mais detalhes.

2. Identifique um provedor para o banco de dados de destino que é compatível com o EF Core 2,0. Consulte [EF Core 2,0 requer um provedor de banco de dados 2,0](#ef-core-20-requires-a-20-database-provider) abaixo.

3. Atualizar todos os pacotes de EF Core (tempo de execução e ferramentas) para 2,0. Consulte [instalando EF Core](../get-started/install/index.md) para obter mais detalhes.

4. Faça as alterações de código necessárias para compensar as alterações significativas descritas no restante deste documento.

## <a name="aspnet-core-now-includes-ef-core"></a>O ASP.NET Core agora inclui EF Core

Aplicativos que têm como destino o ASP.NET Core 2.0 podem usar o EF Core 2.0 sem dependências adicionais além de provedores de banco de dados de terceiros. No entanto, os aplicativos que visam versões anteriores do ASP.NET Core precisam atualizar para o ASP.NET Core 2,0 para usar o EF Core 2,0. Para obter mais detalhes sobre como atualizar ASP.NET Core aplicativos para 2,0, consulte [a documentação do ASP.NET Core no assunto](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Nova maneira de obter serviços de aplicativos no ASP.NET Core

O padrão recomendado para aplicativos web ASP.NET Core foi atualizado para 2.0, de forma que interrompeu a lógica de tempo de design que o EF Core usava na 1.x. Anteriormente no tempo de design, o EF Core tentava invocar `Startup.ConfigureServices` diretamente para acessar o provedor de serviços do aplicativo. No ASP.NET Core 2.0, a configuração é inicializada fora da classe `Startup`. Aplicativos que usam o EF Core normalmente acessam essa string de conexão de `Configuration`, portanto, `Startup` por si só não é mais suficiente. Se você atualizar um aplicativo do ASP.NET Core 1.x, poderá receber o seguinte erro ao usar as ferramentas do EF Core.

> Nenhum construtor sem parâmetros foi encontrado em ' ApplicationContext '. Adicione um construtor sem parâmetros a ' ApplicationContext ' ou adicione uma implementação de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' no mesmo assembly que ' ApplicationContext '

Um novo gancho de tempo de design foi adicionado no modelo padrão do ASP.NET Core 2.0. O método `Program.BuildWebHost` estático permite que EF Core acesse o provedor de serviços do aplicativo em tempo de design. Se você estiver atualizando um aplicativo ASP.NET Core 1. x, será necessário atualizar a `Program` classe para se parecer com o seguinte.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

A adoção desse novo padrão na atualização de aplicativos para o 2.0 é altamente recomendável e é necessária para recursos de produto como as migrações do Entity Framework Core para trabalhar. A outra alternativa comum é [implementar *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory renomeado

Para dar suporte a padrões diversos de aplicativo e fornecer aos usuários mais controle sobre como `DbContext` é usado no momento de design, nós fornecemos, no passado, a interface `IDbContextFactory<TContext>`. No momento do design, as ferramentas do EF Core descobrirão as implementações dessa interface em seu projeto e as usarão para criar objetos `DbContext`.

Essa interface tinha um nome muito geral que levava alguns usuários para tentar reutilizá-la por engano para outros cenários de criação de `DbContext`. os usuários eram surpreendidos quando as ferramentas do EF tentavam usar a implementação no momento de design e faziam com que comandos como `Update-Database` ou `dotnet ef database update` falhassem.

Para comunicar a semântica de tempo de design forte desta interface, nós a renomeamos para `IDesignTimeDbContextFactory<TContext>`.

Para a versão 2,0, `IDbContextFactory<TContext>` o ainda existe, mas está marcado como obsoleto.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions removido

Devido às alterações ASP.NET Core 2,0 descritas acima, descobrimos que `DbContextFactoryOptions` o não era mais necessário na nova `IDesignTimeDbContextFactory<TContext>` interface. Aqui estão as alternativas que você deve usar em vez disso.

| DbContextFactoryOptions | Alternativa                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Diretório de trabalho de tempo de design alterado

As alterações ASP.NET Core 2,0 também exigiam o diretório de trabalho `dotnet ef` usado pelo para se alinhar com o diretório de trabalho usado pelo Visual Studio ao executar o aplicativo. Um efeito colateral observável disso é que os nomes de recurso do SQLite agora são relativos ao diretório do projeto e não ao diretório de saída como eles usaram.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 requer um provedor de banco de dados 2,0

Para o EF Core 2.0, fizemos muitas simplificações e melhorias na maneira como os provedores de banco de dados funcionam. Isso significa que os provedores 1.0.x e 1.1.x não funcionarão com o EF Core 2.0.

Os provedores de SQL Server e SQLite são fornecidos pela equipe do EF e as versões 2,0 estarão disponíveis como parte da versão 2,0. Os provedores de terceiros de código aberto para [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)e [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) estão sendo atualizados para 2,0. Para todos os outros provedores, entre em contato com o gravador do provedor.

## <a name="logging-and-diagnostics-events-have-changed"></a>Eventos de log e diagnóstico foram alterados

Observação: essas alterações não devem afetar a maioria do código do aplicativo.

As IDs de evento para mensagens enviadas a um [ILogger](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.ilogger) foram alteradas em 2,0. As IDs de evento são exclusivas em todo o código do EF Core. Essas mensagens agora também seguem o padrão para o registro em log estruturado usado pelo MVC, por exemplo.

As categorias de agente também foram alteradas. Há agora um conjunto bem conhecido de categorias acessadas por meio de [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

Os eventos de [diagnóstico](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) agora usam os mesmos nomes de ID de evento `ILogger` que as mensagens correspondentes. As cargas de evento são todos os tipos nominais derivados de [EVENTDATA](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

IDs de evento, tipos de carga e categorias são documentados nas classes [CoreEventId](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) e [RelationalEventId](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) .

As IDs também foram movidas de Microsoft. EntityFrameworkCore. Infrastructure para o novo namespace Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core alterações da API de metadados relacionais

O EF Core 2.0 agora criará um [IModel](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) diferente para cada provedor diferente que está sendo usado. Isso normalmente é transparente para o aplicativo. Isso facilitou uma simplificação de APIs de metadados de nível inferior de modo que qualquer acesso a _conceitos de metadados relacionados comuns_ pé feito sempre por meio de uma chamada para `.Relational`, em vez de `.SqlServer`, `.Sqlite` etc. Por exemplo, o código 1.1. x como este:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Agora deve ser escrito da seguinte maneira:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Em vez de usar métodos `ForSqlServerToTable`como, os métodos de extensão agora estão disponíveis para gravar código condicional com base no provedor atual em uso. Por exemplo:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Observe que essa alteração se aplica somente a APIs/metadados que são definidos para _todos os_ provedores relacionais. A API e os metadados permanecem os mesmos quando são específicos de um único provedor. Por exemplo, índices clusterizados são específicos para o SQL Server, `ForSqlServerIsClustered` portanto `.SqlServer().IsClustered()` , e ainda devem ser usados.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Não assuma o controle do provedor de serviços do EF

EF Core usa um interno `IServiceProvider` (um contêiner de injeção de dependência) para sua implementação interna. Os aplicativos devem permitir EF Core criar e gerenciar esse provedor, exceto em casos especiais. É altamente recomendável remover todas `UseInternalServiceProvider`as chamadas para. Se um aplicativo precisar chamar `UseInternalServiceProvider`, considere o arquivamento [de um problema](https://github.com/aspnet/EntityFramework/Issues) para que possamos investigar outras maneiras de lidar com seu cenário.

Chamar `AddEntityFramework` `UseInternalServiceProvider` , `AddEntityFrameworkSqlServer`, etc. não é exigido pelo código do aplicativo, a menos que também seja chamado. Remova todas as chamadas existentes `AddEntityFramework` para `AddEntityFrameworkSqlServer`ou, etc `AddDbContext` ., ainda devem ser usadas da mesma maneira que antes.

## <a name="in-memory-databases-must-be-named"></a>Os bancos de dados na memória devem ser nomeados

O banco de dados na memória global sem nome foi removido e, em vez disso, todos os bancos de dados na memória devem ser nomeados. Por exemplo:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Isso cria/usa um banco de dados com o nome "MyDatabase". Se `UseInMemoryDatabase` for chamado novamente com o mesmo nome, o mesmo banco de dados na memória será usado, permitindo que ele seja compartilhado por várias instâncias de contexto.

## <a name="read-only-api-changes"></a>Alterações de API somente leitura

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, e `IsStoreGeneratedAlways` foram obsoletos e substituídos por [BeforeSaveBehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) e [AfterSaveBehavior](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Esses comportamentos se aplicam a qualquer propriedade (não apenas Propriedades geradas pelo repositório) e determinam como o valor da propriedade deve ser usado ao inserir em uma linha`BeforeSaveBehavior`de banco de dados () ou ao atualizar`AfterSaveBehavior`uma linha de banco de dados existente ().

As propriedades marcadas como [ValueGenerated. OnAddOrUpdate](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (por exemplo, para colunas computadas), por padrão, ignoram qualquer valor definido atualmente na propriedade. Isso significa que um valor gerado pelo armazenamento sempre será obtido, independentemente de algum valor ter sido definido ou modificado na entidade rastreada. Isso pode ser alterado definindo um diferente `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Novo comportamento de exclusão de ClientSetNull

Nas versões anteriores, [DeleteBehavior. restrict](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.deletebehavior) tinha um comportamento para entidades rastreadas pelo contexto que mais uma `SetNull` semântica de correspondência fechada. No EF Core 2,0, um novo `ClientSetNull` comportamento foi introduzido como o padrão para relações opcionais. Esse comportamento tem `SetNull` semântica para entidades rastreadas e `Restrict` comportamento para bancos de dados criados usando EF Core. Em nossa experiência, esses são os comportamentos mais esperados/úteis para entidades rastreadas e o banco de dados. `DeleteBehavior.Restrict`Agora é respeitado para entidades rastreadas quando definido para relações opcionais.

## <a name="provider-design-time-packages-removed"></a>Pacotes de tempo de design do provedor removidos

O `Microsoft.EntityFrameworkCore.Relational.Design` pacote foi removido. Seu conteúdo foi consolidado em `Microsoft.EntityFrameworkCore.Relational` e `Microsoft.EntityFrameworkCore.Design`.

Isso se propaga para os pacotes de tempo de design do provedor. Esses pacotes (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`etc.) foram removidos e seus conteúdos consolidados nos pacotes do provedor principal.

Para habilitar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold` no EF Core 2,0, você só precisa referenciar o pacote de provedor único:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
