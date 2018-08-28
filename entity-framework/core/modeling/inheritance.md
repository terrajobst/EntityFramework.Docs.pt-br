---
title: Herança – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995890"
---
# <a name="inheritance"></a>Herança

Herança no modelo do EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, é responsabilidade do provedor de banco de dados para determinar como herança será representada no banco de dados. Ver [herança (banco de dados relacional)](relational/inheritance.md) de como isso é tratado com um provedor de banco de dados relacional.

EF só irá configurar a herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo. O EF não examinará para tipos de base ou derivados caso contrário, não foram incluídos no modelo. Você pode incluir tipos no modelo, expondo um *DbSet<TEntity>*  para cada tipo na hierarquia de herança.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se você não quiser expor uma *DbSet<TEntity>*  para uma ou mais entidades na hierarquia, você pode usar a API Fluent para garantir que eles são incluídos no modelo.
E se você não confie em convenções, você pode especificar o tipo base explicitamente usando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.

## <a name="data-annotations"></a>Anotações de dados

É possível usar as anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

A API Fluent para herança depende do provedor de banco de dados que você está usando. Ver [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.
