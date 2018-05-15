---
title: Introdução ao ASP.NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="83e4e-102">Introdução ao EF Core no ASP.NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="83e4e-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="83e4e-103">Neste passo a passo, você compilará um aplicativo MVC do ASP.NET Core que executa acesso básico aos dados usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="83e4e-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="83e4e-104">Você usará as migrações para criar o banco de dados de seu modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="83e4e-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="83e4e-105">Veja [Recursos adicionais](#additional-resources) para conferir mais tutoriais do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="83e4e-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="83e4e-106">Este tutorial exige:</span><span class="sxs-lookup"><span data-stu-id="83e4e-106">This tutorial requires:</span></span>
* <span data-ttu-id="83e4e-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:</span><span class="sxs-lookup"><span data-stu-id="83e4e-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="83e4e-108">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="83e4e-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="83e4e-109">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="83e4e-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="83e4e-110">[SDK do .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="83e4e-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="83e4e-111">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="83e4e-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="83e4e-112">Criar um novo projeto no Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="83e4e-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="83e4e-113">**Arquivo > Novo > Projeto**</span><span class="sxs-lookup"><span data-stu-id="83e4e-113">**File > New > Project**</span></span>
* <span data-ttu-id="83e4e-114">No menu à esquerda, selecione **Instalado > Modelos > Visual C# > .Net Core**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="83e4e-115">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="83e4e-116">Insira **EFGetStarted.AspNetCore.NewDb** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="83e4e-117">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="83e4e-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="83e4e-118">Certifique-se de que as opções **.NET Core** e **ASP.NET Core 2.0** estejam selecionadas nas listas suspensas</span><span class="sxs-lookup"><span data-stu-id="83e4e-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="83e4e-119">Selecione o modelo de projeto **Aplicativo Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="83e4e-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="83e4e-120">Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="83e4e-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="83e4e-121">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="83e4e-121">Click **OK**</span></span>

<span data-ttu-id="83e4e-122">Aviso: se você usar **Contas de Usuário Individuais** em vez de **Nenhum** para **Autenticação**, um modelo do Entity Framework Core será adicionado ao seu projeto no `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="83e4e-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="83e4e-123">Com as técnicas que você aprenderá neste passo a passo, você pode optar por adicionar um segundo modelo ou estender esse modelo existente para conter as classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="83e4e-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="83e4e-124">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="83e4e-124">Install Entity Framework Core</span></span>

<span data-ttu-id="83e4e-125">Instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="83e4e-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="83e4e-126">Este passo a passo usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="83e4e-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="83e4e-127">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="83e4e-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="83e4e-128">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="83e4e-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="83e4e-129">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="83e4e-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="83e4e-130">Usaremos Entity Framework Core Tools para criar um banco de dados de se modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="83e4e-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="83e4e-131">Portanto, também instalaremos o pacote de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="83e4e-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="83e4e-132">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="83e4e-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="83e4e-133">Usaremos algumas ferramentas de scaffolding do ASP.NET Core para criar controladores e exibições posteriormente.</span><span class="sxs-lookup"><span data-stu-id="83e4e-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="83e4e-134">Portanto, também instalaremos o pacote de design:</span><span class="sxs-lookup"><span data-stu-id="83e4e-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="83e4e-135">Execute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="83e4e-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="83e4e-136">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="83e4e-136">Create the model</span></span>

<span data-ttu-id="83e4e-137">Defina um contexto e classes de entidade que compõem o modelo:</span><span class="sxs-lookup"><span data-stu-id="83e4e-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="83e4e-138">Clique com o botão direito do mouse na pasta **Modelos** e selecione **Adicionar > Classe**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="83e4e-139">Insira **Model.cs** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="83e4e-140">Substitua o conteúdo do arquivo pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="83e4e-140">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="83e4e-141">Observação: em um aplicativo real, você normalmente colocaria cada classe de seu modelo em um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="83e4e-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="83e4e-142">Para simplificar, estamos colocando todas as classes em um arquivo para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="83e4e-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="83e4e-143">Registre seu contexto com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="83e4e-143">Register your context with dependency injection</span></span>

<span data-ttu-id="83e4e-144">Serviços (como `BloggingContext`) são registrados com [injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83e4e-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="83e4e-145">Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="83e4e-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="83e4e-146">Para que os controladores MVC usem o `BloggingContext`, vamos registrá-lo como um serviço.</span><span class="sxs-lookup"><span data-stu-id="83e4e-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="83e4e-147">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="83e4e-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="83e4e-148">Adicione as seguintes instruções `using`:</span><span class="sxs-lookup"><span data-stu-id="83e4e-148">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="83e4e-149">Adicione o método `AddDbContext` para registrá-lo como um serviço:</span><span class="sxs-lookup"><span data-stu-id="83e4e-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="83e4e-150">Adicione o seguinte código ao método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="83e4e-150">Add the following code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="83e4e-151">Observação: um aplicativo real normalmente colocaria a cadeia de conexão em um arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="83e4e-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="83e4e-152">Para simplificar, definimos isso no código.</span><span class="sxs-lookup"><span data-stu-id="83e4e-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="83e4e-153">Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="83e4e-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="83e4e-154">Criar seu banco de dados</span><span class="sxs-lookup"><span data-stu-id="83e4e-154">Create your database</span></span>

<span data-ttu-id="83e4e-155">Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="83e4e-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="83e4e-156">Abra o PMC:</span><span class="sxs-lookup"><span data-stu-id="83e4e-156">Open the PMC:</span></span>

  <span data-ttu-id="83e4e-157">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="83e4e-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="83e4e-158">Execute `Add-Migration InitialCreate` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="83e4e-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="83e4e-159">Se você receber um erro indicando `The term 'add-migration' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83e4e-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="83e4e-160">Execute `Update-Database` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="83e4e-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="83e4e-161">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="83e4e-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="83e4e-162">Criar um controlador</span><span class="sxs-lookup"><span data-stu-id="83e4e-162">Create a controller</span></span>

<span data-ttu-id="83e4e-163">Habilite o scaffolding no projeto:</span><span class="sxs-lookup"><span data-stu-id="83e4e-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="83e4e-164">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="83e4e-165">Selecione **Dependências Mínimas** e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="83e4e-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="83e4e-166">Você pode ignorar ou excluir o arquivo *ScaffoldingReadMe.txt*.</span><span class="sxs-lookup"><span data-stu-id="83e4e-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="83e4e-167">Agora que o scaffolding está habilitado, é possível realizar o scaffolding de um controlador para a entidade `Blog`.</span><span class="sxs-lookup"><span data-stu-id="83e4e-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="83e4e-168">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="83e4e-169">Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Ok**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="83e4e-170">Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="83e4e-171">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="83e4e-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="83e4e-172">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="83e4e-172">Run the application</span></span>

<span data-ttu-id="83e4e-173">Pressione F5 para executar e testar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="83e4e-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="83e4e-174">Navegue até `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="83e4e-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="83e4e-175">Use o link de criação para criar algumas entradas do blog.</span><span class="sxs-lookup"><span data-stu-id="83e4e-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="83e4e-176">Teste os links de detalhes e exclusão.</span><span class="sxs-lookup"><span data-stu-id="83e4e-176">Test the details and delete links.</span></span>

![imagem](_static/create.png)

![imagem](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="83e4e-179">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="83e4e-179">Additional Resources</span></span>

* <span data-ttu-id="83e4e-180">[EF – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="83e4e-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="83e4e-181">Introdução ao ASP.NET Core MVC no Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="83e4e-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="83e4e-182">Introdução ao ASP.NET Core MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83e4e-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="83e4e-183">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83e4e-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
