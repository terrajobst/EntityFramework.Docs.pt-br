---
title: Consultas SQL brutas – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417091"
---
# <a name="raw-sql-queries"></a>Consultas SQL brutas
Entity Framework permite consultar usando o LINQ com suas classes de entidade. No entanto, pode haver ocasiões em que você deseja executar consultas usando SQL bruto diretamente no banco de dados. Isso inclui a chamada de procedimentos armazenados, o que pode ser útil para modelos de Code First que atualmente não dão suporte a mapeamento para procedimentos armazenados. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Gravando consultas SQL para entidades  

O método SQLQuery em DbSet permite que uma consulta SQL bruta seja gravada, o que retornará as instâncias de entidade. Os objetos retornados serão acompanhados pelo contexto, exatamente como seriam se fossem retornados por uma consulta LINQ. Por exemplo:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Observe que, assim como nas consultas LINQ, a consulta não é executada até que os resultados sejam enumerados — no exemplo acima, isso é feito com a chamada para ToList.  

Deve-se ter cuidado sempre que as consultas SQL brutas são gravadas por dois motivos. Primeiro, a consulta deve ser escrita para garantir que ela retorne apenas entidades que realmente sejam do tipo solicitado. Por exemplo, ao usar recursos como herança, é fácil escrever uma consulta que criará entidades que sejam do tipo CLR errado.  

Em segundo lugar, alguns tipos de consultas SQL brutas expõem possíveis riscos de segurança, especialmente em relação a ataques de injeção de SQL. Certifique-se de usar parâmetros em sua consulta na maneira correta de proteger contra tais ataques.  

### <a name="loading-entities-from-stored-procedures"></a>Carregando entidades de procedimentos armazenados  

Você pode usar DbSet. SQLQuery para carregar entidades dos resultados de um procedimento armazenado. Por exemplo, o código a seguir chama o dbo. Procedimento getblogs no banco de dados:  

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

## <a name="writing-sql-queries-for-non-entity-types"></a>Gravando consultas SQL para tipos que não são de entidade  

Uma consulta SQL retornando instâncias de qualquer tipo, incluindo tipos primitivos, pode ser criada usando o método SQLQuery na classe Database. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Os resultados retornados de SQLQuery no banco de dados nunca serão acompanhados pelo contexto, mesmo se os objetos forem instâncias de um tipo de entidade.  

## <a name="sending-raw-commands-to-the-database"></a>Enviando comandos brutos para o banco de dados  

Os comandos que não são de consulta podem ser enviados ao banco de dados usando o método ExecuteSqlCommand no banco de dados. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Observe que todas as alterações feitas nos dados no banco usando ExecuteSqlCommand são opacas para o contexto até que as entidades sejam carregadas ou recarregadas do banco de dados.  

### <a name="output-parameters"></a>Parâmetros de saída  

Se os parâmetros de saída forem usados, seus valores não estarão disponíveis até que os resultados tenham sido completamente lidos. Isso ocorre devido ao comportamento subjacente de DbDataReader, consulte [recuperando dados usando um DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.  
