---
title: Colunas computadas-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655914"
---
# <a name="computed-columns"></a>Colunas computadas

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma coluna computada é uma coluna cujo valor é calculado no banco de dados. Uma coluna computada pode usar outras colunas na tabela para calcular seu valor.

## <a name="conventions"></a>Convenções

Por convenção, as colunas computadas não são criadas no modelo.

## <a name="data-annotations"></a>Anotações de dados

Colunas computadas não podem ser configuradas com anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar que uma propriedade deve ser mapeada para uma coluna computada.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
