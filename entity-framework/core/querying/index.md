---
title: Consultar Dados – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413111"
---
# <a name="querying-data"></a>Consultar Dados

O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados. O LINQ permite que você use o C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas. Ele usa o contexto derivado e as classes de entidade para fazer referência a objetos de banco de dados. O EF Core passa uma representação da consulta LINQ para o provedor de banco de dados. Os provedores de banco de dados, por sua vez, a convertem em uma linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional).

> [!TIP]
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

Os snippets a seguir mostram alguns exemplos de como realizar tarefas comuns com o Entity Framework Core.

## <a name="loading-all-data"></a>Como carregar todos os dados

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Como carregar uma única entidade

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtragem

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Leituras adicionais

- Saiba mais sobre as [expressões de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Para obter mais informações sobre como uma consulta é processada no EF Core, confira [Como funciona a consulta](xref:core/querying/how-query-works).
