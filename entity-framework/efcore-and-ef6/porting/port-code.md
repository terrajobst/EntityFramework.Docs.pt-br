---
title: Portando de EF6 para EF Core – portando um modelo baseado em código
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997043"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Portando um modelo com base no código do EF6 para EF Core

Se você leu todos os avisos e você está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.

## <a name="install-ef-core-nuget-packages"></a>Instalar os pacotes NuGet do EF Core

Para usar o EF Core, você pode instalar o pacote do NuGet para o provedor de banco de dados que você deseja usar. Por exemplo, ao direcionar o SQL Server, você instalaria `Microsoft.EntityFrameworkCore.SqlServer`. Ver [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.

Se você estiver planejando usar migrações, você também deve instalar o `Microsoft.EntityFrameworkCore.Tools` pacote.

É suficiente deixar o pacote NuGet do EF6 (EntityFramework) instalado, como o EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo. No entanto, se você não estiver pretendendo usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.

## <a name="swap-namespaces"></a>Namespaces de permuta

A maioria das APIs que você usa no EF6 estão no `System.Data.Entity` namespace (e relacionados subnamespaces). A primeira alteração de código é alternar para o `Microsoft.EntityFrameworkCore` namespace. Você teria normalmente começa com seu arquivo de código do contexto derivado e, em seguida, descobrir a partir daí, abordar os erros de compilação, conforme elas ocorrem.

## <a name="context-configuration-connection-etc"></a>Configuração de contexto (conexão etc.)

Conforme descrito em [garantir que o EF Core será trabalho para seu aplicativo](ensure-requirements.md), o EF Core tem menos mágica em torno de detectar o banco de dados para se conectar ao. Você precisará substituir o `OnConfiguring` método no contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão ao banco de dados.

A maioria dos aplicativos EF6 armazenar a cadeia de caracteres de conexão em aplicativos de `App/Web.config` arquivo. No EF Core, leia esta cadeia de caracteres de conexão usando o `ConfigurationManager` API. Talvez você precise adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.

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

Neste ponto, é uma questão de abordar os erros de compilação e revisão de código para ver se as alterações de comportamento afetarão você.

## <a name="existing-migrations"></a>Migrações existentes

Na verdade, não existe uma forma viável a portabilidade de migrações existentes ao EF6 para EF Core.

Se possível, é melhor presumir que todas as migrações anteriores do EF6 foram aplicadas ao banco de dados e, em seguida, iniciar migrando o esquema do que aponte usando o EF Core. Para fazer isso, você usaria o `Add-Migration` comando para adicionar uma migração, depois que o modelo é movido para o EF Core. Em seguida, você poderia remover todo o código do `Up` e `Down` métodos da migração gerado por scaffolding. Migrações subsequentes comparará no modelo quando a migração inicial foi gerado automaticamente.

## <a name="test-the-port"></a>A porta de teste

Só porque seu aplicativo for compilado, não significa que ele é movido com êxito para o EF Core. Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tiver afetado negativamente seu aplicativo.
