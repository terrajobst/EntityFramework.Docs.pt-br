---
title: Mapeamento de coluna – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929856"
---
# <a name="column-mapping"></a>Mapeamento de coluna

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Mapeamento de coluna identifica quais dados de coluna devem ser consultados a partir e salvos no banco de dados.

## <a name="conventions"></a>Convenções

Por convenção, cada propriedade será definida para cima para mapear para uma coluna com o mesmo nome que a propriedade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar anotações de dados para configurar a coluna à qual uma propriedade é mapeada.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar a coluna à qual uma propriedade é mapeada.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
