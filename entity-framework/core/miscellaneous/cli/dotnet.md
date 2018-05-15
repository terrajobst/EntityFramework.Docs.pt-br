---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a6190-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="a6190-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a6190-103">As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="a6190-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a6190-104">Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="a6190-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a6190-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="a6190-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="a6190-106">O núcleo do .NET SDK versão 2.1.300 e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores.</span><span class="sxs-lookup"><span data-stu-id="a6190-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="a6190-107">Portanto, se você estiver usando versões recentes do SDK de núcleo do .NET e o tempo de execução do núcleo do EF, nenhuma instalação é necessária e você pode ignorar o restante desta seção.</span><span class="sxs-lookup"><span data-stu-id="a6190-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="a6190-108">Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="a6190-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="a6190-109">Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF principal em um computador que tem o SDK do .NET Core 2.1.300 ou posterior esteja instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configure o aplicativo para usar a versão antiga, modificando seu  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo.</span><span class="sxs-lookup"><span data-stu-id="a6190-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="a6190-110">Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto).</span><span class="sxs-lookup"><span data-stu-id="a6190-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="a6190-111">Em seguida, você poderá continuar com a instrução 2161DS abaixo.</span><span class="sxs-lookup"><span data-stu-id="a6190-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="a6190-112">Para versões anteriores do SDK do núcleo do .NET, você pode instalar as ferramentas de linha de comando do EF principais .NET usando estas etapas:</span><span class="sxs-lookup"><span data-stu-id="a6190-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a6190-113">Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="a6190-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a6190-114">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="a6190-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="a6190-115">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="a6190-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="a6190-116">Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="a6190-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a6190-117">Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="a6190-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a6190-118">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="a6190-118">Using the tools</span></span>
---------------
<span data-ttu-id="a6190-119">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="a6190-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a6190-120">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="a6190-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a6190-121">O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="a6190-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a6190-122">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="a6190-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a6190-123">Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="a6190-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="a6190-124">Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha o EF Core instalado em um projeto diferente teria esta aparência: `dotnet ef database update --project {project-path}` (do diretório de aplicativo da web)</span><span class="sxs-lookup"><span data-stu-id="a6190-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="a6190-125">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="a6190-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a6190-126">--json</span><span class="sxs-lookup"><span data-stu-id="a6190-126">--json</span></span>                           | <span data-ttu-id="a6190-127">Mostra saída JSON.</span><span class="sxs-lookup"><span data-stu-id="a6190-127">Show JSON output.</span></span>           |
| <span data-ttu-id="a6190-128">-c</span><span class="sxs-lookup"><span data-stu-id="a6190-128">-c</span></span> | <span data-ttu-id="a6190-129">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a6190-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a6190-130">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="a6190-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="a6190-131">-p</span><span class="sxs-lookup"><span data-stu-id="a6190-131">-p</span></span> | <span data-ttu-id="a6190-132">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="a6190-132">--project \<PROJECT></span></span>             | <span data-ttu-id="a6190-133">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="a6190-133">The project to use.</span></span>         |
| <span data-ttu-id="a6190-134">-s</span><span class="sxs-lookup"><span data-stu-id="a6190-134">-s</span></span> | <span data-ttu-id="a6190-135">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="a6190-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a6190-136">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="a6190-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="a6190-137">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="a6190-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a6190-138">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="a6190-138">The target framework.</span></span>       |
|    | <span data-ttu-id="a6190-139">– configuração \<Configuração ></span><span class="sxs-lookup"><span data-stu-id="a6190-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a6190-140">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="a6190-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a6190-141">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="a6190-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a6190-142">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="a6190-142">The runtime to use.</span></span>         |
| <span data-ttu-id="a6190-143">-h</span><span class="sxs-lookup"><span data-stu-id="a6190-143">-h</span></span> | <span data-ttu-id="a6190-144">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="a6190-144">--help</span></span>                           | <span data-ttu-id="a6190-145">Mostra informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="a6190-145">Show help information.</span></span>      |
| <span data-ttu-id="a6190-146">-v</span><span class="sxs-lookup"><span data-stu-id="a6190-146">-v</span></span> | <span data-ttu-id="a6190-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="a6190-147">--verbose</span></span>                        | <span data-ttu-id="a6190-148">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="a6190-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a6190-149">– sem cor</span><span class="sxs-lookup"><span data-stu-id="a6190-149">--no-color</span></span>                       | <span data-ttu-id="a6190-150">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="a6190-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a6190-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="a6190-151">--prefix-output</span></span>                  | <span data-ttu-id="a6190-152">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="a6190-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a6190-153">Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.</span><span class="sxs-lookup"><span data-stu-id="a6190-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a6190-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="a6190-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a6190-155">remoção de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-155">dotnet ef database drop</span></span>

<span data-ttu-id="a6190-156">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6190-156">Drops the database.</span></span>

<span data-ttu-id="a6190-157">Opções:</span><span class="sxs-lookup"><span data-stu-id="a6190-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a6190-158">-f</span><span class="sxs-lookup"><span data-stu-id="a6190-158">-f</span></span> | <span data-ttu-id="a6190-159">-force</span><span class="sxs-lookup"><span data-stu-id="a6190-159">--force</span></span>   | <span data-ttu-id="a6190-160">Não confirme.</span><span class="sxs-lookup"><span data-stu-id="a6190-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a6190-161">-Execute</span><span class="sxs-lookup"><span data-stu-id="a6190-161">--dry-run</span></span> | <span data-ttu-id="a6190-162">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="a6190-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a6190-163">atualização de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-163">dotnet ef database update</span></span>

