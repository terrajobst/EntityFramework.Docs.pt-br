---
title: Criar e configurar um modelo – EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412771"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="13f9d-102">Criar e configurar um modelo</span><span class="sxs-lookup"><span data-stu-id="13f9d-102">Creating and configuring a model</span></span>

<span data-ttu-id="13f9d-103">O Entity Framework usa um conjunto de convenções para criar um modelo com base na forma de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="13f9d-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="13f9d-104">Você pode especificar configurações adicionais para complementar e/ou substituir o que foi descoberto por convenção.</span><span class="sxs-lookup"><span data-stu-id="13f9d-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="13f9d-105">Este artigo aborda a configuração que pode ser aplicada a um modelo que tenha como destino qualquer armazenamento de dados e que possa ser aplicado ao ter qualquer banco de dados relacional como destino.</span><span class="sxs-lookup"><span data-stu-id="13f9d-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="13f9d-106">Os provedores também podem habilitar configuração específica de um armazenamento de dados em particular.</span><span class="sxs-lookup"><span data-stu-id="13f9d-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="13f9d-107">Para mais informações sobre a configuração específica do provedor, consulte a seção [Provedor de Banco de Dados](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="13f9d-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="13f9d-108">Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) deste artigo no GitHub.</span><span class="sxs-lookup"><span data-stu-id="13f9d-108">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="13f9d-109">Usar a API fluente para configurar um modelo</span><span class="sxs-lookup"><span data-stu-id="13f9d-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="13f9d-110">Você pode modificar o método `OnModelCreating` no contexto derivado e usar o `ModelBuilder API` para configurar seu modelo.</span><span class="sxs-lookup"><span data-stu-id="13f9d-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="13f9d-111">Este é o método mais eficiente de configuração e permite que a configuração seja especificada sem modificação de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="13f9d-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="13f9d-112">A configuração da API fluente tem a precedência mais alta e substituirá as anotações de dados e as convenções.</span><span class="sxs-lookup"><span data-stu-id="13f9d-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="13f9d-113">Usar anotações de dados para configurar um modelo</span><span class="sxs-lookup"><span data-stu-id="13f9d-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="13f9d-114">Você também pode aplicar atributos (conhecidos como Anotações de Dados) a classes e propriedades.</span><span class="sxs-lookup"><span data-stu-id="13f9d-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="13f9d-115">As anotações de dados substituirão as convenções, mas serão substituídas pela configuração da API Fluente.</span><span class="sxs-lookup"><span data-stu-id="13f9d-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
