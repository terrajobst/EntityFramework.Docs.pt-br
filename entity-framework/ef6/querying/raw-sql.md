---
title: Consultas SQL brutas - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283778"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="33c65-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="33c65-102">Raw SQL Queries</span></span>
<span data-ttu-id="33c65-103">Entity Framework permite que a consulta usando LINQ com as classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="33c65-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="33c65-104">No entanto, pode haver ocasiões em que você deseja executar consultas usando SQL bruto diretamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33c65-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="33c65-105">Isso inclui chamar procedimentos armazenados, que podem ser útil para modelos Code First que atualmente não há suporte para o mapeamento para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="33c65-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="33c65-106">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="33c65-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="33c65-107">Escrever consultas SQL para entidades</span><span class="sxs-lookup"><span data-stu-id="33c65-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="33c65-108">O método SqlQuery no DbSet permite que uma consulta SQL bruta a ser gravado que irá retornar instâncias da entidade.</span><span class="sxs-lookup"><span data-stu-id="33c65-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="33c65-109">Os objetos retornados serão rastreados pelo contexto exatamente como eles seriam se elas foram retornadas por uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="33c65-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="33c65-110">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="33c65-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="33c65-111">Observe que, como acontece com consultas LINQ, a consulta não é executada até que os resultados são enumerados — no exemplo acima, isso é feito com a chamada ToList.</span><span class="sxs-lookup"><span data-stu-id="33c65-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="33c65-112">Tome cuidado sempre que as consultas SQL brutas são gravadas por dois motivos.</span><span class="sxs-lookup"><span data-stu-id="33c65-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="33c65-113">Em primeiro lugar, a consulta deve ser escrita para garantir que ele retorna apenas as entidades que são realmente do tipo solicitado.</span><span class="sxs-lookup"><span data-stu-id="33c65-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="33c65-114">Por exemplo, quando o uso de recursos, como a herança é fácil escrever uma consulta que criará entidades que são do tipo errado de CLR.</span><span class="sxs-lookup"><span data-stu-id="33c65-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="33c65-115">Em segundo lugar, alguns tipos de consulta SQL bruto expõem possíveis riscos de segurança, especialmente em torno de ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="33c65-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="33c65-116">Certifique-se de que você use os parâmetros na consulta da forma correta para se proteger contra esses ataques.</span><span class="sxs-lookup"><span data-stu-id="33c65-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="33c65-117">Carregando entidades de procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="33c65-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="33c65-118">Você pode usar DbSet.SqlQuery para carregar as entidades dos resultados de um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="33c65-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="33c65-119">Por exemplo, o código a seguir chama o dbo. Procedimento GetBlogs no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="33c65-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="33c65-120">Você também pode passar parâmetros para um procedimento armazenado usando a seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="33c65-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="33c65-121">Escrever consultas SQL para tipos não são de entidade</span><span class="sxs-lookup"><span data-stu-id="33c65-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="33c65-122">Uma consulta SQL retornar instâncias de qualquer tipo, incluindo tipos primitivos, pode ser criada usando o método SqlQuery da classe de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33c65-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="33c65-123">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="33c65-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="33c65-124">Os resultados retornados do SqlQuery no banco de dados nunca serão rastreados pelo contexto, mesmo se os objetos são instâncias de um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="33c65-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="33c65-125">Envio de comandos no banco de dados bruto</span><span class="sxs-lookup"><span data-stu-id="33c65-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="33c65-126">Comandos de consulta não podem ser enviados para o banco de dados usando o método ExecuteSqlCommand no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33c65-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="33c65-127">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="33c65-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="33c65-128">Observe que todas as alterações feitas aos dados no banco de dados usando ExecuteSqlCommand são opacas para o contexto até que as entidades são carregadas ou recarregadas do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="33c65-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="33c65-129">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="33c65-129">Output Parameters</span></span>  

<span data-ttu-id="33c65-130">Se forem usados parâmetros de saída, seus valores não estará disponíveis até que os resultados foram lidas completamente.</span><span class="sxs-lookup"><span data-stu-id="33c65-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="33c65-131">Isso é devido ao comportamento subjacente do DbDataReader, consulte [recuperando dados usando um DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="33c65-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
