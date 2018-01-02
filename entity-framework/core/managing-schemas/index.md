---
title: "Gerenciar Esquemas de Banco de Dados – EF Core"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a><span data-ttu-id="08e18-102">Gerenciar Esquemas de Banco de Dados</span><span class="sxs-lookup"><span data-stu-id="08e18-102">Managing Database Schemas</span></span>
<span data-ttu-id="08e18-103">O EF Core oferece duas maneiras principais de manter seu esquema de banco de dados e modelo do EF Core em sincronia. Para escolher entre as duas, decida se seu modelo do EF Core ou o esquema de banco de dados é a fonte da verdade.</span><span class="sxs-lookup"><span data-stu-id="08e18-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="08e18-104">Se quiser que seu modelo do EF Core seja a fonte da verdade, use [Migrações][1].</span><span class="sxs-lookup"><span data-stu-id="08e18-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="08e18-105">Uma vez que você pode fazer alterações ao seu modelo do EF Core, essa abordagem aplica incrementalmente as alterações de esquema correspondentes ao banco de dados de modo que permaneçam compatíveis com o modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="08e18-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="08e18-106">Use [Engenharia Reversa][2] se quiser que o esquema de banco de dados seja a fonte da verdade.</span><span class="sxs-lookup"><span data-stu-id="08e18-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="08e18-107">Essa abordagem permite que você faça o scaffold de um DbContext e das classes de tipo de entidade realizando engenharia reversa do esquema de banco de dados para um modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="08e18-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="08e18-108">A opção [criar e remover APIs][3] também pode criar o esquema de banco de dados usando seu modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="08e18-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="08e18-109">No entanto, eles são principalmente para teste, criação de protótipos e outros cenários em que remover o banco de dados é aceitável.</span><span class="sxs-lookup"><span data-stu-id="08e18-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
