---
title: Automática as migrações Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283908"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="5307f-102">Migrações automáticas do Code First</span><span class="sxs-lookup"><span data-stu-id="5307f-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="5307f-103">As migrações automáticas permite que você use as migrações Code First sem a necessidade de um arquivo de código em seu projeto para cada alteração feita.</span><span class="sxs-lookup"><span data-stu-id="5307f-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="5307f-104">Nem todas as alterações podem ser aplicadas automaticamente – por exemplo, renomeações de coluna exigem o uso de uma migração baseada em código.</span><span class="sxs-lookup"><span data-stu-id="5307f-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="5307f-105">Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos.</span><span class="sxs-lookup"><span data-stu-id="5307f-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="5307f-106">Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="5307f-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="5307f-107">Recomendação para ambientes de equipe</span><span class="sxs-lookup"><span data-stu-id="5307f-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="5307f-108">Você pode intercalar migrações automáticas e baseada em código, mas isso não é recomendado em cenários de desenvolvimento de equipe.</span><span class="sxs-lookup"><span data-stu-id="5307f-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="5307f-109">Se você fizer parte de uma equipe de desenvolvedores que usam o controle do código-fonte você deve usar migrações automáticas puramente ou puramente baseada em código.</span><span class="sxs-lookup"><span data-stu-id="5307f-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="5307f-110">Dada as limitações das migrações automáticas, é recomendável usar as migrações baseadas em código em ambientes de equipe.</span><span class="sxs-lookup"><span data-stu-id="5307f-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="5307f-111">Criando um modelo inicial e um banco de dados</span><span class="sxs-lookup"><span data-stu-id="5307f-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="5307f-112">Antes de começar a usar as migrações, é necessário um projeto e um modelo do Code First com os quais trabalhar.</span><span class="sxs-lookup"><span data-stu-id="5307f-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="5307f-113">Para este passo a passo, usaremos o **Blog** canônico e o modelo **Post**.</span><span class="sxs-lookup"><span data-stu-id="5307f-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="5307f-114">Criar um novo **MigrationsAutomaticDemo** aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="5307f-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="5307f-115">Adicione a versão mais recente do pacote NuGet **EntityFramework** ao projeto</span><span class="sxs-lookup"><span data-stu-id="5307f-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="5307f-116">**Ferramentas –&gt; Gerenciador de Pacotes de Biblioteca –&gt; Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="5307f-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="5307f-117">Execute o comando **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="5307f-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="5307f-118">Adicione um arquivo **Model.cs** com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5307f-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="5307f-119">Esse código define uma única classe **Blog** que compõe o nosso modelo de domínio e uma classe **BlogContext** que é o nosso contexto do EF Code First</span><span class="sxs-lookup"><span data-stu-id="5307f-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="5307f-120">Agora que temos um modelo, é hora de usá-lo para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="5307f-121">Atualize o arquivo **Program.cs** com o código mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5307f-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="5307f-122">Executar o aplicativo e você verá que um **MigrationsAutomaticCodeDemo.BlogContext** banco de dados é criado para você.</span><span class="sxs-lookup"><span data-stu-id="5307f-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Banco de dados LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="5307f-124">Habilitando as migrações</span><span class="sxs-lookup"><span data-stu-id="5307f-124">Enabling Migrations</span></span>

