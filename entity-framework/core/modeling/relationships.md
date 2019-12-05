---
title: Relações-EF Core
description: Como configurar relações entre tipos de entidade ao usar Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 452169c902d56fda0a65a5c2846a9b42c80640fd
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824768"
---
# <a name="relationships"></a>Relações

Uma relação define como duas entidades se relacionam entre si. Em um banco de dados relacional, isso é representado por uma restrição FOREIGN KEY.

> [!NOTE]  
> A maioria dos exemplos neste artigo usa uma relação um-para-muitos para demonstrar conceitos. Para obter exemplos de relações um-para-um e muitos para muitos, consulte a seção [outros padrões de relação](#other-relationship-patterns) no final do artigo.

## <a name="definition-of-terms"></a>Definição de termos

Há vários termos usados para descrever as relações

* **Entidade dependente:** Essa é a entidade que contém as propriedades de chave estrangeira. Às vezes, chamado de ' filho ' da relação.

* **Entidade principal:** Esta é a entidade que contém as propriedades de chave primária/alternativa. Às vezes, chamado de ' pai ' da relação.

* **Chave estrangeira:** As propriedades na entidade dependente que são usadas para armazenar os valores de chave de entidade de segurança para a entidade relacionada.

* **Chave principal:** As propriedades que identificam exclusivamente a entidade principal. Essa pode ser a chave primária ou uma chave alternativa.

* **Propriedade de navegação:** Uma propriedade definida na entidade principal e/ou dependente que faz referência à entidade relacionada.

  * **Propriedade de navegação da coleção:** Uma propriedade de navegação que contém referências a muitas entidades relacionadas.

  * **Propriedade de navegação de referência:** Uma propriedade de navegação que mantém uma referência a uma única entidade relacionada.

  * **Propriedade de navegação inversa:** Ao discutir uma propriedade de navegação específica, esse termo refere-se à propriedade de navegação na outra extremidade da relação.
  
* **Relação de auto-referência:** Uma relação na qual os tipos de entidade dependente e principal são os mesmos.

O código a seguir mostra uma relação um-para-muitos entre `Blog` e `Post`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

* `Post` é a entidade dependente

* `Blog` é a entidade principal

* `Post.BlogId` é a chave estrangeira

* `Blog.BlogId` é a chave principal (nesse caso, é uma chave primária em vez de uma chave alternativa)

* `Post.Blog` é uma propriedade de navegação de referência

* `Blog.Posts` é uma propriedade de navegação de coleção

* `Post.Blog` é a propriedade de navegação inversa de `Blog.Posts` (e vice-versa)

## <a name="conventions"></a>Convenções

Por padrão, uma relação será criada quando houver uma propriedade de navegação descoberta em um tipo. Uma propriedade é considerada uma propriedade de navegação se o tipo que ele aponta não puder ser mapeado como um tipo escalar pelo provedor de banco de dados atual.

> [!NOTE]  
> As relações descobertas por convenção serão sempre direcionadas à chave primária da entidade principal. Para direcionar uma chave alternativa, a configuração adicional deve ser executada usando a API Fluent.

### <a name="fully-defined-relationships"></a>Relações totalmente definidas

O padrão mais comum para relações é ter propriedades de navegação definidas em ambas as extremidades da relação e uma propriedade de chave estrangeira definida na classe de entidade dependente.

* Se um par de propriedades de navegação for encontrado entre dois tipos, eles serão configurados como propriedades de navegação inversas da mesma relação.

* Se a entidade dependente contiver uma propriedade com um nome que matemática um desses padrões, ela será configurada como a chave estrangeira:
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

Neste exemplo, as propriedades realçadas serão usadas para configurar a relação.

> [!NOTE]
> Se a propriedade for a chave primária ou for de um tipo não compatível com a chave principal, ela não será configurada como a chave estrangeira.

> [!NOTE]
> Antes de EF Core 3,0, a propriedade denominada exatamente igual à propriedade da chave principal [também foi correspondida como a chave estrangeira](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

### <a name="no-foreign-key-property"></a>Nenhuma propriedade de chave estrangeira

Embora seja recomendável ter uma propriedade de chave estrangeira definida na classe de entidade dependente, ela não é necessária. Se nenhuma propriedade de chave estrangeira for encontrada, uma [propriedade de chave estrangeira de sombra](shadow-properties.md) será introduzida com o nome `<navigation property name><principal key property name>` ou `<principal entity name><principal key property name>` se nenhuma navegação estiver presente no tipo dependente.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

Neste exemplo, a chave estrangeira de sombra é `BlogId` porque a prependência do nome de navegação seria redundante.

> [!NOTE]
> Se já existir uma propriedade com o mesmo nome, o nome da propriedade de sombra será sufixado com um número.

### <a name="single-navigation-property"></a>Propriedade de navegação única

Incluir apenas uma propriedade de navegação (sem navegação inversa e nenhuma propriedade de chave estrangeira) é suficiente para ter uma relação definida por convenção. Você também pode ter uma única propriedade de navegação e uma propriedade de chave estrangeira.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="limitations"></a>Limitações

Quando há várias propriedades de navegação definidas entre dois tipos (ou seja, mais do que apenas um par de navegações que apontam entre si), as relações representadas pelas propriedades de navegação são ambíguas. Será necessário configurá-los manualmente para resolver a ambiguidade.

### <a name="cascade-delete"></a>Exclusão em cascata

Por convenção, a exclusão em cascata será definida como *em cascata* para relações necessárias e *ClientSetNull* para relações opcionais. *Cascade* significa que as entidades dependentes também são excluídas. *ClientSetNull* significa que as entidades dependentes que não estão carregadas na memória permanecerão inalteradas e deverão ser excluídas manualmente ou atualizadas para apontar para uma entidade principal válida. Para entidades que são carregadas na memória, EF Core tentará definir as propriedades de chave estrangeira como NULL.

Consulte a seção [relações obrigatórias e opcionais](#required-and-optional-relationships) para obter a diferença entre as relações obrigatórias e opcionais.

Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter mais detalhes sobre os diferentes comportamentos de exclusão e os padrões usados pela Convenção.

## <a name="manual-configuration"></a>Configuração manual

#### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

Para configurar uma relação na API fluente, você começa identificando as propriedades de navegação que compõem a relação. `HasOne` ou `HasMany` identifica a propriedade de navegação no tipo de entidade em que você está iniciando a configuração. Em seguida, você encadea uma chamada para `WithOne` ou `WithMany` para identificar a navegação inversa. os `WithOne` de /`HasOne`são usados para propriedades de navegação de referência e `HasMany`/de `WithMany` são usados para propriedades de navegação de coleção.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

#### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

Você pode usar as anotações de dados para configurar como as propriedades de navegação nas entidades dependentes e de entidade emparelham. Isso normalmente é feito quando há mais de um par de propriedades de navegação entre dois tipos de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

> [!NOTE]
> Você só pode usar [Required] em Propriedades na entidade dependente para afetar a necessidade da relação. [Obrigatório] na navegação da entidade principal geralmente é ignorado, mas pode fazer com que a entidade se torne a dependente.

> [!NOTE]
> As anotações de dados `[ForeignKey]` e `[InverseProperty]` estão disponíveis no namespace `System.ComponentModel.DataAnnotations.Schema`. `[Required]` está disponível no namespace `System.ComponentModel.DataAnnotations`.

---

### <a name="single-navigation-property"></a>Propriedade de navegação única

Se você tiver apenas uma propriedade de navegação, haverá sobrecargas sem parâmetros de `WithOne` e `WithMany`. Isso indica que há uma referência ou uma coleção conceitualmente na outra extremidade da relação, mas não há nenhuma propriedade de navegação incluída na classe de entidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Chave estrangeira

#### <a name="fluent-apitabfluent-api"></a>[API fluente](#tab/fluent-api)

Você pode usar a API fluente para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

#### <a name="data-annotationstabdata-annotations"></a>[Anotações de dados](#tab/data-annotations)

Você pode usar as anotações de dados para configurar qual propriedade deve ser usada como a propriedade de chave estrangeira para uma determinada relação. Isso normalmente é feito quando a propriedade de chave estrangeira não é descoberta pela Convenção.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> A anotação `[ForeignKey]` pode ser colocada em qualquer propriedade de navegação na relação. Ele não precisa ir para a propriedade de navegação na classe de entidade dependente.

> [!NOTE]
> A propriedade especificada usando `[ForeignKey]` em uma propriedade de navegação não precisa existir no tipo dependente. Nesse caso, o nome especificado será usado para criar uma chave estrangeira de sombra.

---

O código a seguir mostra como configurar uma chave estrangeira composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Você pode usar a sobrecarga de cadeia de caracteres de `HasForeignKey(...)` para configurar uma propriedade de sombra como uma chave estrangeira (consulte [Propriedades de sombra](shadow-properties.md) para obter mais informações). É recomendável adicionar explicitamente a propriedade Shadow ao modelo antes de usá-la como uma chave estrangeira (como mostrado abaixo).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Sem propriedade de navegação

Você não precisa necessariamente fornecer uma propriedade de navegação. Você pode simplesmente fornecer uma chave estrangeira em um lado da relação.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Chave principal

Se desejar que a chave estrangeira referencie uma propriedade diferente da chave primária, você poderá usar a API Fluent para configurar a propriedade principal de chave para a relação. A propriedade que você configurar como a chave principal será automaticamente configurada como uma [chave alternativa](alternate-keys.md).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

O código a seguir mostra como configurar uma chave de entidade composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> A ordem na qual você especifica as propriedades da chave de entidade de segurança deve corresponder à ordem na qual elas são especificadas para a chave estrangeira.

### <a name="required-and-optional-relationships"></a>Relações obrigatórias e opcionais

Você pode usar a API fluente para configurar se a relação é necessária ou opcional. Por fim, isso controla se a propriedade de chave estrangeira é necessária ou opcional. Isso é mais útil quando você está usando uma chave estrangeira de estado de sombra. Se você tiver uma propriedade de chave estrangeira em sua classe de entidade, a exigência da relação será determinada com base em se a propriedade de chave estrangeira é necessária ou opcional (consulte [as propriedades obrigatórias e opcionais](required-optional.md) para obter mais informações).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

> [!NOTE]
> Chamar `IsRequired(false)` também torna a propriedade de chave estrangeira opcional, a menos que esteja configurada de outra forma.

### <a name="cascade-delete"></a>Exclusão em cascata

Você pode usar a API Fluent para configurar o comportamento de exclusão em cascata para uma determinada relação explicitamente.

Consulte [exclusão em cascata](../saving/cascade-delete.md) para obter uma discussão detalhada de cada opção.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Outros padrões de relação

### <a name="one-to-one"></a>Um-para-um

Relações um para um têm uma propriedade de navegação de referência em ambos os lados. Eles seguem as mesmas convenções que as relações um-para-muitos, mas um índice exclusivo é introduzido na propriedade Foreign Key para garantir que apenas um dependente esteja relacionado a cada entidade de segurança.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> O EF escolherá uma das entidades como dependente, com base em sua capacidade de detectar uma propriedade de chave estrangeira. Se a entidade incorreta for escolhida como dependente, você poderá usar a API fluente para corrigir isso.

Ao configurar a relação com a API Fluent, você usa os métodos `HasOne` e `WithOne`.

Ao configurar a chave estrangeira, você precisa especificar o tipo de entidade dependente-Observe o parâmetro genérico fornecido para `HasForeignKey` na lista abaixo. Em uma relação um-para-muitos, fica claro que a entidade com a navegação de referência é a dependente e aquela com a coleção é a principal. Mas isso não é tão em relação um-para-um-portanto, a necessidade de defini-lo explicitamente.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Muitos para muitos

Relações muitos para muitos sem uma classe de entidade para representar a tabela de junção ainda não têm suporte. No entanto, você pode representar uma relação muitos para muitos incluindo uma classe de entidade para a tabela de junção e mapeando duas relações um-para-muitos separadas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
