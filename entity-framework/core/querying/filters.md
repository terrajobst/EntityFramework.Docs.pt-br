---
title: Filtros de consulta global - Core EF
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Filtros de consulta global

Filtros de consulta global são predicados de consulta LINQ (uma expressão booleana normalmente passado para o LINQ *onde* operador de consulta) aplicado aos tipos de entidade no modelo de metadados (normalmente em *OnModelCreating*). Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolvem esses tipos de entidade, incluindo referências de propriedade de navegação direta ou referenciado indiretamente, por exemplo, com o uso de inclusão de tipos de entidade. Alguns aplicativos comuns desse recurso são:

* **Exclusão reversível** -um tipo de entidade define uma *IsDeleted* propriedade.
* **Multilocação** -um tipo de entidade define uma *TenantId* propriedade.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar filtros de consulta Global para implementar a consulta exclusão reversível e multilocação comportamentos em um modelo simples de blogs.

> [!TIP]
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) no GitHub.

Primeiro, defina as entidades:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Observe a declaração de um __tenantId_ campo o _Blog_ entidade. Isso será usado para associar cada instância de Blog com um locatário específico. Definido também é um _IsDeleted_ propriedade o _Post_ tipo de entidade. Isso é usado para controlar se uma _Post_ instância foi "excluído". Ou seja A instância está marcada como excluído withouth remover fisicamente os dados subjacentes.

Em seguida, configure os filtros de consulta no _OnModelCreating_ usando o ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

As expressões de predicado transmitido para o _HasQueryFilter_ chamadas agora serão automaticamente aplicadas a quaisquer consultas LINQ para esses tipos.

> [!TIP]
> Observe o uso de um campo de nível de instância de DbContext: ```_tenantId``` usado para definir o locatário atual. Filtros de nível de modelo usará o valor da instância do contexto correto. Ou seja A instância que está executando a consulta.

## <a name="disabling-filters"></a>Desabilitar filtros

Filtros podem ser desabilitados para consultas LINQ individuais usando o ```IgnoreQueryFilters()``` operador.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitações

Os filtros de consulta global têm as seguintes limitações:

* Filtros não podem conter referências a propriedades de navegação.
* Filtros podem ser definidos somente para a raiz do tipo de entidade de uma hierarquia de herança.
