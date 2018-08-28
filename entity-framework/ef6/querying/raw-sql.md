---
title: Consultas SQL brutas - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 99893ca1c634ce6f2e4cf9dcb70b1a1e43532c60
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995728"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas
Entity Framework permite que a consulta usando LINQ com as classes de entidade. No entanto, pode haver ocasiões em que você deseja executar consultas usando SQL bruto diretamente no banco de dados. Isso inclui chamar procedimentos armazenados, que podem ser útil para modelos Code First que atualmente não há suporte para o mapeamento para procedimentos armazenados. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Escrever consultas SQL para entidades  

O método SqlQuery no DbSet permite que uma consulta SQL bruta a ser gravado que irá retornar instâncias da entidade. Os objetos retornados serão rastreados pelo contexto exatamente como eles seriam se elas foram retornadas por uma consulta LINQ. Por exemplo:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Observe que, como acontece com consultas LINQ, a consulta não é executada até que os resultados são enumerados — no exemplo acima, isso é feito com a chamada ToList.  

Tome cuidado sempre que as consultas SQL brutas são gravadas por dois motivos. Em primeiro lugar, a consulta deve ser escrita para garantir que ele retorna apenas as entidades que são realmente do tipo solicitado. Por exemplo, quando o uso de recursos, como a herança é fácil escrever uma consulta que criará entidades que são do tipo errado de CLR.  

Em segundo lugar, alguns tipos de consulta SQL bruto expõem possíveis riscos de segurança, especialmente em torno de ataques de injeção de SQL. Certifique-se de que você use os parâmetros na consulta da forma correta para se proteger contra esses ataques.  

### <a name="loading-entities-from-stored-procedures"></a>Carregando entidades de procedimentos armazenados  

Você pode usar DbSet.SqlQuery para carregar as entidades dos resultados de um procedimento armazenado. Por exemplo, o código a seguir chama o dbo. Procedimento GetBlogs no banco de dados:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Você também pode passar parâmetros para um procedimento armazenado usando a seguinte sintaxe:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Escrever consultas SQL para tipos não são de entidade  

Uma consulta SQL retornar instâncias de qualquer tipo, incluindo tipos primitivos, pode ser criada usando o método SqlQuery da classe de banco de dados. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Os resultados retornados do SqlQuery no banco de dados nunca serão rastreados pelo contexto, mesmo se os objetos são instâncias de um tipo de entidade.  

## <a name="sending-raw-commands-to-the-database"></a>Envio de comandos no banco de dados bruto  

Comandos de consulta não podem ser enviados para o banco de dados usando o método ExecuteSqlCommand no banco de dados. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Observe que todas as alterações feitas aos dados no banco de dados usando ExecuteSqlCommand são opacas para o contexto até que as entidades são carregadas ou recarregadas do banco de dados.  

### <a name="output-parameters"></a>Parâmetros de saída  

Se forem usados parâmetros de saída, seus valores não estará disponíveis até que os resultados foram lidas completamente. Isso é devido ao comportamento subjacente do DbDataReader, consulte [recuperando dados usando um DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.  
