---
title: Novos recursos no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867951"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="9c451-102">Novos recursos incluídos no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="9c451-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c451-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="9c451-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="9c451-104">A lista a seguir inclui os principais novos recursos planejados para o EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="9c451-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="9c451-105">A maioria desses recursos não está incluída na versão prévia atual, mas ficará disponível à medida que avançarmos em direção a RTM.</span><span class="sxs-lookup"><span data-stu-id="9c451-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="9c451-106">A razão é que, no início do lançamento, estamos nos concentrando na implementação de [alterações da falha](xref:core/what-is-new/ef-core-3.0/breaking-changes) planejadas.</span><span class="sxs-lookup"><span data-stu-id="9c451-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="9c451-107">Muitas dessas alterações de falha são melhorias para o EF Core por conta própria.</span><span class="sxs-lookup"><span data-stu-id="9c451-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="9c451-108">Muitas outras são necessárias para possibilitar mais melhorias.</span><span class="sxs-lookup"><span data-stu-id="9c451-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="9c451-109">Para obter uma lista completa de correções de bug e aprimoramentos em andamento, você pode ver [esta consulta em nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="9c451-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="9c451-110">Melhorias do LINQ</span><span class="sxs-lookup"><span data-stu-id="9c451-110">LINQ improvements</span></span> 

[<span data-ttu-id="9c451-111">Acompanhamento de problema nº 12795</span><span class="sxs-lookup"><span data-stu-id="9c451-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="9c451-112">O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.</span><span class="sxs-lookup"><span data-stu-id="9c451-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="9c451-113">O LINQ permite que você escreva consultas de banco de dados sem sair da linguagem de sua escolha, aproveitando as informações de tipo avançado para obter IntelliSense e a verificação do tipo em tempo de compilação.</span><span class="sxs-lookup"><span data-stu-id="9c451-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="9c451-114">Mas o LINQ também permite que você escreva um número ilimitado de consultas complicadas, o que sempre foi um enorme desafio para provedores do LINQ.</span><span class="sxs-lookup"><span data-stu-id="9c451-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="9c451-115">Nas primeiras versões do EF Core, solucionamos parte disso ao descobrir quais partes de uma consulta podem ser convertidas para o SQL e depois permitindo que o restante da consulta seja executada na memória no cliente.</span><span class="sxs-lookup"><span data-stu-id="9c451-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="9c451-116">Essa execução do lado do cliente pode ser desejável em algumas situações, mas, em muitos outros casos, pode resultar em consultas ineficientes que podem não ser identificadas até que um aplicativo seja implantado na produção.</span><span class="sxs-lookup"><span data-stu-id="9c451-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="9c451-117">No EF Core 3.0, planejamos fazer alterações profundas no funcionamento da implementação do LINQ e em como faremos os testes.</span><span class="sxs-lookup"><span data-stu-id="9c451-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="9c451-118">Os objetivos são torná-lo mais robusto (por exemplo, evitar a interrupção de consultas em lançamentos de patch), habilitar a conversão correta de mais expressões em SQL, gerar consultas eficientes em mais casos e impedir que as consultas ineficientes não sejam detectadas.</span><span class="sxs-lookup"><span data-stu-id="9c451-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="9c451-119">Suporte do Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9c451-119">Cosmos DB support</span></span> 

[<span data-ttu-id="9c451-120">Acompanhamento de problema nº 8443</span><span class="sxs-lookup"><span data-stu-id="9c451-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="9c451-121">Esse recurso está incluído na versão prévia atual, mas ainda não está completo.</span><span class="sxs-lookup"><span data-stu-id="9c451-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="9c451-122">Estamos trabalhando em um provedor do Cosmos DB para o EF Core para permitir que desenvolvedores familiarizados com o modelo de programação EF conduzam facilmente o Azure Cosmos DB como um banco de dados de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9c451-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="9c451-123">A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência.</span><span class="sxs-lookup"><span data-stu-id="9c451-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="9c451-124">O provedor ativará a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9c451-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="9c451-125">Iniciamos este trabalho antes do EF Core 2.2 e [fizemos algumas versões prévias do provedor disponível](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="9c451-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="9c451-126">O novo plano é continuar desenvolvendo o provedor junto com o EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="9c451-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="9c451-127">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="9c451-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="9c451-128">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="9c451-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="9c451-129">Esse recurso será introduzido no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="9c451-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="9c451-130">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="9c451-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="9c451-131">Da versão 3.0 do EF Core em diante, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, será possível adicionar um `Order` sem um `OrderDetails`, e todas as propriedades `OrderDetails`, com exceção da chave primária, serão mapeadas para colunas que permitam valor nulo.</span><span class="sxs-lookup"><span data-stu-id="9c451-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties except the primary key will be mapped to nullable columns.</span></span>
<span data-ttu-id="9c451-132">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="9c451-132">When querying EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="9c451-133">Suporte do C# 8.0</span><span class="sxs-lookup"><span data-stu-id="9c451-133">C# 8.0 support</span></span>

<span data-ttu-id="9c451-134">[Acompanhamento do problema nº 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Acompanhamento de problema nº 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="9c451-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="9c451-135">O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.</span><span class="sxs-lookup"><span data-stu-id="9c451-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="9c451-136">Queremos que nossos clientes aproveitem alguns dos [novos recursos que virão no C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como os fluxos assíncronos (incluindo `await foreach`) e os tipos de referência nula durante o uso do EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c451-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="9c451-137">Engenharia reversa de exibições de banco de dados</span><span class="sxs-lookup"><span data-stu-id="9c451-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="9c451-138">Acompanhamento de problema nº 1679</span><span class="sxs-lookup"><span data-stu-id="9c451-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="9c451-139">Esse recurso não está incluído na versão prévia atual.</span><span class="sxs-lookup"><span data-stu-id="9c451-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="9c451-140">[Tipos de consulta](xref:core/modeling/query-types), introduzidos no EF Core 2.1 e considerados tipos de entidade sem chaves no EF Core 3.0, representam dados que podem ser lidos do banco de dados, mas não podem ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="9c451-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="9c451-141">Essa característica as torna uma excelente escolha para exibições de banco de dados na maioria dos cenários, portanto, planejamos automatizar a criação de tipos de entidade sem chaves ao realizar engenharia reversa de exibições de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c451-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="9c451-142">Entidades de recipiente de propriedade</span><span class="sxs-lookup"><span data-stu-id="9c451-142">Property bag entities</span></span>

<span data-ttu-id="9c451-143">[Acompanhamento de problema nº 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) e [nº 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="9c451-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="9c451-144">O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.</span><span class="sxs-lookup"><span data-stu-id="9c451-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="9c451-145">Este recurso trata da habilitação das entidades que armazenam dados em propriedades indexadas em vez de propriedades regulares e também da capacidade de usar as instâncias da mesma classe do .NET (potencialmente algo tão simples quanto um `Dictionary<string, object>`) para representar tipos de entidades diferentes no mesmo modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c451-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="9c451-146">Este recurso é uma etapa fundamental para proporcionar suporte a relacionamentos de muitos para muitos sem uma entidade de junção, ([problema nº 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)) que é um dos aprimoramentos mais solicitados no EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c451-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="9c451-147">EF 6.3 no .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c451-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="9c451-148">Acompanhamento de problema EF6#271</span><span class="sxs-lookup"><span data-stu-id="9c451-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="9c451-149">O trabalho nesse recurso foi iniciado, mas não está incluído na versão prévia atual.</span><span class="sxs-lookup"><span data-stu-id="9c451-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="9c451-150">Entendemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.</span><span class="sxs-lookup"><span data-stu-id="9c451-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="9c451-151">Por esse motivo, adaptaremos a próxima versão do EF 6 para execução no .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="9c451-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="9c451-152">Faremos isso para facilitar a portabilidade de aplicativos existentes com alterações mínimas.</span><span class="sxs-lookup"><span data-stu-id="9c451-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="9c451-153">Haverá algumas limitações.</span><span class="sxs-lookup"><span data-stu-id="9c451-153">There are going to be some limitations.</span></span> <span data-ttu-id="9c451-154">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9c451-154">For example:</span></span>
- <span data-ttu-id="9c451-155">Exigirá que novos provedores trabalham com outros bancos de dados, além de suporte do SQL Server incluído no .NET Core</span><span class="sxs-lookup"><span data-stu-id="9c451-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="9c451-156">Suporte espacial com o SQL Server não será habilitado</span><span class="sxs-lookup"><span data-stu-id="9c451-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="9c451-157">Observe também que não há novos recursos planejados para o EF 6 neste momento.</span><span class="sxs-lookup"><span data-stu-id="9c451-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
