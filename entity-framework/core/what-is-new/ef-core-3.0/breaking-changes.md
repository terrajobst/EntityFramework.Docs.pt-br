---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 10a0f0edc5f98baea26b1a5b9c0aa869b1df01af
ms.sourcegitcommit: df181e201365c20610ba56dcd5c5ed30cfda00c2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2019
ms.locfileid: "70997850"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="f307f-102">Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)</span><span class="sxs-lookup"><span data-stu-id="f307f-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f307f-103">Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.</span><span class="sxs-lookup"><span data-stu-id="f307f-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="f307f-104">As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="f307f-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="f307f-105">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="f307f-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="f307f-106">Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="f307f-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="f307f-107">Resumo</span><span class="sxs-lookup"><span data-stu-id="f307f-107">Summary</span></span>

| <span data-ttu-id="f307f-108">**Alteração significativa**</span><span class="sxs-lookup"><span data-stu-id="f307f-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="f307f-109">**Causa**</span><span class="sxs-lookup"><span data-stu-id="f307f-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="f307f-110">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="f307f-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="f307f-111">Alto</span><span class="sxs-lookup"><span data-stu-id="f307f-111">High</span></span>       |
| [<span data-ttu-id="f307f-112">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="f307f-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="f307f-113">Alto</span><span class="sxs-lookup"><span data-stu-id="f307f-113">High</span></span>      |
| [<span data-ttu-id="f307f-114">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="f307f-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="f307f-115">Alto</span><span class="sxs-lookup"><span data-stu-id="f307f-115">High</span></span>      |
| [<span data-ttu-id="f307f-116">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="f307f-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="f307f-117">Alto</span><span class="sxs-lookup"><span data-stu-id="f307f-117">High</span></span>      |
| [<span data-ttu-id="f307f-118">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="f307f-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="f307f-119">Alto</span><span class="sxs-lookup"><span data-stu-id="f307f-119">High</span></span>      |
| [<span data-ttu-id="f307f-120">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f307f-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="f307f-121">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-121">Medium</span></span>      |
| [<span data-ttu-id="f307f-122">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="f307f-123">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-123">Medium</span></span>      |
| [<span data-ttu-id="f307f-124">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="f307f-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="f307f-125">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-125">Medium</span></span>      |
| [<span data-ttu-id="f307f-126">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="f307f-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="f307f-127">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-127">Medium</span></span>      |
| [<span data-ttu-id="f307f-128">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="f307f-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="f307f-129">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-129">Medium</span></span>      |
| [<span data-ttu-id="f307f-130">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="f307f-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="f307f-131">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-131">Medium</span></span>      |
| [<span data-ttu-id="f307f-132">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="f307f-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="f307f-133">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-133">Medium</span></span>      |
| [<span data-ttu-id="f307f-134">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="f307f-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="f307f-135">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-135">Medium</span></span>      |
| [<span data-ttu-id="f307f-136">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="f307f-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="f307f-137">Média</span><span class="sxs-lookup"><span data-stu-id="f307f-137">Medium</span></span>      |
| [<span data-ttu-id="f307f-138">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="f307f-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="f307f-139">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-139">Low</span></span>      |
| [<span data-ttu-id="f307f-140">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="f307f-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="f307f-141">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-141">Low</span></span>      |
| [<span data-ttu-id="f307f-142">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="f307f-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="f307f-143">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-143">Low</span></span>      |
| [<span data-ttu-id="f307f-144">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="f307f-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="f307f-145">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-145">Low</span></span>      |
| [<span data-ttu-id="f307f-146">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="f307f-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="f307f-147">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-147">Low</span></span>      |
| [<span data-ttu-id="f307f-148">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="f307f-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="f307f-149">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-149">Low</span></span>      |
| [<span data-ttu-id="f307f-150">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="f307f-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="f307f-151">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-151">Low</span></span>      |
| [<span data-ttu-id="f307f-152">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="f307f-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="f307f-153">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-153">Low</span></span>      |
| [<span data-ttu-id="f307f-154">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f307f-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="f307f-155">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-155">Low</span></span>      |
| [<span data-ttu-id="f307f-156">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="f307f-157">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-157">Low</span></span>      |
| [<span data-ttu-id="f307f-158">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="f307f-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="f307f-159">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-159">Low</span></span>      |
| [<span data-ttu-id="f307f-160">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="f307f-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="f307f-161">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-161">Low</span></span>      |
| [<span data-ttu-id="f307f-162">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="f307f-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="f307f-163">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-163">Low</span></span>      |
| [<span data-ttu-id="f307f-164">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="f307f-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="f307f-165">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-165">Low</span></span>      |
| [<span data-ttu-id="f307f-166">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="f307f-167">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-167">Low</span></span>      |
| [<span data-ttu-id="f307f-168">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="f307f-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="f307f-169">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-169">Low</span></span>      |
| [<span data-ttu-id="f307f-170">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="f307f-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="f307f-171">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-171">Low</span></span>      |
| [<span data-ttu-id="f307f-172">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="f307f-173">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-173">Low</span></span>      |
| [<span data-ttu-id="f307f-174">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="f307f-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="f307f-175">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-175">Low</span></span>      |
| [<span data-ttu-id="f307f-176">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="f307f-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="f307f-177">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-177">Low</span></span>      |
| [<span data-ttu-id="f307f-178">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="f307f-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="f307f-179">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-179">Low</span></span>      |
| [<span data-ttu-id="f307f-180">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="f307f-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="f307f-181">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-181">Low</span></span>      |
| [<span data-ttu-id="f307f-182">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="f307f-183">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-183">Low</span></span>      |
| [<span data-ttu-id="f307f-184">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="f307f-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="f307f-185">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-185">Low</span></span>      |
| [<span data-ttu-id="f307f-186">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="f307f-187">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-187">Low</span></span>      |
| [<span data-ttu-id="f307f-188">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="f307f-189">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-189">Low</span></span>      |
| [<span data-ttu-id="f307f-190">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="f307f-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="f307f-191">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-191">Low</span></span>      |
| [<span data-ttu-id="f307f-192">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="f307f-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="f307f-193">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-193">Low</span></span>      |
| [<span data-ttu-id="f307f-194">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="f307f-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="f307f-195">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-195">Low</span></span>      |
| [<span data-ttu-id="f307f-196">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="f307f-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="f307f-197">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-197">Low</span></span>      |
| [<span data-ttu-id="f307f-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="f307f-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="f307f-199">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-199">Low</span></span>      |
| [<span data-ttu-id="f307f-200">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="f307f-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="f307f-201">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-201">Low</span></span>      |
| [<span data-ttu-id="f307f-202">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f307f-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="f307f-203">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-203">Low</span></span>      |
| [<span data-ttu-id="f307f-204">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f307f-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="f307f-205">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-205">Low</span></span>      |
| [<span data-ttu-id="f307f-206">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="f307f-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="f307f-207">Baixa</span><span class="sxs-lookup"><span data-stu-id="f307f-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="f307f-208">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="f307f-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="f307f-209">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="f307f-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="f307f-210">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-211">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-211">**Old behavior**</span></span>

<span data-ttu-id="f307f-212">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="f307f-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="f307f-213">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="f307f-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="f307f-214">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-214">**New behavior**</span></span>

<span data-ttu-id="f307f-215">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="f307f-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="f307f-216">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="f307f-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="f307f-217">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-217">**Why**</span></span>

<span data-ttu-id="f307f-218">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="f307f-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="f307f-219">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="f307f-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="f307f-220">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="f307f-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="f307f-221">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="f307f-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="f307f-222">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f307f-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="f307f-223">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="f307f-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="f307f-224">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-224">**Mitigations**</span></span>

<span data-ttu-id="f307f-225">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="f307f-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="f307f-226">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="f307f-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="f307f-227">Problema de acompanhamento nº 15498</span><span class="sxs-lookup"><span data-stu-id="f307f-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="f307f-228">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-229">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-229">**Old behavior**</span></span>

<span data-ttu-id="f307f-230">Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f307f-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="f307f-231">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-231">**New behavior**</span></span>

<span data-ttu-id="f307f-232">No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão.</span><span class="sxs-lookup"><span data-stu-id="f307f-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="f307f-233">Isso não inclui o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f307f-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="f307f-234">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-234">**Why**</span></span>

<span data-ttu-id="f307f-235">Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.</span><span class="sxs-lookup"><span data-stu-id="f307f-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="f307f-236">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-236">**Mitigations**</span></span>

<span data-ttu-id="f307f-237">Considere a possibilidade de migrar para uma plataforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="f307f-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="f307f-238">Se isso não for possível, continue usando o EF Core 2.1 ou o EF Core 2.2, que dão suporte ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f307f-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="f307f-239">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f307f-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="f307f-240">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="f307f-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="f307f-241">Essa alteração foi introduzida no ASP.NET Core 3.0 versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="f307f-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="f307f-242">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-242">**Old behavior**</span></span>

<span data-ttu-id="f307f-243">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f307f-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="f307f-244">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-244">**New behavior**</span></span>

<span data-ttu-id="f307f-245">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="f307f-246">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-246">**Why**</span></span>

<span data-ttu-id="f307f-247">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="f307f-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="f307f-248">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="f307f-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="f307f-249">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="f307f-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="f307f-250">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="f307f-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="f307f-251">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-251">**Mitigations**</span></span>

<span data-ttu-id="f307f-252">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="f307f-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="f307f-253">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="f307f-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="f307f-254">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="f307f-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="f307f-255">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4 e na versão correspondente do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="f307f-256">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-256">**Old behavior**</span></span>

<span data-ttu-id="f307f-257">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="f307f-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="f307f-258">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-258">**New behavior**</span></span>

<span data-ttu-id="f307f-259">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="f307f-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="f307f-260">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-260">**Why**</span></span>

<span data-ttu-id="f307f-261">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="f307f-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="f307f-262">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-262">**Mitigations**</span></span>

<span data-ttu-id="f307f-263">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="f307f-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="f307f-264">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="f307f-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="f307f-265">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="f307f-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="f307f-266">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="f307f-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="f307f-267">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-268">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-268">**Old behavior**</span></span>

<span data-ttu-id="f307f-269">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="f307f-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="f307f-270">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-270">**New behavior**</span></span>

<span data-ttu-id="f307f-271">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="f307f-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="f307f-272">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="f307f-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="f307f-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="f307f-274">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="f307f-275">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="f307f-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="f307f-276">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-276">**Why**</span></span>

<span data-ttu-id="f307f-277">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="f307f-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="f307f-278">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="f307f-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="f307f-279">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-279">**Mitigations**</span></span>

<span data-ttu-id="f307f-280">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="f307f-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="f307f-281">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="f307f-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="f307f-282">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="f307f-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="f307f-283">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="f307f-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="f307f-284">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-284">**Old behavior**</span></span>

<span data-ttu-id="f307f-285">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="f307f-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="f307f-286">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-286">**New behavior**</span></span>

<span data-ttu-id="f307f-287">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="f307f-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="f307f-288">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="f307f-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="f307f-289">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-289">**Why**</span></span>

<span data-ttu-id="f307f-290">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="f307f-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="f307f-291">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-291">**Mitigations**</span></span>

<span data-ttu-id="f307f-292">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="f307f-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="f307f-293">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="f307f-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="f307f-294">Problema de acompanhamento nº 13518</span><span class="sxs-lookup"><span data-stu-id="f307f-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="f307f-295">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="f307f-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="f307f-296">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-296">**Old behavior**</span></span>

<span data-ttu-id="f307f-297">Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="f307f-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="f307f-298">Isso corresponde ao comportamento das consultas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="f307f-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="f307f-299">Por exemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="f307f-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="f307f-300">retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.</span><span class="sxs-lookup"><span data-stu-id="f307f-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="f307f-301">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-301">**New behavior**</span></span>

<span data-ttu-id="f307f-302">No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado.</span><span class="sxs-lookup"><span data-stu-id="f307f-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="f307f-303">Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.</span><span class="sxs-lookup"><span data-stu-id="f307f-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="f307f-304">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-304">**Why**</span></span>

<span data-ttu-id="f307f-305">A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória.</span><span class="sxs-lookup"><span data-stu-id="f307f-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="f307f-306">Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="f307f-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="f307f-307">Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="f307f-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="f307f-308">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-308">**Mitigations**</span></span>

<span data-ttu-id="f307f-309">Use uma consulta de acompanhamento se a resolução de identidade for necessária.</span><span class="sxs-lookup"><span data-stu-id="f307f-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="f307f-310">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="f307f-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="f307f-311">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="f307f-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="f307f-312">Essa alteração será revertida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-313">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f307f-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="f307f-314">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="f307f-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="f307f-315">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="f307f-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="f307f-316">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="f307f-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="f307f-317">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="f307f-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f307f-318">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-318">**Old behavior**</span></span>

<span data-ttu-id="f307f-319">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f307f-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="f307f-320">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="f307f-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="f307f-321">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-321">**New behavior**</span></span>

<span data-ttu-id="f307f-322">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="f307f-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="f307f-323">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-323">**Why**</span></span>

<span data-ttu-id="f307f-324">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f307f-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="f307f-325">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-325">**Mitigations**</span></span>

<span data-ttu-id="f307f-326">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="f307f-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="f307f-327">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="f307f-327">This can be avoided by:</span></span>
* <span data-ttu-id="f307f-328">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="f307f-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="f307f-329">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="f307f-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="f307f-330">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="f307f-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="f307f-331">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="f307f-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="f307f-332">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="f307f-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="f307f-333">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="f307f-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="f307f-334">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-335">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-335">**Old behavior**</span></span>

<span data-ttu-id="f307f-336">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="f307f-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="f307f-337">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-337">**New behavior**</span></span>

<span data-ttu-id="f307f-338">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="f307f-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="f307f-339">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="f307f-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="f307f-340">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="f307f-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="f307f-341">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-341">**Why**</span></span>

<span data-ttu-id="f307f-342">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="f307f-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="f307f-343">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-343">**Mitigations**</span></span>

<span data-ttu-id="f307f-344">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="f307f-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="f307f-345">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="f307f-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="f307f-346">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="f307f-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="f307f-347">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="f307f-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="f307f-348">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="f307f-349">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="f307f-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="f307f-350">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-351">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-351">**Old behavior**</span></span>

<span data-ttu-id="f307f-352">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="f307f-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="f307f-353">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-353">**New behavior**</span></span>

<span data-ttu-id="f307f-354">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="f307f-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="f307f-355">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="f307f-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="f307f-356">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-356">**Why**</span></span>

<span data-ttu-id="f307f-357">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="f307f-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="f307f-358">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-358">**Mitigations**</span></span>

<span data-ttu-id="f307f-359">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="f307f-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="f307f-360">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="f307f-361">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="f307f-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="f307f-362">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="f307f-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="f307f-363">Essa alteração foi introduzida no EF Core 3.0 versão prévia 5.</span><span class="sxs-lookup"><span data-stu-id="f307f-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="f307f-364">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-364">**Old behavior**</span></span>

<span data-ttu-id="f307f-365">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="f307f-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="f307f-366">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-366">**New behavior**</span></span>

<span data-ttu-id="f307f-367">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="f307f-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="f307f-368">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-368">**Why**</span></span>

<span data-ttu-id="f307f-369">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="f307f-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="f307f-370">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-370">**Mitigations**</span></span>

<span data-ttu-id="f307f-371">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="f307f-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="f307f-372">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="f307f-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="f307f-373">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="f307f-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="f307f-374">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-375">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-375">**Old behavior**</span></span>

<span data-ttu-id="f307f-376">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="f307f-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="f307f-377">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="f307f-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="f307f-378">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-378">**New behavior**</span></span>

<span data-ttu-id="f307f-379">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="f307f-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="f307f-380">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="f307f-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="f307f-381">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-381">**Why**</span></span>

<span data-ttu-id="f307f-382">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="f307f-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="f307f-383">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="f307f-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="f307f-384">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="f307f-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="f307f-385">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-385">**Mitigations**</span></span>

<span data-ttu-id="f307f-386">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="f307f-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="f307f-387">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="f307f-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="f307f-388">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="f307f-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="f307f-389">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="f307f-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="f307f-390">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="f307f-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="f307f-391">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="f307f-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="f307f-392">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="f307f-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="f307f-393">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-394">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-394">**Old behavior**</span></span>

<span data-ttu-id="f307f-395">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="f307f-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="f307f-396">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-396">**New behavior**</span></span>

<span data-ttu-id="f307f-397">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="f307f-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="f307f-398">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="f307f-399">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="f307f-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="f307f-400">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="f307f-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="f307f-401">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-401">For example:</span></span>

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

<span data-ttu-id="f307f-402">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="f307f-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="f307f-403">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-403">**Why**</span></span>

<span data-ttu-id="f307f-404">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f307f-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="f307f-405">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="f307f-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="f307f-406">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-406">**Mitigations**</span></span>

<span data-ttu-id="f307f-407">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="f307f-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="f307f-408">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="f307f-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="f307f-409">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="f307f-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="f307f-410">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-411">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-411">**Old behavior**</span></span>

<span data-ttu-id="f307f-412">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f307f-412">Consider the following model:</span></span>
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
<span data-ttu-id="f307f-413">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="f307f-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="f307f-414">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-414">**New behavior**</span></span>

<span data-ttu-id="f307f-415">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="f307f-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="f307f-416">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="f307f-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="f307f-417">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-417">**Mitigations**</span></span>

<span data-ttu-id="f307f-418">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="f307f-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="f307f-419">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="f307f-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="f307f-420">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="f307f-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="f307f-421">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="f307f-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="f307f-422">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-423">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-423">**Old behavior**</span></span>

<span data-ttu-id="f307f-424">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f307f-424">Consider the following model:</span></span>
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
<span data-ttu-id="f307f-425">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="f307f-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="f307f-426">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-426">**New behavior**</span></span>

<span data-ttu-id="f307f-427">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="f307f-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="f307f-428">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="f307f-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="f307f-429">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-429">**Why**</span></span>

<span data-ttu-id="f307f-430">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="f307f-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="f307f-431">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-431">**Mitigations**</span></span>

<span data-ttu-id="f307f-432">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="f307f-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="f307f-433">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="f307f-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="f307f-434">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="f307f-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="f307f-435">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="f307f-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="f307f-436">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-437">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-437">**Old behavior**</span></span>

<span data-ttu-id="f307f-438">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f307f-438">Consider the following model:</span></span>
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

<span data-ttu-id="f307f-439">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="f307f-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="f307f-440">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-440">**New behavior**</span></span>

<span data-ttu-id="f307f-441">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="f307f-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="f307f-442">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-442">**Why**</span></span>

<span data-ttu-id="f307f-443">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="f307f-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="f307f-444">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-444">**Mitigations**</span></span>

<span data-ttu-id="f307f-445">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="f307f-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="f307f-446">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="f307f-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="f307f-447">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="f307f-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="f307f-448">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-449">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-449">**Old behavior**</span></span>

<span data-ttu-id="f307f-450">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="f307f-450">Consider the following model:</span></span>
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
<span data-ttu-id="f307f-451">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="f307f-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="f307f-452">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="f307f-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="f307f-453">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-453">**New behavior**</span></span>

<span data-ttu-id="f307f-454">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="f307f-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="f307f-455">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="f307f-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="f307f-456">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-456">For example:</span></span>

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

<span data-ttu-id="f307f-457">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-457">**Why**</span></span>

<span data-ttu-id="f307f-458">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="f307f-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="f307f-459">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-459">**Mitigations**</span></span>

<span data-ttu-id="f307f-460">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="f307f-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="f307f-461">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="f307f-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="f307f-462">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="f307f-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="f307f-463">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-464">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-464">**Old behavior**</span></span>

<span data-ttu-id="f307f-465">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="f307f-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="f307f-466">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-466">**New behavior**</span></span>

<span data-ttu-id="f307f-467">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="f307f-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="f307f-468">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-468">**Why**</span></span>

<span data-ttu-id="f307f-469">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="f307f-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="f307f-470">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="f307f-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="f307f-471">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-471">**Mitigations**</span></span>

<span data-ttu-id="f307f-472">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="f307f-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="f307f-473">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="f307f-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="f307f-474">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="f307f-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="f307f-475">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-476">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-476">**Old behavior**</span></span>

<span data-ttu-id="f307f-477">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="f307f-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="f307f-478">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-478">**New behavior**</span></span>

<span data-ttu-id="f307f-479">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="f307f-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="f307f-480">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="f307f-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="f307f-481">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-481">**Why**</span></span>

<span data-ttu-id="f307f-482">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="f307f-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="f307f-483">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-483">**Mitigations**</span></span>

<span data-ttu-id="f307f-484">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="f307f-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="f307f-485">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="f307f-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="f307f-486">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-486">Backing fields are used by default</span></span>

[<span data-ttu-id="f307f-487">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="f307f-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="f307f-488">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="f307f-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f307f-489">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-489">**Old behavior**</span></span>

<span data-ttu-id="f307f-490">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="f307f-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="f307f-491">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="f307f-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="f307f-492">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-492">**New behavior**</span></span>

<span data-ttu-id="f307f-493">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="f307f-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="f307f-494">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="f307f-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="f307f-495">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-495">**Why**</span></span>

<span data-ttu-id="f307f-496">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="f307f-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="f307f-497">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-497">**Mitigations**</span></span>

<span data-ttu-id="f307f-498">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f307f-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="f307f-499">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="f307f-500">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="f307f-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="f307f-501">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="f307f-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="f307f-502">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-503">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-503">**Old behavior**</span></span>

<span data-ttu-id="f307f-504">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="f307f-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="f307f-505">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="f307f-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="f307f-506">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-506">**New behavior**</span></span>

<span data-ttu-id="f307f-507">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="f307f-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="f307f-508">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-508">**Why**</span></span>

<span data-ttu-id="f307f-509">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="f307f-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="f307f-510">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-510">**Mitigations**</span></span>

<span data-ttu-id="f307f-511">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="f307f-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="f307f-512">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="f307f-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="f307f-513">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="f307f-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="f307f-514">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-515">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-515">**Old behavior**</span></span>

<span data-ttu-id="f307f-516">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo, usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="f307f-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="f307f-517">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-517">**New behavior**</span></span>

<span data-ttu-id="f307f-518">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="f307f-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="f307f-519">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-519">**Why**</span></span>

<span data-ttu-id="f307f-520">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="f307f-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="f307f-521">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-521">**Mitigations**</span></span>

<span data-ttu-id="f307f-522">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="f307f-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="f307f-523">Em uma versão futura do EF Core após 3,0, planejamos reabilitar explicitamente um nome de campo diferente do nome da propriedade (consulte o problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="f307f-523">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="f307f-524">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="f307f-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="f307f-525">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="f307f-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="f307f-526">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-527">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-527">**Old behavior**</span></span>

<span data-ttu-id="f307f-528">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f307f-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="f307f-529">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-529">**New behavior**</span></span>

<span data-ttu-id="f307f-530">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="f307f-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="f307f-531">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-531">**Why**</span></span>

<span data-ttu-id="f307f-532">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f307f-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="f307f-533">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="f307f-534">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-534">**Mitigations**</span></span>

<span data-ttu-id="f307f-535">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f307f-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="f307f-536">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="f307f-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="f307f-537">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="f307f-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="f307f-538">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-539">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-539">**Old behavior**</span></span>

<span data-ttu-id="f307f-540">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="f307f-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="f307f-541">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="f307f-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="f307f-542">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-542">**New behavior**</span></span>

<span data-ttu-id="f307f-543">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="f307f-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="f307f-544">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f307f-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="f307f-545">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="f307f-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="f307f-546">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="f307f-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="f307f-547">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-547">**Why**</span></span>

<span data-ttu-id="f307f-548">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="f307f-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="f307f-549">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-549">**Mitigations**</span></span>

<span data-ttu-id="f307f-550">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="f307f-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="f307f-551">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="f307f-552">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="f307f-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="f307f-553">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-554">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-554">**Old behavior**</span></span>

<span data-ttu-id="f307f-555">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="f307f-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="f307f-556">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f307f-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="f307f-557">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-557">**New behavior**</span></span>

<span data-ttu-id="f307f-558">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="f307f-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="f307f-559">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-559">**Why**</span></span>

<span data-ttu-id="f307f-560">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="f307f-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="f307f-561">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-561">**Mitigations**</span></span>

<span data-ttu-id="f307f-562">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="f307f-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="f307f-563">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="f307f-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="f307f-564">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="f307f-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="f307f-565">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="f307f-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="f307f-566">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="f307f-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="f307f-567">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-568">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-568">**Old behavior**</span></span>

<span data-ttu-id="f307f-569">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="f307f-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="f307f-570">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-570">**New behavior**</span></span>

<span data-ttu-id="f307f-571">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="f307f-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="f307f-572">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-572">**Why**</span></span>

<span data-ttu-id="f307f-573">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="f307f-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="f307f-574">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-574">**Mitigations**</span></span>

<span data-ttu-id="f307f-575">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="f307f-576">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="f307f-576">This isn't common.</span></span>
<span data-ttu-id="f307f-577">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="f307f-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="f307f-578">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="f307f-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="f307f-579">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="f307f-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="f307f-580">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="f307f-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="f307f-581">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-582">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-582">**Old behavior**</span></span>

<span data-ttu-id="f307f-583">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="f307f-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="f307f-584">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="f307f-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="f307f-585">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="f307f-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="f307f-586">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-586">**New behavior**</span></span>

<span data-ttu-id="f307f-587">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="f307f-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="f307f-588">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="f307f-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="f307f-589">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="f307f-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="f307f-590">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="f307f-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="f307f-591">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-591">**Why**</span></span>

<span data-ttu-id="f307f-592">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="f307f-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="f307f-593">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-593">**Mitigations**</span></span>

<span data-ttu-id="f307f-594">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="f307f-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="f307f-595">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="f307f-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="f307f-596">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="f307f-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="f307f-597">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-598">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-598">**Old behavior**</span></span>

<span data-ttu-id="f307f-599">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="f307f-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="f307f-600">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-600">**New behavior**</span></span>

<span data-ttu-id="f307f-601">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="f307f-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="f307f-602">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-602">**Why**</span></span>

<span data-ttu-id="f307f-603">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f307f-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="f307f-604">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-604">**Mitigations**</span></span>

<span data-ttu-id="f307f-605">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="f307f-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="f307f-606">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f307f-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="f307f-607">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="f307f-608">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="f307f-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="f307f-609">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="f307f-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="f307f-610">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-611">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-611">**Old behavior**</span></span>

<span data-ttu-id="f307f-612">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="f307f-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="f307f-613">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="f307f-614">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="f307f-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="f307f-615">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f307f-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="f307f-616">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-616">**New behavior**</span></span>

<span data-ttu-id="f307f-617">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="f307f-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="f307f-618">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-618">**Why**</span></span>

<span data-ttu-id="f307f-619">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="f307f-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="f307f-620">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-620">**Mitigations**</span></span>

<span data-ttu-id="f307f-621">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f307f-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="f307f-622">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="f307f-622">This is not common.</span></span>
<span data-ttu-id="f307f-623">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="f307f-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="f307f-624">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="f307f-625">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="f307f-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="f307f-626">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="f307f-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="f307f-627">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-628">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-628">**Old behavior**</span></span>

<span data-ttu-id="f307f-629">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="f307f-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="f307f-630">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="f307f-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="f307f-631">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-631">**New behavior**</span></span>

<span data-ttu-id="f307f-632">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="f307f-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="f307f-633">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-633">**Why**</span></span>

<span data-ttu-id="f307f-634">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="f307f-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="f307f-635">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-635">**Mitigations**</span></span>

<span data-ttu-id="f307f-636">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="f307f-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="f307f-637">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="f307f-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="f307f-638">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="f307f-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="f307f-639">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="f307f-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="f307f-640">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="f307f-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="f307f-641">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="f307f-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f307f-642">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-642">**Old behavior**</span></span>

<span data-ttu-id="f307f-643">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="f307f-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="f307f-644">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-644">**New behavior**</span></span>

<span data-ttu-id="f307f-645">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="f307f-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="f307f-646">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-646">**Why**</span></span>

<span data-ttu-id="f307f-647">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="f307f-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="f307f-648">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-648">**Mitigations**</span></span>

<span data-ttu-id="f307f-649">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="f307f-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="f307f-650">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="f307f-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="f307f-651">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="f307f-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="f307f-652">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="f307f-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="f307f-653">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-654">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-654">**Old behavior**</span></span>

<span data-ttu-id="f307f-655">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="f307f-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="f307f-656">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-656">**New behavior**</span></span>

<span data-ttu-id="f307f-657">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="f307f-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="f307f-658">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-658">**Why**</span></span>

<span data-ttu-id="f307f-659">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="f307f-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="f307f-660">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="f307f-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="f307f-661">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-661">**Mitigations**</span></span>

<span data-ttu-id="f307f-662">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="f307f-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="f307f-663">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="f307f-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="f307f-664">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="f307f-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="f307f-665">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-666">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-666">**Old behavior**</span></span>

<span data-ttu-id="f307f-667">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="f307f-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="f307f-668">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-668">**New behavior**</span></span>

<span data-ttu-id="f307f-669">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="f307f-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="f307f-670">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="f307f-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="f307f-671">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-671">**Why**</span></span>

<span data-ttu-id="f307f-672">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f307f-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="f307f-673">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-673">**Mitigations**</span></span>

<span data-ttu-id="f307f-674">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="f307f-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="f307f-675">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="f307f-675">Metadata API changes</span></span>

[<span data-ttu-id="f307f-676">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="f307f-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="f307f-677">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-678">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-678">**New behavior**</span></span>

<span data-ttu-id="f307f-679">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="f307f-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="f307f-680">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-680">**Why**</span></span>

<span data-ttu-id="f307f-681">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f307f-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="f307f-682">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-682">**Mitigations**</span></span>

<span data-ttu-id="f307f-683">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="f307f-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="f307f-684">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="f307f-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="f307f-685">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="f307f-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="f307f-686">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="f307f-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="f307f-687">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-687">**New behavior**</span></span>

<span data-ttu-id="f307f-688">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="f307f-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="f307f-689">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-689">**Why**</span></span>

<span data-ttu-id="f307f-690">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f307f-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="f307f-691">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-691">**Mitigations**</span></span>

<span data-ttu-id="f307f-692">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="f307f-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="f307f-693">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="f307f-694">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="f307f-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="f307f-695">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="f307f-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f307f-696">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-696">**Old behavior**</span></span>

<span data-ttu-id="f307f-697">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="f307f-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="f307f-698">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-698">**New behavior**</span></span>

<span data-ttu-id="f307f-699">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="f307f-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="f307f-700">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-700">**Why**</span></span>

<span data-ttu-id="f307f-701">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="f307f-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="f307f-702">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-702">**Mitigations**</span></span>

<span data-ttu-id="f307f-703">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="f307f-704">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="f307f-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="f307f-705">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="f307f-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="f307f-706">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-706">**Old behavior**</span></span>

<span data-ttu-id="f307f-707">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="f307f-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="f307f-708">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-708">**New behavior**</span></span>

<span data-ttu-id="f307f-709">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="f307f-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="f307f-710">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-710">**Why**</span></span>

<span data-ttu-id="f307f-711">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="f307f-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="f307f-712">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-712">**Mitigations**</span></span>

<span data-ttu-id="f307f-713">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="f307f-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f307f-714">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f307f-715">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="f307f-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="f307f-716">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-717">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-717">**Old behavior**</span></span>

<span data-ttu-id="f307f-718">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="f307f-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="f307f-719">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-719">**New behavior**</span></span>

<span data-ttu-id="f307f-720">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="f307f-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="f307f-721">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-721">**Why**</span></span>

<span data-ttu-id="f307f-722">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="f307f-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="f307f-723">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="f307f-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="f307f-724">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-724">**Mitigations**</span></span>

<span data-ttu-id="f307f-725">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="f307f-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="f307f-726">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="f307f-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="f307f-727">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="f307f-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f307f-728">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="f307f-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f307f-729">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="f307f-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="f307f-730">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-731">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-731">**Old behavior**</span></span>

<span data-ttu-id="f307f-732">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="f307f-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="f307f-733">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="f307f-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="f307f-734">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-734">**New behavior**</span></span>

<span data-ttu-id="f307f-735">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="f307f-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="f307f-736">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-736">**Why**</span></span>

<span data-ttu-id="f307f-737">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="f307f-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="f307f-738">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-738">**Mitigations**</span></span>

<span data-ttu-id="f307f-739">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="f307f-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="f307f-740">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="f307f-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="f307f-741">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="f307f-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="f307f-742">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="f307f-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="f307f-743">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="f307f-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="f307f-744">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-745">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-745">**Old behavior**</span></span>

<span data-ttu-id="f307f-746">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="f307f-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="f307f-747">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-747">**New behavior**</span></span>

<span data-ttu-id="f307f-748">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="f307f-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="f307f-749">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-749">**Why**</span></span>

<span data-ttu-id="f307f-750">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="f307f-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="f307f-751">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="f307f-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="f307f-752">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-752">**Mitigations**</span></span>

<span data-ttu-id="f307f-753">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="f307f-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="f307f-754">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="f307f-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="f307f-755">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="f307f-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="f307f-756">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="f307f-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="f307f-757">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="f307f-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="f307f-758">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="f307f-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="f307f-759">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="f307f-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="f307f-760">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-760">**Old behavior**</span></span>

<span data-ttu-id="f307f-761">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="f307f-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="f307f-762">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-762">**New behavior**</span></span>

<span data-ttu-id="f307f-763">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f307f-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="f307f-764">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-764">**Why**</span></span>

<span data-ttu-id="f307f-765">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="f307f-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="f307f-766">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-766">**Mitigations**</span></span>

<span data-ttu-id="f307f-767">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="f307f-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="f307f-768">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="f307f-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="f307f-769">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="f307f-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="f307f-770">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="f307f-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="f307f-771">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="f307f-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="f307f-772">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-773">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-773">**Old behavior**</span></span>

<span data-ttu-id="f307f-774">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="f307f-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="f307f-775">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-775">**New behavior**</span></span>

<span data-ttu-id="f307f-776">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="f307f-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="f307f-777">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-777">**Why**</span></span>

<span data-ttu-id="f307f-778">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="f307f-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="f307f-779">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="f307f-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="f307f-780">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-780">**Mitigations**</span></span>

<span data-ttu-id="f307f-781">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="f307f-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="f307f-782">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="f307f-783">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="f307f-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="f307f-784">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="f307f-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="f307f-785">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-786">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="f307f-786">**Change**</span></span>

<span data-ttu-id="f307f-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="f307f-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="f307f-788">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-788">**Why**</span></span>

<span data-ttu-id="f307f-789">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="f307f-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="f307f-790">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-790">**Mitigations**</span></span>

<span data-ttu-id="f307f-791">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="f307f-791">Use the new name.</span></span> <span data-ttu-id="f307f-792">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="f307f-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="f307f-793">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="f307f-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="f307f-794">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="f307f-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="f307f-795">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-796">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-796">**Old behavior**</span></span>

<span data-ttu-id="f307f-797">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="f307f-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="f307f-798">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="f307f-799">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-799">**New behavior**</span></span>

<span data-ttu-id="f307f-800">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="f307f-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="f307f-801">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="f307f-802">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-802">**Why**</span></span>

<span data-ttu-id="f307f-803">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="f307f-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="f307f-804">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-804">**Mitigations**</span></span>

<span data-ttu-id="f307f-805">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="f307f-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="f307f-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="f307f-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="f307f-807">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="f307f-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="f307f-808">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-809">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-809">**Old behavior**</span></span>

<span data-ttu-id="f307f-810">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="f307f-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="f307f-811">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-811">**New behavior**</span></span>

<span data-ttu-id="f307f-812">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="f307f-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="f307f-813">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-813">**Why**</span></span>

<span data-ttu-id="f307f-814">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="f307f-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="f307f-815">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="f307f-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="f307f-816">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-816">**Mitigations**</span></span>

<span data-ttu-id="f307f-817">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="f307f-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="f307f-818">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="f307f-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="f307f-819">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="f307f-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="f307f-820">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="f307f-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f307f-821">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-821">**Old behavior**</span></span>

<span data-ttu-id="f307f-822">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="f307f-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="f307f-823">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-823">**New behavior**</span></span>

<span data-ttu-id="f307f-824">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="f307f-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="f307f-825">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="f307f-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="f307f-826">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-826">**Why**</span></span>

<span data-ttu-id="f307f-827">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="f307f-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="f307f-828">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="f307f-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="f307f-829">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="f307f-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="f307f-830">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-830">**Mitigations**</span></span>

<span data-ttu-id="f307f-831">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f307f-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="f307f-832">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="f307f-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="f307f-833">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f307f-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="f307f-834">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="f307f-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="f307f-835">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-836">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-836">**Old behavior**</span></span>

<span data-ttu-id="f307f-837">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="f307f-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="f307f-838">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-838">**New behavior**</span></span>

<span data-ttu-id="f307f-839">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="f307f-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="f307f-840">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-840">**Why**</span></span>

<span data-ttu-id="f307f-841">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="f307f-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="f307f-842">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="f307f-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="f307f-843">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-843">**Mitigations**</span></span>

<span data-ttu-id="f307f-844">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="f307f-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="f307f-845">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="f307f-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="f307f-846">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="f307f-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="f307f-847">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="f307f-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="f307f-848">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="f307f-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="f307f-849">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-849">**Old behavior**</span></span>

<span data-ttu-id="f307f-850">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="f307f-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="f307f-851">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-851">**New behavior**</span></span>

<span data-ttu-id="f307f-852">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="f307f-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="f307f-853">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-853">**Why**</span></span>

<span data-ttu-id="f307f-854">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="f307f-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="f307f-855">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-855">**Mitigations**</span></span>

<span data-ttu-id="f307f-856">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="f307f-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="f307f-857">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="f307f-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="f307f-858">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="f307f-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="f307f-859">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="f307f-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="f307f-860">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="f307f-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="f307f-861">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="f307f-861">**Old behavior**</span></span>

<span data-ttu-id="f307f-862">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="f307f-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="f307f-863">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-863">For example:</span></span>

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

<span data-ttu-id="f307f-864">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="f307f-864">**New behavior**</span></span>

<span data-ttu-id="f307f-865">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="f307f-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="f307f-866">**Por que**</span><span class="sxs-lookup"><span data-stu-id="f307f-866">**Why**</span></span>

<span data-ttu-id="f307f-867">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="f307f-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="f307f-868">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="f307f-868">**Mitigations**</span></span>

<span data-ttu-id="f307f-869">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="f307f-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="f307f-870">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f307f-870">For example:</span></span>

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
