---
title: Atualizando do EF Core 1,0 RC2 para RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181358"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="f67e8-102">Atualizando do EF Core 1,0 RC2 para RTM</span><span class="sxs-lookup"><span data-stu-id="f67e8-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="f67e8-103">Este artigo fornece diretrizes para mover um aplicativo criado com os pacotes RC2 para o 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="f67e8-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="f67e8-104">Versões do pacote</span><span class="sxs-lookup"><span data-stu-id="f67e8-104">Package Versions</span></span>

<span data-ttu-id="f67e8-105">Os nomes dos pacotes de nível superior que você normalmente instalaria em um aplicativo não foram alterados entre o RC2 e o RTM.</span><span class="sxs-lookup"><span data-stu-id="f67e8-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="f67e8-106">**Você precisa atualizar os pacotes instalados para as versões RTM:**</span><span class="sxs-lookup"><span data-stu-id="f67e8-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="f67e8-107">Pacotes de tempo de execução (por exemplo, `Microsoft.EntityFrameworkCore.SqlServer`) alterados de `1.0.0-rc2-final` para `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="f67e8-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="f67e8-108">O pacote `Microsoft.EntityFrameworkCore.Tools` mudou de `1.0.0-preview1-final` para `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="f67e8-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="f67e8-109">Observe que as ferramentas ainda são pré-lançamento.</span><span class="sxs-lookup"><span data-stu-id="f67e8-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="f67e8-110">As migrações existentes podem precisar de maxLength adicionado</span><span class="sxs-lookup"><span data-stu-id="f67e8-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="f67e8-111">No RC2, a definição de coluna em uma migração ficou como `table.Column<string>(nullable: true)` e o comprimento da coluna foi pesquisado em alguns metadados que armazenamos no código por trás da migração.</span><span class="sxs-lookup"><span data-stu-id="f67e8-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="f67e8-112">No RTM, o comprimento agora está incluído no código com Scaffold `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="f67e8-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="f67e8-113">Todas as migrações existentes que foram com scaffolddas antes de usar a versão RTM não terão o argumento `maxLength` especificado.</span><span class="sxs-lookup"><span data-stu-id="f67e8-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="f67e8-114">Isso significa que o comprimento máximo com suporte do banco de dados será usado (`nvarchar(max)` em SQL Server).</span><span class="sxs-lookup"><span data-stu-id="f67e8-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="f67e8-115">Isso pode ser adequado para algumas colunas, mas as colunas que fazem parte de uma chave, chave estrangeira ou índice precisam ser atualizadas para incluir um comprimento máximo.</span><span class="sxs-lookup"><span data-stu-id="f67e8-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="f67e8-116">Por convenção, 450 é o comprimento máximo usado para chaves, chaves estrangeiras e colunas indexadas.</span><span class="sxs-lookup"><span data-stu-id="f67e8-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="f67e8-117">Se você tiver configurado explicitamente um comprimento no modelo, deverá usar esse comprimento em vez disso.</span><span class="sxs-lookup"><span data-stu-id="f67e8-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="f67e8-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="f67e8-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="f67e8-119">Essa alteração afeta os projetos que usam ASP.NET Identity e foram criados a partir de um modelo de projeto anterior ao RTM.</span><span class="sxs-lookup"><span data-stu-id="f67e8-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="f67e8-120">O modelo de projeto inclui uma migração usada para criar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f67e8-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="f67e8-121">Essa migração deve ser editada para especificar um comprimento máximo de `256` para as colunas a seguir.</span><span class="sxs-lookup"><span data-stu-id="f67e8-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="f67e8-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="f67e8-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="f67e8-123">Nome</span><span class="sxs-lookup"><span data-stu-id="f67e8-123">Name</span></span>

    * <span data-ttu-id="f67e8-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="f67e8-124">NormalizedName</span></span>

*  <span data-ttu-id="f67e8-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="f67e8-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="f67e8-126">Email</span><span class="sxs-lookup"><span data-stu-id="f67e8-126">Email</span></span>

   * <span data-ttu-id="f67e8-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="f67e8-127">NormalizedEmail</span></span>

   * <span data-ttu-id="f67e8-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="f67e8-128">NormalizedUserName</span></span>

   * <span data-ttu-id="f67e8-129">UserName</span><span class="sxs-lookup"><span data-stu-id="f67e8-129">UserName</span></span>

<span data-ttu-id="f67e8-130">A falha ao fazer essa alteração resultará na seguinte exceção quando a migração inicial for aplicada a um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f67e8-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="f67e8-131">.NET Core: Remover "Imports" em Project. JSON</span><span class="sxs-lookup"><span data-stu-id="f67e8-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="f67e8-132">Se você estiver visando o .NET Core com o RC2, precisaria adicionar `imports` ao Project. JSON como uma solução alternativa temporária para algumas das dependências de EF Core que não dão suporte a .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f67e8-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="f67e8-133">Agora eles podem ser removidos.</span><span class="sxs-lookup"><span data-stu-id="f67e8-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="f67e8-134">A partir da versão 1,0 RTM, o [SDK do .NET Core](https://www.microsoft.com/net/download/core) não oferece mais suporte a `project.json` ou a desenvolver aplicativos .NET Core usando o Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="f67e8-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="f67e8-135">Recomendamos que você [migre do project.json para csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="f67e8-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="f67e8-136">Se você estiver usando o Visual Studio, recomendamos que atualize para o [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f67e8-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="f67e8-137">UWP: Adicionar redirecionamentos de associação</span><span class="sxs-lookup"><span data-stu-id="f67e8-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="f67e8-138">A tentativa de executar comandos do EF em projetos Plataforma Universal do Windows (UWP) resulta no seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="f67e8-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="f67e8-139">Você precisa adicionar manualmente os redirecionamentos de associação ao projeto UWP.</span><span class="sxs-lookup"><span data-stu-id="f67e8-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="f67e8-140">Crie um arquivo chamado `App.config` na pasta raiz do projeto e adicione redirecionamentos às versões corretas do assembly.</span><span class="sxs-lookup"><span data-stu-id="f67e8-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

```xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
