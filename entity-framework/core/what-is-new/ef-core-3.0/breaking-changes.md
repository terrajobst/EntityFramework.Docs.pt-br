---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619253"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="44b90-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="44b90-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44b90-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="44b90-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="44b90-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="44b90-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="44b90-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="44b90-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="44b90-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="44b90-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="44b90-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="44b90-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="44b90-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="44b90-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="44b90-109">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-110">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-110">**Old behavior**</span></span>

<span data-ttu-id="44b90-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="44b90-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="44b90-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="44b90-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="44b90-113">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-113">**New behavior**</span></span>

<span data-ttu-id="44b90-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="44b90-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="44b90-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="44b90-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="44b90-116">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-116">**Why**</span></span>

<span data-ttu-id="44b90-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="44b90-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="44b90-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="44b90-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="44b90-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="44b90-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="44b90-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="44b90-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="44b90-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="44b90-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="44b90-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="44b90-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="44b90-123">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-123">**Mitigations**</span></span>

<span data-ttu-id="44b90-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="44b90-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="44b90-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44b90-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="44b90-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="44b90-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="44b90-127">Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="44b90-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="44b90-128">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-128">**Old behavior**</span></span>

<span data-ttu-id="44b90-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="44b90-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="44b90-130">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-130">**New behavior**</span></span>

<span data-ttu-id="44b90-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="44b90-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="44b90-132">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-132">**Why**</span></span>

<span data-ttu-id="44b90-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="44b90-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="44b90-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="44b90-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="44b90-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="44b90-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="44b90-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="44b90-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="44b90-137">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-137">**Mitigations**</span></span>

<span data-ttu-id="44b90-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="44b90-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="44b90-139">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="44b90-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="44b90-140">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="44b90-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="44b90-141">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-142">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-142">**Old behavior**</span></span>

<span data-ttu-id="44b90-143">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="44b90-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="44b90-144">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-144">**New behavior**</span></span>

<span data-ttu-id="44b90-145">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="44b90-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="44b90-146">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="44b90-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="44b90-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="44b90-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="44b90-149">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="44b90-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="44b90-150">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-150">**Why**</span></span>

<span data-ttu-id="44b90-151">Usando sobrecargas de método como esta, é muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="44b90-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="44b90-152">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="44b90-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="44b90-153">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-153">**Mitigations**</span></span>

<span data-ttu-id="44b90-154">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="44b90-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="44b90-155">A execução de consulta é registrada no nível de Depuração</span><span class="sxs-lookup"><span data-stu-id="44b90-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="44b90-156">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="44b90-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="44b90-157">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-158">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-158">**Old behavior**</span></span>

<span data-ttu-id="44b90-159">Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.</span><span class="sxs-lookup"><span data-stu-id="44b90-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="44b90-160">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-160">**New behavior**</span></span>

<span data-ttu-id="44b90-161">A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="44b90-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="44b90-162">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-162">**Why**</span></span>

<span data-ttu-id="44b90-163">Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.</span><span class="sxs-lookup"><span data-stu-id="44b90-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="44b90-164">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-164">**Mitigations**</span></span>

<span data-ttu-id="44b90-165">Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="44b90-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="44b90-166">Para fazer novamente o log do SQL no nível `Info`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="44b90-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="44b90-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="44b90-168">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="44b90-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="44b90-169">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="44b90-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="44b90-170">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="44b90-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="44b90-171">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-171">**Old behavior**</span></span>

<span data-ttu-id="44b90-172">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="44b90-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="44b90-173">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="44b90-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="44b90-174">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-174">**New behavior**</span></span>

<span data-ttu-id="44b90-175">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="44b90-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="44b90-176">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-176">**Why**</span></span>

<span data-ttu-id="44b90-177">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="44b90-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="44b90-178">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-178">**Mitigations**</span></span>

<span data-ttu-id="44b90-179">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="44b90-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="44b90-180">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="44b90-180">This can be avoided by:</span></span>
* <span data-ttu-id="44b90-181">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="44b90-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="44b90-182">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="44b90-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="44b90-183">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="44b90-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="44b90-184">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="44b90-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="44b90-185">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="44b90-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="44b90-186">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="44b90-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="44b90-187">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-188">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-188">**Old behavior**</span></span>

<span data-ttu-id="44b90-189">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="44b90-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="44b90-190">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-190">**New behavior**</span></span>

<span data-ttu-id="44b90-191">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="44b90-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="44b90-192">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="44b90-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="44b90-193">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="44b90-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="44b90-194">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-194">**Why**</span></span>