<span data-ttu-id="a6190-164">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="a6190-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a6190-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a6190-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a6190-166">\<MIGRAÇÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-166">\<MIGRATION></span></span> | <span data-ttu-id="a6190-167">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="a6190-167">The target migration.</span></span> <span data-ttu-id="a6190-168">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="a6190-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a6190-169">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a6190-170">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="a6190-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a6190-171">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="a6190-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a6190-172">lista de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a6190-173">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="a6190-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a6190-174">scaffold de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a6190-175">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6190-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a6190-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a6190-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="a6190-177">\<CONEXÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-177">\<CONNECTION></span></span> | <span data-ttu-id="a6190-178">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6190-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="a6190-179">\<PROVEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-179">\<PROVIDER></span></span>   | <span data-ttu-id="a6190-180">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="a6190-180">The provider to use.</span></span> <span data-ttu-id="a6190-181">(por exemplo,</span><span class="sxs-lookup"><span data-stu-id="a6190-181">(E.g.</span></span> <span data-ttu-id="a6190-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a6190-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a6190-183">Opções:</span><span class="sxs-lookup"><span data-stu-id="a6190-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a6190-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a6190-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="a6190-185">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="a6190-185">--data-annotations</span></span>                      | <span data-ttu-id="a6190-186">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="a6190-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a6190-187">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="a6190-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a6190-188">-c</span><span class="sxs-lookup"><span data-stu-id="a6190-188">-c</span></span>              | <span data-ttu-id="a6190-189">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="a6190-189">--context \<NAME></span></span>                       | <span data-ttu-id="a6190-190">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="a6190-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="a6190-191">-dir contexto \<caminho ></span><span class="sxs-lookup"><span data-stu-id="a6190-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="a6190-192">O diretório para colocar o arquivo DbContext no.</span><span class="sxs-lookup"><span data-stu-id="a6190-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="a6190-193">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="a6190-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a6190-194">-f</span><span class="sxs-lookup"><span data-stu-id="a6190-194">-f</span></span>              | <span data-ttu-id="a6190-195">-force</span><span class="sxs-lookup"><span data-stu-id="a6190-195">--force</span></span>                                 | <span data-ttu-id="a6190-196">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="a6190-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a6190-197">-o</span><span class="sxs-lookup"><span data-stu-id="a6190-197">-o</span></span>              | <span data-ttu-id="a6190-198">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="a6190-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a6190-199">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="a6190-199">The directory to put files in.</span></span> <span data-ttu-id="a6190-200">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="a6190-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a6190-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="a6190-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a6190-202">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="a6190-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a6190-203">-t</span><span class="sxs-lookup"><span data-stu-id="a6190-203">-t</span></span>              | <span data-ttu-id="a6190-204">-tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="a6190-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a6190-205">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="a6190-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a6190-206">– use nomes de banco de dados</span><span class="sxs-lookup"><span data-stu-id="a6190-206">--use-database-names</span></span>                    | <span data-ttu-id="a6190-207">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6190-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a6190-208">Adicionar migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-208">dotnet ef migrations add</span></span>

<span data-ttu-id="a6190-209">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-209">Adds a new migration.</span></span>

<span data-ttu-id="a6190-210">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a6190-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a6190-211">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-211">\<NAME></span></span> | <span data-ttu-id="a6190-212">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-212">The name of the migration.</span></span> |

<span data-ttu-id="a6190-213">Opções:</span><span class="sxs-lookup"><span data-stu-id="a6190-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a6190-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a6190-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="a6190-215"><nobr>-dir saída \<caminho ></nobr></span><span class="sxs-lookup"><span data-stu-id="a6190-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a6190-216">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="a6190-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a6190-217">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="a6190-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="a6190-218">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="a6190-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a6190-219">lista de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-219">dotnet ef migrations list</span></span>

<span data-ttu-id="a6190-220">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="a6190-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a6190-221">Remova as migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="a6190-222">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-222">Removes the last migration.</span></span>

<span data-ttu-id="a6190-223">Opções:</span><span class="sxs-lookup"><span data-stu-id="a6190-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a6190-224">-f</span><span class="sxs-lookup"><span data-stu-id="a6190-224">-f</span></span> | <span data-ttu-id="a6190-225">-force</span><span class="sxs-lookup"><span data-stu-id="a6190-225">--force</span></span> | <span data-ttu-id="a6190-226">Reverta a migração se ela foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6190-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a6190-227">script de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="a6190-227">dotnet ef migrations script</span></span>

<span data-ttu-id="a6190-228">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="a6190-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a6190-229">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a6190-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a6190-230">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-230">\<FROM></span></span> | <span data-ttu-id="a6190-231">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="a6190-231">The starting migration.</span></span> <span data-ttu-id="a6190-232">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="a6190-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a6190-233">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="a6190-233">\<TO></span></span>   | <span data-ttu-id="a6190-234">A migração final.</span><span class="sxs-lookup"><span data-stu-id="a6190-234">The ending migration.</span></span> <span data-ttu-id="a6190-235">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a6190-236">Opções:</span><span class="sxs-lookup"><span data-stu-id="a6190-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a6190-237">-o</span><span class="sxs-lookup"><span data-stu-id="a6190-237">-o</span></span> | <span data-ttu-id="a6190-238">-saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="a6190-238">--output \<FILE></span></span> | <span data-ttu-id="a6190-239">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="a6190-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a6190-240">-i</span><span class="sxs-lookup"><span data-stu-id="a6190-240">-i</span></span> | <span data-ttu-id="a6190-241">– idempotente</span><span class="sxs-lookup"><span data-stu-id="a6190-241">--idempotent</span></span>     | <span data-ttu-id="a6190-242">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="a6190-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
