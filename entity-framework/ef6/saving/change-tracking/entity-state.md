---
title: Trabalhando com os estados da entidade - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490622"
---
# <a name="working-with-entity-states"></a>Trabalhando com os estados da entidade
Este tópico abordará como adicionar e anexar entidades a um contexto e como o Entity Framework processa esses durante SaveChanges.
Entity Framework se encarrega de acompanhamento para o estado das entidades enquanto eles estiverem conectados a um contexto, mas em cenários desconectados ou de N camadas você pode permitir que o EF sabe que estado suas entidades deve estar no.
As técnicas mostradas neste tópico se aplicam igualmente a modelos criados com o Code First e com o EF Designer.  

## <a name="entity-states-and-savechanges"></a>Estados da entidade e SaveChanges

Uma entidade pode estar em um dos cinco estados, conforme definido pela enumeração EntityState. Esses estados são:  

- Adicionado: a entidade está sendo acompanhada pelo contexto, mas ainda não existe no banco de dados  
- Inalterado: a entidade está sendo acompanhada pelo contexto e existe no banco de dados e seus valores de propriedade não foram alterados entre os valores no banco de dados  
- Modificado: a entidade está sendo acompanhada pelo contexto e existe no banco de dados, e alguns ou todos os seus valores de propriedade foram modificados  
- Excluído: a entidade está sendo acompanhada pelo contexto e existe no banco de dados, mas foi marcada para exclusão do banco de dados na próxima vez em que o SaveChanges é chamado  
- Desanexado: a entidade não está sendo acompanhada pelo contexto  

SaveChanges faz coisas diferentes para entidades em diferentes estados:  

- Entidades inalteradas não são tocadas pelo SaveChanges. Atualizações não são enviadas ao banco de dados para entidades no estado inalterado.  
- Adicionada a entidades são inseridas no banco de dados e, em seguida, se tornam inalteradas quando SaveChanges retorna.  
- Entidades modificadas são atualizadas no banco de dados e, em seguida, se tornam inalteradas quando SaveChanges retorna.  
- Entidades excluídas são excluídas do banco de dados e, em seguida, são desanexadas do contexto.  

Os exemplos a seguir mostram maneiras em que o estado de uma entidade ou um grafo de entidade pode ser alterado.  

## <a name="adding-a-new-entity-to-the-context"></a>Adicionar uma nova entidade ao contexto  

Uma nova entidade pode ser adicionada ao contexto chamando o método Add no DbSet.
Isso coloca a entidade no estado adicionado, que significa que ele será inserido no banco de dados da próxima vez que o SaveChanges é chamado.
Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

É outra maneira de adicionar uma nova entidade ao contexto alterar seu estado como adicionado. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Por fim, você pode adicionar uma nova entidade ao contexto, conectá-lo a outra entidade que já está sendo rastreada.
Isso pode ser, adicionando a nova entidade para a propriedade de navegação de coleção de outra entidade ou definindo uma propriedade de navegação de referência de outra entidade para apontar para a nova entidade. Por exemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Observe que, para todos esses exemplos se a entidade que está sendo adicionada tiver referências a outras entidades que não são rastreadas ainda, em seguida, essas novas entidades também será adicionado ao contexto e será inserido no banco de dados da próxima vez que o SaveChanges é chamado.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Anexar uma entidade existente ao contexto  

Se você tiver uma entidade que você sabe que já existe no banco de dados, mas que não está atualmente sendo acompanhado pelo contexto, em seguida, você pode informar o contexto para controlar a entidade usando o método anexar no DbSet. A entidade será no estado inalterado no contexto. Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Observe que nenhuma alteração será feita no banco de dados se SaveChanges for chamado sem fazer qualquer outra manipulação da entidade anexada. Isso ocorre porque a entidade está no estado inalterado.  

É outra maneira de anexar uma entidade existente ao contexto alterar seu estado como não alterado. Por exemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Observe que para ambos os exemplos se a entidade que está sendo anexada tem referências a outras entidades que ainda não são controladas, em seguida, essas novas entidades serão também anexado ao contexto no estado inalterado.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Anexando um existente, mas a entidade modificada para o contexto  

Se você tiver uma entidade que você sabe que já existe no banco de dados, mas as alterações que talvez tenham sido feitas, em seguida, você pode informar o contexto para anexar a entidade e definir seu estado para modificado.
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

Quando você altera o estado para modificado todas as propriedades da entidade serão marcadas como modificada e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.  

Observe que, se a entidade que está sendo anexada tem referências a outras entidades que ainda não são controladas, essas novas entidades será anexado ao contexto no estado inalterado — eles não serão automaticamente feitos modificado.
Se você tiver várias entidades que precisam ser marcadas modificado deve definir o estado individualmente para cada uma dessas entidades.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Alterando o estado de uma entidade rastreada  

Você pode alterar o estado de uma entidade que já está sendo controlado definindo a propriedade State em sua entrada. Por exemplo:  

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

Observe que o Add ou Attach de chamada para uma entidade que já é controlada também pode ser usada para alterar o estado da entidade. Por exemplo, a anexação de chamada para uma entidade que está atualmente no estado adicionado mudará seu estado como não alterado.  

## <a name="insert-or-update-pattern"></a>Inserir ou atualizar padrão  

Um padrão comum para alguns aplicativos é adicionar uma entidade como novo (resultando em uma inserção de banco de dados) ou anexar uma entidade já existente e marcá-la como modificada (resultando em uma atualização de banco de dados), dependendo do valor da chave primária.
Por exemplo, ao usar chaves primárias do banco de dados gerado inteiro é comum para tratar de uma entidade com uma chave zero como novo e uma entidade com uma chave diferente de zero já existente.
Esse padrão pode ser obtido, definindo o estado da entidade com base em uma verificação do valor de chave primária. Por exemplo:  

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

Observe que, quando você altera o estado para modificado todas as propriedades da entidade serão marcadas como modificada e todos os valores de propriedade serão enviados ao banco de dados quando SaveChanges for chamado.  
