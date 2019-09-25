---
title: Novos recursos no Entity Framework Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/index
ms.openlocfilehash: ebebbf286d9d8e06492a3c664799e1127c7dc8c0
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197915"
---
# <a name="new-features-in-entity-framework-core-30"></a>Novos recursos no Entity Framework Core 3.0

A lista a seguir inclui os principais novos recursos no EF Core 3.0.

Por ser uma versão principal, o EF Core 3.0 também contém várias [alterações de falha](xref:core/what-is-new/ef-core-3.0/breaking-changes), que são melhorias de API que podem ter um impacto negativo em aplicativos existentes.  

## <a name="linq-overhaul"></a>Visão geral do LINQ 

O LINQ permite que você escreva consultas de banco de dados usando a linguagem .NET de sua escolha, aproveitando informações de tipo avançado para oferecer IntelliSense e verificação do tipo em tempo de compilação.
Mas o LINQ também permite escrever um número ilimitado de consultas complicadas que contêm expressões arbitrárias (chamadas de método ou operações).
O modo de lidar com todas essas combinações é o principal desafio para os provedores LINQ.

No EF Core 3.0, rearquitetamos nosso provedor LINQ para permitir a conversão de mais padrões de consulta em SQL, gerando consultas eficazes em mais casos e impedindo que consultas ineficazes não sejam detectadas. O novo provedor LINQ é a base sobre a qual será possível oferecer novos recursos de consulta e melhorias de desempenho em versões futuras, sem interromper aplicativos e provedores de dados existentes.

### <a name="restricted-client-evaluation"></a>Avaliação de cliente restrita
A alteração de design mais importante tem a ver com a forma de lidarmos com expressões LINQ que não podem ser convertidas em parâmetros ou convertidas em SQL.

Em versões anteriores, o EF Core identificava quais partes de uma consulta poderiam ser convertidas em SQL e executava o restante da consulta no cliente.
Esse tipo de execução do lado do cliente é recomendável em algumas situações. Porém, em muitos outros casos, pode resultar em consultas ineficazes.

Por exemplo, se o EF Core 2.2 não pudesse converter um predicado em uma chamada `Where()`, ele executava uma instrução SQL sem um filtro, transferia todas as linhas do banco de dados e, em seguida, filtrava-as na memória:

``` csharp
var specialCustomers = 
  context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

Isso pode ser aceitável caso o banco de dados contenha um número pequeno de linhas, mas poderá resultar em problemas significativos de desempenho ou até mesmo em falha de aplicativo se o banco de dados tiver um grande número de linhas.

No EF Core 3.0, restringimos a avaliação do cliente para acontecer somente na projeção de nível superior (essencialmente, a última chamada para `Select()`).
Quando o EF Core 3.0 detecta expressões que não podem ser convertidas em nenhum outro lugar da consulta, ele lança uma exceção de tempo de execução.

Para avaliar uma condição de predicado no cliente como no exemplo anterior, os desenvolvedores agora precisam alternar explicitamente a avaliação da consulta para o LINQ to Objects: 

``` csharp
var specialCustomers =
  context.Customers
    .Where(c => c.Name.StartsWith(n)) 
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

Confira a [documentação sobre alterações de falha](xref:core/what-is-new/ef-core-3.0/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) para saber mais sobre como isso pode afetar os aplicativos existentes.

### <a name="single-sql-statement-per-linq-query"></a>Única instrução SQL por consulta LINQ

Outro aspecto do design que mudou significativamente no 3.0 é que agora sempre geramos uma única instrução SQL por consulta LINQ. Em versões anteriores, costumávamos gerar várias instruções SQL em determinados casos, como converter chamadas `Include()` em propriedades de navegação de coleção e converter consultas que seguiam certos padrões com subconsultas. Embora isso fosse conveniente em alguns casos e, para `Include()`, isso inclusive ajudasse a evitar o envio de dados redundantes, a implementação era complexa, o que resultava em comportamentos extremamente ineficazes (consultas N+1), além de haver situações em que os dados retornados de várias consultas poderiam ser inconsistentes.

De modo semelhante à avaliação do cliente, se o EF Core 3.0 não puder converter uma consulta LINQ em uma única instrução SQL, ele lançará uma exceção de tempo de exceção. No entanto, agora o EF Core é capaz de converter muitos dos padrões comuns que costumavam gerar várias consultas em uma única consulta com JOINs.

## <a name="cosmos-db-support"></a>Suporte do Cosmos DB 

O provedor do Cosmos DB para o EF Core permite que desenvolvedores familiarizados com o modelo de programação EF conduzam facilmente o Azure Cosmos DB como um banco de dados de aplicativo. A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência. O provedor ativa a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.

