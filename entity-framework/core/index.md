---
title: Visão geral do Entity Framework Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412831"
---
# <a name="entity-framework-core"></a><span data-ttu-id="8b842-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8b842-102">Entity Framework Core</span></span>

<span data-ttu-id="8b842-103">O EF (Entity Framework) Core é uma versão leve, extensível, de [software livre](https://github.com/aspnet/EntityFrameworkCore) e multiplataforma da popular tecnologia de acesso a dados do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8b842-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="8b842-104">O EF Core pode servir como um ORM (Mapeador de Objeto Relacional), permitindo que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos do .NET e eliminando a necessidade de grande parte do código de acesso aos dados que eles geralmente precisam escrever.</span><span class="sxs-lookup"><span data-stu-id="8b842-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="8b842-105">O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8b842-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="8b842-106">O modelo</span><span class="sxs-lookup"><span data-stu-id="8b842-106">The Model</span></span>

<span data-ttu-id="8b842-107">Com o EF Core, o acesso a dados é executado usando um modelo.</span><span class="sxs-lookup"><span data-stu-id="8b842-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="8b842-108">Um modelo é composto por classes de entidade e um objeto de contexto que representa uma sessão com o banco de dados, o que permite consultar e salvar dados.</span><span class="sxs-lookup"><span data-stu-id="8b842-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="8b842-109">Consulte [Criar um modelo](modeling/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="8b842-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="8b842-110">Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com o banco de dados ou usar [migrações da classe EF](managing-schemas/migrations/index.md) para criar um banco de dados do modelo e, em seguida, desenvolvê-lo à medida que o modelo for alterado com o tempo.</span><span class="sxs-lookup"><span data-stu-id="8b842-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="8b842-111">Consultas</span><span class="sxs-lookup"><span data-stu-id="8b842-111">Querying</span></span>

<span data-ttu-id="8b842-112">Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem).</span><span class="sxs-lookup"><span data-stu-id="8b842-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="8b842-113">Consulte [Consultar dados](querying/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="8b842-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="8b842-114">Salvar Dados</span><span class="sxs-lookup"><span data-stu-id="8b842-114">Saving Data</span></span>

<span data-ttu-id="8b842-115">Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade.</span><span class="sxs-lookup"><span data-stu-id="8b842-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="8b842-116">Consulte [Salvar dados](saving/index.md) para saber mais.</span><span class="sxs-lookup"><span data-stu-id="8b842-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="8b842-117">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="8b842-117">Next steps</span></span>

<span data-ttu-id="8b842-118">Para ver tutoriais introdutórios, confira [Introdução ao Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8b842-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
