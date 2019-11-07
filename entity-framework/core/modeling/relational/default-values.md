---
title: Valores padrão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655912"
---
# <a name="default-values"></a>Valores padrão

> [!NOTE]  
> A configuração nesta seção é aplicável a bancos de dados relacionais em geral. Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).

O valor padrão de uma coluna é o valor que será inserido se uma nova linha for inserida, mas nenhum valor for especificado para a coluna.

## <a name="conventions"></a>Convenções

Por convenção, um valor padrão não é configurado.

## <a name="data-annotations"></a>Anotações de dados

Você não pode definir um valor padrão usando anotações de dados.

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para especificar o valor padrão de uma propriedade.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Você também pode especificar um fragmento SQL que é usado para calcular o valor padrão.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
