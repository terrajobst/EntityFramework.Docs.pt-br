---
title: Introdução – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412861"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="2cc42-102">Introdução ao EF Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-102">Getting Started with EF Core</span></span>

<span data-ttu-id="2cc42-103">Neste tutorial, você criará um aplicativo de console do .NET Core que executa acesso a dados em um banco de dados SQLite usando o Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2cc42-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="2cc42-104">Você pode seguir o tutorial usando o Visual Studio no Windows ou usando a CLI do .NET Core no Windows, macOS ou Linux.</span><span class="sxs-lookup"><span data-stu-id="2cc42-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="2cc42-105">[Exiba o exemplo deste artigo no GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="2cc42-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cc42-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="2cc42-106">Prerequisites</span></span>

<span data-ttu-id="2cc42-107">Instale o software a seguir:</span><span class="sxs-lookup"><span data-stu-id="2cc42-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-108">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2cc42-109">[SDK do .Net Core](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="2cc42-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cc42-111">[Visual Studio 2019 versão 16.3 ou posterior](https://www.visualstudio.com/downloads/) com esta carga de trabalho:</span><span class="sxs-lookup"><span data-stu-id="2cc42-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="2cc42-112">**Desenvolvimento de plataforma cruzada do .NET Core** (em **Outros conjuntos de ferramentas**)</span><span class="sxs-lookup"><span data-stu-id="2cc42-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="2cc42-113">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="2cc42-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-114">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cc42-116">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-116">Open Visual Studio</span></span>
* <span data-ttu-id="2cc42-117">Clique em **Criar um novo projeto**</span><span class="sxs-lookup"><span data-stu-id="2cc42-117">Click **Create a new project**</span></span>
* <span data-ttu-id="2cc42-118">Selecione **Aplicativo de Console (.NET Core)** com a marca **C#** e clique em **Avançar**</span><span class="sxs-lookup"><span data-stu-id="2cc42-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="2cc42-119">Insira **EFGetStarted** como o nome e clique em **Criar**</span><span class="sxs-lookup"><span data-stu-id="2cc42-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="2cc42-120">Instalar o Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-120">Install Entity Framework Core</span></span>

<span data-ttu-id="2cc42-121">Para instalar o EF Core, instale o pacote dos provedores do banco de dados do EF Core para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="2cc42-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="2cc42-122">Este tutorial usa SQLite porque ele é executado em todas as plataformas que dão suporte a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cc42-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="2cc42-123">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2cc42-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-124">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cc42-126">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="2cc42-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="2cc42-127">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="2cc42-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="2cc42-128">Dica: você também pode instalar pacotes clicando com o botão direito do mouse no projeto e selecionando **Gerenciar Pacotes NuGet**</span><span class="sxs-lookup"><span data-stu-id="2cc42-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="2cc42-129">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="2cc42-129">Create the model</span></span>

<span data-ttu-id="2cc42-130">Defina uma classe de contexto e classes de entidade que compõem o modelo.</span><span class="sxs-lookup"><span data-stu-id="2cc42-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-131">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2cc42-132">No diretório do projeto, crie **Model.cs** com o seguinte código</span><span class="sxs-lookup"><span data-stu-id="2cc42-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cc42-134">Clique com o botão direito do mouse no projeto e selecione **Adicionar > Classe**</span><span class="sxs-lookup"><span data-stu-id="2cc42-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="2cc42-135">Insira **Model.cs** como o nome e clique em **Adicionar**</span><span class="sxs-lookup"><span data-stu-id="2cc42-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="2cc42-136">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="2cc42-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="2cc42-137">O EF Core também pode fazer a [engenharia reversa](../managing-schemas/scaffolding.md) de um modelo de um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="2cc42-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="2cc42-138">Dica: em um aplicativo real, você coloca cada classe em um arquivo separado e coloca a [cadeia de conexão](../miscellaneous/connection-strings.md) em um arquivo de configuração ou na variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="2cc42-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="2cc42-139">Para simplificar o tutorial, tudo está contido em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="2cc42-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="2cc42-140">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="2cc42-140">Create the database</span></span>

<span data-ttu-id="2cc42-141">As etapas a seguir usam [migrações](xref:core/managing-schemas/migrations/index) para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2cc42-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-142">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="2cc42-143">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="2cc42-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="2cc42-144">Isso instala o [dotnet ef](../miscellaneous/cli/dotnet.md) e o pacote de design necessário para executar o comando em um projeto.</span><span class="sxs-lookup"><span data-stu-id="2cc42-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="2cc42-145">O comando `migrations` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="2cc42-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="2cc42-146">O comando `database update` cria o banco de dados e aplica a nova migração a ele.</span><span class="sxs-lookup"><span data-stu-id="2cc42-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2cc42-148">Execute os seguintes comandos no **Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="2cc42-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="2cc42-149">Isso instala as [ferramentas do PMC para EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2cc42-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="2cc42-150">O comando `Add-Migration` realiza o scaffolding de uma migração e cria o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="2cc42-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="2cc42-151">O comando `Update-Database` cria o banco de dados e aplica a nova migração a ele.</span><span class="sxs-lookup"><span data-stu-id="2cc42-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="2cc42-152">Criar, ler, atualizar e excluir</span><span class="sxs-lookup"><span data-stu-id="2cc42-152">Create, read, update & delete</span></span>

* <span data-ttu-id="2cc42-153">Abra *Program.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="2cc42-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="2cc42-154">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2cc42-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="2cc42-155">CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc42-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="2cc42-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2cc42-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2cc42-157">O Visual Studio usa um diretório de trabalho divergente ao executar aplicativos de console do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2cc42-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="2cc42-158">(confira [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) Isso faz com que uma exceção seja gerada: *não existe essa tabela: Blogs*.</span><span class="sxs-lookup"><span data-stu-id="2cc42-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="2cc42-159">Para atualizar o diretório de trabalho:</span><span class="sxs-lookup"><span data-stu-id="2cc42-159">To update the working directory:</span></span>

* <span data-ttu-id="2cc42-160">clique com o botão direito do mouse no projeto e selecione **Editar Arquivo de Projeto**</span><span class="sxs-lookup"><span data-stu-id="2cc42-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="2cc42-161">Logo abaixo da propriedade *TargetFramework*, adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="2cc42-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="2cc42-162">Salve o arquivo</span><span class="sxs-lookup"><span data-stu-id="2cc42-162">Save the file</span></span>

<span data-ttu-id="2cc42-163">Agora, você pode executar o aplicativo:</span><span class="sxs-lookup"><span data-stu-id="2cc42-163">Now you can run the app:</span></span>

* <span data-ttu-id="2cc42-164">**Depurar > Iniciar sem depuração**</span><span class="sxs-lookup"><span data-stu-id="2cc42-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="2cc42-165">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2cc42-165">Next steps</span></span>

* <span data-ttu-id="2cc42-166">Siga o [Tutorial do ASP.NET Core](/aspnet/core/data/ef-rp/intro) para usar o EF Core em um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="2cc42-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="2cc42-167">Saiba mais sobre as [expressões de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="2cc42-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="2cc42-168">[Configure seu modelo](xref:core/modeling/index) para especificar configurações como [obrigatório](xref:core/modeling/entity-properties#required-and-optional-properties) e [comprimento máximo](xref:core/modeling/entity-properties#maximum-length)</span><span class="sxs-lookup"><span data-stu-id="2cc42-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="2cc42-169">Use [Migrações](xref:core/managing-schemas/migrations/index) para atualizar o esquema do banco de dados após alterar seu modelo</span><span class="sxs-lookup"><span data-stu-id="2cc42-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
