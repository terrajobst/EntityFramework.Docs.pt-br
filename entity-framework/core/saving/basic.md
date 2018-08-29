---
title: Salvamento básico – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447805"
---
# <a name="basic-save"></a>Salvamento básico

Saiba como adicionar, modificar e remover dados usando as classes de entidade e contexto.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) deste artigo no GitHub.

## <a name="adding-data"></a>Adicionando dados

Use o método *DbSet.Add* para adicionar novas instâncias de suas classes de entidade. Os dados serão inseridos no banco de dados quando você chamar *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Os métodos Adicionar, Anexar e Atualizar funcionam no gráfico completo de entidades aprovadas para eles, conforme descrito na seção [Dados Relacionados](related-data.md). Como alternativa, a propriedade EntityEntry.State pode ser usada para definir o estado de uma única entidade. Por exemplo, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Atualizando dados

O EF detectará automaticamente as alterações feitas em uma entidade existente que é controlada pelo contexto. Isso inclui as entidades que você carrega/consulta do banco de dados e as entidades que foram anteriormente adicionadas e salvas no banco de dados.

Basta modificar os valores atribuídos às propriedades e chamar *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Excluindo dados

Use o método *DbSet.Remove* para excluir instâncias das suas classes de entidade.

Se a entidade já existir no banco de dados, ela será excluída durante *SaveChanges*. Se a entidade ainda não tiver sido salva no banco de dados (ou seja, ela for rastreada como adicionada), ela será removida do contexto e não será mais inserida quando *SaveChanges* for chamado.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Várias operações em um único SaveChanges

Você pode combinar várias operações Adicionar/Carregar/Remover em uma única chamada para *SaveChanges*.

> [!NOTE]  
> Para a maioria dos provedores de banco de dados, *SaveChanges* é transacional. Isso significa que todas as operações serão bem-sucedidas ou falharão e as operações nunca ficarão parcialmente aplicadas.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
