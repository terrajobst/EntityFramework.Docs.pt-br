---
title: Portando de EF6 para EF Core portando um modelo baseado em código-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181221"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="7f145-102">Portando um modelo baseado em código EF6 para EF Core</span><span class="sxs-lookup"><span data-stu-id="7f145-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="7f145-103">Se você leu todas as advertências e está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.</span><span class="sxs-lookup"><span data-stu-id="7f145-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="7f145-104">Instalar EF Core pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="7f145-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="7f145-105">Para usar EF Core, instale o pacote NuGet para o provedor de banco de dados que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="7f145-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="7f145-106">Por exemplo, ao direcionar SQL Server, você instalará `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="7f145-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="7f145-107">Consulte [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="7f145-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="7f145-108">Se você estiver planejando usar migrações, instale também o pacote `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="7f145-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="7f145-109">É bom deixar o EntityFramework (EF6 NuGet Package) instalado, pois EF Core e EF6 podem ser usados lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7f145-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="7f145-110">No entanto, se você não pretende usar o EF6 em nenhuma área do seu aplicativo, a desinstalação do pacote ajudará a fornecer erros de compilação em partes de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="7f145-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="7f145-111">Alternar namespaces</span><span class="sxs-lookup"><span data-stu-id="7f145-111">Swap namespaces</span></span>

<span data-ttu-id="7f145-112">A maioria das APIs que você usa em EF6 está no namespace `System.Data.Entity` (e subnamespaces relacionados).</span><span class="sxs-lookup"><span data-stu-id="7f145-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="7f145-113">A primeira alteração de código é alternar para o namespace `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="7f145-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="7f145-114">Normalmente, você começaria com o arquivo de código de contexto derivado e, em seguida, trabalhará a partir daí, abordando erros de compilação à medida que eles ocorrerem.</span><span class="sxs-lookup"><span data-stu-id="7f145-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="7f145-115">Configuração de contexto (conexão etc.)</span><span class="sxs-lookup"><span data-stu-id="7f145-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="7f145-116">Conforme descrito em [garantir que EF Core funcionará para seu aplicativo](ensure-requirements.md), EF Core tem menos mágica em relação à detecção do banco de dados ao qual se conectar.</span><span class="sxs-lookup"><span data-stu-id="7f145-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="7f145-117">Será necessário substituir o método `OnConfiguring` em seu contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7f145-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="7f145-118">A maioria dos aplicativos EF6 armazena a cadeia de conexão no arquivo Applications `App/Web.config`.</span><span class="sxs-lookup"><span data-stu-id="7f145-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="7f145-119">No EF Core, você lê essa cadeia de conexão usando a API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="7f145-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="7f145-120">Talvez seja necessário adicionar uma referência ao assembly da estrutura `System.Configuration` para poder usar essa API.</span><span class="sxs-lookup"><span data-stu-id="7f145-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="7f145-121">Atualizar seu código</span><span class="sxs-lookup"><span data-stu-id="7f145-121">Update your code</span></span>

<span data-ttu-id="7f145-122">Neste ponto, é uma questão de resolver erros de compilação e revisar o código para ver se as alterações de comportamento afetarão você.</span><span class="sxs-lookup"><span data-stu-id="7f145-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="7f145-123">Migrações existentes</span><span class="sxs-lookup"><span data-stu-id="7f145-123">Existing migrations</span></span>

<span data-ttu-id="7f145-124">Na verdade, não há uma maneira viável de portar migrações EF6 existentes para EF Core.</span><span class="sxs-lookup"><span data-stu-id="7f145-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="7f145-125">Se possível, é melhor pressupor que todas as migrações anteriores de EF6 tenham sido aplicadas ao banco de dados e, em seguida, começar a migrar o esquema desse ponto usando EF Core.</span><span class="sxs-lookup"><span data-stu-id="7f145-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="7f145-126">Para fazer isso, você usaria o comando `Add-Migration` para adicionar uma migração depois que o modelo é movido para EF Core.</span><span class="sxs-lookup"><span data-stu-id="7f145-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="7f145-127">Em seguida, você removeria todo o código dos métodos `Up` e `Down` da migração com Scaffold.</span><span class="sxs-lookup"><span data-stu-id="7f145-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="7f145-128">As migrações subsequentes serão comparadas com o modelo quando a migração inicial for com Scaffold.</span><span class="sxs-lookup"><span data-stu-id="7f145-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="7f145-129">Testar a porta</span><span class="sxs-lookup"><span data-stu-id="7f145-129">Test the port</span></span>

<span data-ttu-id="7f145-130">Só porque seu aplicativo é compilado, não significa que ele foi portado com êxito para EF Core.</span><span class="sxs-lookup"><span data-stu-id="7f145-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="7f145-131">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento afetou negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7f145-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
