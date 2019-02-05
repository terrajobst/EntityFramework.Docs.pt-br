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
# <a name="code-first-migrations"></a><span data-ttu-id="99ce9-102">Migrações do Code First</span><span class="sxs-lookup"><span data-stu-id="99ce9-102">Code First Migrations</span></span>
<span data-ttu-id="99ce9-103">Migrações do Code First é a maneira recomendada de evoluir o esquema de banco de dados do aplicativo se você está usando o fluxo de trabalho do Code First.</span><span class="sxs-lookup"><span data-stu-id="99ce9-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="99ce9-104">As migrações oferecem um conjunto de ferramentas que permitem:</span><span class="sxs-lookup"><span data-stu-id="99ce9-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="99ce9-105">Criar um banco de dados inicial que funciona com seu modelo do EF</span><span class="sxs-lookup"><span data-stu-id="99ce9-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="99ce9-106">Gerar migrações para acompanhar as alterações feitas no seu modelo do EF</span><span class="sxs-lookup"><span data-stu-id="99ce9-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="99ce9-107">Manter seu banco de dados atualizado com essas alterações</span><span class="sxs-lookup"><span data-stu-id="99ce9-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="99ce9-108">O passo a passo a seguir fornecerá uma visão geral das Migrações do Code First no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="99ce9-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="99ce9-109">Você pode concluir o passo a passo de todo ou ir para o tópico em que está interessado.</span><span class="sxs-lookup"><span data-stu-id="99ce9-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="99ce9-110">Os seguintes tópicos são abordados:</span><span class="sxs-lookup"><span data-stu-id="99ce9-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="99ce9-111">Criando um modelo inicial e um banco de dados</span><span class="sxs-lookup"><span data-stu-id="99ce9-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="99ce9-112">Antes de começar a usar as migrações, é necessário um projeto e um modelo do Code First com os quais trabalhar.</span><span class="sxs-lookup"><span data-stu-id="99ce9-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="99ce9-113">Para este passo a passo, usaremos o **Blog** canônico e o modelo **Post**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="99ce9-114">Crie um novo aplicativo de console **MigrationsDemo**</span><span class="sxs-lookup"><span data-stu-id="99ce9-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="99ce9-115">Adicione a versão mais recente do pacote NuGet **EntityFramework** ao projeto</span><span class="sxs-lookup"><span data-stu-id="99ce9-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="99ce9-116">**Ferramentas –&gt; Gerenciador de Pacotes de Biblioteca –&gt; Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="99ce9-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="99ce9-117">Execute o comando **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="99ce9-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="99ce9-118">Adicione um arquivo **Model.cs** com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="99ce9-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="99ce9-119">Esse código define uma única classe **Blog** que compõe o nosso modelo de domínio e uma classe **BlogContext** que é o nosso contexto do EF Code First</span><span class="sxs-lookup"><span data-stu-id="99ce9-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="99ce9-120">Agora que temos um modelo, é hora de usá-lo para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="99ce9-121">Atualize o arquivo **Program.cs** com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="99ce9-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="99ce9-122">Execute o aplicativo e você verá que um banco de dados **MigrationsCodeDemo.BlogContext** é criado.</span><span class="sxs-lookup"><span data-stu-id="99ce9-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Banco de dados LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="99ce9-124">Habilitando as migrações</span><span class="sxs-lookup"><span data-stu-id="99ce9-124">Enabling Migrations</span></span>

