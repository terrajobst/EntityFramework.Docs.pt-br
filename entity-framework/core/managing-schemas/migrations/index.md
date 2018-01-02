---
title: "Migrações – Core EF"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3e75947e65b8e7707292fec8d74b170aff191395
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
---
<a name="migrations"></a><span data-ttu-id="8ffd2-102">Migrações</span><span class="sxs-lookup"><span data-stu-id="8ffd2-102">Migrations</span></span>
==========
<span data-ttu-id="8ffd2-103">Migrações oferecem uma maneira de aplicar incrementalmente alterações de esquema ao banco de dados para mantê-lo em sincronia com seu modelo EF Core enquanto preserva os dados existentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="8ffd2-104">Criando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="8ffd2-104">Creating the database</span></span>
---------------------
<span data-ttu-id="8ffd2-105">Depois de ter [definido seu modelo inicial][1], é hora de criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="8ffd2-106">Para fazer isso, adicione uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="8ffd2-107">Instalar as [Ferramentas do EF Core][2] e execute o comando apropriado.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="8ffd2-108">Três arquivos são adicionados ao seu projeto no diretório **Migrações**:</span><span class="sxs-lookup"><span data-stu-id="8ffd2-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="8ffd2-109">**00000000000000_InitialCreate.CS**– o arquivo principal de migrações.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="8ffd2-110">Contém as operações necessárias para aplicar a migração (em `Up()`) e revertê-la (em `Down()`).</span><span class="sxs-lookup"><span data-stu-id="8ffd2-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="8ffd2-111">**00000000000000_InitialCreate.Designer.CS**– o arquivo de metadados de migrações.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="8ffd2-112">Contém informações usadas pelo EF.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-112">Contains information used by EF.</span></span>
* <span data-ttu-id="8ffd2-113">**MyContextModelSnapshot.cs**– um instantâneo do seu modelo atual.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="8ffd2-114">Usado para determinar o que mudou ao adicionar a próxima migração.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="8ffd2-115">O carimbo de data/hora no nome de arquivo ajuda a mantê-lo organizado por ordem cronológica para que você possa ver o andamento das alterações.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="8ffd2-116">Você é livre para mover os arquivos de Migrações e alterar o namespace deles.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="8ffd2-117">Novas migrações são criadas como irmãs da última migração.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="8ffd2-118">Em seguida, aplique a migração ao banco de dados para criar o esquema.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="8ffd2-119">Adicionar outra migração</span><span class="sxs-lookup"><span data-stu-id="8ffd2-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="8ffd2-120">Depois de fazer alterações ao seu modelo do EF Core, o esquema de banco de dados ficará fora de sincronia. Para atualizá-las, adicione outra migração.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="8ffd2-121">O nome da migração pode ser usado como uma mensagem de confirmação em um sistema de controle de versão.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="8ffd2-122">Por exemplo, se eu tiver feito alterações para salvar as revisões do cliente de produtos, poderei escolher algo como *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="8ffd2-123">Depois do scaffolding da migração, você deverá examinar sua precisão e adicionar todas as operações extras necessárias para aplicá-la corretamente.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="8ffd2-124">Por exemplo, sua migração pode conter as seguintes operações:</span><span class="sxs-lookup"><span data-stu-id="8ffd2-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="8ffd2-125">Embora essas operações tornem o esquema de banco de dados compatível, elas não preservam os nomes de cliente existentes.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="8ffd2-126">Para torná-las melhor, regrave-as da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="8ffd2-127">Adicionar uma nova migração avisa quando é feito o scaffolding da operação que pode resultar em perda de dados (como remover uma coluna).</span><span class="sxs-lookup"><span data-stu-id="8ffd2-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="8ffd2-128">Não deixe de examinar especialmente a precisão dessas migrações.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="8ffd2-129">Aplique a migração ao banco de dados usando o comando apropriado.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="8ffd2-130">Remover uma migração</span><span class="sxs-lookup"><span data-stu-id="8ffd2-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="8ffd2-131">Às vezes, você adiciona uma migração e percebe que precisa fazer alterações adicionais ao modelo do EF Core antes de aplicá-lo.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="8ffd2-132">Para remover a última migração, use este comando.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="8ffd2-133">Após removê-la, você poderá fazer as alterações adicionais ao modelo e adicione-lo novamente.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="8ffd2-134">Reverter a migração</span><span class="sxs-lookup"><span data-stu-id="8ffd2-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="8ffd2-135">Se você já tiver aplicado uma migração (ou várias migrações) no banco de dados, mas precisar revertê-la, poderá usar o mesmo comando usado para aplicar as migrações, mas especificando o nome da migração para a qual você deseja reverter.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="8ffd2-136">Migrações vazias</span><span class="sxs-lookup"><span data-stu-id="8ffd2-136">Empty migrations</span></span>
----------------
<span data-ttu-id="8ffd2-137">Às vezes é útil adicionar uma migração sem fazer nenhuma alteração ao modelo.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="8ffd2-138">Nesse caso, adicionar uma nova migração cria uma vazia.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="8ffd2-139">Você pode personalizar essa migração para executar operações que não estejam diretamente relacionadas ao modelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="8ffd2-140">Alguns elementos que talvez você queira gerenciar assim são:</span><span class="sxs-lookup"><span data-stu-id="8ffd2-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="8ffd2-141">Pesquisa de Texto Completo</span><span class="sxs-lookup"><span data-stu-id="8ffd2-141">Full-Text Search</span></span>
* <span data-ttu-id="8ffd2-142">Funções</span><span class="sxs-lookup"><span data-stu-id="8ffd2-142">Functions</span></span>
* <span data-ttu-id="8ffd2-143">Procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="8ffd2-143">Stored procedures</span></span>
* <span data-ttu-id="8ffd2-144">Gatilhos</span><span class="sxs-lookup"><span data-stu-id="8ffd2-144">Triggers</span></span>
* <span data-ttu-id="8ffd2-145">Exibições</span><span class="sxs-lookup"><span data-stu-id="8ffd2-145">Views</span></span>
* <span data-ttu-id="8ffd2-146">etc.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="8ffd2-147">Gerando um script SQL</span><span class="sxs-lookup"><span data-stu-id="8ffd2-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="8ffd2-148">Ao depurar suas migrações ou implantá-las em um banco de dados de produção, é útil gerar um script SQL.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="8ffd2-149">O script então pode ser verificado em mais detalhes quanto à precisão e ajustado para atender às necessidades de um banco de dados de produção.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="8ffd2-150">O script também pode ser usado em conjunto com uma tecnologia de implantação.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="8ffd2-151">O comando básico é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="8ffd2-152">Há várias opções para este comando.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-152">There are several options to this command.</span></span>

