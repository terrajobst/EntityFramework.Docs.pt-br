---
title: Portando de EF6 EF núcleo - portando um modelo baseado em EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812684"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="20cde-102">Portabilidade de um modelo de baseado em EDMX EF6 EF Core</span><span class="sxs-lookup"><span data-stu-id="20cde-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="20cde-103">EF Core não suporta o formato do arquivo EDMX para modelos.</span><span class="sxs-lookup"><span data-stu-id="20cde-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="20cde-104">É a melhor opção para a porta esses modelos gerar um novo modelo com base em código do banco de dados para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20cde-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="20cde-105">Instalar os pacotes NuGet do núcleo de EF</span><span class="sxs-lookup"><span data-stu-id="20cde-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="20cde-106">Instalar o `Microsoft.EntityFrameworkCore.Tools` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="20cde-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="20cde-107">Gerar novamente o modelo</span><span class="sxs-lookup"><span data-stu-id="20cde-107">Regenerate the model</span></span>

<span data-ttu-id="20cde-108">Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="20cde-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="20cde-109">Execute o seguinte comando no Console do Gerenciador de pacotes (Ferramentas –> NuGet Package Manager –> Package Manager Console).</span><span class="sxs-lookup"><span data-stu-id="20cde-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="20cde-110">Consulte [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opções de comando Scaffold um subconjunto de tabelas etc.</span><span class="sxs-lookup"><span data-stu-id="20cde-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="20cde-111">Por exemplo, aqui está o comando Scaffold um modelo do banco de dados de blogs em sua instância de LocalDB do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20cde-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="20cde-112">Remover modelo EF6</span><span class="sxs-lookup"><span data-stu-id="20cde-112">Remove EF6 model</span></span>

<span data-ttu-id="20cde-113">Agora, você deverá remover o modelo de EF6 do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20cde-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="20cde-114">Há problema em manter o pacote do NuGet EF6 (EntityFramework) instalado, como EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20cde-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="20cde-115">No entanto, se você não pretender usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="20cde-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="20cde-116">Atualize seu código</span><span class="sxs-lookup"><span data-stu-id="20cde-116">Update your code</span></span>

<span data-ttu-id="20cde-117">Neste ponto, é uma questão de endereçamento erros de compilação e revisão de código para ver se as alterações de comportamento entre EF6 e Core EF afetará.</span><span class="sxs-lookup"><span data-stu-id="20cde-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="20cde-118">A porta de teste</span><span class="sxs-lookup"><span data-stu-id="20cde-118">Test the port</span></span>

<span data-ttu-id="20cde-119">Só porque seu aplicativo é compilado, não significa-lo com êxito é movido para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cde-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="20cde-120">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento ter afetado negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="20cde-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="20cde-121">Consulte [guia de Introdução ao EF Core no núcleo do ASP.NET com um banco de dados](xref:core/get-started/aspnetcore/existing-db) de uma referência adicional sobre como trabalhar com um banco de dados existente</span><span class="sxs-lookup"><span data-stu-id="20cde-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
