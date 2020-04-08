---
title: Consultando e Localizando entidades – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412971"
---
# <a name="querying-and-finding-entities"></a>Consultando e Localizando entidades
Este tópico aborda as várias maneiras que você pode consultar dados usando o Entity Framework, incluindo o LINQ e o método Find. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="finding-entities-using-a-query"></a>Localizando entidades usando uma consulta  

O DbSet e o IDbSet implementam o IQueryable e, portanto, podem ser usados como o ponto de partida para escrever uma consulta LINQ no banco de dados. Este não é o local apropriado para uma discussão aprofundada do LINQ, mas aqui estão alguns exemplos simples:  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

Observe que o DbSet e o IDbSet sempre criam consultas no banco de dados e sempre envolverão uma viagem de ida e volta para o banco de dados, mesmo se as entidades retornadas já existirem no contexto. Uma consulta é executada no banco de dados quando:  

- Ela é enumerada por uma instrução **foreach** (C#) ou **For Each** (Visual Basic).  
- Ela é enumerada por uma operação de coleção, como [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) ou [ToList](https://msdn.microsoft.com/library/bb342261).  
- Operadores LINQ, como [First](https://msdn.microsoft.com/library/bb291976) ou [Any](https://msdn.microsoft.com/library/bb337697) são especificados na parte mais externa da consulta.  
- Os seguintes métodos são chamados: o método de extensão [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) em um DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) e Database.ExecuteSqlCommand.  

Quando os resultados são retornados do banco de dados, objetos que não existem no contexto são anexados ao contexto. Se um objeto já estiver no contexto, o objeto existente será retornado (os valores atuais e originais das propriedades do objeto na entrada **não** serão substituídos pelos valores do banco de dados).  

Quando você executa uma consulta, as entidades que foram adicionadas ao contexto, mas ainda não foram salvas no banco de dados não são retornadas como parte do conjunto de resultados. Para obter os dados que estão no contexto, consulte [Dados locais](~/ef6/querying/local-data.md).  

Se uma consulta não retornar nenhuma linha do banco de dados, o resultado será uma coleção vazia, em vez de **null**.  

## <a name="finding-entities-using-primary-keys"></a>Localizando entidades usando chaves primárias  

O método Find no DbSet usa o valor da chave primária para tentar localizar uma entidade rastreada pelo contexto. Se a entidade não for encontrada no contexto, uma consulta será enviada ao banco de dados para localizar a entidade lá. Null será retornado se a entidade não for encontrada no contexto ou no banco de dados.  

Find é diferente de usar uma consulta de duas maneiras significativas:  

- Uma viagem de ida e volta para o banco de dados será feita apenas se a entidade com a determinada chave não for localizada no contexto.  
- Find retornará entidades que estão com o estado Adicionada. Isto é, Find retornará entidades que foram adicionadas ao contexto, mas ainda não foram salvas no banco de dados.  
### <a name="finding-an-entity-by-primary-key"></a>Localizando uma entidade pela chave primária  

O código a seguir mostra alguns usos de Find:  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>Localizando uma entidade pela chave primária composta  

O Entity Framework permite que as entidades tenham chaves compostas, essa é uma chave composta por mais de uma propriedade. Por exemplo, você pode ter uma entidade BlogSettings que representa as configurações do usuário para um blog específico. Como um usuário teria apenas uma BlogSettings para cada blog, você poderia optar por tornar a chave primária BlogSettings em uma combinação de BlogId e Username. O código a seguir tenta localizar a BlogSettings com BlogId = 3 e Username = "johndoe1987":  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Observe que quando tiver chaves compostas, você precisará usar ColumnAttribute ou a API fluente para especificar uma ordem para as propriedades da chave composta. A chamada para Find deve usar essa ordem quando especificando os valores que formam a chave.  
