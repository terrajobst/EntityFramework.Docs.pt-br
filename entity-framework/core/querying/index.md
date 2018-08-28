---
title: Consultar Dados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993528"
---
# <a name="querying-data"></a><span data-ttu-id="1c343-102">Consultar Dados</span><span class="sxs-lookup"><span data-stu-id="1c343-102">Querying Data</span></span>

<span data-ttu-id="1c343-103">O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1c343-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="1c343-104">O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade.</span><span class="sxs-lookup"><span data-stu-id="1c343-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="1c343-105">Uma representação da consulta LINQ é passada para o provedor de banco de dados, para ser convertida em linguagem de consulta específica do banco de dados (por exemplo, SQL para um banco de dados relacional).</span><span class="sxs-lookup"><span data-stu-id="1c343-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="1c343-106">Para obter mais informações sobre como uma consulta é processada, consulte [Como funciona a consulta](overview.md).</span><span class="sxs-lookup"><span data-stu-id="1c343-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
