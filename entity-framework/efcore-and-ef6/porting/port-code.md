---
title: Portando de EF6 para EF Core – portando um modelo baseado em código
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997043"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="8b4de-102">Portando um modelo com base no código do EF6 para EF Core</span><span class="sxs-lookup"><span data-stu-id="8b4de-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="8b4de-103">Se você leu todos os avisos e você está pronto para a porta, aqui estão algumas diretrizes para ajudá-lo a começar.</span><span class="sxs-lookup"><span data-stu-id="8b4de-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="8b4de-104">Instalar os pacotes NuGet do EF Core</span><span class="sxs-lookup"><span data-stu-id="8b4de-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="8b4de-105">Para usar o EF Core, você pode instalar o pacote do NuGet para o provedor de banco de dados que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="8b4de-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="8b4de-106">Por exemplo, ao direcionar o SQL Server, você instalaria `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="8b4de-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="8b4de-107">Ver [provedores de banco de dados](../../core/providers/index.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="8b4de-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="8b4de-108">Se você estiver planejando usar migrações, você também deve instalar o `Microsoft.EntityFrameworkCore.Tools` pacote.</span><span class="sxs-lookup"><span data-stu-id="8b4de-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="8b4de-109">É suficiente deixar o pacote NuGet do EF6 (EntityFramework) instalado, como o EF Core e EF6 podem ser usada lado a lado no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8b4de-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="8b4de-110">No entanto, se você não estiver pretendendo usar EF6 em todas as áreas do seu aplicativo, em seguida, desinstalar o pacote permitirão que os erros de compilação em trechos de código que precisam de atenção.</span><span class="sxs-lookup"><span data-stu-id="8b4de-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="8b4de-111">Namespaces de permuta</span><span class="sxs-lookup"><span data-stu-id="8b4de-111">Swap namespaces</span></span>

<span data-ttu-id="8b4de-112">A maioria das APIs que você usa no EF6 estão no `System.Data.Entity` namespace (e relacionados subnamespaces).</span><span class="sxs-lookup"><span data-stu-id="8b4de-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="8b4de-113">A primeira alteração de código é alternar para o `Microsoft.EntityFrameworkCore` namespace.</span><span class="sxs-lookup"><span data-stu-id="8b4de-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="8b4de-114">Você teria normalmente começa com seu arquivo de código do contexto derivado e, em seguida, descobrir a partir daí, abordar os erros de compilação, conforme elas ocorrem.</span><span class="sxs-lookup"><span data-stu-id="8b4de-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="8b4de-115">Configuração de contexto (conexão etc.)</span><span class="sxs-lookup"><span data-stu-id="8b4de-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="8b4de-116">Conforme descrito em [garantir que o EF Core será trabalho para seu aplicativo](ensure-requirements.md), o EF Core tem menos mágica em torno de detectar o banco de dados para se conectar ao.</span><span class="sxs-lookup"><span data-stu-id="8b4de-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="8b4de-117">Você precisará substituir o `OnConfiguring` método no contexto derivado e usar a API específica do provedor de banco de dados para configurar a conexão ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8b4de-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="8b4de-118">A maioria dos aplicativos EF6 armazenar a cadeia de caracteres de conexão em aplicativos de `App/Web.config` arquivo.</span><span class="sxs-lookup"><span data-stu-id="8b4de-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="8b4de-119">No EF Core, leia esta cadeia de caracteres de conexão usando o `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="8b4de-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="8b4de-120">Talvez você precise adicionar uma referência para o `System.Configuration` assembly do framework para poder usar essa API.</span><span class="sxs-lookup"><span data-stu-id="8b4de-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="8b4de-121">Atualizar seu código</span><span class="sxs-lookup"><span data-stu-id="8b4de-121">Update your code</span></span>

<span data-ttu-id="8b4de-122">Neste ponto, é uma questão de abordar os erros de compilação e revisão de código para ver se as alterações de comportamento afetarão você.</span><span class="sxs-lookup"><span data-stu-id="8b4de-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="8b4de-123">Migrações existentes</span><span class="sxs-lookup"><span data-stu-id="8b4de-123">Existing migrations</span></span>

<span data-ttu-id="8b4de-124">Na verdade, não existe uma forma viável a portabilidade de migrações existentes ao EF6 para EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b4de-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="8b4de-125">Se possível, é melhor presumir que todas as migrações anteriores do EF6 foram aplicadas ao banco de dados e, em seguida, iniciar migrando o esquema do que aponte usando o EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b4de-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="8b4de-126">Para fazer isso, você usaria o `Add-Migration` comando para adicionar uma migração, depois que o modelo é movido para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b4de-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="8b4de-127">Em seguida, você poderia remover todo o código do `Up` e `Down` métodos da migração gerado por scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8b4de-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="8b4de-128">Migrações subsequentes comparará no modelo quando a migração inicial foi gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="8b4de-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="8b4de-129">A porta de teste</span><span class="sxs-lookup"><span data-stu-id="8b4de-129">Test the port</span></span>

<span data-ttu-id="8b4de-130">Só porque seu aplicativo for compilado, não significa que ele é movido com êxito para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="8b4de-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="8b4de-131">Você precisará testar todas as áreas do seu aplicativo para garantir que nenhuma das alterações de comportamento tiver afetado negativamente seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8b4de-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
