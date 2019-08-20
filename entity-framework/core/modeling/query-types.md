---
title: Tipos de consulta – EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: c023d442b0fa2728bd20694a55ebb3a7b5c0efd1
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628407"
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Este recurso é novo no EF Core 2.1

Além dos tipos de entidade, um modelo do EF Core pode conter _tipos de consulta_, que pode ser usado para executar consultas de banco de dados em relação aos dados que não são mapeados para tipos de entidade.

## <a name="compare-query-types-to-entity-types"></a>Compare os tipos de consulta para tipos de entidade

Tipos de consulta, como tipos de entidade em que eles são:

- Pode ser adicionado ao modelo ou no `OnModelCreating` ou por meio de uma propriedade "set" em um derivada _DbContext_.
- Suporte a muitos dos mesmos recursos de mapeamento, como as propriedades de navegação e de mapeamento de herança. Em repositórios relacionais, eles podem configurar as colunas por meio de métodos da API fluentes ou anotações de dados e objetos de banco de dados de destino.

No entanto, eles são diferentes da entidade tipos em que eles:

- Não exigem uma chave a ser definido.
- Nunca são rastreados para alterações na _DbContext_ e, portanto, são nunca inseridos, atualizados ou excluídos no banco de dados.
- Nunca são descobertos pela convenção.
- Somente suporta um subconjunto de recursos de mapeamento de navegação - especificamente:
  - Eles nunca podem agir como o final principal de uma relação.
  - Eles só podem conter propriedades de navegação de referência que aponta para entidades.
  - Entidades não podem conter propriedades de navegação para tipos de consulta.
- Sejam resolvidos do _ModelBuilder_ usando o `Query` método em vez de `Entity` método.
- São mapeadas na _DbContext_ por meio das propriedades do tipo `DbQuery<T>` em vez de `DbSet<T>`
- São mapeadas para objetos de banco de dados usando o `ToView` método, em vez de `ToTable`.
- Pode ser mapeado para um _definição de consulta_ - uma definição de consulta é uma consulta secundária declarada no modelo que funciona de uma fonte de dados para um tipo de consulta.

## <a name="usage-scenarios"></a>Cenários de uso

Estes são alguns dos cenários de uso principal para tipos de consulta:

- Servindo como o tipo de retorno para o ad-hoc `FromSql()` consultas.
- Mapeando para exibições de banco de dados.
- Mapeamento para tabelas que não têm uma chave primária definida.
- Mapeamento para consultas definidas no modelo.

## <a name="mapping-to-database-objects"></a>Mapeando para objetos de banco de dados

Mapeando um tipo de consulta para um objeto de banco de dados é feito usando o `ToView` API fluente. Da perspectiva do EF Core, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela é tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações. No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados – como alternativa, ele pode ser uma tabela de banco de dados que será tratada como somente leitura. Por outro lado, para tipos de entidade, o EF Core supõe que um objeto de banco de dados especificado na `ToTable` método pode ser tratado como um _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também é direcionado pela atualização, excluir e inserir operações. Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem, desde que o modo de exibição está configurado para ser atualizável no banco de dados.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar o tipo de consulta para consultar uma exibição de banco de dados.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) deste artigo no GitHub.

Primeiro, definimos um modelo simples de Blog e Post:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

Em seguida, definimos uma exibição de banco de dados simples que nos permitirá consultar o número de postagens associada a cada blog:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

Em seguida, definimos uma classe para manter o resultado da exibição do banco de dados:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

Em seguida, configuramos o tipo de consulta no _OnModelCreating_ usando o `modelBuilder.Query<T>` API.
Podemos usar APIs padrão de configuração fluente para configurar o mapeamento para o tipo de consulta:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

Em seguida, configuramos o `DbContext` para `DbQuery<T>`incluir:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

Por fim, é possível consultar a exibição de banco de dados da maneira padrão:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> Observe que também definimos uma propriedade de consulta de nível de contexto (DbQuery) para atuar como uma raiz para consultas em relação a esse tipo.
