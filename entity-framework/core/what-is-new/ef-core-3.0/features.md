---
title: Novos recursos no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d938f17daecd5031147951d0018602c5635de41d
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149094"
---
# <a name="new-features-included-in-ef-core-30"></a>Novos recursos incluídos no EF Core 3,0

A lista a seguir inclui os principais novos recursos planejados para o EF Core 3.0.

EF Core 3,0 é uma versão principal e também contém várias [alterações significativas](xref:core/what-is-new/ef-core-3.0/breaking-changes), que são melhorias de API que podem ter impacto negativo sobre os aplicativos existentes.  

## <a name="linq-improvements"></a>Melhorias do LINQ 

O LINQ permite que você grave consultas de banco de dados sem deixar a linguagem de sua escolha, tirando proveito de informações de tipo avançadas para oferecer verificação de tipo IntelliSense e de tempo de compilação.
Mas o LINQ também permite que você grave um número ilimitado de consultas complicadas que contêm expressões arbitrárias (chamadas de método ou operações).
Lidar com todas essas combinações sempre foi um desafio significativo para os provedores de LINQ.
No EF Core 3,0, reescrevemos nossa implementação de LINQ para permitir a tradução de mais expressões no SQL, a fim de gerar consultas eficientes em mais casos, para evitar que consultas ineficientes não sejam detectadas e para facilitar a introdução de uma nova consulta gradualmente. recursos e desempenho improvementswithout interrompendo aplicativos e provedores de dados existentes.

### <a name="client-evaluation"></a>Avaliação do cliente

A principal alteração de design no EF Core 3,0 tem a ver com como ele lida com expressões LINQ que ele não pode converter em SQL ou parâmetros:

Nas primeiras versões, EF Core simplesmente descobrir quais partes de uma consulta poderiam ser convertidas em SQL e executou o restante da consulta no cliente.
Esse tipo de execução do lado do cliente pode ser desejável em algumas situações, mas em muitos outros casos ele pode resultar em consultas ineficientes.
Por exemplo, se EF Core 2,2 não pôde converter um predicado em uma `Where()` chamada, ele executou uma instrução SQL sem um filtro, leu todas as linhas do banco de dados e as filtrou na memória.
Isso pode ser aceitável se o banco de dados contiver um pequeno número de linhas, mas pode resultar em problemas de desempenho significativos ou até mesmo falha de aplicativo se o banco de dados contiver um grande número ou linhas.
No EF Core 3,0, a avaliação do cliente foi restrita para acontecer apenas na projeção de nível superior (a última `Select()`chamada para).
Quando EF Core 3,0 detecta expressões que não podem ser convertidas em qualquer outro lugar na consulta, ela gera uma exceção de tempo de execução.

## <a name="cosmos-db-support"></a>Suporte do Cosmos DB 

O provedor Cosmos DB para EF Core permite que os desenvolvedores familiarizados com o modelo de programação do EF destinam-se facilmente a Azure Cosmos DB como um banco de dados de aplicativos.
A meta é tornar algumas das vantagens do Cosmos DB ainda mais acessíveis para os desenvolvedores de .NET, tais como a distribuição global, disponibilidade "Always On", escalabilidade elástica e baixa latência.
O provedor ativará a maioria dos recursos do EF Core, como controle automático de alterações, LINQ e conversões de valores, em relação à API do SQL no Cosmos DB.

## <a name="c-80-support"></a>Suporte do C# 8.0

EF Core 3,0 aproveita alguns dos novos recursos do C# 8,0:

### <a name="asynchronous-streams"></a>Fluxos assíncronos

Os resultados da consulta assíncrona agora são expostos usando `IAsyncEnumerable<T>` a nova interface padrão e podem `await foreach`ser consumidos usando o.

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

### <a name="nullable-reference-types"></a>Tipos de referência anuláveis 

Quando esse novo recurso está habilitado em seu código, EF Core pode motivo da nulidade das propriedades de tipos referência (de tipos primitivos, como cadeia de caracteres ou propriedades de navegação) para decidir a nulidade das colunas e relações no banco de dados.

## <a name="interception"></a>Interceptação

A nova API de interceptação no EF Core 3,0 permite observar e modificar de forma programática o resultado de operações de banco de dados de nível baixo que ocorrem como parte da operação normal do EF Core, como abrir conexões, initating transações e executar comandos. 

## <a name="reverse-engineering-of-database-views"></a>Engenharia reversa de exibições de banco de dados

Tipos de entidade sem chaves (anteriormente conhecidas como [tipos de consulta](xref:core/modeling/keyless-entity-types)) representam dados que podem ser lidos do banco de dado, mas não podem ser atualizados.
Essa característica torna-se uma ótima opção para mapear exibições de banco de dados na maioria dos cenários, portanto, automatizamos a criação de tipos de entidade sem chaves ao reverter exibições de banco de dados.

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

Entendemos que muitos aplicativos existentes usam versões anteriores do EF e que a portabilidade deles para o EF Core somente para aproveitar o .NET Core pode exigir um esforço significativo.
Por esse motivo, habilitamos a versão newewst do EF 6 para ser executada no .NET Core 3,0.
Há algumas limitações, por exemplo:
- Novos provedores precisam funcionar no .NET Core
- Suporte espacial com o SQL Server não será habilitado

## <a name="postponed-features"></a>Recursos adiados

Alguns recursos originalmente planejados para EF Core 3,0 foram adiados para versões futuras: 

- Capacidade de ingore partes de um modelo em migrações, rastreadas como [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entidades do recipiente de propriedades, rastreadas como dois problemas separados: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre entidades de tipo compartilhado e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre o suporte a mapeamento de propriedade indexada.
