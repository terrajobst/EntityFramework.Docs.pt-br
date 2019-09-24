---
title: Tipos de dados-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197183"
---
# <a name="data-types"></a><span data-ttu-id="e22b6-102">Tipos de Dados</span><span class="sxs-lookup"><span data-stu-id="e22b6-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="e22b6-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="e22b6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="e22b6-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="e22b6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="e22b6-105">O tipo de dados refere-se ao tipo específico de Database da coluna para a qual uma propriedade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="e22b6-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="e22b6-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="e22b6-106">Conventions</span></span>

<span data-ttu-id="e22b6-107">Por convenção, o provedor de banco de dados seleciona um tipo de dado com base no tipo .NET da propriedade.</span><span class="sxs-lookup"><span data-stu-id="e22b6-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="e22b6-108">Ele também leva em conta outros metadados, como o [comprimento máximo](../max-length.md)configurado, se a propriedade faz parte de uma chave primária, etc.</span><span class="sxs-lookup"><span data-stu-id="e22b6-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="e22b6-109">Por exemplo, SQL Server usa `datetime2(7)` para `DateTime` Propriedades e `nvarchar(max)` para `string` Propriedades (ou `nvarchar(450)` para `string` propriedades que são usadas como uma chave).</span><span class="sxs-lookup"><span data-stu-id="e22b6-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e22b6-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="e22b6-110">Data Annotations</span></span>

<span data-ttu-id="e22b6-111">Você pode usar as anotações de dados para especificar um tipo de dados exato para uma coluna.</span><span class="sxs-lookup"><span data-stu-id="e22b6-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="e22b6-112">Por `Url` exemplo, o código a seguir configura como uma cadeia de caracteres não-Unicode com `200` comprimento máximo `Rating` e como decimal com precisão `5` e escala de `2`.</span><span class="sxs-lookup"><span data-stu-id="e22b6-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="e22b6-113">API fluente</span><span class="sxs-lookup"><span data-stu-id="e22b6-113">Fluent API</span></span>

<span data-ttu-id="e22b6-114">Você também pode usar a API fluente para especificar os mesmos tipos de dados para as colunas.</span><span class="sxs-lookup"><span data-stu-id="e22b6-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
