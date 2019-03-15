---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 748db8a71a04a2d696ef21a03319906b9fc776be
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829220"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="5707f-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="5707f-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5707f-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="5707f-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="5707f-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="5707f-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="5707f-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="5707f-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="5707f-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="5707f-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="5707f-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="5707f-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="5707f-108">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="5707f-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="5707f-109">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-110">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-110">**Old behavior**</span></span>

<span data-ttu-id="5707f-111">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="5707f-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="5707f-112">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="5707f-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="5707f-113">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-113">**New behavior**</span></span>

<span data-ttu-id="5707f-114">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="5707f-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="5707f-115">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="5707f-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="5707f-116">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-116">**Why**</span></span>

<span data-ttu-id="5707f-117">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="5707f-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="5707f-118">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="5707f-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="5707f-119">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="5707f-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="5707f-120">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="5707f-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="5707f-121">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5707f-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="5707f-122">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="5707f-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="5707f-123">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-123">**Mitigations**</span></span>

<span data-ttu-id="5707f-124">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="5707f-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="5707f-125">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5707f-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="5707f-126">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="5707f-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="5707f-127">Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="5707f-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="5707f-128">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-128">**Old behavior**</span></span>

<span data-ttu-id="5707f-129">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5707f-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="5707f-130">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-130">**New behavior**</span></span>

<span data-ttu-id="5707f-131">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5707f-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="5707f-132">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-132">**Why**</span></span>

<span data-ttu-id="5707f-133">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="5707f-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="5707f-134">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="5707f-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="5707f-135">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="5707f-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="5707f-136">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="5707f-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="5707f-137">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-137">**Mitigations**</span></span>

<span data-ttu-id="5707f-138">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="5707f-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="5707f-139">A execução de consulta é registrada no nível de Depuração</span><span class="sxs-lookup"><span data-stu-id="5707f-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="5707f-140">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="5707f-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="5707f-141">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-142">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-142">**Old behavior**</span></span>

<span data-ttu-id="5707f-143">Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.</span><span class="sxs-lookup"><span data-stu-id="5707f-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="5707f-144">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-144">**New behavior**</span></span>

<span data-ttu-id="5707f-145">A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="5707f-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="5707f-146">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-146">**Why**</span></span>

<span data-ttu-id="5707f-147">Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.</span><span class="sxs-lookup"><span data-stu-id="5707f-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="5707f-148">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-148">**Mitigations**</span></span>

<span data-ttu-id="5707f-149">Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="5707f-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="5707f-150">Para fazer novamente o log do SQL no nível `Info`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="5707f-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="5707f-151">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="5707f-152">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="5707f-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="5707f-153">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="5707f-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="5707f-154">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="5707f-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5707f-155">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-155">**Old behavior**</span></span>

<span data-ttu-id="5707f-156">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5707f-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="5707f-157">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="5707f-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="5707f-158">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-158">**New behavior**</span></span>

<span data-ttu-id="5707f-159">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="5707f-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="5707f-160">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-160">**Why**</span></span>

<span data-ttu-id="5707f-161">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5707f-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="5707f-162">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-162">**Mitigations**</span></span>

<span data-ttu-id="5707f-163">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="5707f-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="5707f-164">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="5707f-164">This can be avoided by:</span></span>
* <span data-ttu-id="5707f-165">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="5707f-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="5707f-166">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="5707f-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="5707f-167">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="5707f-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="5707f-168">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="5707f-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="5707f-169">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="5707f-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="5707f-170">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="5707f-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="5707f-171">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-172">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-172">**Old behavior**</span></span>

<span data-ttu-id="5707f-173">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="5707f-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="5707f-174">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-174">**New behavior**</span></span>

<span data-ttu-id="5707f-175">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="5707f-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="5707f-176">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="5707f-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="5707f-177">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="5707f-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="5707f-178">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-178">**Why**</span></span>

<span data-ttu-id="5707f-179">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="5707f-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="5707f-180">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-180">**Mitigations**</span></span>

<span data-ttu-id="5707f-181">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="5707f-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="5707f-182">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="5707f-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="5707f-183">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5707f-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="5707f-184">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="5707f-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="5707f-185">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="5707f-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="5707f-186">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="5707f-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="5707f-187">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-188">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-188">**Old behavior**</span></span>

<span data-ttu-id="5707f-189">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="5707f-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="5707f-190">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-190">**New behavior**</span></span>

