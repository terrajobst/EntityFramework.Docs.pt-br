---
title: "Portando de EF6 EF núcleo - portando um modelo baseado em código"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portando um EF6 baseada em código modelo EF Core

Se você leu todos os avisos e você está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.

## <a name="install-ef-core-nuget-packages"></a>Instalar os pacotes NuGet do núcleo de EF

Para usar o EF Core, você pode instalar o pacote NuGet para o provedor de banco de dados que você deseja usar. Por exemplo, durante o direcionamento do SQL Server, você deve instalar `Microsoft.EntityFrameworkCore.SqlServer`. Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.

Se você estiver planejando usar migrações, você também deve instalar o `Microsoft.EntityFrameworkCore.Tools` pacote.

Há problema em manter o pacote do NuGet EF6 (EntityFramework) instalado, como EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo. No entanto, se você não pretender usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.

## <a name="swap-namespaces"></a>Namespaces de troca

A maioria das APIs que você use EF6 estão no `System.Data.Entity` namespace (e relacionados subnamespaces). A primeira alteração de código é alternar para o `Microsoft.EntityFrameworkCore` namespace. Você teria iniciam normalmente com seu arquivo de código do contexto derivada e, em seguida, calcular a partir daí, erros de compilação de endereçamento que eles ocorrem.

## <a name="context-configuration-connection-etc"></a>Configuração de contexto (conexão etc.)

Conforme descrito em [garantir EF será trabalho para seu aplicativo](ensure-requirements.md), Core EF tem menos magic em torno de detectar o banco de dados para se conectar ao. Você precisará substituir o `OnConfiguring` método no contexto derivado e use a API específica do provedor de banco de dados para a conexão ao banco de dados de configuração.

A maioria dos aplicativos de EF6 armazenar a cadeia de caracteres de conexão em aplicativos `App/Web.config` arquivo. No núcleo do EF, leia esta cadeia de caracteres de conexão usando o `ConfigurationManager` API. Talvez seja necessário adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.

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

## <a name="update-your-code"></a>Atualize seu código

Neste ponto, é uma questão de abordar os erros de compilação e revisão de código para verificar se as alterações de comportamento afetam você.

## <a name="existing-migrations"></a>Migrações existentes

Realmente, não existe uma forma viável para a porta existentes migrações EF6 EF Core.

Se possível, é melhor presumir que todas as migrações anteriores de EF6 foram aplicadas ao banco de dados e iniciar migrando o esquema do que aponte usando EF Core. Para fazer isso, você usaria o `Add-Migration` comando para adicionar uma migração, depois que o modelo é movido para o EF Core. Em seguida, você poderia remover todo o código do `Up` e `Down` métodos de migração scaffolding. Migrações subsequentes comparará no modelo quando essa migração inicial foi Scaffold.

## <a name="test-the-port"></a>A porta de teste

Só porque seu aplicativo é compilado, não significa-lo com êxito é movido para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento ter afetado negativamente seu aplicativo.
