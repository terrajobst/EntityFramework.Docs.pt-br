---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286488"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="38a90-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="38a90-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="38a90-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="38a90-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="38a90-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="38a90-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="38a90-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="38a90-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="38a90-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="38a90-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="38a90-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="38a90-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="38a90-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="38a90-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="38a90-109">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-110">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-110">**Old behavior**</span></span>

<span data-ttu-id="38a90-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="38a90-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="38a90-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="38a90-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="38a90-113">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-113">**New behavior**</span></span>

<span data-ttu-id="38a90-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="38a90-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="38a90-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="38a90-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="38a90-116">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-116">**Why**</span></span>

<span data-ttu-id="38a90-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="38a90-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="38a90-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="38a90-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="38a90-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="38a90-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="38a90-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="38a90-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="38a90-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="38a90-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="38a90-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="38a90-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="38a90-123">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-123">**Mitigations**</span></span>

<span data-ttu-id="38a90-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="38a90-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="38a90-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38a90-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="38a90-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="38a90-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="38a90-127">Essa alteração foi introduzida no ASP.NET Core 3.0 versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="38a90-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="38a90-128">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-128">**Old behavior**</span></span>

<span data-ttu-id="38a90-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38a90-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="38a90-130">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-130">**New behavior**</span></span>

<span data-ttu-id="38a90-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="38a90-132">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-132">**Why**</span></span>

<span data-ttu-id="38a90-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="38a90-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="38a90-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="38a90-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="38a90-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="38a90-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="38a90-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="38a90-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="38a90-137">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-137">**Mitigations**</span></span>

<span data-ttu-id="38a90-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="38a90-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="38a90-139">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="38a90-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="38a90-140">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="38a90-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="38a90-141">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4 e na versão correspondente do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="38a90-142">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-142">**Old behavior**</span></span>

<span data-ttu-id="38a90-143">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="38a90-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="38a90-144">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-144">**New behavior**</span></span>

<span data-ttu-id="38a90-145">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="38a90-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="38a90-146">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-146">**Why**</span></span>

<span data-ttu-id="38a90-147">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="38a90-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="38a90-148">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-148">**Mitigations**</span></span>

<span data-ttu-id="38a90-149">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="38a90-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="38a90-150">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="38a90-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="38a90-151">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="38a90-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="38a90-152">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="38a90-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="38a90-153">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-154">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-154">**Old behavior**</span></span>

<span data-ttu-id="38a90-155">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="38a90-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="38a90-156">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-156">**New behavior**</span></span>

<span data-ttu-id="38a90-157">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="38a90-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="38a90-158">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="38a90-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="38a90-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="38a90-160">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="38a90-161">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="38a90-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="38a90-162">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-162">**Why**</span></span>

<span data-ttu-id="38a90-163">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="38a90-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="38a90-164">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="38a90-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="38a90-165">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-165">**Mitigations**</span></span>

<span data-ttu-id="38a90-166">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="38a90-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="38a90-167">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="38a90-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="38a90-168">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="38a90-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="38a90-169">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="38a90-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="38a90-170">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-170">**Old behavior**</span></span>

<span data-ttu-id="38a90-171">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="38a90-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="38a90-172">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-172">**New behavior**</span></span>

<span data-ttu-id="38a90-173">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="38a90-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="38a90-174">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="38a90-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="38a90-175">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-175">**Why**</span></span>

<span data-ttu-id="38a90-176">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="38a90-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="38a90-177">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-177">**Mitigations**</span></span>

<span data-ttu-id="38a90-178">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="38a90-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="38a90-179">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="38a90-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="38a90-180">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="38a90-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="38a90-181">Essa alteração será revertida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="38a90-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="38a90-182">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="38a90-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="38a90-183">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="38a90-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="38a90-184">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="38a90-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="38a90-185">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="38a90-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="38a90-186">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="38a90-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="38a90-187">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-187">**Old behavior**</span></span>

<span data-ttu-id="38a90-188">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="38a90-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="38a90-189">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="38a90-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="38a90-190">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-190">**New behavior**</span></span>

<span data-ttu-id="38a90-191">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="38a90-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="38a90-192">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-192">**Why**</span></span>

<span data-ttu-id="38a90-193">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="38a90-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="38a90-194">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-194">**Mitigations**</span></span>

