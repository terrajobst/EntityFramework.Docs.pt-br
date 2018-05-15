---
title: Introdução ao .NET Framework – Banco de dados existente – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introdução ao EF Core no .NET Framework com um banco de dados existente

Neste passo a passo, você compilará um aplicativo de console que executa acesso a dados básicos em um banco de dados do Microsoft SQL Server usando o Entity Framework. Você usará engenharia reversa para criar um modelo do Entity Framework com base em um banco de dados existente.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) deste artigo no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará atender aos pré-requisitos a seguir para concluir este passo a passo:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Versão mais recente do Gerenciador de Pacotes NuGet](https://dist.nuget.org/index.html)

* [Versão mais recente do Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Banco de dados de blog](#blogging-database)

### <a name="blogging-database"></a>Banco de dados de blog

Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.

> [!TIP]  
> Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.

* Abrir o Visual Studio

* Ferramentas > Conectar ao Banco de Dados...

* Selecione **Microsoft SQL Server** e clique em **Continuar**

* Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**

* Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**

* O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**

* Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**

* Copie o script, listado abaixo, no editor de consultas

* Clique com o botão direito do mouse no editor de consultas e selecione **Executar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Criar um novo projeto

* Abrir o Visual Studio

* Arquivo > Novo > Projeto...

* No menu à esquerda, selecione Modelos > Visual C# > Windows

* Selecione o modelo de projeto do **Aplicativo de Console**

* Certifique-se de que esteja direcionando para o **.NET Framework 4.5.1** ou posterior

* Dê um nome ao projeto e clique em **OK**

## <a name="install-entity-framework"></a>Instalar o Entity Framework

Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar. Este passo a passo usa o SQL Server. Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Para habilitar a engenharia reversa de um banco de dados existente, precisamos instalar outros pacotes.

* Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Fazer engenharia reversa em seu modelo

Agora é hora de criar o modelo EF com base em seu banco de dados existente.

* Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes

* Execute o seguinte comando para criar um modelo do banco de dados existente

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

O processo de engenharia reversa criou classes de entidade e um contexto derivado com base no esquema do banco de dados existente. As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

O contexto representa uma sessão com o banco de dados e permite que você veja e salve as instâncias das classes de entidade.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Usar o modelo

Agora você pode usar o modelo para executar o acesso aos dados.

* Abra *Program.cs*

* Substitua o conteúdo do arquivo pelo seguinte código

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![imagem](_static/output-existing-db.png)
