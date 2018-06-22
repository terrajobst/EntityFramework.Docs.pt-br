---
title: Herança - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052776"
---
# <a name="inheritance"></a>Herança

Herança no modelo EF é usada para controlar como a herança em classes de entidade é representada no banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, é até o provedor de banco de dados para determinar como herança será representada no banco de dados. Consulte [herança (banco de dados relacional)](relational/inheritance.md) de como isso é tratado com um provedor de banco de dados relacional.

EF apenas configurará herança se dois ou mais tipos herdados estão explicitamente incluídos no modelo. EF não irá procurar tipos base ou derivados que não foram incluídos no modelo. Você pode incluir tipos no modelo ao expor um *DbSet<TEntity>*  para cada tipo na hierarquia de herança.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se você não quiser expor um *DbSet<TEntity>*  para uma ou mais entidades na hierarquia, você pode usar a API fluente para garantir que eles estão incluídos no modelo.
E se você não confiar em convenções, você pode especificar o tipo base explicitamente usando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Você pode usar `.HasBaseType((Type)null)` para remover um tipo de entidade da hierarquia.

## <a name="data-annotations"></a>Anotações de dados

Você não pode usar as anotações de dados para configurar a herança.

## <a name="fluent-api"></a>API fluente

A API fluente de herança depende do provedor de banco de dados que você está usando. Consulte [herança (banco de dados relacional)](relational/inheritance.md) para a configuração que você pode executar para um provedor de banco de dados relacional.
