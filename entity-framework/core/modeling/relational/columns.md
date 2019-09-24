---
title: Mapeamento de coluna-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197203"
---
# <a name="column-mapping"></a>Mapeamento de coluna

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O mapeamento de coluna identifica quais dados de coluna devem ser consultados e salvos no banco de dado.

## <a name="conventions"></a>Convenções

Por convenção, cada propriedade será configurada para mapear para uma coluna com o mesmo nome da propriedade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar a coluna na qual uma propriedade é mapeada.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar a coluna para a qual uma propriedade é mapeada.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
