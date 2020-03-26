---
title: Migrações – Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136204"
---
# <a name="migrations"></a><span data-ttu-id="e8b84-102">Migrações</span><span class="sxs-lookup"><span data-stu-id="e8b84-102">Migrations</span></span>

<span data-ttu-id="e8b84-103">Um modelo de dados muda durante o desenvolvimento e fica fora de sincronia com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="e8b84-104">Você pode descartar o banco de dados e permitir que o EF crie um novo que corresponda ao modelo, mas esse procedimento resulta na perda de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="e8b84-105">O recurso de migrações no EF Core oferece uma maneira de atualizar de forma incremental o esquema de banco de dados para mantê-lo em sincronia com o modelo de dados do aplicativo, preservando os dados existentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="e8b84-106">As migrações incluem ferramentas de linha de comando e APIs que ajudam com as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="e8b84-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="e8b84-107">[Criar uma migração](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="e8b84-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="e8b84-108">Gerar um código que pode atualizar o banco de dados para sincronizá-lo com um conjunto de alterações do modelo.</span><span class="sxs-lookup"><span data-stu-id="e8b84-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="e8b84-109">[Atualizar o banco de dados](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="e8b84-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="e8b84-110">Aplicar migrações pendentes para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="e8b84-111">[Personalizar o código de migração](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="e8b84-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="e8b84-112">Às vezes, o código gerado precisa ser modificado ou complementado.</span><span class="sxs-lookup"><span data-stu-id="e8b84-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="e8b84-113">[Remover uma migração](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="e8b84-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="e8b84-114">Excluir o código gerado.</span><span class="sxs-lookup"><span data-stu-id="e8b84-114">Delete the generated code.</span></span>
* <span data-ttu-id="e8b84-115">[Reverter uma migração](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="e8b84-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="e8b84-116">Desfazer as alterações do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-116">Undo the database changes.</span></span>
* <span data-ttu-id="e8b84-117">[Gerar scripts SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="e8b84-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="e8b84-118">Talvez seja necessário um script para atualizar um banco de dados de produção ou para solucionar problemas de código de migração.</span><span class="sxs-lookup"><span data-stu-id="e8b84-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="e8b84-119">[Aplicar migrações em runtime](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="e8b84-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="e8b84-120">Quando as atualizações de tempo de design e a execução de scripts não forem as melhores opções, chame o método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e8b84-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="e8b84-121">Se `DbContext` está em um assembly diferente do projeto de inicialização, você pode especificar explicitamente o destino e os projetos de inicialização nas [ferramentas do Console do Gerenciador de Pacotes](xref:core/miscellaneous/cli/powershell#target-and-startup-project) ou nas [ferramentas da CLI do .NET Core](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="e8b84-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="e8b84-122">Instalar as ferramentas</span><span class="sxs-lookup"><span data-stu-id="e8b84-122">Install the tools</span></span>

<span data-ttu-id="e8b84-123">Instale as [ferramentas de linha de comando](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="e8b84-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="e8b84-124">Para o Visual Studio, recomendamos as [ferramentas do Console do Gerenciador de Pacotes](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="e8b84-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="e8b84-125">Para outros ambientes de desenvolvimento, escolha as [ferramentas de CLI do .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="e8b84-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="e8b84-126">Criar uma migração</span><span class="sxs-lookup"><span data-stu-id="e8b84-126">Create a migration</span></span>

<span data-ttu-id="e8b84-127">Depois de ter [definido seu modelo inicial](xref:core/modeling/index), é hora de criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="e8b84-128">Execute o comando a seguir para adicionar uma migração inicial.</span><span class="sxs-lookup"><span data-stu-id="e8b84-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-129">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="e8b84-131">Três arquivos são adicionados ao seu projeto no diretório **Migrações**:</span><span class="sxs-lookup"><span data-stu-id="e8b84-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="e8b84-132">**XXXXXXXXXXXXXX_InitialCreate.cs**– o arquivo principal de migrações.</span><span class="sxs-lookup"><span data-stu-id="e8b84-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="e8b84-133">Contém as operações necessárias para aplicar a migração (em `Up()`) e revertê-la (em `Down()`).</span><span class="sxs-lookup"><span data-stu-id="e8b84-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="e8b84-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**– o arquivo de metadados de migrações.</span><span class="sxs-lookup"><span data-stu-id="e8b84-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="e8b84-135">Contém informações usadas pelo EF.</span><span class="sxs-lookup"><span data-stu-id="e8b84-135">Contains information used by EF.</span></span>
* <span data-ttu-id="e8b84-136">**MyContextModelSnapshot.cs**– um instantâneo do seu modelo atual.</span><span class="sxs-lookup"><span data-stu-id="e8b84-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="e8b84-137">Usado para determinar o que mudou ao adicionar a próxima migração.</span><span class="sxs-lookup"><span data-stu-id="e8b84-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="e8b84-138">O carimbo de data/hora no nome de arquivo ajuda a mantê-lo organizado por ordem cronológica para que você possa ver o andamento das alterações.</span><span class="sxs-lookup"><span data-stu-id="e8b84-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="e8b84-139">Você é livre para mover os arquivos de Migrações e alterar o namespace deles.</span><span class="sxs-lookup"><span data-stu-id="e8b84-139">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="e8b84-140">Novas migrações são criadas como irmãs da última migração.</span><span class="sxs-lookup"><span data-stu-id="e8b84-140">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="e8b84-141">Atualizar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="e8b84-141">Update the database</span></span>

<span data-ttu-id="e8b84-142">Em seguida, aplique a migração ao banco de dados para criar o esquema.</span><span class="sxs-lookup"><span data-stu-id="e8b84-142">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-143">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-143">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-144">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="e8b84-145">Personalizar o código de migração</span><span class="sxs-lookup"><span data-stu-id="e8b84-145">Customize migration code</span></span>

<span data-ttu-id="e8b84-146">Após fazer alterações no modelo do EF Core, o esquema de banco de dados pode ficar fora de sincronia. Para atualizá-las, adicione outra migração.</span><span class="sxs-lookup"><span data-stu-id="e8b84-146">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="e8b84-147">O nome da migração pode ser usado como uma mensagem de confirmação em um sistema de controle de versão.</span><span class="sxs-lookup"><span data-stu-id="e8b84-147">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="e8b84-148">Por exemplo, é possível escolher um nome como *AddProductReviews* caso a alteração seja uma nova classe de entidade para revisões.</span><span class="sxs-lookup"><span data-stu-id="e8b84-148">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-149">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-149">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-150">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="e8b84-151">Depois que o scaffolding for aplicado à migração (codificado para ela), examine a precisão do código e adicione, remova ou modifique quaisquer operações necessárias para a correta aplicação.</span><span class="sxs-lookup"><span data-stu-id="e8b84-151">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="e8b84-152">Por exemplo, uma migração poderia conter as seguintes operações:</span><span class="sxs-lookup"><span data-stu-id="e8b84-152">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="e8b84-153">Embora essas operações tornem o esquema de banco de dados compatível, elas não preservam os nomes de cliente existentes.</span><span class="sxs-lookup"><span data-stu-id="e8b84-153">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="e8b84-154">Para torná-las melhor, regrave-as da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e8b84-154">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="e8b84-155">O processo de scaffolding da migração avisa quando uma operação puder resultar em perda de dados (como o descarte de uma coluna).</span><span class="sxs-lookup"><span data-stu-id="e8b84-155">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="e8b84-156">Caso veja esse aviso, não deixe de examinar a precisão do código de migrações.</span><span class="sxs-lookup"><span data-stu-id="e8b84-156">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="e8b84-157">Aplique a migração ao banco de dados usando o comando apropriado.</span><span class="sxs-lookup"><span data-stu-id="e8b84-157">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-158">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="e8b84-160">Migrações vazias</span><span class="sxs-lookup"><span data-stu-id="e8b84-160">Empty migrations</span></span>

<span data-ttu-id="e8b84-161">Às vezes é útil adicionar uma migração sem fazer nenhuma alteração ao modelo.</span><span class="sxs-lookup"><span data-stu-id="e8b84-161">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="e8b84-162">Neste caso, adicionar uma nova migração cria arquivos de código com classes vazias.</span><span class="sxs-lookup"><span data-stu-id="e8b84-162">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="e8b84-163">Você pode personalizar essa migração para executar operações que não estejam diretamente relacionadas ao modelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="e8b84-163">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="e8b84-164">Alguns elementos que talvez você queira gerenciar assim são:</span><span class="sxs-lookup"><span data-stu-id="e8b84-164">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="e8b84-165">Pesquisa de texto completo</span><span class="sxs-lookup"><span data-stu-id="e8b84-165">Full-Text Search</span></span>
* <span data-ttu-id="e8b84-166">Funções</span><span class="sxs-lookup"><span data-stu-id="e8b84-166">Functions</span></span>
* <span data-ttu-id="e8b84-167">Procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="e8b84-167">Stored procedures</span></span>
* <span data-ttu-id="e8b84-168">Gatilhos</span><span class="sxs-lookup"><span data-stu-id="e8b84-168">Triggers</span></span>
* <span data-ttu-id="e8b84-169">Exibições</span><span class="sxs-lookup"><span data-stu-id="e8b84-169">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="e8b84-170">Remover uma migração</span><span class="sxs-lookup"><span data-stu-id="e8b84-170">Remove a migration</span></span>

<span data-ttu-id="e8b84-171">Às vezes, você adiciona uma migração e percebe que precisa fazer alterações adicionais ao modelo do EF Core antes de aplicá-lo.</span><span class="sxs-lookup"><span data-stu-id="e8b84-171">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="e8b84-172">Para remover a última migração, use este comando.</span><span class="sxs-lookup"><span data-stu-id="e8b84-172">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-173">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-173">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-174">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="e8b84-175">Após remover a migração, você poderá fazer as alterações adicionais ao modelo e adicioná-la novamente.</span><span class="sxs-lookup"><span data-stu-id="e8b84-175">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="e8b84-176">Reverter uma migração</span><span class="sxs-lookup"><span data-stu-id="e8b84-176">Revert a migration</span></span>

<span data-ttu-id="e8b84-177">Se você já tiver aplicado uma migração (ou várias migrações) no banco de dados, mas precisar revertê-la, poderá usar o mesmo comando usado para aplicar as migrações, mas especificando o nome da migração para a qual você deseja reverter.</span><span class="sxs-lookup"><span data-stu-id="e8b84-177">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-178">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-178">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-179">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="e8b84-180">Gerar scripts SQL</span><span class="sxs-lookup"><span data-stu-id="e8b84-180">Generate SQL scripts</span></span>

<span data-ttu-id="e8b84-181">Ao depurar suas migrações ou implantá-las em um banco de dados de produção, é útil gerar um script SQL.</span><span class="sxs-lookup"><span data-stu-id="e8b84-181">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="e8b84-182">O script então pode ser verificado em mais detalhes quanto à precisão e ajustado para atender às necessidades de um banco de dados de produção.</span><span class="sxs-lookup"><span data-stu-id="e8b84-182">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="e8b84-183">O script também pode ser usado em conjunto com uma tecnologia de implantação.</span><span class="sxs-lookup"><span data-stu-id="e8b84-183">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="e8b84-184">O comando básico é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="e8b84-184">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e8b84-185">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8b84-185">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="e8b84-186">Uso básico</span><span class="sxs-lookup"><span data-stu-id="e8b84-186">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="e8b84-187">Com From (To implícito)</span><span class="sxs-lookup"><span data-stu-id="e8b84-187">With From (to implied)</span></span>
<span data-ttu-id="e8b84-188">Isso gerará um script SQL dessa migração para a migração mais recente.</span><span class="sxs-lookup"><span data-stu-id="e8b84-188">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="e8b84-189">Com From e To</span><span class="sxs-lookup"><span data-stu-id="e8b84-189">With From and To</span></span>
<span data-ttu-id="e8b84-190">Isso gerará um script SQL da migração `from` para a migração `to` especificada.</span><span class="sxs-lookup"><span data-stu-id="e8b84-190">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="e8b84-191">É possível usar um `from` mais recente que o `to` para gerar um script de reversão.</span><span class="sxs-lookup"><span data-stu-id="e8b84-191">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="e8b84-192">*Anote os possíveis cenários de perda de dados.*</span><span class="sxs-lookup"><span data-stu-id="e8b84-192">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e8b84-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b84-193">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="e8b84-194">Uso básico</span><span class="sxs-lookup"><span data-stu-id="e8b84-194">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="e8b84-195">Com From (To implícito)</span><span class="sxs-lookup"><span data-stu-id="e8b84-195">With From (to implied)</span></span>
<span data-ttu-id="e8b84-196">Isso gerará um script SQL dessa migração para a migração mais recente.</span><span class="sxs-lookup"><span data-stu-id="e8b84-196">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="e8b84-197">Com From e To</span><span class="sxs-lookup"><span data-stu-id="e8b84-197">With From and To</span></span>
<span data-ttu-id="e8b84-198">Isso gerará um script SQL da migração `from` para a migração `to` especificada.</span><span class="sxs-lookup"><span data-stu-id="e8b84-198">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="e8b84-199">É possível usar um `from` mais recente que o `to` para gerar um script de reversão.</span><span class="sxs-lookup"><span data-stu-id="e8b84-199">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="e8b84-200">*Anote os possíveis cenários de perda de dados.*</span><span class="sxs-lookup"><span data-stu-id="e8b84-200">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="e8b84-201">Há várias opções para este comando.</span><span class="sxs-lookup"><span data-stu-id="e8b84-201">There are several options to this command.</span></span>

<span data-ttu-id="e8b84-202">A migração **de** deve ser a última migração aplicada ao banco de dados antes de executar o script.</span><span class="sxs-lookup"><span data-stu-id="e8b84-202">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="e8b84-203">Se nenhuma migração tiver sido aplicada, especifique `0` (esse é o padrão).</span><span class="sxs-lookup"><span data-stu-id="e8b84-203">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="e8b84-204">A migração **para** é a última migração que será aplicada ao banco de dados após a execução do script.</span><span class="sxs-lookup"><span data-stu-id="e8b84-204">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="e8b84-205">O padrão é a última migração em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="e8b84-205">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="e8b84-206">Um script **idempotente** pode ser gerado como opção.</span><span class="sxs-lookup"><span data-stu-id="e8b84-206">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="e8b84-207">Esse script aplicará migrações somente se elas ainda não tiverem sido aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-207">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="e8b84-208">Isso é útil se você não souber exatamente qual foi a última migração aplicada ao banco de dados ou estiver implantando em vários bancos de dados que podem estar, cada um, em uma migração diferente.</span><span class="sxs-lookup"><span data-stu-id="e8b84-208">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="e8b84-209">Aplicar migrações em runtime</span><span class="sxs-lookup"><span data-stu-id="e8b84-209">Apply migrations at runtime</span></span>

<span data-ttu-id="e8b84-210">Alguns aplicativos talvez queiram aplicar migrações no runtime durante a inicialização ou a primeira execução.</span><span class="sxs-lookup"><span data-stu-id="e8b84-210">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="e8b84-211">Faça isso usando o método `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e8b84-211">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="e8b84-212">Este método é criado sobre o serviço `IMigrator`, que pode ser usado em cenários mais avançados.</span><span class="sxs-lookup"><span data-stu-id="e8b84-212">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="e8b84-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` para acessá-lo.</span><span class="sxs-lookup"><span data-stu-id="e8b84-213">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="e8b84-214">Esta abordagem não é para todo mundo.</span><span class="sxs-lookup"><span data-stu-id="e8b84-214">This approach isn't for everyone.</span></span> <span data-ttu-id="e8b84-215">Embora seja ótimo para aplicativos com um banco de dados local, a maioria dos aplicativos exigirá uma estratégia de implantação mais robusta, como gerar scripts SQL.</span><span class="sxs-lookup"><span data-stu-id="e8b84-215">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="e8b84-216">Não chame `EnsureCreated()` antes de `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e8b84-216">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="e8b84-217">O `EnsureCreated()` ignora as Migrações para criar o esquema e causa falha no `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e8b84-217">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8b84-218">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="e8b84-218">Next steps</span></span>

<span data-ttu-id="e8b84-219">Para obter mais informações, consulte <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="e8b84-219">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
