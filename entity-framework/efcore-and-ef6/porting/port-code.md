---
title: Portando de EF6 para EF Core - Portando um modelo baseado em código - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419633"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portando um modelo baseado em código EF6 para o Núcleo EF

Se você leu todas as ressalvas e está pronto para portar, então aqui estão algumas diretrizes para ajudá-lo a começar.

## <a name="install-ef-core-nuget-packages"></a>Instale pacotes EF Core NuGet

Para usar o EF Core, você instala o pacote NuGet para o provedor de banco de dados que deseja usar. Por exemplo, ao direcionar o SQL `Microsoft.EntityFrameworkCore.SqlServer`Server, você instalaria . Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.

Se você está planejando usar migrações, então `Microsoft.EntityFrameworkCore.Tools` você também deve instalar o pacote.

É bom deixar o pacote EF6 NuGet (EntityFramework) instalado, pois eF Core e EF6 podem ser usados lado a lado na mesma aplicação. No entanto, se você não pretende usar o EF6 em qualquer área do seu aplicativo, então a desinstalação do pacote ajudará a dar erros de compilação em pedaços de código que precisam de atenção.

## <a name="swap-namespaces"></a>Trocar espaços de nome

A maioria das APIs que você `System.Data.Entity` usa no EF6 estão no namespace (e sub-nomes relacionados). A primeira mudança de código `Microsoft.EntityFrameworkCore` é trocar para o namespace. Você normalmente começaria com seu arquivo de código de contexto derivado e, em seguida, trabalharia a partir daí, abordando erros de compilação à medida que ocorrem.

## <a name="context-configuration-connection-etc"></a>Configuração de contexto (conexão etc.)

Como descrito em [Ensure EF Core Will Work for Your Application](ensure-requirements.md), o EF Core tem menos magia ao detectar o banco de dados para se conectar. Você precisará substituir o `OnConfiguring` método em seu contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão com o banco de dados.

A maioria dos aplicativos EF6 armazena `App/Web.config` a seqüência de conexões no arquivo de aplicativos. No EF Core, você lê `ConfigurationManager` esta seqüência de conexões usando a API. Você pode precisar adicionar uma `System.Configuration` referência ao conjunto de estruturas para poder usar esta API.

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

Neste ponto, é uma questão de abordar erros de compilação e revisar código para ver se as mudanças de comportamento irão impactá-lo.

## <a name="existing-migrations"></a>Migrações existentes

Não há realmente uma maneira viável de portar migrações EF6 existentes para o EF Core.

Se possível, é melhor supor que todas as migrações anteriores do EF6 foram aplicadas ao banco de dados e, em seguida, começar a migrar o esquema a partir desse ponto usando o EF Core. Para fazer isso, você `Add-Migration` usaria o comando para adicionar uma migração assim que o modelo for portado para o EF Core. Em seguida, você removeria todo o código da `Up` `Down` migração de andaimes. As migrações subseqüentes serão comparadas com o modelo quando essa migração inicial foi cadavez mais andaime.

## <a name="test-the-port"></a>Teste a porta

Só porque seu aplicativo compila, não significa que ele seja portado com sucesso para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tenha afetado negativamente sua aplicação.
