---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394825"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="8ccb3-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="8ccb3-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ccb3-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="8ccb3-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="8ccb3-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="8ccb3-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="8ccb3-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="8ccb3-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="8ccb3-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="8ccb3-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="8ccb3-109">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-110">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-110">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="8ccb3-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="8ccb3-113">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-113">**New behavior**</span></span>

<span data-ttu-id="8ccb3-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="8ccb3-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="8ccb3-116">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-116">**Why**</span></span>

<span data-ttu-id="8ccb3-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="8ccb3-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="8ccb3-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="8ccb3-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="8ccb3-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="8ccb3-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="8ccb3-123">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-123">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="8ccb3-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ccb3-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="8ccb3-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="8ccb3-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="8ccb3-127">Essa alteração foi introduzida no ASP.NET Core 3.0 versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="8ccb3-128">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-128">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="8ccb3-130">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-130">**New behavior**</span></span>

<span data-ttu-id="8ccb3-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="8ccb3-132">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-132">**Why**</span></span>

<span data-ttu-id="8ccb3-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="8ccb3-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="8ccb3-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="8ccb3-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="8ccb3-137">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-137">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="8ccb3-139">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ccb3-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="8ccb3-140">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="8ccb3-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="8ccb3-141">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4 e na versão correspondente do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="8ccb3-142">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-142">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-143">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="8ccb3-144">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-144">**New behavior**</span></span>

<span data-ttu-id="8ccb3-145">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="8ccb3-146">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-146">**Why**</span></span>

