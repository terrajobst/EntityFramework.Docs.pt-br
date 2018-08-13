---
title: Introdução ao ASP.NET Core – Novo banco de dados – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614344"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="8e833-102">Introdução ao EF Core no ASP.NET Core com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="8e833-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="8e833-103">Neste tutorial, você compila um aplicativo MVC do ASP.NET Core que realiza acesso básico a dados usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8e833-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="8e833-104">Você usará as migrações para criar o banco de dados de seu modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8e833-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="8e833-105">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="8e833-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e833-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="8e833-106">Prerequisites</span></span>

<span data-ttu-id="8e833-107">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="8e833-107">Install the following software:</span></span>

* <span data-ttu-id="8e833-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) com estas cargas de trabalho:</span><span class="sxs-lookup"><span data-stu-id="8e833-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="8e833-109">**Desenvolvimento na Web e no ASP.NET** (em **Web e Nuvem**)</span><span class="sxs-lookup"><span data-stu-id="8e833-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="8e833-110">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="8e833-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="8e833-111">[SDK do .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="8e833-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="8e833-112">Criar um novo projeto no Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8e833-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="8e833-113">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8e833-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="8e833-114">**Arquivo > Novo > Projeto**</span><span class="sxs-lookup"><span data-stu-id="8e833-114">**File > New > Project**</span></span>
* <span data-ttu-id="8e833-115">No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8e833-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="8e833-116">Selecione **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8e833-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8e833-117">Insira **EFGetStarted.AspNetCore.NewDb** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e833-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="8e833-118">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="8e833-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="8e833-119">Verifique se as opções **.NET Core** e **ASP.NET Core 2.1** estão selecionadas nas listas suspensas</span><span class="sxs-lookup"><span data-stu-id="8e833-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="8e833-120">Selecione o modelo de projeto **Aplicativo Web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="8e833-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="8e833-121">Certifique-se de que **Autenticação** esteja definida como **Sem Autenticação**</span><span class="sxs-lookup"><span data-stu-id="8e833-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="8e833-122">Clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="8e833-122">Click **OK**</span></span>

<span data-ttu-id="8e833-123">Aviso: se você usar **Contas de Usuário Individuais** em vez de **Nenhum** para **Autenticação**, um modelo do Entity Framework Core será adicionado ao seu projeto no `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="8e833-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="8e833-124">Com as técnicas que você aprende neste tutorial, é possível optar por adicionar um segundo modelo ou estender o modelo existente para conter as classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="8e833-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="8e833-125">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8e833-125">Install Entity Framework Core</span></span>

<span data-ttu-id="8e833-126">Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="8e833-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="8e833-127">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8e833-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="8e833-128">Para este tutorial, não é necessário instalar um pacote de provedor, porque o tutorial usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e833-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="8e833-129">O pacote de provedor do SQL Server está incluído no [metapacote Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="8e833-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="8e833-130">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="8e833-130">Create the model</span></span>

<span data-ttu-id="8e833-131">Defina uma classe de contexto e classes de entidade que compõem o modelo:</span><span class="sxs-lookup"><span data-stu-id="8e833-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="8e833-132">Clique com o botão direito do mouse na pasta **Modelos** e selecione **Adicionar > Classe**.</span><span class="sxs-lookup"><span data-stu-id="8e833-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="8e833-133">Insira **Model.cs** como o nome e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e833-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="8e833-134">Substitua o conteúdo do arquivo pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="8e833-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="8e833-135">Em um aplicativo real, você normalmente colocaria cada classe de seu modelo em um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="8e833-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="8e833-136">Para simplificar, este tutorial coloca todas as classes em um só arquivo.</span><span class="sxs-lookup"><span data-stu-id="8e833-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="8e833-137">Registre seu contexto com injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="8e833-137">Register your context with dependency injection</span></span>

<span data-ttu-id="8e833-138">Serviços (como `BloggingContext`) são registrados com [injeção de dependência](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante a inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e833-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="8e833-139">Agora, os componentes que exigem esses serviços (como os controladores de MVC) os recebem por meio de propriedades ou parâmetros do construtor.</span><span class="sxs-lookup"><span data-stu-id="8e833-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="8e833-140">Para disponibilizar `BloggingContext` para os controladores do MVC, registre-o como um serviço.</span><span class="sxs-lookup"><span data-stu-id="8e833-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="8e833-141">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="8e833-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="8e833-142">Adicione as seguintes instruções `using`:</span><span class="sxs-lookup"><span data-stu-id="8e833-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="8e833-143">Chame o método `AddDbContext` para registrar o contexto como um serviço.</span><span class="sxs-lookup"><span data-stu-id="8e833-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="8e833-144">Adicione o seguinte código realçado ao método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8e833-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="8e833-145">Observação: um aplicativo real normalmente colocaria a cadeia de conexão em um arquivo de configuração ou variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="8e833-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="8e833-146">Para simplificar, este tutorial define isso no código.</span><span class="sxs-lookup"><span data-stu-id="8e833-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="8e833-147">Para saber mais, confira [Cadeias de conexão](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="8e833-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8e833-148">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="8e833-148">Create the database</span></span>

<span data-ttu-id="8e833-149">Assim que você tiver um modelo, use as [migrações](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8e833-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="8e833-150">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="8e833-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="8e833-151">Execute `Add-Migration InitialCreate` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="8e833-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="8e833-152">Se você receber um erro indicando `The term 'add-migration' is not recognized as the name of a cmdlet`, feche e reabra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e833-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="8e833-153">Execute `Update-Database` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8e833-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="8e833-154">Este comando cria o banco de dados antes de aplicar as migrações.</span><span class="sxs-lookup"><span data-stu-id="8e833-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="8e833-155">Criar um controlador</span><span class="sxs-lookup"><span data-stu-id="8e833-155">Create a controller</span></span>

<span data-ttu-id="8e833-156">Gere um controlador e os modos de exibição para a entidade `Blog`.</span><span class="sxs-lookup"><span data-stu-id="8e833-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="8e833-157">Clique com o botão direito do mouse na pasta **Controladores** no **Gerenciador de Soluções** e selecione **Adicionar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="8e833-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="8e833-158">Selecione **Controlador MVC com exibições, usando o Entity Framework** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="8e833-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="8e833-159">Defina **Classe de modelo** como **Blog** e **Classe de contexto de dados** como **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="8e833-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="8e833-160">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="8e833-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="8e833-161">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="8e833-161">Run the application</span></span>

<span data-ttu-id="8e833-162">Pressione F5 para executar e testar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8e833-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="8e833-163">Navegue até `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="8e833-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="8e833-164">Use o link de criação para criar algumas entradas do blog.</span><span class="sxs-lookup"><span data-stu-id="8e833-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="8e833-165">Teste os links de detalhes e exclusão.</span><span class="sxs-lookup"><span data-stu-id="8e833-165">Test the details and delete links.</span></span>

![imagem](_static/create.png)

![imagem](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="8e833-168">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8e833-168">Additional Resources</span></span>

* <span data-ttu-id="8e833-169">[EF – Novo banco de dados com SQLite](xref:core/get-started/netcore/new-db-sqlite) – um tutorial EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="8e833-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="8e833-170">Introdução ao ASP.NET Core MVC no Mac ou Linux</span><span class="sxs-lookup"><span data-stu-id="8e833-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="8e833-171">Introdução ao ASP.NET Core MVC com o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e833-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="8e833-172">Introdução ao ASP.NET Core e ao Entity Framework Core usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e833-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