Confira a [documentação do provedor do Cosmos DB](xref:core/providers/cosmos/index) para saber mais detalhes.

## <a name="c-80-support"></a>Suporte do C# 8.0

O EF Core 3.0 aproveita alguns dos [novos recursos do C# 8.0](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8):

### <a name="asynchronous-streams"></a>Fluxos assíncronos

Os resultados de consulta assíncronos agora são expostos por meio da nova interface padrão `IAsyncEnumerable<T>` e podem ser consumidos usando `await foreach`.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

Confira os [fluxos assíncronos na documentação do C#](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) para saber mais detalhes.

### <a name="nullable-reference-types"></a>Tipos de referência anuláveis 

Quando este novo recurso é habilitado no código, o EF Core examina a nulidade das propriedades de tipo de referência e aplica-a às colunas e relacionamentos correspondentes no banco de dados: as propriedades de tipos de referências que não permite valor nulo são tratadas como se tivessem o atributo de anotação de dados `[Required]`.

Por exemplo, na seguinte classe, as propriedades marcadas como do tipo `string?` serão configuradas como opcionais, enquanto `string` serão configuradas como obrigatória:

``` csharp
public class Customer
{
  public int Id { get; set; }
  public string FirstName { get; set; }
  public string LastName { get; set; }
  public string? MiddleName { get; set; }
}
```

Confira [Trabalhar com tipos de referência que permitem valor nulo](xref:core/miscellaneous/nullable-reference-types) na documentação do EF Core para saber mais detalhes.

## <a name="interception-of-database-operations"></a>Interceptação de operações de banco de dados

A nova API de interceptação do EF Core 3.0 permite fornecer a lógica personalizada a ser invocada automaticamente sempre que operações de banco de dados de baixo nível ocorrerem como parte da operação normal do EF Core. Por exemplo, ao abrir conexões, confirmar transações ou executar comandos. 

Da mesma forma que os recursos de interceptação que existiam no EF 6, os interceptadores permitem que você intercepte operações antes ou depois que elas ocorram. Ao interceptá-las antes que elas aconteçam, você poderá ignorar a execução e fornecer resultados alternativos baseados na lógica de interceptação. 

Por exemplo, para manipular o texto do comando, você pode criar um `IDbCommandInterceptor`:

``` csharp 
public class HintCommandInterceptor : DbCommandInterceptor
{
  public override InterceptionResult ReaderExecuting(
    DbCommand command, 
    CommandEventData eventData, 
    InterceptionResult result)
  {
    // Manipulate the command text, etc. here...
    command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
    return result;
  }
}
``` 

E registrá-lo com  `DbContext`:

``` csharp
services.AddDbContext(b => b
  .UseSqlServer(connectionString)
  .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a>Engenharia reversa de exibições de banco de dados

Os tipos de consulta, que representam dados que podem ser lidos do banco de dados, mas não atualizados, foram renomeados para [tipos de entidade sem chave](xref:core/modeling/keyless-entity-types). Como eles são extremamente adequados para mapear exibições de banco de dados na maioria dos cenários, o EF Core agora cria automaticamente tipos de entidade sem chave ao fazer engenharia reversa de exibições de banco de dados.

Por exemplo, usando a [ferramenta de linha de comando dotnet ef](xref:core/miscellaneous/cli/dotnet), você pode digitar:

``` console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

E a ferramenta fará scaffolding dos tipos automaticamente para exibições e tabelas sem chaves:

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
  modelBuilder.Entity<Names>(entity =>
  {
    entity.HasNoKey();
    entity.ToView("Names");
  });

  modelBuilder.Entity<Things>(entity =>
  {
    entity.HasNoKey();
  });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais

No EF Core 3.0 em diante, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, será possível adicionar um `Order` sem um `OrderDetails` e todas as propriedades `OrderDetails`, com exceção da chave primária, serão mapeadas para colunas que permitem valor nulo.

Ao realizar consultas, o EF Core definirá `OrderDetails` como `null` se uma de suas propriedades necessárias não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 no .NET Core

Esse não é realmente um recurso do EF Core 3.0, mas achamos que ele é importante para muitos dos nossos clientes atuais. 

Reconhecemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo. Por esse motivo, decidimos portar a versão mais recente do EF 6 para execução no .NET Core 3.0. 

Para obter mais detalhes, confira [Novidades no EF 6](xref:ef6/what-is-new/index).

## <a name="postponed-features"></a>Recursos adiados

Alguns recursos originalmente planejados para o EF Core 3.0 foram adiados para futuras versões:

- A habilidade de ignorar partes de um modelo em migrações, controladas como [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entidades de recipiente de propriedades, controladas como dois problemas separados: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre entidades de tipo compartilhado e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre suporte a mapeamento de propriedades indexadas.