<span data-ttu-id="8ccb3-147">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="8ccb3-148">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-148">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-149">Para ser capaz de gerenciar as migrações ou fazer scaffold de um `DbContext`, instale `dotnet-ef` usando o comando `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="8ccb3-150">Por exemplo, para instalá-lo como uma ferramenta global, você pode digitar este comando:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="8ccb3-151">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="8ccb3-152">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="8ccb3-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="8ccb3-153">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="8ccb3-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="8ccb3-154">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-155">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-155">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-156">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="8ccb3-157">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-157">**New behavior**</span></span>

<span data-ttu-id="8ccb3-158">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="8ccb3-159">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="8ccb3-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="8ccb3-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="8ccb3-162">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="8ccb3-163">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-163">**Why**</span></span>

<span data-ttu-id="8ccb3-164">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="8ccb3-165">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="8ccb3-166">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-166">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-167">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="8ccb3-168">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="8ccb3-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="8ccb3-169">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="8ccb3-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="8ccb3-170">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="8ccb3-171">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-171">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-172">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="8ccb3-173">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-173">**New behavior**</span></span>

<span data-ttu-id="8ccb3-174">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="8ccb3-175">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="8ccb3-176">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-176">**Why**</span></span>

<span data-ttu-id="8ccb3-177">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="8ccb3-178">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-178">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-179">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="8ccb3-180">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="8ccb3-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="8ccb3-181">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="8ccb3-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="8ccb3-182">Essa alteração será revertida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="8ccb3-183">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="8ccb3-184">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="8ccb3-185">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="8ccb3-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="8ccb3-186">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="8ccb3-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="8ccb3-187">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8ccb3-188">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-188">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-189">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="8ccb3-190">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="8ccb3-191">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-191">**New behavior**</span></span>

<span data-ttu-id="8ccb3-192">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="8ccb3-193">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-193">**Why**</span></span>

<span data-ttu-id="8ccb3-194">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="8ccb3-195">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-195">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-196">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="8ccb3-197">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-197">This can be avoided by:</span></span>
* <span data-ttu-id="8ccb3-198">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="8ccb3-199">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="8ccb3-200">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="8ccb3-201">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="8ccb3-202">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="8ccb3-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="8ccb3-203">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="8ccb3-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="8ccb3-204">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-205">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-205">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-206">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="8ccb3-207">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-207">**New behavior**</span></span>

<span data-ttu-id="8ccb3-208">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="8ccb3-209">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="8ccb3-210">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="8ccb3-211">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-211">**Why**</span></span>

<span data-ttu-id="8ccb3-212">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="8ccb3-213">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-213">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-214">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="8ccb3-215">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="8ccb3-216">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="8ccb3-217">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="8ccb3-218">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="8ccb3-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="8ccb3-219">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="8ccb3-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="8ccb3-220">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-221">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-221">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-222">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="8ccb3-223">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-223">**New behavior**</span></span>

<span data-ttu-id="8ccb3-224">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="8ccb3-225">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="8ccb3-226">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-226">**Why**</span></span>

<span data-ttu-id="8ccb3-227">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="8ccb3-228">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-228">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-229">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="8ccb3-230">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="8ccb3-231">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="8ccb3-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="8ccb3-232">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="8ccb3-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="8ccb3-233">Essa alteração foi introduzida no EF Core 3.0 versão prévia 5.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="8ccb3-234">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-234">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-235">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="8ccb3-236">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-236">**New behavior**</span></span>

<span data-ttu-id="8ccb3-237">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="8ccb3-238">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-238">**Why**</span></span>

<span data-ttu-id="8ccb3-239">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="8ccb3-240">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-240">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-241">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="8ccb3-242">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="8ccb3-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="8ccb3-243">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="8ccb3-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="8ccb3-244">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-245">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-245">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-246">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="8ccb3-247">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="8ccb3-248">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-248">**New behavior**</span></span>

<span data-ttu-id="8ccb3-249">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="8ccb3-250">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="8ccb3-251">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-251">**Why**</span></span>

<span data-ttu-id="8ccb3-252">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="8ccb3-253">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="8ccb3-254">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="8ccb3-255">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-255">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-256">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="8ccb3-257">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="8ccb3-258">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="8ccb3-259">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="8ccb3-260">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="8ccb3-261">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="8ccb3-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="8ccb3-262">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="8ccb3-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="8ccb3-263">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-264">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-264">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-265">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="8ccb3-266">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-266">**New behavior**</span></span>

<span data-ttu-id="8ccb3-267">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="8ccb3-268">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="8ccb3-269">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="8ccb3-270">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="8ccb3-271">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-271">For example:</span></span>

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

<span data-ttu-id="8ccb3-272">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="8ccb3-273">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-273">**Why**</span></span>

<span data-ttu-id="8ccb3-274">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="8ccb3-275">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="8ccb3-276">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-276">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-277">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="8ccb3-278">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="8ccb3-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="8ccb3-279">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="8ccb3-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="8ccb3-280">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-281">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-281">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-282">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-282">Consider the following model:</span></span>
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
<span data-ttu-id="8ccb3-283">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="8ccb3-284">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-284">**New behavior**</span></span>

<span data-ttu-id="8ccb3-285">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="8ccb3-286">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="8ccb3-287">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-287">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-288">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="8ccb3-289">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="8ccb3-290">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="8ccb3-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="8ccb3-291">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="8ccb3-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="8ccb3-292">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-293">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-293">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-294">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-294">Consider the following model:</span></span>
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
<span data-ttu-id="8ccb3-295">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="8ccb3-296">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-296">**New behavior**</span></span>

<span data-ttu-id="8ccb3-297">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="8ccb3-298">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="8ccb3-299">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-299">**Why**</span></span>

<span data-ttu-id="8ccb3-300">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="8ccb3-301">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-301">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-302">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="8ccb3-303">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="8ccb3-304">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="8ccb3-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="8ccb3-305">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="8ccb3-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="8ccb3-306">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-307">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-307">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-308">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-308">Consider the following model:</span></span>
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

<span data-ttu-id="8ccb3-309">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="8ccb3-310">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-310">**New behavior**</span></span>

<span data-ttu-id="8ccb3-311">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="8ccb3-312">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-312">**Why**</span></span>

<span data-ttu-id="8ccb3-313">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="8ccb3-314">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-314">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-315">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="8ccb3-316">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="8ccb3-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="8ccb3-317">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="8ccb3-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="8ccb3-318">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-319">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-319">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-320">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-320">Consider the following model:</span></span>
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
<span data-ttu-id="8ccb3-321">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="8ccb3-322">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="8ccb3-323">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-323">**New behavior**</span></span>

<span data-ttu-id="8ccb3-324">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="8ccb3-325">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="8ccb3-326">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-326">For example:</span></span>

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

<span data-ttu-id="8ccb3-327">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-327">**Why**</span></span>

<span data-ttu-id="8ccb3-328">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="8ccb3-329">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-329">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-330">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="8ccb3-331">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="8ccb3-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="8ccb3-332">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="8ccb3-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="8ccb3-333">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-334">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-334">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-335">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="8ccb3-336">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-336">**New behavior**</span></span>

<span data-ttu-id="8ccb3-337">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="8ccb3-338">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-338">**Why**</span></span>

<span data-ttu-id="8ccb3-339">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="8ccb3-340">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="8ccb3-341">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-341">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-342">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="8ccb3-343">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="8ccb3-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="8ccb3-344">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="8ccb3-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="8ccb3-345">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-346">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-346">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-347">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="8ccb3-348">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-348">**New behavior**</span></span>

<span data-ttu-id="8ccb3-349">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="8ccb3-350">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="8ccb3-351">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-351">**Why**</span></span>

<span data-ttu-id="8ccb3-352">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="8ccb3-353">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-353">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-354">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="8ccb3-355">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="8ccb3-356">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="8ccb3-356">Backing fields are used by default</span></span>

[<span data-ttu-id="8ccb3-357">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="8ccb3-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="8ccb3-358">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8ccb3-359">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-359">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-360">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="8ccb3-361">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="8ccb3-362">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-362">**New behavior**</span></span>

<span data-ttu-id="8ccb3-363">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="8ccb3-364">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="8ccb3-365">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-365">**Why**</span></span>

<span data-ttu-id="8ccb3-366">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="8ccb3-367">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-367">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-368">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="8ccb3-369">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="8ccb3-370">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="8ccb3-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="8ccb3-371">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="8ccb3-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="8ccb3-372">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-373">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-373">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-374">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="8ccb3-375">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="8ccb3-376">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-376">**New behavior**</span></span>

<span data-ttu-id="8ccb3-377">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="8ccb3-378">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-378">**Why**</span></span>

<span data-ttu-id="8ccb3-379">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="8ccb3-380">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-380">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-381">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="8ccb3-382">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="8ccb3-383">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="8ccb3-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="8ccb3-384">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-385">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-385">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-386">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo, usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="8ccb3-387">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-387">**New behavior**</span></span>

<span data-ttu-id="8ccb3-388">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="8ccb3-389">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-389">**Why**</span></span>

<span data-ttu-id="8ccb3-390">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="8ccb3-391">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-391">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-392">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="8ccb3-393">Em uma versão prévia posterior do EF Core 3.0, planejamos habilitar novamente a configuração explícita de um nome de campo que seja diferente do nome da propriedade:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="8ccb3-394">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="8ccb3-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="8ccb3-395">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="8ccb3-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="8ccb3-396">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-397">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-397">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-398">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="8ccb3-399">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-399">**New behavior**</span></span>

<span data-ttu-id="8ccb3-400">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="8ccb3-401">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-401">**Why**</span></span>

<span data-ttu-id="8ccb3-402">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="8ccb3-403">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="8ccb3-404">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-404">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-405">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="8ccb3-406">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="8ccb3-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="8ccb3-407">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="8ccb3-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="8ccb3-408">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-409">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-409">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-410">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="8ccb3-411">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="8ccb3-412">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-412">**New behavior**</span></span>

<span data-ttu-id="8ccb3-413">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="8ccb3-414">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="8ccb3-415">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="8ccb3-416">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="8ccb3-417">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-417">**Why**</span></span>

<span data-ttu-id="8ccb3-418">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="8ccb3-419">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-419">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-420">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="8ccb3-421">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="8ccb3-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="8ccb3-422">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="8ccb3-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="8ccb3-423">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-424">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-424">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-425">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="8ccb3-426">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="8ccb3-427">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-427">**New behavior**</span></span>

<span data-ttu-id="8ccb3-428">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="8ccb3-429">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-429">**Why**</span></span>

<span data-ttu-id="8ccb3-430">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="8ccb3-431">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-431">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-432">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="8ccb3-433">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="8ccb3-434">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="8ccb3-435">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="8ccb3-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="8ccb3-436">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="8ccb3-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="8ccb3-437">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-438">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-438">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-439">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="8ccb3-440">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-440">**New behavior**</span></span>

<span data-ttu-id="8ccb3-441">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="8ccb3-442">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-442">**Why**</span></span>

<span data-ttu-id="8ccb3-443">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="8ccb3-444">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-444">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-445">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="8ccb3-446">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-446">This isn't common.</span></span>
<span data-ttu-id="8ccb3-447">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="8ccb3-448">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="8ccb3-449">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="8ccb3-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="8ccb3-450">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="8ccb3-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="8ccb3-451">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-452">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-452">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-453">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="8ccb3-454">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="8ccb3-455">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="8ccb3-456">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-456">**New behavior**</span></span>

<span data-ttu-id="8ccb3-457">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="8ccb3-458">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="8ccb3-459">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="8ccb3-460">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="8ccb3-461">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-461">**Why**</span></span>

<span data-ttu-id="8ccb3-462">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="8ccb3-463">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-463">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-464">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="8ccb3-465">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="8ccb3-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="8ccb3-466">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="8ccb3-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="8ccb3-467">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-468">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-468">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-469">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="8ccb3-470">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-470">**New behavior**</span></span>

<span data-ttu-id="8ccb3-471">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="8ccb3-472">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-472">**Why**</span></span>

<span data-ttu-id="8ccb3-473">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="8ccb3-474">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-474">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-475">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="8ccb3-476">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="8ccb3-477">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="8ccb3-478">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="8ccb3-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="8ccb3-479">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="8ccb3-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="8ccb3-480">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-481">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-481">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-482">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="8ccb3-483">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="8ccb3-484">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="8ccb3-485">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="8ccb3-486">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-486">**New behavior**</span></span>

<span data-ttu-id="8ccb3-487">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="8ccb3-488">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-488">**Why**</span></span>

<span data-ttu-id="8ccb3-489">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="8ccb3-490">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-490">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-491">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="8ccb3-492">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-492">This is not common.</span></span>
<span data-ttu-id="8ccb3-493">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="8ccb3-494">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="8ccb3-495">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="8ccb3-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="8ccb3-496">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="8ccb3-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="8ccb3-497">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-498">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-498">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-499">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="8ccb3-500">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="8ccb3-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="8ccb3-501">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-501">**New behavior**</span></span>

<span data-ttu-id="8ccb3-502">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="8ccb3-503">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-503">**Why**</span></span>

<span data-ttu-id="8ccb3-504">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="8ccb3-505">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-505">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-506">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="8ccb3-507">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="8ccb3-508">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="8ccb3-509">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="8ccb3-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="8ccb3-510">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="8ccb3-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="8ccb3-511">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="8ccb3-512">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-512">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-513">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8ccb3-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="8ccb3-514">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-514">**New behavior**</span></span>

<span data-ttu-id="8ccb3-515">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="8ccb3-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="8ccb3-516">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-516">**Why**</span></span>

<span data-ttu-id="8ccb3-517">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="8ccb3-518">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-518">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-519">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="8ccb3-520">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="8ccb3-521">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="8ccb3-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="8ccb3-522">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="8ccb3-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="8ccb3-523">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-524">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-524">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-525">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="8ccb3-526">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-526">**New behavior**</span></span>

<span data-ttu-id="8ccb3-527">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="8ccb3-528">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-528">**Why**</span></span>

<span data-ttu-id="8ccb3-529">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="8ccb3-530">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="8ccb3-531">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-531">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-532">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="8ccb3-533">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="8ccb3-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="8ccb3-534">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="8ccb3-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="8ccb3-535">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-536">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-536">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-537">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="8ccb3-538">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-538">**New behavior**</span></span>

<span data-ttu-id="8ccb3-539">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="8ccb3-540">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="8ccb3-541">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-541">**Why**</span></span>

<span data-ttu-id="8ccb3-542">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="8ccb3-543">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-543">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-544">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="8ccb3-545">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="8ccb3-545">Metadata API changes</span></span>

[<span data-ttu-id="8ccb3-546">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="8ccb3-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="8ccb3-547">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-548">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-548">**New behavior**</span></span>

<span data-ttu-id="8ccb3-549">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="8ccb3-550">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-550">**Why**</span></span>

<span data-ttu-id="8ccb3-551">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="8ccb3-552">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-552">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-553">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="8ccb3-554">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="8ccb3-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="8ccb3-555">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="8ccb3-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="8ccb3-556">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="8ccb3-557">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-557">**New behavior**</span></span>

<span data-ttu-id="8ccb3-558">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="8ccb3-559">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-559">**Why**</span></span>

<span data-ttu-id="8ccb3-560">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="8ccb3-561">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-561">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-562">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="8ccb3-563">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="8ccb3-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="8ccb3-564">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="8ccb3-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="8ccb3-565">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="8ccb3-566">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-566">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-567">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8ccb3-568">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-568">**New behavior**</span></span>

<span data-ttu-id="8ccb3-569">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="8ccb3-570">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-570">**Why**</span></span>

<span data-ttu-id="8ccb3-571">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="8ccb3-572">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-572">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-573">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="8ccb3-574">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="8ccb3-575">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="8ccb3-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="8ccb3-576">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-576">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-577">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="8ccb3-578">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-578">**New behavior**</span></span>

<span data-ttu-id="8ccb3-579">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="8ccb3-580">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-580">**Why**</span></span>

<span data-ttu-id="8ccb3-581">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="8ccb3-582">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-582">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-583">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8ccb3-584">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="8ccb3-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8ccb3-585">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="8ccb3-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="8ccb3-586">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-587">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-587">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-588">Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="8ccb3-589">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-589">**New behavior**</span></span>

<span data-ttu-id="8ccb3-590">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="8ccb3-591">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-591">**Why**</span></span>

<span data-ttu-id="8ccb3-592">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="8ccb3-593">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="8ccb3-594">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-594">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-595">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="8ccb3-596">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="8ccb3-597">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="8ccb3-598">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="8ccb3-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="8ccb3-599">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="8ccb3-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="8ccb3-600">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-601">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-601">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-602">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="8ccb3-603">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="8ccb3-604">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-604">**New behavior**</span></span>

<span data-ttu-id="8ccb3-605">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="8ccb3-606">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-606">**Why**</span></span>

<span data-ttu-id="8ccb3-607">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="8ccb3-608">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-608">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-609">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="8ccb3-610">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="8ccb3-611">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="8ccb3-612">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="8ccb3-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="8ccb3-613">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="8ccb3-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="8ccb3-614">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-615">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-615">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-616">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="8ccb3-617">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-617">**New behavior**</span></span>

<span data-ttu-id="8ccb3-618">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="8ccb3-619">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-619">**Why**</span></span>

<span data-ttu-id="8ccb3-620">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="8ccb3-621">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="8ccb3-622">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-622">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-623">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="8ccb3-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="8ccb3-624">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="8ccb3-625">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="8ccb3-626">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="8ccb3-627">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="8ccb3-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="8ccb3-628">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="8ccb3-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="8ccb3-629">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="8ccb3-630">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-630">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-631">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="8ccb3-632">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-632">**New behavior**</span></span>

<span data-ttu-id="8ccb3-633">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="8ccb3-634">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-634">**Why**</span></span>

<span data-ttu-id="8ccb3-635">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="8ccb3-636">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="8ccb3-637">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-637">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-638">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="8ccb3-639">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="8ccb3-640">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="8ccb3-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="8ccb3-641">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="8ccb3-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="8ccb3-642">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-643">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-643">**Change**</span></span>

<span data-ttu-id="8ccb3-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="8ccb3-645">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-645">**Why**</span></span>

<span data-ttu-id="8ccb3-646">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="8ccb3-647">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-647">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-648">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-648">Use the new name.</span></span> <span data-ttu-id="8ccb3-649">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="8ccb3-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="8ccb3-650">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="8ccb3-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="8ccb3-651">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="8ccb3-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="8ccb3-652">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-653">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-653">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-654">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="8ccb3-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="8ccb3-655">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="8ccb3-656">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-656">**New behavior**</span></span>

<span data-ttu-id="8ccb3-657">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="8ccb3-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="8ccb3-658">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="8ccb3-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="8ccb3-659">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-659">**Why**</span></span>

<span data-ttu-id="8ccb3-660">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="8ccb3-661">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-661">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-662">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="8ccb3-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="8ccb3-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="8ccb3-664">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="8ccb3-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="8ccb3-665">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="8ccb3-666">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-666">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-667">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="8ccb3-668">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-668">**New behavior**</span></span>

<span data-ttu-id="8ccb3-669">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="8ccb3-670">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-670">**Why**</span></span>

<span data-ttu-id="8ccb3-671">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="8ccb3-672">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="8ccb3-673">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-673">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-674">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="8ccb3-675">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="8ccb3-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="8ccb3-676">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="8ccb3-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="8ccb3-677">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="8ccb3-678">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-678">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-679">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="8ccb3-680">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-680">**New behavior**</span></span>

<span data-ttu-id="8ccb3-681">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="8ccb3-682">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="8ccb3-683">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-683">**Why**</span></span>

<span data-ttu-id="8ccb3-684">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="8ccb3-685">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="8ccb3-686">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="8ccb3-687">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-687">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-688">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="8ccb3-689">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="8ccb3-690">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="8ccb3-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="8ccb3-691">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="8ccb3-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="8ccb3-692">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="8ccb3-693">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-693">**Old behavior**</span></span>

<span data-ttu-id="8ccb3-694">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="8ccb3-695">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-695">**New behavior**</span></span>

<span data-ttu-id="8ccb3-696">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="8ccb3-697">**Por que**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-697">**Why**</span></span>

<span data-ttu-id="8ccb3-698">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="8ccb3-699">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="8ccb3-700">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="8ccb3-700">**Mitigations**</span></span>

<span data-ttu-id="8ccb3-701">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="8ccb3-702">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="8ccb3-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
