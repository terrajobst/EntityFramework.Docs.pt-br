---
title: Migrações do Code First automático-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418996"
---
# <a name="automatic-code-first-migrations"></a>Migrações do Code First automática
As migrações automáticas permitem que você use Migrações do Code First sem ter um arquivo de código em seu projeto para cada alteração feita. Nem todas as alterações podem ser aplicadas automaticamente-por exemplo, renomeações de coluna exigem o uso de uma migração baseada em código.

> [!NOTE]
> Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos. Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="recommendation-for-team-environments"></a>Recomendação para ambientes de equipe

Você pode intercalar migrações automáticas e baseadas em código, mas isso não é recomendado em cenários de desenvolvimento de equipe. Se você fizer parte de uma equipe de desenvolvedores que usam o controle do código-fonte, deverá usar migrações puramente automáticas ou migrações puramente baseadas em código. Devido às limitações das migrações automáticas, recomendamos o uso de migrações baseadas em código em ambientes de equipe.

## <a name="building-an-initial-model--database"></a>Criando um modelo inicial e um banco de dados

Antes de começar a usar as migrações, é necessário um projeto e um modelo do Code First com os quais trabalhar. Para este passo a passo, usaremos o **Blog** canônico e o modelo **Post**.

-   Criar um novo aplicativo de console do **MigrationsAutomaticDemo**
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

-   Execute seu aplicativo e você verá que um banco de dados **MigrationsAutomaticCodeDemo. BlogContext** é criado para você.

    ![LocalDB do banco de dados](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Habilitando as migrações

É hora de fazer mais algumas alterações em nosso modelo.

-   Vamos introduzir uma propriedade URL para a classe Blog.

``` csharp
    public string Url { get; set; }
```

Se você fosse executar o aplicativo novamente, obterá uma InvalidOperationException informando que o *modelo de backup do contexto ' BlogContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o banco de dados (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Como a exceção sugere, é hora de começar a usar as Migrações do Code First. Como queremos usar as migrações automáticas, vamos especificar a opção **– EnableAutomaticMigrations** .

-   Execute o comando **Enable-Migrations – EnableAutomaticMigrations** no console do Gerenciador de pacotes este comando adicionou uma pasta **Migrations** ao nosso projeto. Essa nova pasta contém um arquivo:

-   **A classe Configuration.** Essa classe permite que você configure o comportamento das Migrações para seu contexto. Para este passo a passo, usaremos apenas a configuração padrão.
    *Como há apenas um único contexto do Code First no projeto, Enable-Migrations preencheu automaticamente o tipo de contexto ao qual essa configuração se aplica.*

 

## <a name="your-first-automatic-migration"></a>Sua primeira migração automática

As Migrações do Code First têm dois comandos principais com os quais você se familiarizará.

-   **Add-Migration** realizará o scaffolding da próxima migração com base nas alterações feitas no modelo desde a criação da última migração
-   **Update-Database** aplicará quaisquer migrações pendentes ao banco de dados

Vamos evitar o uso de Add-Migration (a menos que realmente precisemos) e se concentre em permitir que Migrações do Code First calcular e aplicar as alterações automaticamente. Vamos usar **Update-Database** para obter migrações do Code First enviar as alterações para o nosso modelo (a nova propriedade **blog. ur**l) para o banco de dados.

-   Execute o comando **Update-Database** no console do Gerenciador de pacotes.

O banco de dados **MigrationsAutomaticDemo. BlogContext** agora é atualizado para incluir a coluna **URL** na tabela **Blogs** .

 

## <a name="your-second-automatic-migration"></a>Sua segunda migração automática

Vamos fazer outra alteração e permitir que Migrações do Code First Envie automaticamente as alterações para o banco de dados para nós.

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

Agora, use **Update-Database para atualizar** o banco de dados. Dessa vez, vamos especificar o sinalizador **–Verbose** para que você possa ver o SQL que as Migrações do Code First estão executando.

-   Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.

## <a name="adding-a-code-based-migration"></a>Adicionando uma migração baseada em código

Agora, vamos ver algo que podemos querer usar uma migração baseada em código para o.

-   Vamos adicionar uma propriedade de **classificação** à classe **blog**

``` csharp
    public int Rating { get; set; }
```

Poderíamos simplesmente executar **Update-Database** para enviar essas alterações ao banco de dados. No entanto, estamos adicionando uma coluna **Blogs. rating** que não permite valor nulo, se houver dados existentes na tabela, ele receberá o padrão CLR do tipo de dados para nova coluna (a classificação é o número inteiro, de modo que seria **0**). Mas queremos especificar um valor padrão de **3** de forma que as linhas existentes na tabela **Blogs** comecem com uma classificação decente.
Vamos usar o comando Add-Migration para gravar essa alteração em uma migração baseada em código, de forma que possamos editá-la. O comando **Add-Migration** nos permite dar um nome a essas migrações, vamos chamar nossas **AddBlogRating**.

-   Execute o comando **Add-Migration AddBlogRating** no console do Gerenciador de pacotes.
-   Na pasta de **migrações** , agora temos uma nova migração **AddBlogRating** . O nome de arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação. Vamos editar o código gerado para especificar um valor padrão de 3 para blog. rating (linha 10 no código abaixo)

*A migração também tem um arquivo code-behind que captura alguns metadados. Esses metadados permitirão Migrações do Code First replicar as migrações automáticas que executamos antes dessa migração baseada em código. Isso é importante se outro desenvolvedor quiser executar nossas migrações ou quando for o momento de implantar nosso aplicativo.*

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

-   Execute o comando **Update-Database** no console do Gerenciador de pacotes.

## <a name="back-to-automatic-migrations"></a>Voltar para migrações automáticas

Agora estamos livres para voltar para as migrações automáticas para as nossas alterações mais simples. Migrações do Code First se encarregará de executar as migrações automáticas e baseadas em código na ordem correta com base nos metadados armazenados no arquivo code-behind para cada migração baseada em código.

-   Vamos adicionar uma propriedade post. Abstract ao nosso modelo

``` csharp
    public string Abstract { get; set; }
```

Agora, podemos usar **Update-Database** para obter migrações do Code First enviar essa alteração para o banco de dados usando uma migração automática.

-   Execute o comando **Update-Database** no console do Gerenciador de pacotes.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como usar migrações automáticas para enviar por push alterações de modelo para o banco de dados. Você também viu como Scaffold e executar migrações baseadas em código entre migrações automáticas quando precisar de mais controle.
