---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7ed55d4cae36f6b25059a5b218db4b0d5e2fb266
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419738"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="79816-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="79816-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79816-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="79816-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="79816-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="79816-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="79816-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="79816-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="79816-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="79816-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="79816-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="79816-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="79816-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="79816-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="79816-109">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-110">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-110">**Old behavior**</span></span>

<span data-ttu-id="79816-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="79816-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="79816-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="79816-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="79816-113">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-113">**New behavior**</span></span>

<span data-ttu-id="79816-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="79816-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="79816-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="79816-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="79816-116">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-116">**Why**</span></span>

<span data-ttu-id="79816-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="79816-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="79816-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="79816-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="79816-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="79816-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="79816-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="79816-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="79816-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="79816-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="79816-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="79816-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="79816-123">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-123">**Mitigations**</span></span>

<span data-ttu-id="79816-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="79816-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="79816-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79816-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="79816-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="79816-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="79816-127">Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="79816-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="79816-128">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-128">**Old behavior**</span></span>

<span data-ttu-id="79816-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="79816-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="79816-130">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-130">**New behavior**</span></span>

<span data-ttu-id="79816-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="79816-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="79816-132">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-132">**Why**</span></span>

<span data-ttu-id="79816-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="79816-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="79816-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="79816-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="79816-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="79816-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="79816-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="79816-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="79816-137">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-137">**Mitigations**</span></span>

<span data-ttu-id="79816-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="79816-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="79816-139">A execução de consulta é registrada no nível de Depuração</span><span class="sxs-lookup"><span data-stu-id="79816-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="79816-140">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="79816-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="79816-141">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-142">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-142">**Old behavior**</span></span>

<span data-ttu-id="79816-143">Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.</span><span class="sxs-lookup"><span data-stu-id="79816-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="79816-144">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-144">**New behavior**</span></span>

<span data-ttu-id="79816-145">A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="79816-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="79816-146">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-146">**Why**</span></span>

<span data-ttu-id="79816-147">Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.</span><span class="sxs-lookup"><span data-stu-id="79816-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="79816-148">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-148">**Mitigations**</span></span>

<span data-ttu-id="79816-149">Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="79816-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="79816-150">Para fazer novamente o log do SQL no nível `Info`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="79816-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="79816-151">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="79816-152">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="79816-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="79816-153">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="79816-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="79816-154">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="79816-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="79816-155">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-155">**Old behavior**</span></span>

<span data-ttu-id="79816-156">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="79816-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="79816-157">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="79816-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="79816-158">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-158">**New behavior**</span></span>

<span data-ttu-id="79816-159">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="79816-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="79816-160">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-160">**Why**</span></span>

<span data-ttu-id="79816-161">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="79816-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="79816-162">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-162">**Mitigations**</span></span>

<span data-ttu-id="79816-163">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="79816-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="79816-164">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="79816-164">This can be avoided by:</span></span>
* <span data-ttu-id="79816-165">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="79816-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="79816-166">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="79816-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="79816-167">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="79816-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="79816-168">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="79816-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="79816-169">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="79816-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="79816-170">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="79816-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="79816-171">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-172">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-172">**Old behavior**</span></span>

<span data-ttu-id="79816-173">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="79816-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="79816-174">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-174">**New behavior**</span></span>

<span data-ttu-id="79816-175">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="79816-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="79816-176">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="79816-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="79816-177">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="79816-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="79816-178">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-178">**Why**</span></span>

<span data-ttu-id="79816-179">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="79816-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="79816-180">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-180">**Mitigations**</span></span>

<span data-ttu-id="79816-181">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="79816-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="79816-182">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="79816-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="79816-183">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="79816-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="79816-184">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="79816-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="79816-185">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="79816-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="79816-186">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="79816-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="79816-187">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-188">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-188">**Old behavior**</span></span>

<span data-ttu-id="79816-189">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="79816-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="79816-190">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-190">**New behavior**</span></span>

