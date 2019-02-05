---
title: Migrações do Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668694"
---
# <a name="code-first-migrations"></a>Migrações do Code First
Migrações do Code First é a maneira recomendada de evoluir o esquema de banco de dados do aplicativo se você está usando o fluxo de trabalho do Code First. As migrações oferecem um conjunto de ferramentas que permitem:

1. Criar um banco de dados inicial que funciona com seu modelo do EF
2. Gerar migrações para acompanhar as alterações feitas no seu modelo do EF
2. Manter seu banco de dados atualizado com essas alterações

O passo a passo a seguir fornecerá uma visão geral das Migrações do Code First no Entity Framework. Você pode concluir o passo a passo de todo ou ir para o tópico em que está interessado. Os seguintes tópicos são abordados:

## <a name="building-an-initial-model--database"></a>Criando um modelo inicial e um banco de dados

Antes de começar a usar as migrações, é necessário um projeto e um modelo do Code First com os quais trabalhar. Para este passo a passo, usaremos o **Blog** canônico e o modelo **Post**.

-   Crie um novo aplicativo de console **MigrationsDemo**
-   Adicione a versão mais recente do pacote NuGet **EntityFramework** ao projeto
    -   **Ferramentas –&gt; Gerenciador de Pacotes de Biblioteca –&gt; Console do Gerenciador de Pacotes**
    -   Execute o comando **Install-Package EntityFramework**
-   Adicione um arquivo **Model.cs** com o código mostrado abaixo. Esse código define uma única classe **Blog** que compõe o nosso modelo de domínio e uma classe **BlogContext** que é o nosso contexto do EF Code First

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
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

      namespace MigrationsDemo
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

-   Execute o aplicativo e você verá que um banco de dados **MigrationsCodeDemo.BlogContext** é criado.

    ![Banco de dados LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Habilitando as migrações

É hora de fazer mais algumas alterações em nosso modelo.

-   Vamos introduzir uma propriedade URL para a classe Blog.

``` csharp
    public string Url { get; set; }
```

Se você fosse executar o aplicativo novamente, obteria um InvalidOperationException informando *O modelo dando suporte ao contexto 'BlogContext' foi alterado desde que o banco de dados foi criado. Considere usar as Migrações do Code First para atualizar o banco de dados (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*

Como a exceção sugere, é hora de começar a usar as Migrações do Code First. A primeira etapa é habilitar migrações para o nosso contexto.

-   Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes

    Este comando adicionou uma pasta **Migrations** ao projeto. Essa nova pasta contém dois arquivos:

-   **A classe Configuration.** Essa classe permite que você configure o comportamento das Migrações para seu contexto. Para este passo a passo, usaremos apenas a configuração padrão.
    *Como há apenas um único contexto do Code First no projeto, Enable-Migrations preencheu automaticamente o tipo de contexto ao qual essa configuração se aplica.*
-   **Uma migração InitialCreate**. Essa migração foi gerada porque o Code First já tinha criado um banco de dados para nós, antes de habilitarmos as migrações. O código nesta migração com scaffolding representa os objetos que já foram criados no banco de dados. Em nosso caso, isso é a tabela **Blog** com as colunas **BlogId** e **Name**. O nome do arquivo inclui um carimbo de data/hora para ajudar com a ordenação.
    *Se o banco de dados ainda não tivesse sido criado, essa migração InitialCreate não teria sido adicionada ao projeto. Em vez disso, na primeira vez em que chamássemos Add-Migration, o código para criar essas tabelas teria o scaffolding realizado para uma nova migração.*

### <a name="multiple-models-targeting-the-same-database"></a>Vários modelos direcionados ao mesmo banco de dados

Ao usar versões anteriores ao EF6, apenas um modelo do Code First poderia ser usado para gerar/gerenciar o esquema de banco de dados. Esse é o resultado de uma única tabela **\_\_MigrationsHistory** por banco de dados sem uma maneira de identificar quais entradas pertencem a qual modelo.

Começando com o EF6, a classe **Configuration** inclui uma propriedade **ContextKey**. Isso funciona como um identificador exclusivo para cada modelo do Code First. Uma coluna correspondente na tabela **\_\_MigrationsHistory** permite que entradas de vários modelos compartilhem a tabela. Por padrão, essa propriedade é definida como o nome totalmente qualificado de seu contexto.

## <a name="generating--running-migrations"></a>Gerando e executando migrações

As Migrações do Code First têm dois comandos principais com os quais você se familiarizará.

-   **Add-Migration** realizará o scaffolding da próxima migração com base nas alterações feitas no modelo desde a criação da última migração
-   **Update-Database** aplicará quaisquer migrações pendentes ao banco de dados

É preciso realizar o scaffolding de uma migração para cuidar da nova propriedade URL que adicionamos. O comando **Add-Migration** nos permite atribuir um nome a essas migrações. Vamos chamar a nossa de **AddBlogUrl**.

-   Execute o comando **Add-Migration AddBlogUrl** no Console do Gerenciador de Pacotes
-   Agora, na pasta **Migrations** temos uma nova migração **AddBlogUrl**. O nome do arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Blogs", "Url", c => c.String());
            }

            public override void Down()
            {
                DropColumn("dbo.Blogs", "Url");
            }
        }
    }