<span data-ttu-id="5707f-191">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="5707f-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="5707f-192">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="5707f-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="5707f-193">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-193">**Why**</span></span>

<span data-ttu-id="5707f-194">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="5707f-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="5707f-195">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-195">**Mitigations**</span></span>

<span data-ttu-id="5707f-196">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="5707f-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="5707f-197">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="5707f-198">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="5707f-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="5707f-199">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="5707f-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="5707f-200">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-201">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-201">**Old behavior**</span></span>

<span data-ttu-id="5707f-202">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="5707f-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="5707f-203">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="5707f-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="5707f-204">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-204">**New behavior**</span></span>

<span data-ttu-id="5707f-205">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="5707f-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="5707f-206">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="5707f-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="5707f-207">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-207">**Why**</span></span>

<span data-ttu-id="5707f-208">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="5707f-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="5707f-209">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="5707f-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="5707f-210">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="5707f-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="5707f-211">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-211">**Mitigations**</span></span>

<span data-ttu-id="5707f-212">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="5707f-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="5707f-213">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="5707f-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="5707f-214">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="5707f-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="5707f-215">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="5707f-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="5707f-216">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="5707f-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="5707f-217">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="5707f-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="5707f-218">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="5707f-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="5707f-219">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-220">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-220">**Old behavior**</span></span>

<span data-ttu-id="5707f-221">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="5707f-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="5707f-222">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-222">**New behavior**</span></span>

<span data-ttu-id="5707f-223">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="5707f-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="5707f-224">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="5707f-225">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="5707f-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="5707f-226">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="5707f-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="5707f-227">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-227">For example:</span></span>

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

<span data-ttu-id="5707f-228">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="5707f-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="5707f-229">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-229">**Why**</span></span>

<span data-ttu-id="5707f-230">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="5707f-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="5707f-231">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="5707f-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="5707f-232">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-232">**Mitigations**</span></span>

<span data-ttu-id="5707f-233">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="5707f-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="5707f-234">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="5707f-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="5707f-235">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="5707f-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="5707f-236">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-237">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-237">**Old behavior**</span></span>

<span data-ttu-id="5707f-238">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5707f-238">Consider the following model:</span></span>
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
<span data-ttu-id="5707f-239">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="5707f-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="5707f-240">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="5707f-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="5707f-241">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-241">**New behavior**</span></span>

<span data-ttu-id="5707f-242">A partir da 3.0, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="5707f-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="5707f-243">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="5707f-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="5707f-244">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-244">For example:</span></span>

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

<span data-ttu-id="5707f-245">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-245">**Why**</span></span>

<span data-ttu-id="5707f-246">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="5707f-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="5707f-247">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-247">**Mitigations**</span></span>

<span data-ttu-id="5707f-248">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="5707f-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="5707f-249">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="5707f-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="5707f-250">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="5707f-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="5707f-251">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-252">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-252">**Old behavior**</span></span>

<span data-ttu-id="5707f-253">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="5707f-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="5707f-254">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-254">**New behavior**</span></span>

<span data-ttu-id="5707f-255">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="5707f-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="5707f-256">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="5707f-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="5707f-257">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-257">**Why**</span></span>

<span data-ttu-id="5707f-258">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="5707f-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="5707f-259">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-259">**Mitigations**</span></span>

<span data-ttu-id="5707f-260">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="5707f-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="5707f-261">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="5707f-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="5707f-262">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="5707f-262">Backing fields are used by default</span></span>

