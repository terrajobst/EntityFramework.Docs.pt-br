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
# <a name="raw-sql-queries"></a><span data-ttu-id="a3215-102">Consultas SQL brutas</span><span class="sxs-lookup"><span data-stu-id="a3215-102">Raw SQL Queries</span></span>
<span data-ttu-id="a3215-103">Entity Framework permite consultar usando o LINQ com suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="a3215-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="a3215-104">No entanto, pode haver ocasiões em que você deseja executar consultas usando SQL bruto diretamente no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a3215-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="a3215-105">Isso inclui a chamada de procedimentos armazenados, o que pode ser útil para modelos de Code First que atualmente não dão suporte a mapeamento para procedimentos armazenados.</span><span class="sxs-lookup"><span data-stu-id="a3215-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="a3215-106">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a3215-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="a3215-107">Gravando consultas SQL para entidades</span><span class="sxs-lookup"><span data-stu-id="a3215-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="a3215-108">O método SQLQuery em DbSet permite que uma consulta SQL bruta seja gravada, o que retornará as instâncias de entidade.</span><span class="sxs-lookup"><span data-stu-id="a3215-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="a3215-109">Os objetos retornados serão acompanhados pelo contexto, exatamente como seriam se fossem retornados por uma consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="a3215-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="a3215-110">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a3215-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="a3215-111">Observe que, assim como nas consultas LINQ, a consulta não é executada até que os resultados sejam enumerados — no exemplo acima, isso é feito com a chamada para ToList.</span><span class="sxs-lookup"><span data-stu-id="a3215-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="a3215-112">Deve-se ter cuidado sempre que as consultas SQL brutas são gravadas por dois motivos.</span><span class="sxs-lookup"><span data-stu-id="a3215-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="a3215-113">Primeiro, a consulta deve ser escrita para garantir que ela retorne apenas entidades que realmente sejam do tipo solicitado.</span><span class="sxs-lookup"><span data-stu-id="a3215-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="a3215-114">Por exemplo, ao usar recursos como herança, é fácil escrever uma consulta que criará entidades que sejam do tipo CLR errado.</span><span class="sxs-lookup"><span data-stu-id="a3215-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="a3215-115">Em segundo lugar, alguns tipos de consultas SQL brutas expõem possíveis riscos de segurança, especialmente em relação a ataques de injeção de SQL.</span><span class="sxs-lookup"><span data-stu-id="a3215-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="a3215-116">Certifique-se de usar parâmetros em sua consulta na maneira correta de proteger contra tais ataques.</span><span class="sxs-lookup"><span data-stu-id="a3215-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="a3215-117">Carregando entidades de procedimentos armazenados</span><span class="sxs-lookup"><span data-stu-id="a3215-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="a3215-118">Você pode usar DbSet. SQLQuery para carregar entidades dos resultados de um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="a3215-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="a3215-119">Por exemplo, o código a seguir chama o dbo. Procedimento getblogs no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="a3215-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="a3215-120">Você também pode passar parâmetros para um procedimento armazenado usando a seguinte sintaxe:</span><span class="sxs-lookup"><span data-stu-id="a3215-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="a3215-121">Gravando consultas SQL para tipos que não são de entidade</span><span class="sxs-lookup"><span data-stu-id="a3215-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="a3215-122">Uma consulta SQL retornando instâncias de qualquer tipo, incluindo tipos primitivos, pode ser criada usando o método SQLQuery na classe Database.</span><span class="sxs-lookup"><span data-stu-id="a3215-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="a3215-123">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a3215-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="a3215-124">Os resultados retornados de SQLQuery no banco de dados nunca serão acompanhados pelo contexto, mesmo se os objetos forem instâncias de um tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="a3215-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="a3215-125">Enviando comandos brutos para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="a3215-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="a3215-126">Os comandos que não são de consulta podem ser enviados ao banco de dados usando o método ExecuteSqlCommand no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a3215-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="a3215-127">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a3215-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="a3215-128">Observe que todas as alterações feitas nos dados no banco usando ExecuteSqlCommand são opacas para o contexto até que as entidades sejam carregadas ou recarregadas do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a3215-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="a3215-129">Parâmetros de saída</span><span class="sxs-lookup"><span data-stu-id="a3215-129">Output Parameters</span></span>  

<span data-ttu-id="a3215-130">Se os parâmetros de saída forem usados, seus valores não estarão disponíveis até que os resultados tenham sido completamente lidos.</span><span class="sxs-lookup"><span data-stu-id="a3215-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="a3215-131">Isso ocorre devido ao comportamento subjacente de DbDataReader, consulte [recuperando dados usando um DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="a3215-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
