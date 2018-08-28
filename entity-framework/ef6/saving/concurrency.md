---
title: Tratando conflitos de simultaneidade - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997804"
---
# <a name="handling-concurrency-conflicts"></a>Como tratar conflitos de simultaneidade
Otimista de simultaneidade suporá envolve a tentativa de salvar a entidade no banco de dados na esperança de que os dados lá não foram alterados desde a entidade foi carregada. Se for descoberto que os dados foram alterados, em seguida, uma exceção será lançada e você deve resolver o conflito antes de tentar salvar novamente. Este tópico aborda como lidar com essas exceções no Entity Framework. As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

Esta postagem não é o local apropriado para uma discussão completa de simultaneidade otimista. As seções a seguir pressupõem algum conhecimento de resolução de simultaneidade e mostram os padrões para tarefas comuns.  

Muitos desses padrões fazem usam os tópicos discutidos [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md).  

Resolver problemas de simultaneidade quando você estiver usando associações independentes (onde a chave estrangeira não é mapeada para uma propriedade em sua entidade) é muito mais difícil do que quando você estiver usando associações de chave estrangeiras. Portanto, se você pretende fazer a resolução de simultaneidade em seu aplicativo é aconselhável sempre mapear chaves estrangeiras em suas entidades. Todos os exemplos abaixo pressupõem que você está usando associações de chave estrangeiras.  

Um DbUpdateConcurrencyException é gerada por SaveChanges quando uma exceção de simultaneidade otimista é detectada ao tentar salvar uma entidade que usa associações de chave estrangeiras.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Resolver as exceções de simultaneidade otimista com recarregar (wins do banco de dados)  

O método Reload pode ser usado para substituir os valores atuais da entidade com os valores agora no banco de dados. A entidade é, em seguida, geralmente dada de volta para o usuário de alguma forma e ele devem tentar fazer novamente suas alterações e salve novamente. Por exemplo:  

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

É uma boa maneira de simular uma exceção de simultaneidade definir um ponto de interrupção na chamada SaveChanges e, em seguida, modificar uma entidade que está sendo salvo no banco de dados usando outra ferramenta como o SQL Management Studio. Você também pode inserir uma linha antes SaveChanges para atualizar o banco de dados utilizando o SqlCommand. Por exemplo:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

O método de entradas no DbUpdateConcurrencyException retorna as instâncias de DbEntityEntry para as entidades que falha ao atualizar. (Essa propriedade atualmente sempre retorna um valor único para problemas de simultaneidade. Ele pode retornar vários valores para as exceções de atualização geral.) Uma alternativa para algumas situações pode ser para obter entradas para todas as entidades que podem precisar ser recarregados do banco de dados e recarregar de chamada para cada um deles.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Resolver as exceções de simultaneidade otimista como cliente ganha  

O exemplo acima que usa o recarregamento às vezes é chamado de wins do banco de dados ou armazenamento vence porque os valores da entidade são substituídos pelos valores do banco de dados. Às vezes, você poderá fazer o oposto e substituir os valores no banco de dados com os valores atualmente na entidade. Isso é chamado, às vezes, o cliente vence e pode ser feito obtendo os valores atuais do banco de dados e defini-los como os valores originais da entidade. (Consulte [trabalhando com valores de propriedade](~/ef6/saving/change-tracking/property-values.md) para obter informações sobre os valores atuais e originais.) Por exemplo:  

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

Às vezes, você talvez queira combinar os valores atualmente no banco de dados com os valores atualmente na entidade. Isso geralmente requer alguma interação lógica ou de usuário personalizada. Por exemplo, você pode apresentar um formulário ao usuário que contém os valores atuais, os valores no banco de dados, e um padrão definido de valores resolvidos. O usuário, em seguida, edita os valores resolvidos conforme necessário e seria esses valores resolvidos que obterem salvos no banco de dados. Isso pode ser feito usando os objetos DbPropertyValues retornado de CurrentValues e GetDatabaseValues na entrada da entidade. Por exemplo:  

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

O código acima usa instâncias DbPropertyValues para passar ao redor de atual, o banco de dados e valores resolvidos. Às vezes, talvez seja mais fácil usar instâncias de seu tipo de entidade para isso. Isso pode ser feito usando os métodos de DbPropertyValues ToObject e SetValues. Por exemplo:  

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
