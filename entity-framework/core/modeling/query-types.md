---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Esse recurso é novo no EF Core 2.1

Além dos tipos de entidade, um modelo de EF Core pode conter _tipos de consulta_, que pode ser usado para executar consultas de banco de dados em dados que não estão mapeadas para tipos de entidade.

Tipos de consulta tem muitas semelhanças com os tipos de entidade:

- Eles também podem ser adicionados ao modelo ou em `OnModelCreating`, ou por meio de uma propriedade "set" em um derivado _DbContext_.
- Oferece suporte a muitos dos mesmos recursos de mapeamento, como herança de mapeamento de propriedades de navegação (consulte limitações abaixo) e, em repositórios relacionais, a capacidade de configurar as colunas por meio de métodos de API fluentes ou as anotações de dados e objetos de banco de dados de destino.

No entanto forem diferentes da entidade tipos em que eles:

- Não exigem uma chave a ser definido.
- Nunca são controladas para alterações no _DbContext_ e, portanto, são nunca inseridos, atualizados ou excluídos no banco de dados.
- Nunca são descobertos por convenção.
- Suporte apenas a um subconjunto de recursos de mapeamento de navegação - especificamente, eles nunca podem agir como a extremidade principal de uma relação.
- São abordadas no _ModelBuilder_ usando o `Query` método em vez de `Entity` método.
- São mapeados a _DbContext_ por meio das propriedades do tipo `DbQuery<T>` em vez de `DbSet<T>`
- São mapeadas para objetos de banco de dados usando o `ToView` método, em vez de `ToTable`.
- Pode ser mapeado para um _definir consulta_ - uma definição de consulta é uma consulta secundária declarada no modelo que atua de uma fonte de dados para um tipo de consulta.

Estes são alguns dos cenários de uso principal para tipos de consulta:

- Serve como o tipo de retorno para ad-hoc `FromSql()` consultas.
- Mapeando para exibições de banco de dados.
- Mapeando para tabelas que não têm uma chave primária definida.
- Mapeamento para consultas definidas no modelo.

> [!TIP]
> Mapeamento de um tipo de consulta para um objeto de banco de dados é obtido usando o `ToView` API fluente. Da perspectiva do EF principal, o objeto de banco de dados especificado neste método é um _exibição_, que significa que ela será tratada como uma fonte de consulta somente leitura e não pode ser o destino da atualização, inserir ou excluir operações. No entanto, isso não significa que o objeto de banco de dados é realmente necessário para ser uma exibição de banco de dados - também pode ser uma tabela de banco de dados será tratada como somente leitura. Por outro lado, para tipos de entidade, Core EF pressupõe que um objeto de banco de dados especificado no `ToTable` método pode ser tratado como um _tabela_, o que significa que ele pode ser usado como uma fonte de consulta, mas também é afetado por atualização, excluir e inserir operações. Na verdade, você pode especificar o nome de uma exibição de banco de dados em `ToTable` e tudo deve funcionar bem como o modo de exibição está configurado para ser atualizada no banco de dados.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar o tipo de consulta para consultar uma exibição de banco de dados.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) deste artigo no GitHub.

Primeiro, definimos um modelo simples de Blog e Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

Em seguida, definimos uma exibição de banco de dados simples que permitirá que o número de postagens associado com cada blog de consulta:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

Em seguida, definimos uma classe para armazenar o resultado da exibição do banco de dados:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

Em seguida, podemos configurar o tipo de consulta em _OnModelCreating_ usando o `modelBuilder.Query<T>` API.
Usamos APIs de configuração fluente padrão para configurar o mapeamento para o tipo de consulta:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Por fim, é possível consultar a exibição de banco de dados da maneira padrão:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Observe que também, definimos uma propriedade de nível de consulta de contexto (DbQuery) para atuar como uma raiz para consultas em relação a esse tipo.
