---
title: Mapeamento de coluna – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929856"
---
# <a name="column-mapping"></a><span data-ttu-id="fe7bf-102">Mapeamento de coluna</span><span class="sxs-lookup"><span data-stu-id="fe7bf-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="fe7bf-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="fe7bf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="fe7bf-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="fe7bf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="fe7bf-105">Mapeamento de coluna identifica quais dados de coluna devem ser consultados a partir e salvos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fe7bf-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fe7bf-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="fe7bf-106">Conventions</span></span>

<span data-ttu-id="fe7bf-107">Por convenção, cada propriedade será definida para cima para mapear para uma coluna com o mesmo nome que a propriedade.</span><span class="sxs-lookup"><span data-stu-id="fe7bf-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fe7bf-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="fe7bf-108">Data Annotations</span></span>

<span data-ttu-id="fe7bf-109">Você pode usar anotações de dados para configurar a coluna à qual uma propriedade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="fe7bf-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="fe7bf-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="fe7bf-110">Fluent API</span></span>

<span data-ttu-id="fe7bf-111">Você pode usar a API Fluent para configurar a coluna à qual uma propriedade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="fe7bf-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
