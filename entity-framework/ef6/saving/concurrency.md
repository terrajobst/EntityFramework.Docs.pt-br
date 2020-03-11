---
title: Lidando com conflitos de simultaneidade – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419688"
---
# <a name="handling-concurrency-conflicts"></a>Como tratar conflitos de simultaneidade
A simultaneidade otimista envolve a tentativa otimista de salvar sua entidade no banco de dados, na esperança de que eles não tenham sido alterados desde que a entidade foi carregada. Se a alteração dos dados for alterada, uma exceção será lançada e você deverá resolver o conflito antes de tentar salvar novamente. Este tópico aborda como lidar com essas exceções no Entity Framework. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Esta postagem não é o local apropriado para uma discussão completa sobre a simultaneidade otimista. As seções a seguir pressupõem algum conhecimento de resolução de simultaneidade e mostram padrões para tarefas comuns.  

Muitos desses padrões fazem uso dos tópicos discutidos em [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md).  

Resolver problemas de simultaneidade quando você está usando associações independentes (em que a chave estrangeira não está mapeada para uma propriedade em sua entidade) é muito mais difícil do que quando você está usando associações de chave estrangeira. Portanto, se você for fazer a resolução de simultaneidade em seu aplicativo, é recomendável que você sempre mapeie chaves estrangeiras em suas entidades. Todos os exemplos a seguir pressupõem que você está usando associações de chave estrangeira.  

Um DbUpdateConcurrencyException é gerado por SaveChanges quando uma exceção de simultaneidade otimista é detectada ao tentar salvar uma entidade que usa associações de chave estrangeira.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Resolvendo exceções de simultaneidade otimista com recarregamento (banco de dados vence)  

O método reload pode ser usado para substituir os valores atuais da entidade pelos valores agora no banco de dados. Em seguida, a entidade é, em seguida, retornada ao usuário de alguma forma e deve tentar fazer suas alterações novamente e salvá-la novamente. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Uma boa maneira de simular uma exceção de simultaneidade é definir um ponto de interrupção na chamada SaveChanges e, em seguida, modificar uma entidade que está sendo salva no banco de dados usando outra ferramenta, como o SQL Management Studio. Você também pode inserir uma linha antes de SaveChanges para atualizar o banco de dados diretamente usando SqlCommand. Por exemplo:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

O método Entries em DbUpdateConcurrencyException retorna as instâncias de DbEntityEntry para as entidades que não foram atualizadas. (Essa propriedade sempre retorna um único valor para problemas de simultaneidade. Ele pode retornar vários valores para exceções de atualização gerais.) Uma alternativa para algumas situações pode ser obter entradas para todas as entidades que talvez precisem ser recarregadas do banco de dados e chamar o recarregamento para cada uma delas.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Resolvendo exceções de simultaneidade otimista como cliente ganha  

O exemplo acima que usa recarregamento às vezes é chamado de banco de dados WINS ou armazenamento vence porque os valores na entidade são substituídos por valores do banco de dados. Às vezes, você pode querer fazer o oposto e substituir os valores no banco de dados pelos valores atualmente na entidade. Isso às vezes é chamado de cliente WINS e pode ser feito obtendo os valores de banco de dados atuais e definindo-os como os valores originais para a entidade. (Consulte [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md) para obter informações sobre os valores atuais e originais.) Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Resolução personalizada de exceções de simultaneidade otimista  

Às vezes, talvez você queira combinar os valores atualmente no banco de dados com os valores atualmente na entidade. Isso geralmente requer uma lógica personalizada ou interação do usuário. Por exemplo, você pode apresentar um formulário ao usuário que contém os valores atuais, os valores no banco de dados e um conjunto padrão de valores resolvidos. O usuário editaria os valores resolvidos conforme necessário e seria esses valores resolvidos que são salvos no banco de dados. Isso pode ser feito usando os objetos DbPropertyValues retornados de CurrentValues e GetDatabaseValues na entrada da entidade. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Resolução personalizada de exceções de simultaneidade otimista usando objetos  

O código acima usa instâncias de DbPropertyValues para passar pelos valores atuais, de banco de dados e resolvidos. Às vezes, pode ser mais fácil usar instâncias do tipo de entidade para isso. Isso pode ser feito usando os métodos ToObject e SetValues de DbPropertyValues. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
