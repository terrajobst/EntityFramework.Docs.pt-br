---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="ff499-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="ff499-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="ff499-103">As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="ff499-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="ff499-104">Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="ff499-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="ff499-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="ff499-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="ff499-106">Instale as ferramentas de linha de comando do EF Core .NET usando estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ff499-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="ff499-107">Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="ff499-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="ff499-108">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ff499-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="ff499-109">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="ff499-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="ff499-110">Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="ff499-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="ff499-111">Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="ff499-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="ff499-112">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="ff499-112">Using the tools</span></span>
---------------
<span data-ttu-id="ff499-113">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="ff499-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="ff499-114">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="ff499-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="ff499-115">O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="ff499-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="ff499-116">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="ff499-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="ff499-117">Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="ff499-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="ff499-118">Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha o EF Core instalado em um projeto diferente teria esta aparência: `dotnet ef database update --project {project-path}` (do diretório de aplicativo da web)</span><span class="sxs-lookup"><span data-stu-id="ff499-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="ff499-119">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="ff499-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="ff499-120">--json</span><span class="sxs-lookup"><span data-stu-id="ff499-120">--json</span></span>                           | <span data-ttu-id="ff499-121">Mostra saída JSON.</span><span class="sxs-lookup"><span data-stu-id="ff499-121">Show JSON output.</span></span>           |
| <span data-ttu-id="ff499-122">-c</span><span class="sxs-lookup"><span data-stu-id="ff499-122">-c</span></span> | <span data-ttu-id="ff499-123">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="ff499-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="ff499-124">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="ff499-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="ff499-125">-p</span><span class="sxs-lookup"><span data-stu-id="ff499-125">-p</span></span> | <span data-ttu-id="ff499-126">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="ff499-126">--project \<PROJECT></span></span>             | <span data-ttu-id="ff499-127">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="ff499-127">The project to use.</span></span>         |
| <span data-ttu-id="ff499-128">-s</span><span class="sxs-lookup"><span data-stu-id="ff499-128">-s</span></span> | <span data-ttu-id="ff499-129">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="ff499-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="ff499-130">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="ff499-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="ff499-131">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="ff499-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="ff499-132">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="ff499-132">The target framework.</span></span>       |
|    | <span data-ttu-id="ff499-133">– configuração \<Configuração ></span><span class="sxs-lookup"><span data-stu-id="ff499-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="ff499-134">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="ff499-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="ff499-135">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="ff499-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="ff499-136">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="ff499-136">The runtime to use.</span></span>         |
| <span data-ttu-id="ff499-137">-h</span><span class="sxs-lookup"><span data-stu-id="ff499-137">-h</span></span> | <span data-ttu-id="ff499-138">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="ff499-138">--help</span></span>                           | <span data-ttu-id="ff499-139">Mostra informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="ff499-139">Show help information.</span></span>      |
| <span data-ttu-id="ff499-140">-v</span><span class="sxs-lookup"><span data-stu-id="ff499-140">-v</span></span> | <span data-ttu-id="ff499-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="ff499-141">--verbose</span></span>                        | <span data-ttu-id="ff499-142">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="ff499-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="ff499-143">– sem cor</span><span class="sxs-lookup"><span data-stu-id="ff499-143">--no-color</span></span>                       | <span data-ttu-id="ff499-144">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="ff499-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="ff499-145">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="ff499-145">--prefix-output</span></span>                  | <span data-ttu-id="ff499-146">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="ff499-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="ff499-147">Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.</span><span class="sxs-lookup"><span data-stu-id="ff499-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="ff499-148">Comandos</span><span class="sxs-lookup"><span data-stu-id="ff499-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="ff499-149">remoção de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-149">dotnet ef database drop</span></span>

<span data-ttu-id="ff499-150">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff499-150">Drops the database.</span></span>

<span data-ttu-id="ff499-151">Opções:</span><span class="sxs-lookup"><span data-stu-id="ff499-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="ff499-152">-f</span><span class="sxs-lookup"><span data-stu-id="ff499-152">-f</span></span> | <span data-ttu-id="ff499-153">-force</span><span class="sxs-lookup"><span data-stu-id="ff499-153">--force</span></span>   | <span data-ttu-id="ff499-154">Não confirme.</span><span class="sxs-lookup"><span data-stu-id="ff499-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="ff499-155">-Execute</span><span class="sxs-lookup"><span data-stu-id="ff499-155">--dry-run</span></span> | <span data-ttu-id="ff499-156">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="ff499-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="ff499-157">atualização de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-157">dotnet ef database update</span></span>

