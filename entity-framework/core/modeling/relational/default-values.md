---
title: Valores padrão-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655912"
---
# <a name="default-values"></a><span data-ttu-id="da636-102">Valores padrão</span><span class="sxs-lookup"><span data-stu-id="da636-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="da636-103">A configuração nesta seção é aplicável a bancos de dados relacionais em geral.</span><span class="sxs-lookup"><span data-stu-id="da636-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="da636-104">Os métodos de extensão mostrados aqui ficarão disponíveis quando você instalar um provedor de banco de dados relacional (devido ao pacote *Microsoft.EntityFrameworkCore.Relational* compartilhado).</span><span class="sxs-lookup"><span data-stu-id="da636-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="da636-105">O valor padrão de uma coluna é o valor que será inserido se uma nova linha for inserida, mas nenhum valor for especificado para a coluna.</span><span class="sxs-lookup"><span data-stu-id="da636-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="da636-106">Convenções</span><span class="sxs-lookup"><span data-stu-id="da636-106">Conventions</span></span>

<span data-ttu-id="da636-107">Por convenção, um valor padrão não é configurado.</span><span class="sxs-lookup"><span data-stu-id="da636-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="da636-108">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="da636-108">Data Annotations</span></span>

<span data-ttu-id="da636-109">Você não pode definir um valor padrão usando anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="da636-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="da636-110">API fluente</span><span class="sxs-lookup"><span data-stu-id="da636-110">Fluent API</span></span>

<span data-ttu-id="da636-111">Você pode usar a API fluente para especificar o valor padrão de uma propriedade.</span><span class="sxs-lookup"><span data-stu-id="da636-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="da636-112">Você também pode especificar um fragmento SQL que é usado para calcular o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="da636-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
