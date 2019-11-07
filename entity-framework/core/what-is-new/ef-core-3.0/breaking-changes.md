---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f02825f5303959997dca6e14e4efe64020b3cb22
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655885"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="dcaee-102">Alterações recentes incluídas no EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="dcaee-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="dcaee-103">As alterações de API e comportamento a seguir têm o potencial de interromper os aplicativos existentes ao atualizá-los para o 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="dcaee-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="dcaee-104">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="dcaee-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="dcaee-105">Resumo</span><span class="sxs-lookup"><span data-stu-id="dcaee-105">Summary</span></span>

| <span data-ttu-id="dcaee-106">**Alteração significativa**</span><span class="sxs-lookup"><span data-stu-id="dcaee-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="dcaee-107">**Causa**</span><span class="sxs-lookup"><span data-stu-id="dcaee-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="dcaee-108">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="dcaee-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="dcaee-109">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-109">High</span></span>       |
| [<span data-ttu-id="dcaee-110">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="dcaee-111">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-111">High</span></span>      |
| [<span data-ttu-id="dcaee-112">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="dcaee-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="dcaee-113">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-113">High</span></span>      |
| [<span data-ttu-id="dcaee-114">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="dcaee-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="dcaee-115">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-115">High</span></span>      |
| [<span data-ttu-id="dcaee-116">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="dcaee-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="dcaee-117">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-117">High</span></span>      |
| [<span data-ttu-id="dcaee-118">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="dcaee-119">Alta</span><span class="sxs-lookup"><span data-stu-id="dcaee-119">High</span></span>      |
| [<span data-ttu-id="dcaee-120">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcaee-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="dcaee-121">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-121">Medium</span></span>      |
| [<span data-ttu-id="dcaee-122">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="dcaee-123">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-123">Medium</span></span>      |
| [<span data-ttu-id="dcaee-124">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="dcaee-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="dcaee-125">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-125">Medium</span></span>      |
| [<span data-ttu-id="dcaee-126">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="dcaee-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="dcaee-127">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-127">Medium</span></span>      |
| [<span data-ttu-id="dcaee-128">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="dcaee-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="dcaee-129">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-129">Medium</span></span>      |
| [<span data-ttu-id="dcaee-130">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="dcaee-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="dcaee-131">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-131">Medium</span></span>      |
| [<span data-ttu-id="dcaee-132">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="dcaee-133">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-133">Medium</span></span>      |
| [<span data-ttu-id="dcaee-134">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="dcaee-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="dcaee-135">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-135">Medium</span></span>      |
| [<span data-ttu-id="dcaee-136">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="dcaee-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="dcaee-137">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-137">Medium</span></span>      |
| [<span data-ttu-id="dcaee-138">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="dcaee-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="dcaee-139">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-139">Medium</span></span>      |
| [<span data-ttu-id="dcaee-140">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="dcaee-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="dcaee-141">Médio</span><span class="sxs-lookup"><span data-stu-id="dcaee-141">Medium</span></span>      |
| [<span data-ttu-id="dcaee-142">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="dcaee-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="dcaee-143">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-143">Low</span></span>      |
| [<span data-ttu-id="dcaee-144">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="dcaee-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="dcaee-145">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-145">Low</span></span>      |
| [<span data-ttu-id="dcaee-146">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="dcaee-147">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-147">Low</span></span>      |
| [<span data-ttu-id="dcaee-148">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="dcaee-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="dcaee-149">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-149">Low</span></span>      |
| [<span data-ttu-id="dcaee-150">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="dcaee-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="dcaee-151">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-151">Low</span></span>      |
| [<span data-ttu-id="dcaee-152">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="dcaee-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="dcaee-153">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-153">Low</span></span>      |
| [<span data-ttu-id="dcaee-154">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="dcaee-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="dcaee-155">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-155">Low</span></span>      |
| [<span data-ttu-id="dcaee-156">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="dcaee-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="dcaee-157">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-157">Low</span></span>      |
| [<span data-ttu-id="dcaee-158">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="dcaee-159">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-159">Low</span></span>      |
| [<span data-ttu-id="dcaee-160">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="dcaee-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="dcaee-161">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-161">Low</span></span>      |
| [<span data-ttu-id="dcaee-162">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="dcaee-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="dcaee-163">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-163">Low</span></span>      |
| [<span data-ttu-id="dcaee-164">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="dcaee-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="dcaee-165">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-165">Low</span></span>      |
| [<span data-ttu-id="dcaee-166">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="dcaee-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="dcaee-167">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-167">Low</span></span>      |
| [<span data-ttu-id="dcaee-168">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="dcaee-169">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-169">Low</span></span>      |
| [<span data-ttu-id="dcaee-170">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="dcaee-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="dcaee-171">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-171">Low</span></span>      |
| [<span data-ttu-id="dcaee-172">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="dcaee-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="dcaee-173">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-173">Low</span></span>      |
| [<span data-ttu-id="dcaee-174">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="dcaee-175">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-175">Low</span></span>      |
| [<span data-ttu-id="dcaee-176">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="dcaee-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="dcaee-177">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-177">Low</span></span>      |
| [<span data-ttu-id="dcaee-178">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="dcaee-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="dcaee-179">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-179">Low</span></span>      |
| [<span data-ttu-id="dcaee-180">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="dcaee-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="dcaee-181">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-181">Low</span></span>      |
| [<span data-ttu-id="dcaee-182">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="dcaee-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="dcaee-183">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-183">Low</span></span>      |
| [<span data-ttu-id="dcaee-184">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="dcaee-185">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-185">Low</span></span>      |
| [<span data-ttu-id="dcaee-186">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="dcaee-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="dcaee-187">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-187">Low</span></span>      |
| [<span data-ttu-id="dcaee-188">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="dcaee-189">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-189">Low</span></span>      |
| [<span data-ttu-id="dcaee-190">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="dcaee-191">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-191">Low</span></span>      |
| [<span data-ttu-id="dcaee-192">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="dcaee-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="dcaee-193">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-193">Low</span></span>      |
| [<span data-ttu-id="dcaee-194">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="dcaee-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="dcaee-195">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-195">Low</span></span>      |
| [<span data-ttu-id="dcaee-196">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="dcaee-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="dcaee-197">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-197">Low</span></span>      |
| [<span data-ttu-id="dcaee-198">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="dcaee-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="dcaee-199">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-199">Low</span></span>      |
| [<span data-ttu-id="dcaee-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="dcaee-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="dcaee-201">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-201">Low</span></span>      |
| [<span data-ttu-id="dcaee-202">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="dcaee-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="dcaee-203">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-203">Low</span></span>      |
| [<span data-ttu-id="dcaee-204">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="dcaee-205">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-205">Low</span></span>      |
| [<span data-ttu-id="dcaee-206">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="dcaee-207">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-207">Low</span></span>      |
| [<span data-ttu-id="dcaee-208">Microsoft. Data. SqlClient é usado em vez de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="dcaee-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="dcaee-209">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-209">Low</span></span>      |
| [<span data-ttu-id="dcaee-210">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="dcaee-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="dcaee-211">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-211">Low</span></span>      |
| [<span data-ttu-id="dcaee-212">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="dcaee-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="dcaee-213">Baixo</span><span class="sxs-lookup"><span data-stu-id="dcaee-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="dcaee-214">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="dcaee-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="dcaee-215">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="dcaee-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="dcaee-216">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-216">**Old behavior**</span></span>

<span data-ttu-id="dcaee-217">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="dcaee-218">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="dcaee-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="dcaee-219">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-219">**New behavior**</span></span>

<span data-ttu-id="dcaee-220">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="dcaee-221">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="dcaee-222">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-222">**Why**</span></span>

<span data-ttu-id="dcaee-223">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="dcaee-224">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="dcaee-225">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="dcaee-226">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="dcaee-227">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="dcaee-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="dcaee-228">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="dcaee-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="dcaee-229">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-229">**Mitigations**</span></span>

<span data-ttu-id="dcaee-230">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="dcaee-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="dcaee-231">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="dcaee-232">Problema de acompanhamento nº 15498</span><span class="sxs-lookup"><span data-stu-id="dcaee-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="dcaee-233">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-233">**Old behavior**</span></span>

<span data-ttu-id="dcaee-234">Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="dcaee-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="dcaee-235">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-235">**New behavior**</span></span>

<span data-ttu-id="dcaee-236">No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="dcaee-237">Isso não inclui o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="dcaee-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="dcaee-238">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-238">**Why**</span></span>

<span data-ttu-id="dcaee-239">Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.</span><span class="sxs-lookup"><span data-stu-id="dcaee-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="dcaee-240">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-240">**Mitigations**</span></span>

<span data-ttu-id="dcaee-241">Considere a possibilidade de migrar para uma plataforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="dcaee-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="dcaee-242">Se isso não for possível, continue usando o EF Core 2.1 ou o EF Core 2.2, que dão suporte ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="dcaee-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="dcaee-243">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcaee-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="dcaee-244">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="dcaee-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="dcaee-245">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-245">**Old behavior**</span></span>

<span data-ttu-id="dcaee-246">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dcaee-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="dcaee-247">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-247">**New behavior**</span></span>

<span data-ttu-id="dcaee-248">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="dcaee-249">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-249">**Why**</span></span>

<span data-ttu-id="dcaee-250">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="dcaee-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="dcaee-251">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="dcaee-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="dcaee-252">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="dcaee-253">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="dcaee-254">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-254">**Mitigations**</span></span>

<span data-ttu-id="dcaee-255">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="dcaee-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="dcaee-256">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="dcaee-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="dcaee-257">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="dcaee-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="dcaee-258">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-258">**Old behavior**</span></span>

<span data-ttu-id="dcaee-259">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="dcaee-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="dcaee-260">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-260">**New behavior**</span></span>

<span data-ttu-id="dcaee-261">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="dcaee-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="dcaee-262">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-262">**Why**</span></span>

<span data-ttu-id="dcaee-263">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="dcaee-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="dcaee-264">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-264">**Mitigations**</span></span>

<span data-ttu-id="dcaee-265">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="dcaee-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="dcaee-266">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="dcaee-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="dcaee-267">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="dcaee-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="dcaee-268">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="dcaee-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="dcaee-269">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-269">**Old behavior**</span></span>

<span data-ttu-id="dcaee-270">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="dcaee-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="dcaee-271">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-271">**New behavior**</span></span>

<span data-ttu-id="dcaee-272">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="dcaee-273">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="dcaee-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="dcaee-275">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="dcaee-276">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="dcaee-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="dcaee-277">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-277">**Why**</span></span>

<span data-ttu-id="dcaee-278">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="dcaee-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="dcaee-279">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="dcaee-280">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-280">**Mitigations**</span></span>

<span data-ttu-id="dcaee-281">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="dcaee-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="dcaee-282">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="dcaee-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="dcaee-283">Acompanhamento do problema #15392</span><span class="sxs-lookup"><span data-stu-id="dcaee-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="dcaee-284">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-284">**Old behavior**</span></span>

<span data-ttu-id="dcaee-285">Antes EF Core 3,0, o método das tentou detectar se o SQL passado pode ser composto.</span><span class="sxs-lookup"><span data-stu-id="dcaee-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="dcaee-286">Ele fez a avaliação do cliente quando o SQL era não combinável como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="dcaee-287">A consulta a seguir funcionou executando o procedimento armazenado no servidor e fazendo FirstOrDefault no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="dcaee-288">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-288">**New behavior**</span></span>

<span data-ttu-id="dcaee-289">A partir do EF Core 3,0, EF Core não tentará analisar o SQL.</span><span class="sxs-lookup"><span data-stu-id="dcaee-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="dcaee-290">Portanto, se você estiver compondo após FromSqlRaw/FromSqlInterpolated, EF Core comporá o SQL fazendo a subconsulta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="dcaee-291">Portanto, se você estiver usando um procedimento armazenado com composição, receberá uma exceção de sintaxe SQL inválida.</span><span class="sxs-lookup"><span data-stu-id="dcaee-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="dcaee-292">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-292">**Why**</span></span>

<span data-ttu-id="dcaee-293">O EF Core 3,0 não dá suporte à avaliação automática do cliente, pois foi propenso a erros, conforme explicado [aqui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="dcaee-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="dcaee-294">**Atenuação**</span><span class="sxs-lookup"><span data-stu-id="dcaee-294">**Mitigation**</span></span>

<span data-ttu-id="dcaee-295">Se você estiver usando um procedimento armazenado em FromSqlRaw/FromSqlInterpolated, saberá que ele não pode ser composto, para que você possa adicionar __AsEnumerable/AsAsyncEnumerable__ logo após a chamada do método das para evitar qualquer composição no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="dcaee-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="dcaee-296">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="dcaee-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="dcaee-297">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="dcaee-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="dcaee-298">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-298">**Old behavior**</span></span>

<span data-ttu-id="dcaee-299">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="dcaee-300">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-300">**New behavior**</span></span>

<span data-ttu-id="dcaee-301">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="dcaee-302">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="dcaee-303">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-303">**Why**</span></span>

<span data-ttu-id="dcaee-304">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="dcaee-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="dcaee-305">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-305">**Mitigations**</span></span>

<span data-ttu-id="dcaee-306">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="dcaee-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="dcaee-307">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="dcaee-308">Problema de acompanhamento nº 13518</span><span class="sxs-lookup"><span data-stu-id="dcaee-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="dcaee-309">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-309">**Old behavior**</span></span>

<span data-ttu-id="dcaee-310">Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="dcaee-311">Isso corresponde ao comportamento das consultas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="dcaee-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="dcaee-312">Por exemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="dcaee-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="dcaee-313">retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="dcaee-314">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-314">**New behavior**</span></span>

<span data-ttu-id="dcaee-315">No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="dcaee-316">Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.</span><span class="sxs-lookup"><span data-stu-id="dcaee-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="dcaee-317">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-317">**Why**</span></span>

<span data-ttu-id="dcaee-318">A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória.</span><span class="sxs-lookup"><span data-stu-id="dcaee-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="dcaee-319">Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="dcaee-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="dcaee-320">Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="dcaee-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="dcaee-321">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-321">**Mitigations**</span></span>

<span data-ttu-id="dcaee-322">Use uma consulta de acompanhamento se a resolução de identidade for necessária.</span><span class="sxs-lookup"><span data-stu-id="dcaee-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="dcaee-323">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="dcaee-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="dcaee-324">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="dcaee-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="dcaee-325">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="dcaee-326">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="dcaee-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="dcaee-327">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="dcaee-328">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="dcaee-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="dcaee-329">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-329">**Old behavior**</span></span>

<span data-ttu-id="dcaee-330">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="dcaee-331">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="dcaee-332">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-332">**New behavior**</span></span>

<span data-ttu-id="dcaee-333">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="dcaee-334">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-334">**Why**</span></span>

<span data-ttu-id="dcaee-335">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="dcaee-336">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-336">**Mitigations**</span></span>

<span data-ttu-id="dcaee-337">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="dcaee-338">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="dcaee-338">This can be avoided by:</span></span>
* <span data-ttu-id="dcaee-339">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="dcaee-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="dcaee-340">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="dcaee-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="dcaee-341">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="dcaee-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="dcaee-342">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="dcaee-343">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="dcaee-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="dcaee-344">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="dcaee-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="dcaee-345">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-345">**Old behavior**</span></span>

<span data-ttu-id="dcaee-346">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="dcaee-347">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-347">**New behavior**</span></span>

<span data-ttu-id="dcaee-348">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="dcaee-349">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="dcaee-350">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="dcaee-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="dcaee-351">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-351">**Why**</span></span>

<span data-ttu-id="dcaee-352">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="dcaee-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="dcaee-353">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-353">**Mitigations**</span></span>

<span data-ttu-id="dcaee-354">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="dcaee-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="dcaee-355">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="dcaee-356">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="dcaee-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="dcaee-357">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="dcaee-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="dcaee-358">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="dcaee-359">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="dcaee-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="dcaee-360">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-360">**Old behavior**</span></span>

<span data-ttu-id="dcaee-361">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="dcaee-362">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-362">**New behavior**</span></span>

<span data-ttu-id="dcaee-363">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="dcaee-364">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="dcaee-365">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-365">**Why**</span></span>

<span data-ttu-id="dcaee-366">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="dcaee-367">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-367">**Mitigations**</span></span>

<span data-ttu-id="dcaee-368">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="dcaee-369">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="dcaee-370">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="dcaee-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="dcaee-371">Acompanhamento do problema #18022</span><span class="sxs-lookup"><span data-stu-id="dcaee-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="dcaee-372">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-372">**Old behavior**</span></span>

<span data-ttu-id="dcaee-373">Antes de 3,0, carregar cuidadosamente as navegações de coleção por meio de operadores `Include` fizeram com que várias consultas sejam geradas no banco de dados relacional, uma para cada tipo de entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="dcaee-374">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-374">**New behavior**</span></span>

<span data-ttu-id="dcaee-375">A partir do 3,0, o EF Core gera uma única consulta com junções em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="dcaee-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="dcaee-376">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-376">**Why**</span></span>

<span data-ttu-id="dcaee-377">A emissão de várias consultas para implementar uma única consulta LINQ causou vários problemas, incluindo o desempenho negativo à medida que várias viagens de ida e volta do banco de dados eram necessárias e os problemas de coerência do dado, uma vez que cada consulta poderia observar um estado diferente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="dcaee-378">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-378">**Mitigations**</span></span>

<span data-ttu-id="dcaee-379">Embora tecnicamente essa não seja uma alteração significativa, ela pode ter um efeito considerável no desempenho do aplicativo quando uma única consulta contém um grande número de operador `Include` em navegações de coleção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="dcaee-380">[Consulte este comentário](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) para obter mais informações e reescrever consultas de maneira mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="dcaee-381">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="dcaee-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="dcaee-382">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="dcaee-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="dcaee-383">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-383">**Old behavior**</span></span>

<span data-ttu-id="dcaee-384">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="dcaee-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="dcaee-385">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-385">**New behavior**</span></span>

<span data-ttu-id="dcaee-386">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="dcaee-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="dcaee-387">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-387">**Why**</span></span>

<span data-ttu-id="dcaee-388">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="dcaee-389">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-389">**Mitigations**</span></span>

<span data-ttu-id="dcaee-390">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="dcaee-391">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="dcaee-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="dcaee-392">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="dcaee-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="dcaee-393">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-393">**Old behavior**</span></span>

<span data-ttu-id="dcaee-394">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/keyless-entity-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="dcaee-395">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="dcaee-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="dcaee-396">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-396">**New behavior**</span></span>

<span data-ttu-id="dcaee-397">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="dcaee-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="dcaee-398">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="dcaee-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="dcaee-399">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-399">**Why**</span></span>

<span data-ttu-id="dcaee-400">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="dcaee-401">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="dcaee-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="dcaee-402">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="dcaee-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="dcaee-403">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-403">**Mitigations**</span></span>

<span data-ttu-id="dcaee-404">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="dcaee-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="dcaee-405">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="dcaee-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="dcaee-406">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="dcaee-407">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="dcaee-408">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="dcaee-409">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="dcaee-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="dcaee-410">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="dcaee-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="dcaee-411">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-411">**Old behavior**</span></span>

<span data-ttu-id="dcaee-412">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="dcaee-413">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-413">**New behavior**</span></span>

<span data-ttu-id="dcaee-414">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="dcaee-415">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="dcaee-416">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="dcaee-417">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="dcaee-418">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-418">For example:</span></span>

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

<span data-ttu-id="dcaee-419">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="dcaee-420">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-420">**Why**</span></span>

<span data-ttu-id="dcaee-421">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="dcaee-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="dcaee-422">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="dcaee-423">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-423">**Mitigations**</span></span>

<span data-ttu-id="dcaee-424">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="dcaee-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="dcaee-425">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="dcaee-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="dcaee-426">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="dcaee-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="dcaee-427">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-427">**Old behavior**</span></span>

<span data-ttu-id="dcaee-428">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="dcaee-428">Consider the following model:</span></span>
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
<span data-ttu-id="dcaee-429">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="dcaee-430">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-430">**New behavior**</span></span>

<span data-ttu-id="dcaee-431">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="dcaee-432">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="dcaee-433">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-433">**Mitigations**</span></span>

<span data-ttu-id="dcaee-434">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="dcaee-435">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="dcaee-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="dcaee-436">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="dcaee-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="dcaee-437">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="dcaee-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="dcaee-438">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-438">**Old behavior**</span></span>

<span data-ttu-id="dcaee-439">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="dcaee-439">Consider the following model:</span></span>
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
<span data-ttu-id="dcaee-440">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="dcaee-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="dcaee-441">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-441">**New behavior**</span></span>

<span data-ttu-id="dcaee-442">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="dcaee-443">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="dcaee-444">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-444">**Why**</span></span>

<span data-ttu-id="dcaee-445">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="dcaee-446">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-446">**Mitigations**</span></span>

<span data-ttu-id="dcaee-447">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="dcaee-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="dcaee-448">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="dcaee-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="dcaee-449">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="dcaee-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="dcaee-450">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="dcaee-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="dcaee-451">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-451">**Old behavior**</span></span>

<span data-ttu-id="dcaee-452">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="dcaee-452">Consider the following model:</span></span>
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

<span data-ttu-id="dcaee-453">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="dcaee-454">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-454">**New behavior**</span></span>

<span data-ttu-id="dcaee-455">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="dcaee-456">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-456">**Why**</span></span>

<span data-ttu-id="dcaee-457">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="dcaee-458">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-458">**Mitigations**</span></span>

<span data-ttu-id="dcaee-459">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="dcaee-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="dcaee-460">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="dcaee-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="dcaee-461">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="dcaee-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="dcaee-462">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-462">**Old behavior**</span></span>

<span data-ttu-id="dcaee-463">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="dcaee-463">Consider the following model:</span></span>
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
<span data-ttu-id="dcaee-464">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="dcaee-465">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="dcaee-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="dcaee-466">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-466">**New behavior**</span></span>

<span data-ttu-id="dcaee-467">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="dcaee-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="dcaee-468">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="dcaee-469">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-469">For example:</span></span>

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

<span data-ttu-id="dcaee-470">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-470">**Why**</span></span>

<span data-ttu-id="dcaee-471">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="dcaee-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="dcaee-472">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-472">**Mitigations**</span></span>

<span data-ttu-id="dcaee-473">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="dcaee-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="dcaee-474">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="dcaee-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="dcaee-475">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="dcaee-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="dcaee-476">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-476">**Old behavior**</span></span>

<span data-ttu-id="dcaee-477">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="dcaee-478">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-478">**New behavior**</span></span>

<span data-ttu-id="dcaee-479">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="dcaee-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="dcaee-480">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-480">**Why**</span></span>

<span data-ttu-id="dcaee-481">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="dcaee-482">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="dcaee-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="dcaee-483">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-483">**Mitigations**</span></span>

<span data-ttu-id="dcaee-484">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="dcaee-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="dcaee-485">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="dcaee-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="dcaee-486">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="dcaee-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="dcaee-487">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-487">**Old behavior**</span></span>

<span data-ttu-id="dcaee-488">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="dcaee-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="dcaee-489">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-489">**New behavior**</span></span>

<span data-ttu-id="dcaee-490">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="dcaee-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="dcaee-491">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="dcaee-492">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-492">**Why**</span></span>

<span data-ttu-id="dcaee-493">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="dcaee-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="dcaee-494">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-494">**Mitigations**</span></span>

<span data-ttu-id="dcaee-495">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="dcaee-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="dcaee-496">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="dcaee-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="dcaee-497">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-497">Backing fields are used by default</span></span>

[<span data-ttu-id="dcaee-498">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="dcaee-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="dcaee-499">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-499">**Old behavior**</span></span>

<span data-ttu-id="dcaee-500">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="dcaee-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="dcaee-501">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="dcaee-502">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-502">**New behavior**</span></span>

<span data-ttu-id="dcaee-503">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="dcaee-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="dcaee-504">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="dcaee-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="dcaee-505">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-505">**Why**</span></span>

<span data-ttu-id="dcaee-506">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="dcaee-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="dcaee-507">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-507">**Mitigations**</span></span>

<span data-ttu-id="dcaee-508">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="dcaee-509">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="dcaee-510">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="dcaee-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="dcaee-511">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="dcaee-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="dcaee-512">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-512">**Old behavior**</span></span>

<span data-ttu-id="dcaee-513">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="dcaee-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="dcaee-514">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="dcaee-515">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-515">**New behavior**</span></span>

<span data-ttu-id="dcaee-516">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="dcaee-517">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-517">**Why**</span></span>

<span data-ttu-id="dcaee-518">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="dcaee-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="dcaee-519">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-519">**Mitigations**</span></span>

<span data-ttu-id="dcaee-520">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="dcaee-521">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="dcaee-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="dcaee-522">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="dcaee-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="dcaee-523">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-523">**Old behavior**</span></span>

<span data-ttu-id="dcaee-524">Antes de EF Core 3,0, uma propriedade poderia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome tiver sido encontrada no tipo .NET, EF Core tentará fazer a correspondência com um campo usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="dcaee-525">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-525">**New behavior**</span></span>

<span data-ttu-id="dcaee-526">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="dcaee-527">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-527">**Why**</span></span>

<span data-ttu-id="dcaee-528">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="dcaee-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="dcaee-529">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-529">**Mitigations**</span></span>

<span data-ttu-id="dcaee-530">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="dcaee-531">Em uma versão futura do EF Core após 3,0, planejamos reabilitar explicitamente um nome de campo diferente do nome da propriedade (consulte o problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="dcaee-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="dcaee-532">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="dcaee-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="dcaee-533">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="dcaee-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="dcaee-534">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-534">**Old behavior**</span></span>

<span data-ttu-id="dcaee-535">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="dcaee-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="dcaee-536">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-536">**New behavior**</span></span>

<span data-ttu-id="dcaee-537">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="dcaee-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="dcaee-538">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-538">**Why**</span></span>

<span data-ttu-id="dcaee-539">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="dcaee-540">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="dcaee-541">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-541">**Mitigations**</span></span>

<span data-ttu-id="dcaee-542">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="dcaee-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="dcaee-543">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="dcaee-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="dcaee-544">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="dcaee-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="dcaee-545">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-545">**Old behavior**</span></span>

<span data-ttu-id="dcaee-546">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="dcaee-547">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="dcaee-548">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-548">**New behavior**</span></span>

<span data-ttu-id="dcaee-549">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="dcaee-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="dcaee-550">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="dcaee-551">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="dcaee-552">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="dcaee-553">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-553">**Why**</span></span>

<span data-ttu-id="dcaee-554">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="dcaee-555">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-555">**Mitigations**</span></span>

<span data-ttu-id="dcaee-556">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="dcaee-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="dcaee-557">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="dcaee-558">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="dcaee-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="dcaee-559">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-559">**Old behavior**</span></span>

<span data-ttu-id="dcaee-560">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="dcaee-561">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="dcaee-562">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-562">**New behavior**</span></span>

<span data-ttu-id="dcaee-563">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="dcaee-564">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-564">**Why**</span></span>

<span data-ttu-id="dcaee-565">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="dcaee-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="dcaee-566">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-566">**Mitigations**</span></span>

<span data-ttu-id="dcaee-567">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="dcaee-568">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="dcaee-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="dcaee-569">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="dcaee-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="dcaee-570">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="dcaee-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="dcaee-571">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="dcaee-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="dcaee-572">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-572">**Old behavior**</span></span>

<span data-ttu-id="dcaee-573">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="dcaee-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="dcaee-574">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-574">**New behavior**</span></span>

<span data-ttu-id="dcaee-575">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="dcaee-576">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-576">**Why**</span></span>

<span data-ttu-id="dcaee-577">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="dcaee-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="dcaee-578">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-578">**Mitigations**</span></span>

<span data-ttu-id="dcaee-579">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="dcaee-580">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="dcaee-580">This isn't common.</span></span>
<span data-ttu-id="dcaee-581">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="dcaee-582">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="dcaee-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="dcaee-583">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="dcaee-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="dcaee-584">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="dcaee-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="dcaee-585">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-585">**Old behavior**</span></span>

<span data-ttu-id="dcaee-586">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="dcaee-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="dcaee-587">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="dcaee-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="dcaee-588">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="dcaee-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="dcaee-589">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-589">**New behavior**</span></span>

<span data-ttu-id="dcaee-590">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="dcaee-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="dcaee-591">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="dcaee-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="dcaee-592">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="dcaee-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="dcaee-593">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="dcaee-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="dcaee-594">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-594">**Why**</span></span>

<span data-ttu-id="dcaee-595">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="dcaee-596">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-596">**Mitigations**</span></span>

<span data-ttu-id="dcaee-597">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="dcaee-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="dcaee-598">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="dcaee-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="dcaee-599">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="dcaee-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="dcaee-600">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-600">**Old behavior**</span></span>

<span data-ttu-id="dcaee-601">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="dcaee-602">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-602">**New behavior**</span></span>

<span data-ttu-id="dcaee-603">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="dcaee-604">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-604">**Why**</span></span>

<span data-ttu-id="dcaee-605">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="dcaee-606">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-606">**Mitigations**</span></span>

<span data-ttu-id="dcaee-607">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="dcaee-608">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="dcaee-609">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="dcaee-610">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="dcaee-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="dcaee-611">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="dcaee-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="dcaee-612">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-612">**Old behavior**</span></span>

<span data-ttu-id="dcaee-613">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="dcaee-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="dcaee-614">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="dcaee-615">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="dcaee-616">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="dcaee-617">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-617">**New behavior**</span></span>

<span data-ttu-id="dcaee-618">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="dcaee-619">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-619">**Why**</span></span>

<span data-ttu-id="dcaee-620">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="dcaee-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="dcaee-621">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-621">**Mitigations**</span></span>

<span data-ttu-id="dcaee-622">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="dcaee-623">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="dcaee-623">This is not common.</span></span>
<span data-ttu-id="dcaee-624">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="dcaee-625">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="dcaee-626">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="dcaee-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="dcaee-627">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="dcaee-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="dcaee-628">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-628">**Old behavior**</span></span>

<span data-ttu-id="dcaee-629">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="dcaee-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="dcaee-630">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="dcaee-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="dcaee-631">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-631">**New behavior**</span></span>

<span data-ttu-id="dcaee-632">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="dcaee-633">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-633">**Why**</span></span>

<span data-ttu-id="dcaee-634">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="dcaee-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="dcaee-635">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-635">**Mitigations**</span></span>

<span data-ttu-id="dcaee-636">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="dcaee-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="dcaee-637">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="dcaee-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="dcaee-638">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="dcaee-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="dcaee-639">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="dcaee-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="dcaee-640">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="dcaee-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="dcaee-641">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-641">**Old behavior**</span></span>

<span data-ttu-id="dcaee-642">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="dcaee-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="dcaee-643">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-643">**New behavior**</span></span>

<span data-ttu-id="dcaee-644">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="dcaee-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="dcaee-645">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-645">**Why**</span></span>

<span data-ttu-id="dcaee-646">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="dcaee-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="dcaee-647">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-647">**Mitigations**</span></span>

<span data-ttu-id="dcaee-648">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="dcaee-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="dcaee-649">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="dcaee-650">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="dcaee-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="dcaee-651">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="dcaee-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="dcaee-652">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-652">**Old behavior**</span></span>

<span data-ttu-id="dcaee-653">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="dcaee-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="dcaee-654">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-654">**New behavior**</span></span>

<span data-ttu-id="dcaee-655">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="dcaee-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="dcaee-656">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-656">**Why**</span></span>

<span data-ttu-id="dcaee-657">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="dcaee-658">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="dcaee-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="dcaee-659">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-659">**Mitigations**</span></span>

<span data-ttu-id="dcaee-660">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="dcaee-661">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="dcaee-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="dcaee-662">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="dcaee-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="dcaee-663">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-663">**Old behavior**</span></span>

<span data-ttu-id="dcaee-664">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="dcaee-665">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-665">**New behavior**</span></span>

<span data-ttu-id="dcaee-666">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="dcaee-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="dcaee-667">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="dcaee-668">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-668">**Why**</span></span>

<span data-ttu-id="dcaee-669">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="dcaee-670">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-670">**Mitigations**</span></span>

<span data-ttu-id="dcaee-671">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="dcaee-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="dcaee-672">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="dcaee-672">Metadata API changes</span></span>

[<span data-ttu-id="dcaee-673">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="dcaee-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="dcaee-674">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-674">**New behavior**</span></span>

<span data-ttu-id="dcaee-675">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="dcaee-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="dcaee-676">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-676">**Why**</span></span>

<span data-ttu-id="dcaee-677">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="dcaee-678">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-678">**Mitigations**</span></span>

<span data-ttu-id="dcaee-679">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="dcaee-680">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="dcaee-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="dcaee-681">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="dcaee-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="dcaee-682">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-682">**New behavior**</span></span>

<span data-ttu-id="dcaee-683">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="dcaee-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="dcaee-684">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-684">**Why**</span></span>

<span data-ttu-id="dcaee-685">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="dcaee-686">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-686">**Mitigations**</span></span>

<span data-ttu-id="dcaee-687">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="dcaee-688">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="dcaee-689">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="dcaee-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="dcaee-690">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-690">**Old behavior**</span></span>

<span data-ttu-id="dcaee-691">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="dcaee-692">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-692">**New behavior**</span></span>

<span data-ttu-id="dcaee-693">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="dcaee-694">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-694">**Why**</span></span>

<span data-ttu-id="dcaee-695">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="dcaee-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="dcaee-696">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-696">**Mitigations**</span></span>

<span data-ttu-id="dcaee-697">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="dcaee-698">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="dcaee-699">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="dcaee-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="dcaee-700">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-700">**Old behavior**</span></span>

<span data-ttu-id="dcaee-701">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="dcaee-702">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-702">**New behavior**</span></span>

<span data-ttu-id="dcaee-703">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="dcaee-704">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-704">**Why**</span></span>

<span data-ttu-id="dcaee-705">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="dcaee-706">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-706">**Mitigations**</span></span>

<span data-ttu-id="dcaee-707">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="dcaee-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="dcaee-708">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="dcaee-709">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="dcaee-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="dcaee-710">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-710">**Old behavior**</span></span>

<span data-ttu-id="dcaee-711">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="dcaee-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="dcaee-712">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-712">**New behavior**</span></span>

<span data-ttu-id="dcaee-713">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="dcaee-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="dcaee-714">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-714">**Why**</span></span>

<span data-ttu-id="dcaee-715">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="dcaee-716">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="dcaee-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="dcaee-717">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-717">**Mitigations**</span></span>

<span data-ttu-id="dcaee-718">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="dcaee-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="dcaee-719">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="dcaee-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="dcaee-720">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="dcaee-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="dcaee-721">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="dcaee-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="dcaee-722">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="dcaee-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="dcaee-723">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-723">**Old behavior**</span></span>

<span data-ttu-id="dcaee-724">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="dcaee-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="dcaee-725">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="dcaee-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="dcaee-726">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-726">**New behavior**</span></span>

<span data-ttu-id="dcaee-727">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="dcaee-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="dcaee-728">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-728">**Why**</span></span>

<span data-ttu-id="dcaee-729">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="dcaee-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="dcaee-730">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-730">**Mitigations**</span></span>

<span data-ttu-id="dcaee-731">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="dcaee-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="dcaee-732">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="dcaee-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="dcaee-733">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="dcaee-734">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="dcaee-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="dcaee-735">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="dcaee-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="dcaee-736">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-736">**Old behavior**</span></span>

<span data-ttu-id="dcaee-737">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="dcaee-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="dcaee-738">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-738">**New behavior**</span></span>

<span data-ttu-id="dcaee-739">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="dcaee-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="dcaee-740">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-740">**Why**</span></span>

<span data-ttu-id="dcaee-741">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="dcaee-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="dcaee-742">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="dcaee-743">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-743">**Mitigations**</span></span>

<span data-ttu-id="dcaee-744">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="dcaee-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="dcaee-745">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="dcaee-746">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="dcaee-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="dcaee-747">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="dcaee-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="dcaee-748">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="dcaee-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="dcaee-749">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="dcaee-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="dcaee-750">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-750">**Old behavior**</span></span>

<span data-ttu-id="dcaee-751">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="dcaee-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="dcaee-752">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-752">**New behavior**</span></span>

<span data-ttu-id="dcaee-753">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dcaee-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="dcaee-754">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-754">**Why**</span></span>

<span data-ttu-id="dcaee-755">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="dcaee-756">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-756">**Mitigations**</span></span>

<span data-ttu-id="dcaee-757">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="dcaee-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="dcaee-758">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="dcaee-759">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="dcaee-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="dcaee-760">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="dcaee-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="dcaee-761">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="dcaee-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="dcaee-762">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-762">**Old behavior**</span></span>

<span data-ttu-id="dcaee-763">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="dcaee-764">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-764">**New behavior**</span></span>

<span data-ttu-id="dcaee-765">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="dcaee-766">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-766">**Why**</span></span>

<span data-ttu-id="dcaee-767">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="dcaee-768">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="dcaee-769">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-769">**Mitigations**</span></span>

<span data-ttu-id="dcaee-770">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="dcaee-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="dcaee-771">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="dcaee-772">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="dcaee-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="dcaee-773">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="dcaee-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="dcaee-774">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="dcaee-774">**Change**</span></span>

<span data-ttu-id="dcaee-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="dcaee-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="dcaee-776">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-776">**Why**</span></span>

<span data-ttu-id="dcaee-777">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="dcaee-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="dcaee-778">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-778">**Mitigations**</span></span>

<span data-ttu-id="dcaee-779">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="dcaee-779">Use the new name.</span></span> <span data-ttu-id="dcaee-780">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="dcaee-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="dcaee-781">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="dcaee-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="dcaee-782">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="dcaee-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="dcaee-783">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-783">**Old behavior**</span></span>

<span data-ttu-id="dcaee-784">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="dcaee-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="dcaee-785">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="dcaee-786">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-786">**New behavior**</span></span>

<span data-ttu-id="dcaee-787">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="dcaee-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="dcaee-788">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="dcaee-789">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-789">**Why**</span></span>

<span data-ttu-id="dcaee-790">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="dcaee-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="dcaee-791">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-791">**Mitigations**</span></span>

<span data-ttu-id="dcaee-792">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="dcaee-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="dcaee-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="dcaee-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="dcaee-794">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="dcaee-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="dcaee-795">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-795">**Old behavior**</span></span>

<span data-ttu-id="dcaee-796">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="dcaee-797">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-797">**New behavior**</span></span>

<span data-ttu-id="dcaee-798">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="dcaee-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="dcaee-799">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-799">**Why**</span></span>

<span data-ttu-id="dcaee-800">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="dcaee-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="dcaee-801">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="dcaee-802">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-802">**Mitigations**</span></span>

<span data-ttu-id="dcaee-803">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="dcaee-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="dcaee-804">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="dcaee-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="dcaee-805">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="dcaee-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="dcaee-806">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-806">**Old behavior**</span></span>

<span data-ttu-id="dcaee-807">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="dcaee-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="dcaee-808">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-808">**New behavior**</span></span>

<span data-ttu-id="dcaee-809">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="dcaee-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="dcaee-810">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="dcaee-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="dcaee-811">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-811">**Why**</span></span>

<span data-ttu-id="dcaee-812">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="dcaee-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="dcaee-813">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="dcaee-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="dcaee-814">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="dcaee-815">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-815">**Mitigations**</span></span>

<span data-ttu-id="dcaee-816">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="dcaee-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="dcaee-817">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="dcaee-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="dcaee-818">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="dcaee-819">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="dcaee-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="dcaee-820">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-820">**Old behavior**</span></span>

<span data-ttu-id="dcaee-821">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="dcaee-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="dcaee-822">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-822">**New behavior**</span></span>

<span data-ttu-id="dcaee-823">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="dcaee-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="dcaee-824">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-824">**Why**</span></span>

<span data-ttu-id="dcaee-825">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="dcaee-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="dcaee-826">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="dcaee-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="dcaee-827">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-827">**Mitigations**</span></span>

<span data-ttu-id="dcaee-828">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="dcaee-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="dcaee-829">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="dcaee-830">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="dcaee-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="dcaee-831">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="dcaee-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="dcaee-832">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-832">**Old behavior**</span></span>

<span data-ttu-id="dcaee-833">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="dcaee-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="dcaee-834">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-834">**New behavior**</span></span>

<span data-ttu-id="dcaee-835">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="dcaee-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="dcaee-836">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-836">**Why**</span></span>

<span data-ttu-id="dcaee-837">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="dcaee-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="dcaee-838">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-838">**Mitigations**</span></span>

<span data-ttu-id="dcaee-839">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="dcaee-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="dcaee-840">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="dcaee-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="dcaee-841">Microsoft. Data. SqlClient é usado em vez de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="dcaee-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="dcaee-842">Acompanhamento do problema #15636</span><span class="sxs-lookup"><span data-stu-id="dcaee-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="dcaee-843">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-843">**Old behavior**</span></span>

<span data-ttu-id="dcaee-844">Microsoft. EntityFrameworkCore. SqlServer anteriormente dependia de System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="dcaee-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="dcaee-845">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-845">**New behavior**</span></span>

<span data-ttu-id="dcaee-846">Atualizamos nosso pacote para depender do Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="dcaee-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="dcaee-847">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-847">**Why**</span></span>

<span data-ttu-id="dcaee-848">Microsoft. Data. SqlClient é o principal driver de acesso a dados para SQL Server em andamento, e System. Data. SqlClient não é mais o foco do desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="dcaee-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="dcaee-849">Alguns recursos importantes, como o Always Encrypted, estão disponíveis apenas em Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="dcaee-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="dcaee-850">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-850">**Mitigations**</span></span>

<span data-ttu-id="dcaee-851">Se o seu código usar uma dependência direta em System. Data. SqlClient, você deverá alterá-lo para fazer referência a Microsoft. Data. SqlClient, em vez disso; como os dois pacotes mantêm um grau muito alto de compatibilidade de API, isso deve ser apenas um pacote simples e uma alteração de namespace.</span><span class="sxs-lookup"><span data-stu-id="dcaee-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="dcaee-852">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="dcaee-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="dcaee-853">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="dcaee-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="dcaee-854">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-854">**Old behavior**</span></span>

<span data-ttu-id="dcaee-855">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="dcaee-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="dcaee-856">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-856">For example:</span></span>

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

<span data-ttu-id="dcaee-857">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-857">**New behavior**</span></span>

<span data-ttu-id="dcaee-858">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="dcaee-859">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-859">**Why**</span></span>

<span data-ttu-id="dcaee-860">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="dcaee-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="dcaee-861">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-861">**Mitigations**</span></span>

<span data-ttu-id="dcaee-862">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="dcaee-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="dcaee-863">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="dcaee-863">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="dcaee-864">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="dcaee-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="dcaee-865">Acompanhamento do problema #12757</span><span class="sxs-lookup"><span data-stu-id="dcaee-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="dcaee-866">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-866">**Old behavior**</span></span>

<span data-ttu-id="dcaee-867">Um DbFunction configurado com o esquema como uma cadeia de caracteres vazia foi tratado como função interna sem um esquema.</span><span class="sxs-lookup"><span data-stu-id="dcaee-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="dcaee-868">Por exemplo, o código a seguir mapeará a função `DatePart` CLR para `DATEPART` função interna no SqlServer.</span><span class="sxs-lookup"><span data-stu-id="dcaee-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="dcaee-869">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="dcaee-869">**New behavior**</span></span>

<span data-ttu-id="dcaee-870">Todos os mapeamentos DbFunction são considerados mapeados para funções definidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="dcaee-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="dcaee-871">Portanto, o valor da cadeia de caracteres vazia colocaria a função dentro do esquema padrão para o modelo.</span><span class="sxs-lookup"><span data-stu-id="dcaee-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="dcaee-872">Que poderia ser o esquema configurado explicitamente por meio da API fluente `modelBuilder.HasDefaultSchema()` ou `dbo`, caso contrário.</span><span class="sxs-lookup"><span data-stu-id="dcaee-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="dcaee-873">**Por que**</span><span class="sxs-lookup"><span data-stu-id="dcaee-873">**Why**</span></span>

<span data-ttu-id="dcaee-874">O esquema anterior sendo vazio era uma maneira de tratar que a função é interna, mas essa lógica só é aplicável ao SqlServer, em que funções internas não pertencem a nenhum esquema.</span><span class="sxs-lookup"><span data-stu-id="dcaee-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="dcaee-875">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="dcaee-875">**Mitigations**</span></span>

<span data-ttu-id="dcaee-876">Configure a tradução do DbFunction manualmente para mapeá-lo para uma função interna.</span><span class="sxs-lookup"><span data-stu-id="dcaee-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
