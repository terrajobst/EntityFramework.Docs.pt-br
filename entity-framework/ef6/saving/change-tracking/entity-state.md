---
title: Trabalhando com Estados de entidade – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419695"
---
# <a name="working-with-entity-states"></a>Trabalhando com Estados de entidade
Este tópico abordará como adicionar e anexar entidades a um contexto e como o Entity Framework os processa durante o SaveChanges.
Entity Framework cuida do acompanhamento do estado das entidades enquanto elas estão conectadas a um contexto, mas em cenários desconectados ou de N camadas, você pode permitir que o EF saiba em que estado suas entidades devem estar.
As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="entity-states-and-savechanges"></a>Estados de entidades e SaveChanges

Uma entidade pode estar em um dos cinco Estados, conforme definido pela Enumeração EntityState. Esses estados são:  

- Adicionado: a entidade está sendo rastreada pelo contexto, mas ainda não existe no banco de dados  
- Inalterado: a entidade está sendo rastreada pelo contexto e existe no banco de dados e seus valores de propriedade não foram alterados dos valores no banco de dados  
- Modified: a entidade está sendo rastreada pelo contexto e existe no banco de dados e alguns ou todos os seus valores de propriedade foram modificados  
- Excluído: a entidade está sendo rastreada pelo contexto e existe no banco de dados, mas foi marcada para exclusão do banco de dados na próxima vez que SaveChanges for chamado  
- Desanexado: a entidade não está sendo acompanhada pelo contexto  

SaveChanges faz diferentes coisas para entidades em Estados diferentes:  

- As entidades inalteradas não são utilizadas pelo SaveChanges. As atualizações não são enviadas ao banco de dados para entidades no estado inalterado.  
- As entidades adicionadas são inseridas no banco de dados e ficam inalteradas quando o SaveChanges retorna.  
- As entidades modificadas são atualizadas no banco de dados e ficam inalteradas quando o SaveChanges retorna.  
- As entidades excluídas são excluídas do banco de dados e, em seguida, desanexadas do contexto.  

Os exemplos a seguir mostram maneiras em que o estado de uma entidade ou um grafo de entidade pode ser alterado.  

## <a name="adding-a-new-entity-to-the-context"></a>Adicionando uma nova entidade ao contexto  

Uma nova entidade pode ser adicionada ao contexto chamando o método Add em DbSet.
Isso coloca a entidade no estado adicionado, o que significa que ela será inserida no banco de dados na próxima vez que SaveChanges for chamado.
Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Outra maneira de adicionar uma nova entidade ao contexto é alterar seu estado para adicionado. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Por fim, você pode adicionar uma nova entidade ao contexto, vinculando-a a outra entidade que já está sendo controlada.
Isso pode ser adicionando a nova entidade à propriedade de navegação da coleção de outra entidade ou definindo uma propriedade de navegação de referência de outra entidade para apontar para a nova entidade. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Observe que, para todos esses exemplos, se a entidade que está sendo adicionada tiver referências a outras entidades que ainda não foram rastreadas, essas novas entidades também serão adicionadas ao contexto e serão inseridas no banco de dados na próxima vez que SaveChanges for chamado.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Anexando uma entidade existente ao contexto  

Se você tiver uma entidade que já existe no banco de dados, mas que não está sendo controlada no momento pelo contexto, você poderá dizer ao contexto para rastrear a entidade usando o método Attach em DbSet. A entidade estará no estado inalterado no contexto. Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Observe que nenhuma alteração será feita no banco de dados se SaveChanges for chamado sem fazer nenhuma outra manipulação da entidade anexada. Isso ocorre porque a entidade está no estado inalterado.  

Outra maneira de anexar uma entidade existente ao contexto é alterar seu estado para inalterado. Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Observe que, para ambos os exemplos, se a entidade que está sendo anexada tiver referências a outras entidades que ainda não foram rastreadas, essas novas entidades também serão anexadas ao contexto no estado inalterado.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Anexando uma entidade existente, mas modificada, ao contexto  

Se você tiver uma entidade que já existe no banco de dados, mas para as quais as alterações podem ter sido feitas, você pode dizer ao contexto para anexar a entidade e definir seu estado como modificado.
Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Quando você altera o estado para modificado, todas as propriedades da entidade serão marcadas como modificadas e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.  

Observe que, se a entidade que está sendo anexada tiver referências a outras entidades que ainda não foram acompanhadas, essas novas entidades serão anexadas ao contexto no estado inalterado — elas não serão automaticamente modificadas.
Se você tiver várias entidades que precisam ser marcadas como modificadas, defina o estado para cada uma dessas entidades individualmente.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Alterando o estado de uma entidade rastreada  

Você pode alterar o estado de uma entidade que já está sendo rastreada definindo a propriedade State em sua entrada. Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Observe que chamar adicionar ou anexar para uma entidade que já está acompanhada também pode ser usado para alterar o estado da entidade. Por exemplo, chamar attach para uma entidade que está atualmente no estado adicionado alterará seu estado para inalterado.  

## <a name="insert-or-update-pattern"></a>Inserir ou atualizar padrão  

Um padrão comum para alguns aplicativos é adicionar uma entidade como nova (resultando em uma inserção de banco de dados) ou anexar uma entidade como existente e marcá-la como modificada (resultando em uma atualização de banco de dados), dependendo do valor da chave primária.
Por exemplo, ao usar chaves primárias de inteiro geradas pelo banco de dados, é comum tratar uma entidade com uma chave zero como New e uma entidade com uma chave diferente de zero como existente.
Esse padrão pode ser obtido definindo o estado da entidade com base em uma verificação do valor da chave primária. Por exemplo:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Observe que, quando você altera o estado para modificado, todas as propriedades da entidade serão marcadas como modificadas e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.  
