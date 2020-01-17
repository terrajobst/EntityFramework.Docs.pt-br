---
title: Filtros de consulta global – EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124385"
---
# <a name="global-query-filters"></a>Filtros de consulta global

> [!NOTE]
> Essa funcionalidade foi introduzida no EF Core 2.0.

Os filtros de consulta global são predicados de consulta LINQ (uma expressão booliana normalmente passada ao operador de consulta LINQ *Where*) aplicado aos Tipos de Entidade no modelo de metadados (normalmente, *OnModelCreating*). Esses filtros são aplicados automaticamente em qualquer consulta LINQ que envolva esses Tipos de Entidade, incluindo Tipos de Entidade referenciados indiretamente, como usando as referências de propriedade de navegação direta ou de Inclusão. Alguns aplicativos comuns desse recurso são:

* **Exclusão reversível** – um Tipo de Entidade define uma propriedade *IsDeleted*.
* **Multilocação** – um Tipo de Entidade define uma propriedade *TenantId*.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar filtros de consulta global para implementar a exclusão reversível e comportamentos de consulta de multilocação em um modelo simples de blogs.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) deste artigo no GitHub.

Primeiro, defina as entidades:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Observe a declaração de um campo _tenantId_ na entidade _Blog_. Isso será usado para associar cada instância de Blog a um locatário específico. Também definido é uma propriedade _IsDeleted_ no tipo de entidade _Post_. Isso é usado para controlar se uma instância _Post_ foi "excluída de maneira reversível". Ou seja, a instância está marcada como excluída sem remover fisicamente os dados subjacentes.

Em seguida, configure os filtros de consulta no _OnModelCreating_ usando a API `HasQueryFilter`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

As expressões de predicado passadas para as chamadas _HasQueryFilter_ agora serão automaticamente aplicadas a quaisquer consultas LINQ para esses tipos.

> [!TIP]
> Observe o uso de um campo de nível de instância de DbContext: `_tenantId` usado para definir o locatário atual. Os filtros de nível de modelo usarão o valor da instância de contexto correta (ou seja, a instância que está executando a consulta).

> [!NOTE]
> No momento, não é possível definir vários filtros de consulta na mesma entidade; somente o último será aplicado. No entanto, você pode definir um único filtro com várias condições usando o operador _and_ lógico ([`&&` em C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Como desabilitar filtros

Os filtros podem ser desabilitados para consultas LINQ individuais usando o operador `IgnoreQueryFilters()`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitações

Os filtros de consulta global têm as seguintes limitações:

* Os filtros podem ser definidos somente para o Tipo de Entidade raiz de uma hierarquia de herança.
