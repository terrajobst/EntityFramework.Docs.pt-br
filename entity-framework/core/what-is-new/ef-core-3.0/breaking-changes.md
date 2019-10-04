---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f7f04efa8fb8ebc1eb06f256b8ccbd3110af47ab
ms.sourcegitcommit: 705e898b4684e639a57c787fb45c932a27650c2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71934878"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="5e5e6-102">Alterações recentes incluídas no EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="5e5e6-103">As alterações de API e comportamento a seguir têm o potencial de interromper os aplicativos existentes ao atualizá-los para o 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="5e5e6-104">As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="5e5e6-105">Resumo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-105">Summary</span></span>

| <span data-ttu-id="5e5e6-106">**Alteração significativa**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="5e5e6-107">**Causa**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="5e5e6-108">As consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="5e5e6-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="5e5e6-109">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-109">High</span></span>       |
| [<span data-ttu-id="5e5e6-110">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="5e5e6-111">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-111">High</span></span>      |
| [<span data-ttu-id="5e5e6-112">A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="5e5e6-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="5e5e6-113">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-113">High</span></span>      |
| [<span data-ttu-id="5e5e6-114">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="5e5e6-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="5e5e6-115">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-115">High</span></span>      |
| [<span data-ttu-id="5e5e6-116">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="5e5e6-117">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-117">High</span></span>      |
| [<span data-ttu-id="5e5e6-118">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="5e5e6-119">Alto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-119">High</span></span>      |
| [<span data-ttu-id="5e5e6-120">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e5e6-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="5e5e6-121">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-121">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-122">Agora, as exclusões em cascata acontecem imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="5e5e6-123">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-123">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-124">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="5e5e6-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="5e5e6-125">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-125">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-126">DeleteBehavior.Restrict tem uma semântica mais limpa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="5e5e6-127">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-127">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-128">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="5e5e6-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="5e5e6-129">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-129">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-130">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="5e5e6-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="5e5e6-131">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-131">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-132">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="5e5e6-133">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-133">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-134">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="5e5e6-135">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-135">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-136">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="5e5e6-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="5e5e6-137">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-137">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-138">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="5e5e6-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="5e5e6-139">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-139">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-140">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="5e5e6-141">Média</span><span class="sxs-lookup"><span data-stu-id="5e5e6-141">Medium</span></span>      |
| [<span data-ttu-id="5e5e6-142">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="5e5e6-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="5e5e6-143">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-143">Low</span></span>      |
| [<span data-ttu-id="5e5e6-144">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="5e5e6-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="5e5e6-145">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-145">Low</span></span>      |
| [<span data-ttu-id="5e5e6-146">Valores de chave temporários não estão mais definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="5e5e6-147">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-147">Low</span></span>      |
| [<span data-ttu-id="5e5e6-148">As entidades dependentes que compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="5e5e6-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="5e5e6-149">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-149">Low</span></span>      |
| [<span data-ttu-id="5e5e6-150">Todas as entidades que compartilham uma tabela com uma coluna de token de simultaneidade precisam mapeá-la para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="5e5e6-151">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-151">Low</span></span>      |
| [<span data-ttu-id="5e5e6-152">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="5e5e6-153">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-153">Low</span></span>      |
| [<span data-ttu-id="5e5e6-154">A convenção da propriedade de chave estrangeira não corresponde mais ao mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="5e5e6-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="5e5e6-155">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-155">Low</span></span>      |
| [<span data-ttu-id="5e5e6-156">Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="5e5e6-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="5e5e6-157">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-157">Low</span></span>      |
| [<span data-ttu-id="5e5e6-158">Os campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="5e5e6-159">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-159">Low</span></span>      |
| [<span data-ttu-id="5e5e6-160">Gerar se vários campos de suporte compatíveis são encontrados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="5e5e6-161">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-161">Low</span></span>      |
| [<span data-ttu-id="5e5e6-162">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="5e5e6-163">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-163">Low</span></span>      |
| [<span data-ttu-id="5e5e6-164">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="5e5e6-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="5e5e6-165">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-165">Low</span></span>      |
| [<span data-ttu-id="5e5e6-166">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="5e5e6-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="5e5e6-167">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-167">Low</span></span>      |
| [<span data-ttu-id="5e5e6-168">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="5e5e6-169">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-169">Low</span></span>      |
| [<span data-ttu-id="5e5e6-170">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="5e5e6-171">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-171">Low</span></span>      |
| [<span data-ttu-id="5e5e6-172">Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="5e5e6-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="5e5e6-173">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-173">Low</span></span>      |
| [<span data-ttu-id="5e5e6-174">Agora, a criação excessiva de provedores de serviço internos é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="5e5e6-175">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-175">Low</span></span>      |
| [<span data-ttu-id="5e5e6-176">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5e5e6-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="5e5e6-177">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-177">Low</span></span>      |
| [<span data-ttu-id="5e5e6-178">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="5e5e6-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="5e5e6-179">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-179">Low</span></span>      |
| [<span data-ttu-id="5e5e6-180">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5e5e6-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="5e5e6-181">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-181">Low</span></span>      |
| [<span data-ttu-id="5e5e6-182">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="5e5e6-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="5e5e6-183">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-183">Low</span></span>      |
| [<span data-ttu-id="5e5e6-184">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="5e5e6-185">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-185">Low</span></span>      |
| [<span data-ttu-id="5e5e6-186">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="5e5e6-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="5e5e6-187">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-187">Low</span></span>      |
| [<span data-ttu-id="5e5e6-188">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="5e5e6-189">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-189">Low</span></span>      |
| [<span data-ttu-id="5e5e6-190">Os valores Char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="5e5e6-191">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-191">Low</span></span>      |
| [<span data-ttu-id="5e5e6-192">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="5e5e6-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="5e5e6-193">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-193">Low</span></span>      |
| [<span data-ttu-id="5e5e6-194">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="5e5e6-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="5e5e6-195">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-195">Low</span></span>      |
| [<span data-ttu-id="5e5e6-196">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="5e5e6-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="5e5e6-197">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-197">Low</span></span>      |
| [<span data-ttu-id="5e5e6-198">Esclarecer a API para nomes da restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="5e5e6-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="5e5e6-199">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-199">Low</span></span>      |
| [<span data-ttu-id="5e5e6-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos</span><span class="sxs-lookup"><span data-stu-id="5e5e6-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="5e5e6-201">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-201">Low</span></span>      |
| [<span data-ttu-id="5e5e6-202">Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="5e5e6-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="5e5e6-203">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-203">Low</span></span>      |
| [<span data-ttu-id="5e5e6-204">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="5e5e6-205">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-205">Low</span></span>      |
| [<span data-ttu-id="5e5e6-206">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="5e5e6-207">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-207">Low</span></span>      |
| [<span data-ttu-id="5e5e6-208">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="5e5e6-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="5e5e6-209">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-209">Low</span></span>      |
| [<span data-ttu-id="5e5e6-210">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="5e5e6-211">Baixa</span><span class="sxs-lookup"><span data-stu-id="5e5e6-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="5e5e6-212">Consultas LINQ não são mais avaliadas no cliente</span><span class="sxs-lookup"><span data-stu-id="5e5e6-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="5e5e6-213">[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="5e5e6-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="5e5e6-214">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-214">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-215">Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="5e5e6-216">Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="5e5e6-217">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-217">**New behavior**</span></span>

<span data-ttu-id="5e5e6-218">A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="5e5e6-219">Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="5e5e6-220">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-220">**Why**</span></span>

<span data-ttu-id="5e5e6-221">A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="5e5e6-222">Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="5e5e6-223">Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="5e5e6-224">Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="5e5e6-225">Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="5e5e6-226">Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="5e5e6-227">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-227">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-228">Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="5e5e6-229">O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="5e5e6-230">Problema de acompanhamento nº 15498</span><span class="sxs-lookup"><span data-stu-id="5e5e6-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="5e5e6-231">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-231">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-232">Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="5e5e6-233">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-233">**New behavior**</span></span>

<span data-ttu-id="5e5e6-234">No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="5e5e6-235">Isso não inclui o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="5e5e6-236">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-236">**Why**</span></span>

<span data-ttu-id="5e5e6-237">Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="5e5e6-238">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-238">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-239">Considere a possibilidade de migrar para uma plataforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="5e5e6-240">Se isso não for possível, continue usando o EF Core 2.1 ou o EF Core 2.2, que dão suporte ao .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="5e5e6-241">O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e5e6-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="5e5e6-242">Anúncios de acompanhamento de problema nº 325</span><span class="sxs-lookup"><span data-stu-id="5e5e6-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="5e5e6-243">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-243">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-244">Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="5e5e6-245">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-245">**New behavior**</span></span>

<span data-ttu-id="5e5e6-246">A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="5e5e6-247">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-247">**Why**</span></span>

<span data-ttu-id="5e5e6-248">Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="5e5e6-249">Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="5e5e6-250">Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="5e5e6-251">Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="5e5e6-252">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-252">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-253">Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="5e5e6-254">A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core</span><span class="sxs-lookup"><span data-stu-id="5e5e6-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="5e5e6-255">Acompanhamento de problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="5e5e6-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="5e5e6-256">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-256">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-257">Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="5e5e6-258">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-258">**New behavior**</span></span>

<span data-ttu-id="5e5e6-259">Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="5e5e6-260">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-260">**Why**</span></span>

<span data-ttu-id="5e5e6-261">Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="5e5e6-262">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-262">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-263">Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="5e5e6-264">Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="5e5e6-265">FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="5e5e6-266">Acompanhamento de questões nº 10996</span><span class="sxs-lookup"><span data-stu-id="5e5e6-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="5e5e6-267">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-267">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-268">Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="5e5e6-269">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-269">**New behavior**</span></span>

<span data-ttu-id="5e5e6-270">Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="5e5e6-271">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="5e5e6-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="5e5e6-273">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="5e5e6-274">Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="5e5e6-275">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-275">**Why**</span></span>

<span data-ttu-id="5e5e6-276">Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="5e5e6-277">Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="5e5e6-278">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-278">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-279">Mude para usar os novos nomes de método.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="5e5e6-280">O método das quando usado com o procedimento armazenado não pode ser composto</span><span class="sxs-lookup"><span data-stu-id="5e5e6-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="5e5e6-281">Acompanhamento do problema #15392</span><span class="sxs-lookup"><span data-stu-id="5e5e6-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="5e5e6-282">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-282">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-283">Antes EF Core 3,0, o método das tentou detectar se o SQL passado pode ser composto.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="5e5e6-284">Ele fez a avaliação do cliente quando o SQL era não combinável como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="5e5e6-285">A consulta a seguir funcionou executando o procedimento armazenado no servidor e fazendo FirstOrDefault no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="5e5e6-286">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-286">**New behavior**</span></span>

<span data-ttu-id="5e5e6-287">A partir do EF Core 3,0, EF Core não tentará analisar o SQL.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="5e5e6-288">Portanto, se você estiver compondo após FromSqlRaw/FromSqlInterpolated, EF Core comporá o SQL fazendo a subconsulta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="5e5e6-289">Portanto, se você estiver usando um procedimento armazenado com composição, receberá uma exceção de sintaxe SQL inválida.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="5e5e6-290">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-290">**Why**</span></span>

<span data-ttu-id="5e5e6-291">O EF Core 3,0 não dá suporte à avaliação automática do cliente, pois foi propenso a erros, conforme explicado [aqui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="5e5e6-292">**Atenuação**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-292">**Mitigation**</span></span>

<span data-ttu-id="5e5e6-293">Se você estiver usando um procedimento armazenado em FromSqlRaw/FromSqlInterpolated, saberá que ele não pode ser composto, para que você possa adicionar __AsEnumerable/AsAsyncEnumerable__ logo após a chamada do método das para evitar qualquer composição no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="5e5e6-294">Os métodos FromSql só podem ser especificados em raízes de consulta</span><span class="sxs-lookup"><span data-stu-id="5e5e6-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="5e5e6-295">Acompanhamento de problema nº 15704</span><span class="sxs-lookup"><span data-stu-id="5e5e6-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="5e5e6-296">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-296">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-297">Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="5e5e6-298">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-298">**New behavior**</span></span>

<span data-ttu-id="5e5e6-299">A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="5e5e6-300">A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="5e5e6-301">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-301">**Why**</span></span>

<span data-ttu-id="5e5e6-302">Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="5e5e6-303">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-303">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-304">As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="5e5e6-305">As consultas sem acompanhamento não executam mais a resolução de identidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="5e5e6-306">Problema de acompanhamento nº 13518</span><span class="sxs-lookup"><span data-stu-id="5e5e6-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="5e5e6-307">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-307">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-308">Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="5e5e6-309">Isso corresponde ao comportamento das consultas de acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="5e5e6-310">Por exemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="5e5e6-311">retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="5e5e6-312">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-312">**New behavior**</span></span>

<span data-ttu-id="5e5e6-313">No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="5e5e6-314">Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="5e5e6-315">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-315">**Why**</span></span>

<span data-ttu-id="5e5e6-316">A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="5e5e6-317">Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="5e5e6-318">Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="5e5e6-319">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-319">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-320">Use uma consulta de acompanhamento se a resolução de identidade for necessária.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="5e5e6-321">~~A execução de consulta é registrada no nível da Depuração~~ Revertida</span><span class="sxs-lookup"><span data-stu-id="5e5e6-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="5e5e6-322">Acompanhamento de problema nº 14523</span><span class="sxs-lookup"><span data-stu-id="5e5e6-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="5e5e6-323">Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="5e5e6-324">Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="5e5e6-325">Valores de chave temporários não estão definidos em instâncias de entidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="5e5e6-326">Acompanhamento de problema nº 12378</span><span class="sxs-lookup"><span data-stu-id="5e5e6-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="5e5e6-327">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-327">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-328">Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="5e5e6-329">Normalmente, esses valores temporários eram grandes números negativos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="5e5e6-330">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-330">**New behavior**</span></span>

<span data-ttu-id="5e5e6-331">A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="5e5e6-332">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-332">**Why**</span></span>

<span data-ttu-id="5e5e6-333">Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="5e5e6-334">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-334">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-335">Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="5e5e6-336">Isso pode ser evitado:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-336">This can be avoided by:</span></span>
* <span data-ttu-id="5e5e6-337">Não usando chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="5e5e6-338">Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="5e5e6-339">Obter os valores de chave temporários reais de informações de acompanhamento de entidade.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="5e5e6-340">Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="5e5e6-341">DetectChanges respeita os valores de chave gerados pelo repositório</span><span class="sxs-lookup"><span data-stu-id="5e5e6-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="5e5e6-342">Acompanhamento de problema nº 14616</span><span class="sxs-lookup"><span data-stu-id="5e5e6-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="5e5e6-343">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-343">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-344">Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="5e5e6-345">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-345">**New behavior**</span></span>

<span data-ttu-id="5e5e6-346">A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="5e5e6-347">Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="5e5e6-348">Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="5e5e6-349">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-349">**Why**</span></span>

<span data-ttu-id="5e5e6-350">Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="5e5e6-351">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-351">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-352">Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="5e5e6-353">A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="5e5e6-354">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="5e5e6-355">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="5e5e6-356">Exclusões em cascata acontecerão imediatamente por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="5e5e6-357">Acompanhamento de problema nº 10114</span><span class="sxs-lookup"><span data-stu-id="5e5e6-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="5e5e6-358">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-358">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-359">Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="5e5e6-360">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-360">**New behavior**</span></span>

<span data-ttu-id="5e5e6-361">A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="5e5e6-362">Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="5e5e6-363">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-363">**Why**</span></span>

<span data-ttu-id="5e5e6-364">Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="5e5e6-365">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-365">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-366">O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="5e5e6-367">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="5e5e6-368">O carregamento adiantado de entidades relacionadas agora ocorre em uma única consulta</span><span class="sxs-lookup"><span data-stu-id="5e5e6-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="5e5e6-369">Acompanhamento do problema #18022</span><span class="sxs-lookup"><span data-stu-id="5e5e6-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="5e5e6-370">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-370">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-371">Antes de 3,0, carregar cuidadosamente as navegações de coleção por meio de operadores `Include` fizeram com que várias consultas sejam geradas no banco de dados relacional, uma para cada tipo de entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="5e5e6-372">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-372">**New behavior**</span></span>

<span data-ttu-id="5e5e6-373">A partir do 3,0, o EF Core gera uma única consulta com junções em bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="5e5e6-374">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-374">**Why**</span></span>

<span data-ttu-id="5e5e6-375">A emissão de várias consultas para implementar uma única consulta LINQ causou vários problemas, incluindo o desempenho negativo à medida que várias viagens de ida e volta do banco de dados eram necessárias e os problemas de coerência do dado, uma vez que cada consulta poderia observar um estado diferente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="5e5e6-376">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-376">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-377">Embora tecnicamente essa não seja uma alteração significativa, ela pode ter um efeito considerável no desempenho do aplicativo quando uma única consulta contém um grande número de operador `Include` em navegações de coleção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="5e5e6-378">[Consulte este comentário](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-537219137) para obter mais informações e reescrever consultas de maneira mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-537219137) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="5e5e6-379">DeleteBehavior.Restrict tem uma semântica de limpeza</span><span class="sxs-lookup"><span data-stu-id="5e5e6-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="5e5e6-380">Acompanhamento de questões nº 12661</span><span class="sxs-lookup"><span data-stu-id="5e5e6-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="5e5e6-381">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-381">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-382">Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="5e5e6-383">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-383">**New behavior**</span></span>

<span data-ttu-id="5e5e6-384">No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="5e5e6-385">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-385">**Why**</span></span>

<span data-ttu-id="5e5e6-386">Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="5e5e6-387">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-387">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-388">O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="5e5e6-389">Tipos de consulta são consolidados com tipos de entidade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="5e5e6-390">Acompanhamento de problema nº 14194</span><span class="sxs-lookup"><span data-stu-id="5e5e6-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="5e5e6-391">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-391">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-392">Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/keyless-entity-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="5e5e6-393">Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="5e5e6-394">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-394">**New behavior**</span></span>

<span data-ttu-id="5e5e6-395">Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="5e5e6-396">Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="5e5e6-397">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-397">**Why**</span></span>

<span data-ttu-id="5e5e6-398">Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="5e5e6-399">Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="5e5e6-400">Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="5e5e6-401">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-401">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-402">As seguintes partes da API agora estão obsoletas:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="5e5e6-403">**`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="5e5e6-404">Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="5e5e6-405">**`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="5e5e6-406">**`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="5e5e6-407">A API de configuração para relações de tipo de propriedade mudou</span><span class="sxs-lookup"><span data-stu-id="5e5e6-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="5e5e6-408">[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="5e5e6-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="5e5e6-409">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-409">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-410">Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="5e5e6-411">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-411">**New behavior**</span></span>

<span data-ttu-id="5e5e6-412">A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="5e5e6-413">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="5e5e6-414">A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="5e5e6-415">No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="5e5e6-416">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-416">For example:</span></span>

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

<span data-ttu-id="5e5e6-417">Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="5e5e6-418">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-418">**Why**</span></span>

<span data-ttu-id="5e5e6-419">Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="5e5e6-420">Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="5e5e6-421">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-421">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-422">Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="5e5e6-423">As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais</span><span class="sxs-lookup"><span data-stu-id="5e5e6-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="5e5e6-424">Acompanhamento de questões nº 9005</span><span class="sxs-lookup"><span data-stu-id="5e5e6-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="5e5e6-425">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-425">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-426">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-426">Consider the following model:</span></span>
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
<span data-ttu-id="5e5e6-427">Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="5e5e6-428">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-428">**New behavior**</span></span>

<span data-ttu-id="5e5e6-429">Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="5e5e6-430">Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="5e5e6-431">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-431">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-432">Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="5e5e6-433">Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="5e5e6-434">Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade</span><span class="sxs-lookup"><span data-stu-id="5e5e6-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="5e5e6-435">Acompanhamento de questões nº 14154</span><span class="sxs-lookup"><span data-stu-id="5e5e6-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="5e5e6-436">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-436">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-437">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-437">Consider the following model:</span></span>
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
<span data-ttu-id="5e5e6-438">Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="5e5e6-439">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-439">**New behavior**</span></span>

<span data-ttu-id="5e5e6-440">Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="5e5e6-441">Caso contrário, uma exceção é gerada durante a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="5e5e6-442">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-442">**Why**</span></span>

<span data-ttu-id="5e5e6-443">Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="5e5e6-444">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-444">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-445">Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="5e5e6-446">É possível criar uma no estado de sombra:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="5e5e6-447">Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="5e5e6-448">Acompanhamento de questões nº 13998</span><span class="sxs-lookup"><span data-stu-id="5e5e6-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="5e5e6-449">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-449">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-450">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-450">Consider the following model:</span></span>
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

<span data-ttu-id="5e5e6-451">Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="5e5e6-452">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-452">**New behavior**</span></span>

<span data-ttu-id="5e5e6-453">Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="5e5e6-454">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-454">**Why**</span></span>

<span data-ttu-id="5e5e6-455">O antigo comportamento era inesperado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="5e5e6-456">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-456">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-457">A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="5e5e6-458">A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança</span><span class="sxs-lookup"><span data-stu-id="5e5e6-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="5e5e6-459">Acompanhamento de problema nº 13274</span><span class="sxs-lookup"><span data-stu-id="5e5e6-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="5e5e6-460">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-460">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-461">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-461">Consider the following model:</span></span>
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
<span data-ttu-id="5e5e6-462">Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="5e5e6-463">No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="5e5e6-464">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-464">**New behavior**</span></span>

<span data-ttu-id="5e5e6-465">Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="5e5e6-466">Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="5e5e6-467">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-467">For example:</span></span>

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

<span data-ttu-id="5e5e6-468">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-468">**Why**</span></span>

<span data-ttu-id="5e5e6-469">Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="5e5e6-470">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-470">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-471">Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="5e5e6-472">A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope</span><span class="sxs-lookup"><span data-stu-id="5e5e6-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="5e5e6-473">Acompanhamento de questões nº 14218</span><span class="sxs-lookup"><span data-stu-id="5e5e6-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="5e5e6-474">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-474">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-475">Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="5e5e6-476">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-476">**New behavior**</span></span>

<span data-ttu-id="5e5e6-477">Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="5e5e6-478">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-478">**Why**</span></span>

<span data-ttu-id="5e5e6-479">Essa alteração permite para usar vários contextos no mesmo `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="5e5e6-480">O novo comportamento também corresponde ao EF6.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="5e5e6-481">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-481">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-482">Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="5e5e6-483">Cada propriedade usa geração de chave de inteiro em memória independente</span><span class="sxs-lookup"><span data-stu-id="5e5e6-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="5e5e6-484">Acompanhamento de problema nº 6872</span><span class="sxs-lookup"><span data-stu-id="5e5e6-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="5e5e6-485">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-485">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-486">Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="5e5e6-487">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-487">**New behavior**</span></span>

<span data-ttu-id="5e5e6-488">Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="5e5e6-489">Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="5e5e6-490">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-490">**Why**</span></span>

<span data-ttu-id="5e5e6-491">Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="5e5e6-492">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-492">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-493">Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="5e5e6-494">Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="5e5e6-495">Campos de suporte são usados por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-495">Backing fields are used by default</span></span>

[<span data-ttu-id="5e5e6-496">Acompanhamento de problema nº 12430</span><span class="sxs-lookup"><span data-stu-id="5e5e6-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="5e5e6-497">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-497">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-498">Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="5e5e6-499">A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="5e5e6-500">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-500">**New behavior**</span></span>

<span data-ttu-id="5e5e6-501">Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="5e5e6-502">Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="5e5e6-503">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-503">**Why**</span></span>

<span data-ttu-id="5e5e6-504">Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="5e5e6-505">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-505">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-506">O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="5e5e6-507">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="5e5e6-508">Lançado se vários campos de suporte compatíveis forem encontrados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="5e5e6-509">Acompanhamento de problema nº 12523</span><span class="sxs-lookup"><span data-stu-id="5e5e6-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="5e5e6-510">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-510">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-511">Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="5e5e6-512">Isso pode fazer o campo errado ser usado em casos ambíguos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="5e5e6-513">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-513">**New behavior**</span></span>

<span data-ttu-id="5e5e6-514">A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="5e5e6-515">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-515">**Why**</span></span>

<span data-ttu-id="5e5e6-516">Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="5e5e6-517">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-517">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-518">As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="5e5e6-519">Por exemplo, usando a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="5e5e6-520">Os nomes de propriedade somente de campo devem corresponder ao nome de campo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="5e5e6-521">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-521">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-522">Antes de EF Core 3,0, uma propriedade poderia ser especificada por um valor de cadeia de caracteres e, se nenhuma propriedade com esse nome tiver sido encontrada no tipo .NET, EF Core tentará fazer a correspondência com um campo usando regras de convenção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="5e5e6-523">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-523">**New behavior**</span></span>

<span data-ttu-id="5e5e6-524">No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="5e5e6-525">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-525">**Why**</span></span>

<span data-ttu-id="5e5e6-526">Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="5e5e6-527">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-527">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-528">As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="5e5e6-529">Em uma versão futura do EF Core após 3,0, planejamos reabilitar explicitamente um nome de campo diferente do nome da propriedade (consulte o problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="5e5e6-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="5e5e6-530">AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="5e5e6-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="5e5e6-531">Acompanhamento de problema nº 14756</span><span class="sxs-lookup"><span data-stu-id="5e5e6-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="5e5e6-532">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-532">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-533">Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="5e5e6-534">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-534">**New behavior**</span></span>

<span data-ttu-id="5e5e6-535">A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="5e5e6-536">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-536">**Why**</span></span>

<span data-ttu-id="5e5e6-537">O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="5e5e6-538">No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="5e5e6-539">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-539">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-540">Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="5e5e6-541">DbContext.Entry agora executa uma DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="5e5e6-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="5e5e6-542">Acompanhamento de problema nº 13552</span><span class="sxs-lookup"><span data-stu-id="5e5e6-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="5e5e6-543">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-543">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-544">Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="5e5e6-545">Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="5e5e6-546">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-546">**New behavior**</span></span>

<span data-ttu-id="5e5e6-547">A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="5e5e6-548">Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="5e5e6-549">Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="5e5e6-550">Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="5e5e6-551">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-551">**Why**</span></span>

<span data-ttu-id="5e5e6-552">Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="5e5e6-553">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-553">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-554">Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="5e5e6-555">As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="5e5e6-556">Acompanhamento de problema nº 14617</span><span class="sxs-lookup"><span data-stu-id="5e5e6-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="5e5e6-557">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-557">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-558">Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="5e5e6-559">Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="5e5e6-560">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-560">**New behavior**</span></span>

<span data-ttu-id="5e5e6-561">A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="5e5e6-562">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-562">**Why**</span></span>

<span data-ttu-id="5e5e6-563">Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="5e5e6-564">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-564">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-565">O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="5e5e6-566">Por exemplo, com a API fluente:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="5e5e6-567">Ou com anotações de dados:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="5e5e6-568">ILoggerFactory agora é um serviço com escopo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="5e5e6-569">Acompanhamento de problema nº 14698</span><span class="sxs-lookup"><span data-stu-id="5e5e6-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="5e5e6-570">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-570">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-571">Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="5e5e6-572">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-572">**New behavior**</span></span>

<span data-ttu-id="5e5e6-573">A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="5e5e6-574">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-574">**Why**</span></span>

<span data-ttu-id="5e5e6-575">Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="5e5e6-576">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-576">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-577">Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="5e5e6-578">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-578">This isn't common.</span></span>
<span data-ttu-id="5e5e6-579">Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="5e5e6-580">Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="5e5e6-581">Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas</span><span class="sxs-lookup"><span data-stu-id="5e5e6-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="5e5e6-582">Acompanhamento de problema nº 12780</span><span class="sxs-lookup"><span data-stu-id="5e5e6-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="5e5e6-583">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-583">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-584">Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="5e5e6-585">Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="5e5e6-586">Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="5e5e6-587">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-587">**New behavior**</span></span>

<span data-ttu-id="5e5e6-588">A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="5e5e6-589">Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="5e5e6-590">Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="5e5e6-591">Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="5e5e6-592">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-592">**Why**</span></span>

<span data-ttu-id="5e5e6-593">Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="5e5e6-594">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-594">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-595">Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="5e5e6-596">Criação excessiva de provedores de serviço internos agora é um erro por padrão</span><span class="sxs-lookup"><span data-stu-id="5e5e6-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="5e5e6-597">Acompanhamento de problema nº 10236</span><span class="sxs-lookup"><span data-stu-id="5e5e6-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="5e5e6-598">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-598">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-599">Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="5e5e6-600">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-600">**New behavior**</span></span>

<span data-ttu-id="5e5e6-601">A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="5e5e6-602">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-602">**Why**</span></span>

<span data-ttu-id="5e5e6-603">Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="5e5e6-604">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-604">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-605">A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="5e5e6-606">No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="5e5e6-607">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="5e5e6-608">Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5e5e6-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="5e5e6-609">Acompanhamento de problema nº 9171</span><span class="sxs-lookup"><span data-stu-id="5e5e6-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="5e5e6-610">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-610">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-611">Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="5e5e6-612">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="5e5e6-613">O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="5e5e6-614">Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="5e5e6-615">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-615">**New behavior**</span></span>

<span data-ttu-id="5e5e6-616">A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="5e5e6-617">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-617">**Why**</span></span>

<span data-ttu-id="5e5e6-618">O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="5e5e6-619">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-619">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-620">Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="5e5e6-621">Isso não é comum.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-621">This is not common.</span></span>
<span data-ttu-id="5e5e6-622">O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="5e5e6-623">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="5e5e6-624">O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask</span><span class="sxs-lookup"><span data-stu-id="5e5e6-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="5e5e6-625">Acompanhamento de questões nº 15184</span><span class="sxs-lookup"><span data-stu-id="5e5e6-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="5e5e6-626">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-626">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-627">Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="5e5e6-628">`ValueGenerator.NextValueAsync()` (e classes derivadas)</span><span class="sxs-lookup"><span data-stu-id="5e5e6-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="5e5e6-629">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-629">**New behavior**</span></span>

<span data-ttu-id="5e5e6-630">Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="5e5e6-631">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-631">**Why**</span></span>

<span data-ttu-id="5e5e6-632">Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="5e5e6-633">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-633">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-634">Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="5e5e6-635">Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="5e5e6-636">Observe que isso anula a redução de alocação feita por essa alteração.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="5e5e6-637">A anotação Relational:TypeMapping agora é apenas TypeMapping</span><span class="sxs-lookup"><span data-stu-id="5e5e6-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="5e5e6-638">Acompanhamento de problema nº 9913</span><span class="sxs-lookup"><span data-stu-id="5e5e6-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="5e5e6-639">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-639">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-640">O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5e5e6-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="5e5e6-641">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-641">**New behavior**</span></span>

<span data-ttu-id="5e5e6-642">O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="5e5e6-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="5e5e6-643">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-643">**Why**</span></span>

<span data-ttu-id="5e5e6-644">Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="5e5e6-645">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-645">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-646">Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="5e5e6-647">A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="5e5e6-648">ToTable em um tipo derivado gera uma exceção</span><span class="sxs-lookup"><span data-stu-id="5e5e6-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="5e5e6-649">Acompanhamento de problema nº 11811</span><span class="sxs-lookup"><span data-stu-id="5e5e6-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="5e5e6-650">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-650">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-651">Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="5e5e6-652">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-652">**New behavior**</span></span>

<span data-ttu-id="5e5e6-653">A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="5e5e6-654">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-654">**Why**</span></span>

<span data-ttu-id="5e5e6-655">Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="5e5e6-656">Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="5e5e6-657">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-657">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-658">Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="5e5e6-659">ForSqlServerHasIndex substituído por HasIndex</span><span class="sxs-lookup"><span data-stu-id="5e5e6-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="5e5e6-660">Acompanhamento de problema nº 12366</span><span class="sxs-lookup"><span data-stu-id="5e5e6-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="5e5e6-661">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-661">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-662">Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="5e5e6-663">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-663">**New behavior**</span></span>

<span data-ttu-id="5e5e6-664">A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="5e5e6-665">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="5e5e6-666">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-666">**Why**</span></span>

<span data-ttu-id="5e5e6-667">Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="5e5e6-668">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-668">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-669">Use a nova API, conforme mostrado acima.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="5e5e6-670">Alterações na API de metadados</span><span class="sxs-lookup"><span data-stu-id="5e5e6-670">Metadata API changes</span></span>

[<span data-ttu-id="5e5e6-671">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="5e5e6-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5e5e6-672">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-672">**New behavior**</span></span>

<span data-ttu-id="5e5e6-673">As seguintes propriedades foram convertidas em métodos de extensão:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="5e5e6-674">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-674">**Why**</span></span>

<span data-ttu-id="5e5e6-675">Essa alteração simplifica a implementação das interfaces mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="5e5e6-676">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-676">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-677">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="5e5e6-678">Alterações na API de metadados específicos do provedor</span><span class="sxs-lookup"><span data-stu-id="5e5e6-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="5e5e6-679">Acompanhamento de questões nº 214</span><span class="sxs-lookup"><span data-stu-id="5e5e6-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="5e5e6-680">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-680">**New behavior**</span></span>

<span data-ttu-id="5e5e6-681">Os métodos de extensão específicos do provedor serão nivelados:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="5e5e6-682">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-682">**Why**</span></span>

<span data-ttu-id="5e5e6-683">Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="5e5e6-684">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-684">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-685">Use os novos métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="5e5e6-686">O EF Core não envia mais pragma para imposição do FK SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="5e5e6-687">Acompanhamento de problema nº 12151</span><span class="sxs-lookup"><span data-stu-id="5e5e6-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="5e5e6-688">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-688">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-689">Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5e5e6-690">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-690">**New behavior**</span></span>

<span data-ttu-id="5e5e6-691">A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="5e5e6-692">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-692">**Why**</span></span>

<span data-ttu-id="5e5e6-693">Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="5e5e6-694">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-694">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-695">Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="5e5e6-696">Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="5e5e6-697">Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="5e5e6-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="5e5e6-698">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-698">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-699">Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="5e5e6-700">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-700">**New behavior**</span></span>

<span data-ttu-id="5e5e6-701">A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="5e5e6-702">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-702">**Why**</span></span>

<span data-ttu-id="5e5e6-703">Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="5e5e6-704">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-704">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-705">Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5e5e6-706">Os valores de Guid agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5e5e6-707">Acompanhamento de problema nº 15078</span><span class="sxs-lookup"><span data-stu-id="5e5e6-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="5e5e6-708">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-708">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-709">Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="5e5e6-710">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-710">**New behavior**</span></span>

<span data-ttu-id="5e5e6-711">Agora os valores de Guid são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="5e5e6-712">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-712">**Why**</span></span>

<span data-ttu-id="5e5e6-713">O formato binário de Guids não é padronizado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="5e5e6-714">O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5e5e6-715">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-715">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-716">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="5e5e6-717">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="5e5e6-718">O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="5e5e6-719">Os valores de char agora são armazenados como TEXTO no SQLite</span><span class="sxs-lookup"><span data-stu-id="5e5e6-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="5e5e6-720">Problema de acompanhamento nº 15020</span><span class="sxs-lookup"><span data-stu-id="5e5e6-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="5e5e6-721">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-721">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-722">Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="5e5e6-723">Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="5e5e6-724">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-724">**New behavior**</span></span>

<span data-ttu-id="5e5e6-725">Agora, os valores de char são armazenados como TEXTO.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="5e5e6-726">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-726">**Why**</span></span>

<span data-ttu-id="5e5e6-727">O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="5e5e6-728">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-728">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-729">Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="5e5e6-730">No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="5e5e6-731">O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="5e5e6-732">As IDs de migração agora são geradas usando o calendário da cultura invariável</span><span class="sxs-lookup"><span data-stu-id="5e5e6-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="5e5e6-733">Problema de acompanhamento nº 12978</span><span class="sxs-lookup"><span data-stu-id="5e5e6-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="5e5e6-734">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-734">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-735">As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="5e5e6-736">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-736">**New behavior**</span></span>

<span data-ttu-id="5e5e6-737">Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="5e5e6-738">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-738">**Why**</span></span>

<span data-ttu-id="5e5e6-739">A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="5e5e6-740">O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="5e5e6-741">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-741">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-742">Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês).</span><span class="sxs-lookup"><span data-stu-id="5e5e6-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="5e5e6-743">As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="5e5e6-744">A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="5e5e6-745">A tabela de histórico de Migrações também precisa ser atualizada.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="5e5e6-746">UseRowNumberForPaging foi removido</span><span class="sxs-lookup"><span data-stu-id="5e5e6-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="5e5e6-747">Acompanhamento de problema nº 16400</span><span class="sxs-lookup"><span data-stu-id="5e5e6-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="5e5e6-748">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-748">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-749">Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="5e5e6-750">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-750">**New behavior**</span></span>

<span data-ttu-id="5e5e6-751">A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="5e5e6-752">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-752">**Why**</span></span>

<span data-ttu-id="5e5e6-753">Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="5e5e6-754">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-754">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-755">É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="5e5e6-756">Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="5e5e6-757">Poderemos rever essa decisão com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="5e5e6-758">As informações de extensão/metadados foram removidas do IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="5e5e6-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="5e5e6-759">Acompanhamento de problema nº 16119</span><span class="sxs-lookup"><span data-stu-id="5e5e6-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="5e5e6-760">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-760">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-761">`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="5e5e6-762">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-762">**New behavior**</span></span>

<span data-ttu-id="5e5e6-763">Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="5e5e6-764">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-764">**Why**</span></span>

<span data-ttu-id="5e5e6-765">Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="5e5e6-766">Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="5e5e6-767">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-767">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-768">Atualize as extensões para que sigam o novo padrão.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="5e5e6-769">Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="5e5e6-770">LogQueryPossibleExceptionWithAggregateOperator foi renomeado</span><span class="sxs-lookup"><span data-stu-id="5e5e6-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="5e5e6-771">Acompanhamento de problema nº 10985</span><span class="sxs-lookup"><span data-stu-id="5e5e6-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="5e5e6-772">**Alteração**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-772">**Change**</span></span>

<span data-ttu-id="5e5e6-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="5e5e6-774">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-774">**Why**</span></span>

<span data-ttu-id="5e5e6-775">Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="5e5e6-776">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-776">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-777">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-777">Use the new name.</span></span> <span data-ttu-id="5e5e6-778">(Observe que o número da ID do evento não foi alterado.)</span><span class="sxs-lookup"><span data-stu-id="5e5e6-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="5e5e6-779">Esclarecer a API para nomes de restrição de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="5e5e6-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="5e5e6-780">Acompanhamento de problema nº 10730</span><span class="sxs-lookup"><span data-stu-id="5e5e6-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="5e5e6-781">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-781">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-782">Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome".</span><span class="sxs-lookup"><span data-stu-id="5e5e6-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="5e5e6-783">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="5e5e6-784">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-784">**New behavior**</span></span>

<span data-ttu-id="5e5e6-785">A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição".</span><span class="sxs-lookup"><span data-stu-id="5e5e6-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="5e5e6-786">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="5e5e6-787">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-787">**Why**</span></span>

<span data-ttu-id="5e5e6-788">Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="5e5e6-789">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-789">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-790">Use o novo nome.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="5e5e6-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos</span><span class="sxs-lookup"><span data-stu-id="5e5e6-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="5e5e6-792">Acompanhamento de problema nº 15997</span><span class="sxs-lookup"><span data-stu-id="5e5e6-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="5e5e6-793">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-793">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-794">Antes do EF Core 3.0, esses métodos eram protegidos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="5e5e6-795">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-795">**New behavior**</span></span>

<span data-ttu-id="5e5e6-796">A partir do EF Core 3.0, esses métodos são públicos.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="5e5e6-797">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-797">**Why**</span></span>

<span data-ttu-id="5e5e6-798">Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="5e5e6-799">Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="5e5e6-800">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-800">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-801">Altere a acessibilidade de todas as substituições.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="5e5e6-802">Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="5e5e6-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="5e5e6-803">Acompanhamento de problema nº 11506</span><span class="sxs-lookup"><span data-stu-id="5e5e6-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="5e5e6-804">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-804">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-805">Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="5e5e6-806">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-806">**New behavior**</span></span>

<span data-ttu-id="5e5e6-807">A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="5e5e6-808">Isso significa que a dependência não fluirá transitivamente para outros projetos e que você não poderá mais, por padrão, referenciar o assembly dele.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="5e5e6-809">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-809">**Why**</span></span>

<span data-ttu-id="5e5e6-810">Esse pacote destina-se para uso somente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="5e5e6-811">Os aplicativos implantados não devem fazer referência a ele.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="5e5e6-812">Tornar o pacote um DevelopmentDependency reforça essa recomendação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="5e5e6-813">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-813">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-814">Se você precisar fazer referência a esse pacote para substituir o comportamento de tempo de design do EF Core, poderá atualizar os metadados do item PackageReference no seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="5e5e6-815">Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="5e5e6-816">SQLitePCL.raw atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="5e5e6-817">Acompanhamento de problema nº 14824</span><span class="sxs-lookup"><span data-stu-id="5e5e6-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="5e5e6-818">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-818">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-819">Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="5e5e6-820">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-820">**New behavior**</span></span>

<span data-ttu-id="5e5e6-821">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5e5e6-822">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-822">**Why**</span></span>

<span data-ttu-id="5e5e6-823">A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="5e5e6-824">Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="5e5e6-825">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-825">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-826">O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5e5e6-827">Confira as [notas sobre a versão](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="5e5e6-828">NetTopologySuite atualizado para a versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5e5e6-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="5e5e6-829">Acompanhamento do problema n.º 14825</span><span class="sxs-lookup"><span data-stu-id="5e5e6-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="5e5e6-830">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-830">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-831">Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="5e5e6-832">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-832">**New behavior**</span></span>

<span data-ttu-id="5e5e6-833">Atualizamos nosso pacote para depender da versão 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="5e5e6-834">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-834">**Why**</span></span>

<span data-ttu-id="5e5e6-835">O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="5e5e6-836">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-836">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-837">O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="5e5e6-838">Confira as [notas sobre a versão](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="5e5e6-839">Várias relações ambíguas de autorreferência devem ser configuradas</span><span class="sxs-lookup"><span data-stu-id="5e5e6-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="5e5e6-840">Acompanhamento de Problema nº 13573</span><span class="sxs-lookup"><span data-stu-id="5e5e6-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="5e5e6-841">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-841">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-842">Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="5e5e6-843">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-843">For example:</span></span>

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

<span data-ttu-id="5e5e6-844">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-844">**New behavior**</span></span>

<span data-ttu-id="5e5e6-845">Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="5e5e6-846">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-846">**Why**</span></span>

<span data-ttu-id="5e5e6-847">O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="5e5e6-848">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-848">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-849">Use a configuração completa da relação.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="5e5e6-850">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5e5e6-850">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="5e5e6-851">DbFunction. Schema sendo nulo ou a cadeia de caracteres vazia o configura para estar no esquema padrão do modelo</span><span class="sxs-lookup"><span data-stu-id="5e5e6-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="5e5e6-852">Acompanhamento do problema #12757</span><span class="sxs-lookup"><span data-stu-id="5e5e6-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="5e5e6-853">**Comportamento antigo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-853">**Old behavior**</span></span>

<span data-ttu-id="5e5e6-854">Um DbFunction configurado com o esquema como uma cadeia de caracteres vazia foi tratado como função interna sem um esquema.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="5e5e6-855">Por exemplo, o código a `DatePart` seguir mapeará `DATEPART` a função CLR para a função interna no SqlServer.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="5e5e6-856">**Comportamento novo**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-856">**New behavior**</span></span>

<span data-ttu-id="5e5e6-857">Todos os mapeamentos DbFunction são considerados mapeados para funções definidas pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="5e5e6-858">Portanto, o valor da cadeia de caracteres vazia colocaria a função dentro do esquema padrão para o modelo.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="5e5e6-859">Que poderia ser o esquema configurado explicitamente por meio da `modelBuilder.HasDefaultSchema()` API `dbo` Fluent ou de outra forma.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="5e5e6-860">**Por que**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-860">**Why**</span></span>

<span data-ttu-id="5e5e6-861">O esquema anterior sendo vazio era uma maneira de tratar que a função é interna, mas essa lógica só é aplicável ao SqlServer, em que funções internas não pertencem a nenhum esquema.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="5e5e6-862">**Mitigações**</span><span class="sxs-lookup"><span data-stu-id="5e5e6-862">**Mitigations**</span></span>

<span data-ttu-id="5e5e6-863">Configure a tradução do DbFunction manualmente para mapeá-lo para uma função interna.</span><span class="sxs-lookup"><span data-stu-id="5e5e6-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
