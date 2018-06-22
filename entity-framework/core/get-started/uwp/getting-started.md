---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049829"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="2b76e-102">Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="2b76e-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="2b76e-103">Este tutorial usa o EF Core 2.0.1 (lançado junto com o ASP.NET Core e o .NET Core SDK 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="2b76e-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="2b76e-104">O EF Core 2.0.0 não tem algumas correções de bug essenciais necessárias para uma boa experiência da UWP.</span><span class="sxs-lookup"><span data-stu-id="2b76e-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="2b76e-105">Neste passo a passo, você compilará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2b76e-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b76e-106">**Considere evitar tipos anônimos em consultas do LINQ na UWP**.</span><span class="sxs-lookup"><span data-stu-id="2b76e-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="2b76e-107">A implantação de um aplicativo da UWP na loja de aplicativos exige a compilação do aplicativo com o .NET Native.</span><span class="sxs-lookup"><span data-stu-id="2b76e-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="2b76e-108">Consultas com tipos anônimos tem um desempenho pior no .NET Native.</span><span class="sxs-lookup"><span data-stu-id="2b76e-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="2b76e-109">Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="2b76e-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b76e-110">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2b76e-110">Prerequisites</span></span>

<span data-ttu-id="2b76e-111">Os itens a seguir são necessários para concluir este passo a passo:</span><span class="sxs-lookup"><span data-stu-id="2b76e-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="2b76e-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="2b76e-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="2b76e-113">[SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.</span><span class="sxs-lookup"><span data-stu-id="2b76e-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="2b76e-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.4 ou posterior com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.</span><span class="sxs-lookup"><span data-stu-id="2b76e-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="2b76e-115">Criar um novo projeto de modelo</span><span class="sxs-lookup"><span data-stu-id="2b76e-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="2b76e-116">Devido às limitações nas formas como as ferramentas do .NET Core interagem com os projetos da UWP, o modelo precisa ser colocado em um projeto não UWP para poder executar comandos de migrações no Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="2b76e-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="2b76e-117">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b76e-117">Open Visual Studio</span></span>

* <span data-ttu-id="2b76e-118">Arquivo > Novo > Projeto...</span><span class="sxs-lookup"><span data-stu-id="2b76e-118">File > New > Project...</span></span>

* <span data-ttu-id="2b76e-119">No menu à esquerda, selecione Modelos > Visual C#</span><span class="sxs-lookup"><span data-stu-id="2b76e-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="2b76e-120">Selecione o modelo de projeto **Biblioteca de Classes (.NET Standard)**</span><span class="sxs-lookup"><span data-stu-id="2b76e-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="2b76e-121">Dê um nome ao projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="2b76e-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="2b76e-122">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2b76e-122">Install Entity Framework</span></span>

<span data-ttu-id="2b76e-123">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="2b76e-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="2b76e-124">Este passo a passo usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="2b76e-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="2b76e-125">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2b76e-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="2b76e-126">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="2b76e-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="2b76e-127">Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="2b76e-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="2b76e-128">Mais adiante neste passo a passo, usaremos também algumas Entity Framework Tools para manter o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2b76e-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="2b76e-129">Portanto, também instalaremos o pacote de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="2b76e-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="2b76e-130">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="2b76e-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="2b76e-131">Edite o arquivo .csproj e substitua `<TargetFramework>netstandard2.0</TargetFramework>` por `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="2b76e-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="2b76e-132">Criar seu modelo</span><span class="sxs-lookup"><span data-stu-id="2b76e-132">Create your model</span></span>

<span data-ttu-id="2b76e-133">Agora é hora de definir um contexto e classes de entidade que compõem seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2b76e-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="2b76e-134">Projeto > Adicionar Classe...</span><span class="sxs-lookup"><span data-stu-id="2b76e-134">Project > Add Class...</span></span>

* <span data-ttu-id="2b76e-135">Insira *Model.cs* como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="2b76e-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="2b76e-136">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="2b76e-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="2b76e-137">Criar um novo projeto da UWP</span><span class="sxs-lookup"><span data-stu-id="2b76e-137">Create a new UWP project</span></span>

* <span data-ttu-id="2b76e-138">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b76e-138">Open Visual Studio</span></span>

* <span data-ttu-id="2b76e-139">Arquivo > Novo > Projeto...</span><span class="sxs-lookup"><span data-stu-id="2b76e-139">File > New > Project...</span></span>

* <span data-ttu-id="2b76e-140">No menu à esquerda, selecione Modelos > Visual C# > Windows Universal</span><span class="sxs-lookup"><span data-stu-id="2b76e-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="2b76e-141">Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**</span><span class="sxs-lookup"><span data-stu-id="2b76e-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="2b76e-142">Dê um nome ao projeto e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="2b76e-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="2b76e-143">Defina as versões de destino e mínima como, pelo menos, `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="2b76e-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="2b76e-144">Criar seu banco de dados</span><span class="sxs-lookup"><span data-stu-id="2b76e-144">Create your database</span></span>

<span data-ttu-id="2b76e-145">Agora que você tem um modelo, use as migrações para criar um banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="2b76e-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="2b76e-146">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="2b76e-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="2b76e-147">Selecione o projeto de modelo como o projeto Padrão e defina-o como o projeto de inicialização</span><span class="sxs-lookup"><span data-stu-id="2b76e-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="2b76e-148">Execute `Add-Migration MyFirstMigration` para realizar scaffolding de uma migração a fim de criar o conjunto inicial de tabelas para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="2b76e-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="2b76e-149">Como queremos que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicionaremos alguns códigos para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2b76e-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="2b76e-150">Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local para nós.</span><span class="sxs-lookup"><span data-stu-id="2b76e-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="2b76e-151">Clique com o botão direito do mouse no **App.xaml** no **Gerenciador de Soluções** e selecione **Exibir Código**</span><span class="sxs-lookup"><span data-stu-id="2b76e-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="2b76e-152">Adicione o using realçado ao início do arquivo</span><span class="sxs-lookup"><span data-stu-id="2b76e-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="2b76e-153">Adicione o código realçado para aplicar quaisquer migrações pendentes</span><span class="sxs-lookup"><span data-stu-id="2b76e-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="2b76e-154">Se você fizer alterações futuras em seu modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2b76e-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="2b76e-155">Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="2b76e-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="2b76e-156">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2b76e-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="2b76e-157">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="2b76e-157">Use your model</span></span>

<span data-ttu-id="2b76e-158">Agora você pode usar o modelo para executar o acesso aos dados.</span><span class="sxs-lookup"><span data-stu-id="2b76e-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="2b76e-159">Abra *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="2b76e-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="2b76e-160">Adicione o manipulador de carregamento de página e o conteúdo da interface do usuário realçado abaixo</span><span class="sxs-lookup"><span data-stu-id="2b76e-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="2b76e-161">Agora, adicionaremos o código para conectar a interface do usuário com o banco de dados</span><span class="sxs-lookup"><span data-stu-id="2b76e-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="2b76e-162">Clique com o botão direito do mouse no **MainPage.xaml** no **Gerenciador de Soluções** e selecione **Exibir Código**</span><span class="sxs-lookup"><span data-stu-id="2b76e-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="2b76e-163">Adicione o código realçado da seguinte lista</span><span class="sxs-lookup"><span data-stu-id="2b76e-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="2b76e-164">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="2b76e-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="2b76e-165">Depurar > Iniciar Sem Depuração</span><span class="sxs-lookup"><span data-stu-id="2b76e-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="2b76e-166">O aplicativo será compilado e iniciado</span><span class="sxs-lookup"><span data-stu-id="2b76e-166">The application will build and launch</span></span>

* <span data-ttu-id="2b76e-167">Insira uma URL e clique no botão **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="2b76e-167">Enter a URL and click the **Add** button</span></span>

![imagem](_static/create.png)

![imagem](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="2b76e-170">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2b76e-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="2b76e-171">O desempenho do `SaveChanges()` pode ser melhorado com a implementação de [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) em seus tipos de entidade e usando `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="2b76e-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="2b76e-172">Pronto!</span><span class="sxs-lookup"><span data-stu-id="2b76e-172">Tada!</span></span> <span data-ttu-id="2b76e-173">Agora você tem um aplicativo UWP simples executando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2b76e-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="2b76e-174">Verifique outros artigos nesta documentação para saber mais sobre os recursos do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2b76e-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