<span data-ttu-id="44b90-195">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="44b90-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="44b90-196">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-196">**Mitigations**</span></span>

<span data-ttu-id="44b90-197">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="44b90-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="44b90-198">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="44b90-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="44b90-199">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="44b90-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="44b90-200">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="44b90-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="44b90-201">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="44b90-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="44b90-202">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="44b90-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="44b90-203">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-204">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-204">**Old behavior**</span></span>

<span data-ttu-id="44b90-205">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="44b90-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="44b90-206">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-206">**New behavior**</span></span>

<span data-ttu-id="44b90-207">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="44b90-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="44b90-208">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="44b90-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="44b90-209">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-209">**Why**</span></span>

<span data-ttu-id="44b90-210">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="44b90-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="44b90-211">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-211">**Mitigations**</span></span>

<span data-ttu-id="44b90-212">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="44b90-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="44b90-213">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="44b90-214">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="44b90-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="44b90-215">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="44b90-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="44b90-216">Essa alteração será introduzida no EF Core 3.0-preview 5.</span><span class="sxs-lookup"><span data-stu-id="44b90-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="44b90-217">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-217">**Old behavior**</span></span>

<span data-ttu-id="44b90-218">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="44b90-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="44b90-219">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-219">**New behavior**</span></span>

<span data-ttu-id="44b90-220">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="44b90-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="44b90-221">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-221">**Why**</span></span>

<span data-ttu-id="44b90-222">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="44b90-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="44b90-223">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-223">**Mitigations**</span></span>

<span data-ttu-id="44b90-224">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="44b90-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="44b90-225">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="44b90-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="44b90-226">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="44b90-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="44b90-227">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-228">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-228">**Old behavior**</span></span>

<span data-ttu-id="44b90-229">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="44b90-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="44b90-230">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="44b90-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="44b90-231">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-231">**New behavior**</span></span>

<span data-ttu-id="44b90-232">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="44b90-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="44b90-233">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="44b90-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="44b90-234">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-234">**Why**</span></span>

<span data-ttu-id="44b90-235">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="44b90-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="44b90-236">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="44b90-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="44b90-237">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="44b90-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="44b90-238">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-238">**Mitigations**</span></span>

<span data-ttu-id="44b90-239">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="44b90-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="44b90-240">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="44b90-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="44b90-241">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="44b90-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="44b90-242">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="44b90-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="44b90-243">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="44b90-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="44b90-244">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="44b90-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="44b90-245">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="44b90-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="44b90-246">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-247">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-247">**Old behavior**</span></span>

<span data-ttu-id="44b90-248">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="44b90-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="44b90-249">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-249">**New behavior**</span></span>

<span data-ttu-id="44b90-250">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="44b90-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="44b90-251">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="44b90-252">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="44b90-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="44b90-253">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="44b90-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="44b90-254">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-254">For example:</span></span>

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

<span data-ttu-id="44b90-255">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="44b90-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="44b90-256">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-256">**Why**</span></span>

<span data-ttu-id="44b90-257">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="44b90-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="44b90-258">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="44b90-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="44b90-259">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-259">**Mitigations**</span></span>

<span data-ttu-id="44b90-260">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="44b90-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="44b90-261">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="44b90-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="44b90-262">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="44b90-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="44b90-263">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-264">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-264">**Old behavior**</span></span>

<span data-ttu-id="44b90-265">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="44b90-265">Consider the following model:</span></span>
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
<span data-ttu-id="44b90-266">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="44b90-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="44b90-267">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-267">**New behavior**</span></span>

<span data-ttu-id="44b90-268">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="44b90-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="44b90-269">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="44b90-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="44b90-270">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-270">**Mitigations**</span></span>

<span data-ttu-id="44b90-271">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="44b90-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="44b90-272">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="44b90-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="44b90-273">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="44b90-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="44b90-274">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="44b90-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="44b90-275">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-276">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-276">**Old behavior**</span></span>

<span data-ttu-id="44b90-277">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="44b90-277">Consider the following model:</span></span>
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
<span data-ttu-id="44b90-278">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="44b90-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="44b90-279">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-279">**New behavior**</span></span>

<span data-ttu-id="44b90-280">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="44b90-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="44b90-281">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="44b90-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="44b90-282">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-282">**Why**</span></span>

<span data-ttu-id="44b90-283">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="44b90-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="44b90-284">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-284">**Mitigations**</span></span>

