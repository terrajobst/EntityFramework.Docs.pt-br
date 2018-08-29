---
title: .NET core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152408"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="cb6ce-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="cb6ce-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="cb6ce-103">As ferramentas de linha de comando do Entity Framework Core .NET são uma extensão para a plataforma cruzada **dotnet** comando, que é parte do [SDK do .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="cb6ce-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="cb6ce-104">Se você estiver usando o Visual Studio, recomendamos [as ferramentas de PMC] [ 1] em vez disso, já que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="cb6ce-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="cb6ce-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="cb6ce-106">SDK do .NET Core 2.1.300 de versão e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="cb6ce-107">Portanto, se você estiver usando versões recentes do SDK do .NET Core e o tempo de execução do EF Core, nenhuma instalação é necessária e você pode ignorar o restante desta seção.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="cb6ce-108">Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão do EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="cb6ce-109">Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF Core em um computador que tem o SDK do .NET Core 2.1.300 ou mais recente instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configurar o aplicativo para usar essa versão, modificando sua  [global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="cb6ce-110">Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto).</span><span class="sxs-lookup"><span data-stu-id="cb6ce-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="cb6ce-111">Em seguida, você pode prosseguir com a instrução 2161DS abaixo.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="cb6ce-112">Para versões anteriores do SDK do .NET Core, você pode instalar as ferramentas de linha de comando do EF Core .NET usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="cb6ce-113">Edite o arquivo de projeto e adicione entityframeworkcore como um item DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="cb6ce-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="cb6ce-114">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="cb6ce-115">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="cb6ce-116">Uma referência de pacote com `PrivateAssets="All"` significa que ele não é exposto aos projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados apenas durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="cb6ce-117">Se você fez tudo certo, você deve ser capaz de executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="cb6ce-118">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="cb6ce-118">Using the tools</span></span>
---------------
<span data-ttu-id="cb6ce-119">Sempre que você invoca um comando, existem dois projetos envolvidos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="cb6ce-120">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="cb6ce-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="cb6ce-121">O projeto de destino padrão é o projeto no diretório atual, mas pode ser alterado usando o <nobr> **`--project`** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="cb6ce-122">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="cb6ce-123">Ele também assume como padrão o projeto no diretório atual, mas pode ser alterado usando o **`--startup-project`** opção.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="cb6ce-124">Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha instalado em um projeto diferente do EF Core teria esta aparência: `dotnet ef database update --project {project-path}` (a partir do seu diretório de aplicativo web)</span><span class="sxs-lookup"><span data-stu-id="cb6ce-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="cb6ce-125">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="cb6ce-126">Mostra a saída JSON.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-126">Show JSON output.</span></span>           |
| <span data-ttu-id="cb6ce-127">-c</span><span class="sxs-lookup"><span data-stu-id="cb6ce-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="cb6ce-128">O DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="cb6ce-129">-p</span><span class="sxs-lookup"><span data-stu-id="cb6ce-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="cb6ce-130">O projeto para usar.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-130">The project to use.</span></span>         |
| <span data-ttu-id="cb6ce-131">-s</span><span class="sxs-lookup"><span data-stu-id="cb6ce-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="cb6ce-132">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="cb6ce-133">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="cb6ce-134">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="cb6ce-135">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-135">The runtime to use.</span></span>         |
| <span data-ttu-id="cb6ce-136">-h</span><span class="sxs-lookup"><span data-stu-id="cb6ce-136">-h</span></span> | `--help`                           | <span data-ttu-id="cb6ce-137">Mostre informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-137">Show help information.</span></span>      |
| <span data-ttu-id="cb6ce-138">-v</span><span class="sxs-lookup"><span data-stu-id="cb6ce-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="cb6ce-139">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="cb6ce-140">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="cb6ce-141">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="cb6ce-142">Para especificar o ambiente do ASP.NET Core, defina as **ASPNETCORE_ENVIRONMENT** variável de ambiente antes da execução.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="cb6ce-143">Comandos</span><span class="sxs-lookup"><span data-stu-id="cb6ce-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="cb6ce-144">lista de banco de dados do dotnet ef</span><span class="sxs-lookup"><span data-stu-id="cb6ce-144">dotnet ef database drop</span></span>

<span data-ttu-id="cb6ce-145">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-145">Drops the database.</span></span>

