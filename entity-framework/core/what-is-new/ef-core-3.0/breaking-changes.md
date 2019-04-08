---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: fd593b2832a5a6ffe27cd4493127b5d405f684ba
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914121"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="fef7d-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="fef7d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fef7d-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="fef7d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="fef7d-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="fef7d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="fef7d-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="fef7d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="fef7d-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="fef7d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="fef7d-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="fef7d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="fef7d-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="fef7d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="fef7d-109">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-110">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-110">Old behavior</span></span>**

<span data-ttu-id="fef7d-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="fef7d-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="fef7d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="fef7d-113">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-113">New behavior</span></span>**

<span data-ttu-id="fef7d-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="fef7d-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="fef7d-116">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-116">Why</span></span>**

<span data-ttu-id="fef7d-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="fef7d-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="fef7d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="fef7d-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="fef7d-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="fef7d-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="fef7d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="fef7d-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="fef7d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="fef7d-123">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-123">Mitigations</span></span>**

<span data-ttu-id="fef7d-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="fef7d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="fef7d-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fef7d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="fef7d-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="fef7d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="fef7d-127">Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="fef7d-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="fef7d-128">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-128">Old behavior</span></span>**

<span data-ttu-id="fef7d-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="fef7d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="fef7d-130">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-130">New behavior</span></span>**

<span data-ttu-id="fef7d-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="fef7d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="fef7d-132">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-132">Why</span></span>**

<span data-ttu-id="fef7d-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="fef7d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="fef7d-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="fef7d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="fef7d-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="fef7d-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="fef7d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="fef7d-137">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-137">Mitigations</span></span>**

<span data-ttu-id="fef7d-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="fef7d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="fef7d-139">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="fef7d-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="fef7d-140">Acompanhamento de problema nº 10996</span><span class="sxs-lookup"><span data-stu-id="fef7d-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="fef7d-141">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-142">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-142">Old behavior</span></span>**

<span data-ttu-id="fef7d-143">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="fef7d-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="fef7d-144">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-144">New behavior</span></span>**

<span data-ttu-id="fef7d-145">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="fef7d-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="fef7d-146">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="fef7d-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="fef7d-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="fef7d-149">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="fef7d-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="fef7d-150">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-150">Why</span></span>**

<span data-ttu-id="fef7d-151">Usando sobrecargas de método como esta, é muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="fef7d-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="fef7d-152">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="fef7d-153">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-153">Mitigations</span></span>**

<span data-ttu-id="fef7d-154">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="fef7d-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="fef7d-155">A execução de consulta é registrada no nível de Depuração</span><span class="sxs-lookup"><span data-stu-id="fef7d-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="fef7d-156">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="fef7d-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="fef7d-157">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-158">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-158">Old behavior</span></span>**

<span data-ttu-id="fef7d-159">Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="fef7d-160">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-160">New behavior</span></span>**

<span data-ttu-id="fef7d-161">A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="fef7d-162">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-162">Why</span></span>**

<span data-ttu-id="fef7d-163">Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="fef7d-164">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-164">Mitigations</span></span>**

<span data-ttu-id="fef7d-165">Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="fef7d-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="fef7d-166">Para fazer novamente o log do SQL no nível `Info`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="fef7d-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="fef7d-168">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="fef7d-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="fef7d-169">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="fef7d-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="fef7d-170">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="fef7d-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="fef7d-171">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-171">Old behavior</span></span>**

<span data-ttu-id="fef7d-172">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fef7d-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="fef7d-173">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="fef7d-174">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-174">New behavior</span></span>**

<span data-ttu-id="fef7d-175">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="fef7d-176">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-176">Why</span></span>**

<span data-ttu-id="fef7d-177">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="fef7d-178">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-178">Mitigations</span></span>**

<span data-ttu-id="fef7d-179">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="fef7d-180">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="fef7d-180">This can be avoided by:</span></span>
* <span data-ttu-id="fef7d-181">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="fef7d-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="fef7d-182">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="fef7d-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="fef7d-183">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="fef7d-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="fef7d-184">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="fef7d-185">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="fef7d-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="fef7d-186">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="fef7d-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="fef7d-187">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-188">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-188">Old behavior</span></span>**

<span data-ttu-id="fef7d-189">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="fef7d-190">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-190">New behavior</span></span>**

<span data-ttu-id="fef7d-191">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="fef7d-192">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="fef7d-193">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="fef7d-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="fef7d-194">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-194">Why</span></span>**

