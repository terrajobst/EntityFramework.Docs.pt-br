---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Esse recurso é novo no EF Core 2.1

Tipos de consulta são tipos de resultados de consulta somente leitura que podem ser adicionados ao modelo EF Core. Tipos de consulta habilitar consultas ad hoc (como tipos anônimos), mas são mais flexíveis porque eles podem ter a configuração de mapeamento especificada.

Eles são conceitualmente semelhantes aos tipos de entidade em que:

- Eles são POCO tipos c# que são adicionados ao modelo, no ```OnModelCreating``` usando o ```ModelBuilder.Query``` método, ou por meio de uma propriedade DbContext "set" (para tipos de consulta essa propriedade é digitada como ```DbQuery<T>``` em vez de ```DbSet<T>```).
- Eles oferecem suporte muito as mesmas capacidades de mapeamento de tipos de entidade normal. Por exemplo, o mapeamento de herança, navegações (consulte limitiations abaixo) e, em repositórios relacionais, a capacidade de configurar os objetos de esquema de banco de dados de destino via ```ToTable```, ```HasColumn``` api fluente métodos (ou as anotações de dados).

Tipos de consulta são diferentes da entidade tipos em que eles:

- Não exigem uma chave a ser definido.
- Nunca são rastreadas pelo rastreador de alteração.
- Nunca são descobertos por convenção.
- Suporte apenas a um subconjunto de recursos de mapeamento de navegação - especificamente, eles nunca podem agir como a extremidade principal de uma relação.
- Pode ser mapeado para um _definir consulta_ -A definição de consulta é uma consulta secundária que atua de uma fonte de dados para um tipo de consulta.

Estes são alguns dos cenários de uso principal para tipos de consulta:

- Mapeando para exibições de banco de dados.
- Mapeando para tabelas que não têm uma chave primária definida.
- Serve como o tipo de retorno para ad-hoc ```FromSql()``` consultas.
- Mapeamento para consultas definidas no modelo.

> [!TIP]
> Mapeamento de um tipo de consulta para um modo de exibição de banco de dados é obtido usando o ```ToTable``` API fluente.

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

Em seguida, podemos configurar o tipo de consulta em _OnModelCreating_ usando o ```modelBuilder.Query<T>``` API.
Usamos APIs de configuração fluente padrão para configurar o mapeamento para o tipo de consulta:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Por fim, é possível consultar a exibição de banco de dados da maneira padrão:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Observe que também, definimos uma propriedade de nível de consulta de contexto (DbQuery) para atuar como uma raiz para consultas em relação a esse tipo.
