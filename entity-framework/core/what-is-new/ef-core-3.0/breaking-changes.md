---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: a7e1a03bf1131cd53123f5cc39b07bed94619b44
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463352"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1ce9e-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="1ce9e-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1ce9e-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1ce9e-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1ce9e-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1ce9e-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1ce9e-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1ce9e-107">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="1ce9e-107">LINQ queries are no longer evaluated on the client</span></span>

[<span data-ttu-id="1ce9e-108">Acompanhamento de problema nº 12795</span><span class="sxs-lookup"><span data-stu-id="1ce9e-108">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

> [!IMPORTANT]
> <span data-ttu-id="1ce9e-109">Estamos pré-anunciando essa interrupção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-109">We are pre-announcing this break.</span></span>
<span data-ttu-id="1ce9e-110">Ela ainda não foi enviada em nenhuma versão prévia 3.0.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-110">It has not yet shipped in any 3.0 preview.</span></span>

<span data-ttu-id="1ce9e-111">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-111">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-112">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-112">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1ce9e-113">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-113">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1ce9e-114">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-114">**New behavior**</span></span>

<span data-ttu-id="1ce9e-115">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-115">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1ce9e-116">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-116">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1ce9e-117">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-117">**Why**</span></span>

<span data-ttu-id="1ce9e-118">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-118">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1ce9e-119">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-119">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1ce9e-120">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-120">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1ce9e-121">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-121">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1ce9e-122">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-122">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1ce9e-123">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-123">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1ce9e-124">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-124">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-125">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-125">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1ce9e-126">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ce9e-126">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1ce9e-127">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="1ce9e-127">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1ce9e-128">Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-128">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1ce9e-129">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-129">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-130">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-130">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1ce9e-131">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-131">**New behavior**</span></span>

<span data-ttu-id="1ce9e-132">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-132">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1ce9e-133">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-133">**Why**</span></span>

<span data-ttu-id="1ce9e-134">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-134">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1ce9e-135">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-135">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1ce9e-136">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-136">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1ce9e-137">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-137">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1ce9e-138">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-138">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-139">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-139">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="1ce9e-140">A execução de consulta é registrada no nível de Depuração</span><span class="sxs-lookup"><span data-stu-id="1ce9e-140">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="1ce9e-141">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="1ce9e-141">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1ce9e-142">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-142">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-143">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-143">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-144">Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-144">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="1ce9e-145">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-145">**New behavior**</span></span>

<span data-ttu-id="1ce9e-146">A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-146">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="1ce9e-147">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-147">**Why**</span></span>

<span data-ttu-id="1ce9e-148">Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-148">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="1ce9e-149">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-149">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-150">Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-150">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="1ce9e-151">Para registrar em log o SQL no nível `Info` novamente, ative o registro em log no nível `Debug` e filtre apenas esse evento.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-151">To log SQL at the `Info` level again, switch on logging at the `Debug` level and filter to just this event.</span></span>


## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1ce9e-152">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="1ce9e-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1ce9e-153">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="1ce9e-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1ce9e-154">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1ce9e-155">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-155">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-156">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1ce9e-157">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1ce9e-158">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-158">**New behavior**</span></span>

<span data-ttu-id="1ce9e-159">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1ce9e-160">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-160">**Why**</span></span>

<span data-ttu-id="1ce9e-161">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1ce9e-162">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-162">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-163">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1ce9e-164">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-164">This can be avoided by:</span></span>
* <span data-ttu-id="1ce9e-165">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="1ce9e-166">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1ce9e-167">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1ce9e-168">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1ce9e-169">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="1ce9e-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1ce9e-170">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="1ce9e-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1ce9e-171">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-172">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-172">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-173">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1ce9e-174">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-174">**New behavior**</span></span>

<span data-ttu-id="1ce9e-175">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1ce9e-176">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1ce9e-177">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1ce9e-178">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-178">**Why**</span></span>

<span data-ttu-id="1ce9e-179">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1ce9e-180">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-180">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-181">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1ce9e-182">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1ce9e-183">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1ce9e-184">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1ce9e-185">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="1ce9e-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1ce9e-186">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="1ce9e-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1ce9e-187">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-188">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-188">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-189">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1ce9e-190">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-190">**New behavior**</span></span>

<span data-ttu-id="1ce9e-191">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1ce9e-192">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1ce9e-193">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-193">**Why**</span></span>

<span data-ttu-id="1ce9e-194">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1ce9e-195">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-195">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-196">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1ce9e-197">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1ce9e-198">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="1ce9e-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1ce9e-199">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="1ce9e-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1ce9e-200">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-201">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-201">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-202">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1ce9e-203">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="1ce9e-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1ce9e-204">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-204">**New behavior**</span></span>

<span data-ttu-id="1ce9e-205">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1ce9e-206">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1ce9e-207">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-207">**Why**</span></span>

<span data-ttu-id="1ce9e-208">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1ce9e-209">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1ce9e-210">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1ce9e-211">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-211">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-212">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1ce9e-213">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1ce9e-214">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1ce9e-215">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1ce9e-216">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1ce9e-217">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="1ce9e-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1ce9e-218">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1ce9e-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1ce9e-219">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-220">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-220">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-221">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1ce9e-222">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-222">**New behavior**</span></span>

<span data-ttu-id="1ce9e-223">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1ce9e-224">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1ce9e-225">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1ce9e-226">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1ce9e-227">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-227">For example:</span></span>

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

<span data-ttu-id="1ce9e-228">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1ce9e-229">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-229">**Why**</span></span>

<span data-ttu-id="1ce9e-230">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1ce9e-231">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1ce9e-232">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-232">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-233">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1ce9e-234">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="1ce9e-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1ce9e-235">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="1ce9e-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1ce9e-236">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-237">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-237">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-238">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-238">Consider the following model:</span></span>
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
<span data-ttu-id="1ce9e-239">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1ce9e-240">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1ce9e-241">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-241">**New behavior**</span></span>

<span data-ttu-id="1ce9e-242">A partir da 3.0, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1ce9e-243">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1ce9e-244">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-244">For example:</span></span>

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

<span data-ttu-id="1ce9e-245">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-245">**Why**</span></span>

<span data-ttu-id="1ce9e-246">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1ce9e-247">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-247">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-248">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1ce9e-249">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="1ce9e-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1ce9e-250">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="1ce9e-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1ce9e-251">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1ce9e-252">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-252">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-253">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1ce9e-254">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-254">**New behavior**</span></span>

<span data-ttu-id="1ce9e-255">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1ce9e-256">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1ce9e-257">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-257">**Why**</span></span>

<span data-ttu-id="1ce9e-258">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1ce9e-259">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-259">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-260">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1ce9e-261">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1ce9e-262">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="1ce9e-262">Backing fields are used by default</span></span>

[<span data-ttu-id="1ce9e-263">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="1ce9e-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1ce9e-264">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1ce9e-265">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-265">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-266">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1ce9e-267">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1ce9e-268">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-268">**New behavior**</span></span>

<span data-ttu-id="1ce9e-269">A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1ce9e-270">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1ce9e-271">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-271">**Why**</span></span>

<span data-ttu-id="1ce9e-272">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1ce9e-273">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-273">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-274">O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="1ce9e-275">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1ce9e-276">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="1ce9e-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1ce9e-277">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="1ce9e-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1ce9e-278">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1ce9e-279">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-279">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-280">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1ce9e-281">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1ce9e-282">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-282">**New behavior**</span></span>

<span data-ttu-id="1ce9e-283">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1ce9e-284">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-284">**Why**</span></span>

<span data-ttu-id="1ce9e-285">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1ce9e-286">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-286">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-287">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1ce9e-288">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1ce9e-289">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="1ce9e-289">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1ce9e-290">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="1ce9e-290">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1ce9e-291">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-291">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-292">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-292">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-293">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-293">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1ce9e-294">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-294">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1ce9e-295">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-295">**New behavior**</span></span>

<span data-ttu-id="1ce9e-296">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-296">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1ce9e-297">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-297">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1ce9e-298">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-298">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1ce9e-299">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-299">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1ce9e-300">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-300">**Why**</span></span>

<span data-ttu-id="1ce9e-301">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-301">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1ce9e-302">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-302">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-303">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-303">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1ce9e-304">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="1ce9e-304">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1ce9e-305">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="1ce9e-305">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1ce9e-306">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-306">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1ce9e-307">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-307">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-308">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-308">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1ce9e-309">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-309">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1ce9e-310">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-310">**New behavior**</span></span>

<span data-ttu-id="1ce9e-311">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-311">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1ce9e-312">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-312">**Why**</span></span>

<span data-ttu-id="1ce9e-313">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-313">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1ce9e-314">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-314">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-315">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-315">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1ce9e-316">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-316">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1ce9e-317">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-317">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1ce9e-318">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="1ce9e-318">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1ce9e-319">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="1ce9e-319">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1ce9e-320">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-320">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-321">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-321">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-322">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-322">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1ce9e-323">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-323">**New behavior**</span></span>

<span data-ttu-id="1ce9e-324">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-324">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1ce9e-325">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-325">**Why**</span></span>

<span data-ttu-id="1ce9e-326">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-326">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1ce9e-327">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-327">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-328">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-328">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1ce9e-329">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-329">This isn't common.</span></span>
<span data-ttu-id="1ce9e-330">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-330">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1ce9e-331">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-331">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="1ce9e-332">IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1ce9e-332">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1ce9e-333">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="1ce9e-333">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1ce9e-334">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-334">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-335">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-335">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-336">`IDbContextOptionsExtensionWithDebugInfo` era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2. x.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-336">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="1ce9e-337">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-337">**New behavior**</span></span>

<span data-ttu-id="1ce9e-338">As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-338">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="1ce9e-339">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-339">**Why**</span></span>

<span data-ttu-id="1ce9e-340">Essa alteração foi feita porque as interfaces são conceitualmente uma.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-340">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="1ce9e-341">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-341">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-342">Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-342">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1ce9e-343">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="1ce9e-343">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1ce9e-344">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="1ce9e-344">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1ce9e-345">Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-345">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1ce9e-346">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-346">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-347">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-347">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1ce9e-348">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-348">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1ce9e-349">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-349">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1ce9e-350">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-350">**New behavior**</span></span>

<span data-ttu-id="1ce9e-351">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-351">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1ce9e-352">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-352">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1ce9e-353">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-353">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1ce9e-354">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-354">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1ce9e-355">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-355">**Why**</span></span>

<span data-ttu-id="1ce9e-356">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-356">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1ce9e-357">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-357">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-358">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-358">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1ce9e-359">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="1ce9e-359">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1ce9e-360">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="1ce9e-360">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1ce9e-361">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-361">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-362">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-362">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-363">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-363">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1ce9e-364">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-364">**New behavior**</span></span>

<span data-ttu-id="1ce9e-365">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-365">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1ce9e-366">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-366">**Why**</span></span>

<span data-ttu-id="1ce9e-367">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-367">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1ce9e-368">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-368">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-369">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-369">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1ce9e-370">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-370">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1ce9e-371">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="1ce9e-371">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1ce9e-372">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1ce9e-372">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1ce9e-373">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="1ce9e-373">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1ce9e-374">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-374">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1ce9e-375">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-375">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-376">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1ce9e-376">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1ce9e-377">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-377">**New behavior**</span></span>

<span data-ttu-id="1ce9e-378">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1ce9e-378">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1ce9e-379">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-379">**Why**</span></span>

<span data-ttu-id="1ce9e-380">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-380">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1ce9e-381">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-381">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-382">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-382">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1ce9e-383">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-383">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1ce9e-384">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="1ce9e-384">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1ce9e-385">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="1ce9e-385">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1ce9e-386">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-386">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-387">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-387">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-388">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-388">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1ce9e-389">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-389">**New behavior**</span></span>

<span data-ttu-id="1ce9e-390">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-390">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1ce9e-391">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-391">**Why**</span></span>

<span data-ttu-id="1ce9e-392">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-392">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1ce9e-393">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-393">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1ce9e-394">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-394">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-395">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-395">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1ce9e-396">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="1ce9e-396">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1ce9e-397">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="1ce9e-397">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1ce9e-398">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-399">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-399">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-400">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-400">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1ce9e-401">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-401">**New behavior**</span></span>

<span data-ttu-id="1ce9e-402">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-402">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1ce9e-403">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-403">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1ce9e-404">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-404">**Why**</span></span>

<span data-ttu-id="1ce9e-405">Essa alteração foi feita para consolidar a API para índices com `Includes` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-405">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="1ce9e-406">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-406">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-407">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-407">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1ce9e-408">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="1ce9e-408">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1ce9e-409">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="1ce9e-409">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1ce9e-410">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-410">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1ce9e-411">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-411">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-412">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-412">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1ce9e-413">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-413">**New behavior**</span></span>

<span data-ttu-id="1ce9e-414">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-414">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1ce9e-415">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-415">**Why**</span></span>

<span data-ttu-id="1ce9e-416">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-416">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1ce9e-417">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-417">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-418">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-418">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1ce9e-419">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-419">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1ce9e-420">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1ce9e-420">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1ce9e-421">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-421">**Old behavior**</span></span>

<span data-ttu-id="1ce9e-422">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-422">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1ce9e-423">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-423">**New behavior**</span></span>

<span data-ttu-id="1ce9e-424">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-424">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1ce9e-425">**Por que**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-425">**Why**</span></span>

<span data-ttu-id="1ce9e-426">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-426">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1ce9e-427">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="1ce9e-427">**Mitigations**</span></span>

<span data-ttu-id="1ce9e-428">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="1ce9e-428">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