<span data-ttu-id="44b90-285">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="44b90-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="44b90-286">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="44b90-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="44b90-287">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="44b90-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="44b90-288">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="44b90-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="44b90-289">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-290">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-290">**Old behavior**</span></span>

<span data-ttu-id="44b90-291">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="44b90-291">Consider the following model:</span></span>
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

<span data-ttu-id="44b90-292">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="44b90-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="44b90-293">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-293">**New behavior**</span></span>

<span data-ttu-id="44b90-294">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="44b90-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="44b90-295">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-295">**Why**</span></span>

<span data-ttu-id="44b90-296">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="44b90-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="44b90-297">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-297">**Mitigations**</span></span>

<span data-ttu-id="44b90-298">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="44b90-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="44b90-299">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="44b90-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="44b90-300">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="44b90-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="44b90-301">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-302">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-302">**Old behavior**</span></span>

<span data-ttu-id="44b90-303">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="44b90-303">Consider the following model:</span></span>
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
<span data-ttu-id="44b90-304">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="44b90-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="44b90-305">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="44b90-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="44b90-306">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-306">**New behavior**</span></span>

<span data-ttu-id="44b90-307">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="44b90-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="44b90-308">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="44b90-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="44b90-309">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-309">For example:</span></span>

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

<span data-ttu-id="44b90-310">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-310">**Why**</span></span>

<span data-ttu-id="44b90-311">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="44b90-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="44b90-312">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-312">**Mitigations**</span></span>

<span data-ttu-id="44b90-313">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="44b90-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="44b90-314">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="44b90-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="44b90-315">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="44b90-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="44b90-316">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-317">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-317">**Old behavior**</span></span>

<span data-ttu-id="44b90-318">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="44b90-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="44b90-319">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-319">**New behavior**</span></span>

<span data-ttu-id="44b90-320">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="44b90-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="44b90-321">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-321">**Why**</span></span>

<span data-ttu-id="44b90-322">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="44b90-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="44b90-323">O novo comportamento corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="44b90-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="44b90-324">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-324">**Mitigations**</span></span>

<span data-ttu-id="44b90-325">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="44b90-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="44b90-326">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="44b90-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="44b90-327">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="44b90-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="44b90-328">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-329">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-329">**Old behavior**</span></span>

<span data-ttu-id="44b90-330">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="44b90-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="44b90-331">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-331">**New behavior**</span></span>

<span data-ttu-id="44b90-332">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="44b90-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="44b90-333">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="44b90-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="44b90-334">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-334">**Why**</span></span>

<span data-ttu-id="44b90-335">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="44b90-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="44b90-336">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-336">**Mitigations**</span></span>

<span data-ttu-id="44b90-337">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="44b90-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="44b90-338">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="44b90-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="44b90-339">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="44b90-339">Backing fields are used by default</span></span>

