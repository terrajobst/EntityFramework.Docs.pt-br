---
title: Novidades no EF Core 2.0 – EF Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 538458cf49ee86b9a5cba2f606adc04e583605e2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949121"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="00836-102">Novos recursos no EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="00836-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="00836-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="00836-103">.NET Standard 2.0</span></span>
<span data-ttu-id="00836-104">O EF Core agora tem como destino o .NET Standard 2.0, o que significa que ele pode funcionar com o .NET Core 2.0, .NET Framework 4.6.1 e outras bibliotecas que implementam o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="00836-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="00836-105">Consulte [Implementações do .NET compatíveis](../platforms/index.md) para obter mais detalhes sobre o que é compatível.</span><span class="sxs-lookup"><span data-stu-id="00836-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="00836-106">Modelagem</span><span class="sxs-lookup"><span data-stu-id="00836-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="00836-107">Divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="00836-107">Table splitting</span></span>

<span data-ttu-id="00836-108">Agora é possível mapear dois ou mais tipos de entidade para a mesma tabela em que as colunas de chave primária serão compartilhadas e cada linha corresponderá a duas ou mais entidades.</span><span class="sxs-lookup"><span data-stu-id="00836-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="00836-109">Para usar a divisão de tabela, uma relação de identificação (em que as propriedades de chave estrangeira formam a chave primária) deve ser configurada entre todos os tipos de entidade que compartilham a tabela:</span><span class="sxs-lookup"><span data-stu-id="00836-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="00836-110">Tipos próprios</span><span class="sxs-lookup"><span data-stu-id="00836-110">Owned types</span></span>

<span data-ttu-id="00836-111">Um tipo de entidade própria pode compartilhar o mesmo tipo CLR com outro tipo de entidade própria, mas, considerando que ele não pode ser identificado apenas pelo tipo CLR, deve haver uma navegação para ele de outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="00836-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="00836-112">A entidade que contém a navegação de definição é o proprietário.</span><span class="sxs-lookup"><span data-stu-id="00836-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="00836-113">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="00836-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="00836-114">Por convenção, uma chave primária de sombra será criada para o tipo próprio e mapeada para a mesma tabela que a do proprietário usando a divisão de tabela.</span><span class="sxs-lookup"><span data-stu-id="00836-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="00836-115">Isso permite usar tipos próprios de maneira similar a como os tipos complexos são usados em EF6:</span><span class="sxs-lookup"><span data-stu-id="00836-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```
<span data-ttu-id="00836-116">Leia a [seção sobre tipos de entidade de propriedade](xref:core/modeling/owned-entities) para obter mais informações sobre esse recurso.</span><span class="sxs-lookup"><span data-stu-id="00836-116">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="00836-117">Filtros de consulta de nível de modelo</span><span class="sxs-lookup"><span data-stu-id="00836-117">Model-level query filters</span></span>

<span data-ttu-id="00836-118">O EF Core 2.0 inclui um novo recurso que chamamos de filtros de consulta de nível de Modelo.</span><span class="sxs-lookup"><span data-stu-id="00836-118">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="00836-119">Esse recurso permite que o LINQ consulte predicados (uma expressão booliana normalmente passada ao operador de consulta LINQ Where) a serem definidos diretamente nos Tipos de Entidade no modelo de metadados (normalmente, OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="00836-119">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="00836-120">Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão.</span><span class="sxs-lookup"><span data-stu-id="00836-120">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="00836-121">Alguns aplicativos comuns desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="00836-121">Some common applications of this feature are:</span></span>

- <span data-ttu-id="00836-122">Exclusão reversível – um Tipo de Entidade define uma propriedade IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="00836-122">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="00836-123">Multilocação – um Tipo de entidade define uma propriedade TenantId.</span><span class="sxs-lookup"><span data-stu-id="00836-123">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="00836-124">Aqui está um exemplo simples que demonstra o recurso para os dois cenários listados acima:</span><span class="sxs-lookup"><span data-stu-id="00836-124">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="00836-125">Definimos um filtro no nível de modelo que implementa a multilocação e a exclusão reversível para instâncias do Tipo de Entidade ```Post```.</span><span class="sxs-lookup"><span data-stu-id="00836-125">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="00836-126">Observe o uso de uma propriedade em nível de instância DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="00836-126">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="00836-127">Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância de contexto que está executando a consulta).</span><span class="sxs-lookup"><span data-stu-id="00836-127">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="00836-128">Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="00836-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="00836-129">Limitações</span><span class="sxs-lookup"><span data-stu-id="00836-129">Limitations</span></span>

