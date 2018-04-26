---
title: Novidades no EF Core 2.1 – EF Core
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: db1648095aa4d612af53f4e10a30be36edc40da5
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
# <a name="new-features-in-ef-core-21"></a>Novos recursos no EF Core 2.1
> [!NOTE]  
> Essa versão ainda está em versão prévia.

Além de várias correções de bugs e pequenas melhorias funcionais e de desempenho, o EF Core 2.1 inclui alguns recursos novos e interessantes:

## <a name="lazy-loading"></a>Carregamento lento
Agora o EF Core contém os blocos de construção necessários para que qualquer pessoa crie classes de entidade que podem carregar suas propriedades de navegação sob demanda. Também criamos um novo pacote, o Microsoft.EntityFrameworkCore.Proxies, que faz uso desses blocos de construção para gerar classes de proxy de carregamento lento com base em classes de entidade minimamente modificadas (por exemplo, classes com propriedades de navegação virtual).

Leia a [seção sobre carregamento lento](xref:core/querying/related-data#lazy-loading) para obter mais informações sobre este tópico.

## <a name="parameters-in-entity-constructors"></a>Parâmetros em construtores de entidade
Como um dos blocos de construção necessários para o carregamento lento, habilitamos a criação de entidades que recebem parâmetros em seus construtores. Você pode usar parâmetros para injetar valores de propriedade, delegados de carregamento lento e serviços.

Leia a [seção sobre construtor de entidade com parâmetros](xref:core/modeling/constructors) para obter mais informações sobre este tópico.

## <a name="value-conversions"></a>Conversões de valor
Até agora, o EF Core só podia mapear propriedades de tipos com suporte nativo do provedor de banco de dados subjacente. Os valores eram copiados entre colunas e propriedades sem qualquer transformação. Começando com o EF Core 2.1, agora é possível aplicar conversões de valor para transformar os valores obtidos das colunas antes que eles sejam aplicados às propriedades e vice-versa. Há diversas conversões que podem ser aplicadas por convenção, conforme necessário, bem como uma API de configuração explícita que permite o registro de conversões personalizadas entre colunas e propriedades. Algumas das aplicações desse recurso são:

- Armazenamento de enumerações como cadeias de caracteres
- Mapeamento de inteiros sem sinal com o SQL Server
- Criptografia e descriptografia automática de valores de propriedade

Leia a [seção sobre conversões de valor](xref:core/modeling/value-conversions) para obter mais informações sobre este tópico.  

## <a name="linq-groupby-translation"></a>Conversão de GroupBy do LINQ
Antes da versão 2.1, o operador GroupBy do LINQ, no EF Core, era sempre avaliado na memória. Agora há suporte para convertê-lo para a cláusula GROUP BY do SQL na maioria dos casos comuns.

Este exemplo mostra uma consulta com GroupBy usada para calcular várias funções de agregação:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

A conversão de SQL correspondente tem esta aparência:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Propagação de dados
Com a nova versão, será possível fornecer os dados iniciais para popular um banco de dados. Diferentemente do EF6, a propagação de dados está associada a um tipo de entidade que faz parte da configuração do modelo. Assim, as migrações do EF Core podem calcular automaticamente quais operações inserir, atualizar ou excluir precisam ser aplicadas ao atualizar o banco de dados para uma nova versão do modelo.

Por exemplo, você pode usar isso para configurar dados de semente para uma Postagem no `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

Leia a [seção sobre propagação de dados](xref:core/modeling/data-seeding) para obter mais informações sobre este tópico.  

## <a name="query-types"></a>Tipos de consulta
Agora um modelo do EF Core pode incluir tipos de consulta. Diferentemente dos tipos de entidade, os tipos de consulta não têm chaves definidas neles e não podem ser inseridos, excluídos nem atualizados (ou seja, eles são somente leitura), mas podem ser retornados diretamente pelas consultas. Estes são alguns dos cenários de uso para tipos de consulta:

- Mapeamento para exibições sem chaves primárias
- Mapeamento para tabelas sem chaves primárias
- Mapeamento para consultas definidas no modelo
- Servir como o tipo de retorno para consultas `FromSql()`

Leia a [seção sobre tipos de consulta](xref:core/modeling/query-types) para obter mais informações sobre este tópico.

## <a name="include-for-derived-types"></a>Include para tipos derivados
Agora será possível especificar propriedades de navegação definidas somente em tipos derivados ao escrever expressões para o método `Include`. Para obter a versão fortemente tipada do `Include`, há suporte tanto para o uso de uma conversão explícita quanto do operador `as`. Agora também há suporte para referenciar os nomes da propriedade de navegação definida nos tipos derivados na versão de cadeia de caracteres do `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Leia a [seção sobre Include com tipos derivados](xref:core/querying/related-data#include-on-derived-types) para obter mais informações sobre este tópico.

## <a name="systemtransactions-support"></a>Suporte para System.Transactions
Adicionamos a capacidade de trabalhar com recursos System.Transactions, como TransactionScope. Isso funcionará no .NET Framework e no .NET Core ao usar os provedores de banco de dados que sejam compatíveis.

Leia a [seção sobre System.Transactions](xref:core/saving/transactions#using-systemtransactions) para obter mais informações sobre este tópico.

## <a name="better-column-ordering-in-initial-migration"></a>Melhor ordenação de colunas na migração inicial
Com base nos comentários dos clientes, atualizamos as migrações para que gerem inicialmente colunas para tabelas na mesma ordem em que as propriedades são declaradas nas classes. Observe que o EF Core não pode alterar a ordem quando novos membros são adicionados após a criação inicial da tabela.

## <a name="optimization-of-correlated-subqueries"></a>Otimização de subconsultas correlacionadas
Aprimoramos nossa conversão de consulta para evitar a execução de consultas SQL "N + 1" em muitos cenários comuns, nos quais o uso de uma propriedade de navegação na projeção leva à associação de dados da consulta raiz com os dados de uma subconsulta correlacionada. A otimização requer o armazenamento em buffer dos resultados da subconsulta e é necessário que você modifique a consulta para aceitar o novo comportamento.

Por exemplo, a consulta a seguir normalmente é convertida em uma consulta para os Clientes, além de N (em que "N" é o número retornado de clientes) consultas separadas para os Pedidos:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Ao incluir `ToList()` no lugar certo, você indica que o buffer é adequado para os Pedidos, permitindo a otimização:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Observe que essa consulta será convertida em apenas duas consultas SQL: uma para os Clientes e o outra para os Pedidos.

## <a name="ownedattribute"></a>OwnedAttribute

Agora é possível configurar [tipos de entidade de propriedade](xref:core/modeling/owned-entities) simplesmente anotando o tipo com `[Owned]` e, em seguida, certificando-se de que a entidade de proprietário seja adicionada ao modelo:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="new-dotnet-ef-global-tool"></a>Nova ferramenta dotnet-ef global

Os comandos _dotnet ef_ foram convertidos em uma ferramenta global CLI do .NET, portanto, não será mais necessário usar DotNetCliToolReference no projeto para usar migrações ou para fazer o scaffold de um DbContext de um banco de dados existente.

## <a name="microsoftentityframeworkcoreabstractions-package"></a>Pacote Microsoft.EntityFrameworkCore.Abstractions
O novo pacote contém atributos e interfaces para você usar em projetos e destacar recursos EF Core sem tomar uma dependência no EF Core como um todo. Por exemplo, o atributo [Owned] introduzido na versão prévia 1 foi movido para cá.

## <a name="state-change-events"></a>Eventos de alteração de estado

Novos eventos `Tracked` e `StateChanged` no `ChangeTracker` podem ser usados para escrever lógica que reage a entidades inserindo o DbContext ou alterando o estado dele.

## <a name="raw-sql-parameter-analyzer"></a>Analisador de parâmetros de SQL bruto

Um novo analisador de código foi incluído no EF Core que detecta usos potencialmente não seguros de nossas APIs de SQL bruto, como `FromSql` ou `ExecuteSqlCommand`. Por exemplo, para a consulta a seguir, você verá um aviso porque o _minAge_ não está parametrizado:

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a>Compatibilidade do provedor de banco de dados

O EF Core 2.1 foi desenvolvido para ser compatível com provedores de banco de dados criados para o EF Core 2.0 ou, no máximo, necessitar de alterações mínimas. Embora alguns dos recursos descritos acima (por exemplo, conversões de valor) exijam um provedor atualizado, outros (por exemplo, carregamento lento) funcionarão com provedores existentes.

> [!TIP]
> Se você encontrar qualquer incompatibilidade inesperada ou qualquer problema nos novos recursos, ou se você tiver comentários sobre eles, informe usando [nosso rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues/new).
