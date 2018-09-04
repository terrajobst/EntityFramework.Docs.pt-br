---
title: Introdução ao UWP – Novo banco de dados – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996904"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="19b29-102">Introdução ao EF Core na UWP (Plataforma Universal do Windows) com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="19b29-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="19b29-103">Neste tutorial, você criará um aplicativo UWP (Plataforma Universal do Windows) que executa acesso a dados básicos em um banco de dados local do SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="19b29-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="19b29-104">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="19b29-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19b29-105">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="19b29-105">Prerequisites</span></span>

* <span data-ttu-id="19b29-106">[Windows 10 Fall Creators Update (10.0; build 16299) ou posterior](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="19b29-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="19b29-107">[Visual Studio 2017 versão 15.7 ou posterior](https://www.visualstudio.com/downloads/) com a carga de trabalho de **Desenvolvimento da Plataforma Universal do Windows**.</span><span class="sxs-lookup"><span data-stu-id="19b29-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="19b29-108">[SDK do .NET Core 2.1 ou posteriores](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="19b29-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="19b29-109">Criar um projeto de modelo</span><span class="sxs-lookup"><span data-stu-id="19b29-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19b29-110">Devido às limitações nas formas como as ferramentas do .NET Core interagem com os projetos da UWP, o modelo precisa ser colocado em um projeto não UWP para poder executar comandos de migrações no PMC **(Console do Gerenciador de Pacotes)**</span><span class="sxs-lookup"><span data-stu-id="19b29-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="19b29-111">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="19b29-111">Open Visual Studio</span></span>

* <span data-ttu-id="19b29-112">**Arquivo > Novo > Projeto**</span><span class="sxs-lookup"><span data-stu-id="19b29-112">**File > New > Project**</span></span>

* <span data-ttu-id="19b29-113">No menu à esquerda, selecione **Instalado > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="19b29-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="19b29-114">Selecione o modelo **Biblioteca de Classes (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="19b29-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="19b29-115">Nomeie o projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="19b29-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="19b29-116">Nomeie a solução *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="19b29-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="19b29-117">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="19b29-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="19b29-118">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="19b29-118">Install Entity Framework Core</span></span>

<span data-ttu-id="19b29-119">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="19b29-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="19b29-120">Este tutorial usa o SQLite.</span><span class="sxs-lookup"><span data-stu-id="19b29-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="19b29-121">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="19b29-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="19b29-122">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="19b29-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="19b29-123">Execute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="19b29-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="19b29-124">Mais adiante neste tutorial, você usará algumas ferramentas do Entity Framework Core para manter o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="19b29-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="19b29-125">Portanto, instale também o pacote de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="19b29-125">So install the tools package as well.</span></span>

* <span data-ttu-id="19b29-126">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="19b29-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="19b29-127">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="19b29-127">Create the model</span></span>

<span data-ttu-id="19b29-128">Agora é hora de definir um contexto e classes de entidade que compõem o modelo.</span><span class="sxs-lookup"><span data-stu-id="19b29-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="19b29-129">Exclua *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="19b29-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="19b29-130">Crie *Models.cs* com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="19b29-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="19b29-131">Criar um novo projeto da UWP</span><span class="sxs-lookup"><span data-stu-id="19b29-131">Create a new UWP project</span></span>

* <span data-ttu-id="19b29-132">No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="19b29-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="19b29-133">No menu à esquerda, selecione **Instalado > Visual C# > Universal do Windows**.</span><span class="sxs-lookup"><span data-stu-id="19b29-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="19b29-134">Selecione o modelo de projeto **Aplicativo em Branco (Universal do Windows)**.</span><span class="sxs-lookup"><span data-stu-id="19b29-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="19b29-135">Nomeie o projeto *Blogging.UWP* e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="19b29-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="19b29-136">Defina as versões de destino e mínima pelo menos para **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="19b29-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="19b29-137">Criar a migração inicial</span><span class="sxs-lookup"><span data-stu-id="19b29-137">Create the initial migration</span></span>

<span data-ttu-id="19b29-138">Agora que você tem um modelo, configure o aplicativo para criar um banco de dados na primeira vez em que ele for executado.</span><span class="sxs-lookup"><span data-stu-id="19b29-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="19b29-139">Nesta seção, você criará a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="19b29-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="19b29-140">Na seção a seguir, você deve adicionar o código que se aplica a essa migração quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="19b29-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="19b29-141">Ferramentas de migrações exigem um projeto de inicialização não UWP, portanto, crie-o primeiro.</span><span class="sxs-lookup"><span data-stu-id="19b29-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="19b29-142">No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução e escolha **Adicionar > Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="19b29-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="19b29-143">No menu à esquerda, selecione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="19b29-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="19b29-144">Selecione o modelo de projeto **Aplicativo de console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="19b29-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="19b29-145">Nomeie o projeto *Blogging.Migrations.Startup* e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="19b29-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="19b29-146">Adicione uma referência de projeto do projeto *Blogging.Migrations.Startup* ao projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="19b29-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="19b29-147">Agora você pode criar sua migração inicial.</span><span class="sxs-lookup"><span data-stu-id="19b29-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="19b29-148">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="19b29-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="19b29-149">Selecione o projeto *Blogging.Model* como o **Projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="19b29-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="19b29-150">No **Gerenciador de Soluções**, defina o projeto *Blogging.Migrations.Startup* como o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="19b29-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="19b29-151">Execute `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="19b29-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="19b29-152">Este comando realiza scaffolding de uma migração que cria o conjunto inicial de tabelas para seu modelo.</span><span class="sxs-lookup"><span data-stu-id="19b29-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="19b29-153">Criar o banco de dados na inicialização do aplicativo</span><span class="sxs-lookup"><span data-stu-id="19b29-153">Create the database on app startup</span></span>

<span data-ttu-id="19b29-154">Como você quer que o banco de dados seja criado no dispositivo no qual o aplicativo é executado, adicione código para aplicar quaisquer migrações pendentes ao banco de dados local na inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="19b29-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="19b29-155">Na primeira execução do aplicativo, isso será responsável pela criação do banco de dados local.</span><span class="sxs-lookup"><span data-stu-id="19b29-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="19b29-156">Adicione uma referência de projeto do projeto *Blogging.UWP* ao projeto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="19b29-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="19b29-157">Abra *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="19b29-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="19b29-158">Adicione o código realçado para aplicar quaisquer migrações pendentes.</span><span class="sxs-lookup"><span data-stu-id="19b29-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="19b29-159">Se você alterar o modelo, use o comando `Add-Migration` para realizar o scaffolding de uma nova migração e aplicar as alterações correspondentes no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="19b29-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="19b29-160">Quaisquer migrações pendentes serão aplicadas ao banco de dados local em cada dispositivo quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="19b29-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="19b29-161">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="19b29-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="19b29-162">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="19b29-162">Use the model</span></span>

<span data-ttu-id="19b29-163">Agora é possível usar o modelo para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="19b29-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="19b29-164">Abra *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="19b29-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="19b29-165">Adicione o manipulador de carregamento de página e o conteúdo da interface do usuário realçado abaixo</span><span class="sxs-lookup"><span data-stu-id="19b29-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="19b29-166">Agora, adicione o código para conectar a interface do usuário com o banco de dados</span><span class="sxs-lookup"><span data-stu-id="19b29-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="19b29-167">Abra *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="19b29-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="19b29-168">Adicione o código realçado da seguinte lista:</span><span class="sxs-lookup"><span data-stu-id="19b29-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="19b29-169">Agora, você pode executar o aplicativo para vê-lo em ação.</span><span class="sxs-lookup"><span data-stu-id="19b29-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="19b29-170">No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto *Blogging.UWP* e selecione **Implantar**.</span><span class="sxs-lookup"><span data-stu-id="19b29-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="19b29-171">Defina *Blogging.UWP* como projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="19b29-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="19b29-172">**Depurar > Iniciar sem depuração**</span><span class="sxs-lookup"><span data-stu-id="19b29-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="19b29-173">O aplicativo é compilado e executado.</span><span class="sxs-lookup"><span data-stu-id="19b29-173">The app builds and runs.</span></span>

* <span data-ttu-id="19b29-174">Insira uma URL e clique no botão **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="19b29-174">Enter a URL and click the **Add** button</span></span>

  ![imagem](_static/create.png)

  ![imagem](_static/list.png)

  <span data-ttu-id="19b29-177">Pronto!</span><span class="sxs-lookup"><span data-stu-id="19b29-177">Tada!</span></span> <span data-ttu-id="19b29-178">Agora você tem um aplicativo UWP simples executando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="19b29-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19b29-179">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="19b29-179">Next steps</span></span>

<span data-ttu-id="19b29-180">Para obter informações de compatibilidade e desempenho que você deve saber ao usar o EF Core com UWP, veja [Implementações .NET compatíveis com o EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="19b29-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="19b29-181">Confira outros artigos nesta documentação para saber mais sobre os recursos do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="19b29-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
