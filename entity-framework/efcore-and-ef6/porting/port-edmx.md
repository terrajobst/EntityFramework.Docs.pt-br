---
title: Portando de EF6 para EF Core – portando um modelo baseado em EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997405"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="95b22-102">Portando um modelo baseado em EDMX do EF6 para EF Core</span><span class="sxs-lookup"><span data-stu-id="95b22-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="95b22-103">O EF Core não suporta o formato de arquivo EDMX para modelos.</span><span class="sxs-lookup"><span data-stu-id="95b22-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="95b22-104">A melhor opção para esses modelos, a porta é gerar um novo modelo baseado em código do banco de dados para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="95b22-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="95b22-105">Instalar os pacotes NuGet do EF Core</span><span class="sxs-lookup"><span data-stu-id="95b22-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="95b22-106">Instalar o `Microsoft.EntityFrameworkCore.Tools` pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="95b22-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="95b22-107">Gerar novamente o modelo</span><span class="sxs-lookup"><span data-stu-id="95b22-107">Regenerate the model</span></span>

<span data-ttu-id="95b22-108">Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="95b22-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="95b22-109">Execute o seguinte comando no Console do Gerenciador de pacotes (ferramentas do Gerenciador de pacotes NuGet de –> –> Package Manager Console).</span><span class="sxs-lookup"><span data-stu-id="95b22-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="95b22-110">Ver [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opções de comando para criar o scaffolding de um subconjunto de tabelas etc.</span><span class="sxs-lookup"><span data-stu-id="95b22-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="95b22-111">Por exemplo, aqui está o comando para criar o scaffolding de um modelo do banco de dados de blogs em sua instância de LocalDB do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="95b22-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="95b22-112">Remover o modelo do EF6</span><span class="sxs-lookup"><span data-stu-id="95b22-112">Remove EF6 model</span></span>

<span data-ttu-id="95b22-113">Agora, você deve remover o modelo do EF6 de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="95b22-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="95b22-114">É suficiente deixar o pacote NuGet do EF6 (EntityFramework) instalado, como o EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="95b22-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="95b22-115">No entanto, se você não estiver pretendendo usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="95b22-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="95b22-116">Atualizar seu código</span><span class="sxs-lookup"><span data-stu-id="95b22-116">Update your code</span></span>

<span data-ttu-id="95b22-117">Neste ponto, é uma questão de endereçamento de erros de compilação e revisão de código para ver se as alterações de comportamento entre o EF6 e EF Core afetarão você.</span><span class="sxs-lookup"><span data-stu-id="95b22-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="95b22-118">A porta de teste</span><span class="sxs-lookup"><span data-stu-id="95b22-118">Test the port</span></span>

<span data-ttu-id="95b22-119">Só porque seu aplicativo for compilado, não significa que ele é movido com êxito para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="95b22-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="95b22-120">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tiver afetado negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="95b22-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="95b22-121">Ver [Introdução ao EF Core no ASP.NET Core com o banco de dados existente](xref:core/get-started/aspnetcore/existing-db) para uma referência adicional sobre como trabalhar com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="95b22-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