<span data-ttu-id="79816-191">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="79816-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="79816-192">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="79816-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="79816-193">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-193">**Why**</span></span>

<span data-ttu-id="79816-194">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="79816-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="79816-195">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-195">**Mitigations**</span></span>

<span data-ttu-id="79816-196">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="79816-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="79816-197">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="79816-198">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="79816-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="79816-199">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="79816-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="79816-200">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-201">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-201">**Old behavior**</span></span>

<span data-ttu-id="79816-202">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="79816-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="79816-203">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="79816-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="79816-204">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-204">**New behavior**</span></span>

<span data-ttu-id="79816-205">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="79816-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="79816-206">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="79816-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="79816-207">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-207">**Why**</span></span>

<span data-ttu-id="79816-208">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="79816-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="79816-209">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="79816-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="79816-210">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="79816-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="79816-211">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-211">**Mitigations**</span></span>

<span data-ttu-id="79816-212">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="79816-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="79816-213">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="79816-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="79816-214">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="79816-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="79816-215">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="79816-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="79816-216">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="79816-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="79816-217">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="79816-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="79816-218">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="79816-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="79816-219">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-220">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-220">**Old behavior**</span></span>

<span data-ttu-id="79816-221">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="79816-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="79816-222">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-222">**New behavior**</span></span>

<span data-ttu-id="79816-223">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="79816-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="79816-224">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="79816-225">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="79816-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="79816-226">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="79816-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="79816-227">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-227">For example:</span></span>

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

<span data-ttu-id="79816-228">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="79816-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="79816-229">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-229">**Why**</span></span>

<span data-ttu-id="79816-230">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="79816-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="79816-231">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="79816-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="79816-232">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-232">**Mitigations**</span></span>

<span data-ttu-id="79816-233">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="79816-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="79816-234">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="79816-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="79816-235">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="79816-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="79816-236">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-237">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-237">**Old behavior**</span></span>

<span data-ttu-id="79816-238">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="79816-238">Consider the following model:</span></span>
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
<span data-ttu-id="79816-239">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="79816-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="79816-240">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="79816-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="79816-241">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-241">**New behavior**</span></span>

<span data-ttu-id="79816-242">A partir da 3.0, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="79816-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="79816-243">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="79816-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="79816-244">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-244">For example:</span></span>

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

<span data-ttu-id="79816-245">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-245">**Why**</span></span>

<span data-ttu-id="79816-246">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="79816-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="79816-247">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-247">**Mitigations**</span></span>

<span data-ttu-id="79816-248">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="79816-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="79816-249">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="79816-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="79816-250">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="79816-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="79816-251">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-252">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-252">**Old behavior**</span></span>

<span data-ttu-id="79816-253">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="79816-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="79816-254">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-254">**New behavior**</span></span>

<span data-ttu-id="79816-255">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="79816-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="79816-256">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="79816-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="79816-257">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-257">**Why**</span></span>

<span data-ttu-id="79816-258">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="79816-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="79816-259">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-259">**Mitigations**</span></span>

<span data-ttu-id="79816-260">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="79816-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="79816-261">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="79816-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="79816-262">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="79816-262">Backing fields are used by default</span></span>

