---
title: Trabalhando com valores de propriedade-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182138"
---
# <a name="working-with-property-values"></a><span data-ttu-id="cfd93-102">Trabalhando com valores de propriedade</span><span class="sxs-lookup"><span data-stu-id="cfd93-102">Working with property values</span></span>
<span data-ttu-id="cfd93-103">Para a maior parte Entity Framework cuidará do estado, dos valores originais e dos valores atuais das propriedades de suas instâncias de entidade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="cfd93-104">No entanto, pode haver alguns casos, como cenários desconectados, onde você deseja exibir ou manipular as informações que o EF tem sobre as propriedades.</span><span class="sxs-lookup"><span data-stu-id="cfd93-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="cfd93-105">As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="cfd93-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="cfd93-106">Entity Framework mantém o controle de dois valores para cada propriedade de uma entidade rastreada.</span><span class="sxs-lookup"><span data-stu-id="cfd93-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="cfd93-107">O valor atual é, como o nome indica, o valor atual da propriedade na entidade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="cfd93-108">O valor original é o valor que a propriedade tinha quando a entidade foi consultada do banco de dados ou anexada ao contexto.</span><span class="sxs-lookup"><span data-stu-id="cfd93-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="cfd93-109">Há dois mecanismos gerais para trabalhar com valores de propriedade:</span><span class="sxs-lookup"><span data-stu-id="cfd93-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="cfd93-110">O valor de uma única propriedade pode ser obtido de forma fortemente tipada usando o método Property.</span><span class="sxs-lookup"><span data-stu-id="cfd93-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="cfd93-111">Os valores para todas as propriedades de uma entidade podem ser lidos em um objeto DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="cfd93-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="cfd93-112">DbPropertyValues, em seguida, atua como um objeto do tipo Dictionary para permitir que os valores de propriedade sejam lidos e definidos.</span><span class="sxs-lookup"><span data-stu-id="cfd93-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="cfd93-113">Os valores em um objeto DbPropertyValues podem ser definidos a partir de valores em outro objeto DbPropertyValues ou de valores em algum outro objeto, como outra cópia da entidade ou um DTO (objeto de transferência de dados simples).</span><span class="sxs-lookup"><span data-stu-id="cfd93-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="cfd93-114">As seções a seguir mostram exemplos de como usar os mecanismos acima.</span><span class="sxs-lookup"><span data-stu-id="cfd93-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="cfd93-115">Obter e definir o valor atual ou original de uma propriedade individual</span><span class="sxs-lookup"><span data-stu-id="cfd93-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="cfd93-116">O exemplo a seguir mostra como o valor atual de uma propriedade pode ser lido e, em seguida, definido como um novo valor:</span><span class="sxs-lookup"><span data-stu-id="cfd93-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="cfd93-117">Use a propriedade OriginalValue em vez da Propriedade CurrentValue para ler ou definir o valor original.</span><span class="sxs-lookup"><span data-stu-id="cfd93-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="cfd93-118">Observe que o valor retornado é digitado como "Object" quando uma cadeia de caracteres é usada para especificar o nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="cfd93-119">Por outro lado, o valor retornado será fortemente tipado se uma expressão lambda for usada.</span><span class="sxs-lookup"><span data-stu-id="cfd93-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="cfd93-120">Definir o valor da propriedade como isso só marcará a propriedade como modificada se o novo valor for diferente do valor antigo.</span><span class="sxs-lookup"><span data-stu-id="cfd93-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="cfd93-121">Quando um valor de propriedade é definido dessa maneira, a alteração é detectada automaticamente mesmo se AutoDetectChanges estiver desativado.</span><span class="sxs-lookup"><span data-stu-id="cfd93-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="cfd93-122">Obter e definir o valor atual de uma propriedade não mapeada</span><span class="sxs-lookup"><span data-stu-id="cfd93-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="cfd93-123">O valor atual de uma propriedade que não está mapeada para o banco de dados também pode ser lido.</span><span class="sxs-lookup"><span data-stu-id="cfd93-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="cfd93-124">Um exemplo de uma propriedade não mapeada pode ser uma propriedade RssLink no blog.</span><span class="sxs-lookup"><span data-stu-id="cfd93-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="cfd93-125">Esse valor pode ser calculado com base no blogid e, portanto, não precisa ser armazenado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cfd93-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="cfd93-126">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="cfd93-127">O valor atual também pode ser definido se a propriedade expõe um setter.</span><span class="sxs-lookup"><span data-stu-id="cfd93-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="cfd93-128">Ler os valores de propriedades não mapeadas é útil ao executar Entity Framework validação de propriedades não mapeadas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="cfd93-129">Pelo mesmo motivo, os valores atuais podem ser lidos e definidos para propriedades de entidades que não estão sendo rastreadas no momento pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="cfd93-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="cfd93-130">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="cfd93-131">Observe que os valores originais não estão disponíveis para propriedades não mapeadas ou para propriedades de entidades que não estão sendo controladas pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="cfd93-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="cfd93-132">Verificando se uma propriedade está marcada como modificada</span><span class="sxs-lookup"><span data-stu-id="cfd93-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="cfd93-133">O exemplo a seguir mostra como verificar se uma propriedade individual está marcada como modificada:</span><span class="sxs-lookup"><span data-stu-id="cfd93-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="cfd93-134">Os valores das propriedades modificadas são enviados como atualizações para o banco de dados quando SaveChanges é chamado.</span><span class="sxs-lookup"><span data-stu-id="cfd93-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="cfd93-135">Marcando uma propriedade como modificada</span><span class="sxs-lookup"><span data-stu-id="cfd93-135">Marking a property as modified</span></span>  

