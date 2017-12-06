---
title: Colocar em cascata Delete - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>Excluir em cascata

Exclusão em cascata é geralmente usado na terminologia de banco de dados para descrever uma característica que permite a exclusão de uma linha para disparar automaticamente a exclusão de linhas relacionadas. Um conceito relacionado também coberto por comportamentos de exclusão de EF principal é a exclusão automática de uma entidade filho quando ela é a relação a um pai tem foi desfeita – este i conhecido como "excluir órfãos".

EF Core implementa vários comportamentos de exclusão diferente e permite a configuração dos comportamentos de exclusão de relações individuais. EF principal também implementa convenções que configuram automaticamente os comportamentos de exclusão padrão útil para cada relação com base em [requiredness da relação] (... /Modeling/Relationships.MD#Required-and-Optional-Relationships).

## <a name="delete-behaviors"></a>Excluir comportamentos
Excluir comportamentos são definidos no *DeleteBehavior* enumerador de tipo e pode ser passado para o *OnDelete* API fluente para controlar se a exclusão de uma entidade principal/pai ou o corte do relação com entidades dependentes/filho deve ter um efeito colateral nas entidades dependentes/filho.

Há três ações que EF pode ser executadas quando uma entidade principal/pai é excluída ou a relação para o filho é desligada:
* O filho/dependente pode ser excluído.
* Os valores de chave estrangeira do filho podem ser definidos como null
* O filho permanece inalterado

> [!NOTE]  
> O comportamento de exclusão configurado no modelo de núcleo EF só é aplicado quando a entidade principal é excluída usando EF Core e entidades dependentes são carregadas na memória (ou seja, para controladas dependentes). Um comportamento em cascata correspondente deve ser a instalação no banco de dados para garantir que não está sendo controlados pelo contexto de dados tem a ação necessária aplicada. Se você usar o EF principal para criar o banco de dados, esse comportamento em cascata será instalado para você.

Para a segunda ação acima, definir um valor de chave estrangeiro como null não é válido se a chave estrangeira não é anulável. (Uma chave estrangeira não anuláveis é equivalente a uma relação necessária.) Nesses casos, Core EF controla se a propriedade de chave estrangeira foi marcada como nulo até SaveChanges é chamado, o momento em que uma exceção será lançada porque as alterações não podem ser mantidas no banco de dados. Isso é semelhante à obtenção de uma violação de restrição do banco de dados.

Existem em quatro excluir comportamentos, conforme listado nas tabelas a seguir. Para relações opcionais (chave estrangeira anulável)- _é_ possível salvar um nulo valor de chave estrangeiro, que resulta nos seguintes efeitos:

| Nome de comportamento | Efeito em dependente/filho na memória | Efeito em dependente/filho no banco de dados
|-|-|-
| **Em cascata** | Entidades são excluídas | Entidades são excluídas
| **ClientSetNull** (padrão) | Propriedades de chave estrangeira são definidas como null | Nenhum
| **SetNull** | Propriedades de chave estrangeira são definidas como null | Propriedades de chave estrangeira são definidas como null
| **Restringir** | Nenhum | Nenhum

Para relações necessárias (chave estrangeira não anuláveis) é _não_ possível salvar um nulo valor de chave estrangeiro, que resulta nos seguintes efeitos:

| Nome de comportamento | Efeito em dependente/filho na memória | Efeito em dependente/filho no banco de dados
|-|-|-
| **CASCADE** (padrão) | Entidades são excluídas | Entidades são excluídas
| **ClientSetNull** | SaveChanges lança | Nenhum
| **SetNull** | SaveChanges lança | SaveChanges lança
| **Restringir** | Nenhum | Nenhum

Nas tabelas acima, *nenhum* pode resultar em uma violação de restrição. Por exemplo, se uma entidade principal/filho é excluída, mas nenhuma ação será tomada para alterar a chave estrangeira da dependente/filho, em seguida, o banco de dados provavelmente lançará em SaveChanges devido a uma violação de restrição foreign.

Em um alto nível:
* Se você tiver entidades que não podem existir sem um pai, e você deseja EF para cuidar para excluir os filhos automaticamente e usa *Cascade*.
  * Entidades que não podem existir sem um pai geralmente faz uso de relações necessárias, para o qual *Cascade* é o padrão.
* Se você tiver entidades que podem ou não ter um pai, e você deseja EF para cuidar de anular a chave estrangeira para você, e depois usar *ClientSetNull*
  * Entidades que podem existir sem um pai geralmente faz uso de relações opcionais, para o qual *ClientSetNull* é o padrão.
  * Se você quiser que o banco de dados também tenta propagar os valores nulos para chaves estrangeiras filho até mesmo quando a entidade filho não está carregada, em seguida, use *SetNull*. No entanto, observe que o banco de dados deve oferecer suporte a isso e configurar o banco de dados como isso pode resultar em outras restrições, que, na prática, geralmente faz essa opção impraticável. É por isso que *SetNull* não é o padrão.
* Se você não quiser núcleo para nunca excluir uma entidade automaticamente ou null automaticamente a chave estrangeira, em seguida, usar o EF *restringir*. Observe que isso requer que seu código manter entidades filho e seus valores de chave estrangeiras em sincronia manualmente caso contrário, as exceções de restrição serão lançadas.

> [!NOTE]
> No núcleo do EF, ao contrário de EF6, efeitos em cascata não ocorrer imediatamente, mas em vez disso, apenas quando SaveChanges é chamado.

> [!NOTE]  
> **Alterações no EF Core 2.0:** em versões anteriores, *restringir* causaria opcionais propriedades de chave estrangeira em entidades dependentes controladas para ser definido como null e era o padrão excluir comportamento para relações opcionais. No EF Core 2.0, o *ClientSetNull* foi introduzido para representar esse comportamento e tornou-se o padrão de relações opcionais. O comportamento de *restringir* foi ajustado para nunca têm efeitos colaterais em entidades dependentes.

## <a name="entity-deletion-examples"></a>Exemplos de exclusão de entidade

O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado um tempo de execução. O exemplo mostra o que acontece para cada comportamento de exclusão de relações necessários e opcionais quando uma entidade pai é excluída.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Vamos examinar cada variação para entender o que está acontecendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade com relação obrigatória ou opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Blog está marcado como excluído
* Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges
* SaveChanges envia exclusões para dependentes/filhos (postagens) e, em seguida, o principal/pai (blog)
* Depois de salvar, todas as entidades são desanexadas desde agora foram excluídas do banco de dados

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação necessária

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog está marcado como excluído
* Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges
* SaveChanges tenta definir a postagem FK como nulo, mas falha porque a CE não é anulável

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog está marcado como excluído
* Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges
* Tentativas de SaveChanges define FK (postagens) em filhos dependentes/como nulo antes de excluir a entidade de segurança/pai (blog)
* Depois de salvar, o principal/pai (blog) será excluído, mas os seus dependentes/filhos (postagens) ainda são controlados
* Os dependentes/filhos controlados (postagens) agora tem valores nulos de FK e sua referência para a entidade de segurança/pai excluído (blog) foi removida

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict com relação obrigatória ou opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog está marcado como excluído
* Postagens inicialmente permanecem inalterados desde cascatas não acontecerá até SaveChanges
* Como *restringir* informa ao EF para definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges gera sem salvar

## <a name="delete-orphans-examples"></a>Exemplos de órfãos de exclusão

O código a seguir faz parte de um [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que pode ser baixado um tempo de execução. O exemplo mostra o que acontece para cada comportamento de exclusão de relações necessários e opcionais quando a relação entre um pai/principal e seus filhos/dependentes for desfeita. Neste exemplo, a relação for desfeita, removendo os dependentes/filhos (postagens) da propriedade de navegação da coleção no principal/pai (blog). No entanto, o comportamento é o mesmo se a referência de dependente/filho ao principal/pai em vez disso, é nulled-out.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Vamos examinar cada variação para entender o que está acontecendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade com relação obrigatória ou opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo
  * Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo
* SaveChanges envia exclusões para dependentes/filhos (postagens)
* Depois de salvar, dependentes/filhos (postagens) são desanexados desde agora foram excluídas do banco de dados

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação necessária

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo
  * Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo
* SaveChanges tenta definir a postagem FK como nulo, mas falha porque a CE não é anulável

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo
  * Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo
* SaveChanges define FK (postagens) em filhos dependentes/como nulo
* Depois de salvar, dependentes/filhos (postagens) agora tem valores nulos de FK e sua referência para a entidade de segurança/pai excluído (blog) foi removida

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict com relação obrigatória ou opcional

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Postagens são marcadas como modificado porque cortar a relação causou FK deve ser marcado como nulo
  * Se a CE não é anulável, em seguida, o valor real não mudará mesmo que ele está marcado como nulo
* Como *restringir* informa ao EF para definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges gera sem salvar

## <a name="cascading-to-untracked-entities"></a>Em cascata para entidades não controladas

Quando você chama *SaveChanges*, as regras de exclusão em cascata serão aplicadas a qualquer entidade que está sendo controlada pelo contexto. Esta é a situação em todos os exemplos mostrados acima, que é por SQL foi gerado para excluir a entidade de segurança/pai (blog) e todos os seus dependentes/filhos (postagens):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Se apenas a entidade de segurança é carregada – por exemplo, quando uma consulta for feita para um blog sem um `Include(b => b.Posts)` para incluir também as postagens – em seguida, SaveChanges gerará somente o SQL para excluir a entidade de segurança/pai:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Os dependentes/filhos (postagens) só serão excluídos se o banco de dados tem um comportamento de cascata correspondente configurado. Se você usar o EF para criar o banco de dados, esse comportamento em cascata será instalado para você.