<span data-ttu-id="38a90-195">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="38a90-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="38a90-196">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="38a90-196">This can be avoided by:</span></span>
* <span data-ttu-id="38a90-197">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="38a90-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="38a90-198">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="38a90-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="38a90-199">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="38a90-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="38a90-200">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="38a90-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="38a90-201">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="38a90-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="38a90-202">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="38a90-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="38a90-203">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-204">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-204">**Old behavior**</span></span>

<span data-ttu-id="38a90-205">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="38a90-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="38a90-206">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-206">**New behavior**</span></span>

<span data-ttu-id="38a90-207">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="38a90-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="38a90-208">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="38a90-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="38a90-209">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="38a90-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="38a90-210">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-210">**Why**</span></span>

<span data-ttu-id="38a90-211">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="38a90-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="38a90-212">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-212">**Mitigations**</span></span>

<span data-ttu-id="38a90-213">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="38a90-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="38a90-214">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="38a90-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="38a90-215">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="38a90-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="38a90-216">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="38a90-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="38a90-217">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="38a90-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="38a90-218">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="38a90-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="38a90-219">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-220">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-220">**Old behavior**</span></span>

<span data-ttu-id="38a90-221">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="38a90-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="38a90-222">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-222">**New behavior**</span></span>

<span data-ttu-id="38a90-223">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="38a90-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="38a90-224">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="38a90-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="38a90-225">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-225">**Why**</span></span>

<span data-ttu-id="38a90-226">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="38a90-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="38a90-227">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-227">**Mitigations**</span></span>

<span data-ttu-id="38a90-228">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="38a90-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="38a90-229">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="38a90-230">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="38a90-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="38a90-231">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="38a90-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="38a90-232">Essa alteração foi introduzida no EF Core 3.0 versão prévia 5.</span><span class="sxs-lookup"><span data-stu-id="38a90-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="38a90-233">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-233">**Old behavior**</span></span>

<span data-ttu-id="38a90-234">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="38a90-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="38a90-235">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-235">**New behavior**</span></span>

<span data-ttu-id="38a90-236">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="38a90-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="38a90-237">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-237">**Why**</span></span>

<span data-ttu-id="38a90-238">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="38a90-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="38a90-239">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-239">**Mitigations**</span></span>

<span data-ttu-id="38a90-240">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="38a90-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="38a90-241">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="38a90-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="38a90-242">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="38a90-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="38a90-243">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-244">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-244">**Old behavior**</span></span>

<span data-ttu-id="38a90-245">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="38a90-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="38a90-246">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="38a90-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="38a90-247">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-247">**New behavior**</span></span>

<span data-ttu-id="38a90-248">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="38a90-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="38a90-249">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="38a90-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="38a90-250">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-250">**Why**</span></span>

<span data-ttu-id="38a90-251">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="38a90-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="38a90-252">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="38a90-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="38a90-253">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="38a90-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="38a90-254">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-254">**Mitigations**</span></span>

<span data-ttu-id="38a90-255">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="38a90-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="38a90-256">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="38a90-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="38a90-257">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="38a90-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="38a90-258">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="38a90-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="38a90-259">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="38a90-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="38a90-260">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="38a90-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="38a90-261">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="38a90-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="38a90-262">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-263">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-263">**Old behavior**</span></span>

<span data-ttu-id="38a90-264">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="38a90-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="38a90-265">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-265">**New behavior**</span></span>

<span data-ttu-id="38a90-266">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="38a90-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="38a90-267">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="38a90-268">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="38a90-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="38a90-269">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="38a90-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="38a90-270">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-270">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="38a90-271">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="38a90-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="38a90-272">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-272">**Why**</span></span>

<span data-ttu-id="38a90-273">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="38a90-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="38a90-274">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="38a90-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="38a90-275">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-275">**Mitigations**</span></span>

<span data-ttu-id="38a90-276">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="38a90-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="38a90-277">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="38a90-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="38a90-278">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="38a90-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="38a90-279">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-280">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-280">**Old behavior**</span></span>

<span data-ttu-id="38a90-281">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="38a90-281">Consider the following model:</span></span>
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
<span data-ttu-id="38a90-282">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="38a90-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="38a90-283">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-283">**New behavior**</span></span>

<span data-ttu-id="38a90-284">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="38a90-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="38a90-285">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="38a90-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="38a90-286">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-286">**Mitigations**</span></span>

