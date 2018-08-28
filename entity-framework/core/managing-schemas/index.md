---
title: Gerenciar Esquemas de Banco de Dados – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994379"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="d5597-102">Gerenciar Esquemas de Banco de Dados</span><span class="sxs-lookup"><span data-stu-id="d5597-102">Managing Database Schemas</span></span>
<span data-ttu-id="d5597-103">O EF Core oferece duas maneiras principais de manter seu esquema de banco de dados e modelo do EF Core em sincronia. Para escolher entre as duas, decida se seu modelo do EF Core ou o esquema de banco de dados é a fonte da verdade.</span><span class="sxs-lookup"><span data-stu-id="d5597-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="d5597-104">Se quiser que seu modelo do EF Core seja a fonte da verdade, use [Migrações][1].</span><span class="sxs-lookup"><span data-stu-id="d5597-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="d5597-105">Uma vez que você pode fazer alterações ao seu modelo do EF Core, essa abordagem aplica incrementalmente as alterações de esquema correspondentes ao banco de dados de modo que permaneçam compatíveis com o modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5597-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="d5597-106">Use [Engenharia Reversa][2] se quiser que o esquema de banco de dados seja a fonte da verdade.</span><span class="sxs-lookup"><span data-stu-id="d5597-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="d5597-107">Essa abordagem permite que você faça o scaffold de um DbContext e das classes de tipo de entidade realizando engenharia reversa do esquema de banco de dados para um modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5597-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="d5597-108">A opção [criar e remover APIs][3] também pode criar o esquema de banco de dados usando seu modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5597-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="d5597-109">No entanto, eles são principalmente para teste, criação de protótipos e outros cenários em que remover o banco de dados é aceitável.</span><span class="sxs-lookup"><span data-stu-id="d5597-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