<span data-ttu-id="fef7d-195">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="fef7d-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="fef7d-196">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-196">Mitigations</span></span>**

<span data-ttu-id="fef7d-197">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="fef7d-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="fef7d-198">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="fef7d-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="fef7d-199">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="fef7d-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="fef7d-200">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="fef7d-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="fef7d-201">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="fef7d-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="fef7d-202">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="fef7d-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="fef7d-203">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-204">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-204">Old behavior</span></span>**

<span data-ttu-id="fef7d-205">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="fef7d-206">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-206">New behavior</span></span>**

<span data-ttu-id="fef7d-207">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="fef7d-208">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="fef7d-209">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-209">Why</span></span>**

<span data-ttu-id="fef7d-210">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="fef7d-211">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-211">Mitigations</span></span>**

<span data-ttu-id="fef7d-212">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="fef7d-213">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="fef7d-214">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="fef7d-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="fef7d-215">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="fef7d-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="fef7d-216">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-217">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-217">Old behavior</span></span>**

<span data-ttu-id="fef7d-218">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="fef7d-219">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="fef7d-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="fef7d-220">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-220">New behavior</span></span>**

<span data-ttu-id="fef7d-221">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="fef7d-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="fef7d-222">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="fef7d-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="fef7d-223">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-223">Why</span></span>**

<span data-ttu-id="fef7d-224">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="fef7d-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="fef7d-225">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="fef7d-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="fef7d-226">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="fef7d-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="fef7d-227">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-227">Mitigations</span></span>**

<span data-ttu-id="fef7d-228">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="fef7d-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="fef7d-229">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="fef7d-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="fef7d-230">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="fef7d-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="fef7d-231">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="fef7d-232">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="fef7d-233">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="fef7d-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="fef7d-234">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="fef7d-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="fef7d-235">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-236">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-236">Old behavior</span></span>**

<span data-ttu-id="fef7d-237">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="fef7d-238">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-238">New behavior</span></span>**

<span data-ttu-id="fef7d-239">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="fef7d-240">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="fef7d-241">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="fef7d-242">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="fef7d-243">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-243">For example:</span></span>

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

<span data-ttu-id="fef7d-244">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="fef7d-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="fef7d-245">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-245">Why</span></span>**

<span data-ttu-id="fef7d-246">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="fef7d-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="fef7d-247">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="fef7d-248">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-248">Mitigations</span></span>**

<span data-ttu-id="fef7d-249">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="fef7d-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="fef7d-250">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="fef7d-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="fef7d-251">Acompanhamento de problema nº 9005</span><span class="sxs-lookup"><span data-stu-id="fef7d-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="fef7d-252">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-253">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-253">Old behavior</span></span>**

<span data-ttu-id="fef7d-254">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="fef7d-254">Consider the following model:</span></span>
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
<span data-ttu-id="fef7d-255">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="fef7d-256">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-256">New behavior</span></span>**

<span data-ttu-id="fef7d-257">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="fef7d-258">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="fef7d-259">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-259">Mitigations</span></span>**

<span data-ttu-id="fef7d-260">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="fef7d-261">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="fef7d-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="fef7d-262">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="fef7d-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="fef7d-263">Acompanhamento de problema nº 14154</span><span class="sxs-lookup"><span data-stu-id="fef7d-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="fef7d-264">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-265">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-265">Old behavior</span></span>**

<span data-ttu-id="fef7d-266">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="fef7d-266">Consider the following model:</span></span>
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
<span data-ttu-id="fef7d-267">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="fef7d-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="fef7d-268">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-268">New behavior</span></span>**

<span data-ttu-id="fef7d-269">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="fef7d-270">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="fef7d-271">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-271">Why</span></span>**

<span data-ttu-id="fef7d-272">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="fef7d-273">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-273">Mitigations</span></span>**

<span data-ttu-id="fef7d-274">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="fef7d-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="fef7d-275">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="fef7d-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="fef7d-276">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="fef7d-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="fef7d-277">Acompanhamento de problema nº 13998</span><span class="sxs-lookup"><span data-stu-id="fef7d-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="fef7d-278">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-279">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-279">Old behavior</span></span>**

<span data-ttu-id="fef7d-280">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="fef7d-280">Consider the following model:</span></span>
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

<span data-ttu-id="fef7d-281">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="fef7d-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="fef7d-282">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-282">New behavior</span></span>**