<span data-ttu-id="38a90-287">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="38a90-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="38a90-288">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="38a90-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="38a90-289">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="38a90-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="38a90-290">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="38a90-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="38a90-291">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-292">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-292">**Old behavior**</span></span>

<span data-ttu-id="38a90-293">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="38a90-293">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="38a90-294">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="38a90-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="38a90-295">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-295">**New behavior**</span></span>

<span data-ttu-id="38a90-296">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="38a90-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="38a90-297">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="38a90-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="38a90-298">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-298">**Why**</span></span>

<span data-ttu-id="38a90-299">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="38a90-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="38a90-300">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-300">**Mitigations**</span></span>

<span data-ttu-id="38a90-301">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="38a90-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="38a90-302">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="38a90-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="38a90-303">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="38a90-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="38a90-304">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="38a90-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="38a90-305">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-306">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-306">**Old behavior**</span></span>

<span data-ttu-id="38a90-307">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="38a90-307">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="38a90-308">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="38a90-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="38a90-309">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-309">**New behavior**</span></span>

<span data-ttu-id="38a90-310">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="38a90-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="38a90-311">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-311">**Why**</span></span>

<span data-ttu-id="38a90-312">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="38a90-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="38a90-313">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-313">**Mitigations**</span></span>

<span data-ttu-id="38a90-314">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="38a90-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="38a90-315">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="38a90-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="38a90-316">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="38a90-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="38a90-317">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-318">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-318">**Old behavior**</span></span>

<span data-ttu-id="38a90-319">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="38a90-319">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="38a90-320">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="38a90-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="38a90-321">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="38a90-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="38a90-322">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-322">**New behavior**</span></span>

<span data-ttu-id="38a90-323">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="38a90-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="38a90-324">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="38a90-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="38a90-325">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-325">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="38a90-326">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-326">**Why**</span></span>

<span data-ttu-id="38a90-327">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="38a90-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="38a90-328">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-328">**Mitigations**</span></span>

<span data-ttu-id="38a90-329">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="38a90-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="38a90-330">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="38a90-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="38a90-331">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="38a90-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="38a90-332">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-333">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-333">**Old behavior**</span></span>

<span data-ttu-id="38a90-334">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="38a90-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="38a90-335">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-335">**New behavior**</span></span>

<span data-ttu-id="38a90-336">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="38a90-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="38a90-337">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-337">**Why**</span></span>

<span data-ttu-id="38a90-338">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="38a90-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="38a90-339">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="38a90-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="38a90-340">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-340">**Mitigations**</span></span>

<span data-ttu-id="38a90-341">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="38a90-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="38a90-342">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="38a90-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="38a90-343">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="38a90-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="38a90-344">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-345">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-345">**Old behavior**</span></span>

<span data-ttu-id="38a90-346">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="38a90-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="38a90-347">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-347">**New behavior**</span></span>

<span data-ttu-id="38a90-348">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="38a90-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="38a90-349">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="38a90-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="38a90-350">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-350">**Why**</span></span>

<span data-ttu-id="38a90-351">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="38a90-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="38a90-352">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-352">**Mitigations**</span></span>

<span data-ttu-id="38a90-353">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="38a90-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="38a90-354">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="38a90-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="38a90-355">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="38a90-355">Backing fields are used by default</span></span>

