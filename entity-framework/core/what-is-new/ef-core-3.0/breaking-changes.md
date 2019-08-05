---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497525"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="b6cc0-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="b6cc0-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b6cc0-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="b6cc0-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="b6cc0-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="b6cc0-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="b6cc0-107">Resumo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-107">Summary</span></span>

| <span data-ttu-id="b6cc0-108">**Alterações da falha**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="b6cc0-109">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="b6cc0-110">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="b6cc0-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="b6cc0-111">Alta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-111">High</span></span>       |
| [<span data-ttu-id="b6cc0-112">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="b6cc0-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="b6cc0-113">Alta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-113">High</span></span>      |
| [<span data-ttu-id="b6cc0-114">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="b6cc0-115">Alta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-115">High</span></span>      |
| [<span data-ttu-id="b6cc0-116">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="b6cc0-117">Alta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-117">High</span></span>      |
| [<span data-ttu-id="b6cc0-118">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6cc0-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="b6cc0-119">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-119">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-120">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="b6cc0-121">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-121">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-122">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="b6cc0-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="b6cc0-123">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-123">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-124">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="b6cc0-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="b6cc0-125">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-125">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-126">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="b6cc0-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="b6cc0-127">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-127">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-128">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="b6cc0-129">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-129">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-130">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="b6cc0-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="b6cc0-131">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-131">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-132">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="b6cc0-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="b6cc0-133">Médio</span><span class="sxs-lookup"><span data-stu-id="b6cc0-133">Medium</span></span>      |
| [<span data-ttu-id="b6cc0-134">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="b6cc0-135">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-135">Low</span></span>      |
| [<span data-ttu-id="b6cc0-136">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="b6cc0-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="b6cc0-137">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-137">Low</span></span>      |
| [<span data-ttu-id="b6cc0-138">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="b6cc0-139">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-139">Low</span></span>      |
| [<span data-ttu-id="b6cc0-140">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="b6cc0-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="b6cc0-141">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-141">Low</span></span>      |
| [<span data-ttu-id="b6cc0-142">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="b6cc0-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="b6cc0-143">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-143">Low</span></span>      |
| [<span data-ttu-id="b6cc0-144">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="b6cc0-145">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-145">Low</span></span>      |
| [<span data-ttu-id="b6cc0-146">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="b6cc0-147">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-147">Low</span></span>      |
| [<span data-ttu-id="b6cc0-148">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="b6cc0-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="b6cc0-149">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-149">Low</span></span>      |
| [<span data-ttu-id="b6cc0-150">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="b6cc0-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="b6cc0-151">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-151">Low</span></span>      |
| [<span data-ttu-id="b6cc0-152">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="b6cc0-153">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-153">Low</span></span>      |
| [<span data-ttu-id="b6cc0-154">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="b6cc0-155">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-155">Low</span></span>      |
| [<span data-ttu-id="b6cc0-156">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="b6cc0-157">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-157">Low</span></span>      |
| [<span data-ttu-id="b6cc0-158">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="b6cc0-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="b6cc0-159">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-159">Low</span></span>      |
| [<span data-ttu-id="b6cc0-160">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="b6cc0-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="b6cc0-161">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-161">Low</span></span>      |
| [<span data-ttu-id="b6cc0-162">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="b6cc0-163">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-163">Low</span></span>      |
| [<span data-ttu-id="b6cc0-164">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="b6cc0-165">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-165">Low</span></span>      |
| [<span data-ttu-id="b6cc0-166">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="b6cc0-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="b6cc0-167">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-167">Low</span></span>      |
| [<span data-ttu-id="b6cc0-168">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="b6cc0-169">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-169">Low</span></span>      |
| [<span data-ttu-id="b6cc0-170">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="b6cc0-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="b6cc0-171">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-171">Low</span></span>      |
| [<span data-ttu-id="b6cc0-172">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="b6cc0-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="b6cc0-173">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-173">Low</span></span>      |
| [<span data-ttu-id="b6cc0-174">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="b6cc0-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="b6cc0-175">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-175">Low</span></span>      |
| [<span data-ttu-id="b6cc0-176">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="b6cc0-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="b6cc0-177">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-177">Low</span></span>      |
| [<span data-ttu-id="b6cc0-178">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="b6cc0-179">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-179">Low</span></span>      |
| [<span data-ttu-id="b6cc0-180">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b6cc0-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="b6cc0-181">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-181">Low</span></span>      |
| [<span data-ttu-id="b6cc0-182">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="b6cc0-183">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-183">Low</span></span>      |
| [<span data-ttu-id="b6cc0-184">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="b6cc0-185">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-185">Low</span></span>      |
| [<span data-ttu-id="b6cc0-186">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="b6cc0-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="b6cc0-187">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-187">Low</span></span>      |
| [<span data-ttu-id="b6cc0-188">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="b6cc0-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="b6cc0-189">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-189">Low</span></span>      |
| [<span data-ttu-id="b6cc0-190">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="b6cc0-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="b6cc0-191">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-191">Low</span></span>      |
| [<span data-ttu-id="b6cc0-192">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="b6cc0-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="b6cc0-193">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-193">Low</span></span>      |
| [<span data-ttu-id="b6cc0-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="b6cc0-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="b6cc0-195">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-195">Low</span></span>      |
| [<span data-ttu-id="b6cc0-196">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="b6cc0-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="b6cc0-197">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-197">Low</span></span>      |
| [<span data-ttu-id="b6cc0-198">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6cc0-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="b6cc0-199">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-199">Low</span></span>      |
| [<span data-ttu-id="b6cc0-200">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6cc0-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="b6cc0-201">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-201">Low</span></span>      |
| [<span data-ttu-id="b6cc0-202">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="b6cc0-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="b6cc0-203">Baixo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="b6cc0-204">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="b6cc0-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="b6cc0-205">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="b6cc0-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="b6cc0-206">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-207">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-207">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-208">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="b6cc0-209">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="b6cc0-210">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-210">**New behavior**</span></span>

<span data-ttu-id="b6cc0-211">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="b6cc0-212">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="b6cc0-213">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-213">**Why**</span></span>

<span data-ttu-id="b6cc0-214">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="b6cc0-215">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="b6cc0-216">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="b6cc0-217">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="b6cc0-218">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="b6cc0-219">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="b6cc0-220">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-220">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-221">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="b6cc0-222">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6cc0-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="b6cc0-223">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="b6cc0-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="b6cc0-224">Essa alteração foi introduzida no ASP.NET Core 3.0 versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="b6cc0-225">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-225">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-226">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="b6cc0-227">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-227">**New behavior**</span></span>

<span data-ttu-id="b6cc0-228">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="b6cc0-229">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-229">**Why**</span></span>

<span data-ttu-id="b6cc0-230">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="b6cc0-231">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="b6cc0-232">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="b6cc0-233">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="b6cc0-234">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-234">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-235">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="b6cc0-236">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="b6cc0-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="b6cc0-237">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="b6cc0-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="b6cc0-238">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4 e na versão correspondente do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="b6cc0-239">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-239">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-240">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="b6cc0-241">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-241">**New behavior**</span></span>

<span data-ttu-id="b6cc0-242">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="b6cc0-243">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-243">**Why**</span></span>

<span data-ttu-id="b6cc0-244">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="b6cc0-245">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-245">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-246">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="b6cc0-247">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="b6cc0-248">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="b6cc0-249">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="b6cc0-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="b6cc0-250">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-251">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-251">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-252">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="b6cc0-253">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-253">**New behavior**</span></span>

<span data-ttu-id="b6cc0-254">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="b6cc0-255">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="b6cc0-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="b6cc0-257">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="b6cc0-258">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="b6cc0-259">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-259">**Why**</span></span>

<span data-ttu-id="b6cc0-260">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="b6cc0-261">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="b6cc0-262">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-262">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-263">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="b6cc0-264">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="b6cc0-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="b6cc0-265">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="b6cc0-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="b6cc0-266">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b6cc0-267">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-267">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-268">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="b6cc0-269">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-269">**New behavior**</span></span>

<span data-ttu-id="b6cc0-270">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="b6cc0-271">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="b6cc0-272">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-272">**Why**</span></span>

<span data-ttu-id="b6cc0-273">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="b6cc0-274">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-274">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-275">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="b6cc0-276">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="b6cc0-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="b6cc0-277">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="b6cc0-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="b6cc0-278">Essa alteração será revertida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="b6cc0-279">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="b6cc0-280">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="b6cc0-281">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="b6cc0-282">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="b6cc0-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="b6cc0-283">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b6cc0-284">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-284">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-285">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="b6cc0-286">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="b6cc0-287">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-287">**New behavior**</span></span>

<span data-ttu-id="b6cc0-288">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="b6cc0-289">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-289">**Why**</span></span>

<span data-ttu-id="b6cc0-290">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="b6cc0-291">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-291">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-292">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="b6cc0-293">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-293">This can be avoided by:</span></span>
* <span data-ttu-id="b6cc0-294">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="b6cc0-295">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="b6cc0-296">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="b6cc0-297">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="b6cc0-298">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="b6cc0-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="b6cc0-299">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="b6cc0-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="b6cc0-300">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-301">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-301">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-302">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="b6cc0-303">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-303">**New behavior**</span></span>

<span data-ttu-id="b6cc0-304">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="b6cc0-305">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="b6cc0-306">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="b6cc0-307">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-307">**Why**</span></span>

<span data-ttu-id="b6cc0-308">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="b6cc0-309">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-309">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-310">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="b6cc0-311">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="b6cc0-312">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="b6cc0-313">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="b6cc0-314">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="b6cc0-315">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="b6cc0-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="b6cc0-316">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-317">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-317">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-318">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="b6cc0-319">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-319">**New behavior**</span></span>

<span data-ttu-id="b6cc0-320">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="b6cc0-321">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="b6cc0-322">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-322">**Why**</span></span>

<span data-ttu-id="b6cc0-323">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="b6cc0-324">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-324">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-325">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="b6cc0-326">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="b6cc0-327">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="b6cc0-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="b6cc0-328">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="b6cc0-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="b6cc0-329">Essa alteração foi introduzida no EF Core 3.0 versão prévia 5.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="b6cc0-330">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-330">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-331">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="b6cc0-332">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-332">**New behavior**</span></span>

<span data-ttu-id="b6cc0-333">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="b6cc0-334">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-334">**Why**</span></span>

<span data-ttu-id="b6cc0-335">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="b6cc0-336">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-336">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-337">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="b6cc0-338">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="b6cc0-339">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="b6cc0-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="b6cc0-340">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-341">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-341">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-342">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="b6cc0-343">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="b6cc0-344">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-344">**New behavior**</span></span>

<span data-ttu-id="b6cc0-345">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="b6cc0-346">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="b6cc0-347">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-347">**Why**</span></span>

<span data-ttu-id="b6cc0-348">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="b6cc0-349">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="b6cc0-350">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="b6cc0-351">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-351">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-352">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="b6cc0-353">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="b6cc0-354">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="b6cc0-355">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="b6cc0-356">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="b6cc0-357">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="b6cc0-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="b6cc0-358">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="b6cc0-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="b6cc0-359">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-360">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-360">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-361">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="b6cc0-362">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-362">**New behavior**</span></span>

<span data-ttu-id="b6cc0-363">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="b6cc0-364">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="b6cc0-365">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="b6cc0-366">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="b6cc0-367">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-367">For example:</span></span>

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

<span data-ttu-id="b6cc0-368">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="b6cc0-369">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-369">**Why**</span></span>

<span data-ttu-id="b6cc0-370">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="b6cc0-371">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="b6cc0-372">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-372">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-373">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="b6cc0-374">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="b6cc0-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="b6cc0-375">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="b6cc0-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="b6cc0-376">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-377">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-377">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-378">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-378">Consider the following model:</span></span>
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
<span data-ttu-id="b6cc0-379">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="b6cc0-380">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-380">**New behavior**</span></span>

<span data-ttu-id="b6cc0-381">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="b6cc0-382">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="b6cc0-383">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-383">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-384">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="b6cc0-385">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="b6cc0-386">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="b6cc0-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="b6cc0-387">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="b6cc0-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="b6cc0-388">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-389">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-389">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-390">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-390">Consider the following model:</span></span>
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
<span data-ttu-id="b6cc0-391">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="b6cc0-392">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-392">**New behavior**</span></span>

<span data-ttu-id="b6cc0-393">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="b6cc0-394">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="b6cc0-395">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-395">**Why**</span></span>

<span data-ttu-id="b6cc0-396">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b6cc0-397">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-397">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-398">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="b6cc0-399">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="b6cc0-400">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="b6cc0-401">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="b6cc0-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="b6cc0-402">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-403">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-403">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-404">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-404">Consider the following model:</span></span>
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

<span data-ttu-id="b6cc0-405">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="b6cc0-406">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-406">**New behavior**</span></span>

<span data-ttu-id="b6cc0-407">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="b6cc0-408">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-408">**Why**</span></span>

<span data-ttu-id="b6cc0-409">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="b6cc0-410">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-410">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-411">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="b6cc0-412">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="b6cc0-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="b6cc0-413">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="b6cc0-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="b6cc0-414">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-415">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-415">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-416">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-416">Consider the following model:</span></span>
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
<span data-ttu-id="b6cc0-417">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="b6cc0-418">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="b6cc0-419">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-419">**New behavior**</span></span>

<span data-ttu-id="b6cc0-420">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="b6cc0-421">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="b6cc0-422">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-422">For example:</span></span>

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

<span data-ttu-id="b6cc0-423">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-423">**Why**</span></span>

<span data-ttu-id="b6cc0-424">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="b6cc0-425">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-425">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-426">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="b6cc0-427">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="b6cc0-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="b6cc0-428">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="b6cc0-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="b6cc0-429">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-430">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-430">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-431">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="b6cc0-432">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-432">**New behavior**</span></span>

<span data-ttu-id="b6cc0-433">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="b6cc0-434">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-434">**Why**</span></span>

<span data-ttu-id="b6cc0-435">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="b6cc0-436">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="b6cc0-437">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-437">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-438">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="b6cc0-439">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="b6cc0-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="b6cc0-440">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="b6cc0-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="b6cc0-441">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-442">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-442">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-443">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="b6cc0-444">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-444">**New behavior**</span></span>

<span data-ttu-id="b6cc0-445">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="b6cc0-446">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="b6cc0-447">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-447">**Why**</span></span>

<span data-ttu-id="b6cc0-448">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="b6cc0-449">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-449">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-450">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="b6cc0-451">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="b6cc0-452">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-452">Backing fields are used by default</span></span>

[<span data-ttu-id="b6cc0-453">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="b6cc0-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="b6cc0-454">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b6cc0-455">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-455">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-456">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="b6cc0-457">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="b6cc0-458">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-458">**New behavior**</span></span>

<span data-ttu-id="b6cc0-459">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="b6cc0-460">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="b6cc0-461">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-461">**Why**</span></span>

<span data-ttu-id="b6cc0-462">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="b6cc0-463">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-463">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-464">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="b6cc0-465">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="b6cc0-466">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="b6cc0-467">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="b6cc0-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="b6cc0-468">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-469">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-469">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-470">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="b6cc0-471">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="b6cc0-472">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-472">**New behavior**</span></span>

<span data-ttu-id="b6cc0-473">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="b6cc0-474">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-474">**Why**</span></span>

<span data-ttu-id="b6cc0-475">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="b6cc0-476">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-476">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-477">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="b6cc0-478">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="b6cc0-479">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="b6cc0-480">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-481">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-481">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-482">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo, usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="b6cc0-483">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-483">**New behavior**</span></span>

<span data-ttu-id="b6cc0-484">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="b6cc0-485">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-485">**Why**</span></span>

<span data-ttu-id="b6cc0-486">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="b6cc0-487">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-487">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-488">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="b6cc0-489">Em uma versão prévia posterior do EF Core 3.0, planejamos habilitar novamente a configuração explícita de um nome de campo que seja diferente do nome da propriedade:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="b6cc0-490">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="b6cc0-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="b6cc0-491">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="b6cc0-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="b6cc0-492">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-493">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-493">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-494">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="b6cc0-495">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-495">**New behavior**</span></span>

<span data-ttu-id="b6cc0-496">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="b6cc0-497">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-497">**Why**</span></span>

<span data-ttu-id="b6cc0-498">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="b6cc0-499">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="b6cc0-500">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-500">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-501">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="b6cc0-502">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="b6cc0-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="b6cc0-503">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="b6cc0-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b6cc0-504">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-505">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-505">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-506">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="b6cc0-507">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="b6cc0-508">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-508">**New behavior**</span></span>

<span data-ttu-id="b6cc0-509">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="b6cc0-510">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="b6cc0-511">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="b6cc0-512">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="b6cc0-513">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-513">**Why**</span></span>

<span data-ttu-id="b6cc0-514">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="b6cc0-515">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-515">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-516">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="b6cc0-517">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="b6cc0-518">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="b6cc0-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="b6cc0-519">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-520">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-520">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-521">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="b6cc0-522">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="b6cc0-523">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-523">**New behavior**</span></span>

<span data-ttu-id="b6cc0-524">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="b6cc0-525">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-525">**Why**</span></span>

<span data-ttu-id="b6cc0-526">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="b6cc0-527">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-527">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-528">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="b6cc0-529">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="b6cc0-530">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="b6cc0-531">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="b6cc0-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="b6cc0-532">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="b6cc0-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="b6cc0-533">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-534">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-534">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-535">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="b6cc0-536">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-536">**New behavior**</span></span>

<span data-ttu-id="b6cc0-537">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="b6cc0-538">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-538">**Why**</span></span>

<span data-ttu-id="b6cc0-539">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="b6cc0-540">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-540">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-541">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="b6cc0-542">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-542">This isn't common.</span></span>
<span data-ttu-id="b6cc0-543">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="b6cc0-544">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="b6cc0-545">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="b6cc0-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="b6cc0-546">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="b6cc0-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="b6cc0-547">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-548">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-548">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-549">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="b6cc0-550">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="b6cc0-551">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="b6cc0-552">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-552">**New behavior**</span></span>

<span data-ttu-id="b6cc0-553">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="b6cc0-554">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="b6cc0-555">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="b6cc0-556">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="b6cc0-557">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-557">**Why**</span></span>

<span data-ttu-id="b6cc0-558">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="b6cc0-559">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-559">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-560">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="b6cc0-561">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="b6cc0-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="b6cc0-562">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="b6cc0-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="b6cc0-563">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-564">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-564">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-565">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="b6cc0-566">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-566">**New behavior**</span></span>

<span data-ttu-id="b6cc0-567">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="b6cc0-568">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-568">**Why**</span></span>

<span data-ttu-id="b6cc0-569">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="b6cc0-570">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-570">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-571">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="b6cc0-572">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="b6cc0-573">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="b6cc0-574">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="b6cc0-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="b6cc0-575">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="b6cc0-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="b6cc0-576">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-577">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-577">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-578">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="b6cc0-579">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="b6cc0-580">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="b6cc0-581">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="b6cc0-582">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-582">**New behavior**</span></span>

<span data-ttu-id="b6cc0-583">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="b6cc0-584">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-584">**Why**</span></span>

<span data-ttu-id="b6cc0-585">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="b6cc0-586">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-586">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-587">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="b6cc0-588">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-588">This is not common.</span></span>
<span data-ttu-id="b6cc0-589">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="b6cc0-590">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="b6cc0-591">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="b6cc0-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="b6cc0-592">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="b6cc0-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="b6cc0-593">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-594">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-594">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-595">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="b6cc0-596">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="b6cc0-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="b6cc0-597">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-597">**New behavior**</span></span>

<span data-ttu-id="b6cc0-598">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="b6cc0-599">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-599">**Why**</span></span>

<span data-ttu-id="b6cc0-600">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="b6cc0-601">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-601">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-602">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="b6cc0-603">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="b6cc0-604">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="b6cc0-605">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="b6cc0-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="b6cc0-606">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="b6cc0-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="b6cc0-607">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b6cc0-608">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-608">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-609">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b6cc0-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="b6cc0-610">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-610">**New behavior**</span></span>

<span data-ttu-id="b6cc0-611">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b6cc0-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="b6cc0-612">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-612">**Why**</span></span>

<span data-ttu-id="b6cc0-613">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="b6cc0-614">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-614">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-615">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="b6cc0-616">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="b6cc0-617">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="b6cc0-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="b6cc0-618">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="b6cc0-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="b6cc0-619">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-620">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-620">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-621">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="b6cc0-622">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-622">**New behavior**</span></span>

<span data-ttu-id="b6cc0-623">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="b6cc0-624">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-624">**Why**</span></span>

<span data-ttu-id="b6cc0-625">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="b6cc0-626">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="b6cc0-627">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-627">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-628">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="b6cc0-629">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="b6cc0-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="b6cc0-630">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="b6cc0-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="b6cc0-631">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-632">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-632">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-633">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="b6cc0-634">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-634">**New behavior**</span></span>

<span data-ttu-id="b6cc0-635">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="b6cc0-636">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="b6cc0-637">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-637">**Why**</span></span>

<span data-ttu-id="b6cc0-638">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="b6cc0-639">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-639">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-640">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="b6cc0-641">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="b6cc0-641">Metadata API changes</span></span>

[<span data-ttu-id="b6cc0-642">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="b6cc0-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b6cc0-643">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-644">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-644">**New behavior**</span></span>

<span data-ttu-id="b6cc0-645">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="b6cc0-646">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-646">**Why**</span></span>

<span data-ttu-id="b6cc0-647">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="b6cc0-648">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-648">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-649">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="b6cc0-650">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="b6cc0-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="b6cc0-651">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="b6cc0-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b6cc0-652">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b6cc0-653">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-653">**New behavior**</span></span>

<span data-ttu-id="b6cc0-654">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="b6cc0-655">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-655">**Why**</span></span>

<span data-ttu-id="b6cc0-656">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="b6cc0-657">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-657">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-658">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="b6cc0-659">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="b6cc0-660">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="b6cc0-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="b6cc0-661">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b6cc0-662">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-662">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-663">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b6cc0-664">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-664">**New behavior**</span></span>

<span data-ttu-id="b6cc0-665">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b6cc0-666">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-666">**Why**</span></span>

<span data-ttu-id="b6cc0-667">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="b6cc0-668">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-668">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-669">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="b6cc0-670">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="b6cc0-671">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b6cc0-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="b6cc0-672">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-672">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-673">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="b6cc0-674">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-674">**New behavior**</span></span>

<span data-ttu-id="b6cc0-675">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="b6cc0-676">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-676">**Why**</span></span>

<span data-ttu-id="b6cc0-677">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="b6cc0-678">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-678">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-679">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b6cc0-680">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b6cc0-681">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="b6cc0-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="b6cc0-682">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-683">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-683">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-684">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="b6cc0-685">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-685">**New behavior**</span></span>

<span data-ttu-id="b6cc0-686">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="b6cc0-687">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-687">**Why**</span></span>

<span data-ttu-id="b6cc0-688">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="b6cc0-689">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b6cc0-690">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-690">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-691">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="b6cc0-692">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="b6cc0-693">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b6cc0-694">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="b6cc0-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b6cc0-695">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="b6cc0-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="b6cc0-696">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-697">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-697">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-698">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="b6cc0-699">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="b6cc0-700">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-700">**New behavior**</span></span>

<span data-ttu-id="b6cc0-701">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="b6cc0-702">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-702">**Why**</span></span>

<span data-ttu-id="b6cc0-703">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b6cc0-704">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-704">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-705">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="b6cc0-706">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="b6cc0-707">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="b6cc0-708">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="b6cc0-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="b6cc0-709">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="b6cc0-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="b6cc0-710">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-711">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-711">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-712">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="b6cc0-713">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-713">**New behavior**</span></span>

<span data-ttu-id="b6cc0-714">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="b6cc0-715">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-715">**Why**</span></span>

<span data-ttu-id="b6cc0-716">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="b6cc0-717">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="b6cc0-718">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-718">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-719">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="b6cc0-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="b6cc0-720">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="b6cc0-721">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="b6cc0-722">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="b6cc0-723">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="b6cc0-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="b6cc0-724">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="b6cc0-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="b6cc0-725">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b6cc0-726">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-726">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-727">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="b6cc0-728">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-728">**New behavior**</span></span>

<span data-ttu-id="b6cc0-729">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="b6cc0-730">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-730">**Why**</span></span>

<span data-ttu-id="b6cc0-731">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="b6cc0-732">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-732">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-733">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="b6cc0-734">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="b6cc0-735">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="b6cc0-736">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="b6cc0-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="b6cc0-737">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="b6cc0-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="b6cc0-738">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="b6cc0-739">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-739">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-740">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="b6cc0-741">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-741">**New behavior**</span></span>

<span data-ttu-id="b6cc0-742">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="b6cc0-743">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-743">**Why**</span></span>

<span data-ttu-id="b6cc0-744">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="b6cc0-745">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="b6cc0-746">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-746">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-747">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="b6cc0-748">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="b6cc0-749">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="b6cc0-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="b6cc0-750">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="b6cc0-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="b6cc0-751">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-752">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-752">**Change**</span></span>

<span data-ttu-id="b6cc0-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="b6cc0-754">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-754">**Why**</span></span>

<span data-ttu-id="b6cc0-755">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="b6cc0-756">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-756">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-757">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-757">Use the new name.</span></span> <span data-ttu-id="b6cc0-758">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="b6cc0-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="b6cc0-759">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="b6cc0-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="b6cc0-760">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="b6cc0-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="b6cc0-761">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-762">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-762">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-763">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="b6cc0-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="b6cc0-764">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="b6cc0-765">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-765">**New behavior**</span></span>

<span data-ttu-id="b6cc0-766">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="b6cc0-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="b6cc0-767">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="b6cc0-768">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-768">**Why**</span></span>

<span data-ttu-id="b6cc0-769">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="b6cc0-770">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-770">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-771">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="b6cc0-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="b6cc0-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="b6cc0-773">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="b6cc0-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="b6cc0-774">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="b6cc0-775">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-775">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-776">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="b6cc0-777">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-777">**New behavior**</span></span>

<span data-ttu-id="b6cc0-778">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="b6cc0-779">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-779">**Why**</span></span>

<span data-ttu-id="b6cc0-780">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="b6cc0-781">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="b6cc0-782">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-782">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-783">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="b6cc0-784">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="b6cc0-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="b6cc0-785">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="b6cc0-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="b6cc0-786">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b6cc0-787">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-787">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-788">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="b6cc0-789">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-789">**New behavior**</span></span>

<span data-ttu-id="b6cc0-790">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="b6cc0-791">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="b6cc0-792">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-792">**Why**</span></span>

<span data-ttu-id="b6cc0-793">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="b6cc0-794">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="b6cc0-795">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="b6cc0-796">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-796">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-797">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="b6cc0-798">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="b6cc0-799">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6cc0-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="b6cc0-800">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="b6cc0-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="b6cc0-801">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="b6cc0-802">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-802">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-803">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="b6cc0-804">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-804">**New behavior**</span></span>

<span data-ttu-id="b6cc0-805">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="b6cc0-806">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-806">**Why**</span></span>

<span data-ttu-id="b6cc0-807">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="b6cc0-808">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="b6cc0-809">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-809">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-810">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="b6cc0-811">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="b6cc0-812">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6cc0-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="b6cc0-813">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="b6cc0-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="b6cc0-814">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="b6cc0-815">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-815">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-816">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="b6cc0-817">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-817">**New behavior**</span></span>

<span data-ttu-id="b6cc0-818">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="b6cc0-819">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-819">**Why**</span></span>

<span data-ttu-id="b6cc0-820">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="b6cc0-821">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-821">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-822">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="b6cc0-823">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="b6cc0-824">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="b6cc0-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="b6cc0-825">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="b6cc0-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="b6cc0-826">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="b6cc0-827">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-827">**Old behavior**</span></span>

<span data-ttu-id="b6cc0-828">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="b6cc0-829">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-829">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="b6cc0-830">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-830">**New behavior**</span></span>

<span data-ttu-id="b6cc0-831">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="b6cc0-832">**Por que**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-832">**Why**</span></span>

<span data-ttu-id="b6cc0-833">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="b6cc0-834">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="b6cc0-834">**Mitigations**</span></span>

<span data-ttu-id="b6cc0-835">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="b6cc0-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="b6cc0-836">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b6cc0-836">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
