---
title: Chaves alternativas (restrições exclusivas) - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052786"
---
# <a name="alternate-keys-unique-constraints"></a>Chaves alternativas (restrições exclusivas)

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).

Uma restrição exclusiva é introduzida para cada chave alternativa no modelo.

## <a name="conventions"></a>Convenções

Por convenção, o índice e a restrição que são apresentados para uma chave alternativa serão nomeados `AK_<type name>_<property name>`. Para chaves compostas de alternativas `<property name>` torna-se uma lista de sublinhado separado dos nomes de propriedade.

## <a name="data-annotations"></a>Anotações de dados

Restrições UNIQUE não podem ser configuradas usando as anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar o nome de índice e de restrição para uma chave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