- <span data-ttu-id="00836-130">Não são permitidas referências de navegação.</span><span class="sxs-lookup"><span data-stu-id="00836-130">Navigation references are not allowed.</span></span> <span data-ttu-id="00836-131">Esse recurso pode ser adicionado com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="00836-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="00836-132">Os filtros podem ser definidos somente no Tipo de Entidade raiz de uma hierarquia.</span><span class="sxs-lookup"><span data-stu-id="00836-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="00836-133">Mapeamento de função escalar do banco de dados</span><span class="sxs-lookup"><span data-stu-id="00836-133">Database scalar function mapping</span></span>

<span data-ttu-id="00836-134">O EF Core 2.0 inclui uma contribuição importante de [Paul Middleton](https://github.com/pmiddleton) que permite o mapeamento de funções escalares do banco de dados para o stubs de método, de maneira que possam ser usadas em consultas LINQ e convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="00836-134">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="00836-135">Aqui está uma breve descrição de como o recurso pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="00836-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="00836-136">Declarar um método estático em seu `DbContext` e anotá-lo com `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="00836-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="00836-137">Métodos assim são automaticamente registrados.</span><span class="sxs-lookup"><span data-stu-id="00836-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="00836-138">Após o registro, as chamadas ao método em uma consulta LINQ podem ser convertidas em chamadas de função no SQL:</span><span class="sxs-lookup"><span data-stu-id="00836-138">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="00836-139">Alguns pontos a observar:</span><span class="sxs-lookup"><span data-stu-id="00836-139">A few things to note:</span></span>

- <span data-ttu-id="00836-140">Por convenção, que o nome do método é usado como o nome da função (neste caso, uma função definida pelo usuário) ao gerar o SQL, mas você pode substituir o nome e o esquema durante o registro do método</span><span class="sxs-lookup"><span data-stu-id="00836-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="00836-141">No momento, apenas funções escalares são compatíveis</span><span class="sxs-lookup"><span data-stu-id="00836-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="00836-142">É necessário criar a função mapeada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="00836-142">You must create the mapped function in the database.</span></span> <span data-ttu-id="00836-143">As migrações do EF Core não se encarregarão de criá-la</span><span class="sxs-lookup"><span data-stu-id="00836-143">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="00836-144">Configuração de tipo autossuficiente primeiro para código</span><span class="sxs-lookup"><span data-stu-id="00836-144">Self-contained type configuration for code first</span></span>

<span data-ttu-id="00836-145">No EF6, era possível encapsular a configuração primeiro para código de um tipo de entidade específico derivando de *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="00836-145">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="00836-146">No EF Core 2.0, trazemos de volta este padrão:</span><span class="sxs-lookup"><span data-stu-id="00836-146">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="00836-147">Alto desempenho</span><span class="sxs-lookup"><span data-stu-id="00836-147">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="00836-148">Pool de DbContext</span><span class="sxs-lookup"><span data-stu-id="00836-148">DbContext pooling</span></span>

<span data-ttu-id="00836-149">O padrão básico para usar o EF Core em um aplicativo ASP.NET Core geralmente envolve registrar um tipo DbContext personalizado no sistema de injeção de dependência e então obter instâncias desse tipo por meio de parâmetros do construtor em controladores.</span><span class="sxs-lookup"><span data-stu-id="00836-149">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="00836-150">Isso significa que uma nova instância de DbContext é criada para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="00836-150">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="00836-151">Na versão 2.0, estamos introduzindo uma nova maneira de registrar os tipos DbContext personalizados na injeção de dependência que introduz de modo transparente um pool de instâncias de DbContext reutilizáveis.</span><span class="sxs-lookup"><span data-stu-id="00836-151">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="00836-152">Para usar o pool de DbContext, use o ```AddDbContextPool```, em vez de ```AddDbContext```, durante o registro do serviço:</span><span class="sxs-lookup"><span data-stu-id="00836-152">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="00836-153">Se este método for usado, no momento em que uma instância de DbContext for solicitada por um controlador, primeiro vamos verificar se há uma instância disponível no pool.</span><span class="sxs-lookup"><span data-stu-id="00836-153">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="00836-154">Depois que o processamento da solicitação for finalizado, qualquer estado na instância será redefinido e a instância será retornada para o pool.</span><span class="sxs-lookup"><span data-stu-id="00836-154">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="00836-155">Isso é conceitualmente semelhante a como o pool de conexão opera em provedores ADO.NET e tem a vantagem de economizar alguns custos da inicialização da instância de DbContext.</span><span class="sxs-lookup"><span data-stu-id="00836-155">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="00836-156">Limitações</span><span class="sxs-lookup"><span data-stu-id="00836-156">Limitations</span></span>

<span data-ttu-id="00836-157">O novo método apresenta algumas limitações sobre o que pode ser feito no método ```OnConfiguring()``` do DbContext.</span><span class="sxs-lookup"><span data-stu-id="00836-157">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="00836-158">Evite usar o pooling do DbContext se você mantém seu próprio estado (por exemplo, campos privados) em sua classe DbContext derivada que não deve ser compartilhada entre solicitações.</span><span class="sxs-lookup"><span data-stu-id="00836-158">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="00836-159">O EF Core somente redefinirá o estado que reconhecer antes de adicionar uma instância de DbContext ao pool.</span><span class="sxs-lookup"><span data-stu-id="00836-159">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="00836-160">Consultas explicitamente compiladas</span><span class="sxs-lookup"><span data-stu-id="00836-160">Explicitly compiled queries</span></span>

<span data-ttu-id="00836-161">Este é o segundo recurso de desempenho de aceitação projetado para oferecer os benefícios em cenários de alta escala.</span><span class="sxs-lookup"><span data-stu-id="00836-161">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="00836-162">APIs de consulta compiladas manual ou explicitamente estavam disponíveis em versões anteriores do EF e também no LINQ to SQL para permitir aos aplicativos armazenar em cache a conversão das consultas para que elas possam ser calculadas apenas uma vez e executadas várias vezes.</span><span class="sxs-lookup"><span data-stu-id="00836-162">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="00836-163">Embora em geral o EF Core possa automaticamente compilar e armazenar em cache as consultas com base em uma representação em hash das expressões de consulta, esse mecanismo pode ser usado para obter um pequeno ganho de desempenho ignorando o cálculo de hash e a pesquisa de cache, permitindo que o aplicativo use uma consulta já compilada invocando um delegado.</span><span class="sxs-lookup"><span data-stu-id="00836-163">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="00836-164">Controle de Alterações</span><span class="sxs-lookup"><span data-stu-id="00836-164">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="00836-165">Anexar pode acompanhar um gráfico de entidades novas e existentes</span><span class="sxs-lookup"><span data-stu-id="00836-165">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="00836-166">O EF Core é compatível com a geração automática de valores de chave por meio de diversos mecanismos.</span><span class="sxs-lookup"><span data-stu-id="00836-166">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="00836-167">Ao usar esse recurso, um valor será gerado se a propriedade da chave for o padrão CLR – normalmente zero ou nulo.</span><span class="sxs-lookup"><span data-stu-id="00836-167">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="00836-168">Isso significa que um gráfico de entidades pode ser passado para `DbContext.Attach` ou `DbSet.Attach`, e o Core EF marcará as entidades que têm uma chave já definida como `Unchanged` enquanto as entidades que não têm um conjunto de chaves serão marcadas como `Added`.</span><span class="sxs-lookup"><span data-stu-id="00836-168">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="00836-169">Isso torna fácil anexar um gráfico de entidades mistas novas e existentes ao usar chaves geradas.</span><span class="sxs-lookup"><span data-stu-id="00836-169">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="00836-170">`DbContext.Update` e `DbSet.Update` funcionam da mesma forma, exceto que entidades com um conjunto de chaves são marcadas como `Modified`, em vez de `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="00836-170">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="00836-171">Consulta</span><span class="sxs-lookup"><span data-stu-id="00836-171">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="00836-172">Conversão de LINQ aprimorada</span><span class="sxs-lookup"><span data-stu-id="00836-172">Improved LINQ Translation</span></span>

<span data-ttu-id="00836-173">Permite a exceção mais bem-sucedida de consultas, avaliando mais lógica no banco de dados (em vez de na memória) e menos recuperação desnecessária de dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="00836-173">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="00836-174">Aprimoramentos de GroupJoin</span><span class="sxs-lookup"><span data-stu-id="00836-174">GroupJoin improvements</span></span>

<span data-ttu-id="00836-175">Este trabalho melhora o SQL gerado para associações de grupo.</span><span class="sxs-lookup"><span data-stu-id="00836-175">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="00836-176">Associações de grupo geralmente são o resultado de subconsultas em propriedades de navegação opcionais.</span><span class="sxs-lookup"><span data-stu-id="00836-176">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="00836-177">Interpolação de cadeia de caracteres em FromSql e ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="00836-177">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="00836-178">C# 6 introduziu Interpolação de Cadeia de Caracteres, um recurso que permite inserir expressões C# diretamente em literais de cadeia de caracteres, oferecendo uma ótima maneira de criar cadeias de caracteres no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="00836-178">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="00836-179">No EF Core 2.0, adicionamos suporte especial para cadeias de caracteres interpoladas às nossas duas APIs primárias que aceitam cadeias de caracteres SQL brutas: ```FromSql``` e ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="00836-179">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="00836-180">Esse novo suporte permite que a interpolação de cadeia de caracteres C# seja usada de maneira “segura”.</span><span class="sxs-lookup"><span data-stu-id="00836-180">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="00836-181">Ou seja, de uma maneira que proteja contra erros comuns de injeção de SQL que podem ocorrer ao criar SQL dinamicamente no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="00836-181">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="00836-182">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="00836-182">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="00836-183">Neste exemplo, há duas variáveis integradas na cadeia de caracteres em formato SQL.</span><span class="sxs-lookup"><span data-stu-id="00836-183">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="00836-184">O EF Core produzirá o SQL a seguir:</span><span class="sxs-lookup"><span data-stu-id="00836-184">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="00836-185">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="00836-185">EF.Functions.Like()</span></span>

<span data-ttu-id="00836-186">Adicionamos a propriedade EF.Functions, que pode ser usada pelo EF Core ou provedores para definir métodos que são mapeados para funções de banco de dados ou operadores de maneira a poderem ser invocados em consultas LINQ.</span><span class="sxs-lookup"><span data-stu-id="00836-186">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="00836-187">O primeiro exemplo de tal método é Like():</span><span class="sxs-lookup"><span data-stu-id="00836-187">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="00836-188">Observe que Like() vem com uma implementação em memória, que pode ser útil ao trabalhar com um banco de dados em memória ou quando a avaliação do predicado precisa ocorrer no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="00836-188">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="00836-189">Gerenciamento de banco de dados</span><span class="sxs-lookup"><span data-stu-id="00836-189">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="00836-190">Gancho de pluralização de scaffolding de DbContext</span><span class="sxs-lookup"><span data-stu-id="00836-190">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="00836-191">O EF Core 2.0 apresenta um novo serviço *IPluralizer* que é usado para singularizar os nomes de tipo de entidade e pluralizar os nomes DbSet.</span><span class="sxs-lookup"><span data-stu-id="00836-191">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="00836-192">A implementação padrão é não operacional, portanto, este é apenas um gancho que o pessoal pode conectar facilmente em seu próprios pluralizadores.</span><span class="sxs-lookup"><span data-stu-id="00836-192">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="00836-193">Esta é a aparência para um desenvolvedor conectar seu próprio pluralizador:</span><span class="sxs-lookup"><span data-stu-id="00836-193">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="00836-194">Outras pessoas</span><span class="sxs-lookup"><span data-stu-id="00836-194">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="00836-195">Mover o provedor ADO.NET SQLite para SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="00836-195">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="00836-196">Isso oferece uma solução mais robusta no Microsoft.Data.Sqlite para distribuir binários SQLite nativos em diferentes plataformas.</span><span class="sxs-lookup"><span data-stu-id="00836-196">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="00836-197">Somente um provedor por modelo</span><span class="sxs-lookup"><span data-stu-id="00836-197">Only one provider per model</span></span>
<span data-ttu-id="00836-198">Aumenta significativamente o modo como provedores podem interagir com o modelo e simplifica o modo como convenções, anotações e APIs fluente funcionam com diferentes provedores.</span><span class="sxs-lookup"><span data-stu-id="00836-198">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="00836-199">O EF Core 2.0 agora criará um [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diferente para cada provedor diferente que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="00836-199">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="00836-200">Isso normalmente é transparente para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="00836-200">This is usually transparent to the application.</span></span> <span data-ttu-id="00836-201">Isso facilitou uma simplificação de APIs de metadados de nível inferior de modo que qualquer acesso a *conceitos de metadados relacionados comuns* pé feito sempre por meio de uma chamada para `.Relational`, em vez de `.SqlServer`, `.Sqlite` etc.</span><span class="sxs-lookup"><span data-stu-id="00836-201">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="00836-202">Consolidar registro em log e diagnóstico</span><span class="sxs-lookup"><span data-stu-id="00836-202">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="00836-203">Registro em log (com base em ILogger) e mecanismos de diagnóstico (com base em DiagnosticSource) agora compartilham mais código.</span><span class="sxs-lookup"><span data-stu-id="00836-203">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="00836-204">As IDs de evento das mensagens enviadas a um ILogger foram alteradas no 2.0.</span><span class="sxs-lookup"><span data-stu-id="00836-204">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="00836-205">As IDs de evento são exclusivas em todo o código do EF Core.</span><span class="sxs-lookup"><span data-stu-id="00836-205">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="00836-206">Essas mensagens agora também seguem o padrão para o registro em log estruturado usado pelo MVC, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="00836-206">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="00836-207">As categorias de agente também foram alteradas.</span><span class="sxs-lookup"><span data-stu-id="00836-207">Logger categories have also changed.</span></span> <span data-ttu-id="00836-208">Há agora um conjunto bem conhecido de categorias acessadas por meio de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="00836-208">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="00836-209">Eventos de DiagnosticSource agora usam os mesmos nomes de ID de evento como as mensagens `ILogger` correspondentes.</span><span class="sxs-lookup"><span data-stu-id="00836-209">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