<span data-ttu-id="5307f-125">É hora de fazer mais algumas alterações em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="5307f-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="5307f-126">Vamos introduzir uma propriedade URL para a classe Blog.</span><span class="sxs-lookup"><span data-stu-id="5307f-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="5307f-127">Se você fosse executar o aplicativo novamente, obteria um InvalidOperationException informando *O modelo dando suporte ao contexto 'BlogContext' foi alterado desde que o banco de dados foi criado. Considere usar as Migrações do Code First para atualizar o banco de dados (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span><span class="sxs-lookup"><span data-stu-id="5307f-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="5307f-128">Como a exceção sugere, é hora de começar a usar as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="5307f-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="5307f-129">Como queremos usar as migrações automáticas, vamos especificar as **EnableAutomaticMigrations –** alternar.</span><span class="sxs-lookup"><span data-stu-id="5307f-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="5307f-130">Execute o **Enable-Migrations – EnableAutomaticMigrations** comando no pacote Manager Console este comando tiver adicionado um **migrações** pasta ao nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="5307f-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="5307f-131">Essa nova pasta contém um arquivo:</span><span class="sxs-lookup"><span data-stu-id="5307f-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="5307f-132">**A classe Configuration.**</span><span class="sxs-lookup"><span data-stu-id="5307f-132">**The Configuration class.**</span></span> <span data-ttu-id="5307f-133">Essa classe permite que você configure o comportamento das Migrações para seu contexto.</span><span class="sxs-lookup"><span data-stu-id="5307f-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="5307f-134">Para este passo a passo, usaremos apenas a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="5307f-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="5307f-135">*Como há apenas um único contexto do Code First no projeto, Enable-Migrations preencheu automaticamente o tipo de contexto ao qual essa configuração se aplica.*</span><span class="sxs-lookup"><span data-stu-id="5307f-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="5307f-136">A primeira migração automática</span><span class="sxs-lookup"><span data-stu-id="5307f-136">Your First Automatic Migration</span></span>

<span data-ttu-id="5307f-137">As Migrações do Code First têm dois comandos principais com os quais você se familiarizará.</span><span class="sxs-lookup"><span data-stu-id="5307f-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="5307f-138">**Add-Migration** realizará o scaffolding da próxima migração com base nas alterações feitas no modelo desde a criação da última migração</span><span class="sxs-lookup"><span data-stu-id="5307f-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="5307f-139">**Update-Database** aplicará quaisquer migrações pendentes ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="5307f-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="5307f-140">Vamos para evitar o uso de Add-Migration (a menos que realmente precisamos) e me concentrar nas permitindo que as migrações Code First automaticamente calcular e aplicar as alterações.</span><span class="sxs-lookup"><span data-stu-id="5307f-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="5307f-141">Vamos usar **Update-Database** para obter as migrações do Code First para enviar por push as alterações ao nosso modelo (o novo **Blog.Ur**propriedade l) para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="5307f-142">Execute o **Update-Database** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="5307f-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="5307f-143">O **MigrationsAutomaticDemo.BlogContext** banco de dados foi atualizado para incluir a **Url** coluna o **Blogs** tabela.</span><span class="sxs-lookup"><span data-stu-id="5307f-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="5307f-144">A segunda migração automática</span><span class="sxs-lookup"><span data-stu-id="5307f-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="5307f-145">Vamos fazer outra alteração e permitem que as migrações Code First automaticamente enviar por push as alterações para o banco de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="5307f-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="5307f-146">Vamos adicionar também uma nova classe **Post**</span><span class="sxs-lookup"><span data-stu-id="5307f-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="5307f-147">Também adicionaremos uma coleção **Posts** à classe **Blog** para formar a outra extremidade do relacionamento entre **Blog** e **Post**</span><span class="sxs-lookup"><span data-stu-id="5307f-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="5307f-148">Agora use **Update-Database** para atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="5307f-149">Dessa vez, vamos especificar o sinalizador **–Verbose** para que você possa ver o SQL que as Migrações do Code First estão executando.</span><span class="sxs-lookup"><span data-stu-id="5307f-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="5307f-150">Execute o comando **Update-Database –Verbose** no Console do Gerenciador de Pacotes.</span><span class="sxs-lookup"><span data-stu-id="5307f-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="5307f-151">Adicionar um código de migração baseada em</span><span class="sxs-lookup"><span data-stu-id="5307f-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="5307f-152">Agora vamos dar uma olhada em algo, talvez queira usar uma migração baseada em código para.</span><span class="sxs-lookup"><span data-stu-id="5307f-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="5307f-153">Vamos adicionar um **Rating** propriedade para o **Blog** classe</span><span class="sxs-lookup"><span data-stu-id="5307f-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="5307f-154">Poderíamos simplesmente executar **Update-Database** para enviar por push essas alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="5307f-155">No entanto, estamos adicionando um não-anulável **Blogs.Rating** coluna, se houver quaisquer dados existentes na tabela a ele será atribuído o padrão CLR do tipo de dados para a nova coluna (a classificação é inteiro, então isso seria **0**).</span><span class="sxs-lookup"><span data-stu-id="5307f-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="5307f-156">Mas queremos especificar um valor padrão de **3** de forma que as linhas existentes na tabela **Blogs** comecem com uma classificação decente.</span><span class="sxs-lookup"><span data-stu-id="5307f-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="5307f-157">Vamos usar o comando Add-Migration para escrever essa alteração-out para uma migração baseada em código, de modo que podemos pode editá-lo.</span><span class="sxs-lookup"><span data-stu-id="5307f-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="5307f-158">O **Add-Migration** comando nos permite dar um nome dessas migrações, vamos chamar nossa **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="5307f-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="5307f-159">Execute o **Add-Migration AddBlogRating** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="5307f-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="5307f-160">No **migrações** pasta agora temos uma nova **AddBlogRating** migração.</span><span class="sxs-lookup"><span data-stu-id="5307f-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="5307f-161">O nome do arquivo de migração é previamente corrigido com um carimbo de hora para ajudar com a ordenação.</span><span class="sxs-lookup"><span data-stu-id="5307f-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="5307f-162">Vamos editar o código gerado para especificar um valor padrão de 3 para Blog.Rating (linha 10, o código a seguir)</span><span class="sxs-lookup"><span data-stu-id="5307f-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="5307f-163">*A migração também tem um arquivo code-behind que captura alguns metadados. Esses metadados permitirá que as migrações do Code First replicar as migrações automáticas que executamos antes dessa migração baseada em código. Isso é importante se outro desenvolvedor quer executar nosso migrações ou quando é hora de implantar nosso aplicativo.*</span><span class="sxs-lookup"><span data-stu-id="5307f-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="5307f-164">Nossa migração editada parece boa, portanto, vamos usar **Update-Database** para atualizar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="5307f-165">Execute o **Update-Database** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="5307f-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="5307f-166">Para as migrações automáticas</span><span class="sxs-lookup"><span data-stu-id="5307f-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="5307f-167">Agora estamos livres para voltar para as migrações automáticas para nossas alterações mais simples.</span><span class="sxs-lookup"><span data-stu-id="5307f-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="5307f-168">Migrações do Code First se encarregará de executar as migrações automáticas e baseada em código na ordem correta com base nos metadados que ele armazena o arquivo code-behind para cada migração baseada em código.</span><span class="sxs-lookup"><span data-stu-id="5307f-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="5307f-169">Vamos adicionar uma propriedade Post.Abstract ao nosso modelo</span><span class="sxs-lookup"><span data-stu-id="5307f-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="5307f-170">Agora podemos usar **Update-Database** obter migrações do Code First para enviar a alteração ao banco de dados usando uma migração automática.</span><span class="sxs-lookup"><span data-stu-id="5307f-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="5307f-171">Execute o **Update-Database** comando no Console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="5307f-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="5307f-172">Resumo</span><span class="sxs-lookup"><span data-stu-id="5307f-172">Summary</span></span>

<span data-ttu-id="5307f-173">Neste passo a passo que você viu como usar as migrações automáticas para enviar por push o modelo for alterado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5307f-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="5307f-174">Você também viu como criar o scaffolding e executar as migrações baseadas em código entre as migrações automáticas quando você precisa de mais controle.</span><span class="sxs-lookup"><span data-stu-id="5307f-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
