---
title: Comprimento máximo-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197217"
---
# <a name="maximum-length"></a>Comprimento máximo

A configuração de um comprimento máximo fornece uma dica para o armazenamento de dados sobre o tipo de dados apropriado a ser usado para uma determinada propriedade. O `string` comprimento máximo só se aplica a tipos de dados de matriz `byte[]`, como e.

> [!NOTE]  
> Entity Framework não realiza nenhuma validação de comprimento máximo antes de passar os dados para o provedor. Cabe ao provedor ou repositório de dados validar se apropriado. Por exemplo, ao direcionar SQL Server, exceder o comprimento máximo resultará em uma exceção, pois o tipo de dados da coluna subjacente não permitirá que os dados em excesso sejam armazenados.

## <a name="conventions"></a>Convenções

Por convenção, ele é deixado para o provedor de banco de dados para escolher um tipo de dado apropriado para propriedades. Para propriedades que têm um comprimento, o provedor de banco de dados geralmente escolherá um tipo de dado que permite o comprimento mais longo dos dados. Por exemplo, Microsoft SQL Server será usado `nvarchar(max)` para `string` Propriedades (ou `nvarchar(450)` se a coluna for usada como uma chave).

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando SQL Server isso resultaria no tipo `nvarchar(500)` de dados que está sendo usado.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar um comprimento máximo para uma propriedade. Neste exemplo, direcionando SQL Server isso resultaria no tipo `nvarchar(500)` de dados que está sendo usado.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
