---
title: Novidades no EF Core 2.0 – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 28b2180e898b91d233b590b1639674a464f8c679
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271427"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="58c89-102">Novos recursos no EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="58c89-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="58c89-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="58c89-103">.NET Standard 2.0</span></span>
<span data-ttu-id="58c89-104">O EF Core agora tem como destino o .NET Standard 2.0, o que significa que ele pode funcionar com o .NET Core 2.0, .NET Framework 4.6.1 e outras bibliotecas que implementam o .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="58c89-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="58c89-105">Consulte [Implementações do .NET compatíveis](../platforms/index.md) para obter mais detalhes sobre o que é compatível.</span><span class="sxs-lookup"><span data-stu-id="58c89-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="58c89-106">Modelagem</span><span class="sxs-lookup"><span data-stu-id="58c89-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="58c89-107">Divisão de tabela</span><span class="sxs-lookup"><span data-stu-id="58c89-107">Table splitting</span></span>

<span data-ttu-id="58c89-108">Agora é possível mapear dois ou mais tipos de entidade para a mesma tabela em que as colunas de chave primária serão compartilhadas e cada linha corresponderá a duas ou mais entidades.</span><span class="sxs-lookup"><span data-stu-id="58c89-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="58c89-109">Para usar a divisão de tabela, uma relação de identificação (em que as propriedades de chave estrangeira formam a chave primária) deve ser configurada entre todos os tipos de entidade que compartilham a tabela:</span><span class="sxs-lookup"><span data-stu-id="58c89-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```
<span data-ttu-id="58c89-110">Leia a [seção sobre separação de tabela](xref:core/modeling/table-splitting) para obter mais informações sobre esse recurso.</span><span class="sxs-lookup"><span data-stu-id="58c89-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="58c89-111">Tipos próprios</span><span class="sxs-lookup"><span data-stu-id="58c89-111">Owned types</span></span>

<span data-ttu-id="58c89-112">Um tipo de entidade própria pode compartilhar o mesmo tipo CLR com outro tipo de entidade própria, mas, considerando que ele não pode ser identificado apenas pelo tipo CLR, deve haver uma navegação para ele de outro tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="58c89-112">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="58c89-113">A entidade que contém a navegação de definição é o proprietário.</span><span class="sxs-lookup"><span data-stu-id="58c89-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="58c89-114">Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.</span><span class="sxs-lookup"><span data-stu-id="58c89-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="58c89-115">Por convenção, uma chave primária de sombra será criada para o tipo próprio e mapeada para a mesma tabela que a do proprietário usando a divisão de tabela.</span><span class="sxs-lookup"><span data-stu-id="58c89-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="58c89-116">Isso permite usar tipos próprios de maneira similar a como os tipos complexos são usados em EF6:</span><span class="sxs-lookup"><span data-stu-id="58c89-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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
<span data-ttu-id="58c89-117">Leia a [seção sobre tipos de entidade de propriedade](xref:core/modeling/owned-entities) para obter mais informações sobre esse recurso.</span><span class="sxs-lookup"><span data-stu-id="58c89-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="58c89-118">Filtros de consulta de nível de modelo</span><span class="sxs-lookup"><span data-stu-id="58c89-118">Model-level query filters</span></span>

<span data-ttu-id="58c89-119">O EF Core 2.0 inclui um novo recurso que chamamos de filtros de consulta de nível de Modelo.</span><span class="sxs-lookup"><span data-stu-id="58c89-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="58c89-120">Esse recurso permite que o LINQ consulte predicados (uma expressão booliana normalmente passada ao operador de consulta LINQ Where) a serem definidos diretamente nos Tipos de Entidade no modelo de metadados (normalmente, OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="58c89-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="58c89-121">Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão.</span><span class="sxs-lookup"><span data-stu-id="58c89-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="58c89-122">Alguns aplicativos comuns desse recurso são:</span><span class="sxs-lookup"><span data-stu-id="58c89-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="58c89-123">Exclusão reversível – um Tipo de Entidade define uma propriedade IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="58c89-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="58c89-124">Multilocação – um Tipo de entidade define uma propriedade TenantId.</span><span class="sxs-lookup"><span data-stu-id="58c89-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="58c89-125">Aqui está um exemplo simples que demonstra o recurso para os dois cenários listados acima:</span><span class="sxs-lookup"><span data-stu-id="58c89-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
<span data-ttu-id="58c89-126">Definimos um filtro no nível de modelo que implementa a multilocação e a exclusão reversível para instâncias do Tipo de Entidade `Post`.</span><span class="sxs-lookup"><span data-stu-id="58c89-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="58c89-127">Observe o uso de uma propriedade em nível de instância DbContext: `TenantId`.</span><span class="sxs-lookup"><span data-stu-id="58c89-127">Note the use of a DbContext instance level property: `TenantId`.</span></span> <span data-ttu-id="58c89-128">Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância de contexto que está executando a consulta).</span><span class="sxs-lookup"><span data-stu-id="58c89-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="58c89-129">Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="58c89-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="58c89-130">Limitações</span><span class="sxs-lookup"><span data-stu-id="58c89-130">Limitations</span></span>

- <span data-ttu-id="58c89-131">Não são permitidas referências de navegação.</span><span class="sxs-lookup"><span data-stu-id="58c89-131">Navigation references are not allowed.</span></span> <span data-ttu-id="58c89-132">Esse recurso pode ser adicionado com base nos comentários.</span><span class="sxs-lookup"><span data-stu-id="58c89-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="58c89-133">Os filtros podem ser definidos somente no Tipo de Entidade raiz de uma hierarquia.</span><span class="sxs-lookup"><span data-stu-id="58c89-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="58c89-134">Mapeamento de função escalar do banco de dados</span><span class="sxs-lookup"><span data-stu-id="58c89-134">Database scalar function mapping</span></span>

<span data-ttu-id="58c89-135">O EF Core 2.0 inclui uma contribuição importante de [Paul Middleton](https://github.com/pmiddleton) que permite o mapeamento de funções escalares do banco de dados para o stubs de método, de maneira que possam ser usadas em consultas LINQ e convertidas em SQL.</span><span class="sxs-lookup"><span data-stu-id="58c89-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="58c89-136">Aqui está uma breve descrição de como o recurso pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="58c89-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="58c89-137">Declarar um método estático em seu `DbContext` e anotá-lo com `DbFunctionAttribute`:</span><span class="sxs-lookup"><span data-stu-id="58c89-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="58c89-138">Métodos assim são automaticamente registrados.</span><span class="sxs-lookup"><span data-stu-id="58c89-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="58c89-139">Após o registro, as chamadas ao método em uma consulta LINQ podem ser convertidas em chamadas de função no SQL:</span><span class="sxs-lookup"><span data-stu-id="58c89-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="58c89-140">Alguns pontos a observar:</span><span class="sxs-lookup"><span data-stu-id="58c89-140">A few things to note:</span></span>

- <span data-ttu-id="58c89-141">Por convenção, que o nome do método é usado como o nome da função (neste caso, uma função definida pelo usuário) ao gerar o SQL, mas você pode substituir o nome e o esquema durante o registro do método</span><span class="sxs-lookup"><span data-stu-id="58c89-141">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="58c89-142">No momento, apenas funções escalares são compatíveis</span><span class="sxs-lookup"><span data-stu-id="58c89-142">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="58c89-143">É necessário criar a função mapeada no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58c89-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="58c89-144">As migrações do EF Core não se encarregarão de criá-la</span><span class="sxs-lookup"><span data-stu-id="58c89-144">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="58c89-145">Configuração de tipo autossuficiente primeiro para código</span><span class="sxs-lookup"><span data-stu-id="58c89-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="58c89-146">No EF6, era possível encapsular a configuração primeiro para código de um tipo de entidade específico derivando de *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="58c89-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="58c89-147">No EF Core 2.0, trazemos de volta este padrão:</span><span class="sxs-lookup"><span data-stu-id="58c89-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="58c89-148">Alto desempenho</span><span class="sxs-lookup"><span data-stu-id="58c89-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="58c89-149">Pool de DbContext</span><span class="sxs-lookup"><span data-stu-id="58c89-149">DbContext pooling</span></span>

<span data-ttu-id="58c89-150">O padrão básico para usar o EF Core em um aplicativo ASP.NET Core geralmente envolve registrar um tipo DbContext personalizado no sistema de injeção de dependência e então obter instâncias desse tipo por meio de parâmetros do construtor em controladores.</span><span class="sxs-lookup"><span data-stu-id="58c89-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="58c89-151">Isso significa que uma nova instância de DbContext é criada para cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="58c89-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="58c89-152">Na versão 2.0, estamos introduzindo uma nova maneira de registrar os tipos DbContext personalizados na injeção de dependência que introduz de modo transparente um pool de instâncias de DbContext reutilizáveis.</span><span class="sxs-lookup"><span data-stu-id="58c89-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="58c89-153">Para usar o pool de DbContext, use o `AddDbContextPool`, em vez de `AddDbContext`, durante o registro do serviço:</span><span class="sxs-lookup"><span data-stu-id="58c89-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="58c89-154">Se este método for usado, no momento em que uma instância de DbContext for solicitada por um controlador, primeiro vamos verificar se há uma instância disponível no pool.</span><span class="sxs-lookup"><span data-stu-id="58c89-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="58c89-155">Depois que o processamento da solicitação for finalizado, qualquer estado na instância será redefinido e a instância será retornada para o pool.</span><span class="sxs-lookup"><span data-stu-id="58c89-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="58c89-156">Isso é conceitualmente semelhante a como o pool de conexão opera em provedores ADO.NET e tem a vantagem de economizar alguns custos da inicialização da instância de DbContext.</span><span class="sxs-lookup"><span data-stu-id="58c89-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="58c89-157">Limitações</span><span class="sxs-lookup"><span data-stu-id="58c89-157">Limitations</span></span>

<span data-ttu-id="58c89-158">O novo método apresenta algumas limitações sobre o que pode ser feito no método `OnConfiguring()` do DbContext.</span><span class="sxs-lookup"><span data-stu-id="58c89-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="58c89-159">Evite usar o pooling do DbContext se você mantém seu próprio estado (por exemplo, campos privados) em sua classe DbContext derivada que não deve ser compartilhada entre solicitações.</span><span class="sxs-lookup"><span data-stu-id="58c89-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="58c89-160">O EF Core somente redefinirá o estado que reconhecer antes de adicionar uma instância de DbContext ao pool.</span><span class="sxs-lookup"><span data-stu-id="58c89-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="58c89-161">Consultas explicitamente compiladas</span><span class="sxs-lookup"><span data-stu-id="58c89-161">Explicitly compiled queries</span></span>

<span data-ttu-id="58c89-162">Este é o segundo recurso de desempenho de aceitação projetado para oferecer os benefícios em cenários de alta escala.</span><span class="sxs-lookup"><span data-stu-id="58c89-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="58c89-163">APIs de consulta compiladas manual ou explicitamente estavam disponíveis em versões anteriores do EF e também no LINQ to SQL para permitir aos aplicativos armazenar em cache a conversão das consultas para que elas possam ser calculadas apenas uma vez e executadas várias vezes.</span><span class="sxs-lookup"><span data-stu-id="58c89-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="58c89-164">Embora em geral o EF Core possa automaticamente compilar e armazenar em cache as consultas com base em uma representação em hash das expressões de consulta, esse mecanismo pode ser usado para obter um pequeno ganho de desempenho ignorando o cálculo de hash e a pesquisa de cache, permitindo que o aplicativo use uma consulta já compilada invocando um delegado.</span><span class="sxs-lookup"><span data-stu-id="58c89-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="58c89-165">Controle de Alterações</span><span class="sxs-lookup"><span data-stu-id="58c89-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="58c89-166">Anexar pode acompanhar um gráfico de entidades novas e existentes</span><span class="sxs-lookup"><span data-stu-id="58c89-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="58c89-167">O EF Core é compatível com a geração automática de valores de chave por meio de diversos mecanismos.</span><span class="sxs-lookup"><span data-stu-id="58c89-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="58c89-168">Ao usar esse recurso, um valor será gerado se a propriedade da chave for o padrão CLR – normalmente zero ou nulo.</span><span class="sxs-lookup"><span data-stu-id="58c89-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="58c89-169">Isso significa que um gráfico de entidades pode ser passado para `DbContext.Attach` ou `DbSet.Attach`, e o Core EF marcará as entidades que têm uma chave já definida como `Unchanged` enquanto as entidades que não têm um conjunto de chaves serão marcadas como `Added`.</span><span class="sxs-lookup"><span data-stu-id="58c89-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="58c89-170">Isso torna fácil anexar um gráfico de entidades mistas novas e existentes ao usar chaves geradas.</span><span class="sxs-lookup"><span data-stu-id="58c89-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="58c89-171">`DbContext.Update` e `DbSet.Update` funcionam da mesma forma, exceto que entidades com um conjunto de chaves são marcadas como `Modified`, em vez de `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="58c89-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="58c89-172">Consulta</span><span class="sxs-lookup"><span data-stu-id="58c89-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="58c89-173">Conversão de LINQ aprimorada</span><span class="sxs-lookup"><span data-stu-id="58c89-173">Improved LINQ Translation</span></span>

