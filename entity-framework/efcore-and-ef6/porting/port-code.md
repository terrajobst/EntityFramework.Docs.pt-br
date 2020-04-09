---
title: Portando de EF6 para EF Core - Portando um modelo baseado em código - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419633"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="31b14-102">Portando um modelo baseado em código EF6 para o Núcleo EF</span><span class="sxs-lookup"><span data-stu-id="31b14-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="31b14-103">Se você leu todas as ressalvas e está pronto para portar, então aqui estão algumas diretrizes para ajudá-lo a começar.</span><span class="sxs-lookup"><span data-stu-id="31b14-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="31b14-104">Instale pacotes EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="31b14-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="31b14-105">Para usar o EF Core, você instala o pacote NuGet para o provedor de banco de dados que deseja usar.</span><span class="sxs-lookup"><span data-stu-id="31b14-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="31b14-106">Por exemplo, ao direcionar o SQL `Microsoft.EntityFrameworkCore.SqlServer`Server, você instalaria .</span><span class="sxs-lookup"><span data-stu-id="31b14-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="31b14-107">Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="31b14-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="31b14-108">Se você está planejando usar migrações, então `Microsoft.EntityFrameworkCore.Tools` você também deve instalar o pacote.</span><span class="sxs-lookup"><span data-stu-id="31b14-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="31b14-109">É bom deixar o pacote EF6 NuGet (EntityFramework) instalado, pois eF Core e EF6 podem ser usados lado a lado na mesma aplicação.</span><span class="sxs-lookup"><span data-stu-id="31b14-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="31b14-110">No entanto, se você não pretende usar o EF6 em qualquer área do seu aplicativo, então a desinstalação do pacote ajudará a dar erros de compilação em pedaços de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="31b14-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="31b14-111">Trocar espaços de nome</span><span class="sxs-lookup"><span data-stu-id="31b14-111">Swap namespaces</span></span>

<span data-ttu-id="31b14-112">A maioria das APIs que você `System.Data.Entity` usa no EF6 estão no namespace (e sub-nomes relacionados).</span><span class="sxs-lookup"><span data-stu-id="31b14-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="31b14-113">A primeira mudança de código `Microsoft.EntityFrameworkCore` é trocar para o namespace.</span><span class="sxs-lookup"><span data-stu-id="31b14-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="31b14-114">Você normalmente começaria com seu arquivo de código de contexto derivado e, em seguida, trabalharia a partir daí, abordando erros de compilação à medida que ocorrem.</span><span class="sxs-lookup"><span data-stu-id="31b14-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="31b14-115">Configuração de contexto (conexão etc.)</span><span class="sxs-lookup"><span data-stu-id="31b14-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="31b14-116">Como descrito em [Ensure EF Core Will Work for Your Application](ensure-requirements.md), o EF Core tem menos magia ao detectar o banco de dados para se conectar.</span><span class="sxs-lookup"><span data-stu-id="31b14-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="31b14-117">Você precisará substituir o `OnConfiguring` método em seu contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="31b14-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="31b14-118">A maioria dos aplicativos EF6 armazena `App/Web.config` a seqüência de conexões no arquivo de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="31b14-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="31b14-119">No EF Core, você lê `ConfigurationManager` esta seqüência de conexões usando a API.</span><span class="sxs-lookup"><span data-stu-id="31b14-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="31b14-120">Você pode precisar adicionar uma `System.Configuration` referência ao conjunto de estruturas para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="31b14-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="31b14-121">Atualize seu código</span><span class="sxs-lookup"><span data-stu-id="31b14-121">Update your code</span></span>

<span data-ttu-id="31b14-122">Neste ponto, é uma questão de abordar erros de compilação e revisar código para ver se as mudanças de comportamento irão impactá-lo.</span><span class="sxs-lookup"><span data-stu-id="31b14-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="31b14-123">Migrações existentes</span><span class="sxs-lookup"><span data-stu-id="31b14-123">Existing migrations</span></span>

<span data-ttu-id="31b14-124">Não há realmente uma maneira viável de portar migrações EF6 existentes para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="31b14-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="31b14-125">Se possível, é melhor supor que todas as migrações anteriores do EF6 foram aplicadas ao banco de dados e, em seguida, começar a migrar o esquema a partir desse ponto usando o EF Core.</span><span class="sxs-lookup"><span data-stu-id="31b14-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="31b14-126">Para fazer isso, você `Add-Migration` usaria o comando para adicionar uma migração assim que o modelo for portado para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="31b14-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="31b14-127">Em seguida, você removeria todo o código da `Up` `Down` migração de andaimes.</span><span class="sxs-lookup"><span data-stu-id="31b14-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="31b14-128">As migrações subseqüentes serão comparadas com o modelo quando essa migração inicial foi cadavez mais andaime.</span><span class="sxs-lookup"><span data-stu-id="31b14-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="31b14-129">Teste a porta</span><span class="sxs-lookup"><span data-stu-id="31b14-129">Test the port</span></span>

<span data-ttu-id="31b14-130">Só porque seu aplicativo compila, não significa que ele seja portado com sucesso para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="31b14-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="31b14-131">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tenha afetado negativamente sua aplicação.</span><span class="sxs-lookup"><span data-stu-id="31b14-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
