---
title: .NET core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995291"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="47fcb-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="47fcb-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="47fcb-103">As ferramentas de linha de comando do Entity Framework Core .NET são uma extensão para a plataforma cruzada **dotnet** comando, que é parte do [SDK do .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="47fcb-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="47fcb-104">Se você estiver usando o Visual Studio, recomendamos [as ferramentas de PMC] [ 1] em vez disso, já que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="47fcb-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="47fcb-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="47fcb-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="47fcb-106">SDK do .NET Core 2.1.300 de versão e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="47fcb-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="47fcb-107">Portanto, se você estiver usando versões recentes do SDK do .NET Core e o tempo de execução do EF Core, nenhuma instalação é necessária e você pode ignorar o restante desta seção.</span><span class="sxs-lookup"><span data-stu-id="47fcb-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="47fcb-108">Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão do EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="47fcb-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="47fcb-109">Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF Core em um computador que tem o SDK do .NET Core 2.1.300 ou mais recente instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configurar o aplicativo para usar essa versão, modificando sua  [global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo.</span><span class="sxs-lookup"><span data-stu-id="47fcb-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="47fcb-110">Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto).</span><span class="sxs-lookup"><span data-stu-id="47fcb-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="47fcb-111">Em seguida, você pode prosseguir com a instrução 2161DS abaixo.</span><span class="sxs-lookup"><span data-stu-id="47fcb-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="47fcb-112">Para versões anteriores do SDK do .NET Core, você pode instalar as ferramentas de linha de comando do EF Core .NET usando as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="47fcb-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="47fcb-113">Edite o arquivo de projeto e adicione entityframeworkcore como um item DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="47fcb-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="47fcb-114">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="47fcb-115">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="47fcb-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="47fcb-116">Uma referência de pacote com `PrivateAssets="All"` significa que ele não é exposto aos projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados apenas durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="47fcb-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="47fcb-117">Se você fez tudo certo, você deve ser capaz de executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="47fcb-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="47fcb-118">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="47fcb-118">Using the tools</span></span>
---------------
<span data-ttu-id="47fcb-119">Sempre que você invoca um comando, existem dois projetos envolvidos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="47fcb-120">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="47fcb-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="47fcb-121">O projeto de destino padrão é o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="47fcb-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="47fcb-122">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="47fcb-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="47fcb-123">Ele também assume como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="47fcb-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="47fcb-124">Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha instalado em um projeto diferente do EF Core teria esta aparência: `dotnet ef database update --project {project-path}` (a partir do seu diretório de aplicativo web)</span><span class="sxs-lookup"><span data-stu-id="47fcb-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="47fcb-125">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="47fcb-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="47fcb-126">--json</span><span class="sxs-lookup"><span data-stu-id="47fcb-126">--json</span></span>                           | <span data-ttu-id="47fcb-127">Mostra a saída JSON.</span><span class="sxs-lookup"><span data-stu-id="47fcb-127">Show JSON output.</span></span>           |
| <span data-ttu-id="47fcb-128">-c</span><span class="sxs-lookup"><span data-stu-id="47fcb-128">-c</span></span> | <span data-ttu-id="47fcb-129">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="47fcb-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="47fcb-130">O DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="47fcb-131">-p</span><span class="sxs-lookup"><span data-stu-id="47fcb-131">-p</span></span> | <span data-ttu-id="47fcb-132">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="47fcb-132">--project \<PROJECT></span></span>             | <span data-ttu-id="47fcb-133">O projeto para usar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-133">The project to use.</span></span>         |
| <span data-ttu-id="47fcb-134">-s</span><span class="sxs-lookup"><span data-stu-id="47fcb-134">-s</span></span> | <span data-ttu-id="47fcb-135">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="47fcb-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="47fcb-136">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="47fcb-137">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="47fcb-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="47fcb-138">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="47fcb-138">The target framework.</span></span>       |
|    | <span data-ttu-id="47fcb-139">– configuração \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="47fcb-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="47fcb-140">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="47fcb-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="47fcb-141">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="47fcb-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="47fcb-142">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-142">The runtime to use.</span></span>         |
| <span data-ttu-id="47fcb-143">-h</span><span class="sxs-lookup"><span data-stu-id="47fcb-143">-h</span></span> | <span data-ttu-id="47fcb-144">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="47fcb-144">--help</span></span>                           | <span data-ttu-id="47fcb-145">Mostre informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="47fcb-145">Show help information.</span></span>      |
| <span data-ttu-id="47fcb-146">-v</span><span class="sxs-lookup"><span data-stu-id="47fcb-146">-v</span></span> | <span data-ttu-id="47fcb-147">– detalhado</span><span class="sxs-lookup"><span data-stu-id="47fcb-147">--verbose</span></span>                        | <span data-ttu-id="47fcb-148">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="47fcb-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="47fcb-149">– sem cor</span><span class="sxs-lookup"><span data-stu-id="47fcb-149">--no-color</span></span>                       | <span data-ttu-id="47fcb-150">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="47fcb-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="47fcb-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="47fcb-151">--prefix-output</span></span>                  | <span data-ttu-id="47fcb-152">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="47fcb-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="47fcb-153">Para especificar o ambiente do ASP.NET Core, defina as **ASPNETCORE_ENVIRONMENT** variável de ambiente antes da execução.</span><span class="sxs-lookup"><span data-stu-id="47fcb-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="47fcb-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="47fcb-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="47fcb-155">lista de banco de dados do dotnet ef</span><span class="sxs-lookup"><span data-stu-id="47fcb-155">dotnet ef database drop</span></span>

<span data-ttu-id="47fcb-156">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47fcb-156">Drops the database.</span></span>

<span data-ttu-id="47fcb-157">Opções:</span><span class="sxs-lookup"><span data-stu-id="47fcb-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="47fcb-158">-f</span><span class="sxs-lookup"><span data-stu-id="47fcb-158">-f</span></span> | <span data-ttu-id="47fcb-159">-force</span><span class="sxs-lookup"><span data-stu-id="47fcb-159">--force</span></span>   | <span data-ttu-id="47fcb-160">Confirme se não.</span><span class="sxs-lookup"><span data-stu-id="47fcb-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="47fcb-161">-dry-run</span><span class="sxs-lookup"><span data-stu-id="47fcb-161">--dry-run</span></span> | <span data-ttu-id="47fcb-162">Mostrar qual banco de dados deve ser descartado, mas não o remova.</span><span class="sxs-lookup"><span data-stu-id="47fcb-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="47fcb-163">atualização de banco de dados do dotnet ef</span><span class="sxs-lookup"><span data-stu-id="47fcb-163">dotnet ef database update</span></span>

<span data-ttu-id="47fcb-164">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="47fcb-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="47fcb-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="47fcb-166">\<MIGRAÇÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-166">\<MIGRATION></span></span> | <span data-ttu-id="47fcb-167">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="47fcb-167">The target migration.</span></span> <span data-ttu-id="47fcb-168">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="47fcb-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="47fcb-169">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="47fcb-170">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="47fcb-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="47fcb-171">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="47fcb-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="47fcb-172">lista do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="47fcb-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="47fcb-173">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="47fcb-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="47fcb-174">scaffold do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="47fcb-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="47fcb-175">Usa o Scaffold de um DbContext e tipos de entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47fcb-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="47fcb-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="47fcb-177">\<CONEXÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-177">\<CONNECTION></span></span> | <span data-ttu-id="47fcb-178">A cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47fcb-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="47fcb-179">\<PROVEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-179">\<PROVIDER></span></span>   | <span data-ttu-id="47fcb-180">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="47fcb-180">The provider to use.</span></span> <span data-ttu-id="47fcb-181">(por exemplo, entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="47fcb-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="47fcb-182">Opções:</span><span class="sxs-lookup"><span data-stu-id="47fcb-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47fcb-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="47fcb-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="47fcb-184">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="47fcb-184">--data-annotations</span></span>                      | <span data-ttu-id="47fcb-185">Use atributos para configurar o modelo (quando possível).</span><span class="sxs-lookup"><span data-stu-id="47fcb-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="47fcb-186">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="47fcb-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="47fcb-187">-c</span><span class="sxs-lookup"><span data-stu-id="47fcb-187">-c</span></span>              | <span data-ttu-id="47fcb-188">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="47fcb-188">--context \<NAME></span></span>                       | <span data-ttu-id="47fcb-189">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="47fcb-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="47fcb-190">– contexto-dir \<caminho ></span><span class="sxs-lookup"><span data-stu-id="47fcb-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="47fcb-191">O diretório para colocar o arquivo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="47fcb-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="47fcb-192">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="47fcb-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="47fcb-193">-f</span><span class="sxs-lookup"><span data-stu-id="47fcb-193">-f</span></span>              | <span data-ttu-id="47fcb-194">-force</span><span class="sxs-lookup"><span data-stu-id="47fcb-194">--force</span></span>                                 | <span data-ttu-id="47fcb-195">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="47fcb-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="47fcb-196">-o</span><span class="sxs-lookup"><span data-stu-id="47fcb-196">-o</span></span>              | <span data-ttu-id="47fcb-197">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="47fcb-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="47fcb-198">O diretório para colocar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="47fcb-198">The directory to put files in.</span></span> <span data-ttu-id="47fcb-199">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="47fcb-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="47fcb-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="47fcb-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="47fcb-201">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="47fcb-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="47fcb-202">-t</span><span class="sxs-lookup"><span data-stu-id="47fcb-202">-t</span></span>              | <span data-ttu-id="47fcb-203">– tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="47fcb-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="47fcb-204">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="47fcb-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="47fcb-205">– use nomes de banco de dados</span><span class="sxs-lookup"><span data-stu-id="47fcb-205">--use-database-names</span></span>                    | <span data-ttu-id="47fcb-206">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47fcb-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="47fcb-207">Adicionar migrações do ef dotnet</span><span class="sxs-lookup"><span data-stu-id="47fcb-207">dotnet ef migrations add</span></span>

<span data-ttu-id="47fcb-208">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-208">Adds a new migration.</span></span>

<span data-ttu-id="47fcb-209">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="47fcb-210">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-210">\<NAME></span></span> | <span data-ttu-id="47fcb-211">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-211">The name of the migration.</span></span> |

<span data-ttu-id="47fcb-212">Opções:</span><span class="sxs-lookup"><span data-stu-id="47fcb-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="47fcb-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="47fcb-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="47fcb-214"><nobr>-dir saída \<caminho ></nobr></span><span class="sxs-lookup"><span data-stu-id="47fcb-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="47fcb-215">O diretório (e sub-namespace) para usar.</span><span class="sxs-lookup"><span data-stu-id="47fcb-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="47fcb-216">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="47fcb-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="47fcb-217">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="47fcb-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="47fcb-218">lista de migrações dotnet ef</span><span class="sxs-lookup"><span data-stu-id="47fcb-218">dotnet ef migrations list</span></span>

<span data-ttu-id="47fcb-219">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="47fcb-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="47fcb-220">Remover de migrações do ef dotnet</span><span class="sxs-lookup"><span data-stu-id="47fcb-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="47fcb-221">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-221">Removes the last migration.</span></span>

<span data-ttu-id="47fcb-222">Opções:</span><span class="sxs-lookup"><span data-stu-id="47fcb-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="47fcb-223">-f</span><span class="sxs-lookup"><span data-stu-id="47fcb-223">-f</span></span> | <span data-ttu-id="47fcb-224">-force</span><span class="sxs-lookup"><span data-stu-id="47fcb-224">--force</span></span> | <span data-ttu-id="47fcb-225">Reverta a migração, se ele tiver sido aplicado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="47fcb-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="47fcb-226">script de migrações dotnet ef</span><span class="sxs-lookup"><span data-stu-id="47fcb-226">dotnet ef migrations script</span></span>

<span data-ttu-id="47fcb-227">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="47fcb-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="47fcb-228">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="47fcb-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="47fcb-229">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-229">\<FROM></span></span> | <span data-ttu-id="47fcb-230">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="47fcb-230">The starting migration.</span></span> <span data-ttu-id="47fcb-231">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="47fcb-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="47fcb-232">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="47fcb-232">\<TO></span></span>   | <span data-ttu-id="47fcb-233">A migração final.</span><span class="sxs-lookup"><span data-stu-id="47fcb-233">The ending migration.</span></span> <span data-ttu-id="47fcb-234">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="47fcb-235">Opções:</span><span class="sxs-lookup"><span data-stu-id="47fcb-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="47fcb-236">-o</span><span class="sxs-lookup"><span data-stu-id="47fcb-236">-o</span></span> | <span data-ttu-id="47fcb-237">– saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="47fcb-237">--output \<FILE></span></span> | <span data-ttu-id="47fcb-238">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="47fcb-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="47fcb-239">-i</span><span class="sxs-lookup"><span data-stu-id="47fcb-239">-i</span></span> | <span data-ttu-id="47fcb-240">– idempotentes</span><span class="sxs-lookup"><span data-stu-id="47fcb-240">--idempotent</span></span>     | <span data-ttu-id="47fcb-241">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="47fcb-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
