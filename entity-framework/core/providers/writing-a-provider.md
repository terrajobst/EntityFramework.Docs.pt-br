---
title: Gravando um provedor de banco de dados-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417879"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="56f64-102">Escrever um provedor de dados</span><span class="sxs-lookup"><span data-stu-id="56f64-102">Writing a Database Provider</span></span>

<span data-ttu-id="56f64-103">Para obter informações sobre como escrever um provedor de banco de dados Entity Framework Core, consulte para [escrever um provedor de EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) de [Arthur Vicker](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="56f64-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="56f64-104">Essas postagens não foram atualizadas desde EF Core 1,1 e houve alterações significativas desde que o [problema 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) está acompanhando as atualizações desta documentação.</span><span class="sxs-lookup"><span data-stu-id="56f64-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="56f64-105">O EF Core codebase é código aberto e contém vários provedores de banco de dados que podem ser usados como referência.</span><span class="sxs-lookup"><span data-stu-id="56f64-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="56f64-106">Você pode encontrar o código-fonte em <https://github.com/aspnet/EntityFrameworkCore>.</span><span class="sxs-lookup"><span data-stu-id="56f64-106">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="56f64-107">Também pode ser útil examinar o código para provedores de terceiros comumente usados, como [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)e [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="56f64-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="56f64-108">Em particular, esses projetos são configurados para estender e executar testes funcionais que publicamos no NuGet.</span><span class="sxs-lookup"><span data-stu-id="56f64-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="56f64-109">Esse tipo de configuração é altamente recomendável.</span><span class="sxs-lookup"><span data-stu-id="56f64-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="56f64-110">Mantendo-se atualizado com as alterações do provedor</span><span class="sxs-lookup"><span data-stu-id="56f64-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="56f64-111">Começando com o trabalho após a versão 2,1, criamos um [log de alterações](provider-log.md) que podem precisar de alterações correspondentes no código do provedor.</span><span class="sxs-lookup"><span data-stu-id="56f64-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="56f64-112">Isso é destinado a ajudar ao atualizar um provedor existente para trabalhar com uma nova versão do EF Core.</span><span class="sxs-lookup"><span data-stu-id="56f64-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="56f64-113">Antes de 2,1, usamos os rótulos [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) em nossos problemas do GitHub e solicitações pull para uma finalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="56f64-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="56f64-114">Vamos continiue usar esses rótulos em problemas para dar uma indicação de quais itens de trabalho em uma determinada versão também podem exigir que o trabalho seja feito em provedores.</span><span class="sxs-lookup"><span data-stu-id="56f64-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="56f64-115">Um rótulo de `providers-beware` normalmente significa que a implementação de um item de trabalho pode interromper provedores, enquanto um rótulo de `providers-fyi` normalmente significa que os provedores não serão interrompidos, mas talvez o código precise ser alterado de qualquer forma, por exemplo, para habilitar a nova funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="56f64-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="56f64-116">Nomenclatura sugerida de provedores de terceiros</span><span class="sxs-lookup"><span data-stu-id="56f64-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="56f64-117">Sugerimos o uso da seguinte nomenclatura para pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="56f64-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="56f64-118">Isso é consistente com os nomes dos pacotes fornecidos pela equipe de EF Core.</span><span class="sxs-lookup"><span data-stu-id="56f64-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="56f64-119">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="56f64-119">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
