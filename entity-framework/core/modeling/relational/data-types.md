---
title: Tipos de dados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a><span data-ttu-id="3c227-102">Tipos de dados</span><span class="sxs-lookup"><span data-stu-id="3c227-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="3c227-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="3c227-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3c227-104">Os métodos de extensão mostrados aqui estará disponíveis quando você instala um provedor de banco de dados relacional (devido a compartilhado *Microsoft.EntityFrameworkCore.Relational* pacote).</span><span class="sxs-lookup"><span data-stu-id="3c227-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3c227-105">Tipo de dados refere-se para o tipo específico de banco de dados da coluna à qual uma propriedade está mapeada.</span><span class="sxs-lookup"><span data-stu-id="3c227-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="3c227-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="3c227-106">Conventions</span></span>

<span data-ttu-id="3c227-107">Por convenção, o provedor de banco de dados seleciona um tipo de dados com base no tipo CLR da propriedade.</span><span class="sxs-lookup"><span data-stu-id="3c227-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="3c227-108">Ele também leva em conta outros metadados, como configurado [comprimento máximo](../max-length.md), se a propriedade é parte de uma chave primária, etc.</span><span class="sxs-lookup"><span data-stu-id="3c227-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="3c227-109">Por exemplo, o SQL Server usa `datetime2(7)` para `DateTime` propriedades, e `nvarchar(max)` para `string` propriedades (ou `nvarchar(450)` para `string` propriedades que são usadas como uma chave).</span><span class="sxs-lookup"><span data-stu-id="3c227-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3c227-110">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="3c227-110">Data Annotations</span></span>

<span data-ttu-id="3c227-111">Você pode usar as anotações de dados para especificar um tipo de dados exato de uma coluna.</span><span class="sxs-lookup"><span data-stu-id="3c227-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="3c227-112">Por exemplo de código a seguir configura `Url` como uma cadeia de caracteres não unicode com um comprimento máximo de `200` e `Rating` como decimal com precisão de `5` e a escala de `2`.</span><span class="sxs-lookup"><span data-stu-id="3c227-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="3c227-113">API fluente</span><span class="sxs-lookup"><span data-stu-id="3c227-113">Fluent API</span></span>

<span data-ttu-id="3c227-114">Você também pode usar a API fluente para especificar os mesmos tipos de dados para as colunas.</span><span class="sxs-lookup"><span data-stu-id="3c227-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