<span data-ttu-id="cb6ce-146">Opções:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="cb6ce-147">-f</span><span class="sxs-lookup"><span data-stu-id="cb6ce-147">-f</span></span> | `--force`   | <span data-ttu-id="cb6ce-148">Confirme se não.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="cb6ce-149">Mostrar qual banco de dados deve ser descartado, mas não o remova.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="cb6ce-150">atualização de banco de dados do dotnet ef</span><span class="sxs-lookup"><span data-stu-id="cb6ce-150">dotnet ef database update</span></span>

<span data-ttu-id="cb6ce-151">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="cb6ce-152">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="cb6ce-153">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-153">The target migration.</span></span> <span data-ttu-id="cb6ce-154">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="cb6ce-155">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="cb6ce-156">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="cb6ce-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="cb6ce-157">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="cb6ce-158">lista do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="cb6ce-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="cb6ce-159">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="cb6ce-160">scaffold do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="cb6ce-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="cb6ce-161">Usa o Scaffold de um DbContext e tipos de entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="cb6ce-162">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="cb6ce-163">A cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="cb6ce-164">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-164">The provider to use.</span></span> <span data-ttu-id="cb6ce-165">(por exemplo, entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="cb6ce-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="cb6ce-166">Opções:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb6ce-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="cb6ce-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="cb6ce-168">Use atributos para configurar o modelo (quando possível).</span><span class="sxs-lookup"><span data-stu-id="cb6ce-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="cb6ce-169">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="cb6ce-170">-c</span><span class="sxs-lookup"><span data-stu-id="cb6ce-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="cb6ce-171">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="cb6ce-172">O diretório para colocar o arquivo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="cb6ce-173">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="cb6ce-174">-f</span><span class="sxs-lookup"><span data-stu-id="cb6ce-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="cb6ce-175">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="cb6ce-176">-o</span><span class="sxs-lookup"><span data-stu-id="cb6ce-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="cb6ce-177">O diretório para colocar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-177">The directory to put files in.</span></span> <span data-ttu-id="cb6ce-178">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="cb6ce-179">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="cb6ce-180">-t</span><span class="sxs-lookup"><span data-stu-id="cb6ce-180">-t</span></span>              | <span data-ttu-id="cb6ce-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="cb6ce-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="cb6ce-182">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="cb6ce-183">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="cb6ce-184">Adicionar migrações do ef dotnet</span><span class="sxs-lookup"><span data-stu-id="cb6ce-184">dotnet ef migrations add</span></span>

<span data-ttu-id="cb6ce-185">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-185">Adds a new migration.</span></span>

<span data-ttu-id="cb6ce-186">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="cb6ce-187">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-187">The name of the migration.</span></span> |

<span data-ttu-id="cb6ce-188">Opções:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cb6ce-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="cb6ce-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="cb6ce-190"><nobr>`--output-dir <PATH>`</nobr></span><span class="sxs-lookup"><span data-stu-id="cb6ce-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="cb6ce-191">O diretório (e sub-namespace) para usar.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="cb6ce-192">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="cb6ce-193">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="cb6ce-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="cb6ce-194">lista de migrações dotnet ef</span><span class="sxs-lookup"><span data-stu-id="cb6ce-194">dotnet ef migrations list</span></span>

<span data-ttu-id="cb6ce-195">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="cb6ce-196">Remover de migrações do ef dotnet</span><span class="sxs-lookup"><span data-stu-id="cb6ce-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="cb6ce-197">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-197">Removes the last migration.</span></span>

<span data-ttu-id="cb6ce-198">Opções:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="cb6ce-199">-f</span><span class="sxs-lookup"><span data-stu-id="cb6ce-199">-f</span></span> | `--force` | <span data-ttu-id="cb6ce-200">Reverta a migração, se ele tiver sido aplicado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="cb6ce-201">script de migrações dotnet ef</span><span class="sxs-lookup"><span data-stu-id="cb6ce-201">dotnet ef migrations script</span></span>

<span data-ttu-id="cb6ce-202">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="cb6ce-203">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="cb6ce-204">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-204">The starting migration.</span></span> <span data-ttu-id="cb6ce-205">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="cb6ce-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="cb6ce-206">A migração final.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-206">The ending migration.</span></span> <span data-ttu-id="cb6ce-207">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="cb6ce-208">Opções:</span><span class="sxs-lookup"><span data-stu-id="cb6ce-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="cb6ce-209">-o</span><span class="sxs-lookup"><span data-stu-id="cb6ce-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="cb6ce-210">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="cb6ce-211">-i</span><span class="sxs-lookup"><span data-stu-id="cb6ce-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="cb6ce-212">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="cb6ce-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