<span data-ttu-id="99ce9-125">É hora de fazer mais algumas alterações em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="99ce9-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="99ce9-126">Vamos introduzir uma propriedade URL para a classe Blog.</span><span class="sxs-lookup"><span data-stu-id="99ce9-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="99ce9-127">Se você fosse executar o aplicativo novamente, obteria um InvalidOperationException informando *O modelo dando suporte ao contexto 'BlogContext' foi alterado desde que o banco de dados foi criado. Considere usar as Migrações do Code First para atualizar o banco de dados (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span><span class="sxs-lookup"><span data-stu-id="99ce9-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="99ce9-128">Como a exceção sugere, é hora de começar a usar as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="99ce9-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="99ce9-129">A primeira etapa é habilitar migrações para o nosso contexto.</span><span class="sxs-lookup"><span data-stu-id="99ce9-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="99ce9-130">Execute o comando **Enable-Migrations** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="99ce9-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="99ce9-131">Este comando adicionou uma pasta **Migrations** ao projeto.</span><span class="sxs-lookup"><span data-stu-id="99ce9-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="99ce9-132">Essa nova pasta contém dois arquivos:</span><span class="sxs-lookup"><span data-stu-id="99ce9-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="99ce9-133">**A classe Configuration.**</span><span class="sxs-lookup"><span data-stu-id="99ce9-133">**The Configuration class.**</span></span> <span data-ttu-id="99ce9-134">Essa classe permite que você configure o comportamento das Migrações para seu contexto.</span><span class="sxs-lookup"><span data-stu-id="99ce9-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="99ce9-135">Para este passo a passo, usaremos apenas a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="99ce9-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="99ce9-136">*Como há apenas um único contexto do Code First no projeto, Enable-Migrations preencheu automaticamente o tipo de contexto ao qual essa configuração se aplica.*</span><span class="sxs-lookup"><span data-stu-id="99ce9-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="99ce9-137">**Uma migração InitialCreate**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="99ce9-138">Essa migração foi gerada porque o Code First já tinha criado um banco de dados para nós, antes de habilitarmos as migrações.</span><span class="sxs-lookup"><span data-stu-id="99ce9-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="99ce9-139">O código nesta migração com scaffolding representa os objetos que já foram criados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="99ce9-140">Em nosso caso, isso é a tabela **Blog** com as colunas **BlogId** e **Name**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="99ce9-141">O nome do arquivo inclui um carimbo de data/hora para ajudar com a ordenação.</span><span class="sxs-lookup"><span data-stu-id="99ce9-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="99ce9-142">*Se o banco de dados ainda não tivesse sido criado, essa migração InitialCreate não teria sido adicionada ao projeto. Em vez disso, na primeira vez em que chamássemos Add-Migration, o código para criar essas tabelas teria o scaffolding realizado para uma nova migração.*</span><span class="sxs-lookup"><span data-stu-id="99ce9-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="99ce9-143">Vários modelos direcionados ao mesmo banco de dados</span><span class="sxs-lookup"><span data-stu-id="99ce9-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="99ce9-144">Ao usar versões anteriores ao EF6, apenas um modelo do Code First poderia ser usado para gerar/gerenciar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="99ce9-145">Esse é o resultado de uma única tabela **\_\_MigrationsHistory** por banco de dados sem uma maneira de identificar quais entradas pertencem a qual modelo.</span><span class="sxs-lookup"><span data-stu-id="99ce9-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="99ce9-146">Começando com o EF6, a classe **Configuration** inclui uma propriedade **ContextKey**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="99ce9-147">Isso funciona como um identificador exclusivo para cada modelo do Code First.</span><span class="sxs-lookup"><span data-stu-id="99ce9-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="99ce9-148">Uma coluna correspondente na tabela **\_\_MigrationsHistory** permite que entradas de vários modelos compartilhem a tabela.</span><span class="sxs-lookup"><span data-stu-id="99ce9-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="99ce9-149">Por padrão, essa propriedade é definida como o nome totalmente qualificado de seu contexto.</span><span class="sxs-lookup"><span data-stu-id="99ce9-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="99ce9-150">Gerando e executando migrações</span><span class="sxs-lookup"><span data-stu-id="99ce9-150">Generating & Running Migrations</span></span>

<span data-ttu-id="99ce9-151">As Migrações do Code First têm dois comandos principais com os quais você se familiarizará.</span><span class="sxs-lookup"><span data-stu-id="99ce9-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="99ce9-152">**Add-Migration** realizará o scaffolding da próxima migração com base nas alterações feitas no modelo desde a criação da última migração</span><span class="sxs-lookup"><span data-stu-id="99ce9-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="99ce9-153">**Update-Database** aplicará quaisquer migrações pendentes ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="99ce9-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="99ce9-154">É preciso realizar o scaffolding de uma migração para cuidar da nova propriedade URL que adicionamos.</span><span class="sxs-lookup"><span data-stu-id="99ce9-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="99ce9-155">O comando **Add-Migration** nos permite atribuir um nome a essas migrações. Vamos chamar a nossa de **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="99ce9-156">Execute o comando **Add-Migration AddBlogUrl** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="99ce9-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="99ce9-157">Agora, na pasta **Migrations** temos uma nova migração **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="99ce9-158">O nome do arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação</span><span class="sxs-lookup"><span data-stu-id="99ce9-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="99ce9-159">Agora podemos editar ou adicionar a essa migração, mas tudo parece muito bom.</span><span class="sxs-lookup"><span data-stu-id="99ce9-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="99ce9-160">Vamos usar **Update-Database** para aplicar essa migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="99ce9-161">Execute o comando **Update-Database** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="99ce9-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="99ce9-162">As Migrações do Code First vão comparar as migrações em nossa pasta **Migrations** com aquelas que foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="99ce9-163">Isso verá que a migração **AddBlogUrl** precisa ser aplicada e a executará.</span><span class="sxs-lookup"><span data-stu-id="99ce9-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="99ce9-164">O banco de dados **MigrationsDemo.BlogContext** foi atualizado para incluir a coluna **Url** na tabela **Blogs**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="99ce9-165">Personalizando as migrações</span><span class="sxs-lookup"><span data-stu-id="99ce9-165">Customizing Migrations</span></span>

<span data-ttu-id="99ce9-166">Até agora geramos e executamos uma migração sem fazer nenhuma alteração.</span><span class="sxs-lookup"><span data-stu-id="99ce9-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="99ce9-167">Agora, vamos analisar a edição do código gerada por padrão.</span><span class="sxs-lookup"><span data-stu-id="99ce9-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="99ce9-168">É hora de fazer mais algumas alterações em nosso modelo. Vamos adicionar uma nova propriedade **Rating** à classe **Blog**</span><span class="sxs-lookup"><span data-stu-id="99ce9-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="99ce9-169">Vamos adicionar também uma nova classe **Post**</span><span class="sxs-lookup"><span data-stu-id="99ce9-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="99ce9-170">Também adicionaremos uma coleção **Posts** à classe **Blog** para formar a outra extremidade do relacionamento entre **Blog** e **Post**</span><span class="sxs-lookup"><span data-stu-id="99ce9-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="99ce9-171">Usaremos o comando **Add-Migration** para permitir que as Migrações do Code First realizem o scaffolding de sua melhor estimativa na migração.</span><span class="sxs-lookup"><span data-stu-id="99ce9-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="99ce9-172">Chamaremos essa migração de **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="99ce9-173">Execute o comando **Add-Migration AddPostClass** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="99ce9-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="99ce9-174">As Migrações do Code First fizeram um trabalho muito bom de realização de scaffolding dessas alterações, mas há algumas coisas que queremos alterar:</span><span class="sxs-lookup"><span data-stu-id="99ce9-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="99ce9-175">Primeiro, vamos adicionar um índice exclusivo à coluna **Posts.Title** (adicionando o código abaixo nas linhas 22 e 29).</span><span class="sxs-lookup"><span data-stu-id="99ce9-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="99ce9-176">Adicionaremos também uma coluna **Blogs.Rating** que não permite valores nulos.</span><span class="sxs-lookup"><span data-stu-id="99ce9-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="99ce9-177">Se houver qualquer dado existente na tabela, ele receberá o CLR padrão do tipo de dados da nova coluna (Rating é um inteiro, portanto seria **0**).</span><span class="sxs-lookup"><span data-stu-id="99ce9-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="99ce9-178">Mas queremos especificar um valor padrão de **3** de forma que as linhas existentes na tabela **Blogs** comecem com uma classificação decente.</span><span class="sxs-lookup"><span data-stu-id="99ce9-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="99ce9-179">(Você pode ver o valor padrão especificado na linha 24 do código abaixo)</span><span class="sxs-lookup"><span data-stu-id="99ce9-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="99ce9-180">Nossa migração editada está pronta, portanto, vamos usar **Update-Database** para atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="99ce9-181">Dessa vez, vamos especificar o sinalizador **–Verbose** para que você possa ver o SQL que as Migrações do Code First estão executando.</span><span class="sxs-lookup"><span data-stu-id="99ce9-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="99ce9-182">Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="99ce9-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="99ce9-183">Movimento de dados/SQL personalizado</span><span class="sxs-lookup"><span data-stu-id="99ce9-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="99ce9-184">Até agora, vimos as operações de migração que não alteram nem movem dados, agora vamos ver algo que precisa mover alguns dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="99ce9-185">Ainda não há nenhum suporte nativo para a movimentação de dados, mas podemos executar alguns comandos SQL arbitrários em qualquer ponto no nosso script.</span><span class="sxs-lookup"><span data-stu-id="99ce9-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="99ce9-186">Vamos adicionar uma propriedade **Post.Abstract** ao nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="99ce9-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="99ce9-187">Posteriormente, vamos pré-popular a **Abstract** para postagens existentes usando texto do início da coluna **Content**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="99ce9-188">Usaremos o comando **Add-Migration** para permitir que as Migrações do Code First realizem o scaffolding de sua melhor estimativa na migração.</span><span class="sxs-lookup"><span data-stu-id="99ce9-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="99ce9-189">Execute o comando **Add-Migration AddPostAbstract** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="99ce9-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="99ce9-190">A migração gerada cuida das alterações de esquema, mas também queremos pré-popular a coluna **Abstract** usando os 100 primeiros caracteres de conteúdo para cada postagem.</span><span class="sxs-lookup"><span data-stu-id="99ce9-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="99ce9-191">Podemos fazer isso indo até o SQL e executando uma instrução **UPDATE** após a coluna ser adicionada.</span><span class="sxs-lookup"><span data-stu-id="99ce9-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="99ce9-192">(Adicionando na linha 12 no código a seguir)</span><span class="sxs-lookup"><span data-stu-id="99ce9-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="99ce9-193">Nossa migração editada parece boa, portanto, vamos usar **Update-Database** para atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="99ce9-194">Especificaremos o sinalizador **–Verbose** para que possamos ver o SQL sendo executado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="99ce9-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="99ce9-195">Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="99ce9-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="99ce9-196">Migrar para uma versão específica (incluindo downgrade)</span><span class="sxs-lookup"><span data-stu-id="99ce9-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="99ce9-197">Até agora, sempre atualizamos para a migração mais recente, mas pode haver casos em que você deseja atualizar/fazer o downgrade para uma migração específica.</span><span class="sxs-lookup"><span data-stu-id="99ce9-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="99ce9-198">Vamos supor que queremos migrar nosso banco de dados para o estado em que estava após executar a migração **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="99ce9-199">Podemos usar a opção **–TargetMigration** para fazer o downgrade para essa migração.</span><span class="sxs-lookup"><span data-stu-id="99ce9-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="99ce9-200">Execute o **Update-Database –TargetMigration: Comando AddBlogUrl** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="99ce9-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="99ce9-201">Este comando executará o script Down para nossas migrações **AddBlogAbstract** e **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="99ce9-202">Se você quiser voltar totalmente para um banco de dados vazio, pode usar o comando **Update-Database –TargetMigration: $InitialDatabase**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="99ce9-203">Obtendo um script SQL</span><span class="sxs-lookup"><span data-stu-id="99ce9-203">Getting a SQL Script</span></span>

<span data-ttu-id="99ce9-204">Se outro desenvolvedor quiser essas alterações em seu computador, ele poderá apenas sincronizar após inserirmos nossas alterações no controle do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="99ce9-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="99ce9-205">Depois que tiver as novas migrações, ele poderá apenas executar o comando Update-Database para que as alterações sejam aplicadas localmente.</span><span class="sxs-lookup"><span data-stu-id="99ce9-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="99ce9-206">No entanto, se quisermos efetuar o push dessas alterações para um servidor de teste e, eventuamente, para produção, provavelmente vamos querer um script SQL que possamos entregar ao nosso DBA.</span><span class="sxs-lookup"><span data-stu-id="99ce9-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="99ce9-207">Execute o comando **Update-Database**, mas desta vez especifique o sinalizador **–Script** para que as alterações sejam gravadas e um script em vez de aplicadas.</span><span class="sxs-lookup"><span data-stu-id="99ce9-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="99ce9-208">Também especificaremos uma migração de origem e uma de destino para as quais gerar o script.</span><span class="sxs-lookup"><span data-stu-id="99ce9-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="99ce9-209">Queremos um script que vá de um banco de dados vazio (**$InitialDatabase**) para a versão mais recente (migração **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="99ce9-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="99ce9-210">*Se você não especificar uma migração de destino, as migrações usarão a última migração como o destino. Se você não especificar uma migração de origem, as migrações usarão o estado atual do banco de dados.*</span><span class="sxs-lookup"><span data-stu-id="99ce9-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="99ce9-211">Execute o **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: Comando AddPostAbstract** no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="99ce9-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="99ce9-212">As Migrações do Code First executarão o pipeline de migração, mas em vez de aplicar as alterações de fato, elas as gravarão em um arquivo .sql para você.</span><span class="sxs-lookup"><span data-stu-id="99ce9-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="99ce9-213">Depois que o script é gerado, ele é aberto no Visual Studio, pronto para você exibir ou salvar.</span><span class="sxs-lookup"><span data-stu-id="99ce9-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="99ce9-214">Geração de scripts idempotentes</span><span class="sxs-lookup"><span data-stu-id="99ce9-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="99ce9-215">Começando com o EF6, se você especificar **–SourceMigration $InitialDatabase**, o script gerado será “idempotent”.</span><span class="sxs-lookup"><span data-stu-id="99ce9-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="99ce9-216">Os scripts idempotentes podem atualizar um banco de dados atualmente em qualquer versão para a versão mais recente (ou para a especificada se **–TargetMigration** for usado).</span><span class="sxs-lookup"><span data-stu-id="99ce9-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="99ce9-217">O script gerado inclui a lógica para verificar a tabela **\_\_MigrationsHistory** e apenas aplicar alterações que não foram aplicadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="99ce9-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="99ce9-218">Atualizando automaticamente na inicialização do aplicativo (inicializador MigrateDatabaseToLatestVersion)</span><span class="sxs-lookup"><span data-stu-id="99ce9-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="99ce9-219">Se você estiver implantando seu aplicativo talvez queira atualizar automaticamente o banco de dados (aplicando quaisquer migrações pendentes) quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="99ce9-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="99ce9-220">Você pode fazer isso registrando o inicializador de banco de dados **MigrateDatabaseToLatestVersion**.</span><span class="sxs-lookup"><span data-stu-id="99ce9-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="99ce9-221">Um inicializador de banco de dados simplesmente contém alguma lógica que é usada para garantir que o banco de dados está configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="99ce9-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="99ce9-222">Essa lógica é executada na primeira vez em que o contexto é usado dentro do processo do aplicativo (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="99ce9-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="99ce9-223">Podemos atualizar o arquivo **Program.cs**, conforme mostrado abaixo, para definir o inicializador **MigrateDatabaseToLatestVersion** para BlogContext antes de usarmos o contexto (linha 14).</span><span class="sxs-lookup"><span data-stu-id="99ce9-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="99ce9-224">Observe que você também precisará adicionar uma instrução using para o namespace **System.Data.Entity** (linha 5).</span><span class="sxs-lookup"><span data-stu-id="99ce9-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="99ce9-225">*Quando criamos uma instância desse inicializador, precisamos especificar o tipo de contexto (**BlogContext**) e a configuração das migrações (**Configuration**). A configuração das migrações é a classe que foi adicionada à pasta **Migrations** quando habilitamos as migrações.*</span><span class="sxs-lookup"><span data-stu-id="99ce9-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="99ce9-226">Agora sempre que nosso aplicativo for executado, ele primeiro verificará se o banco de dados de destino está atualizado e aplicará quaisquer migrações pendentes caso não esteja.</span><span class="sxs-lookup"><span data-stu-id="99ce9-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