<span data-ttu-id="fef7d-283">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="fef7d-284">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-284">Why</span></span>**

<span data-ttu-id="fef7d-285">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="fef7d-286">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-286">Mitigations</span></span>**

<span data-ttu-id="fef7d-287">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="fef7d-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="fef7d-288">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="fef7d-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="fef7d-289">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="fef7d-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="fef7d-290">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-291">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-291">Old behavior</span></span>**

<span data-ttu-id="fef7d-292">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="fef7d-292">Consider the following model:</span></span>
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
<span data-ttu-id="fef7d-293">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="fef7d-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="fef7d-294">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="fef7d-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="fef7d-295">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-295">New behavior</span></span>**

<span data-ttu-id="fef7d-296">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="fef7d-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="fef7d-297">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="fef7d-298">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-298">For example:</span></span>

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

**<span data-ttu-id="fef7d-299">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-299">Why</span></span>**

<span data-ttu-id="fef7d-300">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="fef7d-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="fef7d-301">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-301">Mitigations</span></span>**

<span data-ttu-id="fef7d-302">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="fef7d-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="fef7d-303">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="fef7d-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="fef7d-304">Acompanhamento de problema nº 14218</span><span class="sxs-lookup"><span data-stu-id="fef7d-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="fef7d-305">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-306">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-306">Old behavior</span></span>**

<span data-ttu-id="fef7d-307">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

**<span data-ttu-id="fef7d-308">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-308">New behavior</span></span>**

<span data-ttu-id="fef7d-309">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="fef7d-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="fef7d-310">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-310">Why</span></span>**

<span data-ttu-id="fef7d-311">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="fef7d-312">O novo comportamento corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="fef7d-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="fef7d-313">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-313">Mitigations</span></span>**

<span data-ttu-id="fef7d-314">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="fef7d-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="fef7d-315">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="fef7d-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="fef7d-316">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="fef7d-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="fef7d-317">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-318">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-318">Old behavior</span></span>**

<span data-ttu-id="fef7d-319">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="fef7d-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="fef7d-320">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-320">New behavior</span></span>**

<span data-ttu-id="fef7d-321">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="fef7d-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="fef7d-322">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="fef7d-323">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-323">Why</span></span>**

<span data-ttu-id="fef7d-324">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="fef7d-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="fef7d-325">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-325">Mitigations</span></span>**

<span data-ttu-id="fef7d-326">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="fef7d-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="fef7d-327">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="fef7d-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="fef7d-328">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="fef7d-328">Backing fields are used by default</span></span>

