---
title: Novidades no EF Core 1.0 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417520"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="28046-102">Recursos incluídos no EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="28046-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="28046-103">Plataformas</span><span class="sxs-lookup"><span data-stu-id="28046-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="28046-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="28046-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="28046-105">Inclui Console, WPF, WinForms, ASP.NET 4 etc.</span><span class="sxs-lookup"><span data-stu-id="28046-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="28046-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="28046-106">.NET Standard 1.3</span></span>

<span data-ttu-id="28046-107">Incluindo o ASP.NET Core direcionado para o .NET Framework e .NET Core no Windows, OSX e Linux.</span><span class="sxs-lookup"><span data-stu-id="28046-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="28046-108">Modelagem</span><span class="sxs-lookup"><span data-stu-id="28046-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="28046-109">Modelagem básica</span><span class="sxs-lookup"><span data-stu-id="28046-109">Basic modelling</span></span>

<span data-ttu-id="28046-110">Com base em entidades POCO com propriedades get/set de tipos escalares comuns (`int`, `string` etc.).</span><span class="sxs-lookup"><span data-stu-id="28046-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="28046-111">Relações e propriedades de navegação</span><span class="sxs-lookup"><span data-stu-id="28046-111">Relationships and navigation properties</span></span>

<span data-ttu-id="28046-112">As relações de um para muitos e um para zero ou um podem ser especificadas no modelo com base em uma chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="28046-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="28046-113">É possível associar propriedades de navegação de coleção simples ou tipos de referência a essas relações.</span><span class="sxs-lookup"><span data-stu-id="28046-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="28046-114">Convenções internas</span><span class="sxs-lookup"><span data-stu-id="28046-114">Built-in conventions</span></span>

<span data-ttu-id="28046-115">Elas compilam um modelo inicial com base na forma das classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="28046-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="28046-116">API fluente</span><span class="sxs-lookup"><span data-stu-id="28046-116">Fluent API</span></span>

<span data-ttu-id="28046-117">Permite que você substitua o método `OnModelCreating` em seu contexto para configurar ainda mais o modelo que foi descoberto por convenção.</span><span class="sxs-lookup"><span data-stu-id="28046-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="28046-118">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="28046-118">Data annotations</span></span>

<span data-ttu-id="28046-119">São atributos que podem ser adicionados às suas propriedades/classes de entidade e influenciarão o modelo do EF.</span><span class="sxs-lookup"><span data-stu-id="28046-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="28046-120">Por exemplo, adicionar `[Required]` permitirá que o EF saiba que uma propriedade é necessária.</span><span class="sxs-lookup"><span data-stu-id="28046-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="28046-121">Mapeamento de tabela relacional</span><span class="sxs-lookup"><span data-stu-id="28046-121">Relational Table mapping</span></span>

<span data-ttu-id="28046-122">Permite o mapeamento de entidades para tabelas/colunas.</span><span class="sxs-lookup"><span data-stu-id="28046-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="28046-123">Geração de valor de chave</span><span class="sxs-lookup"><span data-stu-id="28046-123">Key value generation</span></span>

<span data-ttu-id="28046-124">Incluindo geração do lado do cliente e a geração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="28046-125">Valores gerados no banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-125">Database generated values</span></span>

<span data-ttu-id="28046-126">Permite a geração de valores pelo banco de dados na inserção (valores padrão) ou atualização (colunas computadas).</span><span class="sxs-lookup"><span data-stu-id="28046-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="28046-127">Sequências no SQL Server</span><span class="sxs-lookup"><span data-stu-id="28046-127">Sequences in SQL Server</span></span>

<span data-ttu-id="28046-128">Permite a definição de objetos de sequência no modelo.</span><span class="sxs-lookup"><span data-stu-id="28046-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="28046-129">Restrições únicas</span><span class="sxs-lookup"><span data-stu-id="28046-129">Unique constraints</span></span>

<span data-ttu-id="28046-130">Permite a definição de chaves alternativas e a capacidade de definir relações direcionadas para essa chave.</span><span class="sxs-lookup"><span data-stu-id="28046-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="28046-131">Índices</span><span class="sxs-lookup"><span data-stu-id="28046-131">Indexes</span></span>

