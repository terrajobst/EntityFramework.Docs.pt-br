---
title: Novidades no EF Core 2.2 – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688740"
---
# <a name="new-features-in-ef-core-22"></a>Novos recursos no EF Core 2.2

## <a name="spatial-data-support"></a>Suporte a dados espaciais

Os dados espaciais podem ser usados para representar o local físico e a forma de objetos.
Muitos bancos de dados podem armazenar, indexar e consultar dados espaciais nativamente. Cenários comuns incluem consultas para objetos em uma determinada distância e testar se um polígono contém um determinado local.
Agora o EF Core 2.2 permite trabalhar com dados espaciais de vários bancos de dados usando tipos de biblioteca de [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).

O suporte a dados espaciais é implementado como uma série de pacotes de extensão específica do provedor.
Cada um desses pacotes contribui com mapeamentos de métodos e tipos NTS e as funções e tipos espaciais correspondentes no banco de dados.
Essas extensões de provedor agora estão disponíveis para [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) e [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (do [projeto Npgsql](http://www.npgsql.org/)).
Os tipos espaciais podem ser usados diretamente com o [provedor na memória do EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) sem extensões adicionais.

Depois de instalar a extensão do provedor, você pode adicionar propriedades de tipos com suporte às suas entidades. Por exemplo:

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

Você pode usar a persistência de entidades com dados espaciais:

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
E você pode executar consultas de banco de dados com base em operações e dados espaciais:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Para saber mais sobre esse recurso, confira a [documentação de tipos espaciais](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Coleções de entidades de propriedade

O EF Core 2.0 adicionou a capacidade de modelar propriedade em associações de um para um.
O EF Core 2.2 amplia a capacidade de expressar a propriedade para associações de um para muitos.
A propriedade ajuda a restringir como as entidades são usadas.

Por exemplo, entidades de propriedade:
- Só podem aparecer nas propriedades de navegação de outros tipos de entidades. 
- São carregadas automaticamente e só podem ser acompanhadas por um DbContext junto com seu proprietário.

Em bancos de dados relacionais, as coleções de propriedade são mapeadas em tabelas separadas do proprietário, assim como as associações regulares de um para muitos.
Mas, em bancos de dados orientado a documentos, planejamos manter juntas as entidades de propriedade (em referências ou coleções de propriedade) dentro do mesmo documento que o proprietário.

Você pode usar o recurso chamando a nova API OwnsMany():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Para saber mais, confira a [documentação atualizada de entidades de propriedade](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Marcas de consulta

Esse recurso simplifica a correlação de consultas LINQ no código com as consultas SQL geradas capturadas nos logs.

Para aproveitar as marcas de consulta, você deve anotar uma consulta LINQ usando o novo método TagWith().
Usando a consulta espacial do exemplo anterior:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Essa consulta LINQ produzirá a seguinte saída SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Para saber mais, confira a [documentação de marcas de consulta](xref:core/querying/tags). 