---
title: Chaves alternativas (restrições exclusivas) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994185"
---
# <a name="alternate-keys-unique-constraints"></a>Chaves alternativas (restrições exclusivas)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

Uma restrição unique é introduzida para cada chave alternativa no modelo.

## <a name="conventions"></a>Convenções

Por convenção, o índice e a restrição que são apresentados para uma chave alternativa serão nomeados `AK_<type name>_<property name>`. Para chaves compostas de alternativas `<property name>` se tornará uma lista de sublinhado separado de nomes de propriedade.

## <a name="data-annotations"></a>Anotações de dados

As restrições UNIQUE não podem ser configuradas usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API Fluent para configurar o nome de índice e de restrição para uma chave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