```

Agora podemos editar ou adicionar a essa migração, mas tudo parece muito bom. Vamos usar **Update-Database** para aplicar essa migração ao banco de dados.

-   Execute o comando **Update-Database** no Console do Gerenciador de Pacotes
-   As Migrações do Code First vão comparar as migrações em nossa pasta **Migrations** com aquelas que foram aplicadas ao banco de dados. Isso verá que a migração **AddBlogUrl** precisa ser aplicada e a executará.

O banco de dados **MigrationsDemo.BlogContext** foi atualizado para incluir a coluna **Url** na tabela **Blogs**.

## <a name="customizing-migrations"></a>Personalizando as migrações

Até agora geramos e executamos uma migração sem fazer nenhuma alteração. Agora, vamos analisar a edição do código gerada por padrão.

-   É hora de fazer mais algumas alterações em nosso modelo. Vamos adicionar uma nova propriedade **Rating** à classe **Blog**

``` csharp
    public int Rating { get; set; }
```

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

Usaremos o comando **Add-Migration** para permitir que as Migrações do Code First realizem o scaffolding de sua melhor estimativa na migração. Chamaremos essa migração de **AddPostClass**.

-   Execute o comando **Add-Migration AddPostClass** no Console do Gerenciador de Pacotes.

As Migrações do Code First fizeram um trabalho muito bom de realização de scaffolding dessas alterações, mas há algumas coisas que queremos alterar:

1.  Primeiro, vamos adicionar um índice exclusivo à coluna **Posts.Title** (adicionando o código abaixo nas linhas 22 e 29).
2.  Adicionaremos também uma coluna **Blogs.Rating** que não permite valores nulos. Se houver qualquer dado existente na tabela, ele receberá o CLR padrão do tipo de dados da nova coluna (Rating é um inteiro, portanto seria **0**). Mas queremos especificar um valor padrão de **3** de forma que as linhas existentes na tabela **Blogs** comecem com uma classificação decente.
    (Você pode ver o valor padrão especificado na linha 24 do código abaixo)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

Nossa migração editada está pronta, portanto, vamos usar **Update-Database** para atualizar o banco de dados. Dessa vez, vamos especificar o sinalizador **–Verbose** para que você possa ver o SQL que as Migrações do Code First estão executando.

-   Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.

## <a name="data-motion--custom-sql"></a>Movimento de dados/SQL personalizado

Até agora, vimos as operações de migração que não alteram nem movem dados, agora vamos ver algo que precisa mover alguns dados. Ainda não há nenhum suporte nativo para a movimentação de dados, mas podemos executar alguns comandos SQL arbitrários em qualquer ponto no nosso script.

-   Vamos adicionar uma propriedade **Post.Abstract** ao nosso modelo. Posteriormente, vamos pré-popular a **Abstract** para postagens existentes usando texto do início da coluna **Content**.

``` csharp
    public string Abstract { get; set; }
