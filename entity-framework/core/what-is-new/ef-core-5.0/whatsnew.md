---
title: O que há de novo no EF Core 5.0
description: Visão geral dos novos recursos no EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634276"
---
# <a name="whats-new-in-ef-core-50"></a>O que há de novo no EF Core 5.0

O EF Core 5.0 está atualmente em desenvolvimento.
Esta página conterá uma visão geral das alterações interessantes introduzidas em cada visualização.

Esta página não duplica o [plano para o EF Core 5.0](plan.md).
O plano descreve os temas gerais do EF Core 5.0, incluindo tudo o que estamos planejando incluir antes de enviar o lançamento final.

Adicionaremos links daqui à documentação oficial à medida que for publicada.

## <a name="preview-2"></a>Preview 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Use um atributo C# para especificar um campo de backup de propriedades

Um atributo C# agora pode ser usado para especificar o campo de backup de uma propriedade.
Este atributo permite que o EF Core ainda escreva e leia a partir do campo de respaldo como normalmente aconteceria, mesmo quando o campo de backup não pode ser encontrado automaticamente.
Por exemplo:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

A documentação é rastreada por [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230)de emissão .

### <a name="complete-discriminator-mapping"></a>Mapeamento discriminatório completo

O EF Core usa uma coluna discriminatória para [mapeamento TPH de uma hierarquia de herança](/ef/core/modeling/inheritance).
Alguns aprimoramentos de desempenho são possíveis desde que o EF Core conheça todos os valores possíveis para o discriminador.
O EF Core 5.0 agora implementa esses aprimoramentos.

Por exemplo, versões anteriores do EF Core sempre gerariam esse SQL para uma consulta retornando todos os tipos em uma hierarquia:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

O EF Core 5.0 agora gerará o seguinte quando um mapeamento discriminatório completo for configurado:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Será o comportamento padrão começando com a pré-visualização 3.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Melhorias de desempenho no Microsoft.Data.Sqlite

Fizemos duas melhorias de desempenho para o SQLIte:

* Recuperar dados binários e de seqüência com GetBytes, GetChars e GetTextReader agora é mais eficiente, fazendo uso de SqliteBlob e streams.
* A inicialização do SqliteConnection agora é preguiçosa.

Essas melhorias estão no ADO.NET provedor Microsoft.Data.Sqlite e, portanto, também melhoram o desempenho fora do EF Core.

## <a name="preview-1"></a>Preview 1

### <a name="simple-logging"></a>Registro simples

Este recurso adiciona funcionalidade `Database.Log` semelhante à do EF6.
Ou seja, ele fornece uma maneira simples de obter logs da EF Core sem a necessidade de configurar qualquer tipo de estrutura de registro externo.

