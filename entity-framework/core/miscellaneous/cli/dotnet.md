---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754490"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="c85c6-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="c85c6-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="c85c6-103">As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="c85c6-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="c85c6-104">Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="c85c6-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="c85c6-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="c85c6-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="c85c6-106">O núcleo do .NET SDK versão 2.1.300 e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="c85c6-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="c85c6-107">Portanto, se você estiver usando versões recentes do SDK de núcleo do .NET e o tempo de execução do núcleo do EF, nenhuma instalação é necessária e você pode ignorar o restante desta seção.</span><span class="sxs-lookup"><span data-stu-id="c85c6-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="c85c6-108">Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="c85c6-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="c85c6-109">Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF principal em um computador que tem o SDK do .NET Core 2.1.300 ou posterior esteja instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configure o aplicativo para usar a versão antiga, modificando seu  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo.</span><span class="sxs-lookup"><span data-stu-id="c85c6-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="c85c6-110">Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto).</span><span class="sxs-lookup"><span data-stu-id="c85c6-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="c85c6-111">Em seguida, você poderá continuar com a instrução 2161DS abaixo.</span><span class="sxs-lookup"><span data-stu-id="c85c6-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="c85c6-112">Para versões anteriores do SDK do núcleo do .NET, você pode instalar as ferramentas de linha de comando do EF principais .NET usando estas etapas:</span><span class="sxs-lookup"><span data-stu-id="c85c6-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="c85c6-113">Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="c85c6-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="c85c6-114">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="c85c6-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="c85c6-115">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="c85c6-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="c85c6-116">Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="c85c6-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="c85c6-117">Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="c85c6-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="c85c6-118">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="c85c6-118">Using the tools</span></span>
---------------
<span data-ttu-id="c85c6-119">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="c85c6-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="c85c6-120">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="c85c6-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="c85c6-121">O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="c85c6-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="c85c6-122">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="c85c6-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="c85c6-123">Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="c85c6-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="c85c6-124">Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha o EF Core instalado em um projeto diferente teria esta aparência: `dotnet ef database update --project {project-path}` (do diretório de aplicativo da web)</span><span class="sxs-lookup"><span data-stu-id="c85c6-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="c85c6-125">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="c85c6-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="c85c6-126">--json</span><span class="sxs-lookup"><span data-stu-id="c85c6-126">--json</span></span>                           | <span data-ttu-id="c85c6-127">Mostra saída JSON.</span><span class="sxs-lookup"><span data-stu-id="c85c6-127">Show JSON output.</span></span>           |
| <span data-ttu-id="c85c6-128">-c</span><span class="sxs-lookup"><span data-stu-id="c85c6-128">-c</span></span> | <span data-ttu-id="c85c6-129">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="c85c6-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="c85c6-130">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="c85c6-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="c85c6-131">-p</span><span class="sxs-lookup"><span data-stu-id="c85c6-131">-p</span></span> | <span data-ttu-id="c85c6-132">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="c85c6-132">--project \<PROJECT></span></span>             | <span data-ttu-id="c85c6-133">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="c85c6-133">The project to use.</span></span>         |
| <span data-ttu-id="c85c6-134">-s</span><span class="sxs-lookup"><span data-stu-id="c85c6-134">-s</span></span> | <span data-ttu-id="c85c6-135">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="c85c6-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="c85c6-136">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="c85c6-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="c85c6-137">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="c85c6-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="c85c6-138">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="c85c6-138">The target framework.</span></span>       |
|    | <span data-ttu-id="c85c6-139">– configuração \<Configuração ></span><span class="sxs-lookup"><span data-stu-id="c85c6-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="c85c6-140">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="c85c6-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="c85c6-141">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="c85c6-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="c85c6-142">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="c85c6-142">The runtime to use.</span></span>         |
| <span data-ttu-id="c85c6-143">-h</span><span class="sxs-lookup"><span data-stu-id="c85c6-143">-h</span></span> | <span data-ttu-id="c85c6-144">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="c85c6-144">--help</span></span>                           | <span data-ttu-id="c85c6-145">Mostra informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="c85c6-145">Show help information.</span></span>      |
| <span data-ttu-id="c85c6-146">-v</span><span class="sxs-lookup"><span data-stu-id="c85c6-146">-v</span></span> | <span data-ttu-id="c85c6-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="c85c6-147">--verbose</span></span>                        | <span data-ttu-id="c85c6-148">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="c85c6-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="c85c6-149">– sem cor</span><span class="sxs-lookup"><span data-stu-id="c85c6-149">--no-color</span></span>                       | <span data-ttu-id="c85c6-150">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="c85c6-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="c85c6-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="c85c6-151">--prefix-output</span></span>                  | <span data-ttu-id="c85c6-152">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="c85c6-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="c85c6-153">Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.</span><span class="sxs-lookup"><span data-stu-id="c85c6-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="c85c6-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="c85c6-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="c85c6-155">remoção de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-155">dotnet ef database drop</span></span>

