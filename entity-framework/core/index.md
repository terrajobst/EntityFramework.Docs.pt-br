---
title: "Visão Geral Rápida – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a>Visão geral rápida do Entity Framework Core

O Entity Framework (EF) Core é uma versão de multiplaforma leve, extensível e de plataforma cruzada da popular tecnologia de acesso a dados do Entity Framework.

O EF Core é O/RM (mapeador relacional de objeto) que permite que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos .NET. Elimina a necessidade da maioria do código de acesso a dados que os desenvolvedores geralmente precisam gravar. O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).

Se você quiser aprender escrevendo código, recomendamos um dos nossos guias de [Introdução](get-started/index.md) para ajudá-lo a começar a usar o EF Core.

## <a name="what-is-new-in-ef-core"></a>Novidades no EF Core

Se você estiver familiarizado com EF Core e quiser ir diretamente para os detalhes dos lançamentos mais recentes:

- **[Novidades no EF Core 2.1 (atualmente em versão prévia)](xref:core/what-is-new/ef-core-2.1)**
- **[Novidades no EF Core 2.0 (a versão mais recente)](xref:core/what-is-new/ef-core-2.0)**
- **[Fazer upgrade dos aplicativos existentes para o EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Obter o Entity Framework Core

[Instale o pacote NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) para o provedor de banco de dados que você deseja usar. Por exemplo, para instalar o provedor do SQL Server no desenvolvimento de multiplaforma com a ferramenta `dotnet` na linha de comando:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Ou, no Visual Studio, usando o Console do Gerenciador de Pacotes:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Consulte [Provedores de Banco de Dados](providers/index.md) para obter informações sobre provedores disponíveis e [Instalar o EF Core](get-started/install/index.md) para etapas de instalação mais detalhadas.

## <a name="the-model"></a>O modelo

Com o EF Core, o acesso a dados é executado usando um modelo. Um modelo é composto por classes de entidade e um contexto derivado que representa uma sessão com o banco de dados, permitindo que você consulte e salve os dados. Consulte [Criar um modelo](modeling/index.md) para saber mais.

Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com seu banco de dados ou usar Migrações do EF para criar um banco de dados do seu modelo (e evolui-lo conforme o modelo muda com o tempo).

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a>Consultar

Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem). Consulte [Consultar dados](querying/index.md) para saber mais.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Salvar Dados

Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade. Consulte [Salvar dados](saving/index.md) para saber mais.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