<span data-ttu-id="8ffd2-153">A migração **de** deve ser a última migração aplicada ao banco de dados antes de executar o script.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="8ffd2-154">Se nenhuma migração tiver sido aplicada, especifique `0` (esse é o padrão).</span><span class="sxs-lookup"><span data-stu-id="8ffd2-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="8ffd2-155">A migração **para** é a última migração que será aplicada ao banco de dados após a execução do script.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="8ffd2-156">O padrão é a última migração em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="8ffd2-157">Um script **idempotente** pode ser gerado como opção.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="8ffd2-158">Esse script aplicará migrações somente se elas ainda não tiverem sido aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="8ffd2-159">Isso é útil se você não souber exatamente qual foi a última migração aplicada ao banco de dados ou estiver implantando em vários bancos de dados que podem estar, cada um, em uma migração diferente.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="8ffd2-160">Aplicar migrações em tempo de execução</span><span class="sxs-lookup"><span data-stu-id="8ffd2-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="8ffd2-161">Alguns aplicativos talvez queiram aplicar migrações no tempo de execução durante a inicialização ou a primeira execução.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="8ffd2-162">Faça isso usando o método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="8ffd2-163">Cuidado, essa abordagem não é para todos.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="8ffd2-164">Embora seja ótimo para aplicativos com um banco de dados local, a maioria dos aplicativos exigirá uma estratégia de implantação mais robusta, como gerar scripts SQL.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="8ffd2-165">Não chame `EnsureCreated()` antes de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="8ffd2-166">`EnsureCreated()` ignora as Migrações para criar o esquema e faz `Migrate()` falhar.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-166">`EnsureCreated()` bypasses Migrations to create the schema and cause `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="8ffd2-167">Este método é criado sobre o serviço `IMigrator`, que pode ser usado em cenários mais avançados.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="8ffd2-168">Use `DbContext.GetService<IMigrator>()` para acessá-lo.</span><span class="sxs-lookup"><span data-stu-id="8ffd2-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
