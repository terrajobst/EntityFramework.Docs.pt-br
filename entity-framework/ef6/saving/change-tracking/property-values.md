---
title: Trabalhando com valores de propriedade - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119732"
---
# <a name="working-with-property-values"></a>Trabalhando com valores de propriedade
Na maior parte do Entity Framework cuidará de acompanhar o estado, os valores originais e os valores atuais das propriedades de suas instâncias de entidade. No entanto, pode haver alguns casos, como cenários desconectados - onde você deseja exibir ou manipular as informações que o EF tem sobre as propriedades. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Entity Framework mantém registro dos dois valores para cada propriedade de uma entidade rastreada. O valor atual é, como o nome indica, o valor atual da propriedade na entidade. O valor original é o valor que a propriedade tinha quando a entidade foi consultada do banco de dados ou anexada ao contexto.  

Há dois mecanismos gerais para trabalhar com valores de propriedade:  

- O valor de uma única propriedade pode ser obtido de uma forma fortemente tipada usando o método de propriedade.  
- Valores para todas as propriedades de uma entidade podem ser lido em um objeto DbPropertyValues. DbPropertyValues, em seguida, atua como um objeto de dicionário semelhante para permitir que os valores de propriedade a ser lido e definido. Os valores em um objeto DbPropertyValues podem ser definidos de valores em outro objeto DbPropertyValues ou de valores em algum outro objeto, como outra cópia da entidade ou um objeto de transferência de dados simples (DTO).  

As seções a seguir mostram exemplos de como usar ambos os mecanismos acima.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Obtendo e definindo o valor atual ou original de uma propriedade individual  

O exemplo a seguir mostra como o valor atual de uma propriedade pode ler e, em seguida, defina como um novo valor:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Use a propriedade OriginalValue em vez da propriedade CurrentValue para ler ou definir o valor original.  

Observe que o valor retornado é tipado como "objeto", quando uma cadeia de caracteres é usada para especificar o nome da propriedade. Por outro lado, o valor retornado é fortemente tipado se uma expressão lambda é usada.  

Definir o valor da propriedade assim marcará apenas a propriedade como modificado se o novo valor é diferente do valor antigo.  

Quando um valor da propriedade é definido dessa maneira, a alteração é detectada automaticamente mesmo se AutoDetectChanges está desativado.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Obtendo e definindo o valor atual de uma propriedade não mapeado  

O valor atual de uma propriedade que não está mapeado para o banco de dados também pode ser lidos. Um exemplo de uma propriedade não mapeado poderia ser uma propriedade RssLink no Blog. Esse valor pode ser calculado com base no BlogId e, portanto, não precisa ser armazenados no banco de dados. Por exemplo:  

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

O valor atual também pode ser definido se a propriedade expõe um setter.  

Lendo os valores das propriedades não mapeadas é útil ao executar a validação do Entity Framework de propriedades não mapeadas. Pelo mesmo motivo valores atuais podem ler e definir propriedades de entidades que atualmente não estão sendo controladas pelo contexto. Por exemplo:  

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

Observe que os valores originais não estão disponíveis para as propriedades não mapeadas ou para as propriedades de entidades que não estão sendo controladas pelo contexto.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Verificando se uma propriedade é marcada como modificada  

O exemplo a seguir mostra como verificar se uma propriedade individual está marcada como modificada:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Os valores das propriedades modificadas são enviados como atualizações no banco de dados quando SaveChanges for chamado.  

##  <a name="marking-a-property-as-modified"></a>Marcar uma propriedade como modificada  

O exemplo a seguir mostra como forçar uma propriedade individual a ser marcada como modificada:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Marcando uma propriedade como modificada força uma atualização para ser enviar para o banco de dados para a propriedade quando SaveChanges for chamado, mesmo se o valor atual da propriedade é o mesmo que seu valor original.  

Não é possível redefinir uma propriedade individual, a não ser modificada depois que ela foi marcada como modificada no momento. Isso é algo que planejamos dar suporte em uma versão futura.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Ler valores do banco de dados para todas as propriedades de uma entidade, original e atual  

O exemplo a seguir mostra como ler os valores atuais, os valores originais e os valores, na verdade, no banco de dados para todas as propriedades mapeadas de uma entidade.  

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