<span data-ttu-id="28046-132">A definição de índices no modelo introduz automaticamente índices no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="28046-133">Também há suporte para índices exclusivos.</span><span class="sxs-lookup"><span data-stu-id="28046-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="28046-134">Propriedades de estado sombra</span><span class="sxs-lookup"><span data-stu-id="28046-134">Shadow state properties</span></span>

<span data-ttu-id="28046-135">Permite a definição de propriedades no modelo que não são declaradas e não são armazenadas na classe do .NET, mas podem ser controladas e atualizadas pelo EF Core.</span><span class="sxs-lookup"><span data-stu-id="28046-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="28046-136">Normalmente usado para propriedades de chave estrangeira, quando a exposição delas no objeto não for desejada.</span><span class="sxs-lookup"><span data-stu-id="28046-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="28046-137">Padrão de herança de tabela por hierarquia</span><span class="sxs-lookup"><span data-stu-id="28046-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="28046-138">Permite a gravação de entidades em uma hierarquia de herança em uma única tabela usando uma coluna discriminadora para identificar o tipo de entidade para um determinado registro no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="28046-139">Validação de modelo</span><span class="sxs-lookup"><span data-stu-id="28046-139">Model validation</span></span>

<span data-ttu-id="28046-140">Detecta padrões inválidos no modelo e fornece mensagens de erro úteis.</span><span class="sxs-lookup"><span data-stu-id="28046-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="28046-141">controle de alterações</span><span class="sxs-lookup"><span data-stu-id="28046-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="28046-142">Controle de alterações de instantâneo</span><span class="sxs-lookup"><span data-stu-id="28046-142">Snapshot change tracking</span></span>

<span data-ttu-id="28046-143">Permite a detecção automática de alterações em entidades por meio da comparação do estado atual com uma cópia (instantâneo) do estado original.</span><span class="sxs-lookup"><span data-stu-id="28046-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="28046-144">Controle de alterações de notificação</span><span class="sxs-lookup"><span data-stu-id="28046-144">Notification change tracking</span></span>

<span data-ttu-id="28046-145">Permite que as entidades notifiquem o controlador de alteração quando houver alteração nos valores de propriedade.</span><span class="sxs-lookup"><span data-stu-id="28046-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="28046-146">Acesso ao estado controlado</span><span class="sxs-lookup"><span data-stu-id="28046-146">Accessing tracked state</span></span>

<span data-ttu-id="28046-147">Via `DbContext.Entry` e `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="28046-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="28046-148">Anexar entidades/gráficos desanexados</span><span class="sxs-lookup"><span data-stu-id="28046-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="28046-149">A nova API do `DbContext.AttachGraph` ajuda a reanexar entidades a um contexto para salvar as entidades novas/modificadas.</span><span class="sxs-lookup"><span data-stu-id="28046-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="28046-150">Salvando dados</span><span class="sxs-lookup"><span data-stu-id="28046-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="28046-151">Funcionalidade de gravação básica</span><span class="sxs-lookup"><span data-stu-id="28046-151">Basic save functionality</span></span>

<span data-ttu-id="28046-152">Permite que as alterações para instâncias de entidade persistam para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="28046-153">Simultaneidade otimista</span><span class="sxs-lookup"><span data-stu-id="28046-153">Optimistic Concurrency</span></span>

<span data-ttu-id="28046-154">Protege contra a substituição das alterações feitas por outro usuário, já que os dados foram obtidos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="28046-155">SaveChanges assíncrono</span><span class="sxs-lookup"><span data-stu-id="28046-155">Async SaveChanges</span></span>

<span data-ttu-id="28046-156">Pode liberar o thread atual para processar outras solicitações enquanto o banco de dados processa os comandos emitidos do `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="28046-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="28046-157">Transações de banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-157">Database Transactions</span></span>

<span data-ttu-id="28046-158">Significa que `SaveChanges` sempre é atômica (isso significa que, ou a alteração é concluída com sucesso, ou nenhuma alteração é feita no banco de dados).</span><span class="sxs-lookup"><span data-stu-id="28046-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="28046-159">Também há APIs relacionadas à transação para permitir o compartilhamento de transações entre instâncias de contexto etc.</span><span class="sxs-lookup"><span data-stu-id="28046-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="28046-160">Relacional: envio em lote de instruções</span><span class="sxs-lookup"><span data-stu-id="28046-160">Relational: Batching of statements</span></span>