[<span data-ttu-id="5707f-263">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="5707f-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="5707f-264">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="5707f-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5707f-265">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-265">**Old behavior**</span></span>

<span data-ttu-id="5707f-266">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="5707f-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="5707f-267">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="5707f-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="5707f-268">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-268">**New behavior**</span></span>

<span data-ttu-id="5707f-269">A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="5707f-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="5707f-270">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="5707f-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="5707f-271">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-271">**Why**</span></span>

<span data-ttu-id="5707f-272">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="5707f-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="5707f-273">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-273">**Mitigations**</span></span>

<span data-ttu-id="5707f-274">O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="5707f-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="5707f-275">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="5707f-276">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="5707f-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="5707f-277">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="5707f-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="5707f-278">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-279">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-279">**Old behavior**</span></span>

<span data-ttu-id="5707f-280">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="5707f-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="5707f-281">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="5707f-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="5707f-282">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-282">**New behavior**</span></span>

<span data-ttu-id="5707f-283">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="5707f-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="5707f-284">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-284">**Why**</span></span>

<span data-ttu-id="5707f-285">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="5707f-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="5707f-286">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-286">**Mitigations**</span></span>

<span data-ttu-id="5707f-287">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="5707f-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="5707f-288">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5707f-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="5707f-289">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="5707f-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="5707f-290">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="5707f-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="5707f-291">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-292">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-292">**Old behavior**</span></span>

<span data-ttu-id="5707f-293">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5707f-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="5707f-294">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-294">**New behavior**</span></span>

<span data-ttu-id="5707f-295">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="5707f-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="5707f-296">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-296">**Why**</span></span>

<span data-ttu-id="5707f-297">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5707f-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="5707f-298">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="5707f-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="5707f-299">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-299">**Mitigations**</span></span>

<span data-ttu-id="5707f-300">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5707f-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="5707f-301">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="5707f-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="5707f-302">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="5707f-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5707f-303">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-304">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-304">**Old behavior**</span></span>

<span data-ttu-id="5707f-305">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="5707f-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="5707f-306">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="5707f-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="5707f-307">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-307">**New behavior**</span></span>

<span data-ttu-id="5707f-308">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="5707f-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="5707f-309">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5707f-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="5707f-310">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="5707f-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="5707f-311">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="5707f-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="5707f-312">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-312">**Why**</span></span>

<span data-ttu-id="5707f-313">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="5707f-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="5707f-314">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-314">**Mitigations**</span></span>

<span data-ttu-id="5707f-315">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="5707f-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="5707f-316">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="5707f-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="5707f-317">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="5707f-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="5707f-318">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-319">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-319">**Old behavior**</span></span>

<span data-ttu-id="5707f-320">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="5707f-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="5707f-321">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5707f-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="5707f-322">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-322">**New behavior**</span></span>

<span data-ttu-id="5707f-323">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="5707f-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="5707f-324">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-324">**Why**</span></span>

<span data-ttu-id="5707f-325">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="5707f-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="5707f-326">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-326">**Mitigations**</span></span>

<span data-ttu-id="5707f-327">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="5707f-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="5707f-328">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5707f-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="5707f-329">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="5707f-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="5707f-330">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="5707f-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="5707f-331">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="5707f-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="5707f-332">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-333">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-333">**Old behavior**</span></span>

<span data-ttu-id="5707f-334">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="5707f-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="5707f-335">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-335">**New behavior**</span></span>

<span data-ttu-id="5707f-336">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="5707f-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="5707f-337">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-337">**Why**</span></span>

<span data-ttu-id="5707f-338">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="5707f-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="5707f-339">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-339">**Mitigations**</span></span>

<span data-ttu-id="5707f-340">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5707f-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="5707f-341">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="5707f-341">This isn't common.</span></span>
<span data-ttu-id="5707f-342">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="5707f-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="5707f-343">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="5707f-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="5707f-344">IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="5707f-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="5707f-345">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="5707f-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5707f-346">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-347">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-347">**Old behavior**</span></span>

<span data-ttu-id="5707f-348">`IDbContextOptionsExtensionWithDebugInfo` era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2. x.</span><span class="sxs-lookup"><span data-stu-id="5707f-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="5707f-349">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-349">**New behavior**</span></span>

<span data-ttu-id="5707f-350">As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="5707f-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="5707f-351">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-351">**Why**</span></span>

<span data-ttu-id="5707f-352">Essa alteração foi feita porque as interfaces são conceitualmente uma.</span><span class="sxs-lookup"><span data-stu-id="5707f-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="5707f-353">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-353">**Mitigations**</span></span>

<span data-ttu-id="5707f-354">Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.</span><span class="sxs-lookup"><span data-stu-id="5707f-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="5707f-355">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="5707f-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="5707f-356">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="5707f-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="5707f-357">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-358">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-358">**Old behavior**</span></span>

<span data-ttu-id="5707f-359">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="5707f-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="5707f-360">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="5707f-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="5707f-361">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="5707f-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="5707f-362">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-362">**New behavior**</span></span>

<span data-ttu-id="5707f-363">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="5707f-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="5707f-364">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="5707f-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="5707f-365">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="5707f-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="5707f-366">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="5707f-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="5707f-367">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-367">**Why**</span></span>

<span data-ttu-id="5707f-368">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="5707f-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="5707f-369">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-369">**Mitigations**</span></span>

<span data-ttu-id="5707f-370">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="5707f-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="5707f-371">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="5707f-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="5707f-372">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="5707f-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="5707f-373">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-374">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-374">**Old behavior**</span></span>

<span data-ttu-id="5707f-375">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="5707f-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="5707f-376">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-376">**New behavior**</span></span>

<span data-ttu-id="5707f-377">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="5707f-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="5707f-378">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-378">**Why**</span></span>

<span data-ttu-id="5707f-379">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5707f-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="5707f-380">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-380">**Mitigations**</span></span>

<span data-ttu-id="5707f-381">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="5707f-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="5707f-382">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5707f-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="5707f-383">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="5707f-384">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5707f-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="5707f-385">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="5707f-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="5707f-386">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="5707f-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="5707f-387">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-387">**Old behavior**</span></span>

<span data-ttu-id="5707f-388">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="5707f-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="5707f-389">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="5707f-390">O código parece estar relacionando `Samuri` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="5707f-390">The code looks like it is relating `Samuri` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="5707f-391">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="5707f-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="5707f-392">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-392">**New behavior**</span></span>

<span data-ttu-id="5707f-393">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="5707f-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="5707f-394">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-394">**Why**</span></span>

<span data-ttu-id="5707f-395">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="5707f-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="5707f-396">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-396">**Mitigations**</span></span>

<span data-ttu-id="5707f-397">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5707f-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="5707f-398">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="5707f-398">This is not common.</span></span>
<span data-ttu-id="5707f-399">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="5707f-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="5707f-400">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5707f-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="5707f-401">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5707f-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="5707f-402">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="5707f-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="5707f-403">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="5707f-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="5707f-404">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-404">**Old behavior**</span></span>

<span data-ttu-id="5707f-405">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5707f-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="5707f-406">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-406">**New behavior**</span></span>

<span data-ttu-id="5707f-407">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5707f-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="5707f-408">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-408">**Why**</span></span>

<span data-ttu-id="5707f-409">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="5707f-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="5707f-410">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-410">**Mitigations**</span></span>

<span data-ttu-id="5707f-411">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="5707f-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="5707f-412">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="5707f-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="5707f-413">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="5707f-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="5707f-414">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="5707f-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="5707f-415">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-416">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-416">**Old behavior**</span></span>

<span data-ttu-id="5707f-417">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="5707f-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="5707f-418">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-418">**New behavior**</span></span>

<span data-ttu-id="5707f-419">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="5707f-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="5707f-420">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-420">**Why**</span></span>

<span data-ttu-id="5707f-421">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="5707f-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="5707f-422">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="5707f-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="5707f-423">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-423">**Mitigations**</span></span>

<span data-ttu-id="5707f-424">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="5707f-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="5707f-425">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="5707f-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="5707f-426">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="5707f-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="5707f-427">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-428">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-428">**Old behavior**</span></span>

<span data-ttu-id="5707f-429">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="5707f-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="5707f-430">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-430">**New behavior**</span></span>

<span data-ttu-id="5707f-431">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="5707f-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="5707f-432">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="5707f-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="5707f-433">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-433">**Why**</span></span>

<span data-ttu-id="5707f-434">Essa alteração foi feita para consolidar a API para índices com `Includes` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5707f-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="5707f-435">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-435">**Mitigations**</span></span>

<span data-ttu-id="5707f-436">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="5707f-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="5707f-437">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="5707f-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="5707f-438">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="5707f-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="5707f-439">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="5707f-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="5707f-440">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-440">**Old behavior**</span></span>

<span data-ttu-id="5707f-441">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="5707f-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5707f-442">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-442">**New behavior**</span></span>

<span data-ttu-id="5707f-443">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="5707f-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5707f-444">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-444">**Why**</span></span>

<span data-ttu-id="5707f-445">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="5707f-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="5707f-446">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-446">**Mitigations**</span></span>

<span data-ttu-id="5707f-447">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="5707f-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="5707f-448">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="5707f-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="5707f-449">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="5707f-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="5707f-450">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5707f-450">**Old behavior**</span></span>

<span data-ttu-id="5707f-451">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="5707f-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="5707f-452">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5707f-452">**New behavior**</span></span>

<span data-ttu-id="5707f-453">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="5707f-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="5707f-454">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5707f-454">**Why**</span></span>

<span data-ttu-id="5707f-455">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="5707f-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="5707f-456">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5707f-456">**Mitigations**</span></span>

<span data-ttu-id="5707f-457">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="5707f-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
