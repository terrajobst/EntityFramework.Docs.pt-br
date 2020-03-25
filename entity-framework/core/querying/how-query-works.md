---
title: Como funciona a consulta – EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136236"
---
# <a name="how-queries-work"></a><span data-ttu-id="1afc0-102">Como funciona a consulta</span><span class="sxs-lookup"><span data-stu-id="1afc0-102">How Queries Work</span></span>

<span data-ttu-id="1afc0-103">O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1afc0-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="1afc0-104">O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade.</span><span class="sxs-lookup"><span data-stu-id="1afc0-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="1afc0-105">A vida de uma consulta</span><span class="sxs-lookup"><span data-stu-id="1afc0-105">The life of a query</span></span>

<span data-ttu-id="1afc0-106">Veja a seguir uma visão geral de alto nível do processo pelo qual cada consulta passa.</span><span class="sxs-lookup"><span data-stu-id="1afc0-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="1afc0-107">A consulta LINQ é processada pelo Entity Framework Core para criar uma representação que está pronta para ser processada pelo provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="1afc0-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="1afc0-108">O resultado é armazenado em cache para que esse processamento não precise ser feito sempre que a consulta for executada</span><span class="sxs-lookup"><span data-stu-id="1afc0-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="1afc0-109">O resultado é passado para o provedor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="1afc0-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="1afc0-110">O provedor do banco de dados identifica quais partes da consulta podem ser avaliadas no banco de dados</span><span class="sxs-lookup"><span data-stu-id="1afc0-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="1afc0-111">Essas partes da consulta são convertidas em linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional)</span><span class="sxs-lookup"><span data-stu-id="1afc0-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="1afc0-112">Uma consulta é enviada ao banco de dados e o conjunto de resultados retornado (resultados são valores do banco de dados, não instâncias de entidade)</span><span class="sxs-lookup"><span data-stu-id="1afc0-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="1afc0-113">Para cada item no conjunto de resultados</span><span class="sxs-lookup"><span data-stu-id="1afc0-113">For each item in the result set</span></span>
   1. <span data-ttu-id="1afc0-114">Se for um rastreamento de consulta, o EF verificará se os dados representam uma entidade já no controlador de alterações para a instância de contexto</span><span class="sxs-lookup"><span data-stu-id="1afc0-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="1afc0-115">Nesse caso, a entidade existente será retornada</span><span class="sxs-lookup"><span data-stu-id="1afc0-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="1afc0-116">Caso contrário, uma nova entidade será criada, o controle de alterações será configurado e a nova entidade será retornada</span><span class="sxs-lookup"><span data-stu-id="1afc0-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="1afc0-117">Se esta for uma consulta sem rastreamento, uma nova entidade será sempre criada e retornada</span><span class="sxs-lookup"><span data-stu-id="1afc0-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="1afc0-118">Quando as consultas são executadas</span><span class="sxs-lookup"><span data-stu-id="1afc0-118">When queries are executed</span></span>

<span data-ttu-id="1afc0-119">Quando você chama operadores LINQ, está criando simplesmente uma representação na memória da consulta.</span><span class="sxs-lookup"><span data-stu-id="1afc0-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="1afc0-120">A consulta é enviada somente para o banco de dados quando os resultados são consumidos.</span><span class="sxs-lookup"><span data-stu-id="1afc0-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="1afc0-121">As operações mais comuns que resultam na consulta que está sendo enviada para o banco de dados são:</span><span class="sxs-lookup"><span data-stu-id="1afc0-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="1afc0-122">Iteração dos resultados em um loop `for`</span><span class="sxs-lookup"><span data-stu-id="1afc0-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="1afc0-123">Usando um operador como `ToList`, `ToArray`, `Single``Count` ou as sobrecargas assíncronas equivalentes</span><span class="sxs-lookup"><span data-stu-id="1afc0-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="1afc0-124">**Sempre validar a entrada do usuário:** enquanto o EF Core protege contra ataques de injeção SQL usando parâmetros e ignorando literais em consultas, ele não valida entradas.</span><span class="sxs-lookup"><span data-stu-id="1afc0-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="1afc0-125">A validação apropriada, de acordo com os requisitos do aplicativo, deve ser executada antes que os valores de fontes não confiáveis sejam usados em consultas LINQ, atribuídas a propriedades de entidade ou transmitidas para outras APIs de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1afc0-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="1afc0-126">Isso inclui qualquer entrada do usuário usada para construir consultas dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="1afc0-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="1afc0-127">Mesmo ao usar o LINQ, se você está aceitando entrada do usuário para criar expressões, precisa para garantir que apenas as expressões pretendidas possam ser criadas.</span><span class="sxs-lookup"><span data-stu-id="1afc0-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