<span data-ttu-id="ff499-158">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="ff499-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="ff499-159">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="ff499-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="ff499-160">\<MIGRAÇÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-160">\<MIGRATION></span></span> | <span data-ttu-id="ff499-161">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="ff499-161">The target migration.</span></span> <span data-ttu-id="ff499-162">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="ff499-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="ff499-163">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="ff499-164">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="ff499-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="ff499-165">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="ff499-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="ff499-166">lista de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="ff499-167">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="ff499-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="ff499-168">scaffold de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="ff499-169">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff499-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="ff499-170">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="ff499-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="ff499-171">\<CONEXÃO &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-171">\<CONNECTION></span></span> | <span data-ttu-id="ff499-172">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff499-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="ff499-173">\<PROVEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-173">\<PROVIDER></span></span>   | <span data-ttu-id="ff499-174">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="ff499-174">The provider to use.</span></span> <span data-ttu-id="ff499-175">(por exemplo,</span><span class="sxs-lookup"><span data-stu-id="ff499-175">(E.g.</span></span> <span data-ttu-id="ff499-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="ff499-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="ff499-177">Opções:</span><span class="sxs-lookup"><span data-stu-id="ff499-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ff499-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="ff499-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="ff499-179">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="ff499-179">--data-annotations</span></span>                      | <span data-ttu-id="ff499-180">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="ff499-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="ff499-181">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="ff499-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="ff499-182">-c</span><span class="sxs-lookup"><span data-stu-id="ff499-182">-c</span></span>              | <span data-ttu-id="ff499-183">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="ff499-183">--context \<NAME></span></span>                       | <span data-ttu-id="ff499-184">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="ff499-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="ff499-185">-dir contexto \<caminho ></span><span class="sxs-lookup"><span data-stu-id="ff499-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="ff499-186">O diretório para colocar o arquivo DbContext no.</span><span class="sxs-lookup"><span data-stu-id="ff499-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="ff499-187">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="ff499-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="ff499-188">-f</span><span class="sxs-lookup"><span data-stu-id="ff499-188">-f</span></span>              | <span data-ttu-id="ff499-189">-force</span><span class="sxs-lookup"><span data-stu-id="ff499-189">--force</span></span>                                 | <span data-ttu-id="ff499-190">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="ff499-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="ff499-191">-o</span><span class="sxs-lookup"><span data-stu-id="ff499-191">-o</span></span>              | <span data-ttu-id="ff499-192">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="ff499-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="ff499-193">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="ff499-193">The directory to put files in.</span></span> <span data-ttu-id="ff499-194">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="ff499-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="ff499-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="ff499-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="ff499-196">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="ff499-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="ff499-197">-t</span><span class="sxs-lookup"><span data-stu-id="ff499-197">-t</span></span>              | <span data-ttu-id="ff499-198">-tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="ff499-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="ff499-199">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="ff499-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="ff499-200">– use nomes de banco de dados</span><span class="sxs-lookup"><span data-stu-id="ff499-200">--use-database-names</span></span>                    | <span data-ttu-id="ff499-201">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff499-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="ff499-202">Adicionar migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-202">dotnet ef migrations add</span></span>

<span data-ttu-id="ff499-203">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-203">Adds a new migration.</span></span>

<span data-ttu-id="ff499-204">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="ff499-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="ff499-205">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-205">\<NAME></span></span> | <span data-ttu-id="ff499-206">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-206">The name of the migration.</span></span> |

<span data-ttu-id="ff499-207">Opções:</span><span class="sxs-lookup"><span data-stu-id="ff499-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ff499-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="ff499-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="ff499-209"><nobr>-dir saída \<caminho ></nobr></span><span class="sxs-lookup"><span data-stu-id="ff499-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="ff499-210">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="ff499-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="ff499-211">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="ff499-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="ff499-212">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="ff499-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="ff499-213">lista de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-213">dotnet ef migrations list</span></span>

<span data-ttu-id="ff499-214">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="ff499-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="ff499-215">Remova as migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="ff499-216">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-216">Removes the last migration.</span></span>

<span data-ttu-id="ff499-217">Opções:</span><span class="sxs-lookup"><span data-stu-id="ff499-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="ff499-218">-f</span><span class="sxs-lookup"><span data-stu-id="ff499-218">-f</span></span> | <span data-ttu-id="ff499-219">-force</span><span class="sxs-lookup"><span data-stu-id="ff499-219">--force</span></span> | <span data-ttu-id="ff499-220">Reverta a migração se ela foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ff499-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="ff499-221">script de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="ff499-221">dotnet ef migrations script</span></span>

<span data-ttu-id="ff499-222">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="ff499-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="ff499-223">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="ff499-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="ff499-224">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-224">\<FROM></span></span> | <span data-ttu-id="ff499-225">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="ff499-225">The starting migration.</span></span> <span data-ttu-id="ff499-226">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="ff499-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="ff499-227">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="ff499-227">\<TO></span></span>   | <span data-ttu-id="ff499-228">A migração final.</span><span class="sxs-lookup"><span data-stu-id="ff499-228">The ending migration.</span></span> <span data-ttu-id="ff499-229">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="ff499-230">Opções:</span><span class="sxs-lookup"><span data-stu-id="ff499-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="ff499-231">-o</span><span class="sxs-lookup"><span data-stu-id="ff499-231">-o</span></span> | <span data-ttu-id="ff499-232">-saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="ff499-232">--output \<FILE></span></span> | <span data-ttu-id="ff499-233">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="ff499-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="ff499-234">-i</span><span class="sxs-lookup"><span data-stu-id="ff499-234">-i</span></span> | <span data-ttu-id="ff499-235">– idempotente</span><span class="sxs-lookup"><span data-stu-id="ff499-235">--idempotent</span></span>     | <span data-ttu-id="ff499-236">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="ff499-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
