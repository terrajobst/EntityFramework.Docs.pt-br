---
title: Chaves alternativas (restrições exclusivas)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197606"
---
# <a name="alternate-keys-unique-constraints"></a>Chaves alternativas (restrições exclusivas)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma restrição UNIQUE é introduzida para cada chave alternativa no modelo.

## <a name="conventions"></a>Convenções

Por convenção, o índice e a restrição que são introduzidos para uma chave alternativa serão `AK_<type name>_<property name>`nomeados. Para chaves `<property name>` alternativas compostas se torna uma lista de nomes de propriedade separados por sublinhado.

## <a name="data-annotations"></a>Anotações de dados

Restrições exclusivas não podem ser configuradas usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o índice e o nome da restrição para uma chave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
