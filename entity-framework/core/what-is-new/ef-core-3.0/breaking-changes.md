---
title: Alterações significativas no EF Core 3.0 – EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417457"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="222d8-102">Alterações recentes incluídas no EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="222d8-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="222d8-103">As alterações de API e comportamento a seguir têm o potencial de interromper os aplicativos existentes ao atualizá-los para o 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="222d8-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="222d8-104">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="222d8-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="222d8-105">Resumo</span><span class="sxs-lookup"><span data-stu-id="222d8-105">Summary</span></span>

| <span data-ttu-id="222d8-106">**Alterações da falha**</span><span class="sxs-lookup"><span data-stu-id="222d8-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="222d8-107">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="222d8-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="222d8-108">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="222d8-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="222d8-109">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-109">High</span></span>       |
| [<span data-ttu-id="222d8-110">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="222d8-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="222d8-111">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-111">High</span></span>      |
| [<span data-ttu-id="222d8-112">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="222d8-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="222d8-113">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-113">High</span></span>      |
| [<span data-ttu-id="222d8-114">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="222d8-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="222d8-115">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-115">High</span></span>      |
| [<span data-ttu-id="222d8-116">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="222d8-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="222d8-117">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-117">High</span></span>      |
| [<span data-ttu-id="222d8-118">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="222d8-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="222d8-119">Alta</span><span class="sxs-lookup"><span data-stu-id="222d8-119">High</span></span>      |
| [<span data-ttu-id="222d8-120">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="222d8-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="222d8-121">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-121">Medium</span></span>      |
| [<span data-ttu-id="222d8-122">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="222d8-123">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-123">Medium</span></span>      |
| [<span data-ttu-id="222d8-124">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="222d8-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="222d8-125">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-125">Medium</span></span>      |
| [<span data-ttu-id="222d8-126">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="222d8-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="222d8-127">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-127">Medium</span></span>      |
| [<span data-ttu-id="222d8-128">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="222d8-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="222d8-129">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-129">Medium</span></span>      |
| [<span data-ttu-id="222d8-130">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="222d8-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="222d8-131">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-131">Medium</span></span>      |
| [<span data-ttu-id="222d8-132">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="222d8-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="222d8-133">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-133">Medium</span></span>      |
| [<span data-ttu-id="222d8-134">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="222d8-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="222d8-135">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-135">Medium</span></span>      |
| [<span data-ttu-id="222d8-136">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="222d8-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="222d8-137">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-137">Medium</span></span>      |
| [<span data-ttu-id="222d8-138">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="222d8-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="222d8-139">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-139">Medium</span></span>      |
| [<span data-ttu-id="222d8-140">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="222d8-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="222d8-141">Médio</span><span class="sxs-lookup"><span data-stu-id="222d8-141">Medium</span></span>      |
| [<span data-ttu-id="222d8-142">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="222d8-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="222d8-143">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-143">Low</span></span>      |
| [<span data-ttu-id="222d8-144">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="222d8-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="222d8-145">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-145">Low</span></span>      |
| [<span data-ttu-id="222d8-146">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="222d8-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="222d8-147">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-147">Low</span></span>      |
| [<span data-ttu-id="222d8-148">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="222d8-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="222d8-149">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-149">Low</span></span>      |
| [<span data-ttu-id="222d8-150">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="222d8-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="222d8-151">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-151">Low</span></span>      |
| [<span data-ttu-id="222d8-152">Entidades de propriedade não podem ser consultadas sem o proprietário usando uma consulta de rastreamento</span><span class="sxs-lookup"><span data-stu-id="222d8-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="222d8-153">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-153">Low</span></span>      |
| [<span data-ttu-id="222d8-154">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="222d8-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="222d8-155">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-155">Low</span></span>      |
| [<span data-ttu-id="222d8-156">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="222d8-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="222d8-157">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-157">Low</span></span>      |
| [<span data-ttu-id="222d8-158">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="222d8-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="222d8-159">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-159">Low</span></span>      |
| [<span data-ttu-id="222d8-160">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="222d8-161">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-161">Low</span></span>      |
| [<span data-ttu-id="222d8-162">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="222d8-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="222d8-163">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-163">Low</span></span>      |
| [<span data-ttu-id="222d8-164">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="222d8-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="222d8-165">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-165">Low</span></span>      |
| [<span data-ttu-id="222d8-166">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="222d8-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="222d8-167">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-167">Low</span></span>      |
| [<span data-ttu-id="222d8-168">AddEntityFramework \* adiciona IMemoryCache com um limite de tamanho</span><span class="sxs-lookup"><span data-stu-id="222d8-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="222d8-169">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-169">Low</span></span>      |
| [<span data-ttu-id="222d8-170">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="222d8-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="222d8-171">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-171">Low</span></span>      |
| [<span data-ttu-id="222d8-172">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="222d8-173">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-173">Low</span></span>      |
| [<span data-ttu-id="222d8-174">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="222d8-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="222d8-175">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-175">Low</span></span>      |
| [<span data-ttu-id="222d8-176">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="222d8-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="222d8-177">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-177">Low</span></span>      |
| [<span data-ttu-id="222d8-178">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="222d8-179">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-179">Low</span></span>      |
| [<span data-ttu-id="222d8-180">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="222d8-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="222d8-181">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-181">Low</span></span>      |
| [<span data-ttu-id="222d8-182">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="222d8-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="222d8-183">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-183">Low</span></span>      |
| [<span data-ttu-id="222d8-184">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="222d8-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="222d8-185">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-185">Low</span></span>      |
| [<span data-ttu-id="222d8-186">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="222d8-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="222d8-187">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-187">Low</span></span>      |
| [<span data-ttu-id="222d8-188">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="222d8-189">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-189">Low</span></span>      |
| [<span data-ttu-id="222d8-190">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="222d8-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="222d8-191">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-191">Low</span></span>      |
| [<span data-ttu-id="222d8-192">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="222d8-193">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-193">Low</span></span>      |
| [<span data-ttu-id="222d8-194">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="222d8-195">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-195">Low</span></span>      |
| [<span data-ttu-id="222d8-196">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="222d8-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="222d8-197">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-197">Low</span></span>      |
| [<span data-ttu-id="222d8-198">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="222d8-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="222d8-199">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-199">Low</span></span>      |
| [<span data-ttu-id="222d8-200">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="222d8-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="222d8-201">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-201">Low</span></span>      |
| [<span data-ttu-id="222d8-202">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="222d8-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="222d8-203">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-203">Low</span></span>      |
| [<span data-ttu-id="222d8-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="222d8-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="222d8-205">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-205">Low</span></span>      |
| [<span data-ttu-id="222d8-206">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="222d8-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="222d8-207">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-207">Low</span></span>      |
| [<span data-ttu-id="222d8-208">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="222d8-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="222d8-209">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-209">Low</span></span>      |
| [<span data-ttu-id="222d8-210">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="222d8-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="222d8-211">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-211">Low</span></span>      |
| [<span data-ttu-id="222d8-212">Microsoft. Data. SqlClient é usado em vez de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="222d8-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="222d8-213">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-213">Low</span></span>      |
| [<span data-ttu-id="222d8-214">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="222d8-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="222d8-215">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-215">Low</span></span>      |
| [<span data-ttu-id="222d8-216">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="222d8-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="222d8-217">Baixo</span><span class="sxs-lookup"><span data-stu-id="222d8-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="222d8-218">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="222d8-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="222d8-219">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="222d8-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="222d8-220">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-220">**Old behavior**</span></span>

<span data-ttu-id="222d8-221">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="222d8-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="222d8-222">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="222d8-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="222d8-223">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-223">**New behavior**</span></span>

<span data-ttu-id="222d8-224">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="222d8-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="222d8-225">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="222d8-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="222d8-226">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-226">**Why**</span></span>

<span data-ttu-id="222d8-227">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="222d8-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="222d8-228">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="222d8-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="222d8-229">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="222d8-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="222d8-230">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="222d8-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="222d8-231">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="222d8-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="222d8-232">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="222d8-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="222d8-233">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-233">**Mitigations**</span></span>

<span data-ttu-id="222d8-234">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="222d8-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="222d8-235">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="222d8-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="222d8-236">Problema de acompanhamento nº 15498</span><span class="sxs-lookup"><span data-stu-id="222d8-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> <span data-ttu-id="222d8-237">EF Core 3,1 destinos .NET Standard 2,0 novamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-237">EF Core 3.1 targets .NET Standard 2.0 again.</span></span> <span data-ttu-id="222d8-238">Isso traz suporte de volta para .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="222d8-238">This brings back support for .NET Framework.</span></span>

<span data-ttu-id="222d8-239">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-239">**Old behavior**</span></span>

<span data-ttu-id="222d8-240">Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="222d8-240">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="222d8-241">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-241">**New behavior**</span></span>

<span data-ttu-id="222d8-242">No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão.</span><span class="sxs-lookup"><span data-stu-id="222d8-242">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="222d8-243">Isso não inclui o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="222d8-243">This does not include .NET Framework.</span></span>

<span data-ttu-id="222d8-244">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-244">**Why**</span></span>

<span data-ttu-id="222d8-245">Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.</span><span class="sxs-lookup"><span data-stu-id="222d8-245">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="222d8-246">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-246">**Mitigations**</span></span>

<span data-ttu-id="222d8-247">Use EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="222d8-247">Use EF Core 3.1.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="222d8-248">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="222d8-248">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="222d8-249">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="222d8-249">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="222d8-250">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-250">**Old behavior**</span></span>

<span data-ttu-id="222d8-251">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="222d8-251">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="222d8-252">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-252">**New behavior**</span></span>

<span data-ttu-id="222d8-253">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-253">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="222d8-254">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-254">**Why**</span></span>

<span data-ttu-id="222d8-255">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="222d8-255">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="222d8-256">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="222d8-256">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="222d8-257">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="222d8-257">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="222d8-258">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="222d8-258">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="222d8-259">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-259">**Mitigations**</span></span>

<span data-ttu-id="222d8-260">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="222d8-260">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="222d8-261">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="222d8-261">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="222d8-262">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="222d8-262">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="222d8-263">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-263">**Old behavior**</span></span>

<span data-ttu-id="222d8-264">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="222d8-264">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="222d8-265">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-265">**New behavior**</span></span>

<span data-ttu-id="222d8-266">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="222d8-266">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="222d8-267">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-267">**Why**</span></span>

<span data-ttu-id="222d8-268">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="222d8-268">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="222d8-269">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-269">**Mitigations**</span></span>

<span data-ttu-id="222d8-270">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="222d8-270">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="222d8-271">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="222d8-271">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="222d8-272">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="222d8-272">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="222d8-273">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="222d8-273">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="222d8-274">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-274">**Old behavior**</span></span>

<span data-ttu-id="222d8-275">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="222d8-275">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="222d8-276">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-276">**New behavior**</span></span>

<span data-ttu-id="222d8-277">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="222d8-277">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="222d8-278">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-278">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="222d8-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="222d8-279">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="222d8-280">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-280">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="222d8-281">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="222d8-281">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="222d8-282">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-282">**Why**</span></span>

<span data-ttu-id="222d8-283">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="222d8-283">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="222d8-284">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="222d8-284">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="222d8-285">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-285">**Mitigations**</span></span>

<span data-ttu-id="222d8-286">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="222d8-286">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="222d8-287">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="222d8-287">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="222d8-288">Acompanhamento do problema #15392</span><span class="sxs-lookup"><span data-stu-id="222d8-288">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="222d8-289">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-289">**Old behavior**</span></span>

<span data-ttu-id="222d8-290">Antes EF Core 3,0, o método das tentou detectar se o SQL passado pode ser composto.</span><span class="sxs-lookup"><span data-stu-id="222d8-290">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="222d8-291">Ele fez a avaliação do cliente quando o SQL era não combinável como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="222d8-291">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="222d8-292">A consulta a seguir funcionou executando o procedimento armazenado no servidor e fazendo FirstOrDefault no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="222d8-292">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="222d8-293">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-293">**New behavior**</span></span>

<span data-ttu-id="222d8-294">A partir do EF Core 3,0, EF Core não tentará analisar o SQL.</span><span class="sxs-lookup"><span data-stu-id="222d8-294">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="222d8-295">Portanto, se você estiver compondo após FromSqlRaw/FromSqlInterpolated, EF Core comporá o SQL fazendo a subconsulta.</span><span class="sxs-lookup"><span data-stu-id="222d8-295">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="222d8-296">Portanto, se você estiver usando um procedimento armazenado com composição, receberá uma exceção de sintaxe SQL inválida.</span><span class="sxs-lookup"><span data-stu-id="222d8-296">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="222d8-297">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-297">**Why**</span></span>

<span data-ttu-id="222d8-298">O EF Core 3,0 não dá suporte à avaliação automática do cliente, pois foi propenso a erros, conforme explicado [aqui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="222d8-298">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="222d8-299">**Mitigação**</span><span class="sxs-lookup"><span data-stu-id="222d8-299">**Mitigation**</span></span>

<span data-ttu-id="222d8-300">Se você estiver usando um procedimento armazenado em FromSqlRaw/FromSqlInterpolated, saberá que ele não pode ser composto, para que você possa adicionar __AsEnumerable/AsAsyncEnumerable__ logo após a chamada do método das para evitar qualquer composição no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="222d8-300">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="222d8-301">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="222d8-301">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="222d8-302">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="222d8-302">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="222d8-303">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-303">**Old behavior**</span></span>

<span data-ttu-id="222d8-304">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="222d8-304">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="222d8-305">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-305">**New behavior**</span></span>

<span data-ttu-id="222d8-306">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="222d8-306">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="222d8-307">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="222d8-307">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="222d8-308">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-308">**Why**</span></span>

<span data-ttu-id="222d8-309">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="222d8-309">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="222d8-310">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-310">**Mitigations**</span></span>

<span data-ttu-id="222d8-311">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="222d8-311">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="222d8-312">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="222d8-312">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="222d8-313">Problema de acompanhamento nº 13518</span><span class="sxs-lookup"><span data-stu-id="222d8-313">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="222d8-314">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-314">**Old behavior**</span></span>

<span data-ttu-id="222d8-315">Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="222d8-315">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="222d8-316">Isso corresponde ao comportamento das consultas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="222d8-316">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="222d8-317">Por exemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="222d8-317">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="222d8-318">retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.</span><span class="sxs-lookup"><span data-stu-id="222d8-318">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="222d8-319">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-319">**New behavior**</span></span>

<span data-ttu-id="222d8-320">No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado.</span><span class="sxs-lookup"><span data-stu-id="222d8-320">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="222d8-321">Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.</span><span class="sxs-lookup"><span data-stu-id="222d8-321">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="222d8-322">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-322">**Why**</span></span>

<span data-ttu-id="222d8-323">A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória.</span><span class="sxs-lookup"><span data-stu-id="222d8-323">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="222d8-324">Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="222d8-324">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="222d8-325">Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="222d8-325">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="222d8-326">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-326">**Mitigations**</span></span>

<span data-ttu-id="222d8-327">Use uma consulta de acompanhamento se a resolução de identidade for necessária.</span><span class="sxs-lookup"><span data-stu-id="222d8-327">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="222d8-328">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="222d8-328">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="222d8-329">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="222d8-329">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="222d8-330">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="222d8-330">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="222d8-331">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="222d8-331">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="222d8-332">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="222d8-332">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="222d8-333">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="222d8-333">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="222d8-334">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-334">**Old behavior**</span></span>

<span data-ttu-id="222d8-335">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="222d8-335">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="222d8-336">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="222d8-336">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="222d8-337">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-337">**New behavior**</span></span>

<span data-ttu-id="222d8-338">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="222d8-338">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="222d8-339">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-339">**Why**</span></span>

<span data-ttu-id="222d8-340">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="222d8-340">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="222d8-341">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-341">**Mitigations**</span></span>

<span data-ttu-id="222d8-342">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="222d8-342">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="222d8-343">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="222d8-343">This can be avoided by:</span></span>
* <span data-ttu-id="222d8-344">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="222d8-344">Not using store-generated keys.</span></span>
* <span data-ttu-id="222d8-345">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="222d8-345">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="222d8-346">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="222d8-346">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="222d8-347">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="222d8-347">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="222d8-348">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="222d8-348">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="222d8-349">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="222d8-349">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="222d8-350">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-350">**Old behavior**</span></span>

<span data-ttu-id="222d8-351">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="222d8-351">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="222d8-352">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-352">**New behavior**</span></span>

<span data-ttu-id="222d8-353">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="222d8-353">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="222d8-354">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="222d8-354">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="222d8-355">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="222d8-355">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="222d8-356">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-356">**Why**</span></span>

<span data-ttu-id="222d8-357">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="222d8-357">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="222d8-358">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-358">**Mitigations**</span></span>

<span data-ttu-id="222d8-359">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="222d8-359">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="222d8-360">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="222d8-360">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="222d8-361">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="222d8-361">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="222d8-362">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="222d8-362">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="222d8-363">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-363">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="222d8-364">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="222d8-364">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="222d8-365">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-365">**Old behavior**</span></span>

<span data-ttu-id="222d8-366">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="222d8-366">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="222d8-367">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-367">**New behavior**</span></span>

<span data-ttu-id="222d8-368">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="222d8-368">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="222d8-369">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-369">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="222d8-370">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-370">**Why**</span></span>

<span data-ttu-id="222d8-371">Essa alteração foi feita para melhorar a experiência de vinculação de dados e cenários de auditoria, em que é importante entender quais entidades serão excluídas _antes_ que `SaveChanges` seja chamado.</span><span class="sxs-lookup"><span data-stu-id="222d8-371">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="222d8-372">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-372">**Mitigations**</span></span>

<span data-ttu-id="222d8-373">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="222d8-373">The previous behavior can be restored through settings on `context.ChangeTracker`.</span></span>
<span data-ttu-id="222d8-374">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-374">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="222d8-375">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="222d8-375">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="222d8-376">Acompanhamento do problema #18022</span><span class="sxs-lookup"><span data-stu-id="222d8-376">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="222d8-377">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-377">**Old behavior**</span></span>

<span data-ttu-id="222d8-378">Antes de 3,0, carregar cuidadosamente as navegações de coleção por meio de operadores de `Include` fez com que várias consultas sejam geradas no banco de dados relacional, uma para cada tipo de entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="222d8-378">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="222d8-379">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-379">**New behavior**</span></span>

<span data-ttu-id="222d8-380">A partir do 3,0, o EF Core gera uma única consulta com junções em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="222d8-380">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="222d8-381">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-381">**Why**</span></span>

<span data-ttu-id="222d8-382">A emissão de várias consultas para implementar uma única consulta LINQ causou vários problemas, incluindo o desempenho negativo à medida que várias viagens de ida e volta do banco de dados eram necessárias e os problemas de coerência do dado, uma vez que cada consulta poderia observar um estado diferente.</span><span class="sxs-lookup"><span data-stu-id="222d8-382">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="222d8-383">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-383">**Mitigations**</span></span>

<span data-ttu-id="222d8-384">Embora tecnicamente essa não seja uma alteração significativa, ela pode ter um efeito considerável no desempenho do aplicativo quando uma única consulta contém um grande número de operador de `Include` em navegações de coleção.</span><span class="sxs-lookup"><span data-stu-id="222d8-384">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="222d8-385">[Consulte este comentário](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) para obter mais informações e reescrever consultas de maneira mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="222d8-385">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="222d8-386">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="222d8-386">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="222d8-387">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="222d8-387">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="222d8-388">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-388">**Old behavior**</span></span>

<span data-ttu-id="222d8-389">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="222d8-389">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="222d8-390">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-390">**New behavior**</span></span>

<span data-ttu-id="222d8-391">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="222d8-391">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="222d8-392">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-392">**Why**</span></span>

<span data-ttu-id="222d8-393">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="222d8-393">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="222d8-394">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-394">**Mitigations**</span></span>

<span data-ttu-id="222d8-395">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="222d8-395">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="222d8-396">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="222d8-396">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="222d8-397">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="222d8-397">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="222d8-398">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-398">**Old behavior**</span></span>

<span data-ttu-id="222d8-399">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/keyless-entity-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="222d8-399">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="222d8-400">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="222d8-400">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="222d8-401">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-401">**New behavior**</span></span>

<span data-ttu-id="222d8-402">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="222d8-402">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="222d8-403">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="222d8-403">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="222d8-404">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-404">**Why**</span></span>

<span data-ttu-id="222d8-405">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="222d8-405">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="222d8-406">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="222d8-406">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="222d8-407">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="222d8-407">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="222d8-408">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-408">**Mitigations**</span></span>

<span data-ttu-id="222d8-409">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="222d8-409">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="222d8-410">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="222d8-410">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="222d8-411">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="222d8-411">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="222d8-412">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="222d8-412">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="222d8-413">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="222d8-413">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="222d8-414">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="222d8-414">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="222d8-415">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="222d8-415">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="222d8-416">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-416">**Old behavior**</span></span>

<span data-ttu-id="222d8-417">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="222d8-417">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="222d8-418">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-418">**New behavior**</span></span>

<span data-ttu-id="222d8-419">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="222d8-419">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="222d8-420">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-420">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="222d8-421">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="222d8-421">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="222d8-422">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="222d8-422">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="222d8-423">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-423">For example:</span></span>

```csharp
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

<span data-ttu-id="222d8-424">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="222d8-424">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="222d8-425">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-425">**Why**</span></span>

<span data-ttu-id="222d8-426">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="222d8-426">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="222d8-427">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="222d8-427">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="222d8-428">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-428">**Mitigations**</span></span>

<span data-ttu-id="222d8-429">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="222d8-429">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="222d8-430">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="222d8-430">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="222d8-431">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="222d8-431">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="222d8-432">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-432">**Old behavior**</span></span>

<span data-ttu-id="222d8-433">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="222d8-433">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="222d8-434">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="222d8-434">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="222d8-435">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-435">**New behavior**</span></span>

<span data-ttu-id="222d8-436">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="222d8-436">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="222d8-437">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="222d8-437">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="222d8-438">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-438">**Mitigations**</span></span>

<span data-ttu-id="222d8-439">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="222d8-439">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="222d8-440">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="222d8-440">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="222d8-441">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="222d8-441">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="222d8-442">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="222d8-442">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="222d8-443">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-443">**Old behavior**</span></span>

<span data-ttu-id="222d8-444">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="222d8-444">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="222d8-445">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="222d8-445">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="222d8-446">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-446">**New behavior**</span></span>

<span data-ttu-id="222d8-447">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="222d8-447">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="222d8-448">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="222d8-448">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="222d8-449">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-449">**Why**</span></span>

<span data-ttu-id="222d8-450">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="222d8-450">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="222d8-451">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-451">**Mitigations**</span></span>

<span data-ttu-id="222d8-452">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="222d8-452">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="222d8-453">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="222d8-453">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="222d8-454">Entidades de propriedade não podem ser consultadas sem o proprietário usando uma consulta de rastreamento</span><span class="sxs-lookup"><span data-stu-id="222d8-454">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="222d8-455">Acompanhamento do problema #18876</span><span class="sxs-lookup"><span data-stu-id="222d8-455">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="222d8-456">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-456">**Old behavior**</span></span>

<span data-ttu-id="222d8-457">Antes de EF Core 3,0, as entidades de propriedade podem ser consultadas como qualquer outra navegação.</span><span class="sxs-lookup"><span data-stu-id="222d8-457">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="222d8-458">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-458">**New behavior**</span></span>

<span data-ttu-id="222d8-459">A partir do 3,0, o EF Core gerará se uma consulta de rastreamento projeta uma entidade de propriedade sem o proprietário.</span><span class="sxs-lookup"><span data-stu-id="222d8-459">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="222d8-460">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-460">**Why**</span></span>

<span data-ttu-id="222d8-461">Entidades de propriedade não podem ser manipuladas sem o proprietário, portanto, na grande maioria dos casos, consultá-las dessa maneira é um erro.</span><span class="sxs-lookup"><span data-stu-id="222d8-461">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="222d8-462">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-462">**Mitigations**</span></span>

<span data-ttu-id="222d8-463">Se a entidade de propriedade deve ser controlada para ser modificada de qualquer forma posteriormente, o proprietário deve ser incluído na consulta.</span><span class="sxs-lookup"><span data-stu-id="222d8-463">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="222d8-464">Caso contrário, adicione uma chamada de `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="222d8-464">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="222d8-465">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="222d8-465">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="222d8-466">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="222d8-466">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="222d8-467">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-467">**Old behavior**</span></span>

<span data-ttu-id="222d8-468">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="222d8-468">Consider the following model:</span></span>
```csharp
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

<span data-ttu-id="222d8-469">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="222d8-469">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="222d8-470">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-470">**New behavior**</span></span>

<span data-ttu-id="222d8-471">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="222d8-471">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="222d8-472">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-472">**Why**</span></span>

<span data-ttu-id="222d8-473">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="222d8-473">The old behavoir was unexpected.</span></span>

<span data-ttu-id="222d8-474">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-474">**Mitigations**</span></span>

<span data-ttu-id="222d8-475">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="222d8-475">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="222d8-476">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="222d8-476">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="222d8-477">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="222d8-477">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="222d8-478">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-478">**Old behavior**</span></span>

<span data-ttu-id="222d8-479">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="222d8-479">Consider the following model:</span></span>
```csharp
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
<span data-ttu-id="222d8-480">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="222d8-480">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="222d8-481">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="222d8-481">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="222d8-482">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-482">**New behavior**</span></span>

<span data-ttu-id="222d8-483">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="222d8-483">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="222d8-484">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="222d8-484">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="222d8-485">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-485">For example:</span></span>

```csharp
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

```csharp
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

<span data-ttu-id="222d8-486">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-486">**Why**</span></span>

<span data-ttu-id="222d8-487">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="222d8-487">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="222d8-488">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-488">**Mitigations**</span></span>

<span data-ttu-id="222d8-489">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="222d8-489">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="222d8-490">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="222d8-490">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="222d8-491">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="222d8-491">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="222d8-492">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-492">**Old behavior**</span></span>

<span data-ttu-id="222d8-493">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="222d8-493">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
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

<span data-ttu-id="222d8-494">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-494">**New behavior**</span></span>

<span data-ttu-id="222d8-495">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="222d8-495">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="222d8-496">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-496">**Why**</span></span>

<span data-ttu-id="222d8-497">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="222d8-497">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="222d8-498">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="222d8-498">The new behavior also matches EF6.</span></span>

<span data-ttu-id="222d8-499">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-499">**Mitigations**</span></span>

<span data-ttu-id="222d8-500">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="222d8-500">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="222d8-501">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="222d8-501">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="222d8-502">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="222d8-502">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="222d8-503">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-503">**Old behavior**</span></span>

<span data-ttu-id="222d8-504">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="222d8-504">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="222d8-505">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-505">**New behavior**</span></span>

<span data-ttu-id="222d8-506">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="222d8-506">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="222d8-507">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="222d8-507">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="222d8-508">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-508">**Why**</span></span>

<span data-ttu-id="222d8-509">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="222d8-509">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="222d8-510">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-510">**Mitigations**</span></span>

<span data-ttu-id="222d8-511">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="222d8-511">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="222d8-512">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="222d8-512">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="222d8-513">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-513">Backing fields are used by default</span></span>

[<span data-ttu-id="222d8-514">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="222d8-514">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="222d8-515">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-515">**Old behavior**</span></span>

<span data-ttu-id="222d8-516">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="222d8-516">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="222d8-517">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="222d8-517">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="222d8-518">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-518">**New behavior**</span></span>

<span data-ttu-id="222d8-519">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="222d8-519">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="222d8-520">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="222d8-520">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="222d8-521">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-521">**Why**</span></span>

<span data-ttu-id="222d8-522">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="222d8-522">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="222d8-523">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-523">**Mitigations**</span></span>

<span data-ttu-id="222d8-524">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="222d8-524">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="222d8-525">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-525">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="222d8-526">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="222d8-526">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="222d8-527">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="222d8-527">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="222d8-528">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-528">**Old behavior**</span></span>

<span data-ttu-id="222d8-529">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="222d8-529">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="222d8-530">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="222d8-530">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="222d8-531">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-531">**New behavior**</span></span>

<span data-ttu-id="222d8-532">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="222d8-532">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="222d8-533">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-533">**Why**</span></span>

<span data-ttu-id="222d8-534">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="222d8-534">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="222d8-535">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-535">**Mitigations**</span></span>

<span data-ttu-id="222d8-536">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="222d8-536">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="222d8-537">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="222d8-537">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="222d8-538">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="222d8-538">Field-only property names should match the field name</span></span>

<span data-ttu-id="222d8-539">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-539">**Old behavior**</span></span>

<span data-ttu-id="222d8-540">Antes de EF Core 3,0, uma propriedade poderia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome tiver sido encontrada no tipo .NET, EF Core tentará fazer a correspondência com um campo usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="222d8-540">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="222d8-541">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-541">**New behavior**</span></span>

<span data-ttu-id="222d8-542">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="222d8-542">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="222d8-543">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-543">**Why**</span></span>

<span data-ttu-id="222d8-544">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="222d8-544">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="222d8-545">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-545">**Mitigations**</span></span>

<span data-ttu-id="222d8-546">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="222d8-546">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="222d8-547">Em uma versão futura do EF Core após 3,0, planejamos reabilitar explicitamente um nome de campo diferente do nome da propriedade (consulte o problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="222d8-547">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="222d8-548">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="222d8-548">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="222d8-549">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="222d8-549">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="222d8-550">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-550">**Old behavior**</span></span>

<span data-ttu-id="222d8-551">Antes de EF Core 3,0, chamar `AddDbContext` ou `AddDbContextPool` também registraria o log e os serviços de cache de memória com DI por meio de chamadas para [addlogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="222d8-551">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="222d8-552">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-552">**New behavior**</span></span>

<span data-ttu-id="222d8-553">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="222d8-553">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="222d8-554">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-554">**Why**</span></span>

<span data-ttu-id="222d8-555">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="222d8-555">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="222d8-556">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-556">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="222d8-557">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-557">**Mitigations**</span></span>

<span data-ttu-id="222d8-558">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="222d8-558">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="222d8-559">AddEntityFramework \* adiciona IMemoryCache com um limite de tamanho</span><span class="sxs-lookup"><span data-stu-id="222d8-559">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="222d8-560">Acompanhamento do problema #12905</span><span class="sxs-lookup"><span data-stu-id="222d8-560">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="222d8-561">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-561">**Old behavior**</span></span>

<span data-ttu-id="222d8-562">Antes de EF Core 3,0, chamar `AddEntityFramework*` métodos também registraria os serviços de cache de memória com DI sem um limite de tamanho.</span><span class="sxs-lookup"><span data-stu-id="222d8-562">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="222d8-563">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-563">**New behavior**</span></span>

<span data-ttu-id="222d8-564">A partir do EF Core 3,0, `AddEntityFramework*` registrará um serviço IMemoryCache com um limite de tamanho.</span><span class="sxs-lookup"><span data-stu-id="222d8-564">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="222d8-565">Se qualquer outro serviço adicionado posteriormente depender do IMemoryCache, ele poderá alcançar rapidamente o limite padrão causando exceções ou degradação do desempenho.</span><span class="sxs-lookup"><span data-stu-id="222d8-565">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="222d8-566">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-566">**Why**</span></span>

<span data-ttu-id="222d8-567">Usar IMemoryCache sem um limite pode resultar em uso não controlado de memória se houver um bug na lógica de cache de consulta ou se as consultas forem geradas dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-567">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="222d8-568">Ter um limite padrão reduz um ataque de DoS potencial.</span><span class="sxs-lookup"><span data-stu-id="222d8-568">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="222d8-569">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-569">**Mitigations**</span></span>

<span data-ttu-id="222d8-570">Na maioria dos casos, a chamada de `AddEntityFramework*` não será necessária se `AddDbContext` ou `AddDbContextPool` também for chamado.</span><span class="sxs-lookup"><span data-stu-id="222d8-570">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="222d8-571">Portanto, a melhor mitigação é remover a chamada `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="222d8-571">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="222d8-572">Se seu aplicativo precisar desses serviços, registre uma implementação de IMemoryCache explicitamente com o contêiner DI com antecedência usando o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="222d8-572">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="222d8-573">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="222d8-573">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="222d8-574">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="222d8-574">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="222d8-575">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-575">**Old behavior**</span></span>

<span data-ttu-id="222d8-576">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="222d8-576">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="222d8-577">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="222d8-577">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="222d8-578">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-578">**New behavior**</span></span>

<span data-ttu-id="222d8-579">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="222d8-579">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="222d8-580">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="222d8-580">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="222d8-581">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="222d8-581">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="222d8-582">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="222d8-582">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="222d8-583">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-583">**Why**</span></span>

<span data-ttu-id="222d8-584">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="222d8-584">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="222d8-585">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-585">**Mitigations**</span></span>

<span data-ttu-id="222d8-586">Chame `ChangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="222d8-586">Call `ChangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="222d8-587">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-587">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="222d8-588">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="222d8-588">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="222d8-589">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-589">**Old behavior**</span></span>

<span data-ttu-id="222d8-590">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="222d8-590">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="222d8-591">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="222d8-591">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="222d8-592">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-592">**New behavior**</span></span>

<span data-ttu-id="222d8-593">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="222d8-593">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="222d8-594">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-594">**Why**</span></span>

<span data-ttu-id="222d8-595">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="222d8-595">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="222d8-596">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-596">**Mitigations**</span></span>

<span data-ttu-id="222d8-597">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="222d8-597">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="222d8-598">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="222d8-598">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="222d8-599">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="222d8-599">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="222d8-600">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="222d8-600">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="222d8-601">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="222d8-601">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="222d8-602">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-602">**Old behavior**</span></span>

<span data-ttu-id="222d8-603">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="222d8-603">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="222d8-604">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-604">**New behavior**</span></span>

<span data-ttu-id="222d8-605">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="222d8-605">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="222d8-606">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-606">**Why**</span></span>

<span data-ttu-id="222d8-607">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="222d8-607">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="222d8-608">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-608">**Mitigations**</span></span>

<span data-ttu-id="222d8-609">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-609">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="222d8-610">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="222d8-610">This isn't common.</span></span>
<span data-ttu-id="222d8-611">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="222d8-611">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="222d8-612">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="222d8-612">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="222d8-613">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="222d8-613">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="222d8-614">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="222d8-614">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="222d8-615">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-615">**Old behavior**</span></span>

<span data-ttu-id="222d8-616">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="222d8-616">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="222d8-617">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="222d8-617">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="222d8-618">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="222d8-618">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="222d8-619">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-619">**New behavior**</span></span>

<span data-ttu-id="222d8-620">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="222d8-620">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="222d8-621">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="222d8-621">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="222d8-622">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="222d8-622">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="222d8-623">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="222d8-623">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="222d8-624">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-624">**Why**</span></span>

<span data-ttu-id="222d8-625">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="222d8-625">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="222d8-626">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-626">**Mitigations**</span></span>

<span data-ttu-id="222d8-627">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="222d8-627">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="222d8-628">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="222d8-628">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="222d8-629">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="222d8-629">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="222d8-630">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-630">**Old behavior**</span></span>

<span data-ttu-id="222d8-631">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="222d8-631">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="222d8-632">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-632">**New behavior**</span></span>

<span data-ttu-id="222d8-633">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="222d8-633">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="222d8-634">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-634">**Why**</span></span>

<span data-ttu-id="222d8-635">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-635">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="222d8-636">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-636">**Mitigations**</span></span>

<span data-ttu-id="222d8-637">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="222d8-637">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="222d8-638">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="222d8-638">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="222d8-639">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-639">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="222d8-640">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="222d8-640">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="222d8-641">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="222d8-641">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="222d8-642">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-642">**Old behavior**</span></span>

<span data-ttu-id="222d8-643">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="222d8-643">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="222d8-644">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-644">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="222d8-645">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="222d8-645">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="222d8-646">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="222d8-646">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="222d8-647">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-647">**New behavior**</span></span>

<span data-ttu-id="222d8-648">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="222d8-648">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="222d8-649">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-649">**Why**</span></span>

<span data-ttu-id="222d8-650">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="222d8-650">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="222d8-651">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-651">**Mitigations**</span></span>

<span data-ttu-id="222d8-652">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-652">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="222d8-653">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="222d8-653">This is not common.</span></span>
<span data-ttu-id="222d8-654">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="222d8-654">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="222d8-655">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-655">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="222d8-656">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="222d8-656">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="222d8-657">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="222d8-657">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="222d8-658">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-658">**Old behavior**</span></span>

<span data-ttu-id="222d8-659">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="222d8-659">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="222d8-660">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="222d8-660">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="222d8-661">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-661">**New behavior**</span></span>

<span data-ttu-id="222d8-662">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="222d8-662">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="222d8-663">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-663">**Why**</span></span>

<span data-ttu-id="222d8-664">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="222d8-664">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="222d8-665">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-665">**Mitigations**</span></span>

<span data-ttu-id="222d8-666">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="222d8-666">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="222d8-667">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="222d8-667">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="222d8-668">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="222d8-668">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="222d8-669">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="222d8-669">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="222d8-670">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="222d8-670">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="222d8-671">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-671">**Old behavior**</span></span>

<span data-ttu-id="222d8-672">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="222d8-672">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="222d8-673">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-673">**New behavior**</span></span>

<span data-ttu-id="222d8-674">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="222d8-674">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="222d8-675">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-675">**Why**</span></span>

<span data-ttu-id="222d8-676">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="222d8-676">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="222d8-677">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-677">**Mitigations**</span></span>

<span data-ttu-id="222d8-678">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="222d8-678">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="222d8-679">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="222d8-679">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="222d8-680">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="222d8-680">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="222d8-681">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="222d8-681">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="222d8-682">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-682">**Old behavior**</span></span>

<span data-ttu-id="222d8-683">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="222d8-683">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="222d8-684">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-684">**New behavior**</span></span>

<span data-ttu-id="222d8-685">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="222d8-685">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="222d8-686">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-686">**Why**</span></span>

<span data-ttu-id="222d8-687">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="222d8-687">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="222d8-688">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="222d8-688">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="222d8-689">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-689">**Mitigations**</span></span>

<span data-ttu-id="222d8-690">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="222d8-690">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="222d8-691">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="222d8-691">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="222d8-692">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="222d8-692">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="222d8-693">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-693">**Old behavior**</span></span>

<span data-ttu-id="222d8-694">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="222d8-694">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="222d8-695">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-695">**New behavior**</span></span>

<span data-ttu-id="222d8-696">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="222d8-696">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="222d8-697">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="222d8-697">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="222d8-698">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-698">**Why**</span></span>

<span data-ttu-id="222d8-699">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="222d8-699">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="222d8-700">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-700">**Mitigations**</span></span>

<span data-ttu-id="222d8-701">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="222d8-701">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="222d8-702">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="222d8-702">Metadata API changes</span></span>

[<span data-ttu-id="222d8-703">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="222d8-703">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="222d8-704">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-704">**New behavior**</span></span>

<span data-ttu-id="222d8-705">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="222d8-705">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="222d8-706">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-706">**Why**</span></span>

<span data-ttu-id="222d8-707">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="222d8-707">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="222d8-708">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-708">**Mitigations**</span></span>

<span data-ttu-id="222d8-709">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="222d8-709">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="222d8-710">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="222d8-710">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="222d8-711">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="222d8-711">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="222d8-712">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-712">**New behavior**</span></span>

<span data-ttu-id="222d8-713">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="222d8-713">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="222d8-714">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-714">**Why**</span></span>

<span data-ttu-id="222d8-715">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="222d8-715">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="222d8-716">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-716">**Mitigations**</span></span>

<span data-ttu-id="222d8-717">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="222d8-717">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="222d8-718">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-718">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="222d8-719">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="222d8-719">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="222d8-720">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-720">**Old behavior**</span></span>

<span data-ttu-id="222d8-721">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="222d8-721">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="222d8-722">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-722">**New behavior**</span></span>

<span data-ttu-id="222d8-723">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="222d8-723">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="222d8-724">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-724">**Why**</span></span>

<span data-ttu-id="222d8-725">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="222d8-725">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="222d8-726">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-726">**Mitigations**</span></span>

<span data-ttu-id="222d8-727">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-727">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="222d8-728">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="222d8-728">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="222d8-729">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="222d8-729">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="222d8-730">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-730">**Old behavior**</span></span>

<span data-ttu-id="222d8-731">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="222d8-731">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="222d8-732">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-732">**New behavior**</span></span>

<span data-ttu-id="222d8-733">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="222d8-733">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="222d8-734">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-734">**Why**</span></span>

<span data-ttu-id="222d8-735">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="222d8-735">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="222d8-736">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-736">**Mitigations**</span></span>

<span data-ttu-id="222d8-737">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="222d8-737">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="222d8-738">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-738">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="222d8-739">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="222d8-739">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="222d8-740">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-740">**Old behavior**</span></span>

<span data-ttu-id="222d8-741">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="222d8-741">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="222d8-742">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-742">**New behavior**</span></span>

<span data-ttu-id="222d8-743">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="222d8-743">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="222d8-744">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-744">**Why**</span></span>

<span data-ttu-id="222d8-745">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="222d8-745">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="222d8-746">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="222d8-746">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="222d8-747">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-747">**Mitigations**</span></span>

<span data-ttu-id="222d8-748">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="222d8-748">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="222d8-749">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="222d8-749">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="222d8-750">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="222d8-750">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="222d8-751">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="222d8-751">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="222d8-752">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="222d8-752">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="222d8-753">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-753">**Old behavior**</span></span>

<span data-ttu-id="222d8-754">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="222d8-754">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="222d8-755">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="222d8-755">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="222d8-756">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-756">**New behavior**</span></span>

<span data-ttu-id="222d8-757">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="222d8-757">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="222d8-758">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-758">**Why**</span></span>

<span data-ttu-id="222d8-759">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="222d8-759">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="222d8-760">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-760">**Mitigations**</span></span>

<span data-ttu-id="222d8-761">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="222d8-761">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="222d8-762">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="222d8-762">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="222d8-763">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="222d8-763">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="222d8-764">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="222d8-764">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="222d8-765">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="222d8-765">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="222d8-766">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-766">**Old behavior**</span></span>

<span data-ttu-id="222d8-767">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="222d8-767">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="222d8-768">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-768">**New behavior**</span></span>

<span data-ttu-id="222d8-769">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="222d8-769">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="222d8-770">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-770">**Why**</span></span>

<span data-ttu-id="222d8-771">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="222d8-771">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="222d8-772">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="222d8-772">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="222d8-773">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-773">**Mitigations**</span></span>

<span data-ttu-id="222d8-774">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="222d8-774">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="222d8-775">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="222d8-775">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="222d8-776">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="222d8-776">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="222d8-777">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="222d8-777">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="222d8-778">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="222d8-778">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="222d8-779">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="222d8-779">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="222d8-780">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-780">**Old behavior**</span></span>

<span data-ttu-id="222d8-781">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="222d8-781">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="222d8-782">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-782">**New behavior**</span></span>

<span data-ttu-id="222d8-783">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="222d8-783">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="222d8-784">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-784">**Why**</span></span>

<span data-ttu-id="222d8-785">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="222d8-785">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="222d8-786">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-786">**Mitigations**</span></span>

<span data-ttu-id="222d8-787">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="222d8-787">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="222d8-788">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="222d8-788">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="222d8-789">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="222d8-789">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="222d8-790">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="222d8-790">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="222d8-791">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="222d8-791">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="222d8-792">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-792">**Old behavior**</span></span>

<span data-ttu-id="222d8-793">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="222d8-793">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="222d8-794">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-794">**New behavior**</span></span>

<span data-ttu-id="222d8-795">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="222d8-795">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="222d8-796">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-796">**Why**</span></span>

<span data-ttu-id="222d8-797">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="222d8-797">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="222d8-798">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="222d8-798">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="222d8-799">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-799">**Mitigations**</span></span>

<span data-ttu-id="222d8-800">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="222d8-800">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="222d8-801">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-801">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="222d8-802">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="222d8-802">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="222d8-803">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="222d8-803">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="222d8-804">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="222d8-804">**Change**</span></span>

<span data-ttu-id="222d8-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="222d8-805">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="222d8-806">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-806">**Why**</span></span>

<span data-ttu-id="222d8-807">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="222d8-807">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="222d8-808">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-808">**Mitigations**</span></span>

<span data-ttu-id="222d8-809">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="222d8-809">Use the new name.</span></span> <span data-ttu-id="222d8-810">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="222d8-810">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="222d8-811">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="222d8-811">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="222d8-812">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="222d8-812">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="222d8-813">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-813">**Old behavior**</span></span>

<span data-ttu-id="222d8-814">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="222d8-814">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="222d8-815">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-815">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="222d8-816">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-816">**New behavior**</span></span>

<span data-ttu-id="222d8-817">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="222d8-817">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="222d8-818">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-818">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="222d8-819">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-819">**Why**</span></span>

<span data-ttu-id="222d8-820">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="222d8-820">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="222d8-821">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-821">**Mitigations**</span></span>

<span data-ttu-id="222d8-822">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="222d8-822">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="222d8-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="222d8-823">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="222d8-824">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="222d8-824">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="222d8-825">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-825">**Old behavior**</span></span>

<span data-ttu-id="222d8-826">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="222d8-826">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="222d8-827">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-827">**New behavior**</span></span>

<span data-ttu-id="222d8-828">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="222d8-828">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="222d8-829">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-829">**Why**</span></span>

<span data-ttu-id="222d8-830">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="222d8-830">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="222d8-831">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="222d8-831">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="222d8-832">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-832">**Mitigations**</span></span>

<span data-ttu-id="222d8-833">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="222d8-833">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="222d8-834">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="222d8-834">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="222d8-835">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="222d8-835">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="222d8-836">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-836">**Old behavior**</span></span>

<span data-ttu-id="222d8-837">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="222d8-837">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="222d8-838">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-838">**New behavior**</span></span>

<span data-ttu-id="222d8-839">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="222d8-839">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="222d8-840">Isso significa que a dependência não flui transitivamente em outros projetos e que você não pode mais, por padrão, referenciar seu assembly.</span><span class="sxs-lookup"><span data-stu-id="222d8-840">This means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="222d8-841">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-841">**Why**</span></span>

<span data-ttu-id="222d8-842">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="222d8-842">This package is only intended to be used at design time.</span></span> <span data-ttu-id="222d8-843">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="222d8-843">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="222d8-844">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="222d8-844">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="222d8-845">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-845">**Mitigations**</span></span>

<span data-ttu-id="222d8-846">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="222d8-846">If you need to reference this package to override EF Core's design-time behavior, then you can update PackageReference item metadata in your project.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="222d8-847">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="222d8-847">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span> <span data-ttu-id="222d8-848">Essa referência explícita deve ser adicionada a qualquer projeto em que os tipos do pacote sejam necessários.</span><span class="sxs-lookup"><span data-stu-id="222d8-848">Such an explicit reference must be added to any project where the types from the package are needed.</span></span>

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="222d8-849">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="222d8-849">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="222d8-850">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="222d8-850">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="222d8-851">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-851">**Old behavior**</span></span>

<span data-ttu-id="222d8-852">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="222d8-852">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="222d8-853">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-853">**New behavior**</span></span>

<span data-ttu-id="222d8-854">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="222d8-854">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="222d8-855">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-855">**Why**</span></span>

<span data-ttu-id="222d8-856">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="222d8-856">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="222d8-857">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="222d8-857">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="222d8-858">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-858">**Mitigations**</span></span>

<span data-ttu-id="222d8-859">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="222d8-859">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="222d8-860">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="222d8-860">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="222d8-861">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="222d8-861">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="222d8-862">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="222d8-862">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="222d8-863">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-863">**Old behavior**</span></span>

<span data-ttu-id="222d8-864">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="222d8-864">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="222d8-865">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-865">**New behavior**</span></span>

<span data-ttu-id="222d8-866">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="222d8-866">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="222d8-867">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-867">**Why**</span></span>

<span data-ttu-id="222d8-868">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="222d8-868">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="222d8-869">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-869">**Mitigations**</span></span>

<span data-ttu-id="222d8-870">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="222d8-870">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="222d8-871">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="222d8-871">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="222d8-872">Microsoft. Data. SqlClient é usado em vez de System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="222d8-872">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="222d8-873">Acompanhamento do problema #15636</span><span class="sxs-lookup"><span data-stu-id="222d8-873">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="222d8-874">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-874">**Old behavior**</span></span>

<span data-ttu-id="222d8-875">Microsoft. EntityFrameworkCore. SqlServer anteriormente dependia de System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="222d8-875">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="222d8-876">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-876">**New behavior**</span></span>

<span data-ttu-id="222d8-877">Atualizamos nosso pacote para depender do Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="222d8-877">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="222d8-878">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-878">**Why**</span></span>

<span data-ttu-id="222d8-879">Microsoft. Data. SqlClient é o principal driver de acesso a dados para SQL Server em andamento, e System. Data. SqlClient não é mais o foco do desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="222d8-879">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="222d8-880">Alguns recursos importantes, como o Always Encrypted, estão disponíveis apenas em Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="222d8-880">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="222d8-881">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-881">**Mitigations**</span></span>

<span data-ttu-id="222d8-882">Se o seu código usar uma dependência direta em System. Data. SqlClient, você deverá alterá-lo para fazer referência a Microsoft. Data. SqlClient, em vez disso; como os dois pacotes mantêm um grau muito alto de compatibilidade de API, isso deve ser apenas um pacote simples e uma alteração de namespace.</span><span class="sxs-lookup"><span data-stu-id="222d8-882">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="222d8-883">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="222d8-883">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="222d8-884">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="222d8-884">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="222d8-885">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-885">**Old behavior**</span></span>

<span data-ttu-id="222d8-886">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="222d8-886">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="222d8-887">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-887">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="222d8-888">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-888">**New behavior**</span></span>

<span data-ttu-id="222d8-889">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="222d8-889">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="222d8-890">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-890">**Why**</span></span>

<span data-ttu-id="222d8-891">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="222d8-891">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="222d8-892">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-892">**Mitigations**</span></span>

<span data-ttu-id="222d8-893">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="222d8-893">Use full configuration of the relationship.</span></span> <span data-ttu-id="222d8-894">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="222d8-894">For example:</span></span>

```csharp
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="222d8-895">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="222d8-895">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="222d8-896">Acompanhamento do problema #12757</span><span class="sxs-lookup"><span data-stu-id="222d8-896">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="222d8-897">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="222d8-897">**Old behavior**</span></span>

<span data-ttu-id="222d8-898">Um DbFunction configurado com o esquema como uma cadeia de caracteres vazia foi tratado como função interna sem um esquema.</span><span class="sxs-lookup"><span data-stu-id="222d8-898">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="222d8-899">Por exemplo, o código a seguir mapeará `DatePart` função CLR para `DATEPART` função interna no SqlServer.</span><span class="sxs-lookup"><span data-stu-id="222d8-899">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="222d8-900">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="222d8-900">**New behavior**</span></span>

<span data-ttu-id="222d8-901">Todos os mapeamentos DbFunction são considerados mapeados para funções definidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="222d8-901">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="222d8-902">Portanto, o valor da cadeia de caracteres vazia colocaria a função dentro do esquema padrão para o modelo.</span><span class="sxs-lookup"><span data-stu-id="222d8-902">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="222d8-903">Que pode ser o esquema configurado explicitamente por meio da API Fluent `modelBuilder.HasDefaultSchema()` ou `dbo` caso contrário.</span><span class="sxs-lookup"><span data-stu-id="222d8-903">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="222d8-904">**Por que**</span><span class="sxs-lookup"><span data-stu-id="222d8-904">**Why**</span></span>

<span data-ttu-id="222d8-905">O esquema anterior sendo vazio era uma maneira de tratar que a função é interna, mas essa lógica só é aplicável ao SqlServer, em que funções internas não pertencem a nenhum esquema.</span><span class="sxs-lookup"><span data-stu-id="222d8-905">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="222d8-906">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="222d8-906">**Mitigations**</span></span>

<span data-ttu-id="222d8-907">Configure a tradução do DbFunction manualmente para mapeá-lo para uma função interna.</span><span class="sxs-lookup"><span data-stu-id="222d8-907">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
