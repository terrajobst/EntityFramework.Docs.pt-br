---
title: "Salvar básico - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Salvar básico

Saiba como adicionar, modificar e remover dados usando as classes de entidade e de contexto.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) no GitHub.

## <a name="adding-data"></a>Adicionando dados

Use o *DbSet.Add* método para adicionar novas instâncias de suas classes de entidade. Os dados serão inseridos no banco de dados quando você chama *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

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