[<span data-ttu-id="fef7d-329">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="fef7d-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="fef7d-330">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="fef7d-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="fef7d-331">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-331">Old behavior</span></span>**

<span data-ttu-id="fef7d-332">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="fef7d-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="fef7d-333">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="fef7d-334">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-334">New behavior</span></span>**

<span data-ttu-id="fef7d-335">A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="fef7d-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="fef7d-336">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="fef7d-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="fef7d-337">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-337">Why</span></span>**

<span data-ttu-id="fef7d-338">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="fef7d-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="fef7d-339">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-339">Mitigations</span></span>**

<span data-ttu-id="fef7d-340">O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="fef7d-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="fef7d-341">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="fef7d-342">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="fef7d-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="fef7d-343">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="fef7d-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="fef7d-344">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-345">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-345">Old behavior</span></span>**

<span data-ttu-id="fef7d-346">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="fef7d-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="fef7d-347">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="fef7d-348">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-348">New behavior</span></span>**

<span data-ttu-id="fef7d-349">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="fef7d-350">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-350">Why</span></span>**

<span data-ttu-id="fef7d-351">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="fef7d-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="fef7d-352">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-352">Mitigations</span></span>**

<span data-ttu-id="fef7d-353">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="fef7d-354">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="fef7d-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="fef7d-355">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="fef7d-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="fef7d-356">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="fef7d-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="fef7d-357">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-358">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-358">Old behavior</span></span>**

<span data-ttu-id="fef7d-359">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="fef7d-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="fef7d-360">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-360">New behavior</span></span>**

<span data-ttu-id="fef7d-361">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="fef7d-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="fef7d-362">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-362">Why</span></span>**

<span data-ttu-id="fef7d-363">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="fef7d-364">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="fef7d-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="fef7d-365">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-365">Mitigations</span></span>**

<span data-ttu-id="fef7d-366">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="fef7d-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="fef7d-367">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="fef7d-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="fef7d-368">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="fef7d-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="fef7d-369">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-370">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-370">Old behavior</span></span>**

<span data-ttu-id="fef7d-371">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="fef7d-372">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="fef7d-373">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-373">New behavior</span></span>**

<span data-ttu-id="fef7d-374">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="fef7d-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="fef7d-375">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="fef7d-376">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="fef7d-377">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="fef7d-378">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-378">Why</span></span>**

<span data-ttu-id="fef7d-379">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="fef7d-380">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-380">Mitigations</span></span>**

<span data-ttu-id="fef7d-381">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="fef7d-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="fef7d-382">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="fef7d-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="fef7d-383">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="fef7d-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="fef7d-384">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-385">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-385">Old behavior</span></span>**

<span data-ttu-id="fef7d-386">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="fef7d-387">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="fef7d-388">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-388">New behavior</span></span>**

<span data-ttu-id="fef7d-389">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="fef7d-390">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-390">Why</span></span>**

<span data-ttu-id="fef7d-391">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="fef7d-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="fef7d-392">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-392">Mitigations</span></span>**

<span data-ttu-id="fef7d-393">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="fef7d-394">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="fef7d-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="fef7d-395">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="fef7d-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="fef7d-396">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="fef7d-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="fef7d-397">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="fef7d-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="fef7d-398">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-399">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-399">Old behavior</span></span>**

<span data-ttu-id="fef7d-400">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="fef7d-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="fef7d-401">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-401">New behavior</span></span>**

<span data-ttu-id="fef7d-402">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="fef7d-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="fef7d-403">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-403">Why</span></span>**

<span data-ttu-id="fef7d-404">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="fef7d-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="fef7d-405">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-405">Mitigations</span></span>**

<span data-ttu-id="fef7d-406">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="fef7d-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="fef7d-407">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="fef7d-407">This isn't common.</span></span>
<span data-ttu-id="fef7d-408">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="fef7d-409">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="fef7d-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="fef7d-410">IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="fef7d-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="fef7d-411">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="fef7d-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="fef7d-412">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-413">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="fef7d-414">era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2.x.</span><span class="sxs-lookup"><span data-stu-id="fef7d-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="fef7d-415">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-415">New behavior</span></span>**

<span data-ttu-id="fef7d-416">As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="fef7d-417">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-417">Why</span></span>**

<span data-ttu-id="fef7d-418">Essa alteração foi feita porque as interfaces são conceitualmente uma.</span><span class="sxs-lookup"><span data-stu-id="fef7d-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="fef7d-419">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-419">Mitigations</span></span>**

<span data-ttu-id="fef7d-420">Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.</span><span class="sxs-lookup"><span data-stu-id="fef7d-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="fef7d-421">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="fef7d-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="fef7d-422">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="fef7d-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="fef7d-423">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-424">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-424">Old behavior</span></span>**

<span data-ttu-id="fef7d-425">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="fef7d-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="fef7d-426">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="fef7d-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="fef7d-427">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="fef7d-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="fef7d-428">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-428">New behavior</span></span>**

<span data-ttu-id="fef7d-429">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="fef7d-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="fef7d-430">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="fef7d-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="fef7d-431">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="fef7d-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="fef7d-432">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="fef7d-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="fef7d-433">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-433">Why</span></span>**

<span data-ttu-id="fef7d-434">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="fef7d-435">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-435">Mitigations</span></span>**

<span data-ttu-id="fef7d-436">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="fef7d-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="fef7d-437">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="fef7d-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="fef7d-438">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="fef7d-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="fef7d-439">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-440">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-440">Old behavior</span></span>**

<span data-ttu-id="fef7d-441">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="fef7d-442">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-442">New behavior</span></span>**

<span data-ttu-id="fef7d-443">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="fef7d-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="fef7d-444">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-444">Why</span></span>**

<span data-ttu-id="fef7d-445">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="fef7d-446">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-446">Mitigations</span></span>**

<span data-ttu-id="fef7d-447">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="fef7d-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="fef7d-448">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="fef7d-449">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="fef7d-450">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="fef7d-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="fef7d-451">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="fef7d-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="fef7d-452">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-453">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-453">Old behavior</span></span>**

<span data-ttu-id="fef7d-454">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="fef7d-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="fef7d-455">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="fef7d-456">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="fef7d-457">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="fef7d-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="fef7d-458">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-458">New behavior</span></span>**

<span data-ttu-id="fef7d-459">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="fef7d-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="fef7d-460">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-460">Why</span></span>**

<span data-ttu-id="fef7d-461">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="fef7d-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="fef7d-462">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-462">Mitigations</span></span>**

<span data-ttu-id="fef7d-463">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="fef7d-464">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="fef7d-464">This is not common.</span></span>
<span data-ttu-id="fef7d-465">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="fef7d-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="fef7d-466">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="fef7d-467">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="fef7d-467">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="fef7d-468">Acompanhamento de problema nº 15184</span><span class="sxs-lookup"><span data-stu-id="fef7d-468">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="fef7d-469">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-469">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-470">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-470">Old behavior</span></span>**

<span data-ttu-id="fef7d-471">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="fef7d-471">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` <span data-ttu-id="fef7d-472">(e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="fef7d-472">(and deriving classes)</span></span>

**<span data-ttu-id="fef7d-473">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-473">New behavior</span></span>**

<span data-ttu-id="fef7d-474">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="fef7d-474">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

**<span data-ttu-id="fef7d-475">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-475">Why</span></span>**

<span data-ttu-id="fef7d-476">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="fef7d-476">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

**<span data-ttu-id="fef7d-477">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-477">Mitigations</span></span>**

<span data-ttu-id="fef7d-478">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="fef7d-478">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="fef7d-479">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="fef7d-479">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="fef7d-480">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="fef7d-480">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="fef7d-481">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="fef7d-481">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="fef7d-482">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="fef7d-482">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="fef7d-483">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="fef7d-483">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="fef7d-484">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-484">Old behavior</span></span>**

<span data-ttu-id="fef7d-485">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="fef7d-485">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="fef7d-486">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-486">New behavior</span></span>**

<span data-ttu-id="fef7d-487">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="fef7d-487">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="fef7d-488">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-488">Why</span></span>**

<span data-ttu-id="fef7d-489">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="fef7d-489">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="fef7d-490">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-490">Mitigations</span></span>**

<span data-ttu-id="fef7d-491">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="fef7d-491">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="fef7d-492">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-492">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="fef7d-493">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="fef7d-493">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="fef7d-494">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="fef7d-494">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="fef7d-495">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-495">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-496">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-496">Old behavior</span></span>**

<span data-ttu-id="fef7d-497">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="fef7d-497">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="fef7d-498">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-498">New behavior</span></span>**

<span data-ttu-id="fef7d-499">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="fef7d-499">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="fef7d-500">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-500">Why</span></span>**

<span data-ttu-id="fef7d-501">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-501">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="fef7d-502">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="fef7d-502">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="fef7d-503">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-503">Mitigations</span></span>**

<span data-ttu-id="fef7d-504">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-504">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="fef7d-505">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="fef7d-505">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="fef7d-506">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="fef7d-506">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="fef7d-507">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-507">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-508">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-508">Old behavior</span></span>**

<span data-ttu-id="fef7d-509">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-509">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="fef7d-510">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-510">New behavior</span></span>**

<span data-ttu-id="fef7d-511">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="fef7d-511">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="fef7d-512">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-512">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="fef7d-513">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-513">Why</span></span>**

<span data-ttu-id="fef7d-514">Essa alteração foi feita para consolidar a API para índices com `Includes` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fef7d-514">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="fef7d-515">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-515">Mitigations</span></span>**

<span data-ttu-id="fef7d-516">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="fef7d-516">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="fef7d-517">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="fef7d-517">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="fef7d-518">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="fef7d-518">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="fef7d-519">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="fef7d-519">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="fef7d-520">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-520">Old behavior</span></span>**

<span data-ttu-id="fef7d-521">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="fef7d-521">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="fef7d-522">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-522">New behavior</span></span>**

<span data-ttu-id="fef7d-523">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="fef7d-523">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="fef7d-524">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-524">Why</span></span>**

<span data-ttu-id="fef7d-525">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="fef7d-525">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="fef7d-526">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-526">Mitigations</span></span>**

<span data-ttu-id="fef7d-527">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="fef7d-527">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="fef7d-528">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="fef7d-528">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="fef7d-529">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="fef7d-529">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="fef7d-530">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-530">Old behavior</span></span>**

<span data-ttu-id="fef7d-531">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-531">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="fef7d-532">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-532">New behavior</span></span>**

<span data-ttu-id="fef7d-533">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-533">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="fef7d-534">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-534">Why</span></span>**

<span data-ttu-id="fef7d-535">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="fef7d-535">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="fef7d-536">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-536">Mitigations</span></span>**

<span data-ttu-id="fef7d-537">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="fef7d-537">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fef7d-538">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="fef7d-538">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fef7d-539">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="fef7d-539">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="fef7d-540">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-541">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-541">Old behavior</span></span>**

<span data-ttu-id="fef7d-542">Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="fef7d-542">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="fef7d-543">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-543">New behavior</span></span>**

<span data-ttu-id="fef7d-544">Agora, os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="fef7d-544">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="fef7d-545">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-545">Why</span></span>**

<span data-ttu-id="fef7d-546">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="fef7d-546">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="fef7d-547">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="fef7d-547">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="fef7d-548">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-548">Mitigations</span></span>**

<span data-ttu-id="fef7d-549">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="fef7d-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="fef7d-550">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="fef7d-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="fef7d-551">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="fef7d-551">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fef7d-552">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="fef7d-552">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fef7d-553">Acompanhamento de problema nº 15020</span><span class="sxs-lookup"><span data-stu-id="fef7d-553">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="fef7d-554">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-555">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-555">Old behavior</span></span>**

<span data-ttu-id="fef7d-556">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="fef7d-556">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="fef7d-557">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="fef7d-557">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="fef7d-558">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-558">New behavior</span></span>**

<span data-ttu-id="fef7d-559">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="fef7d-559">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="fef7d-560">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-560">Why</span></span>**

<span data-ttu-id="fef7d-561">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="fef7d-561">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="fef7d-562">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-562">Mitigations</span></span>**

<span data-ttu-id="fef7d-563">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="fef7d-563">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="fef7d-564">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="fef7d-564">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="fef7d-565">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="fef7d-565">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="fef7d-566">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="fef7d-566">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="fef7d-567">Acompanhamento de problema nº 12978</span><span class="sxs-lookup"><span data-stu-id="fef7d-567">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="fef7d-568">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-568">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-569">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-569">Old behavior</span></span>**

<span data-ttu-id="fef7d-570">As IDs de migração eram geradas inadvertidamente usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="fef7d-570">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="fef7d-571">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-571">New behavior</span></span>**

<span data-ttu-id="fef7d-572">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="fef7d-572">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="fef7d-573">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-573">Why</span></span>**

<span data-ttu-id="fef7d-574">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="fef7d-574">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="fef7d-575">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="fef7d-575">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="fef7d-576">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-576">Mitigations</span></span>**

<span data-ttu-id="fef7d-577">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="fef7d-577">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="fef7d-578">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="fef7d-578">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="fef7d-579">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="fef7d-579">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="fef7d-580">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="fef7d-580">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="fef7d-581">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="fef7d-581">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="fef7d-582">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="fef7d-582">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="fef7d-583">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-583">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-584">Alteração</span><span class="sxs-lookup"><span data-stu-id="fef7d-584">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="fef7d-585">foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="fef7d-585">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="fef7d-586">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-586">Why</span></span>**

<span data-ttu-id="fef7d-587">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="fef7d-587">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="fef7d-588">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-588">Mitigations</span></span>**

<span data-ttu-id="fef7d-589">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="fef7d-589">Use the new name.</span></span> <span data-ttu-id="fef7d-590">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="fef7d-590">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="fef7d-591">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="fef7d-591">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="fef7d-592">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="fef7d-592">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="fef7d-593">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="fef7d-593">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="fef7d-594">Comportamento antigo</span><span class="sxs-lookup"><span data-stu-id="fef7d-594">Old behavior</span></span>**

<span data-ttu-id="fef7d-595">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="fef7d-595">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="fef7d-596">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-596">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="fef7d-597">Novo comportamento</span><span class="sxs-lookup"><span data-stu-id="fef7d-597">New behavior</span></span>**

<span data-ttu-id="fef7d-598">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="fef7d-598">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="fef7d-599">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fef7d-599">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="fef7d-600">Por que</span><span class="sxs-lookup"><span data-stu-id="fef7d-600">Why</span></span>**

<span data-ttu-id="fef7d-601">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="fef7d-601">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="fef7d-602">Mitigações</span><span class="sxs-lookup"><span data-stu-id="fef7d-602">Mitigations</span></span>**

<span data-ttu-id="fef7d-603">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="fef7d-603">Use the new name.</span></span>
