---
title: Automática as migrações Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 21f77ef49db2485047292b3928b4f63d49dbb180
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489980"
---
# <a name="automatic-code-first-migrations"></a>Migrações automáticas do Code First
As migrações automáticas permite que você use as migrações Code First sem a necessidade de um arquivo de código em seu projeto para cada alteração feita. Nem todas as alterações podem ser aplicadas automaticamente – por exemplo, renomeações de coluna exigem o uso de uma migração baseada em código.

> [!NOTE]
> Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos. Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="recommendation-for-team-environments"></a>Recomendação para ambientes de equipe

Você pode intercalar migrações automáticas e baseada em código, mas isso não é recomendado em cenários de desenvolvimento de equipe. Se você fizer parte de uma equipe de desenvolvedores que usam o controle do código-fonte você deve usar migrações automáticas puramente ou puramente baseada em código. Dada as limitações das migrações automáticas, é recomendável usar as migrações baseadas em código em ambientes de equipe.

## <a name="building-an-initial-model--database"></a>Criando um modelo inicial e um banco de dados

Antes de começar a usar as migrações, é necessário um projeto e um modelo do Code First com os quais trabalhar. Para este passo a passo, usaremos o **Blog** canônico e o modelo **Post**.

-   Criar um novo **MigrationsAutomaticDemo** aplicativo de Console
-   Adicione a versão mais recente do pacote NuGet **EntityFramework** ao projeto
    -   **Ferramentas –&gt; Gerenciador de Pacotes de Biblioteca –&gt; Console do Gerenciador de Pacotes**
    -   Execute o comando **Install-Package EntityFramework**
-   Adicione um arquivo **Model.cs** com o código mostrado abaixo. Esse código define uma única classe **Blog** que compõe o nosso modelo de domínio e uma classe **BlogContext** que é o nosso contexto do EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Agora que temos um modelo, é hora de usá-lo para executar o acesso a dados. Atualize o arquivo **Program.cs** com o código mostrado abaixo.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Executar o aplicativo e você verá que um **MigrationsAutomaticCodeDemo.BlogContext** banco de dados é criado para você.

    ![Banco de dados LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Habilitando as migrações

É hora de fazer mais algumas alterações em nosso modelo.

-   Vamos introduzir uma propriedade URL para a classe Blog.

``` csharp
    public string Url { get; set; }
```

Se você fosse executar o aplicativo novamente, obteria um InvalidOperationException informando *O modelo dando suporte ao contexto 'BlogContext' foi alterado desde que o banco de dados foi criado. Considere usar as Migrações do Code First para atualizar o banco de dados (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

Como a exceção sugere, é hora de começar a usar as Migrações do Code First. Como queremos usar as migrações automáticas, vamos especificar as **EnableAutomaticMigrations –** alternar.

-   Execute o **Enable-Migrations – EnableAutomaticMigrations** comando no pacote Manager Console este comando tiver adicionado um **migrações** pasta ao nosso projeto. Essa nova pasta contém um arquivo:

-   **A classe Configuration.** Essa classe permite que você configure o comportamento das Migrações para seu contexto. Para este passo a passo, usaremos apenas a configuração padrão.
    *Como há apenas um único contexto do Code First no projeto, Enable-Migrations preencheu automaticamente o tipo de contexto ao qual essa configuração se aplica.*

 

## <a name="your-first-automatic-migration"></a>A primeira migração automática

As Migrações do Code First têm dois comandos principais com os quais você se familiarizará.

-   **Add-Migration** realizará o scaffolding da próxima migração com base nas alterações feitas no modelo desde a criação da última migração
-   **Update-Database** aplicará quaisquer migrações pendentes ao banco de dados

Vamos para evitar o uso de Add-Migration (a menos que realmente precisamos) e me concentrar nas permitindo que as migrações Code First automaticamente calcular e aplicar as alterações. Vamos usar **Update-Database** para obter as migrações do Code First para enviar por push as alterações ao nosso modelo (o novo **Blog.Ur**propriedade l) para o banco de dados.

-   Execute o **Update-Database** comando no Console do Gerenciador de pacotes.

O **MigrationsAutomaticDemo.BlogContext** banco de dados foi atualizado para incluir a **Url** coluna o **Blogs** tabela.

 

## <a name="your-second-automatic-migration"></a>A segunda migração automática

Vamos fazer outra alteração e permitem que as migrações Code First automaticamente enviar por push as alterações para o banco de dados para nós.

-   Vamos adicionar também uma nova classe **Post**

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Também adicionaremos uma coleção **Posts** à classe **Blog** para formar a outra extremidade do relacionamento entre **Blog** e **Post**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Agora use **Update-Database** para atualizar o banco de dados. Dessa vez, vamos especificar o sinalizador **–Verbose** para que você possa ver o SQL que as Migrações do Code First estão executando.

-   Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.

## <a name="adding-a-code-based-migration"></a>Adicionar um código de migração baseada em

Agora vamos dar uma olhada em algo, talvez queira usar uma migração baseada em código para.

-   Vamos adicionar um **Rating** propriedade para o **Blog** classe

``` csharp
    public int Rating { get; set; }
```

Poderíamos simplesmente executar **Update-Database** para enviar por push essas alterações no banco de dados. No entanto, estamos adicionando um não-anulável **Blogs.Rating** coluna, se houver quaisquer dados existentes na tabela a ele será atribuído o padrão CLR do tipo de dados para a nova coluna (a classificação é inteiro, então isso seria **0**). Mas queremos especificar um valor padrão de **3** de forma que as linhas existentes na tabela **Blogs** comecem com uma classificação decente.
Vamos usar o comando Add-Migration para escrever essa alteração-out para uma migração baseada em código, de modo que podemos pode editá-lo. O **Add-Migration** comando nos permite dar um nome dessas migrações, vamos chamar nossa **AddBlogRating**.

-   Execute o **Add-Migration AddBlogRating** comando no Console do Gerenciador de pacotes.
-   No **migrações** pasta agora temos uma nova **AddBlogRating** migração. O nome do arquivo de migração é previamente corrigido com um carimbo de hora para ajudar com a ordenação. Vamos editar o código gerado para especificar um valor padrão de 3 para Blog.Rating (linha 10, o código a seguir)

*A migração também tem um arquivo code-behind que captura alguns metadados. Esses metadados permitirá que as migrações do Code First replicar as migrações automáticas que executamos antes dessa migração baseada em código. Isso é importante se outro desenvolvedor quer executar nosso migrações ou quando é hora de implantar nosso aplicativo.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Nossa migração editada parece boa, portanto, vamos usar **Update-Database** para atualizar o banco de dados.

-   Execute o **Update-Database** comando no Console do Gerenciador de pacotes.

## <a name="back-to-automatic-migrations"></a>Para as migrações automáticas

Agora estamos livres para voltar para as migrações automáticas para nossas alterações mais simples. Migrações do Code First se encarregará de executar as migrações automáticas e baseada em código na ordem correta com base nos metadados que ele armazena o arquivo code-behind para cada migração baseada em código.

-   Vamos adicionar uma propriedade Post.Abstract ao nosso modelo

``` csharp
    public string Abstract { get; set; }
```

Agora podemos usar **Update-Database** obter migrações do Code First para enviar a alteração ao banco de dados usando uma migração automática.

-   Execute o **Update-Database** comando no Console do Gerenciador de pacotes.

## <a name="summary"></a>Resumo

Neste passo a passo que você viu como usar as migrações automáticas para enviar por push o modelo for alterado no banco de dados. Você também viu como criar o scaffolding e executar as migrações baseadas em código entre as migrações automáticas quando você precisa de mais controle.
