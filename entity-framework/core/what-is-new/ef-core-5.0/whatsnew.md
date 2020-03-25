---
title: O que há de novo no EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136253"
---
# <a name="whats-new-in-ef-core-50"></a>O que há de novo no EF Core 5,0

O EF Core 5,0 está em desenvolvimento no momento.
Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.

Esta página não duplica o [plano para EF Core 5,0](plan.md).
O plano descreve os temas gerais para EF Core 5,0, incluindo tudo o que estamos planejando incluir antes de enviar a versão final.

Vamos adicionar links daqui à documentação oficial conforme ele é publicado.

## <a name="preview-1"></a>Preview 1

### <a name="simple-logging"></a>Log simples

Esse recurso adiciona uma funcionalidade semelhante à `Database.Log` em EF6.
Ou seja, ele fornece uma maneira simples de obter logs de EF Core sem a necessidade de configurar qualquer tipo de estrutura de log externa.

A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é acompanhada pelo problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Maneira simples de obter o SQL gerado

EF Core 5,0 apresenta o método de extensão `ToQueryString`, que retornará o SQL que EF Core gerará ao executar uma consulta LINQ.

A documentação preliminar está incluída no [status semanal do EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

A documentação adicional é acompanhada pelo problema [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Use um C# atributo para indicar que uma entidade não tem nenhuma chave

Um tipo de entidade agora pode ser configurado como sem chave usando o novo `KeylessAttribute`.
Por exemplo:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

A documentação é controlada pelo problema [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>A conexão ou a cadeia de conexão pode ser alterada no DbContext inicializado

Agora é mais fácil criar uma instância DbContext sem nenhuma conexão ou cadeia de conexão.
Além disso, a conexão ou a cadeia de conexão agora pode ser modificada na instância de contexto.
Isso permite que a mesma instância de contexto se conecte dinamicamente a bancos de dados diferentes.

A documentação é controlada pelo problema [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxies de controle de alterações

EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Em seguida, eles relatam alterações de valor nas propriedades de entidade diretamente em EF Core, evitando a necessidade de verificar se há alterações.
No entanto, os proxies vêm com seu próprio conjunto de limitações, para que eles não sejam para todos.

A documentação é controlada pelo problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Exibições de depuração aprimoradas

As exibições de depuração são uma maneira fácil de examinar os elementos internos de EF Core ao depurar problemas.
Uma exibição de depuração para o modelo foi implementada há algum tempo.
Para EF Core 5,0, tornamos a exibição de modelo mais fácil de ler e adicionamos uma nova exibição de depuração para entidades rastreadas no Gerenciador de estado.

A documentação preliminar está incluída no [status semanal do EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

A documentação adicional é acompanhada pelo problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Tratamento aprimorado da semântica nula do banco de dados

Os bancos de dados relacionais normalmente tratam nulo como um valor desconhecido e, portanto, não são iguais a nenhum outro nulo.
C#, por outro lado, trata NULL como um valor definido que compara igual a qualquer outro nulo.
EF Core, por padrão, traduz as consultas para que elas C# usem a semântica nula.
EF Core 5,0 melhora muito a eficiência dessas traduções.

A documentação é controlada pelo problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Propriedades do indexador

EF Core 5,0 dá suporte ao C# mapeamento de propriedades do indexador.
Isso permite que as entidades atuem como pacotes de propriedades, em que as colunas são mapeadas para as propriedades nomeadas na bolsa.

A documentação é controlada pelo problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Geração de restrições de verificação para mapeamentos de enumeração

EF Core migrações 5,0 agora podem gerar restrições de verificação para mapeamentos de propriedade de enumeração.
Por exemplo:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

A documentação é controlada pelo problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>Isrelacional

Um novo método `IsRelational` foi adicionado além do `IsSqlServer`existente, `IsSqlite`e `IsInMemory`.
Isso pode ser usado para testar se DbContext está usando qualquer provedor de banco de dados relacional.
Por exemplo:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

A documentação é controlada pelo problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos simultaneidade otimista com ETags

O provedor de banco de dados Azure Cosmos DB agora dá suporte à simultaneidade otimista usando ETags.
Use o construtor de modelos no OnModelCreating para configurar uma ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

Em seguida, SaveChanges lançará um `DbUpdateConcurrencyException` em um conflito de simultaneidade, que [pode ser tratado](https://docs.microsoft.com/ef/core/saving/concurrency) para implementar repetições, etc.


A documentação é controlada pelo problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Conversões de consulta para mais construções DateTime

As consultas que contêm a nova construção DateTime agora são traduzidas.

Além disso, as seguintes funções de SQL Server agora são mapeadas:
* DateDiffWeek
* DateFromParts

Por exemplo:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Conversões de consulta para mais constructos de matriz de bytes

As consultas que usam Contains, Length, SequenceEqual, etc. nas propriedades byte [] agora são convertidas em SQL.

A documentação preliminar está incluída no [status semanal do EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é acompanhada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Conversão de consulta para reversa

As consultas que usam `Reverse` agora são traduzidas.
Por exemplo:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Conversão de consulta para operadores bit a bit

Agora, as consultas que usam operadores de bit que são são traduzidas em mais casos, por exemplo:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Conversão de consulta para cadeias de caracteres em Cosmos

As consultas que usam os métodos de cadeia de caracteres contém, StartsWith e EndsWith agora são traduzidas ao usar o provedor de Azure Cosmos DB.

A documentação é controlada pelo problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
