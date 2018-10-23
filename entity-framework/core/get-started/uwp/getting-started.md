---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315614"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="561a8-102">Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="561a8-103">Neste tutorial, você criará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="561a8-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="561a8-104">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="561a8-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="561a8-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="561a8-105">Prerequisites</span></span>

* <span data-ttu-id="561a8-106">[Windows 10 Fall Creators Update (10.0; build 16299) ou posterior](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="561a8-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="561a8-107">[Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.</span><span class="sxs-lookup"><span data-stu-id="561a8-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="561a8-108">[SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="561a8-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="561a8-109">Este tutorial usa os comandos de [migrações](xref:core/managing-schemas/migrations/index) do Entity Framework Core para criar e atualizar o esquema do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="561a8-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="561a8-110">Esses comandos não funcionam diretamente com projetos UWP.</span><span class="sxs-lookup"><span data-stu-id="561a8-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="561a8-111">Por esse motivo, o modelo de dados do aplicativo é colocado em um projeto de biblioteca compartilhada, e um aplicativo de console do .NET Core é usado para executar os comandos.</span><span class="sxs-lookup"><span data-stu-id="561a8-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="561a8-112">Criar um projeto de biblioteca para armazenar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="561a8-113">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="561a8-113">Open Visual Studio</span></span>

* <span data-ttu-id="561a8-114">**Arquivo > Novo > Projeto**</span><span class="sxs-lookup"><span data-stu-id="561a8-114">**File > New > Project**</span></span>

* <span data-ttu-id="561a8-115">No menu à esquerda, selecione **Instalado > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="561a8-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="561a8-116">Selecione o modelo **Biblioteca de Classes (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="561a8-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="561a8-117">Nomeie o projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="561a8-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="561a8-118">Nomeie a solução *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="561a8-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="561a8-119">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="561a8-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="561a8-120">Instalar o tempo de execução do Entity Framework Core no projeto de modelo de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="561a8-121">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="561a8-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="561a8-122">Este tutorial usa o SQLite.</span><span class="sxs-lookup"><span data-stu-id="561a8-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="561a8-123">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="561a8-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="561a8-124">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="561a8-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="561a8-125">Certifique-se de que o projeto de biblioteca *Blogging.Model* foi selecionado como o **Projeto padrão** no console do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="561a8-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="561a8-126">Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="561a8-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="561a8-127">Criar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-127">Create the data model</span></span>

<span data-ttu-id="561a8-128">Agora é hora de definir o *DbContext* e as classes de entidade que compõem o modelo.</span><span class="sxs-lookup"><span data-stu-id="561a8-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="561a8-129">Exclua *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="561a8-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="561a8-130">Crie *Models.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="561a8-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="561a8-131">Criar um novo projeto de console para executar os comandos de migrações</span><span class="sxs-lookup"><span data-stu-id="561a8-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="561a8-132">No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="561a8-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="561a8-133">No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="561a8-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="561a8-134">Selecione o modelo de projeto **Aplicativo de console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="561a8-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="561a8-135">Nomeie o projeto *Blogging.Migrations.Startup* e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="561a8-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="561a8-136">Adicione uma referência de projeto do projeto *Blogging.Migrations.Startup* ao projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="561a8-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="561a8-137">Instalar as ferramentas do Entity Framework Core no projeto de inicialização de migrações</span><span class="sxs-lookup"><span data-stu-id="561a8-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="561a8-138">Para habilitar os comandos de migração do EF Core no Console do Gerenciador de pacotes, instale o pacote de ferramentas do EF Core no aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="561a8-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="561a8-139">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="561a8-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="561a8-140">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="561a8-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="561a8-141">Criar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="561a8-141">Create the initial migration</span></span>

 <span data-ttu-id="561a8-142">Crie a migração inicial, especificando o aplicativo de console como o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="561a8-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="561a8-143">Execute `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="561a8-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="561a8-144">Este comando realiza scaffolding de uma migração que cria o conjunto inicial de tabelas de banco de dados para seu modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="561a8-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="561a8-145">Criar o projeto UWP</span><span class="sxs-lookup"><span data-stu-id="561a8-145">Create the UWP project</span></span>

* <span data-ttu-id="561a8-146">No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="561a8-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="561a8-147">No menu à esquerda, selecione **Instalado > Visual C# > Universal do Windows**.</span><span class="sxs-lookup"><span data-stu-id="561a8-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="561a8-148">Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**.</span><span class="sxs-lookup"><span data-stu-id="561a8-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="561a8-149">Nomeie o projeto *Blogging.UWP* e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="561a8-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="561a8-150">Defina as versões de destino e mínima pelo menos para **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="561a8-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="561a8-151">As versões anteriores do Windows 10 não são compatíveis com o .NET Standard 2.0, exigido pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="561a8-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="561a8-152">Adicione código para criar o banco de dados na inicialização do aplicativo</span><span class="sxs-lookup"><span data-stu-id="561a8-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="561a8-153">Como você quer que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicione código para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="561a8-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="561a8-154">Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local.</span><span class="sxs-lookup"><span data-stu-id="561a8-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="561a8-155">Adicione uma referência de projeto do projeto *Blogging.UWP* ao projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="561a8-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="561a8-156">Abra *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="561a8-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="561a8-157">Adicione o código realçado para aplicar quaisquer migrações pendentes.</span><span class="sxs-lookup"><span data-stu-id="561a8-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="561a8-158">Se você alterar o modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="561a8-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="561a8-159">Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="561a8-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="561a8-160">O EF Core usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="561a8-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="561a8-161">Usar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-161">Use the data model</span></span>

<span data-ttu-id="561a8-162">Agora é possível usar o EF Core para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="561a8-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="561a8-163">Abra *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="561a8-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="561a8-164">Adicione o manipulador de carregamento de página e o conteúdo da interface do usuário realçado abaixo</span><span class="sxs-lookup"><span data-stu-id="561a8-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="561a8-165">Agora, adicione o código para conectar a interface do usuário com o banco de dados</span><span class="sxs-lookup"><span data-stu-id="561a8-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="561a8-166">Abra *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="561a8-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="561a8-167">Adicione o código realçado da seguinte lista:</span><span class="sxs-lookup"><span data-stu-id="561a8-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="561a8-168">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="561a8-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="561a8-169">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto *Blogging.UWP* e selecione **Implantar**.</span><span class="sxs-lookup"><span data-stu-id="561a8-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="561a8-170">Defina *Blogging.UWP* como projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="561a8-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="561a8-171">**Depurar > Iniciar sem depuração**</span><span class="sxs-lookup"><span data-stu-id="561a8-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="561a8-172">O aplicativo é compilado e executado.</span><span class="sxs-lookup"><span data-stu-id="561a8-172">The app builds and runs.</span></span>

* <span data-ttu-id="561a8-173">Insira uma URL e clique no botão **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="561a8-173">Enter a URL and click the **Add** button</span></span>

  ![imagem](_static/create.png)

  ![imagem](_static/list.png)

  <span data-ttu-id="561a8-176">Pronto!</span><span class="sxs-lookup"><span data-stu-id="561a8-176">Tada!</span></span> <span data-ttu-id="561a8-177">Agora você tem um aplicativo UWP simples executando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="561a8-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="561a8-178">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="561a8-178">Next steps</span></span>

<span data-ttu-id="561a8-179">Para obter informações de compatibilidade e desempenho que você deve saber ao usar o EF Core com UWP, veja [Implementações .NET compatíveis com o EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="561a8-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="561a8-180">Confira outros artigos nesta documentação para saber mais sobre os recursos do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="561a8-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
