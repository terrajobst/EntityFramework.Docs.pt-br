---
title: Portando de EF6 EF núcleo - portando um modelo baseado em código
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052946"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="28b63-102">Portando um EF6 baseada em código modelo EF Core</span><span class="sxs-lookup"><span data-stu-id="28b63-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="28b63-103">Se você leu todos os avisos e você está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.</span><span class="sxs-lookup"><span data-stu-id="28b63-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="28b63-104">Instalar os pacotes NuGet do núcleo de EF</span><span class="sxs-lookup"><span data-stu-id="28b63-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="28b63-105">Para usar o EF Core, você pode instalar o pacote NuGet para o provedor de banco de dados que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="28b63-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="28b63-106">Por exemplo, durante o direcionamento do SQL Server, você deve instalar `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="28b63-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="28b63-107">Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="28b63-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="28b63-108">Se você estiver planejando usar migrações, você também deve instalar o `Microsoft.EntityFrameworkCore.Tools` pacote.</span><span class="sxs-lookup"><span data-stu-id="28b63-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="28b63-109">Há problema em manter o pacote do NuGet EF6 (EntityFramework) instalado, como EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="28b63-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="28b63-110">No entanto, se você não pretender usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="28b63-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="28b63-111">Namespaces de troca</span><span class="sxs-lookup"><span data-stu-id="28b63-111">Swap namespaces</span></span>

<span data-ttu-id="28b63-112">A maioria das APIs que você use EF6 estão no `System.Data.Entity` namespace (e relacionados subnamespaces).</span><span class="sxs-lookup"><span data-stu-id="28b63-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="28b63-113">A primeira alteração de código é alternar para o `Microsoft.EntityFrameworkCore` namespace.</span><span class="sxs-lookup"><span data-stu-id="28b63-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="28b63-114">Você teria iniciam normalmente com seu arquivo de código do contexto derivada e, em seguida, calcular a partir daí, erros de compilação de endereçamento que eles ocorrem.</span><span class="sxs-lookup"><span data-stu-id="28b63-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="28b63-115">Configuração de contexto (conexão etc.)</span><span class="sxs-lookup"><span data-stu-id="28b63-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="28b63-116">Conforme descrito em [garantir EF será trabalho para seu aplicativo](ensure-requirements.md), Core EF tem menos magic em torno de detectar o banco de dados para se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="28b63-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="28b63-117">Você precisará substituir o `OnConfiguring` método no contexto derivado e use a API específica do provedor de banco de dados para a conexão ao banco de dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="28b63-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="28b63-118">A maioria dos aplicativos de EF6 armazenar a cadeia de caracteres de conexão em aplicativos `App/Web.config` arquivo.</span><span class="sxs-lookup"><span data-stu-id="28b63-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="28b63-119">No núcleo do EF, leia esta cadeia de caracteres de conexão usando o `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="28b63-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="28b63-120">Talvez seja necessário adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.</span><span class="sxs-lookup"><span data-stu-id="28b63-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="28b63-121">Atualize seu código</span><span class="sxs-lookup"><span data-stu-id="28b63-121">Update your code</span></span>

<span data-ttu-id="28b63-122">Neste ponto, é uma questão de abordar os erros de compilação e revisão de código para verificar se as alterações de comportamento afetam você.</span><span class="sxs-lookup"><span data-stu-id="28b63-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="28b63-123">Migrações existentes</span><span class="sxs-lookup"><span data-stu-id="28b63-123">Existing migrations</span></span>

<span data-ttu-id="28b63-124">Realmente, não existe uma forma viável para a porta existentes migrações EF6 EF Core.</span><span class="sxs-lookup"><span data-stu-id="28b63-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="28b63-125">Se possível, é melhor presumir que todas as migrações anteriores de EF6 foram aplicadas ao banco de dados e iniciar migrando o esquema do que aponte usando EF Core.</span><span class="sxs-lookup"><span data-stu-id="28b63-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="28b63-126">Para fazer isso, você usaria o `Add-Migration` comando para adicionar uma migração, depois que o modelo é movido para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="28b63-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="28b63-127">Em seguida, você poderia remover todo o código do `Up` e `Down` métodos de migração scaffolding.</span><span class="sxs-lookup"><span data-stu-id="28b63-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="28b63-128">Migrações subsequentes comparará no modelo quando essa migração inicial foi Scaffold.</span><span class="sxs-lookup"><span data-stu-id="28b63-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="28b63-129">A porta de teste</span><span class="sxs-lookup"><span data-stu-id="28b63-129">Test the port</span></span>

<span data-ttu-id="28b63-130">Só porque seu aplicativo é compilado, não significa-lo com êxito é movido para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="28b63-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="28b63-131">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento ter afetado negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="28b63-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
