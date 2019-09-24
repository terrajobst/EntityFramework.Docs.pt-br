---
title: Herança-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197295"
---
# <a name="inheritance"></a>Herança

A herança no modelo do EF é usada para controlar como a herança nas classes de entidade é representada no banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, cabe ao provedor de banco de dados determinar como a herança será representada no banco de dados. Consulte [herança (banco de dados relacional)](relational/inheritance.md) para saber como isso é tratado com um provedor de banco de dados relacional.

O EF só configurará a herança se dois ou mais tipos herdados forem explicitamente incluídos no modelo. O EF não examinará os tipos base ou derivados que não foram incluídos de outra forma no modelo. Você pode incluir tipos no modelo expondo um *DbSet<TEntity>*  para cada tipo na hierarquia de herança.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se você não quiser expor um *DbSet<TEntity>*  para uma ou mais entidades na hierarquia, poderá usar a API fluente para garantir que elas sejam incluídas no modelo.
E se você não depender de convenções, poderá especificar o tipo base explicitamente usando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.

## <a name="data-annotations"></a>Anotações de dados

Você não pode usar anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

A API fluente para herança depende do provedor de banco de dados que você está usando. Consulte [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.
