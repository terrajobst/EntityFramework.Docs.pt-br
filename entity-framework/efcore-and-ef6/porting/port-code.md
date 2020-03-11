---
title: Portando de EF6 para EF Core portando um modelo baseado em código-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419633"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portando um modelo baseado em código EF6 para EF Core

Se você leu todas as advertências e está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.

## <a name="install-ef-core-nuget-packages"></a>Instalar EF Core pacotes NuGet

Para usar EF Core, instale o pacote NuGet para o provedor de banco de dados que você deseja usar. Por exemplo, ao direcionar SQL Server, você instalará `Microsoft.EntityFrameworkCore.SqlServer`. Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.

Se você estiver planejando usar migrações, instale também o pacote `Microsoft.EntityFrameworkCore.Tools`.

É bom deixar o EntityFramework (EF6 NuGet Package) instalado, pois EF Core e EF6 podem ser usados lado a lado no mesmo aplicativo. No entanto, se você não pretende usar o EF6 em nenhuma área do seu aplicativo, a desinstalação do pacote ajudará a fornecer erros de compilação em partes de código que precisam de atenção.

## <a name="swap-namespaces"></a>Alternar namespaces

A maioria das APIs que você usa em EF6 está no namespace `System.Data.Entity` (e subnamespaces relacionados). A primeira alteração de código é alternar para o namespace `Microsoft.EntityFrameworkCore`. Normalmente, você começaria com o arquivo de código de contexto derivado e, em seguida, trabalhará a partir daí, abordando erros de compilação à medida que eles ocorrerem.

## <a name="context-configuration-connection-etc"></a>Configuração de contexto (conexão etc.)

Conforme descrito em [garantir que EF Core funcionará para seu aplicativo](ensure-requirements.md), EF Core tem menos mágica em relação à detecção do banco de dados ao qual se conectar. Será necessário substituir o método `OnConfiguring` no contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão com o banco de dados.

A maioria dos aplicativos EF6 armazena a cadeia de conexão no arquivo de `App/Web.config` de aplicativos. No EF Core, você lê essa cadeia de conexão usando a API `ConfigurationManager`. Talvez seja necessário adicionar uma referência ao assembly do `System.Configuration` Framework para poder usar essa API.

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

## <a name="update-your-code"></a>Atualizar seu código

Neste ponto, é uma questão de resolver erros de compilação e revisar o código para ver se as alterações de comportamento afetarão você.

## <a name="existing-migrations"></a>Migrações existentes

Na verdade, não há uma maneira viável de portar migrações EF6 existentes para EF Core.

Se possível, é melhor pressupor que todas as migrações anteriores de EF6 tenham sido aplicadas ao banco de dados e, em seguida, começar a migrar o esquema desse ponto usando EF Core. Para fazer isso, você usaria o comando `Add-Migration` para adicionar uma migração depois que o modelo é movido para EF Core. Em seguida, você removeria todo o código do `Up` e `Down` métodos da migração com Scaffold. As migrações subsequentes serão comparadas com o modelo quando a migração inicial for com Scaffold.

## <a name="test-the-port"></a>Testar a porta

Só porque seu aplicativo é compilado, não significa que ele foi portado com êxito para EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento afetou negativamente seu aplicativo.