<span data-ttu-id="c85c6-156">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c85c6-156">Drops the database.</span></span>

<span data-ttu-id="c85c6-157">Opções:</span><span class="sxs-lookup"><span data-stu-id="c85c6-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="c85c6-158">-f</span><span class="sxs-lookup"><span data-stu-id="c85c6-158">-f</span></span> | <span data-ttu-id="c85c6-159">-force</span><span class="sxs-lookup"><span data-stu-id="c85c6-159">--force</span></span>   | <span data-ttu-id="c85c6-160">Não confirme.</span><span class="sxs-lookup"><span data-stu-id="c85c6-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="c85c6-161">-Execute</span><span class="sxs-lookup"><span data-stu-id="c85c6-161">--dry-run</span></span> | <span data-ttu-id="c85c6-162">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="c85c6-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="c85c6-163">atualização de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-163">dotnet ef database update</span></span>

<span data-ttu-id="c85c6-164">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="c85c6-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="c85c6-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="c85c6-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="c85c6-166">\<MIGRAÇÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-166">\<MIGRATION></span></span> | <span data-ttu-id="c85c6-167">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="c85c6-167">The target migration.</span></span> <span data-ttu-id="c85c6-168">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="c85c6-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="c85c6-169">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="c85c6-170">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="c85c6-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="c85c6-171">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="c85c6-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="c85c6-172">lista de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="c85c6-173">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="c85c6-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="c85c6-174">scaffold de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="c85c6-175">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c85c6-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="c85c6-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="c85c6-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="c85c6-177">\<CONEXÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-177">\<CONNECTION></span></span> | <span data-ttu-id="c85c6-178">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c85c6-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="c85c6-179">\<PROVEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-179">\<PROVIDER></span></span>   | <span data-ttu-id="c85c6-180">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="c85c6-180">The provider to use.</span></span> <span data-ttu-id="c85c6-181">(por exemplo, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="c85c6-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="c85c6-182">Opções:</span><span class="sxs-lookup"><span data-stu-id="c85c6-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c85c6-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="c85c6-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="c85c6-184">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="c85c6-184">--data-annotations</span></span>                      | <span data-ttu-id="c85c6-185">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="c85c6-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="c85c6-186">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="c85c6-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="c85c6-187">-c</span><span class="sxs-lookup"><span data-stu-id="c85c6-187">-c</span></span>              | <span data-ttu-id="c85c6-188">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="c85c6-188">--context \<NAME></span></span>                       | <span data-ttu-id="c85c6-189">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="c85c6-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="c85c6-190">-dir contexto \<caminho ></span><span class="sxs-lookup"><span data-stu-id="c85c6-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="c85c6-191">O diretório para colocar o arquivo DbContext no.</span><span class="sxs-lookup"><span data-stu-id="c85c6-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="c85c6-192">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c85c6-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="c85c6-193">-f</span><span class="sxs-lookup"><span data-stu-id="c85c6-193">-f</span></span>              | <span data-ttu-id="c85c6-194">-force</span><span class="sxs-lookup"><span data-stu-id="c85c6-194">--force</span></span>                                 | <span data-ttu-id="c85c6-195">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="c85c6-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="c85c6-196">-o</span><span class="sxs-lookup"><span data-stu-id="c85c6-196">-o</span></span>              | <span data-ttu-id="c85c6-197">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="c85c6-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="c85c6-198">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="c85c6-198">The directory to put files in.</span></span> <span data-ttu-id="c85c6-199">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c85c6-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="c85c6-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="c85c6-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="c85c6-201">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="c85c6-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="c85c6-202">-t</span><span class="sxs-lookup"><span data-stu-id="c85c6-202">-t</span></span>              | <span data-ttu-id="c85c6-203">-tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="c85c6-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="c85c6-204">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="c85c6-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="c85c6-205">– use nomes de banco de dados</span><span class="sxs-lookup"><span data-stu-id="c85c6-205">--use-database-names</span></span>                    | <span data-ttu-id="c85c6-206">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c85c6-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="c85c6-207">Adicionar migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-207">dotnet ef migrations add</span></span>

<span data-ttu-id="c85c6-208">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-208">Adds a new migration.</span></span>

<span data-ttu-id="c85c6-209">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="c85c6-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="c85c6-210">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-210">\<NAME></span></span> | <span data-ttu-id="c85c6-211">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-211">The name of the migration.</span></span> |

<span data-ttu-id="c85c6-212">Opções:</span><span class="sxs-lookup"><span data-stu-id="c85c6-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c85c6-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="c85c6-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="c85c6-214"><nobr>-dir saída \<caminho ></nobr></span><span class="sxs-lookup"><span data-stu-id="c85c6-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="c85c6-215">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="c85c6-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="c85c6-216">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c85c6-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="c85c6-217">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="c85c6-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="c85c6-218">lista de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-218">dotnet ef migrations list</span></span>

<span data-ttu-id="c85c6-219">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="c85c6-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="c85c6-220">Remova as migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="c85c6-221">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-221">Removes the last migration.</span></span>

<span data-ttu-id="c85c6-222">Opções:</span><span class="sxs-lookup"><span data-stu-id="c85c6-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="c85c6-223">-f</span><span class="sxs-lookup"><span data-stu-id="c85c6-223">-f</span></span> | <span data-ttu-id="c85c6-224">-force</span><span class="sxs-lookup"><span data-stu-id="c85c6-224">--force</span></span> | <span data-ttu-id="c85c6-225">Reverta a migração se ela foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c85c6-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="c85c6-226">script de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="c85c6-226">dotnet ef migrations script</span></span>

<span data-ttu-id="c85c6-227">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="c85c6-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="c85c6-228">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="c85c6-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="c85c6-229">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-229">\<FROM></span></span> | <span data-ttu-id="c85c6-230">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="c85c6-230">The starting migration.</span></span> <span data-ttu-id="c85c6-231">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="c85c6-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="c85c6-232">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="c85c6-232">\<TO></span></span>   | <span data-ttu-id="c85c6-233">A migração final.</span><span class="sxs-lookup"><span data-stu-id="c85c6-233">The ending migration.</span></span> <span data-ttu-id="c85c6-234">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="c85c6-235">Opções:</span><span class="sxs-lookup"><span data-stu-id="c85c6-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="c85c6-236">-o</span><span class="sxs-lookup"><span data-stu-id="c85c6-236">-o</span></span> | <span data-ttu-id="c85c6-237">-saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="c85c6-237">--output \<FILE></span></span> | <span data-ttu-id="c85c6-238">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="c85c6-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="c85c6-239">-i</span><span class="sxs-lookup"><span data-stu-id="c85c6-239">-i</span></span> | <span data-ttu-id="c85c6-240">– idempotente</span><span class="sxs-lookup"><span data-stu-id="c85c6-240">--idempotent</span></span>     | <span data-ttu-id="c85c6-241">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="c85c6-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
