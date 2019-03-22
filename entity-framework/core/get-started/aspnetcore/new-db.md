---
title: Introdução ao ASP.NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 25e5a683acf4bbed0b978cc6a80f1b50a0b64ca1
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319173"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3f6dc-102">Introdução ao EF Core no ASP.NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="3f6dc-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3f6dc-103">Neste tutorial, você compila um aplicativo MVC do ASP.NET Core que realiza acesso básico a dados usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3f6dc-104">O tutorial usa as migrações para criar o banco de dados do modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="3f6dc-105">Você pode seguir o tutorial usando o Visual Studio 2017 no Windows ou usando a CLI do .NET Core no Windows, macOS ou Linux.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="3f6dc-106">Exiba o exemplo deste artigo no GitHub:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="3f6dc-107">Visual Studio 2017 com SQL Server</span><span class="sxs-lookup"><span data-stu-id="3f6dc-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="3f6dc-108">[CLI do .NET Core com SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f6dc-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="3f6dc-109">Prerequisites</span></span>

<span data-ttu-id="3f6dc-110">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-112">[Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3f6dc-113">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="3f6dc-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3f6dc-114">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="3f6dc-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3f6dc-115">[SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-116">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-117">[SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="3f6dc-118">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="3f6dc-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-120">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3f6dc-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3f6dc-121">**Arquivo > Novo > Projeto**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-121">**File > New > Project**</span></span>
* <span data-ttu-id="3f6dc-122">No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3f6dc-123">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3f6dc-124">Insira **EFGetStarted.AspNetCore.NewDb** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3f6dc-125">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3f6dc-126">Garanta que **.NET Core** e **ASP.NET Core 2.1** estejam selecionados nas listas suspensas</span><span class="sxs-lookup"><span data-stu-id="3f6dc-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="3f6dc-127">Selecione o modelo de projeto **Aplicativo Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3f6dc-128">Verifique se **Autenticação** está definida como **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3f6dc-129">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-129">Click **OK**</span></span>

<span data-ttu-id="3f6dc-130">Aviso: se você usar **Contas de Usuário Individuais** em vez de **Nenhum** para **Autenticação**, um modelo do Entity Framework Core será adicionado ao seu projeto no `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3f6dc-131">Com as técnicas que você aprende neste tutorial, é possível optar por adicionar um segundo modelo ou estender o modelo existente para conter as classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-132">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-133">Execute o seguinte comando para criar um projeto do MVC:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="3f6dc-134">Altere para o diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-134">Change to the project directory.</span></span> <span data-ttu-id="3f6dc-135">Os próximos comandos que você inserir precisam ser executados no novo projeto.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="3f6dc-136">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-136">Install Entity Framework Core</span></span>

<span data-ttu-id="3f6dc-137">Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3f6dc-138">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3f6dc-140">Para este tutorial, não é necessário instalar um pacote de provedor, porque o tutorial usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="3f6dc-141">O pacote de provedor do SQL Server está incluído no [metapacote Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-142">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="3f6dc-143">Este tutorial usa SQLite porque ele é executado em todas as plataformas que dão suporte a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="3f6dc-144">Execute o seguinte comando para instalar o provedor SQLite:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="3f6dc-145">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="3f6dc-145">Create the model</span></span>

<span data-ttu-id="3f6dc-146">Defina uma classe de contexto e classes de entidade que compõem o modelo.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-148">Clique com o botão direito do mouse na pasta **Modelos** e selecione **Adicionar > Classe**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3f6dc-149">Insira **Model.cs** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3f6dc-150">Substitua o conteúdo do arquivo pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-151">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-152">Na pasta **Modelos**, crie **Model.cs** com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="3f6dc-153">Um aplicativo de produção normalmente colocaria cada classe em um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="3f6dc-154">Para simplificar, este tutorial coloca todas essas classes em um só arquivo.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="3f6dc-155">Registrar o contexto com a injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="3f6dc-155">Register the context with dependency injection</span></span>

<span data-ttu-id="3f6dc-156">Para disponibilizar o `BloggingContext` aos controladores MVC, registre-o como um serviço em `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="3f6dc-157">Os serviços (como `BloggingContext`) são registrados com a [injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante a inicialização do aplicativo para que eles possam ser fornecidos automaticamente aos componentes que os consomem (como os controladores MVC) por meio de parâmetros e propriedades do construtor.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-159">Em **Startup.cs**, adicione as seguintes instruções `using`:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="3f6dc-160">Adicione o seguinte código realçado ao método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-161">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-162">Em **Startup.cs**, adicione as seguintes instruções `using`:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="3f6dc-163">Adicione o seguinte código realçado ao método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="3f6dc-164">Um aplicativo normalmente colocaria a cadeia de conexão em um arquivo de configuração ou variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="3f6dc-165">Para simplificar, este tutorial define isso no código.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="3f6dc-166">Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="3f6dc-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="3f6dc-167">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="3f6dc-167">Create the database</span></span>

<span data-ttu-id="3f6dc-168">As etapas a seguir usam [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-170">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="3f6dc-171">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="3f6dc-172">Se você receber um erro indicando `The term 'add-migration' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="3f6dc-173">O comando `Add-Migration` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="3f6dc-174">O comando `Update-Database` cria o banco de dados e aplica a nova migração a ele.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-175">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-176">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="3f6dc-177">O comando `migrations` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="3f6dc-178">O comando `database update` cria o banco de dados e aplica a nova migração a ele.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="3f6dc-179">Criar um controlador</span><span class="sxs-lookup"><span data-stu-id="3f6dc-179">Create a controller</span></span>

<span data-ttu-id="3f6dc-180">Gere um controlador e os modos de exibição para a entidade `Blog`.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-182">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3f6dc-183">Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="3f6dc-184">Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3f6dc-185">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-186">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="3f6dc-187">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="3f6dc-188">Os comandos `tool install` e `add package` instalam as ferramentas que podem realizar o scaffolding de controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="3f6dc-189">O `restore` comando garante que todos os pacotes do projeto foram baixados e o `aspnet-codegenerator` comando realiza o scaffolding.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="3f6dc-190">O mecanismo de scaffolding cria os seguintes arquivos:</span><span class="sxs-lookup"><span data-stu-id="3f6dc-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="3f6dc-191">Um controlador (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="3f6dc-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="3f6dc-192">Exibições das Razor Pages Criar, Excluir, Detalhes, Editar e Índice (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="3f6dc-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3f6dc-193">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="3f6dc-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3f6dc-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f6dc-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3f6dc-195">**Depurar** > **Iniciar sem Depuração**</span><span class="sxs-lookup"><span data-stu-id="3f6dc-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3f6dc-196">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="3f6dc-197">Navegue até `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3f6dc-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="3f6dc-198">Use o link **Criar Novo** para criar algumas entradas do blog.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Criar página](_static/create.png)

* <span data-ttu-id="3f6dc-200">Teste os links **Detalhes**, **Editar** e **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="3f6dc-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Página de índice](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3f6dc-202">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="3f6dc-202">Additional Resources</span></span>

* [<span data-ttu-id="3f6dc-203">Tutorial: introdução ao EF Core no .NET Core com um novo banco de dados usando o SQLite</span><span class="sxs-lookup"><span data-stu-id="3f6dc-203">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="3f6dc-204">Tutorial: introdução às Razor Pages no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-204">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="3f6dc-205">Tutorial: Razor Pages com o Entity Framework Core no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f6dc-205">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