[<span data-ttu-id="38a90-356">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="38a90-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="38a90-357">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="38a90-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="38a90-358">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-358">**Old behavior**</span></span>

<span data-ttu-id="38a90-359">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="38a90-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="38a90-360">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="38a90-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="38a90-361">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-361">**New behavior**</span></span>

<span data-ttu-id="38a90-362">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="38a90-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="38a90-363">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="38a90-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="38a90-364">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-364">**Why**</span></span>

<span data-ttu-id="38a90-365">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="38a90-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="38a90-366">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-366">**Mitigations**</span></span>

<span data-ttu-id="38a90-367">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="38a90-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="38a90-368">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="38a90-369">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="38a90-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="38a90-370">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="38a90-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="38a90-371">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-372">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-372">**Old behavior**</span></span>

<span data-ttu-id="38a90-373">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="38a90-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="38a90-374">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="38a90-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="38a90-375">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-375">**New behavior**</span></span>

<span data-ttu-id="38a90-376">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="38a90-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="38a90-377">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-377">**Why**</span></span>

<span data-ttu-id="38a90-378">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="38a90-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="38a90-379">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-379">**Mitigations**</span></span>

<span data-ttu-id="38a90-380">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="38a90-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="38a90-381">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="38a90-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="38a90-382">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="38a90-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="38a90-383">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-384">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-384">**Old behavior**</span></span>

<span data-ttu-id="38a90-385">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo, usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="38a90-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="38a90-386">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-386">**New behavior**</span></span>

<span data-ttu-id="38a90-387">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="38a90-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="38a90-388">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-388">**Why**</span></span>

<span data-ttu-id="38a90-389">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="38a90-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="38a90-390">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-390">**Mitigations**</span></span>

<span data-ttu-id="38a90-391">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="38a90-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="38a90-392">Em uma versão prévia posterior do EF Core 3.0, planejamos habilitar novamente a configuração explícita de um nome de campo que seja diferente do nome da propriedade:</span><span class="sxs-lookup"><span data-stu-id="38a90-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="38a90-393">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="38a90-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="38a90-394">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="38a90-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="38a90-395">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-396">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-396">**Old behavior**</span></span>

<span data-ttu-id="38a90-397">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="38a90-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="38a90-398">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-398">**New behavior**</span></span>

<span data-ttu-id="38a90-399">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="38a90-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="38a90-400">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-400">**Why**</span></span>

<span data-ttu-id="38a90-401">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="38a90-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="38a90-402">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="38a90-403">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-403">**Mitigations**</span></span>

<span data-ttu-id="38a90-404">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="38a90-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="38a90-405">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="38a90-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="38a90-406">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="38a90-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="38a90-407">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-408">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-408">**Old behavior**</span></span>

<span data-ttu-id="38a90-409">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="38a90-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="38a90-410">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="38a90-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="38a90-411">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-411">**New behavior**</span></span>

<span data-ttu-id="38a90-412">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="38a90-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="38a90-413">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="38a90-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="38a90-414">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="38a90-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="38a90-415">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="38a90-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="38a90-416">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-416">**Why**</span></span>

<span data-ttu-id="38a90-417">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="38a90-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="38a90-418">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-418">**Mitigations**</span></span>

<span data-ttu-id="38a90-419">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="38a90-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="38a90-420">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="38a90-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="38a90-421">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="38a90-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="38a90-422">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-423">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-423">**Old behavior**</span></span>

<span data-ttu-id="38a90-424">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="38a90-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="38a90-425">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="38a90-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="38a90-426">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-426">**New behavior**</span></span>

<span data-ttu-id="38a90-427">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="38a90-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="38a90-428">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-428">**Why**</span></span>

<span data-ttu-id="38a90-429">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="38a90-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="38a90-430">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-430">**Mitigations**</span></span>

<span data-ttu-id="38a90-431">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="38a90-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="38a90-432">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="38a90-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="38a90-433">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="38a90-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="38a90-434">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="38a90-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="38a90-435">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="38a90-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="38a90-436">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-437">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-437">**Old behavior**</span></span>

<span data-ttu-id="38a90-438">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="38a90-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="38a90-439">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-439">**New behavior**</span></span>

<span data-ttu-id="38a90-440">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="38a90-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="38a90-441">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-441">**Why**</span></span>

<span data-ttu-id="38a90-442">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="38a90-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="38a90-443">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-443">**Mitigations**</span></span>

<span data-ttu-id="38a90-444">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="38a90-445">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="38a90-445">This isn't common.</span></span>
<span data-ttu-id="38a90-446">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="38a90-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="38a90-447">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="38a90-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="38a90-448">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="38a90-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="38a90-449">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="38a90-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="38a90-450">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-451">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-451">**Old behavior**</span></span>

<span data-ttu-id="38a90-452">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="38a90-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="38a90-453">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="38a90-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="38a90-454">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="38a90-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="38a90-455">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-455">**New behavior**</span></span>

<span data-ttu-id="38a90-456">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="38a90-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="38a90-457">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="38a90-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="38a90-458">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="38a90-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="38a90-459">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="38a90-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="38a90-460">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-460">**Why**</span></span>

<span data-ttu-id="38a90-461">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="38a90-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="38a90-462">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-462">**Mitigations**</span></span>

<span data-ttu-id="38a90-463">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="38a90-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="38a90-464">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="38a90-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="38a90-465">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="38a90-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="38a90-466">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-467">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-467">**Old behavior**</span></span>

<span data-ttu-id="38a90-468">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="38a90-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="38a90-469">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-469">**New behavior**</span></span>

<span data-ttu-id="38a90-470">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="38a90-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="38a90-471">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-471">**Why**</span></span>

<span data-ttu-id="38a90-472">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="38a90-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="38a90-473">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-473">**Mitigations**</span></span>

<span data-ttu-id="38a90-474">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="38a90-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="38a90-475">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="38a90-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="38a90-476">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="38a90-477">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="38a90-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="38a90-478">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="38a90-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="38a90-479">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-480">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-480">**Old behavior**</span></span>

<span data-ttu-id="38a90-481">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="38a90-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="38a90-482">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="38a90-483">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="38a90-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="38a90-484">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="38a90-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="38a90-485">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-485">**New behavior**</span></span>

<span data-ttu-id="38a90-486">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="38a90-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="38a90-487">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-487">**Why**</span></span>

<span data-ttu-id="38a90-488">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="38a90-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="38a90-489">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-489">**Mitigations**</span></span>

<span data-ttu-id="38a90-490">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="38a90-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="38a90-491">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="38a90-491">This is not common.</span></span>
<span data-ttu-id="38a90-492">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="38a90-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="38a90-493">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="38a90-494">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="38a90-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="38a90-495">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="38a90-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="38a90-496">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-497">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-497">**Old behavior**</span></span>

<span data-ttu-id="38a90-498">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="38a90-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="38a90-499">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="38a90-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="38a90-500">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-500">**New behavior**</span></span>

<span data-ttu-id="38a90-501">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="38a90-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="38a90-502">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-502">**Why**</span></span>

<span data-ttu-id="38a90-503">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="38a90-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="38a90-504">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-504">**Mitigations**</span></span>

<span data-ttu-id="38a90-505">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="38a90-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="38a90-506">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="38a90-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="38a90-507">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="38a90-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="38a90-508">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="38a90-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="38a90-509">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="38a90-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="38a90-510">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="38a90-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="38a90-511">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-511">**Old behavior**</span></span>

<span data-ttu-id="38a90-512">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="38a90-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="38a90-513">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-513">**New behavior**</span></span>

<span data-ttu-id="38a90-514">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="38a90-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="38a90-515">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-515">**Why**</span></span>

<span data-ttu-id="38a90-516">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="38a90-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="38a90-517">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-517">**Mitigations**</span></span>

<span data-ttu-id="38a90-518">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="38a90-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="38a90-519">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="38a90-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="38a90-520">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="38a90-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="38a90-521">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="38a90-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="38a90-522">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-523">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-523">**Old behavior**</span></span>

<span data-ttu-id="38a90-524">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="38a90-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="38a90-525">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-525">**New behavior**</span></span>

<span data-ttu-id="38a90-526">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="38a90-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="38a90-527">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-527">**Why**</span></span>

<span data-ttu-id="38a90-528">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="38a90-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="38a90-529">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="38a90-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="38a90-530">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-530">**Mitigations**</span></span>

<span data-ttu-id="38a90-531">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="38a90-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="38a90-532">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="38a90-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="38a90-533">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="38a90-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="38a90-534">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-535">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-535">**Old behavior**</span></span>

<span data-ttu-id="38a90-536">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="38a90-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="38a90-537">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-537">**New behavior**</span></span>

<span data-ttu-id="38a90-538">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="38a90-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="38a90-539">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="38a90-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="38a90-540">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-540">**Why**</span></span>

<span data-ttu-id="38a90-541">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="38a90-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="38a90-542">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-542">**Mitigations**</span></span>

<span data-ttu-id="38a90-543">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="38a90-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="38a90-544">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="38a90-544">Metadata API changes</span></span>

[<span data-ttu-id="38a90-545">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="38a90-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="38a90-546">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-547">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-547">**New behavior**</span></span>

<span data-ttu-id="38a90-548">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="38a90-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="38a90-549">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-549">**Why**</span></span>

<span data-ttu-id="38a90-550">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="38a90-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="38a90-551">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-551">**Mitigations**</span></span>

<span data-ttu-id="38a90-552">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="38a90-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="38a90-553">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="38a90-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="38a90-554">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="38a90-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="38a90-555">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="38a90-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="38a90-556">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-556">**New behavior**</span></span>

<span data-ttu-id="38a90-557">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="38a90-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="38a90-558">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-558">**Why**</span></span>

<span data-ttu-id="38a90-559">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="38a90-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="38a90-560">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-560">**Mitigations**</span></span>

<span data-ttu-id="38a90-561">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="38a90-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="38a90-562">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="38a90-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="38a90-563">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="38a90-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="38a90-564">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="38a90-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="38a90-565">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-565">**Old behavior**</span></span>

<span data-ttu-id="38a90-566">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="38a90-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="38a90-567">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-567">**New behavior**</span></span>

<span data-ttu-id="38a90-568">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="38a90-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="38a90-569">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-569">**Why**</span></span>

<span data-ttu-id="38a90-570">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="38a90-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="38a90-571">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-571">**Mitigations**</span></span>

<span data-ttu-id="38a90-572">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="38a90-573">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="38a90-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="38a90-574">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="38a90-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="38a90-575">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-575">**Old behavior**</span></span>

<span data-ttu-id="38a90-576">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="38a90-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="38a90-577">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-577">**New behavior**</span></span>

<span data-ttu-id="38a90-578">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="38a90-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="38a90-579">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-579">**Why**</span></span>

<span data-ttu-id="38a90-580">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="38a90-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="38a90-581">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-581">**Mitigations**</span></span>

<span data-ttu-id="38a90-582">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="38a90-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="38a90-583">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="38a90-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="38a90-584">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="38a90-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="38a90-585">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-586">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-586">**Old behavior**</span></span>

<span data-ttu-id="38a90-587">Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="38a90-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="38a90-588">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-588">**New behavior**</span></span>

<span data-ttu-id="38a90-589">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="38a90-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="38a90-590">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-590">**Why**</span></span>

<span data-ttu-id="38a90-591">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="38a90-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="38a90-592">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="38a90-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="38a90-593">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-593">**Mitigations**</span></span>

<span data-ttu-id="38a90-594">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="38a90-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="38a90-595">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="38a90-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="38a90-596">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="38a90-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="38a90-597">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="38a90-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="38a90-598">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="38a90-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="38a90-599">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-600">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-600">**Old behavior**</span></span>

<span data-ttu-id="38a90-601">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="38a90-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="38a90-602">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="38a90-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="38a90-603">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-603">**New behavior**</span></span>

<span data-ttu-id="38a90-604">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="38a90-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="38a90-605">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-605">**Why**</span></span>

<span data-ttu-id="38a90-606">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="38a90-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="38a90-607">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-607">**Mitigations**</span></span>

<span data-ttu-id="38a90-608">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="38a90-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="38a90-609">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="38a90-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="38a90-610">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="38a90-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="38a90-611">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="38a90-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="38a90-612">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="38a90-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="38a90-613">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-614">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-614">**Old behavior**</span></span>

<span data-ttu-id="38a90-615">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="38a90-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="38a90-616">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-616">**New behavior**</span></span>

<span data-ttu-id="38a90-617">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="38a90-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="38a90-618">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-618">**Why**</span></span>

<span data-ttu-id="38a90-619">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="38a90-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="38a90-620">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="38a90-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="38a90-621">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-621">**Mitigations**</span></span>

<span data-ttu-id="38a90-622">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="38a90-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="38a90-623">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="38a90-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="38a90-624">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="38a90-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="38a90-625">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="38a90-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="38a90-626">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="38a90-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="38a90-627">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="38a90-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="38a90-628">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="38a90-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="38a90-629">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-629">**Old behavior**</span></span>

<span data-ttu-id="38a90-630">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="38a90-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="38a90-631">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-631">**New behavior**</span></span>

<span data-ttu-id="38a90-632">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38a90-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="38a90-633">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-633">**Why**</span></span>

<span data-ttu-id="38a90-634">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="38a90-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="38a90-635">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-635">**Mitigations**</span></span>

<span data-ttu-id="38a90-636">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="38a90-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="38a90-637">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="38a90-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="38a90-638">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="38a90-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="38a90-639">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="38a90-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="38a90-640">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="38a90-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="38a90-641">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="38a90-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="38a90-642">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-642">**Old behavior**</span></span>

<span data-ttu-id="38a90-643">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="38a90-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="38a90-644">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-644">**New behavior**</span></span>

<span data-ttu-id="38a90-645">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="38a90-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="38a90-646">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-646">**Why**</span></span>

<span data-ttu-id="38a90-647">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="38a90-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="38a90-648">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="38a90-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="38a90-649">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-649">**Mitigations**</span></span>

<span data-ttu-id="38a90-650">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="38a90-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="38a90-651">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="38a90-652">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="38a90-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="38a90-653">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="38a90-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="38a90-654">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-655">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="38a90-655">**Change**</span></span>

<span data-ttu-id="38a90-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="38a90-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="38a90-657">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-657">**Why**</span></span>

<span data-ttu-id="38a90-658">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="38a90-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="38a90-659">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-659">**Mitigations**</span></span>

<span data-ttu-id="38a90-660">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="38a90-660">Use the new name.</span></span> <span data-ttu-id="38a90-661">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="38a90-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="38a90-662">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="38a90-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="38a90-663">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="38a90-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="38a90-664">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-665">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-665">**Old behavior**</span></span>

<span data-ttu-id="38a90-666">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="38a90-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="38a90-667">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="38a90-668">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-668">**New behavior**</span></span>

<span data-ttu-id="38a90-669">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="38a90-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="38a90-670">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="38a90-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="38a90-671">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-671">**Why**</span></span>

<span data-ttu-id="38a90-672">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="38a90-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="38a90-673">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-673">**Mitigations**</span></span>

<span data-ttu-id="38a90-674">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="38a90-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="38a90-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="38a90-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="38a90-676">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="38a90-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="38a90-677">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="38a90-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="38a90-678">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-678">**Old behavior**</span></span>

<span data-ttu-id="38a90-679">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="38a90-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="38a90-680">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-680">**New behavior**</span></span>

<span data-ttu-id="38a90-681">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="38a90-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="38a90-682">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-682">**Why**</span></span>

<span data-ttu-id="38a90-683">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="38a90-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="38a90-684">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="38a90-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="38a90-685">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-685">**Mitigations**</span></span>

<span data-ttu-id="38a90-686">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="38a90-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="38a90-687">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="38a90-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="38a90-688">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="38a90-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="38a90-689">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="38a90-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="38a90-690">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-690">**Old behavior**</span></span>

<span data-ttu-id="38a90-691">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="38a90-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="38a90-692">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-692">**New behavior**</span></span>

<span data-ttu-id="38a90-693">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="38a90-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="38a90-694">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="38a90-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="38a90-695">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-695">**Why**</span></span>

<span data-ttu-id="38a90-696">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="38a90-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="38a90-697">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="38a90-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="38a90-698">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="38a90-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="38a90-699">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-699">**Mitigations**</span></span>

<span data-ttu-id="38a90-700">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="38a90-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="38a90-701">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="38a90-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="38a90-702">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="38a90-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="38a90-703">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="38a90-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="38a90-704">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="38a90-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="38a90-705">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-705">**Old behavior**</span></span>

<span data-ttu-id="38a90-706">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="38a90-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="38a90-707">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-707">**New behavior**</span></span>

<span data-ttu-id="38a90-708">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="38a90-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="38a90-709">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-709">**Why**</span></span>

<span data-ttu-id="38a90-710">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="38a90-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="38a90-711">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="38a90-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="38a90-712">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-712">**Mitigations**</span></span>

<span data-ttu-id="38a90-713">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="38a90-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="38a90-714">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="38a90-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="38a90-715">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="38a90-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="38a90-716">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="38a90-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="38a90-717">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="38a90-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="38a90-718">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="38a90-718">**Old behavior**</span></span>

<span data-ttu-id="38a90-719">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="38a90-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="38a90-720">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="38a90-720">**New behavior**</span></span>

<span data-ttu-id="38a90-721">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="38a90-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="38a90-722">**Por que**</span><span class="sxs-lookup"><span data-stu-id="38a90-722">**Why**</span></span>

<span data-ttu-id="38a90-723">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="38a90-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="38a90-724">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="38a90-724">**Mitigations**</span></span>

<span data-ttu-id="38a90-725">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="38a90-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="38a90-726">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="38a90-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
