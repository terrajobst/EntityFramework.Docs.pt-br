---
title: Índices-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655694"
---
# <a name="indexes"></a>Índices

Os índices são um conceito comum entre vários armazenamentos de dados. Embora sua implementação no armazenamento de dados possa variar, elas são usadas para fazer pesquisas com base em uma coluna (ou conjunto de colunas) mais eficiente.

## <a name="conventions"></a>Convenções

Por convenção, um índice é criado em cada propriedade (ou conjunto de propriedades) que são usados como uma chave estrangeira.

## <a name="data-annotations"></a>Anotações de dados

Não é possível criar índices usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar um índice em uma única propriedade. Por padrão, os índices são não exclusivos.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Você também pode especificar que um índice deve ser exclusivo, o que significa que duas entidades podem ter os mesmos valores para as propriedades especificadas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Você também pode especificar um índice em mais de uma coluna.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Há apenas um índice por conjunto distinto de propriedades. Se você usar a API fluente para configurar um índice em um conjunto de propriedades que já tem um índice definido, seja por convenção ou configuração anterior, você alterará a definição desse índice. Isso será útil se você quiser configurar ainda mais um índice que foi criado por convenção.
