---
title: Incluindo & excluindo tipos-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655734"
---
# <a name="including--excluding-types"></a>Incluir e Excluir Tipos

A inclusão de um tipo no modelo significa que o EF tem metadados sobre esse tipo e tentará ler e gravar instâncias de/para o banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, os tipos expostos em Propriedades `DbSet` no seu contexto são incluídos em seu modelo. Além disso, os tipos mencionados no método `OnModelCreating` também são incluídos. Finalmente, todos os tipos que são encontrados pela exploração recursiva das propriedades de navegação de tipos descobertos também são incluídos no modelo.

**Por exemplo, na listagem de código a seguir, todos os três tipos são descobertos:**

* `Blog` porque ele é exposto em uma propriedade `DbSet` no contexto

* `Post` porque ele é descoberto por meio da propriedade de navegação `Blog.Posts`

* `AuditEntry` porque ele é mencionado em `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para excluir um tipo do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para excluir um tipo do modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
