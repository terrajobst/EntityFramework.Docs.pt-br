---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 04487291f24bb702dad4b497c34234afdd5e3c9a
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005580"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="7e64d-102">Alterações recentes incluídas no EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="7e64d-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="7e64d-103">As alterações de API e comportamento a seguir têm o potencial de interromper os aplicativos existentes ao atualizá-los para o 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="7e64d-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="7e64d-104">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="7e64d-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="7e64d-105">As interrupções de uma visualização 3,0 para outra versão prévia 3,0 não estão documentadas aqui.</span><span class="sxs-lookup"><span data-stu-id="7e64d-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="7e64d-106">Resumo</span><span class="sxs-lookup"><span data-stu-id="7e64d-106">Summary</span></span>

| <span data-ttu-id="7e64d-107">**Alteração significativa**</span><span class="sxs-lookup"><span data-stu-id="7e64d-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="7e64d-108">**Causa**</span><span class="sxs-lookup"><span data-stu-id="7e64d-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="7e64d-109">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="7e64d-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="7e64d-110">Alto</span><span class="sxs-lookup"><span data-stu-id="7e64d-110">High</span></span>       |
| [<span data-ttu-id="7e64d-111">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="7e64d-112">Alto</span><span class="sxs-lookup"><span data-stu-id="7e64d-112">High</span></span>      |
| [<span data-ttu-id="7e64d-113">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="7e64d-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="7e64d-114">Alto</span><span class="sxs-lookup"><span data-stu-id="7e64d-114">High</span></span>      |
| [<span data-ttu-id="7e64d-115">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="7e64d-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="7e64d-116">Alto</span><span class="sxs-lookup"><span data-stu-id="7e64d-116">High</span></span>      |
| [<span data-ttu-id="7e64d-117">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="7e64d-118">Alto</span><span class="sxs-lookup"><span data-stu-id="7e64d-118">High</span></span>      |
| [<span data-ttu-id="7e64d-119">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e64d-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="7e64d-120">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-120">Medium</span></span>      |
| [<span data-ttu-id="7e64d-121">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="7e64d-122">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-122">Medium</span></span>      |
| [<span data-ttu-id="7e64d-123">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="7e64d-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="7e64d-124">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-124">Medium</span></span>      |
| [<span data-ttu-id="7e64d-125">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="7e64d-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="7e64d-126">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-126">Medium</span></span>      |
| [<span data-ttu-id="7e64d-127">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="7e64d-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="7e64d-128">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-128">Medium</span></span>      |
| [<span data-ttu-id="7e64d-129">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="7e64d-130">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-130">Medium</span></span>      |
| [<span data-ttu-id="7e64d-131">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="7e64d-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="7e64d-132">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-132">Medium</span></span>      |
| [<span data-ttu-id="7e64d-133">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="7e64d-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="7e64d-134">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-134">Medium</span></span>      |
| [<span data-ttu-id="7e64d-135">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="7e64d-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="7e64d-136">Média</span><span class="sxs-lookup"><span data-stu-id="7e64d-136">Medium</span></span>      |
| [<span data-ttu-id="7e64d-137">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="7e64d-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="7e64d-138">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-138">Low</span></span>      |
| [<span data-ttu-id="7e64d-139">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="7e64d-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="7e64d-140">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-140">Low</span></span>      |
| [<span data-ttu-id="7e64d-141">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="7e64d-142">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-142">Low</span></span>      |
| [<span data-ttu-id="7e64d-143">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="7e64d-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="7e64d-144">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-144">Low</span></span>      |
| [<span data-ttu-id="7e64d-145">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="7e64d-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="7e64d-146">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-146">Low</span></span>      |
| [<span data-ttu-id="7e64d-147">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="7e64d-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="7e64d-148">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-148">Low</span></span>      |
| [<span data-ttu-id="7e64d-149">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="7e64d-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="7e64d-150">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-150">Low</span></span>      |
| [<span data-ttu-id="7e64d-151">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="7e64d-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="7e64d-152">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-152">Low</span></span>      |
| [<span data-ttu-id="7e64d-153">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="7e64d-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="7e64d-154">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-154">Low</span></span>      |
| [<span data-ttu-id="7e64d-155">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="7e64d-156">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-156">Low</span></span>      |
| [<span data-ttu-id="7e64d-157">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="7e64d-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="7e64d-158">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-158">Low</span></span>      |
| [<span data-ttu-id="7e64d-159">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="7e64d-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="7e64d-160">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-160">Low</span></span>      |
| [<span data-ttu-id="7e64d-161">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="7e64d-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="7e64d-162">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-162">Low</span></span>      |
| [<span data-ttu-id="7e64d-163">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="7e64d-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="7e64d-164">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-164">Low</span></span>      |
| [<span data-ttu-id="7e64d-165">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="7e64d-166">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-166">Low</span></span>      |
| [<span data-ttu-id="7e64d-167">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="7e64d-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="7e64d-168">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-168">Low</span></span>      |
| [<span data-ttu-id="7e64d-169">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="7e64d-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="7e64d-170">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-170">Low</span></span>      |
| [<span data-ttu-id="7e64d-171">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="7e64d-172">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-172">Low</span></span>      |
| [<span data-ttu-id="7e64d-173">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="7e64d-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="7e64d-174">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-174">Low</span></span>      |
| [<span data-ttu-id="7e64d-175">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="7e64d-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="7e64d-176">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-176">Low</span></span>      |
| [<span data-ttu-id="7e64d-177">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="7e64d-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="7e64d-178">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-178">Low</span></span>      |
| [<span data-ttu-id="7e64d-179">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="7e64d-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="7e64d-180">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-180">Low</span></span>      |
| [<span data-ttu-id="7e64d-181">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="7e64d-182">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-182">Low</span></span>      |
| [<span data-ttu-id="7e64d-183">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="7e64d-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="7e64d-184">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-184">Low</span></span>      |
| [<span data-ttu-id="7e64d-185">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="7e64d-186">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-186">Low</span></span>      |
| [<span data-ttu-id="7e64d-187">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="7e64d-188">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-188">Low</span></span>      |
| [<span data-ttu-id="7e64d-189">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="7e64d-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="7e64d-190">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-190">Low</span></span>      |
| [<span data-ttu-id="7e64d-191">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="7e64d-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="7e64d-192">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-192">Low</span></span>      |
| [<span data-ttu-id="7e64d-193">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="7e64d-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="7e64d-194">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-194">Low</span></span>      |
| [<span data-ttu-id="7e64d-195">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="7e64d-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="7e64d-196">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-196">Low</span></span>      |
| [<span data-ttu-id="7e64d-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="7e64d-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="7e64d-198">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-198">Low</span></span>      |
| [<span data-ttu-id="7e64d-199">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="7e64d-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="7e64d-200">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-200">Low</span></span>      |
| [<span data-ttu-id="7e64d-201">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="7e64d-202">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-202">Low</span></span>      |
| [<span data-ttu-id="7e64d-203">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="7e64d-204">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-204">Low</span></span>      |
| [<span data-ttu-id="7e64d-205">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="7e64d-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="7e64d-206">Baixa</span><span class="sxs-lookup"><span data-stu-id="7e64d-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="7e64d-207">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="7e64d-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="7e64d-208">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="7e64d-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="7e64d-209">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-210">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-210">**Old behavior**</span></span>

<span data-ttu-id="7e64d-211">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="7e64d-212">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="7e64d-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="7e64d-213">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-213">**New behavior**</span></span>

<span data-ttu-id="7e64d-214">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="7e64d-215">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="7e64d-216">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-216">**Why**</span></span>

<span data-ttu-id="7e64d-217">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="7e64d-218">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="7e64d-219">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="7e64d-220">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="7e64d-221">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="7e64d-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="7e64d-222">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="7e64d-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="7e64d-223">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-223">**Mitigations**</span></span>

<span data-ttu-id="7e64d-224">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="7e64d-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="7e64d-225">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="7e64d-226">Problema de acompanhamento nº 15498</span><span class="sxs-lookup"><span data-stu-id="7e64d-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="7e64d-227">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-228">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-228">**Old behavior**</span></span>

<span data-ttu-id="7e64d-229">Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7e64d-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="7e64d-230">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-230">**New behavior**</span></span>

<span data-ttu-id="7e64d-231">No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="7e64d-232">Isso não inclui o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7e64d-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="7e64d-233">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-233">**Why**</span></span>

<span data-ttu-id="7e64d-234">Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.</span><span class="sxs-lookup"><span data-stu-id="7e64d-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="7e64d-235">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-235">**Mitigations**</span></span>

<span data-ttu-id="7e64d-236">Considere a possibilidade de migrar para uma plataforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="7e64d-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="7e64d-237">Se isso não for possível, continue usando o EF Core 2.1 ou o EF Core 2.2, que dão suporte ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7e64d-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="7e64d-238">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e64d-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="7e64d-239">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="7e64d-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="7e64d-240">Essa alteração foi introduzida no ASP.NET Core 3.0 versão prévia 1.</span><span class="sxs-lookup"><span data-stu-id="7e64d-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="7e64d-241">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-241">**Old behavior**</span></span>

<span data-ttu-id="7e64d-242">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e64d-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="7e64d-243">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-243">**New behavior**</span></span>

<span data-ttu-id="7e64d-244">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="7e64d-245">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-245">**Why**</span></span>

<span data-ttu-id="7e64d-246">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="7e64d-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="7e64d-247">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="7e64d-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="7e64d-248">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="7e64d-249">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="7e64d-250">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-250">**Mitigations**</span></span>

<span data-ttu-id="7e64d-251">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="7e64d-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="7e64d-252">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="7e64d-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="7e64d-253">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="7e64d-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="7e64d-254">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4 e na versão correspondente do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="7e64d-255">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-255">**Old behavior**</span></span>

<span data-ttu-id="7e64d-256">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="7e64d-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="7e64d-257">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-257">**New behavior**</span></span>

<span data-ttu-id="7e64d-258">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="7e64d-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="7e64d-259">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-259">**Why**</span></span>

<span data-ttu-id="7e64d-260">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="7e64d-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="7e64d-261">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-261">**Mitigations**</span></span>

<span data-ttu-id="7e64d-262">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="7e64d-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="7e64d-263">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="7e64d-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="7e64d-264">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="7e64d-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="7e64d-265">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="7e64d-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="7e64d-266">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-267">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-267">**Old behavior**</span></span>

<span data-ttu-id="7e64d-268">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="7e64d-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="7e64d-269">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-269">**New behavior**</span></span>

<span data-ttu-id="7e64d-270">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="7e64d-271">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="7e64d-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="7e64d-273">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="7e64d-274">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="7e64d-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="7e64d-275">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-275">**Why**</span></span>

<span data-ttu-id="7e64d-276">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="7e64d-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="7e64d-277">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="7e64d-278">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-278">**Mitigations**</span></span>

<span data-ttu-id="7e64d-279">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="7e64d-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="7e64d-280">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="7e64d-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="7e64d-281">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="7e64d-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="7e64d-282">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="7e64d-283">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-283">**Old behavior**</span></span>

<span data-ttu-id="7e64d-284">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="7e64d-285">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-285">**New behavior**</span></span>

<span data-ttu-id="7e64d-286">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="7e64d-287">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="7e64d-288">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-288">**Why**</span></span>

<span data-ttu-id="7e64d-289">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="7e64d-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="7e64d-290">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-290">**Mitigations**</span></span>

<span data-ttu-id="7e64d-291">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="7e64d-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="7e64d-292">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="7e64d-293">Problema de acompanhamento nº 13518</span><span class="sxs-lookup"><span data-stu-id="7e64d-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="7e64d-294">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="7e64d-295">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-295">**Old behavior**</span></span>

<span data-ttu-id="7e64d-296">Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="7e64d-297">Isso corresponde ao comportamento das consultas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="7e64d-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="7e64d-298">Por exemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="7e64d-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="7e64d-299">retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="7e64d-300">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-300">**New behavior**</span></span>

<span data-ttu-id="7e64d-301">No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="7e64d-302">Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.</span><span class="sxs-lookup"><span data-stu-id="7e64d-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="7e64d-303">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-303">**Why**</span></span>

<span data-ttu-id="7e64d-304">A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória.</span><span class="sxs-lookup"><span data-stu-id="7e64d-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="7e64d-305">Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="7e64d-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="7e64d-306">Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="7e64d-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="7e64d-307">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-307">**Mitigations**</span></span>

<span data-ttu-id="7e64d-308">Use uma consulta de acompanhamento se a resolução de identidade for necessária.</span><span class="sxs-lookup"><span data-stu-id="7e64d-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="7e64d-309">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="7e64d-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="7e64d-310">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="7e64d-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="7e64d-311">Essa alteração será revertida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-312">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="7e64d-313">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="7e64d-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="7e64d-314">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="7e64d-315">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="7e64d-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="7e64d-316">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="7e64d-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="7e64d-317">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-317">**Old behavior**</span></span>

<span data-ttu-id="7e64d-318">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="7e64d-319">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="7e64d-320">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-320">**New behavior**</span></span>

<span data-ttu-id="7e64d-321">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="7e64d-322">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-322">**Why**</span></span>

<span data-ttu-id="7e64d-323">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="7e64d-324">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-324">**Mitigations**</span></span>

<span data-ttu-id="7e64d-325">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="7e64d-326">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="7e64d-326">This can be avoided by:</span></span>
* <span data-ttu-id="7e64d-327">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="7e64d-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="7e64d-328">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="7e64d-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="7e64d-329">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="7e64d-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="7e64d-330">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="7e64d-331">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="7e64d-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="7e64d-332">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="7e64d-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="7e64d-333">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-334">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-334">**Old behavior**</span></span>

<span data-ttu-id="7e64d-335">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="7e64d-336">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-336">**New behavior**</span></span>

<span data-ttu-id="7e64d-337">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="7e64d-338">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="7e64d-339">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="7e64d-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="7e64d-340">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-340">**Why**</span></span>

<span data-ttu-id="7e64d-341">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="7e64d-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="7e64d-342">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-342">**Mitigations**</span></span>

<span data-ttu-id="7e64d-343">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="7e64d-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="7e64d-344">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="7e64d-345">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="7e64d-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="7e64d-346">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="7e64d-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="7e64d-347">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="7e64d-348">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="7e64d-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="7e64d-349">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-350">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-350">**Old behavior**</span></span>

<span data-ttu-id="7e64d-351">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="7e64d-352">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-352">**New behavior**</span></span>

<span data-ttu-id="7e64d-353">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="7e64d-354">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="7e64d-355">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-355">**Why**</span></span>

<span data-ttu-id="7e64d-356">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="7e64d-357">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-357">**Mitigations**</span></span>

<span data-ttu-id="7e64d-358">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="7e64d-359">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="7e64d-360">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="7e64d-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="7e64d-361">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="7e64d-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="7e64d-362">Essa alteração foi introduzida no EF Core 3.0 versão prévia 5.</span><span class="sxs-lookup"><span data-stu-id="7e64d-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="7e64d-363">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-363">**Old behavior**</span></span>

<span data-ttu-id="7e64d-364">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="7e64d-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="7e64d-365">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-365">**New behavior**</span></span>

<span data-ttu-id="7e64d-366">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="7e64d-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="7e64d-367">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-367">**Why**</span></span>

<span data-ttu-id="7e64d-368">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="7e64d-369">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-369">**Mitigations**</span></span>

<span data-ttu-id="7e64d-370">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="7e64d-371">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="7e64d-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="7e64d-372">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="7e64d-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="7e64d-373">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-374">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-374">**Old behavior**</span></span>

<span data-ttu-id="7e64d-375">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-375">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="7e64d-376">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="7e64d-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="7e64d-377">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-377">**New behavior**</span></span>

<span data-ttu-id="7e64d-378">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="7e64d-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="7e64d-379">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="7e64d-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="7e64d-380">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-380">**Why**</span></span>

<span data-ttu-id="7e64d-381">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="7e64d-382">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="7e64d-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="7e64d-383">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="7e64d-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="7e64d-384">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-384">**Mitigations**</span></span>

<span data-ttu-id="7e64d-385">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="7e64d-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="7e64d-386">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="7e64d-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="7e64d-387">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="7e64d-388">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="7e64d-389">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="7e64d-390">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="7e64d-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="7e64d-391">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="7e64d-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="7e64d-392">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-393">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-393">**Old behavior**</span></span>

<span data-ttu-id="7e64d-394">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="7e64d-395">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-395">**New behavior**</span></span>

<span data-ttu-id="7e64d-396">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="7e64d-397">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="7e64d-398">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="7e64d-399">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="7e64d-400">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-400">For example:</span></span>

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

<span data-ttu-id="7e64d-401">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="7e64d-402">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-402">**Why**</span></span>

<span data-ttu-id="7e64d-403">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="7e64d-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="7e64d-404">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="7e64d-405">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-405">**Mitigations**</span></span>

<span data-ttu-id="7e64d-406">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="7e64d-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="7e64d-407">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="7e64d-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="7e64d-408">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="7e64d-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="7e64d-409">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-410">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-410">**Old behavior**</span></span>

<span data-ttu-id="7e64d-411">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7e64d-411">Consider the following model:</span></span>
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
<span data-ttu-id="7e64d-412">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="7e64d-413">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-413">**New behavior**</span></span>

<span data-ttu-id="7e64d-414">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="7e64d-415">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="7e64d-416">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-416">**Mitigations**</span></span>

<span data-ttu-id="7e64d-417">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="7e64d-418">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="7e64d-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="7e64d-419">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="7e64d-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="7e64d-420">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="7e64d-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="7e64d-421">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-422">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-422">**Old behavior**</span></span>

<span data-ttu-id="7e64d-423">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7e64d-423">Consider the following model:</span></span>
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
<span data-ttu-id="7e64d-424">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="7e64d-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="7e64d-425">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-425">**New behavior**</span></span>

<span data-ttu-id="7e64d-426">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="7e64d-427">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="7e64d-428">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-428">**Why**</span></span>

<span data-ttu-id="7e64d-429">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="7e64d-430">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-430">**Mitigations**</span></span>

<span data-ttu-id="7e64d-431">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="7e64d-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="7e64d-432">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="7e64d-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="7e64d-433">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="7e64d-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="7e64d-434">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="7e64d-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="7e64d-435">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-436">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-436">**Old behavior**</span></span>

<span data-ttu-id="7e64d-437">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7e64d-437">Consider the following model:</span></span>
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

<span data-ttu-id="7e64d-438">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="7e64d-439">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-439">**New behavior**</span></span>

<span data-ttu-id="7e64d-440">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="7e64d-441">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-441">**Why**</span></span>

<span data-ttu-id="7e64d-442">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="7e64d-443">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-443">**Mitigations**</span></span>

<span data-ttu-id="7e64d-444">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="7e64d-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="7e64d-445">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="7e64d-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="7e64d-446">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="7e64d-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="7e64d-447">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-448">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-448">**Old behavior**</span></span>

<span data-ttu-id="7e64d-449">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7e64d-449">Consider the following model:</span></span>
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
<span data-ttu-id="7e64d-450">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="7e64d-451">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="7e64d-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="7e64d-452">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-452">**New behavior**</span></span>

<span data-ttu-id="7e64d-453">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="7e64d-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="7e64d-454">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="7e64d-455">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-455">For example:</span></span>

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

<span data-ttu-id="7e64d-456">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-456">**Why**</span></span>

<span data-ttu-id="7e64d-457">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="7e64d-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="7e64d-458">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-458">**Mitigations**</span></span>

<span data-ttu-id="7e64d-459">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="7e64d-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="7e64d-460">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="7e64d-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="7e64d-461">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="7e64d-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="7e64d-462">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-463">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-463">**Old behavior**</span></span>

<span data-ttu-id="7e64d-464">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="7e64d-465">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-465">**New behavior**</span></span>

<span data-ttu-id="7e64d-466">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="7e64d-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="7e64d-467">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-467">**Why**</span></span>

<span data-ttu-id="7e64d-468">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="7e64d-469">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="7e64d-470">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-470">**Mitigations**</span></span>

<span data-ttu-id="7e64d-471">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="7e64d-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="7e64d-472">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="7e64d-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="7e64d-473">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="7e64d-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="7e64d-474">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-475">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-475">**Old behavior**</span></span>

<span data-ttu-id="7e64d-476">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="7e64d-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="7e64d-477">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-477">**New behavior**</span></span>

<span data-ttu-id="7e64d-478">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="7e64d-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="7e64d-479">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="7e64d-480">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-480">**Why**</span></span>

<span data-ttu-id="7e64d-481">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="7e64d-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="7e64d-482">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-482">**Mitigations**</span></span>

<span data-ttu-id="7e64d-483">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="7e64d-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="7e64d-484">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="7e64d-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="7e64d-485">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-485">Backing fields are used by default</span></span>

[<span data-ttu-id="7e64d-486">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="7e64d-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="7e64d-487">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="7e64d-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="7e64d-488">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-488">**Old behavior**</span></span>

<span data-ttu-id="7e64d-489">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="7e64d-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="7e64d-490">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="7e64d-491">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-491">**New behavior**</span></span>

<span data-ttu-id="7e64d-492">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="7e64d-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="7e64d-493">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="7e64d-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="7e64d-494">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-494">**Why**</span></span>

<span data-ttu-id="7e64d-495">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="7e64d-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="7e64d-496">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-496">**Mitigations**</span></span>

<span data-ttu-id="7e64d-497">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="7e64d-498">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="7e64d-499">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="7e64d-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="7e64d-500">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="7e64d-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="7e64d-501">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-502">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-502">**Old behavior**</span></span>

<span data-ttu-id="7e64d-503">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="7e64d-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="7e64d-504">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="7e64d-505">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-505">**New behavior**</span></span>

<span data-ttu-id="7e64d-506">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="7e64d-507">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-507">**Why**</span></span>

<span data-ttu-id="7e64d-508">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="7e64d-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="7e64d-509">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-509">**Mitigations**</span></span>

<span data-ttu-id="7e64d-510">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="7e64d-511">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="7e64d-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="7e64d-512">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="7e64d-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="7e64d-513">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-514">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-514">**Old behavior**</span></span>

<span data-ttu-id="7e64d-515">Antes do EF Core 3.0, uma propriedade podia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome fosse encontrada no tipo CLR, o EF Core tentaria correspondê-lo a um campo, usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="7e64d-516">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-516">**New behavior**</span></span>

<span data-ttu-id="7e64d-517">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="7e64d-518">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-518">**Why**</span></span>

<span data-ttu-id="7e64d-519">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="7e64d-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="7e64d-520">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-520">**Mitigations**</span></span>

<span data-ttu-id="7e64d-521">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="7e64d-522">Em uma versão futura do EF Core após 3,0, planejamos reabilitar explicitamente um nome de campo diferente do nome da propriedade (consulte o problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="7e64d-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="7e64d-523">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="7e64d-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="7e64d-524">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="7e64d-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="7e64d-525">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-526">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-526">**Old behavior**</span></span>

<span data-ttu-id="7e64d-527">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="7e64d-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="7e64d-528">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-528">**New behavior**</span></span>

<span data-ttu-id="7e64d-529">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="7e64d-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="7e64d-530">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-530">**Why**</span></span>

<span data-ttu-id="7e64d-531">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="7e64d-532">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="7e64d-533">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-533">**Mitigations**</span></span>

<span data-ttu-id="7e64d-534">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="7e64d-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="7e64d-535">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="7e64d-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="7e64d-536">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="7e64d-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="7e64d-537">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-538">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-538">**Old behavior**</span></span>

<span data-ttu-id="7e64d-539">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="7e64d-540">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="7e64d-541">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-541">**New behavior**</span></span>

<span data-ttu-id="7e64d-542">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="7e64d-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="7e64d-543">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="7e64d-544">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="7e64d-545">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="7e64d-546">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-546">**Why**</span></span>

<span data-ttu-id="7e64d-547">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="7e64d-548">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-548">**Mitigations**</span></span>

<span data-ttu-id="7e64d-549">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="7e64d-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="7e64d-550">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="7e64d-551">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="7e64d-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="7e64d-552">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-553">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-553">**Old behavior**</span></span>

<span data-ttu-id="7e64d-554">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="7e64d-555">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="7e64d-556">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-556">**New behavior**</span></span>

<span data-ttu-id="7e64d-557">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="7e64d-558">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-558">**Why**</span></span>

<span data-ttu-id="7e64d-559">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="7e64d-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="7e64d-560">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-560">**Mitigations**</span></span>

<span data-ttu-id="7e64d-561">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="7e64d-562">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="7e64d-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="7e64d-563">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="7e64d-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="7e64d-564">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="7e64d-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="7e64d-565">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="7e64d-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="7e64d-566">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-567">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-567">**Old behavior**</span></span>

<span data-ttu-id="7e64d-568">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="7e64d-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="7e64d-569">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-569">**New behavior**</span></span>

<span data-ttu-id="7e64d-570">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="7e64d-571">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-571">**Why**</span></span>

<span data-ttu-id="7e64d-572">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="7e64d-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="7e64d-573">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-573">**Mitigations**</span></span>

<span data-ttu-id="7e64d-574">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="7e64d-575">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="7e64d-575">This isn't common.</span></span>
<span data-ttu-id="7e64d-576">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="7e64d-577">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="7e64d-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="7e64d-578">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="7e64d-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="7e64d-579">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="7e64d-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="7e64d-580">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-581">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-581">**Old behavior**</span></span>

<span data-ttu-id="7e64d-582">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="7e64d-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="7e64d-583">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="7e64d-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="7e64d-584">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="7e64d-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="7e64d-585">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-585">**New behavior**</span></span>

<span data-ttu-id="7e64d-586">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="7e64d-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="7e64d-587">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="7e64d-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="7e64d-588">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="7e64d-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="7e64d-589">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="7e64d-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="7e64d-590">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-590">**Why**</span></span>

<span data-ttu-id="7e64d-591">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="7e64d-592">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-592">**Mitigations**</span></span>

<span data-ttu-id="7e64d-593">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="7e64d-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="7e64d-594">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="7e64d-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="7e64d-595">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="7e64d-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="7e64d-596">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-597">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-597">**Old behavior**</span></span>

<span data-ttu-id="7e64d-598">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="7e64d-599">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-599">**New behavior**</span></span>

<span data-ttu-id="7e64d-600">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="7e64d-601">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-601">**Why**</span></span>

<span data-ttu-id="7e64d-602">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="7e64d-603">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-603">**Mitigations**</span></span>

<span data-ttu-id="7e64d-604">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="7e64d-605">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="7e64d-606">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="7e64d-607">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="7e64d-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="7e64d-608">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="7e64d-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="7e64d-609">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-610">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-610">**Old behavior**</span></span>

<span data-ttu-id="7e64d-611">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="7e64d-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="7e64d-612">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="7e64d-613">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="7e64d-614">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="7e64d-615">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-615">**New behavior**</span></span>

<span data-ttu-id="7e64d-616">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="7e64d-617">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-617">**Why**</span></span>

<span data-ttu-id="7e64d-618">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="7e64d-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="7e64d-619">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-619">**Mitigations**</span></span>

<span data-ttu-id="7e64d-620">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="7e64d-621">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="7e64d-621">This is not common.</span></span>
<span data-ttu-id="7e64d-622">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="7e64d-623">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="7e64d-624">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="7e64d-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="7e64d-625">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="7e64d-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="7e64d-626">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-627">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-627">**Old behavior**</span></span>

<span data-ttu-id="7e64d-628">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="7e64d-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="7e64d-629">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="7e64d-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="7e64d-630">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-630">**New behavior**</span></span>

<span data-ttu-id="7e64d-631">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="7e64d-632">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-632">**Why**</span></span>

<span data-ttu-id="7e64d-633">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="7e64d-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="7e64d-634">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-634">**Mitigations**</span></span>

<span data-ttu-id="7e64d-635">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="7e64d-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="7e64d-636">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="7e64d-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="7e64d-637">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="7e64d-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="7e64d-638">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="7e64d-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="7e64d-639">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="7e64d-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="7e64d-640">Essa alteração foi introduzida no EF Core 3.0 versão prévia 2.</span><span class="sxs-lookup"><span data-stu-id="7e64d-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="7e64d-641">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-641">**Old behavior**</span></span>

<span data-ttu-id="7e64d-642">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="7e64d-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="7e64d-643">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-643">**New behavior**</span></span>

<span data-ttu-id="7e64d-644">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="7e64d-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="7e64d-645">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-645">**Why**</span></span>

<span data-ttu-id="7e64d-646">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="7e64d-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="7e64d-647">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-647">**Mitigations**</span></span>

<span data-ttu-id="7e64d-648">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="7e64d-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="7e64d-649">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="7e64d-650">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="7e64d-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="7e64d-651">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="7e64d-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="7e64d-652">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-653">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-653">**Old behavior**</span></span>

<span data-ttu-id="7e64d-654">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="7e64d-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="7e64d-655">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-655">**New behavior**</span></span>

<span data-ttu-id="7e64d-656">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="7e64d-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="7e64d-657">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-657">**Why**</span></span>

<span data-ttu-id="7e64d-658">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="7e64d-659">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="7e64d-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="7e64d-660">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-660">**Mitigations**</span></span>

<span data-ttu-id="7e64d-661">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="7e64d-662">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="7e64d-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="7e64d-663">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="7e64d-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="7e64d-664">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-665">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-665">**Old behavior**</span></span>

<span data-ttu-id="7e64d-666">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="7e64d-667">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-667">**New behavior**</span></span>

<span data-ttu-id="7e64d-668">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="7e64d-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="7e64d-669">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="7e64d-670">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-670">**Why**</span></span>

<span data-ttu-id="7e64d-671">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="7e64d-672">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-672">**Mitigations**</span></span>

<span data-ttu-id="7e64d-673">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="7e64d-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="7e64d-674">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="7e64d-674">Metadata API changes</span></span>

[<span data-ttu-id="7e64d-675">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="7e64d-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="7e64d-676">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-677">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-677">**New behavior**</span></span>

<span data-ttu-id="7e64d-678">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="7e64d-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="7e64d-679">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-679">**Why**</span></span>

<span data-ttu-id="7e64d-680">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="7e64d-681">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-681">**Mitigations**</span></span>

<span data-ttu-id="7e64d-682">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="7e64d-683">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="7e64d-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="7e64d-684">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="7e64d-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="7e64d-685">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="7e64d-686">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-686">**New behavior**</span></span>

<span data-ttu-id="7e64d-687">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="7e64d-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="7e64d-688">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-688">**Why**</span></span>

<span data-ttu-id="7e64d-689">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="7e64d-690">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-690">**Mitigations**</span></span>

<span data-ttu-id="7e64d-691">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="7e64d-692">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="7e64d-693">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="7e64d-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="7e64d-694">Essa alteração foi introduzida no EF Core 3.0 versão prévia 3.</span><span class="sxs-lookup"><span data-stu-id="7e64d-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="7e64d-695">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-695">**Old behavior**</span></span>

<span data-ttu-id="7e64d-696">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="7e64d-697">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-697">**New behavior**</span></span>

<span data-ttu-id="7e64d-698">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="7e64d-699">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-699">**Why**</span></span>

<span data-ttu-id="7e64d-700">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="7e64d-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="7e64d-701">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-701">**Mitigations**</span></span>

<span data-ttu-id="7e64d-702">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="7e64d-703">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="7e64d-704">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="7e64d-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="7e64d-705">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-705">**Old behavior**</span></span>

<span data-ttu-id="7e64d-706">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="7e64d-707">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-707">**New behavior**</span></span>

<span data-ttu-id="7e64d-708">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="7e64d-709">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-709">**Why**</span></span>

<span data-ttu-id="7e64d-710">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="7e64d-711">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-711">**Mitigations**</span></span>

<span data-ttu-id="7e64d-712">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="7e64d-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="7e64d-713">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="7e64d-714">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="7e64d-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="7e64d-715">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-716">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-716">**Old behavior**</span></span>

<span data-ttu-id="7e64d-717">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="7e64d-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="7e64d-718">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-718">**New behavior**</span></span>

<span data-ttu-id="7e64d-719">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="7e64d-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="7e64d-720">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-720">**Why**</span></span>

<span data-ttu-id="7e64d-721">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="7e64d-722">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="7e64d-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="7e64d-723">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-723">**Mitigations**</span></span>

<span data-ttu-id="7e64d-724">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="7e64d-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="7e64d-725">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="7e64d-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="7e64d-726">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="7e64d-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="7e64d-727">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="7e64d-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="7e64d-728">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="7e64d-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="7e64d-729">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-730">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-730">**Old behavior**</span></span>

<span data-ttu-id="7e64d-731">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="7e64d-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="7e64d-732">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="7e64d-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="7e64d-733">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-733">**New behavior**</span></span>

<span data-ttu-id="7e64d-734">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="7e64d-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="7e64d-735">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-735">**Why**</span></span>

<span data-ttu-id="7e64d-736">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="7e64d-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="7e64d-737">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-737">**Mitigations**</span></span>

<span data-ttu-id="7e64d-738">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="7e64d-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="7e64d-739">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="7e64d-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="7e64d-740">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="7e64d-741">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="7e64d-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="7e64d-742">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="7e64d-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="7e64d-743">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-744">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-744">**Old behavior**</span></span>

<span data-ttu-id="7e64d-745">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="7e64d-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="7e64d-746">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-746">**New behavior**</span></span>

<span data-ttu-id="7e64d-747">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="7e64d-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="7e64d-748">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-748">**Why**</span></span>

<span data-ttu-id="7e64d-749">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="7e64d-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="7e64d-750">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="7e64d-751">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-751">**Mitigations**</span></span>

<span data-ttu-id="7e64d-752">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="7e64d-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="7e64d-753">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="7e64d-754">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="7e64d-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="7e64d-755">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="7e64d-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="7e64d-756">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="7e64d-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="7e64d-757">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="7e64d-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="7e64d-758">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="7e64d-759">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-759">**Old behavior**</span></span>

<span data-ttu-id="7e64d-760">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="7e64d-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="7e64d-761">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-761">**New behavior**</span></span>

<span data-ttu-id="7e64d-762">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e64d-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="7e64d-763">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-763">**Why**</span></span>

<span data-ttu-id="7e64d-764">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="7e64d-765">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-765">**Mitigations**</span></span>

<span data-ttu-id="7e64d-766">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="7e64d-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="7e64d-767">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="7e64d-768">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="7e64d-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="7e64d-769">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="7e64d-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="7e64d-770">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="7e64d-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="7e64d-771">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-772">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-772">**Old behavior**</span></span>

<span data-ttu-id="7e64d-773">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="7e64d-774">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-774">**New behavior**</span></span>

<span data-ttu-id="7e64d-775">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="7e64d-776">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-776">**Why**</span></span>

<span data-ttu-id="7e64d-777">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="7e64d-778">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="7e64d-779">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-779">**Mitigations**</span></span>

<span data-ttu-id="7e64d-780">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="7e64d-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="7e64d-781">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="7e64d-782">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="7e64d-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="7e64d-783">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="7e64d-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="7e64d-784">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-785">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="7e64d-785">**Change**</span></span>

<span data-ttu-id="7e64d-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="7e64d-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="7e64d-787">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-787">**Why**</span></span>

<span data-ttu-id="7e64d-788">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="7e64d-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="7e64d-789">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-789">**Mitigations**</span></span>

<span data-ttu-id="7e64d-790">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="7e64d-790">Use the new name.</span></span> <span data-ttu-id="7e64d-791">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="7e64d-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="7e64d-792">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="7e64d-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="7e64d-793">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="7e64d-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="7e64d-794">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-795">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-795">**Old behavior**</span></span>

<span data-ttu-id="7e64d-796">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="7e64d-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="7e64d-797">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="7e64d-798">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-798">**New behavior**</span></span>

<span data-ttu-id="7e64d-799">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="7e64d-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="7e64d-800">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="7e64d-801">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-801">**Why**</span></span>

<span data-ttu-id="7e64d-802">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="7e64d-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="7e64d-803">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-803">**Mitigations**</span></span>

<span data-ttu-id="7e64d-804">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="7e64d-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="7e64d-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="7e64d-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="7e64d-806">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="7e64d-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="7e64d-807">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-808">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-808">**Old behavior**</span></span>

<span data-ttu-id="7e64d-809">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="7e64d-810">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-810">**New behavior**</span></span>

<span data-ttu-id="7e64d-811">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="7e64d-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="7e64d-812">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-812">**Why**</span></span>

<span data-ttu-id="7e64d-813">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="7e64d-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="7e64d-814">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="7e64d-815">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-815">**Mitigations**</span></span>

<span data-ttu-id="7e64d-816">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="7e64d-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="7e64d-817">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="7e64d-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="7e64d-818">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="7e64d-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="7e64d-819">Essa alteração foi introduzida no EF Core 3.0 versão prévia 4.</span><span class="sxs-lookup"><span data-stu-id="7e64d-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e64d-820">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-820">**Old behavior**</span></span>

<span data-ttu-id="7e64d-821">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="7e64d-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="7e64d-822">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-822">**New behavior**</span></span>

<span data-ttu-id="7e64d-823">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="7e64d-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="7e64d-824">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="7e64d-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="7e64d-825">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-825">**Why**</span></span>

<span data-ttu-id="7e64d-826">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="7e64d-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="7e64d-827">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="7e64d-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="7e64d-828">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="7e64d-829">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-829">**Mitigations**</span></span>

<span data-ttu-id="7e64d-830">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="7e64d-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="7e64d-831">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="7e64d-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="7e64d-832">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="7e64d-833">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="7e64d-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="7e64d-834">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-835">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-835">**Old behavior**</span></span>

<span data-ttu-id="7e64d-836">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="7e64d-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="7e64d-837">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-837">**New behavior**</span></span>

<span data-ttu-id="7e64d-838">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7e64d-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="7e64d-839">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-839">**Why**</span></span>

<span data-ttu-id="7e64d-840">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="7e64d-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="7e64d-841">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="7e64d-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="7e64d-842">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-842">**Mitigations**</span></span>

<span data-ttu-id="7e64d-843">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="7e64d-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="7e64d-844">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="7e64d-845">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7e64d-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="7e64d-846">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="7e64d-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="7e64d-847">Essa alteração foi introduzida no EF Core 3.0 – versão prévia 7.</span><span class="sxs-lookup"><span data-stu-id="7e64d-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="7e64d-848">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-848">**Old behavior**</span></span>

<span data-ttu-id="7e64d-849">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="7e64d-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="7e64d-850">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-850">**New behavior**</span></span>

<span data-ttu-id="7e64d-851">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="7e64d-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="7e64d-852">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-852">**Why**</span></span>

<span data-ttu-id="7e64d-853">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e64d-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="7e64d-854">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-854">**Mitigations**</span></span>

<span data-ttu-id="7e64d-855">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="7e64d-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="7e64d-856">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="7e64d-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="7e64d-857">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="7e64d-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="7e64d-858">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="7e64d-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="7e64d-859">Essa alteração foi introduzida no EF Core 3.0 versão prévia 6.</span><span class="sxs-lookup"><span data-stu-id="7e64d-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="7e64d-860">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-860">**Old behavior**</span></span>

<span data-ttu-id="7e64d-861">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="7e64d-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="7e64d-862">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-862">For example:</span></span>

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

<span data-ttu-id="7e64d-863">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="7e64d-863">**New behavior**</span></span>

<span data-ttu-id="7e64d-864">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="7e64d-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="7e64d-865">**Por que**</span><span class="sxs-lookup"><span data-stu-id="7e64d-865">**Why**</span></span>

<span data-ttu-id="7e64d-866">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="7e64d-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="7e64d-867">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="7e64d-867">**Mitigations**</span></span>

<span data-ttu-id="7e64d-868">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="7e64d-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="7e64d-869">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7e64d-869">For example:</span></span>

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
