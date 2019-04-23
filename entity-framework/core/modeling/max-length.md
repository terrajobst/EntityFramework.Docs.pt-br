---
title: Maximum Length - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929843"
---
# <a name="maximum-length"></a>Comprimento máximo

A configuração de um comprimento máximo fornece uma dica para o armazenamento de dados sobre o tipo de dados apropriado a ser usado para uma determinada propriedade. Comprimento máximo só se aplica a tipos de dados de matriz, como `string` e `byte[]`.

> [!NOTE]  
> Não faz nenhuma validação de comprimento máximo antes de passar dados para o provedor do Entity Framework. É responsabilidade do provedor ou repositório de dados para validar se apropriado. Por exemplo, quando o SQL Server, que excede o comprimento máximo de direcionamento resultará em uma exceção como o tipo de dados da coluna subjacente não permitirá excesso de dados a ser armazenado.

## <a name="conventions"></a>Convenções

Por convenção, é deixado para o provedor de banco de dados para escolher um tipo de dados apropriado para as propriedades. Para propriedades que têm um comprimento, o provedor de banco de dados geralmente escolherá um tipo de dados que permite que o maior tamanho de dados. Por exemplo, Microsoft SQL Server usará `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` se a coluna é usada como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando o SQL Server, isso resultaria no `nvarchar(500)` tipo de dados que está sendo usado.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
