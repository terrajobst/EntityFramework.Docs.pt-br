---
title: Portando de EF6 para EF Core portando um modelo baseado em EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148995"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="4cfa3-102">Portando um modelo baseado em EF6 EDMX para EF Core</span><span class="sxs-lookup"><span data-stu-id="4cfa3-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="4cfa3-103">EF Core não oferece suporte ao formato de arquivo EDMX para modelos.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="4cfa3-104">A melhor opção para portar esses modelos é gerar um novo modelo baseado em código a partir do banco de dados para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="4cfa3-105">Instalar EF Core pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="4cfa3-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="4cfa3-106">Instale o pacote do NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="4cfa3-107">Regenerar o modelo</span><span class="sxs-lookup"><span data-stu-id="4cfa3-107">Regenerate the model</span></span>

<span data-ttu-id="4cfa3-108">Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo com base em seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="4cfa3-109">Execute o seguinte comando no console do Gerenciador de pacotes (Ferramentas – > Gerenciador de pacotes NuGet – console do Gerenciador de pacotes do >).</span><span class="sxs-lookup"><span data-stu-id="4cfa3-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="4cfa3-110">Consulte [console do Gerenciador de pacotes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para obter opções de comando para Scaffold um subconjunto de tabelas, etc.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="4cfa3-111">Por exemplo, aqui está o comando para Scaffold um modelo do banco de dados de Blogs em sua instância do SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="4cfa3-112">Remover modelo EF6</span><span class="sxs-lookup"><span data-stu-id="4cfa3-112">Remove EF6 model</span></span>

<span data-ttu-id="4cfa3-113">Agora, você removeria o modelo EF6 do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="4cfa3-114">É bom deixar o EntityFramework (EF6 NuGet Package) instalado, pois EF Core e EF6 podem ser usados lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="4cfa3-115">No entanto, se você não pretende usar o EF6 em nenhuma área do seu aplicativo, a desinstalação do pacote ajudará a fornecer erros de compilação em partes de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="4cfa3-116">Atualizar seu código</span><span class="sxs-lookup"><span data-stu-id="4cfa3-116">Update your code</span></span>

<span data-ttu-id="4cfa3-117">Neste ponto, é uma questão de resolver erros de compilação e revisar o código para ver se o comportamento é alterado entre EF6 e EF Core afetará você.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="4cfa3-118">Testar a porta</span><span class="sxs-lookup"><span data-stu-id="4cfa3-118">Test the port</span></span>

<span data-ttu-id="4cfa3-119">Só porque seu aplicativo é compilado, não significa que ele foi portado com êxito para EF Core.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="4cfa3-120">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento afetou negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4cfa3-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
