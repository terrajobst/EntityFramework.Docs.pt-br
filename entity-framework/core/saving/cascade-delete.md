---
title: Excluir em cascata – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e92b869d691d0224abf1997d9eb7ea035489c5d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417610"
---
# <a name="cascade-delete"></a>Excluir em cascata

A Exclusão em cascata é geralmente usada na terminologia de banco de dados para descrever uma característica que permite a exclusão de uma linha para disparar automaticamente a exclusão de linhas relacionadas. Um conceito relacionado também coberto por comportamentos de exclusão de EF Core é a exclusão automática de uma entidade filho quando sua relação com um pai foi desfeita, isso é conhecido como "excluir órfãos".

O EF Core implementa vários comportamentos de exclusão diferentes e permite a configuração dos comportamentos de exclusão de relações individuais. O EF Core também implementa convenções que configuram automaticamente os comportamentos de exclusão padrão úteis para cada relação com base nas [exigências do relacionamento](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Comportamentos de exclusão

Os comportamentos de exclusão são definidos no tipo de enumerador *DeleteBehavior* e pode ser passado para a API fluente *OnDelete* para controlar se a exclusão de uma entidade de segurança/pai ou o corte da relação com entidades dependentes/filho deve ter um efeito colateral nas entidades dependentes/filho.

Há três ações que o EF pode executar quando uma entidade de segurança/pai é excluída ou a relação com o filho é desligada:

* O filho/dependente pode ser excluído
* Os valores de chave estrangeira do filho podem ser definidos como nulo
* O filho permanece inalterado

> [!NOTE]  
> O comportamento de exclusão configurado no modelo do EF Core só é aplicado quando a entidade de segurança é excluída usando o EF Core e as entidades dependentes são carregadas na memória (ou seja, para dependentes controlados). Um comportamento em cascata correspondente precisa ser configurado no banco de dados para garantir que não esteja sendo controlado pelo contexto e tenha a ação necessária aplicada. Se você usar o EF Core para criar o banco de dados, esse comportamento em cascata será configurado para você.

Para a segunda ação acima, definir um valor de chave estrangeira como nulo não será válido se a chave estrangeira não for anulável. (Uma chave estrangeira não anulável é equivalente a uma relação necessária.) Nesses casos, EF Core rastreia que a propriedade de chave estrangeira foi marcada como nula até que SaveChanges seja chamado, quando uma exceção é gerada porque a alteração não pode persistir no banco de dados. Isso é semelhante a obter uma violação de restrição do banco de dados.

Há quatro comportamentos de exclusão, conforme o listado nas tabelas a seguir.

### <a name="optional-relationships"></a>Relações opcionais

Para relações opcionais (chave estrangeira anulável), _é_ possível salvar um valor de chave estrangeiro nulo, que resulta nos seguintes efeitos:

| Nome do comportamento               | Efeito em dependente/filho na memória    | Efeito em dependente/filho no banco de dados  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascata**                 | As entidades são excluídas                   | As entidades são excluídas                   |
| **ClientSetNull** (padrão) | Propriedades de chave estrangeira são definidas como nulas | Nenhum                                   |
| **SetNull**                 | Propriedades de chave estrangeira são definidas como nulas | Propriedades de chave estrangeira são definidas como nulas |
| **Restrict**                | Nenhum                                   | Nenhum                                   |

### <a name="required-relationships"></a>Relações necessárias

Para relações obrigatórias (chave estrangeira não anulável), _não_ pode salvar um valor de chave estrangeiro nulo, que resulta nos seguintes efeitos:

| Nome do comportamento         | Efeito em dependente/filho na memória | Efeito em dependente/filho no banco de dados |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Cascata** (Padrão) | As entidades são excluídas                | As entidades são excluídas                  |
| **ClientSetNull**     | SaveChanges gera                  | Nenhum                                  |
| **SetNull**           | SaveChanges gera                  | SaveChanges gera                    |
| **Restrict**          | Nenhum                                | Nenhum                                  |

Nas tabelas acima, *Nenhum* pode resultar em uma violação de restrição. Por exemplo, se uma entidade de segurança/filho for excluída, mas nenhuma ação for tomada para alterar a chave estrangeira de um dependente/filho, então o banco de dados provavelmente gerará SaveChanges devido a uma violação de restrição de chave estrangeira.

Em um alto nível:

* Se você tiver entidades que não podem existir sem um pai, e você deseja que o EF cuide para excluir os filhos automaticamente, use *Cascade*.
  * As entidades que não podem existir sem um pai geralmente usam as relações obrigatórias, para as quais *Cascade* é o padrão.
* Se você tiver entidades que podem ou não ter um pai, e deseja que o EF cuide de anular a chave estrangeira para você, use *ClientSetNull*
  * As entidades que podem existir sem um pai geralmente usam as relações opcionais, para as quais *ClientSetNull* é o padrão.
  * Se você quiser que o banco de dados também tente propagar os valores nulos para chaves estrangeiras filho até mesmo quando a entidade filho não está carregada, em seguida, use *SetNull*. No entanto, observe que o banco de dados deve oferecer suporte a isso e configurar o banco de dados assim pode resultar em outras restrições, que, na prática, geralmente tornam essa opção impraticável. É por isso que *SetNull* não é o padrão.
* Se você não quiser que o EF Core exclua uma entidade automaticamente ou anule automaticamente a chave estrangeira, use *Restrict*. Observe que isso requer que seu código mantenha entidades filho e seus valores de chave estrangeira em sincronia manualmente, caso contrário, as exceções de restrição serão geradas.

> [!NOTE]
> No EF Core, ao contrário de EF6, os efeitos em cascata não ocorrem imediatamente, apenas quando SaveChanges é chamado.

> [!NOTE]  
> **Alterações no EF Core 2.0:** em versões anteriores, *Restrict* causaria as propriedades de chave estrangeira opcionais em entidades dependentes controladas serem definidas como nulas e o padrão era o comportamento de exclusão para relações opcionais. No EF Core 2.0, o *ClientSetNull* foi introduzido para representar esse comportamento e tornou-se o padrão para relações opcionais. O comportamento de *Restrict* foi ajustado para nunca ter efeitos colaterais em entidades dependentes.

## <a name="entity-deletion-examples"></a>Exemplos de exclusão de entidade

O código a seguir faz parte de um [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) que pode ser baixado e executado. O exemplo mostra o que acontece para cada comportamento de exclusão para relações obrigatórias e opcionais quando uma entidade pai é excluída.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Vamos examinar cada variação para entender o que está acontecendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade com relação obrigatória ou opcional

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* O blog está marcado como Excluído
* As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges
* SaveChanges envia exclusões para dependentes/filhos (postagens) e, em seguida, a entidade de segurança/pai (blog)
* Depois de salvar, todas as entidades serão desanexadas porque agora foram excluídas do banco de dados

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação obrigatória

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* O blog está marcado como Excluído
* As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges
* SaveChanges tenta definir a postagem FK como nula, mas falha porque a FK não é anulável

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* O blog está marcado como Excluído
* As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges
* SaveChanges tenta definir FK de filhos/dependentes (postagens) como nulo antes de excluir a entidade de segurança/pai (blog)
* Depois de salvar, a entidade de segurança/pai (blog) será excluída, mas os seus dependentes/filhos (postagens) ainda são controlados
* Os dependentes/filhos controlados (postagens) agora têm valores FK nulos e sua referência para a entidade de segurança/pai excluída (blog) foi removida

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict com relação obrigatória ou opcional

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* O blog está marcado como Excluído
* As postagens inicialmente permanecem inalteradas porque cascatas não acontecem até SaveChanges
* Como *Restrict* informa ao EF para não definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges é gerada sem salvar

## <a name="delete-orphans-examples"></a>Exemplos de exclusão de órfãos

O código a seguir faz parte de um [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) que pode ser baixado e executado. O exemplo mostra o que acontece para cada comportamento de exclusão de relações obrigatórias e opcionais quando a relação entre um pai/entidade de segurança e seus filhos/dependentes for desfeita. Neste exemplo, a relação é desfeita removendo os dependentes/filhos (postagens) da propriedade de navegação da coleção na entidade de segurança/pai (blog). No entanto, o comportamento é o mesmo se a referência de dependente/filho a entidade de segurança/pai em vez disso for anulada.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Vamos examinar cada variação para entender o que está acontecendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade com relação obrigatória ou opcional

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo
  * Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo
* SaveChanges envia exclusões para dependentes/filhos (postagens)
* Depois de salvar, os dependentes/filhos (postagens) serão desanexados porque agora foram excluídos do banco de dados

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação obrigatória

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo
  * Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo
* SaveChanges tenta definir a postagem FK como nula, mas falha porque a FK não é anulável

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull ou DeleteBehavior.SetNull com relação opcional

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo
  * Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo
* SaveChanges define FK de filhos/dependentes (postagens) como nulo
* Depois de salvar, os dependentes/filhos (postagens) agora têm valores FK nulos e sua referência para a entidade de segurança/pai excluída (blog) foi removida

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict com relação obrigatória ou opcional

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* As postagens são marcadas como modificadas porque cortar a relação fez o FK ser marcado como nulo
  * Se o FK não for anulável, o valor real não mudará mesmo que ele esteja marcado como nulo
* Como *Restrict* informa ao EF para não definir automaticamente o FK como nulo, ele permanece não nulo e SaveChanges é gerada sem salvar

## <a name="cascading-to-untracked-entities"></a>Em cascata para entidades sem controle

Quando você chama *SaveChanges*, as regras de exclusão em cascata serão aplicadas a qualquer entidade que está sendo controlada pelo contexto. Esta é a situação em todos os exemplos mostrados acima, que é por que o SQL foi gerado para excluir a entidade de segurança/pai (blog) e todos os seus dependentes/filhos (postagens):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Se apenas a entidade de segurança for carregada, por exemplo, quando uma consulta for feita para um blog sem um `Include(b => b.Posts)` para incluir também as postagens, SaveChanges apenas gerará o SQL para excluir a entidade de segurança/pai:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Os dependentes/filhos (postagens) só serão excluídos se o banco de dados tiver um comportamento em cascata correspondente configurado. Se você usar o EF para criar o banco de dados, esse comportamento em cascata será configurado para você.