[<span data-ttu-id="79816-263">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="79816-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="79816-264">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="79816-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="79816-265">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-265">**Old behavior**</span></span>

<span data-ttu-id="79816-266">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="79816-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="79816-267">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="79816-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="79816-268">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-268">**New behavior**</span></span>

<span data-ttu-id="79816-269">A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="79816-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="79816-270">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="79816-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="79816-271">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-271">**Why**</span></span>

<span data-ttu-id="79816-272">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="79816-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="79816-273">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-273">**Mitigations**</span></span>

<span data-ttu-id="79816-274">O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="79816-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="79816-275">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="79816-276">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="79816-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="79816-277">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="79816-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="79816-278">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-279">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-279">**Old behavior**</span></span>

<span data-ttu-id="79816-280">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="79816-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="79816-281">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="79816-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="79816-282">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-282">**New behavior**</span></span>

<span data-ttu-id="79816-283">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="79816-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="79816-284">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-284">**Why**</span></span>

<span data-ttu-id="79816-285">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="79816-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="79816-286">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-286">**Mitigations**</span></span>

<span data-ttu-id="79816-287">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="79816-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="79816-288">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="79816-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="79816-289">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="79816-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="79816-290">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="79816-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="79816-291">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-292">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-292">**Old behavior**</span></span>

<span data-ttu-id="79816-293">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="79816-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="79816-294">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-294">**New behavior**</span></span>

<span data-ttu-id="79816-295">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="79816-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="79816-296">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-296">**Why**</span></span>

<span data-ttu-id="79816-297">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="79816-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="79816-298">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="79816-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="79816-299">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-299">**Mitigations**</span></span>

<span data-ttu-id="79816-300">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="79816-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="79816-301">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="79816-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="79816-302">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="79816-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="79816-303">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-304">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-304">**Old behavior**</span></span>

<span data-ttu-id="79816-305">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="79816-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="79816-306">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="79816-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="79816-307">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-307">**New behavior**</span></span>

<span data-ttu-id="79816-308">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="79816-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="79816-309">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="79816-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="79816-310">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="79816-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="79816-311">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="79816-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="79816-312">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-312">**Why**</span></span>

<span data-ttu-id="79816-313">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="79816-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="79816-314">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-314">**Mitigations**</span></span>

<span data-ttu-id="79816-315">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="79816-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="79816-316">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="79816-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="79816-317">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="79816-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="79816-318">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-319">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-319">**Old behavior**</span></span>

<span data-ttu-id="79816-320">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="79816-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="79816-321">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="79816-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="79816-322">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-322">**New behavior**</span></span>

<span data-ttu-id="79816-323">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="79816-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="79816-324">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-324">**Why**</span></span>

<span data-ttu-id="79816-325">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="79816-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="79816-326">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-326">**Mitigations**</span></span>

<span data-ttu-id="79816-327">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="79816-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="79816-328">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="79816-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="79816-329">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="79816-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="79816-330">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="79816-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="79816-331">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="79816-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="79816-332">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-333">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-333">**Old behavior**</span></span>

<span data-ttu-id="79816-334">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="79816-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="79816-335">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-335">**New behavior**</span></span>

<span data-ttu-id="79816-336">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="79816-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="79816-337">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-337">**Why**</span></span>

<span data-ttu-id="79816-338">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="79816-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="79816-339">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-339">**Mitigations**</span></span>

<span data-ttu-id="79816-340">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="79816-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="79816-341">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="79816-341">This isn't common.</span></span>
<span data-ttu-id="79816-342">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="79816-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="79816-343">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="79816-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="79816-344">IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="79816-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="79816-345">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="79816-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="79816-346">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-347">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-347">**Old behavior**</span></span>

<span data-ttu-id="79816-348">`IDbContextOptionsExtensionWithDebugInfo` era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2. x.</span><span class="sxs-lookup"><span data-stu-id="79816-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="79816-349">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-349">**New behavior**</span></span>

<span data-ttu-id="79816-350">As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="79816-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="79816-351">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-351">**Why**</span></span>

<span data-ttu-id="79816-352">Essa alteração foi feita porque as interfaces são conceitualmente uma.</span><span class="sxs-lookup"><span data-stu-id="79816-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="79816-353">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-353">**Mitigations**</span></span>

<span data-ttu-id="79816-354">Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.</span><span class="sxs-lookup"><span data-stu-id="79816-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="79816-355">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="79816-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="79816-356">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="79816-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="79816-357">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-358">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-358">**Old behavior**</span></span>

<span data-ttu-id="79816-359">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="79816-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="79816-360">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="79816-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="79816-361">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="79816-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="79816-362">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-362">**New behavior**</span></span>

<span data-ttu-id="79816-363">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="79816-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="79816-364">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="79816-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="79816-365">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="79816-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="79816-366">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="79816-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="79816-367">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-367">**Why**</span></span>

<span data-ttu-id="79816-368">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="79816-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="79816-369">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-369">**Mitigations**</span></span>

<span data-ttu-id="79816-370">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="79816-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="79816-371">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="79816-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="79816-372">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="79816-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="79816-373">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-374">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-374">**Old behavior**</span></span>

<span data-ttu-id="79816-375">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="79816-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="79816-376">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-376">**New behavior**</span></span>

<span data-ttu-id="79816-377">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="79816-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="79816-378">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-378">**Why**</span></span>

<span data-ttu-id="79816-379">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="79816-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="79816-380">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-380">**Mitigations**</span></span>

<span data-ttu-id="79816-381">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="79816-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="79816-382">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="79816-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="79816-383">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="79816-384">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="79816-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="79816-385">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="79816-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="79816-386">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-387">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-387">**Old behavior**</span></span>

<span data-ttu-id="79816-388">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="79816-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="79816-389">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="79816-390">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="79816-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="79816-391">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="79816-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="79816-392">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-392">**New behavior**</span></span>

<span data-ttu-id="79816-393">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="79816-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="79816-394">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-394">**Why**</span></span>

<span data-ttu-id="79816-395">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="79816-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="79816-396">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-396">**Mitigations**</span></span>

<span data-ttu-id="79816-397">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="79816-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="79816-398">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="79816-398">This is not common.</span></span>
<span data-ttu-id="79816-399">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="79816-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="79816-400">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="79816-401">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="79816-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="79816-402">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="79816-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="79816-403">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="79816-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="79816-404">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-404">**Old behavior**</span></span>

<span data-ttu-id="79816-405">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="79816-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="79816-406">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-406">**New behavior**</span></span>

<span data-ttu-id="79816-407">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="79816-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="79816-408">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-408">**Why**</span></span>

<span data-ttu-id="79816-409">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="79816-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="79816-410">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-410">**Mitigations**</span></span>

<span data-ttu-id="79816-411">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="79816-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="79816-412">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="79816-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="79816-413">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="79816-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="79816-414">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="79816-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="79816-415">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-416">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-416">**Old behavior**</span></span>

<span data-ttu-id="79816-417">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="79816-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="79816-418">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-418">**New behavior**</span></span>

<span data-ttu-id="79816-419">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="79816-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="79816-420">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-420">**Why**</span></span>

<span data-ttu-id="79816-421">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="79816-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="79816-422">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="79816-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="79816-423">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-423">**Mitigations**</span></span>

<span data-ttu-id="79816-424">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="79816-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="79816-425">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="79816-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="79816-426">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="79816-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="79816-427">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-428">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-428">**Old behavior**</span></span>

<span data-ttu-id="79816-429">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="79816-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="79816-430">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-430">**New behavior**</span></span>

<span data-ttu-id="79816-431">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="79816-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="79816-432">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="79816-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="79816-433">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-433">**Why**</span></span>

<span data-ttu-id="79816-434">Essa alteração foi feita para consolidar a API para índices com `Includes` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="79816-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="79816-435">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-435">**Mitigations**</span></span>

<span data-ttu-id="79816-436">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="79816-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="79816-437">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="79816-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="79816-438">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="79816-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="79816-439">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="79816-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="79816-440">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-440">**Old behavior**</span></span>

<span data-ttu-id="79816-441">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="79816-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="79816-442">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-442">**New behavior**</span></span>

<span data-ttu-id="79816-443">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="79816-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="79816-444">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-444">**Why**</span></span>

<span data-ttu-id="79816-445">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="79816-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="79816-446">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-446">**Mitigations**</span></span>

<span data-ttu-id="79816-447">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="79816-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="79816-448">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="79816-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="79816-449">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="79816-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="79816-450">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-450">**Old behavior**</span></span>

<span data-ttu-id="79816-451">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="79816-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="79816-452">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-452">**New behavior**</span></span>

<span data-ttu-id="79816-453">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="79816-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="79816-454">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-454">**Why**</span></span>

<span data-ttu-id="79816-455">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="79816-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="79816-456">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-456">**Mitigations**</span></span>

<span data-ttu-id="79816-457">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="79816-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="79816-458">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="79816-458">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="79816-459">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="79816-459">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="79816-460">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-461">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-461">**Old behavior**</span></span>

<span data-ttu-id="79816-462">Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="79816-462">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="79816-463">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-463">**New behavior**</span></span>

<span data-ttu-id="79816-464">Agora, os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="79816-464">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="79816-465">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-465">**Why**</span></span>

<span data-ttu-id="79816-466">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="79816-466">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="79816-467">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="79816-467">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="79816-468">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-468">**Mitigations**</span></span>

<span data-ttu-id="79816-469">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="79816-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="79816-470">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="79816-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="79816-471">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="79816-471">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="79816-472">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="79816-472">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="79816-473">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="79816-473">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="79816-474">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-475">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-475">**Old behavior**</span></span>

<span data-ttu-id="79816-476">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="79816-476">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="79816-477">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="79816-477">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="79816-478">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-478">**New behavior**</span></span>

<span data-ttu-id="79816-479">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="79816-479">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="79816-480">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-480">**Why**</span></span>

<span data-ttu-id="79816-481">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="79816-481">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="79816-482">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-482">**Mitigations**</span></span>

<span data-ttu-id="79816-483">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="79816-483">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="79816-484">No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="79816-484">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="79816-485">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="79816-485">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="79816-486">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="79816-486">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="79816-487">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="79816-487">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="79816-488">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-488">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-489">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-489">**Old behavior**</span></span>

<span data-ttu-id="79816-490">As IDs de migração eram geradas inadvertidamente usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="79816-490">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="79816-491">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-491">**New behavior**</span></span>

<span data-ttu-id="79816-492">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="79816-492">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="79816-493">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-493">**Why**</span></span>

<span data-ttu-id="79816-494">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="79816-494">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="79816-495">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="79816-495">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="79816-496">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-496">**Mitigations**</span></span>

<span data-ttu-id="79816-497">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="79816-497">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="79816-498">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="79816-498">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="79816-499">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="79816-499">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="79816-500">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="79816-500">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="79816-501">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="79816-501">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="79816-502">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="79816-502">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="79816-503">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-503">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-504">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="79816-504">**Change**</span></span>

<span data-ttu-id="79816-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="79816-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="79816-506">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-506">**Why**</span></span>

<span data-ttu-id="79816-507">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="79816-507">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="79816-508">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-508">**Mitigations**</span></span>

<span data-ttu-id="79816-509">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="79816-509">Use the new name.</span></span> <span data-ttu-id="79816-510">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="79816-510">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="79816-511">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="79816-511">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="79816-512">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="79816-512">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="79816-513">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="79816-513">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="79816-514">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="79816-514">**Old behavior**</span></span>

<span data-ttu-id="79816-515">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="79816-515">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="79816-516">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-516">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="79816-517">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="79816-517">**New behavior**</span></span>

<span data-ttu-id="79816-518">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="79816-518">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="79816-519">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="79816-519">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="79816-520">**Por que**</span><span class="sxs-lookup"><span data-stu-id="79816-520">**Why**</span></span>

<span data-ttu-id="79816-521">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="79816-521">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="79816-522">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="79816-522">**Mitigations**</span></span>

<span data-ttu-id="79816-523">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="79816-523">Use the new name.</span></span>
