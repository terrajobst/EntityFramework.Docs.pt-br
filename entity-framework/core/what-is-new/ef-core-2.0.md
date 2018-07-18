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
# <a name="new-features-in-ef-core-20"></a>Novos recursos no EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0
O EF Core agora tem como destino o .NET Standard 2.0, o que significa que ele pode funcionar com o .NET Core 2.0, .NET Framework 4.6.1 e outras bibliotecas que implementam o .NET Standard 2.0.
Consulte [Implementações do .NET compatíveis](../platforms/index.md) para obter mais detalhes sobre o que é compatível.

## <a name="modeling"></a>Modelagem

### <a name="table-splitting"></a>Divisão de tabela

Agora é possível mapear dois ou mais tipos de entidade para a mesma tabela em que as colunas de chave primária serão compartilhadas e cada linha corresponderá a duas ou mais entidades.

Para usar a divisão de tabela, uma relação de identificação (em que as propriedades de chave estrangeira formam a chave primária) deve ser configurada entre todos os tipos de entidade que compartilham a tabela:

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a>Tipos próprios

Um tipo de entidade própria pode compartilhar o mesmo tipo CLR com outro tipo de entidade própria, mas, considerando que ele não pode ser identificado apenas pelo tipo CLR, deve haver uma navegação para ele de outro tipo de entidade. A entidade que contém a navegação de definição é o proprietário. Ao consultar o proprietário, os tipos próprios serão incluídos por padrão.

Por convenção, uma chave primária de sombra será criada para o tipo próprio e mapeada para a mesma tabela que a do proprietário usando a divisão de tabela. Isso permite usar tipos próprios de maneira similar a como os tipos complexos são usados em EF6:

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
Leia a [seção sobre tipos de entidade de propriedade](xref:core/modeling/owned-entities) para obter mais informações sobre esse recurso.

### <a name="model-level-query-filters"></a>Filtros de consulta de nível de modelo

O EF Core 2.0 inclui um novo recurso que chamamos de filtros de consulta de nível de Modelo. Esse recurso permite que o LINQ consulte predicados (uma expressão booliana normalmente passada ao operador de consulta LINQ Where) a serem definidos diretamente nos Tipos de Entidade no modelo de metadados (normalmente, OnModelCreating). Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão. Alguns aplicativos comuns desse recurso são:

- Exclusão reversível – um Tipo de Entidade define uma propriedade IsDeleted.
- Multilocação – um Tipo de entidade define uma propriedade TenantId.

Aqui está um exemplo simples que demonstra o recurso para os dois cenários listados acima:

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
Definimos um filtro no nível de modelo que implementa a multilocação e a exclusão reversível para instâncias do Tipo de Entidade ```Post```. Observe o uso de uma propriedade em nível de instância DbContext: ```TenantId```. Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância de contexto que está executando a consulta).

Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador IgnoreQueryFilters().

#### <a name="limitations"></a>Limitações

- Não são permitidas referências de navegação. Esse recurso pode ser adicionado com base nos comentários.
- Os filtros podem ser definidos somente no Tipo de Entidade raiz de uma hierarquia.

### <a name="database-scalar-function-mapping"></a>Mapeamento de função escalar do banco de dados

