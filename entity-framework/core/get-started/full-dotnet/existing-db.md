---
title: Introdução ao .NET Framework – Banco de dados existente – EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: edcdc0b76394c4d604cf43fc170424e474532b17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993412"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="7635a-102">Introdução ao EF Core no .NET Framework com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="7635a-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="7635a-103">Neste tutorial, você compila um aplicativo de console que realiza o acesso básico a dados em um banco de dados do Microsoft SQL Server usando o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7635a-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="7635a-104">Você criará um modelo do Entity Framework realizando a engenharia reversa de um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7635a-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="7635a-105">[Exiba o exemplo deste artigo no GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="7635a-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7635a-106">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="7635a-106">Prerequisites</span></span>

* [<span data-ttu-id="7635a-107">Visual Studio 2017 versão 15.7 ou posterior</span><span class="sxs-lookup"><span data-stu-id="7635a-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="7635a-108">Criar banco de dados de blog</span><span class="sxs-lookup"><span data-stu-id="7635a-108">Create Blogging database</span></span>

<span data-ttu-id="7635a-109">Este tutorial usa um banco de dados de **blog** em sua instância do LocalDb como o banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7635a-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="7635a-110">Se você já tiver criado o banco de dados de **blog** como parte de outro tutorial, ignore estas etapas.</span><span class="sxs-lookup"><span data-stu-id="7635a-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="7635a-111">Abrir o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7635a-111">Open Visual Studio</span></span>

* <span data-ttu-id="7635a-112">**Ferramentas > Conectar ao Banco de Dados...**</span><span class="sxs-lookup"><span data-stu-id="7635a-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="7635a-113">Selecione **Microsoft SQL Server** e clique em **Continuar**</span><span class="sxs-lookup"><span data-stu-id="7635a-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="7635a-114">Insira **(localdb)\mssqllocaldb** como o **Nome do Servidor**</span><span class="sxs-lookup"><span data-stu-id="7635a-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="7635a-115">Insira **mestre** como o **Nome do Banco de Dados** e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="7635a-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="7635a-116">O banco de dados mestre agora é exibido em **Conexões de Dados** no **Gerenciador de Servidores**</span><span class="sxs-lookup"><span data-stu-id="7635a-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="7635a-117">Clique com botão direito do mouse no banco de dados em **Gerenciador de Servidores** e selecione **Nova Consulta**</span><span class="sxs-lookup"><span data-stu-id="7635a-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="7635a-118">Copie o script listado abaixo para o editor de consultas</span><span class="sxs-lookup"><span data-stu-id="7635a-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="7635a-119">Clique com o botão direito do mouse no editor de consultas e selecione **Executar**</span><span class="sxs-lookup"><span data-stu-id="7635a-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="7635a-120">Criar um novo projeto</span><span class="sxs-lookup"><span data-stu-id="7635a-120">Create a new project</span></span>

* <span data-ttu-id="7635a-121">Abra o Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7635a-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="7635a-122">**Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="7635a-122">**File > New > Project...**</span></span>

* <span data-ttu-id="7635a-123">No menu à esquerda, selecione **Instalado > Visual C# > Área de Trabalho do Windows**</span><span class="sxs-lookup"><span data-stu-id="7635a-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="7635a-124">Selecione o modelo de projeto **Aplicativo de console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="7635a-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="7635a-125">Verifique se o projeto direciona o **.NET Framework 4.6.1** ou posterior</span><span class="sxs-lookup"><span data-stu-id="7635a-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="7635a-126">Nomeie o projeto *ConsoleApp.ExistingDb* e clique em **OK**</span><span class="sxs-lookup"><span data-stu-id="7635a-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="7635a-127">Instalar o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7635a-127">Install Entity Framework</span></span>

<span data-ttu-id="7635a-128">Para usar o EF Core, instale o pacote do provedor do banco de dados para o qual você deseja direcionar.</span><span class="sxs-lookup"><span data-stu-id="7635a-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7635a-129">Este tutorial usa o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7635a-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="7635a-130">Para obter uma lista de provedores disponíveis, veja [Provedores de Banco de Dados](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7635a-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7635a-131">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="7635a-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="7635a-132">Execute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="7635a-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="7635a-133">Na próxima etapa, você usará o Entity Framework Tools para realizar a engenharia reversa do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7635a-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="7635a-134">Portanto, instale também o pacote de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="7635a-134">So install the tools package as well.</span></span>

* <span data-ttu-id="7635a-135">Execute `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="7635a-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="7635a-136">Realizar engenharia reversa do modelo</span><span class="sxs-lookup"><span data-stu-id="7635a-136">Reverse engineer the model</span></span>

<span data-ttu-id="7635a-137">Agora é hora de criar o modelo EF com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7635a-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="7635a-138">**Ferramentas > Gerenciador de Pacotes NuGet > Console do Gerenciador de Pacotes**</span><span class="sxs-lookup"><span data-stu-id="7635a-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="7635a-139">Execute o seguinte comando para criar um modelo do banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="7635a-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="7635a-140">É possível especificar as tabelas para as quais gerar entidades adicionado o argumento `-Tables` ao comando acima.</span><span class="sxs-lookup"><span data-stu-id="7635a-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="7635a-141">Por exemplo, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="7635a-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="7635a-142">O processo de engenharia reversa criou classes de entidade (`Blog` e `Post`) e um contexto derivado (`BloggingContext`) com base no esquema do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="7635a-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="7635a-143">As classes de entidade são objetos C# simples que representam os dados que você vai consultar e salvar.</span><span class="sxs-lookup"><span data-stu-id="7635a-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="7635a-144">Veja as classes de entidade `Blog` e `Post`:</span><span class="sxs-lookup"><span data-stu-id="7635a-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="7635a-145">Para habilitar o carregamento lento, crie propriedades de navegação `virtual` (Blog.Post e Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="7635a-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="7635a-146">O contexto representa uma sessão com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7635a-146">The context represents a session with the database.</span></span> <span data-ttu-id="7635a-147">Ele tem métodos que você pode usar para consultar e salvar instâncias das classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="7635a-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="7635a-148">Usar o modelo</span><span class="sxs-lookup"><span data-stu-id="7635a-148">Use the model</span></span>

<span data-ttu-id="7635a-149">Agora é possível usar o modelo para executar o acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="7635a-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="7635a-150">Abra *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="7635a-150">Open *Program.cs*</span></span>

* <span data-ttu-id="7635a-151">Substitua o conteúdo do arquivo pelo seguinte código</span><span class="sxs-lookup"><span data-stu-id="7635a-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="7635a-152">Depurar > Iniciar Sem Depuração</span><span class="sxs-lookup"><span data-stu-id="7635a-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="7635a-153">Repare que um blog é salvo no banco de dados, e os detalhes de todos os blogs são impressos no console.</span><span class="sxs-lookup"><span data-stu-id="7635a-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![imagem](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="7635a-155">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7635a-155">Additional Resources</span></span>

* [<span data-ttu-id="7635a-156">EF Core no .NET Framework com um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="7635a-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="7635a-157">[EF Core no .NET Core com um novo banco de dados, SQLite](xref:core/get-started/netcore/new-db-sqlite), um tutorial do EF de console de plataforma cruzada.</span><span class="sxs-lookup"><span data-stu-id="7635a-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
