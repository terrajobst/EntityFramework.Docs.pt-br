---
title: Introdução ao .NET Framework – Novo banco de dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614422"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="ff9ab-102">Introdução ao EF Core no .NET Framework com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="ff9ab-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="ff9ab-103">Neste tutorial, você compila um aplicativo de console que realiza o acesso básico a dados em um banco de dados do Microsoft SQL Server usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="ff9ab-104">Você usará migrações para criar o banco de dados com base em um modelo.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="ff9ab-105">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="ff9ab-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff9ab-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ff9ab-106">Prerequisites</span></span>

* [<span data-ttu-id="ff9ab-107">Visual Studio 2017 versão 15.7 ou posterior</span><span class="sxs-lookup"><span data-stu-id="ff9ab-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="ff9ab-108">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="ff9ab-108">Create a new project</span></span>

* <span data-ttu-id="ff9ab-109">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ff9ab-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="ff9ab-110">**Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-110">**File > New > Project...**</span></span>

* <span data-ttu-id="ff9ab-111">No menu à esquerda, selecione **Instalado > Visual C# > Área de Trabalho do Windows**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="ff9ab-112">Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="ff9ab-113">Verifique se o projeto direciona o **.NET Framework 4.6.1** ou posterior</span><span class="sxs-lookup"><span data-stu-id="ff9ab-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="ff9ab-114">Nomeie o projeto *ConsoleApp.NewDb* e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ff9ab-115">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ff9ab-115">Install Entity Framework</span></span>

<span data-ttu-id="ff9ab-116">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ff9ab-117">Este tutorial usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="ff9ab-118">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ff9ab-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ff9ab-119">Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes</span><span class="sxs-lookup"><span data-stu-id="ff9ab-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="ff9ab-120">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="ff9ab-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="ff9ab-121">Mais adiante neste tutorial, você usará o Entity Framework Tools para manter o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="ff9ab-122">Portanto, instale também o pacote de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-122">So install the tools package as well.</span></span>

* <span data-ttu-id="ff9ab-123">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="ff9ab-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="ff9ab-124">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="ff9ab-124">Create the model</span></span>

<span data-ttu-id="ff9ab-125">Agora é hora de definir um contexto e classes de entidade que compõem o modelo.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="ff9ab-126">**Projeto > Adicionar classe...**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="ff9ab-127">Insira *Model.cs* como o nome e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="ff9ab-128">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="ff9ab-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="ff9ab-129">Em um aplicativo real, você colocaria cada classe em um arquivo separado e colocaria a cadeia de conexão em um arquivo de configuração ou na variável de ambiente.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="ff9ab-130">Para simplificar, tudo está em um único arquivo de código para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="ff9ab-131">Criar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="ff9ab-131">Create the database</span></span>

<span data-ttu-id="ff9ab-132">Agora que você tem um modelo, é possível usar as migrações para criar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="ff9ab-133">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ff9ab-134">Execute `Add-Migration InitialCreate` para gerar uma migração e criar o conjunto inicial de tabelas para o modelo.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="ff9ab-135">Execute `Update-Database` para aplicar a nova migração ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="ff9ab-136">Como o banco de dados ainda não existe, ele será criado antes que a migração seja aplicada.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="ff9ab-137">Se você fizer alterações no modelo, use o comando `Add-Migration` para gerar uma nova migração para fazer as alterações correspondentes de esquema no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="ff9ab-138">Depois de verificar o código após o scaffolding (e fazer as alterações necessárias), use o comando `Update-Database` para aplicar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="ff9ab-139">O EF usa uma tabela `__EFMigrationsHistory` no banco de dados para controlar quais migrações já foram aplicadas ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="ff9ab-140">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="ff9ab-140">Use the model</span></span>

<span data-ttu-id="ff9ab-141">Agora é possível usar o modelo para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="ff9ab-142">Abra *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="ff9ab-142">Open *Program.cs*</span></span>

* <span data-ttu-id="ff9ab-143">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="ff9ab-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="ff9ab-144">**Depurar > Iniciar sem depuração**</span><span class="sxs-lookup"><span data-stu-id="ff9ab-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="ff9ab-145">Repare que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![imagem](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="ff9ab-147">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ff9ab-147">Additional Resources</span></span>

* [<span data-ttu-id="ff9ab-148">EF Core no .NET Framework com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="ff9ab-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="ff9ab-149">[EF Core no .NET Core com um novo banco de dados, SQLite](xref:core/get-started/netcore/new-db-sqlite), um tutorial do EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="ff9ab-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