<span data-ttu-id="58c89-174">Permite a exceção mais bem-sucedida de consultas, avaliando mais lógica no banco de dados (em vez de na memória) e menos recuperação desnecessária de dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="58c89-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="58c89-175">Aprimoramentos de GroupJoin</span><span class="sxs-lookup"><span data-stu-id="58c89-175">GroupJoin improvements</span></span>

<span data-ttu-id="58c89-176">Este trabalho melhora o SQL gerado para associações de grupo.</span><span class="sxs-lookup"><span data-stu-id="58c89-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="58c89-177">Associações de grupo geralmente são o resultado de subconsultas em propriedades de navegação opcionais.</span><span class="sxs-lookup"><span data-stu-id="58c89-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="58c89-178">Interpolação de cadeia de caracteres em FromSql e ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="58c89-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="58c89-179">C# 6 introduziu Interpolação de Cadeia de Caracteres, um recurso que permite inserir expressões C# diretamente em literais de cadeia de caracteres, oferecendo uma ótima maneira de criar cadeias de caracteres no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="58c89-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="58c89-180">No EF Core 2.0, adicionamos suporte especial para cadeias de caracteres interpoladas às nossas duas APIs primárias que aceitam cadeias de caracteres SQL brutas: `FromSql` e `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="58c89-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="58c89-181">Esse novo suporte permite que a interpolação de cadeia de caracteres C# seja usada de maneira “segura”.</span><span class="sxs-lookup"><span data-stu-id="58c89-181">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="58c89-182">Ou seja, de uma maneira que proteja contra erros comuns de injeção de SQL que podem ocorrer ao criar SQL dinamicamente no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="58c89-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="58c89-183">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="58c89-183">Here is an example:</span></span>

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

<span data-ttu-id="58c89-184">Neste exemplo, há duas variáveis integradas na cadeia de caracteres em formato SQL.</span><span class="sxs-lookup"><span data-stu-id="58c89-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="58c89-185">O EF Core produzirá o SQL a seguir:</span><span class="sxs-lookup"><span data-stu-id="58c89-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="58c89-186">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="58c89-186">EF.Functions.Like()</span></span>

<span data-ttu-id="58c89-187">Adicionamos a propriedade EF.Functions, que pode ser usada pelo EF Core ou provedores para definir métodos que são mapeados para funções de banco de dados ou operadores de maneira a poderem ser invocados em consultas LINQ.</span><span class="sxs-lookup"><span data-stu-id="58c89-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="58c89-188">O primeiro exemplo de tal método é Like():</span><span class="sxs-lookup"><span data-stu-id="58c89-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="58c89-189">Observe que Like() vem com uma implementação em memória, que pode ser útil ao trabalhar com um banco de dados em memória ou quando a avaliação do predicado precisa ocorrer no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="58c89-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="58c89-190">Gerenciamento de banco de dados</span><span class="sxs-lookup"><span data-stu-id="58c89-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="58c89-191">Gancho de pluralização de scaffolding de DbContext</span><span class="sxs-lookup"><span data-stu-id="58c89-191">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="58c89-192">O EF Core 2.0 apresenta um novo serviço *IPluralizer* que é usado para singularizar os nomes de tipo de entidade e pluralizar os nomes DbSet.</span><span class="sxs-lookup"><span data-stu-id="58c89-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="58c89-193">A implementação padrão é não operacional, portanto, este é apenas um gancho que o pessoal pode conectar facilmente em seu próprios pluralizadores.</span><span class="sxs-lookup"><span data-stu-id="58c89-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="58c89-194">Esta é a aparência para um desenvolvedor conectar seu próprio pluralizador:</span><span class="sxs-lookup"><span data-stu-id="58c89-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="58c89-195">Outras pessoas</span><span class="sxs-lookup"><span data-stu-id="58c89-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="58c89-196">Mover o provedor ADO.NET SQLite para SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="58c89-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="58c89-197">Isso oferece uma solução mais robusta no Microsoft.Data.Sqlite para distribuir binários SQLite nativos em diferentes plataformas.</span><span class="sxs-lookup"><span data-stu-id="58c89-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="58c89-198">Somente um provedor por modelo</span><span class="sxs-lookup"><span data-stu-id="58c89-198">Only one provider per model</span></span>
<span data-ttu-id="58c89-199">Aumenta significativamente o modo como provedores podem interagir com o modelo e simplifica o modo como convenções, anotações e APIs fluente funcionam com diferentes provedores.</span><span class="sxs-lookup"><span data-stu-id="58c89-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="58c89-200">O EF Core 2.0 agora criará um [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) diferente para cada provedor diferente que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="58c89-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="58c89-201">Isso normalmente é transparente para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="58c89-201">This is usually transparent to the application.</span></span> <span data-ttu-id="58c89-202">Isso facilitou uma simplificação de APIs de metadados de nível inferior de modo que qualquer acesso a *conceitos de metadados relacionados comuns* pé feito sempre por meio de uma chamada para `.Relational`, em vez de `.SqlServer`, `.Sqlite` etc.</span><span class="sxs-lookup"><span data-stu-id="58c89-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="58c89-203">Consolidar registro em log e diagnóstico</span><span class="sxs-lookup"><span data-stu-id="58c89-203">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="58c89-204">Registro em log (com base em ILogger) e mecanismos de diagnóstico (com base em DiagnosticSource) agora compartilham mais código.</span><span class="sxs-lookup"><span data-stu-id="58c89-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="58c89-205">As IDs de evento das mensagens enviadas a um ILogger foram alteradas no 2.0.</span><span class="sxs-lookup"><span data-stu-id="58c89-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="58c89-206">As IDs de evento são exclusivas em todo o código do EF Core.</span><span class="sxs-lookup"><span data-stu-id="58c89-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="58c89-207">Essas mensagens agora também seguem o padrão para o registro em log estruturado usado pelo MVC, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="58c89-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="58c89-208">As categorias de agente também foram alteradas.</span><span class="sxs-lookup"><span data-stu-id="58c89-208">Logger categories have also changed.</span></span> <span data-ttu-id="58c89-209">Há agora um conjunto bem conhecido de categorias acessadas por meio de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="58c89-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="58c89-210">Eventos de DiagnosticSource agora usam os mesmos nomes de ID de evento como as mensagens `ILogger` correspondentes.</span><span class="sxs-lookup"><span data-stu-id="58c89-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