Os valores atuais são os valores que atualmente contém as propriedades da entidade. Os valores originais são os valores que foram lidos do banco de dados quando a entidade foi consultada. Os valores de banco de dados são os valores conforme eles são armazenados no momento no banco de dados. Obtendo os valores de banco de dados é útil quando os valores no banco de dados podem ter sido alterado desde a entidade foi consultada, como quando uma simultâneas Editar para o banco de dados foi feito por outro usuário.  

## <a name="setting-current-or-original-values-from-another-object"></a>Valores de configuração atuais ou originais de outro objeto  

Os valores atuais ou originais de uma entidade rastreada podem ser atualizados, copiando os valores de outro objeto. Por exemplo:  

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

Executar o código acima imprimirá:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Essa técnica é usada, às vezes, ao atualizar uma entidade com valores obtidos de uma chamada de serviço ou um cliente em um aplicativo de n camadas. Observe que o objeto usado não precisa ser do mesmo tipo que a entidade desde que ela tem propriedades cujos nomes correspondam da entidade. No exemplo acima, uma instância de BlogDTO é usada para atualizar os valores originais.  

Observe que apenas as propriedades que são definidas com valores diferentes quando copiado de outro objeto serão marcadas como modificada.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Valores de configuração atuais ou originais de um dicionário  

Os valores atuais ou originais de uma entidade rastreada podem ser atualizados, copiando os valores de um dicionário ou outra estrutura de dados. Por exemplo:  

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

Use a propriedade OriginalValues em vez da propriedade CurrentValues para definir valores originais.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Valores de configuração atuais ou originais de um dicionário usando a propriedade  

Uma alternativa ao uso CurrentValues ou OriginalValues conforme mostrado acima é usar o método de propriedade para definir o valor de cada propriedade. Isso pode ser preferível quando você precisa definir os valores de propriedades complexas. Por exemplo:  

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

No exemplo acima, propriedades complexas são acessadas usando nomes pontilhados. Para outras maneiras de acessar propriedades complexas, consulte as duas seções neste tópico especificamente sobre propriedades complexas.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Criando um objeto clonado que contém os valores de banco de dados, original ou atual  

O objeto de DbPropertyValues retornado do CurrentValues, OriginalValues, ou GetDatabaseValues pode ser usado para criar um clone da entidade. O clone conterá os valores de propriedade do objeto DbPropertyValues usado para criá-lo. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Observe que o objeto retornado não é a entidade e não está sendo acompanhado pelo contexto. O objeto retornado também não tem nenhuma relação definida como outros objetos.  

O objeto clonado pode ser útil para resolver problemas relacionados a atualizações simultâneas ao banco de dados, especialmente onde uma interface do usuário que envolve a vinculação de dados para objetos de um determinado tipo está sendo usado.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Obtendo e definindo os valores atuais ou originais de propriedades complexas  

O valor de um objeto complexo inteiro pode ser de leitura e set usando o método de propriedade, assim como pode ser para uma propriedade primitiva. Além disso você pode detalhar o objeto complexo e de leitura ou definidas propriedades de objeto, ou até mesmo um objeto aninhado. Estes são alguns exemplos:  

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

Use a propriedade OriginalValue em vez da propriedade CurrentValue para obter ou definir um valor original.  

Observe que a propriedade ou o método ComplexProperty pode ser usado para acessar uma propriedade complexa. No entanto, o método ComplexProperty deve ser usado se você quiser detalhar o objeto complexo com propriedades adicionais ou ComplexProperty chama.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Usando DbPropertyValues para acessar propriedades complexas  

Quando você usa o CurrentValues, OriginalValues ou GetDatabaseValues para obter os atuais, originais, ou valores do banco de dados para uma entidade, os valores de todas as propriedades complexas são retornados como objetos de DbPropertyValues aninhados. Esses aninhados objetos pode então ser usados para obter valores de objeto complexo. Por exemplo, o seguinte método imprimirá os valores de todas as propriedades, incluindo valores de quaisquer propriedades complexas e propriedades aninhadas complexas.  

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

Para imprimir todos os valores de propriedade atuais do método seria chamado assim:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