<span data-ttu-id="28046-161">Fornece um desempenho melhor colocando em um único lote vários comandos INSERT/UPDATE/DELETE em uma única ida e volta ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="28046-162">Consulta</span><span class="sxs-lookup"><span data-stu-id="28046-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="28046-163">Suporte básico a LINQ</span><span class="sxs-lookup"><span data-stu-id="28046-163">Basic LINQ support</span></span>

<span data-ttu-id="28046-164">Fornece a capacidade de usar o LINQ para recuperar dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="28046-165">Avaliação mista de cliente/servidor</span><span class="sxs-lookup"><span data-stu-id="28046-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="28046-166">Permite que as consultas contenham lógica que não pode ser avaliada no banco de dados e, portanto, devem ser avaliadas após a recuperação dos dados na memória.</span><span class="sxs-lookup"><span data-stu-id="28046-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="28046-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="28046-167">NoTracking</span></span>

<span data-ttu-id="28046-168">As consultas permitem uma execução mais rápida de consultas quando o contexto não precisar monitorar a existência de alterações nas instâncias de entidade (isso será útil se os resultados forem somente leitura).</span><span class="sxs-lookup"><span data-stu-id="28046-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="28046-169">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="28046-169">Eager loading</span></span>

<span data-ttu-id="28046-170">Fornece os métodos `Include` e `ThenInclude` para identificar dados relacionados que também devem ser obtidos ao consultar.</span><span class="sxs-lookup"><span data-stu-id="28046-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="28046-171">Consulta assíncrona</span><span class="sxs-lookup"><span data-stu-id="28046-171">Async query</span></span>

<span data-ttu-id="28046-172">Pode liberar o thread atual (e seus recursos associados) para processar outras solicitações enquanto o banco de dados processa a consulta.</span><span class="sxs-lookup"><span data-stu-id="28046-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="28046-173">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="28046-173">Raw SQL queries</span></span>

<span data-ttu-id="28046-174">Fornece o método `DbSet.FromSql` para usar consultas SQL brutas a fim de buscar dados.</span><span class="sxs-lookup"><span data-stu-id="28046-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="28046-175">Essas consultas também podem ser compostas usando LINQ.</span><span class="sxs-lookup"><span data-stu-id="28046-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="28046-176">Gerenciamento de esquemas de banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="28046-177">APIs de criação/exclusão de banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="28046-178">São projetadas principalmente para teste, em que você deseja criar/excluir rapidamente o banco de dados sem o uso de migrações.</span><span class="sxs-lookup"><span data-stu-id="28046-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="28046-179">Migrações de banco de dados relacional</span><span class="sxs-lookup"><span data-stu-id="28046-179">Relational database migrations</span></span>

<span data-ttu-id="28046-180">Permitir a evolução de um esquema de banco de dados relacional após o expediente, à medida que o modelo sofre alterações.</span><span class="sxs-lookup"><span data-stu-id="28046-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="28046-181">Engenharia reversa do banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-181">Reverse engineer from database</span></span>

<span data-ttu-id="28046-182">Realiza o scaffolding de um modelo EF com base em um esquema de banco de dados relacional existente.</span><span class="sxs-lookup"><span data-stu-id="28046-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="28046-183">Provedores de banco de dados</span><span class="sxs-lookup"><span data-stu-id="28046-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="28046-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="28046-184">SQL Server</span></span>

<span data-ttu-id="28046-185">Conecta-se ao Microsoft SQL Server 2008 e posteriores.</span><span class="sxs-lookup"><span data-stu-id="28046-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="28046-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="28046-186">SQLite</span></span>

<span data-ttu-id="28046-187">Conecta-se a um banco de dados SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="28046-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="28046-188">Em Memória</span><span class="sxs-lookup"><span data-stu-id="28046-188">In-Memory</span></span>

<span data-ttu-id="28046-189">Foi projetado para habilitar facilmente o teste sem se conectar a um banco de dados real.</span><span class="sxs-lookup"><span data-stu-id="28046-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="28046-190">Provedores de terceiros</span><span class="sxs-lookup"><span data-stu-id="28046-190">3rd party providers</span></span>

<span data-ttu-id="28046-191">Há vários provedores disponíveis para outros mecanismos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="28046-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="28046-192">Veja [Provedores de banco de dados](../providers/index.md) para obter uma lista completa.</span><span class="sxs-lookup"><span data-stu-id="28046-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