<span data-ttu-id="cfd93-136">O exemplo a seguir mostra como forçar uma propriedade individual a ser marcada como modificada:</span><span class="sxs-lookup"><span data-stu-id="cfd93-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="cfd93-137">Marcar uma propriedade como modificada forçará a envio de uma atualização para o banco de dados para a propriedade quando SaveChanges for chamado mesmo se o valor atual da propriedade for igual ao seu valor original.</span><span class="sxs-lookup"><span data-stu-id="cfd93-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="cfd93-138">No momento, não é possível redefinir uma propriedade individual para que ela não seja modificada após ser marcada como modificada.</span><span class="sxs-lookup"><span data-stu-id="cfd93-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="cfd93-139">Isso é algo que planejamos dar suporte em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="cfd93-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="cfd93-140">Lendo os valores atuais, originais e de banco de dados para todas as propriedades de uma entidade</span><span class="sxs-lookup"><span data-stu-id="cfd93-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="cfd93-141">O exemplo a seguir mostra como ler os valores atuais, os valores originais e os valores na verdade no banco de dados para todas as propriedades mapeadas de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="cfd93-142">Os valores atuais são os valores que as propriedades da entidade contêm atualmente.</span><span class="sxs-lookup"><span data-stu-id="cfd93-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="cfd93-143">Os valores originais são os valores que foram lidos do banco de dados quando a entidade foi consultada.</span><span class="sxs-lookup"><span data-stu-id="cfd93-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="cfd93-144">Os valores de banco de dados são os valores que estão atualmente armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cfd93-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="cfd93-145">Obter os valores de banco de dados é útil quando os valores no banco de dados podem ter sido alterados desde que a entidade foi consultada, como quando uma edição simultânea no banco de dados foi feita por outro usuário.</span><span class="sxs-lookup"><span data-stu-id="cfd93-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="cfd93-146">Definindo valores atuais ou originais de outro objeto</span><span class="sxs-lookup"><span data-stu-id="cfd93-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="cfd93-147">Os valores atuais ou originais de uma entidade rastreada podem ser atualizados copiando valores de outro objeto.</span><span class="sxs-lookup"><span data-stu-id="cfd93-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="cfd93-148">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="cfd93-149">A execução do código acima será impressa:</span><span class="sxs-lookup"><span data-stu-id="cfd93-149">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="cfd93-150">Essa técnica às vezes é usada ao atualizar uma entidade com valores obtidos de uma chamada de serviço ou um cliente em um aplicativo de n camadas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="cfd93-151">Observe que o objeto usado não precisa ser do mesmo tipo que a entidade, desde que ela tenha propriedades cujos nomes correspondam àquelas da entidade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="cfd93-152">No exemplo acima, uma instância de BlogDTO é usada para atualizar os valores originais.</span><span class="sxs-lookup"><span data-stu-id="cfd93-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="cfd93-153">Observe que somente as propriedades definidas para valores diferentes quando copiadas do outro objeto serão marcadas como modificadas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="cfd93-154">Configurando valores atuais ou originais de um dicionário</span><span class="sxs-lookup"><span data-stu-id="cfd93-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="cfd93-155">Os valores atuais ou originais de uma entidade rastreada podem ser atualizados copiando valores de um dicionário ou de alguma outra estrutura de dados.</span><span class="sxs-lookup"><span data-stu-id="cfd93-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="cfd93-156">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="cfd93-157">Use a Propriedade OriginalValues em vez da propriedade CurrentValues para definir valores originais.</span><span class="sxs-lookup"><span data-stu-id="cfd93-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="cfd93-158">Definindo valores atuais ou originais de um dicionário usando a propriedade</span><span class="sxs-lookup"><span data-stu-id="cfd93-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="cfd93-159">Uma alternativa ao uso de CurrentValues ou OriginalValues, conforme mostrado acima, é usar o método Property para definir o valor de cada propriedade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="cfd93-160">Isso pode ser preferível quando você precisa definir os valores de propriedades complexas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="cfd93-161">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="cfd93-162">No exemplo acima, as propriedades complexas são acessadas usando nomes pontilhados.</span><span class="sxs-lookup"><span data-stu-id="cfd93-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="cfd93-163">Para outras maneiras de acessar propriedades complexas, consulte as duas seções mais adiante neste tópico especificamente sobre propriedades complexas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="cfd93-164">Criando um objeto clonado contendo valores atuais, originais ou de banco de dados</span><span class="sxs-lookup"><span data-stu-id="cfd93-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="cfd93-165">O objeto DbPropertyValues retornado de CurrentValues, OriginalValues ou GetDatabaseValues pode ser usado para criar um clone da entidade.</span><span class="sxs-lookup"><span data-stu-id="cfd93-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="cfd93-166">Esse clone conterá os valores de Propriedade do objeto DbPropertyValues usado para criá-lo.</span><span class="sxs-lookup"><span data-stu-id="cfd93-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="cfd93-167">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cfd93-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="cfd93-168">Observe que o objeto retornado não é a entidade e não está sendo acompanhado pelo contexto.</span><span class="sxs-lookup"><span data-stu-id="cfd93-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="cfd93-169">O objeto retornado também não tem nenhuma relação definida para outros objetos.</span><span class="sxs-lookup"><span data-stu-id="cfd93-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="cfd93-170">O objeto clonado pode ser útil para resolver problemas relacionados a atualizações simultâneas para o banco de dados, especialmente quando uma interface do usuário que envolve a vinculação de dado a objetos de um determinado tipo está sendo usada.</span><span class="sxs-lookup"><span data-stu-id="cfd93-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="cfd93-171">Obtendo e definindo os valores atuais ou originais de propriedades complexas</span><span class="sxs-lookup"><span data-stu-id="cfd93-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="cfd93-172">O valor de um objeto complexo inteiro pode ser lido e definido usando o método de propriedade da mesma forma que pode ser para uma propriedade primitiva.</span><span class="sxs-lookup"><span data-stu-id="cfd93-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="cfd93-173">Além disso, você pode fazer uma busca detalhada do objeto complexo e ler ou definir as propriedades desse objeto, ou até mesmo um objeto aninhado.</span><span class="sxs-lookup"><span data-stu-id="cfd93-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="cfd93-174">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="cfd93-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="cfd93-175">Use a propriedade OriginalValue em vez da Propriedade CurrentValue para obter ou definir um valor original.</span><span class="sxs-lookup"><span data-stu-id="cfd93-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="cfd93-176">Observe que a propriedade ou o método ComplexProperty pode ser usado para acessar uma propriedade complexa.</span><span class="sxs-lookup"><span data-stu-id="cfd93-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="cfd93-177">No entanto, o método ComplexProperty deve ser usado se você quiser fazer uma busca detalhada no objeto complexo com uma propriedade adicional ou chamadas deproperty complexas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="cfd93-178">Usando DbPropertyValues para acessar propriedades complexas</span><span class="sxs-lookup"><span data-stu-id="cfd93-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="cfd93-179">Quando você usa CurrentValues, OriginalValues ou GetDatabaseValues para obter todos os valores atuais, originais ou de banco de dados para uma entidade, os valores de todas as propriedades complexas são retornados como objetos DbPropertyValues aninhados.</span><span class="sxs-lookup"><span data-stu-id="cfd93-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="cfd93-180">Esses objetos aninhados podem ser usados para obter valores do objeto complexo.</span><span class="sxs-lookup"><span data-stu-id="cfd93-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="cfd93-181">Por exemplo, o método a seguir imprimirá os valores de todas as propriedades, incluindo valores de qualquer propriedade complexa e propriedades complexas aninhadas.</span><span class="sxs-lookup"><span data-stu-id="cfd93-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="cfd93-182">Para imprimir todos os valores de propriedade atuais, o método seria chamado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="cfd93-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