A documentação preliminar está incluída no status semanal da [EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é rastreada por [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)de emissão .

### <a name="simple-way-to-get-generated-sql"></a>Maneira simples de gerar SQL gerado

O EF Core 5.0 introduz o `ToQueryString` método de extensão, que retornará o SQL que o EF Core irá gerar ao executar uma consulta LINQ.

A documentação preliminar está incluída no status semanal da [EF para 9 de janeiro de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

A documentação adicional é rastreada por [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)de emissão .

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Use um atributo C# para indicar que uma entidade não tem chave

Um tipo de entidade agora pode ser configurado como não tendo nenhuma chave usando o novo `KeylessAttribute`.
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

A documentação é rastreada por [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186)de emissão .

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>A seqüência de conexão ou conexão pode ser alterada no DbContext inicializado

Agora é mais fácil criar uma instância DbContext sem qualquer conexão ou seqüência de conexão.
Além disso, a seqüência de conexão ou conexão agora pode ser mutada na instância de contexto.
Esse recurso permite que a mesma instância de contexto se conecte dinamicamente a diferentes bancos de dados.

A documentação é rastreada por [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)de emissão .

### <a name="change-tracking-proxies"></a>Proxies de rastreamento de alterações

O EF Core agora pode gerar proxies de tempo de execução que implementam automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Em seguida, eles relatam alterações de valor nas propriedades da entidade diretamente para o EF Core, evitando a necessidade de varredura para alterações.
No entanto, os proxies vêm com seu próprio conjunto de limitações, então eles não são para todos.

A documentação é rastreada por [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)de emissão .

### <a name="enhanced-debug-views"></a>Visualizações aprimoradas de depuração

As visualizações de depuração são uma maneira fácil de olhar para os internos do EF Core quando problemas de depuração.
Uma visão dedepuração para o Modelo foi implementada há algum tempo.
Para o EF Core 5.0, facilitamos a visualização do modelo e adicionamos uma nova visão de depuração para entidades rastreadas no gestor estadual.

A documentação preliminar está incluída no status semanal da [EF para 12 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

A documentação adicional é rastreada por [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)de emissão .

### <a name="improved-handling-of-database-null-semantics"></a>Melhor manuseio da semântica nula do banco de dados

As bases de dados relacionais normalmente tratam null como um valor desconhecido e, portanto, não é igual a qualquer outro NULL.
Enquanto C# trata nulo como um valor definido, que se compara a qualquer outro nulo.
O EF Core por padrão traduz consultas para que eles usem a semântica nula C#.
O EF Core 5.0 melhora muito a eficiência dessas traduções.

A documentação é rastreada por [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)de emissão .

### <a name="indexer-properties"></a>Propriedades do indexador

O EF Core 5.0 suporta o mapeamento das propriedades do indexador C#.
Essas propriedades permitem que as entidades atuem como sacos de propriedades onde as colunas são mapeadas para nomear propriedades no saco.

A documentação é rastreada por [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)de emissão .

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Geração de restrições de verificação para mapeamentos de enum

As migrações do EF Core 5.0 agora podem gerar restrições check para mapeamentos de propriedades enum.
Por exemplo:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

A documentação é rastreada por [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)de emissão .

### <a name="isrelational"></a>IsRelational

Um `IsRelational` novo método foi adicionado, além `IsSqlServer` `IsSqlite`do `IsInMemory`existente, e .
Este método pode ser usado para testar se o DbContext está usando algum provedor de banco de dados relacional.
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

A documentação é rastreada por [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185)de emissão .

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos otimista selada com ETags

O provedor de banco de dados Azure Cosmos DB agora suporta concorrência otimista usando ETags.
Use o construtor de modelos em OnModelCriando para configurar um ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges, em `DbUpdateConcurrencyException` seguida, lançará um conflito de si0, que [pode ser manipulado](https://docs.microsoft.com/ef/core/saving/concurrency) para implementar repetições, etc.

A documentação é rastreada por [emissão #2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Tradução de consulta para mais construções DateTime

As consultas que contêm nova construção DateTime são agora traduzidas.

Além disso, as seguintes funções do SQL Server são agora mapeadas:

* DateDiffWeek
* DateFromParts

Por exemplo:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .

### <a name="query-translations-for-more-byte-array-constructs"></a>Tradução de consultas para mais construções de matriz de bytes

As consultas usando Contains, Length, SequenceEqual, etc. em byte[] propriedades são agora traduzidas para SQL.

A documentação preliminar está incluída no status semanal da [EF para 5 de dezembro de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

A documentação adicional é rastreada por [emissão #2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Tradução de consulta para Reverter

As consultas `Reverse` que usam agora são traduzidas.
Por exemplo:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .

### <a name="query-translation-for-bitwise-operators"></a>Tradução de consulta para operadores bitwise

Consultas usando operadores bitwise agora são traduzidas em mais casos Por exemplo:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .

### <a name="query-translation-for-strings-on-cosmos"></a>Tradução de consulta para strings no Cosmos

As consultas que usam os métodos de string Contains, StartsWith e EndsWith são agora traduzidas ao usar o provedor Azure Cosmos DB.

A documentação é rastreada por [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)de emissão .
