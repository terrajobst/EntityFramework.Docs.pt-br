---
title: Portando de EF6 para EF Core - Portando um modelo baseado em EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416919"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="86111-102">Portando um modelo baseado em EDMX EF6 para o EF Core</span><span class="sxs-lookup"><span data-stu-id="86111-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="86111-103">O EF Core não suporta o formato de arquivo EDMX para modelos.</span><span class="sxs-lookup"><span data-stu-id="86111-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="86111-104">A melhor opção para portar esses modelos, é gerar um novo modelo baseado em código a partir do banco de dados para sua aplicação.</span><span class="sxs-lookup"><span data-stu-id="86111-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="86111-105">Instale pacotes EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="86111-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="86111-106">Instale o pacote do NuGet `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="86111-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="86111-107">Regenerar o modelo</span><span class="sxs-lookup"><span data-stu-id="86111-107">Regenerate the model</span></span>

<span data-ttu-id="86111-108">Agora você pode usar a funcionalidade de engenharia reversa para criar um modelo baseado no seu banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="86111-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="86111-109">Execute o seguinte comando no Console do Gerenciador de Pacotes (Ferramentas –> NuGet Package Manager –> Console de Gerenciador de Pacotes).</span><span class="sxs-lookup"><span data-stu-id="86111-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="86111-110">Consulte [O Console do Gerenciador de Pacotes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para obter opções de comando para andaime de um subconjunto de tabelas etc.</span><span class="sxs-lookup"><span data-stu-id="86111-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="86111-111">Por exemplo, aqui está o comando para andaime um modelo do banco de dados Blogging na instância SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="86111-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="86111-112">Remover modelo EF6</span><span class="sxs-lookup"><span data-stu-id="86111-112">Remove EF6 model</span></span>

<span data-ttu-id="86111-113">Agora você removeria o modelo EF6 da sua aplicação.</span><span class="sxs-lookup"><span data-stu-id="86111-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="86111-114">É bom deixar o pacote EF6 NuGet (EntityFramework) instalado, pois eF Core e EF6 podem ser usados lado a lado na mesma aplicação.</span><span class="sxs-lookup"><span data-stu-id="86111-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="86111-115">No entanto, se você não pretende usar o EF6 em qualquer área do seu aplicativo, então a desinstalação do pacote ajudará a dar erros de compilação em pedaços de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="86111-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="86111-116">Atualize seu código</span><span class="sxs-lookup"><span data-stu-id="86111-116">Update your code</span></span>

<span data-ttu-id="86111-117">Neste ponto, é uma questão de abordar erros de compilação e revisar código para ver se as mudanças de comportamento entre eF6 e EF Core irão impactá-lo.</span><span class="sxs-lookup"><span data-stu-id="86111-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="86111-118">Teste a porta</span><span class="sxs-lookup"><span data-stu-id="86111-118">Test the port</span></span>

<span data-ttu-id="86111-119">Só porque seu aplicativo compila, não significa que ele seja portado com sucesso para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="86111-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="86111-120">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tenha afetado negativamente sua aplicação.</span><span class="sxs-lookup"><span data-stu-id="86111-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
