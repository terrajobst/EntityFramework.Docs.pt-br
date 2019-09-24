---
title: Mapeamento de coluna-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197203"
---
# <a name="column-mapping"></a><span data-ttu-id="d8ab6-102">Mapeamento de coluna</span><span class="sxs-lookup"><span data-stu-id="d8ab6-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="d8ab6-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="d8ab6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d8ab6-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="d8ab6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d8ab6-105">O mapeamento de coluna identifica quais dados de coluna devem ser consultados e salvos no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="d8ab6-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8ab6-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="d8ab6-106">Conventions</span></span>

<span data-ttu-id="d8ab6-107">Por convenção, cada propriedade será configurada para mapear para uma coluna com o mesmo nome da propriedade.</span><span class="sxs-lookup"><span data-stu-id="d8ab6-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d8ab6-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="d8ab6-108">Data Annotations</span></span>

<span data-ttu-id="d8ab6-109">Você pode usar as anotações de dados para configurar a coluna na qual uma propriedade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="d8ab6-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="d8ab6-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="d8ab6-110">Fluent API</span></span>

<span data-ttu-id="d8ab6-111">Você pode usar a API Fluent para configurar a coluna para a qual uma propriedade é mapeada.</span><span class="sxs-lookup"><span data-stu-id="d8ab6-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
