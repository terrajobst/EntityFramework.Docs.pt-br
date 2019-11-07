---
title: Sequências-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656108"
---
# <a name="sequences"></a>Sequências

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma sequência gera valores numéricos sequenciais no banco de dados. As sequências não estão associadas a uma tabela específica.

## <a name="conventions"></a>Convenções

Por convenção, as sequências não são introduzidas no modelo.

## <a name="data-annotations"></a>Anotações de dados

Você não pode configurar uma sequência usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para criar uma sequência no modelo.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Você também pode configurar o aspecto adicional da sequência, como seu esquema, valor inicial e incremento.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Depois que uma sequência é introduzida, você pode usá-la para gerar valores para propriedades em seu modelo. Por exemplo, você pode usar [valores padrão](default-values.md) para inserir o próximo valor da sequência.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
