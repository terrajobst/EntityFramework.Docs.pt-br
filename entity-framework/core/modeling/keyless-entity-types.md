---
title: Tipos de entidade de subunidade-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: b968ac9602b9aa1f1c1e3181b6b76a64394d70f0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150839"
---
# <a name="keyless-entity-types"></a>Tipos de entidade de subunidade
> [!NOTE]
> Este recurso é novo no EF Core 2.1. Antes de 3,0 eles eram conhecidos como tipos de consulta

Além dos tipos de entidade regulares, um modelo de EF Core pode conter _tipos de entidade sem_chave, que podem ser usados para realizar consultas de banco de dados em relação a data que não contém valores de chaves.

## <a name="keyless-entity-types-characteristics"></a>Características de tipos de entidade de subtipo

Os tipos de entidade keyless dão suporte a muitos dos mesmos recursos de mapeamento que os tipos de entidade regular, como o mapeamento de herança e as propriedades de navegação. Em repositórios relacionais, eles podem configurar as colunas por meio de métodos da API fluentes ou anotações de dados e objetos de banco de dados de destino.

No entanto, eles são diferentes dos tipos de entidade regulares, pois:

- Não é possível definir uma chave.
- Nunca são rastreadas para alterações no _DbContext_ e, portanto, nunca são inseridas, atualizadas ou excluídas no banco de dados.
- Nunca são descobertos pela convenção.
- Dá suporte apenas a um subconjunto de recursos de mapeamento de navegação, especificamente:
  - Eles nunca podem agir como o final principal de uma relação.
  - Eles podem não ter navegações para entidades pertencentes
  - Eles só podem conter Propriedades de navegação de referência apontando para entidades regulares.
  - As entidades não podem conter Propriedades de navegação para tipos de entidade sem nenhum.
- Precisa ser configurado com a `.HasNoKey()` chamada de método.
- Pode ser mapeado para uma _consulta de definição_. Uma consulta de definição é uma consulta declarada no modelo que atua como uma fonte de dados para um tipo de entidade sem modelo.

## <a name="usage-scenarios"></a>Cenários de uso

Alguns dos principais cenários de uso para tipos de entidade de tipo de subunidade são:

- Servindo como o tipo de retorno para [consultas SQL brutas](xref:core/querying/raw-sql).
- Mapeamento para exibições de banco de dados que não contêm uma chave primária.
- Mapeamento para tabelas que não têm uma chave primária definida.
- Mapeamento para consultas definidas no modelo.

## <a name="mapping-to-database-objects"></a>Mapeando para objetos de banco de dados

O mapeamento de um tipo de entidade sem um objeto de banco de dados `ToTable` é `ToView` obtido usando o ou a API fluente. Da perspectiva do EF Core, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela é tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações. No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados. Como alternativa, ele pode ser uma tabela de banco de dados que será tratada como somente leitura. Por outro lado, para tipos de entidade regulares, EF Core pressupõe que um objeto de banco de `ToTable` dados especificado no método possa ser tratado como uma _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também direcionado por operações de atualização, exclusão e inserção. Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem, desde que o modo de exibição está configurado para ser atualizável no banco de dados.

> [!NOTE]
> `ToView`supõe que o objeto já existe no banco de dados e não será criado por migrações.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar tipos de entidade para consultar uma exibição de banco de dados.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) deste artigo no GitHub.

Primeiro, definimos um modelo simples de Blog e Post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

Em seguida, definimos uma exibição de banco de dados simples que nos permitirá consultar o número de postagens associada a cada blog:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

Em seguida, definimos uma classe para manter o resultado da exibição do banco de dados:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

Em seguida, configuramos o tipo de entidade keyless em `HasNoKey` OnModelCreating usando a API.
Usamos a API de configuração fluente para configurar o mapeamento para o tipo de entidade de automenos:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

Em seguida, configuramos o `DbContext` para `DbSet<T>`incluir:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Por fim, é possível consultar a exibição de banco de dados da maneira padrão:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Observe que também definimos uma propriedade de consulta de nível de contexto (DbSet) para atuar como uma raiz para consultas nesse tipo.