---
title: "Atualizando de versões anteriores ao EF Core 2 - Core de EF"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 0bd1ea2476621f826cca7d4a526a49a1b902acf8
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Atualizando aplicativos de versões anteriores ao EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Procedimentos comuns a todos os aplicativos

Atualizando um aplicativo existente para EF Core 2.0 pode exigir:

1. Atualização da plataforma do .NET de destino do aplicativo para um que suporte o .NET 2.0 padrão. Consulte [plataformas com suporte](../platforms/index.md) para obter mais detalhes.

2. Identifica um provedor para o banco de dados de destino que é compatível com o EF Core 2.0. Consulte [EF Core 2.0 requer um provedor de banco de 2.0 dados](#ef-core-20-requires-a-20-database-provider) abaixo.

3. Atualizar todos os pacotes de EF Core (tempo de execução e ferramentas) 2.0. Consulte [instalando EF Core](../get-started/install/index.md) para obter mais detalhes.

4. Faça as alterações necessárias de código para compensar alterações significativas. Consulte o [alterações significativas](#breaking-changes) seção abaixo para obter mais detalhes.

## <a name="aspnet-core-applications"></a>Aplicativos do ASP.NET Core

1. Consulte em particular a [novo padrão para inicializar o provedor de serviços do aplicativo](#new-way-of-getting-application-services) descrito abaixo.

> [!TIP]  
> A adoção desse novo padrão quando atualizar aplicativos 2.0 é altamente recomendado e é necessário para recursos de produto como Entity Framework Core migrações para trabalhar. A outra alternativa comum é [implementar *IDesignTimeDbContextFactory\<TContext >*](configuring-dbcontext.md#using-idesigntimedbcontextfactorytcontext).

2. Aplicativos voltados para o ASP.NET 2.0 de núcleo podem usar o EF Core 2.0 sem dependências adicionais além de provedores de banco de dados de terceiros. No entanto, aplicativos destinados a versões anteriores do ASP.NET Core precisam atualizar para o ASP.NET 2.0 de núcleo para usar o EF Core 2.0. Para obter mais detalhes sobre como atualizar aplicativos ASP.NET Core 2.0, consulte [a documentação do ASP.NET Core no assunto](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Alterações significativas

Levamos a oportunidade de refinar significativamente os comportamentos no 2.0 e APIs existentes. Existem alguns aprimoramentos que podem exigir a modificação do código de aplicativo existente, embora acreditamos que para a maioria dos aplicativos o impacto será baixo, na maioria dos casos que exigem apenas recompilação e alterações mínimas de interativa para substituir as APIs obsoletas.

### <a name="new-way-of-getting-application-services"></a>Nova maneira de obter serviços de aplicativo

O padrão recomendado para aplicativos da web de ASP.NET Core foi atualizado para 2.0 de forma que interrompeu a lógica de tempo de design que EF principal usada em 1. x. Anteriormente em tempo de design, Core EF tentar invocar `Startup.ConfigureServices` diretamente para acessar o provedor de serviços do aplicativo. No ASP.NET 2.0 de núcleo, a configuração é inicializada fora do `Startup` classe. Aplicativos que utilizam o EF Core geralmente acessam sua cadeia de caracteres de conexão de configuração, portanto `Startup` por si só não é suficiente. Se você atualizar um aplicativo do ASP.NET Core 1. x, você receberá o seguinte erro ao usar as ferramentas de EF Core.

> Nenhum construtor sem parâmetros foi encontrado em 'ApplicationContext'. Adicione um construtor sem parâmetros para 'ApplicationContext' ou adicione uma implementação de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' no mesmo assembly como 'ApplicationContext'

Foi adicionado um novo gancho de tempo de design no modelo de padrão do ASP.NET Core 2.0. Estático `Program.BuildWebHost` método permite que o EF principal acessar o provedor de serviços do aplicativo em tempo de design. Se você estiver atualizando um aplicativo do ASP.NET Core 1. x, você precisará atualizar `Program` classe para ser semelhante à seguinte.

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

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory renomeado

Para dar suporte a padrões de diversos aplicativos e fornecer aos usuários mais controle sobre como suas `DbContext` é usada em tempo de design, nós fornecemos, no passado, o `IDbContextFactory<TContext>` interface. Em tempo de design, as ferramentas principais EF descobrirá implementações dessa interface em seu projeto e usá-lo para criar `DbContext` objetos.

Essa interface tiver um nome muito geral que alguns usuários para tentar novamente usando-o para outro de enganar `DbContext`-criação de cenários. Foram pega de surpresa quando as ferramentas de EF, em seguida, tentou usar sua implementação em tempo de design e causados comandos como `Update-Database` ou `dotnet ef database update` falha.

Para comunicar-se a semântica de tempo de design forte desta interface, podemos ter renomeado para `IDesignTimeDbContextFactory<TContext>`.

Para obter o 2.0 versão o `IDbContextFactory<TContext>` ainda existe, mas está marcado como obsoleto.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions removido

Devido às alterações de ASP.NET Core 2.0 descritas acima, descobrimos que `DbContextFactoryOptions` não era mais necessário no novo `IDesignTimeDbContextFactory<TContext>` interface. Aqui estão as alternativas, que você deve usar em vez disso.

DbContextFactoryOptions | Alternativa
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>Diretório de trabalho de tempo de design alterado

As alterações do ASP.NET Core 2.0 também necessário o diretório de trabalho usado por `dotnet ef` para se alinhar com o diretório de trabalho usado pelo Visual Studio ao executar seu aplicativo. Um efeito colateral observável disso é que SQLite nomes de arquivos agora estão em relação ao diretório do projeto e não no diretório de saída que costumavam ser.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 requer um provedor de banco de 2.0 dados

Para o EF Core 2.0 fizemos muitas simplificações e aprimoramentos nos provedores de banco de dados de maneira de trabalho. Isso significa que provedores 1.0. x e 1.1 não funcionarão com o EF Core 2.0.

Os provedores SQL Server e SQLite são fornecidos pela equipe do EF e 2.0 versões estarão disponíveis como parte do 2.0 de versão. Os provedores de terceiros do código-fonte aberto para [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), e [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) estão sendo atualizadas para 2.0. Para todos os outros provedores, entre em contato com o gravador de provedor.

### <a name="logging-and-diagnostics-events-have-changed"></a>Eventos de log e diagnóstico foram alterados

Observação: essas alterações não devem afetar a maioria dos códigos de aplicativo.

As identificações de evento das mensagens enviadas para um [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) foram alterados no 2.0. As identificações de evento são exclusivas em código EF principal. Essas mensagens agora também pode seguir o padrão para o log estruturado usado por, por exemplo, o MVC.

Categorias de agente de log também foram alterados. Há agora um conjunto conhecido de categorias acessados por meio de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eventos agora usam os mesmos nomes de ID de evento como correspondente `ILogger` mensagens. As cargas do evento são todos os tipos nominais derivados de [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Categorias, tipos de carga e IDs de eventos estão documentadas no [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) e [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classes.

IDs também foram movidos de Microsoft.EntityFrameworkCore.Infraestructure para o novo namespace Microsoft.EntityFrameworkCore.Diagnostics.

### <a name="ef-core-relational-metadata-api-changes"></a>Alterações de metadados relacionais API Core EF

EF Core 2.0 agora criará outro [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) para cada provedor diferente que está sendo usado. Isso normalmente é transparente para o aplicativo. Isso facilitou uma simplificação de APIs de metadados de nível inferior, de modo que qualquer acesso ao _comuns conceitos de metadados relacionais_ sempre é feita por meio de uma chamada para `.Relational` em vez de `.SqlServer`, `.Sqlite`, etc. Por exemplo, 1.1 código como este:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Agora deve ser escrito como este:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Em vez de usar os métodos, como `ForSqlServerToTable`, métodos de extensão estão agora disponíveis para escrever código condicional com base no provedor atual em uso. Por exemplo:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Observe que essa alteração se aplica apenas a APIs/metadados que está definido para _todos os_ provedores relacionais. A API e metadados permanece o mesmo quando ele é específico para apenas um único provedor. Por exemplo, os índices clusterizados são específicos para SQL Server, portanto `ForSqlServerIsClustered` e `.SqlServer().IsClustered()` deve ser usado.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Não assumir o controle do provedor de serviços EF

Núcleo EF usa interno `IServiceProvider` (ou seja, um contêiner de injeção de dependência) para sua implementação interna. Aplicativos devem permitir que o EF principal criar e gerenciar esse provedor, exceto em casos especiais. Altamente Considere remover todas as chamadas para `UseInternalServiceProvider`. Se um aplicativo precisa chamar `UseInternalServiceProvider`, em seguida, considere [um problema de arquivamento](https://github.com/aspnet/EntityFramework/Issues) para investigamos outras maneiras de tratar o cenário.

Chamando `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. não é necessário pelo código do aplicativo, a menos que `UseInternalServiceProvider` também é chamado. Remova as chamadas existentes para `AddEntityFramework` ou `AddEntityFrameworkSqlServer`, etc. `AddDbContext` ainda deve ser usada da mesma forma como antes.

### <a name="in-memory-databases-must-be-named"></a>Bancos de dados na memória devem ser nomeados.

O banco de dados sem nome por na memória global foi removido e, em vez disso, todos os bancos de dados na memória devem ser nomeados. Por exemplo:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Isso cria/usa um banco de dados com o nome "MyDatabase". Se `UseInMemoryDatabase` é chamado novamente com o mesmo nome, o mesmo banco de dados na memória será usado, permitindo que ele seja compartilhado por várias instâncias de contexto.

### <a name="read-only-api-changes"></a>Alterações de API de somente leitura

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, e `IsStoreGeneratedAlways` obsoletos e substituídos por [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) e [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Esses comportamentos se aplicam a qualquer propriedade (não apenas propriedades geradas pelo repositório) e determinar como o valor da propriedade deve ser usado ao inserir em uma linha de banco de dados (`BeforeSaveBehavior`) ou quando uma existente de atualização de banco de dados linha (`AfterSaveBehavior`).

Propriedades marcadas como [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (por exemplo, para colunas computadas) por padrão ignorará qualquer valor atualmente definido na propriedade. Isso significa que um valor gerado pelo repositório sempre será obtido independentemente se nenhum valor foi definido ou modificado na entidade controlada. Isso pode ser alterado definindo outro `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Novo comportamento de exclusão ClientSetNull

Em versões anteriores, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) teve um comportamento para entidades controladas pelo contexto que mais fechado correspondentes `SetNull` semântica. No EF Core 2.0, um novo `ClientSetNull` comportamento foi introduzido como padrão para relações opcionais. Esse comportamento tem `SetNull` semântica para entidades controladas e `Restrict` comportamento para bancos de dados criados usando EF Core. Em nossa experiência, esses são os comportamentos mais esperado/úteis para entidades controladas e o banco de dados. `DeleteBehavior.Restrict`Agora é cumprido para entidades controladas quando definidas para relações opcionais.

### <a name="provider-design-time-packages-removed"></a>Pacotes de tempo de design de provedor removidos

O `Microsoft.EntityFrameworkCore.Relational.Design` pacote tiver sido removido. Conteúdo do arquivo foram consolidado em `Microsoft.EntityFrameworkCore.Relational` e `Microsoft.EntityFrameworkCore.Design`.

Isso é propagada para os pacotes de tempo de design do provedor. Esses pacotes (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) foram removidas e seus conteúdos são consolidados em pacotes do provedor principal.

Para habilitar `Scaffold-DbContext` ou `dotnet ef dbcontext scaffold` no EF Core 2.0, você só precisa referenciar o pacote único provedor:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
