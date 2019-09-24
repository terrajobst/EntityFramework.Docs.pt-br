---
title: Comprimento máximo-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197217"
---
# <a name="maximum-length"></a><span data-ttu-id="d0a27-102">Comprimento máximo</span><span class="sxs-lookup"><span data-stu-id="d0a27-102">Maximum Length</span></span>

<span data-ttu-id="d0a27-103">A configuração de um comprimento máximo fornece uma dica para o armazenamento de dados sobre o tipo de dados apropriado a ser usado para uma determinada propriedade.</span><span class="sxs-lookup"><span data-stu-id="d0a27-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="d0a27-104">O `string` comprimento máximo só se aplica a tipos de dados de matriz `byte[]`, como e.</span><span class="sxs-lookup"><span data-stu-id="d0a27-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="d0a27-105">Entity Framework não realiza nenhuma validação de comprimento máximo antes de passar os dados para o provedor.</span><span class="sxs-lookup"><span data-stu-id="d0a27-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="d0a27-106">Cabe ao provedor ou repositório de dados validar se apropriado.</span><span class="sxs-lookup"><span data-stu-id="d0a27-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="d0a27-107">Por exemplo, ao direcionar SQL Server, exceder o comprimento máximo resultará em uma exceção, pois o tipo de dados da coluna subjacente não permitirá que os dados em excesso sejam armazenados.</span><span class="sxs-lookup"><span data-stu-id="d0a27-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="d0a27-108">Convenções</span><span class="sxs-lookup"><span data-stu-id="d0a27-108">Conventions</span></span>

<span data-ttu-id="d0a27-109">Por convenção, ele é deixado para o provedor de banco de dados para escolher um tipo de dado apropriado para propriedades.</span><span class="sxs-lookup"><span data-stu-id="d0a27-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="d0a27-110">Para propriedades que têm um comprimento, o provedor de banco de dados geralmente escolherá um tipo de dado que permite o comprimento mais longo dos dados.</span><span class="sxs-lookup"><span data-stu-id="d0a27-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="d0a27-111">Por exemplo, Microsoft SQL Server será usado `nvarchar(max)` para `string` Propriedades (ou `nvarchar(450)` se a coluna for usada como uma chave).</span><span class="sxs-lookup"><span data-stu-id="d0a27-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d0a27-112">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d0a27-112">Data Annotations</span></span>

<span data-ttu-id="d0a27-113">Você pode usar as anotações de dados para configurar um comprimento máximo para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="d0a27-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="d0a27-114">Neste exemplo, direcionando SQL Server isso resultaria no tipo `nvarchar(500)` de dados que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="d0a27-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="d0a27-115">API fluente</span><span class="sxs-lookup"><span data-stu-id="d0a27-115">Fluent API</span></span>

<span data-ttu-id="d0a27-116">Você pode usar a API Fluent para configurar um comprimento máximo para uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="d0a27-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="d0a27-117">Neste exemplo, direcionando SQL Server isso resultaria no tipo `nvarchar(500)` de dados que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="d0a27-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