```

Usaremos o comando **Add-Migration** para permitir que as Migrações do Code First realizem o scaffolding de sua melhor estimativa na migração.

-   Execute o comando **Add-Migration AddPostAbstract** no Console do Gerenciador de Pacotes.
-   A migração gerada cuida das alterações de esquema, mas também queremos pré-popular a coluna **Abstract** usando os 100 primeiros caracteres de conteúdo para cada postagem. Podemos fazer isso indo até o SQL e executando uma instrução **UPDATE** após a coluna ser adicionada.
    (Adicionando na linha 12 no código a seguir)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

Nossa migração editada parece boa, portanto, vamos usar **Update-Database** para atualizar o banco de dados. Especificaremos o sinalizador **–Verbose** para que possamos ver o SQL sendo executado no banco de dados.

-   Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Migrar para uma versão específica (incluindo downgrade)

Até agora, sempre atualizamos para a migração mais recente, mas pode haver casos em que você deseja atualizar/fazer o downgrade para uma migração específica.

Vamos supor que queremos migrar nosso banco de dados para o estado em que estava após executar a migração **AddBlogUrl**. Podemos usar a opção **–TargetMigration** para fazer o downgrade para essa migração.

-   Execute o **Update-Database –TargetMigration: Comando AddBlogUrl** no Console do Gerenciador de Pacotes.

Este comando executará o script Down para nossas migrações **AddBlogAbstract** e **AddPostClass**.

Se você quiser voltar totalmente para um banco de dados vazio, pode usar o comando **Update-Database –TargetMigration: $InitialDatabase**.

## <a name="getting-a-sql-script"></a>Obtendo um script SQL

Se outro desenvolvedor quiser essas alterações em seu computador, ele poderá apenas sincronizar após inserirmos nossas alterações no controle do código-fonte. Depois que tiver as novas migrações, ele poderá apenas executar o comando Update-Database para que as alterações sejam aplicadas localmente. No entanto, se quisermos efetuar o push dessas alterações para um servidor de teste e, eventuamente, para produção, provavelmente vamos querer um script SQL que possamos entregar ao nosso DBA.

-   Execute o comando **Update-Database**, mas desta vez especifique o sinalizador **–Script** para que as alterações sejam gravadas e um script em vez de aplicadas. Também especificaremos uma migração de origem e uma de destino para as quais gerar o script. Queremos um script que vá de um banco de dados vazio (**$InitialDatabase**) para a versão mais recente (migração **AddPostAbstract**).
    *Se você não especificar uma migração de destino, as migrações usarão a última migração como o destino. Se você não especificar uma migração de origem, as migrações usarão o estado atual do banco de dados.*
-   Execute o **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: Comando AddPostAbstract** no Console do Gerenciador de Pacotes

As Migrações do Code First executarão o pipeline de migração, mas em vez de aplicar as alterações de fato, elas as gravarão em um arquivo .sql para você. Depois que o script é gerado, ele é aberto no Visual Studio, pronto para você exibir ou salvar.

### <a name="generating-idempotent-scripts"></a>Geração de scripts idempotentes

Começando com o EF6, se você especificar **–SourceMigration $InitialDatabase**, o script gerado será “idempotent”. Os scripts idempotentes podem atualizar um banco de dados atualmente em qualquer versão para a versão mais recente (ou para a especificada se **–TargetMigration** for usado). O script gerado inclui a lógica para verificar a tabela **\_\_MigrationsHistory** e apenas aplicar alterações que não foram aplicadas anteriormente.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Atualizando automaticamente na inicialização do aplicativo (inicializador MigrateDatabaseToLatestVersion)

Se você estiver implantando seu aplicativo talvez queira atualizar automaticamente o banco de dados (aplicando quaisquer migrações pendentes) quando o aplicativo for iniciado. Você pode fazer isso registrando o inicializador de banco de dados **MigrateDatabaseToLatestVersion**. Um inicializador de banco de dados simplesmente contém alguma lógica que é usada para garantir que o banco de dados está configurado corretamente. Essa lógica é executada na primeira vez em que o contexto é usado dentro do processo do aplicativo (**AppDomain**).

Podemos atualizar o arquivo **Program.cs**, conforme mostrado abaixo, para definir o inicializador **MigrateDatabaseToLatestVersion** para BlogContext antes de usarmos o contexto (linha 14). Observe que você também precisará adicionar uma instrução using para o namespace **System.Data.Entity** (linha 5).

*Quando criamos uma instância desse inicializador, precisamos especificar o tipo de contexto (**BlogContext**) e a configuração das migrações (**Configuration**). A configuração das migrações é a classe que foi adicionada à pasta **Migrations** quando habilitamos as migrações.*

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

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

Agora sempre que nosso aplicativo for executado, ele primeiro verificará se o banco de dados de destino está atualizado e aplicará quaisquer migrações pendentes caso não esteja.