O EF Core 2.0 inclui uma contribuição importante de [Paul Middleton](https://github.com/pmiddleton) que permite o mapeamento de funções escalares do banco de dados para o stubs de método, de maneira que possam ser usadas em consultas LINQ e convertidas em SQL.

Aqui está uma breve descrição de como o recurso pode ser usado:

Declarar um método estático em seu `DbContext` e anotá-lo com `DbFunctionAttribute`:

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

Métodos assim são automaticamente registrados. Após o registro, as chamadas ao método em uma consulta LINQ podem ser convertidas em chamadas de função no SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Alguns pontos a observar:

- Por convenção, que o nome do método é usado como o nome da função (neste caso, uma função definida pelo usuário) ao gerar o SQL, mas você pode substituir o nome e o esquema durante o registro do método
- No momento, apenas funções escalares são compatíveis
- É necessário criar a função mapeada no banco de dados. As migrações do EF Core não se encarregarão de criá-la

### <a name="self-contained-type-configuration-for-code-first"></a>Configuração de tipo autossuficiente primeiro para código

No EF6, era possível encapsular a configuração primeiro para código de um tipo de entidade específico derivando de *EntityTypeConfiguration*. No EF Core 2.0, trazemos de volta este padrão:

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

## <a name="high-performance"></a>Alto desempenho

### <a name="dbcontext-pooling"></a>Pool de DbContext

O padrão básico para usar o EF Core em um aplicativo ASP.NET Core geralmente envolve registrar um tipo DbContext personalizado no sistema de injeção de dependência e então obter instâncias desse tipo por meio de parâmetros do construtor em controladores. Isso significa que uma nova instância de DbContext é criada para cada solicitação.

Na versão 2.0, estamos introduzindo uma nova maneira de registrar os tipos DbContext personalizados na injeção de dependência que introduz de modo transparente um pool de instâncias de DbContext reutilizáveis. Para usar o pool de DbContext, use o ```AddDbContextPool```, em vez de ```AddDbContext```, durante o registro do serviço:

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Se este método for usado, no momento em que uma instância de DbContext for solicitada por um controlador, primeiro vamos verificar se há uma instância disponível no pool. Depois que o processamento da solicitação for finalizado, qualquer estado na instância será redefinido e a instância será retornada para o pool.

Isso é conceitualmente semelhante a como o pool de conexão opera em provedores ADO.NET e tem a vantagem de economizar alguns custos da inicialização da instância de DbContext.

### <a name="limitations"></a>Limitações

O novo método apresenta algumas limitações sobre o que pode ser feito no método ```OnConfiguring()``` do DbContext.

> [!WARNING]  
> Evite usar o pooling do DbContext se você mantém seu próprio estado (por exemplo, campos privados) em sua classe DbContext derivada que não deve ser compartilhada entre solicitações. O EF Core somente redefinirá o estado que reconhecer antes de adicionar uma instância de DbContext ao pool.

### <a name="explicitly-compiled-queries"></a>Consultas explicitamente compiladas

Este é o segundo recurso de desempenho de aceitação projetado para oferecer os benefícios em cenários de alta escala.

APIs de consulta compiladas manual ou explicitamente estavam disponíveis em versões anteriores do EF e também no LINQ to SQL para permitir aos aplicativos armazenar em cache a conversão das consultas para que elas possam ser calculadas apenas uma vez e executadas várias vezes.

Embora em geral o EF Core possa automaticamente compilar e armazenar em cache as consultas com base em uma representação em hash das expressões de consulta, esse mecanismo pode ser usado para obter um pequeno ganho de desempenho ignorando o cálculo de hash e a pesquisa de cache, permitindo que o aplicativo use uma consulta já compilada invocando um delegado.

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

## <a name="change-tracking"></a>Controle de Alterações

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Anexar pode acompanhar um gráfico de entidades novas e existentes

O EF Core é compatível com a geração automática de valores de chave por meio de diversos mecanismos. Ao usar esse recurso, um valor será gerado se a propriedade da chave for o padrão CLR – normalmente zero ou nulo. Isso significa que um gráfico de entidades pode ser passado para `DbContext.Attach` ou `DbSet.Attach`, e o Core EF marcará as entidades que têm uma chave já definida como `Unchanged` enquanto as entidades que não têm um conjunto de chaves serão marcadas como `Added`. Isso torna fácil anexar um gráfico de entidades mistas novas e existentes ao usar chaves geradas. `DbContext.Update` e `DbSet.Update` funcionam da mesma forma, exceto que entidades com um conjunto de chaves são marcadas como `Modified`, em vez de `Unchanged`.

## <a name="query"></a>Consulta

### <a name="improved-linq-translation"></a>Conversão de LINQ aprimorada

Permite a exceção mais bem-sucedida de consultas, avaliando mais lógica no banco de dados (em vez de na memória) e menos recuperação desnecessária de dados do banco de dados.

### <a name="groupjoin-improvements"></a>Aprimoramentos de GroupJoin

Este trabalho melhora o SQL gerado para associações de grupo. Associações de grupo geralmente são o resultado de subconsultas em propriedades de navegação opcionais.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Interpolação de cadeia de caracteres em FromSql e ExecuteSqlCommand

C# 6 introduziu Interpolação de Cadeia de Caracteres, um recurso que permite inserir expressões C# diretamente em literais de cadeia de caracteres, oferecendo uma ótima maneira de criar cadeias de caracteres no tempo de execução. No EF Core 2.0, adicionamos suporte especial para cadeias de caracteres interpoladas às nossas duas APIs primárias que aceitam cadeias de caracteres SQL brutas: ```FromSql``` e ```ExecuteSqlCommand```. Esse novo suporte permite que a interpolação de cadeia de caracteres C# seja usada de maneira “segura”. Ou seja, de uma maneira que proteja contra erros comuns de injeção de SQL que podem ocorrer ao criar SQL dinamicamente no tempo de execução.

Veja um exemplo:

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

Neste exemplo, há duas variáveis integradas na cadeia de caracteres em formato SQL. O EF Core produzirá o SQL a seguir:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Adicionamos a propriedade EF.Functions, que pode ser usada pelo EF Core ou provedores para definir métodos que são mapeados para funções de banco de dados ou operadores de maneira a poderem ser invocados em consultas LINQ. O primeiro exemplo de tal método é Like():

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Observe que Like() vem com uma implementação em memória, que pode ser útil ao trabalhar com um banco de dados em memória ou quando a avaliação do predicado precisa ocorrer no lado do cliente.

## <a name="database-management"></a>Gerenciamento de banco de dados

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Gancho de pluralização de scaffolding de DbContext

O EF Core 2.0 apresenta um novo serviço *IPluralizer* que é usado para singularizar os nomes de tipo de entidade e pluralizar os nomes DbSet. A implementação padrão é não operacional, portanto, este é apenas um gancho que o pessoal pode conectar facilmente em seu próprios pluralizadores.

Esta é a aparência para um desenvolvedor conectar seu próprio pluralizador:

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

## <a name="others"></a>Outras pessoas

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Mover o provedor ADO.NET SQLite para SQLitePCL.raw
Isso oferece uma solução mais robusta no Microsoft.Data.Sqlite para distribuir binários SQLite nativos em diferentes plataformas.

### <a name="only-one-provider-per-model"></a>Somente um provedor por modelo
Aumenta significativamente o modo como provedores podem interagir com o modelo e simplifica o modo como convenções, anotações e APIs fluente funcionam com diferentes provedores.

O EF Core 2.0 agora criará um [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diferente para cada provedor diferente que está sendo usado. Isso normalmente é transparente para o aplicativo. Isso facilitou uma simplificação de APIs de metadados de nível inferior de modo que qualquer acesso a *conceitos de metadados relacionados comuns* pé feito sempre por meio de uma chamada para `.Relational`, em vez de `.SqlServer`, `.Sqlite` etc.

### <a name="consolidated-logging-and-diagnostics"></a>Consolidar registro em log e diagnóstico

Registro em log (com base em ILogger) e mecanismos de diagnóstico (com base em DiagnosticSource) agora compartilham mais código.

As IDs de evento das mensagens enviadas a um ILogger foram alteradas no 2.0. As IDs de evento são exclusivas em todo o código do EF Core. Essas mensagens agora também seguem o padrão para o registro em log estruturado usado pelo MVC, por exemplo.

As categorias de agente também foram alteradas. Há agora um conjunto bem conhecido de categorias acessadas por meio de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Eventos de DiagnosticSource agora usam os mesmos nomes de ID de evento como as mensagens `ILogger` correspondentes.