[<span data-ttu-id="44b90-340">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="44b90-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="44b90-341">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="44b90-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="44b90-342">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-342">**Old behavior**</span></span>

<span data-ttu-id="44b90-343">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="44b90-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="44b90-344">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="44b90-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="44b90-345">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-345">**New behavior**</span></span>

<span data-ttu-id="44b90-346">A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="44b90-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="44b90-347">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="44b90-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="44b90-348">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-348">**Why**</span></span>

<span data-ttu-id="44b90-349">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="44b90-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="44b90-350">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-350">**Mitigations**</span></span>

<span data-ttu-id="44b90-351">O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="44b90-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="44b90-352">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="44b90-353">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="44b90-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="44b90-354">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="44b90-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="44b90-355">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-356">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-356">**Old behavior**</span></span>

<span data-ttu-id="44b90-357">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="44b90-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="44b90-358">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="44b90-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="44b90-359">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-359">**New behavior**</span></span>

<span data-ttu-id="44b90-360">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="44b90-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="44b90-361">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-361">**Why**</span></span>

<span data-ttu-id="44b90-362">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="44b90-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="44b90-363">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-363">**Mitigations**</span></span>

<span data-ttu-id="44b90-364">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="44b90-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="44b90-365">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="44b90-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="44b90-366">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="44b90-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="44b90-367">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-368">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-368">**Old behavior**</span></span>

<span data-ttu-id="44b90-369">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="44b90-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
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

<span data-ttu-id="44b90-370">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-370">**New behavior**</span></span>

<span data-ttu-id="44b90-371">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="44b90-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="44b90-372">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-372">**Why**</span></span>

<span data-ttu-id="44b90-373">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="44b90-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="44b90-374">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-374">**Mitigations**</span></span>

<span data-ttu-id="44b90-375">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="44b90-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="44b90-376">Em uma versão prévia posterior do EF Core 3.0, planejamos habilitar novamente a configuração explícita de um nome de campo que seja diferente do nome da propriedade:</span><span class="sxs-lookup"><span data-stu-id="44b90-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="44b90-377">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="44b90-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="44b90-378">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="44b90-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="44b90-379">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-380">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-380">**Old behavior**</span></span>

<span data-ttu-id="44b90-381">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="44b90-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="44b90-382">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-382">**New behavior**</span></span>

<span data-ttu-id="44b90-383">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="44b90-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="44b90-384">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-384">**Why**</span></span>

<span data-ttu-id="44b90-385">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="44b90-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="44b90-386">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="44b90-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="44b90-387">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-387">**Mitigations**</span></span>

<span data-ttu-id="44b90-388">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="44b90-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="44b90-389">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="44b90-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="44b90-390">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="44b90-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="44b90-391">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-392">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-392">**Old behavior**</span></span>

<span data-ttu-id="44b90-393">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="44b90-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="44b90-394">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="44b90-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="44b90-395">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-395">**New behavior**</span></span>

<span data-ttu-id="44b90-396">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="44b90-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="44b90-397">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="44b90-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="44b90-398">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="44b90-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="44b90-399">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="44b90-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="44b90-400">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-400">**Why**</span></span>

<span data-ttu-id="44b90-401">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="44b90-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="44b90-402">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-402">**Mitigations**</span></span>

<span data-ttu-id="44b90-403">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="44b90-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="44b90-404">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="44b90-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="44b90-405">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="44b90-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="44b90-406">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-407">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-407">**Old behavior**</span></span>

<span data-ttu-id="44b90-408">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="44b90-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="44b90-409">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="44b90-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="44b90-410">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-410">**New behavior**</span></span>

<span data-ttu-id="44b90-411">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="44b90-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="44b90-412">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-412">**Why**</span></span>

<span data-ttu-id="44b90-413">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="44b90-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="44b90-414">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-414">**Mitigations**</span></span>

<span data-ttu-id="44b90-415">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="44b90-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="44b90-416">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="44b90-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="44b90-417">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="44b90-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="44b90-418">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="44b90-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="44b90-419">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="44b90-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="44b90-420">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-421">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-421">**Old behavior**</span></span>

<span data-ttu-id="44b90-422">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="44b90-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="44b90-423">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-423">**New behavior**</span></span>

<span data-ttu-id="44b90-424">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="44b90-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="44b90-425">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-425">**Why**</span></span>

<span data-ttu-id="44b90-426">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="44b90-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="44b90-427">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-427">**Mitigations**</span></span>

<span data-ttu-id="44b90-428">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="44b90-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="44b90-429">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="44b90-429">This isn't common.</span></span>
<span data-ttu-id="44b90-430">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="44b90-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="44b90-431">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="44b90-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="44b90-432">IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="44b90-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="44b90-433">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="44b90-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="44b90-434">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-435">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-435">**Old behavior**</span></span>

<span data-ttu-id="44b90-436">`IDbContextOptionsExtensionWithDebugInfo` era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2. x.</span><span class="sxs-lookup"><span data-stu-id="44b90-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="44b90-437">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-437">**New behavior**</span></span>

<span data-ttu-id="44b90-438">As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="44b90-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="44b90-439">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-439">**Why**</span></span>

<span data-ttu-id="44b90-440">Essa alteração foi feita porque as interfaces são conceitualmente uma.</span><span class="sxs-lookup"><span data-stu-id="44b90-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="44b90-441">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-441">**Mitigations**</span></span>

<span data-ttu-id="44b90-442">Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.</span><span class="sxs-lookup"><span data-stu-id="44b90-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="44b90-443">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="44b90-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="44b90-444">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="44b90-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="44b90-445">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-446">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-446">**Old behavior**</span></span>

<span data-ttu-id="44b90-447">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="44b90-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="44b90-448">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="44b90-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="44b90-449">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="44b90-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="44b90-450">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-450">**New behavior**</span></span>

<span data-ttu-id="44b90-451">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="44b90-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="44b90-452">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="44b90-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="44b90-453">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="44b90-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="44b90-454">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="44b90-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="44b90-455">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-455">**Why**</span></span>

<span data-ttu-id="44b90-456">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="44b90-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="44b90-457">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-457">**Mitigations**</span></span>

<span data-ttu-id="44b90-458">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="44b90-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="44b90-459">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="44b90-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="44b90-460">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="44b90-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="44b90-461">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-462">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-462">**Old behavior**</span></span>

<span data-ttu-id="44b90-463">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="44b90-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="44b90-464">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-464">**New behavior**</span></span>

<span data-ttu-id="44b90-465">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="44b90-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="44b90-466">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-466">**Why**</span></span>

<span data-ttu-id="44b90-467">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="44b90-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="44b90-468">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-468">**Mitigations**</span></span>

<span data-ttu-id="44b90-469">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="44b90-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="44b90-470">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="44b90-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="44b90-471">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="44b90-472">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="44b90-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="44b90-473">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="44b90-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="44b90-474">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-475">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-475">**Old behavior**</span></span>

<span data-ttu-id="44b90-476">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="44b90-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="44b90-477">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="44b90-478">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="44b90-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="44b90-479">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="44b90-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="44b90-480">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-480">**New behavior**</span></span>

<span data-ttu-id="44b90-481">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="44b90-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="44b90-482">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-482">**Why**</span></span>

<span data-ttu-id="44b90-483">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="44b90-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="44b90-484">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-484">**Mitigations**</span></span>

<span data-ttu-id="44b90-485">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="44b90-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="44b90-486">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="44b90-486">This is not common.</span></span>
<span data-ttu-id="44b90-487">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="44b90-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="44b90-488">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="44b90-489">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="44b90-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="44b90-490">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="44b90-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="44b90-491">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-492">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-492">**Old behavior**</span></span>

<span data-ttu-id="44b90-493">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="44b90-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="44b90-494">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="44b90-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="44b90-495">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-495">**New behavior**</span></span>

<span data-ttu-id="44b90-496">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="44b90-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="44b90-497">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-497">**Why**</span></span>

<span data-ttu-id="44b90-498">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="44b90-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="44b90-499">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-499">**Mitigations**</span></span>

<span data-ttu-id="44b90-500">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="44b90-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="44b90-501">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="44b90-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="44b90-502">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="44b90-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="44b90-503">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="44b90-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="44b90-504">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="44b90-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="44b90-505">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="44b90-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="44b90-506">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-506">**Old behavior**</span></span>

<span data-ttu-id="44b90-507">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="44b90-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="44b90-508">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-508">**New behavior**</span></span>

<span data-ttu-id="44b90-509">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="44b90-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="44b90-510">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-510">**Why**</span></span>

<span data-ttu-id="44b90-511">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="44b90-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="44b90-512">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-512">**Mitigations**</span></span>

<span data-ttu-id="44b90-513">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="44b90-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="44b90-514">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="44b90-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="44b90-515">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="44b90-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="44b90-516">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="44b90-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="44b90-517">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-518">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-518">**Old behavior**</span></span>

<span data-ttu-id="44b90-519">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="44b90-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="44b90-520">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-520">**New behavior**</span></span>

<span data-ttu-id="44b90-521">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="44b90-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="44b90-522">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-522">**Why**</span></span>

<span data-ttu-id="44b90-523">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="44b90-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="44b90-524">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="44b90-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="44b90-525">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-525">**Mitigations**</span></span>

<span data-ttu-id="44b90-526">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="44b90-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="44b90-527">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="44b90-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="44b90-528">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="44b90-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="44b90-529">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-530">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-530">**Old behavior**</span></span>

<span data-ttu-id="44b90-531">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="44b90-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="44b90-532">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-532">**New behavior**</span></span>

<span data-ttu-id="44b90-533">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="44b90-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="44b90-534">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="44b90-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="44b90-535">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-535">**Why**</span></span>

<span data-ttu-id="44b90-536">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="44b90-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="44b90-537">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-537">**Mitigations**</span></span>

<span data-ttu-id="44b90-538">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="44b90-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="44b90-539">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="44b90-539">Metadata API changes</span></span>

[<span data-ttu-id="44b90-540">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="44b90-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="44b90-541">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-542">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-542">**New behavior**</span></span>

<span data-ttu-id="44b90-543">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="44b90-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="44b90-544">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-544">**Why**</span></span>

<span data-ttu-id="44b90-545">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="44b90-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="44b90-546">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-546">**Mitigations**</span></span>

<span data-ttu-id="44b90-547">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="44b90-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="44b90-548">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="44b90-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="44b90-549">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="44b90-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="44b90-550">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="44b90-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="44b90-551">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-551">**Old behavior**</span></span>

<span data-ttu-id="44b90-552">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="44b90-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="44b90-553">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-553">**New behavior**</span></span>

<span data-ttu-id="44b90-554">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="44b90-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="44b90-555">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-555">**Why**</span></span>

<span data-ttu-id="44b90-556">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="44b90-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="44b90-557">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-557">**Mitigations**</span></span>

<span data-ttu-id="44b90-558">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="44b90-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="44b90-559">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="44b90-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="44b90-560">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="44b90-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="44b90-561">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-561">**Old behavior**</span></span>

<span data-ttu-id="44b90-562">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="44b90-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="44b90-563">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-563">**New behavior**</span></span>

<span data-ttu-id="44b90-564">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="44b90-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="44b90-565">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-565">**Why**</span></span>

<span data-ttu-id="44b90-566">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="44b90-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="44b90-567">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-567">**Mitigations**</span></span>

<span data-ttu-id="44b90-568">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="44b90-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="44b90-569">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="44b90-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="44b90-570">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="44b90-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="44b90-571">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-572">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-572">**Old behavior**</span></span>

<span data-ttu-id="44b90-573">Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="44b90-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="44b90-574">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-574">**New behavior**</span></span>

<span data-ttu-id="44b90-575">Agora, os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="44b90-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="44b90-576">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-576">**Why**</span></span>

<span data-ttu-id="44b90-577">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="44b90-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="44b90-578">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="44b90-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="44b90-579">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-579">**Mitigations**</span></span>

<span data-ttu-id="44b90-580">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="44b90-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="44b90-581">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="44b90-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="44b90-582">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="44b90-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="44b90-583">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="44b90-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="44b90-584">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="44b90-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="44b90-585">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-586">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-586">**Old behavior**</span></span>

<span data-ttu-id="44b90-587">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="44b90-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="44b90-588">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="44b90-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="44b90-589">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-589">**New behavior**</span></span>

<span data-ttu-id="44b90-590">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="44b90-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="44b90-591">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-591">**Why**</span></span>

<span data-ttu-id="44b90-592">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="44b90-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="44b90-593">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-593">**Mitigations**</span></span>

<span data-ttu-id="44b90-594">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="44b90-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="44b90-595">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="44b90-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="44b90-596">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="44b90-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="44b90-597">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="44b90-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="44b90-598">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="44b90-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="44b90-599">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-600">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-600">**Old behavior**</span></span>

<span data-ttu-id="44b90-601">As IDs de migração eram geradas inadvertidamente usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="44b90-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="44b90-602">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-602">**New behavior**</span></span>

<span data-ttu-id="44b90-603">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="44b90-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="44b90-604">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-604">**Why**</span></span>

<span data-ttu-id="44b90-605">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="44b90-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="44b90-606">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="44b90-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="44b90-607">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-607">**Mitigations**</span></span>

<span data-ttu-id="44b90-608">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="44b90-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="44b90-609">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="44b90-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="44b90-610">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="44b90-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="44b90-611">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="44b90-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="44b90-612">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="44b90-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="44b90-613">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="44b90-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="44b90-614">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-615">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="44b90-615">**Change**</span></span>

<span data-ttu-id="44b90-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="44b90-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="44b90-617">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-617">**Why**</span></span>

<span data-ttu-id="44b90-618">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="44b90-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="44b90-619">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-619">**Mitigations**</span></span>

<span data-ttu-id="44b90-620">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="44b90-620">Use the new name.</span></span> <span data-ttu-id="44b90-621">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="44b90-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="44b90-622">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="44b90-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="44b90-623">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="44b90-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="44b90-624">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="44b90-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="44b90-625">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="44b90-625">**Old behavior**</span></span>

<span data-ttu-id="44b90-626">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="44b90-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="44b90-627">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="44b90-628">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="44b90-628">**New behavior**</span></span>

<span data-ttu-id="44b90-629">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="44b90-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="44b90-630">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="44b90-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="44b90-631">**Por que**</span><span class="sxs-lookup"><span data-stu-id="44b90-631">**Why**</span></span>

<span data-ttu-id="44b90-632">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="44b90-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="44b90-633">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="44b90-633">**Mitigations**</span></span>

<span data-ttu-id="44b90-634">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="44b90-634">Use the new name.</span></span>
