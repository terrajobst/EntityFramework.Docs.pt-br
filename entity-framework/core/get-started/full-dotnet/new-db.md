---
title: "Introdução ao .NET Framework – Novo banco de dados – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introdução ao EF Core no .NET Framework com um novo banco de dados

Neste passo a passo, você compilará um aplicativo de console que executa acesso a dados básicos em um banco de dados do Microsoft SQL Server usando o Entity Framework. Você usará as migrações para criar o banco de dados de seu modelo.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Versão mais recente do Gerenciador de Pacotes NuGet](https://dist.nuget.org/index.html)

* [Versão mais recente do Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abrir o Visual Studio

* Arquivo > Novo > Projeto...

* No menu à esquerda, selecione Modelos > Visual C# > Área de Trabalho Clássica do Windows

* Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**

* Certifique-se de que esteja direcionando para o **.NET Framework 4.5.1** ou posterior

* Dê um nome ao projeto e clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Mais adiante neste passo a passo, usaremos também algumas Entity Framework Tools para manter o banco de dados. Portanto, também instalaremos o pacote de ferramentas.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Criar seu modelo

Agora é hora de definir um contexto e classes de entidade que compõem seu modelo.

* Projeto > Adicionar Classe...

* Insira *Model.cs* como o nome e clique em **OK**

* Substitua o conteúdo do arquivo pelo seguinte código

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

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

> [!TIP]  
> Em um aplicativo real, você colocaria cada classe em um arquivo separado, e colocaria a cadeia de conexão no arquivo `App.Config`, e a leria usando `ConfigurationManager`. Para simplificar, estamos colocando tudo em um único arquivo de código para este tutorial.

## <a name="create-your-database"></a>Criar seu banco de dados

Agora que você tem um modelo, use as migrações para criar um banco de dados para você.

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute `Add-Migration MyFirstMigration` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.

* Execute `Update-Database` para aplicar a nova migração ao banco de dados. Como o banco de dados ainda não existe, ele será criado para você antes que a migração seja aplicada.

> [!TIP]  
> Se você fizer alterações futuras em seu modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração para fazer as alterações correspondentes de esquema no banco de dados. Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `Update-Database` para aplicar as alterações no banco de dados.
>
>O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.

## <a name="use-your-model"></a>Usar o modelo

Agora você pode usar o modelo para executar o acesso aos dados.

* Abra *Program.cs*

* Substitua o conteúdo do arquivo pelo seguinte código

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Depurar > Iniciar Sem Depuração

Você verá que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.

![imagem](_static/output-new-db.png)
