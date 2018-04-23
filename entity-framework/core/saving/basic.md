---
title: Salvar básico - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>Salvar básico

Saiba como adicionar, modificar e remover dados usando as classes de entidade e de contexto.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) deste artigo no GitHub.

## <a name="adding-data"></a>Adicionando dados

Use o *DbSet.Add* método para adicionar novas instâncias de suas classes de entidade. Os dados serão inseridos no banco de dados quando você chama *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Os métodos de atualização, anexar e adicionar todos eles funcionam no gráfico completo das entidades passados a eles, conforme descrito no [dados relacionados](related-data.md) seção. Como alternativa, a propriedade EntityEntry.State pode ser usada para definir o estado de apenas uma única entidade. Por exemplo, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Atualizando dados

EF detectará automaticamente as alterações feitas a uma entidade existente que é controlada pelo contexto. Isso inclui entidades que você carregar/consulta do banco de dados e entidades que foram adicionadas anteriormente e salvos no banco de dados.

Basta modificar os valores atribuídos às propriedades e, em seguida, chamar *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Excluindo dados

Use o *DbSet.Remove* método para excluir instâncias que classes de entidade.

Se a entidade já existe no banco de dados, ele será excluído durante *SaveChanges*. Se a entidade ainda não foram salvas no banco de dados (ou seja, ele é controlado como adicionado) e em seguida, ele será removido do contexto e deixará de ser inserido quando *SaveChanges* é chamado.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Várias operações em um único SaveChanges

Você pode combinar várias operações de atualização/Adicionar/remover em uma única chamada para *SaveChanges*.

> [!NOTE]  
> Para a maioria dos provedores de banco de dados, *SaveChanges* é transacional. Isso significa que todas as operações serão bem-sucedidas ou falham e as operações serão nunca esquerda parcialmente aplicadas.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
