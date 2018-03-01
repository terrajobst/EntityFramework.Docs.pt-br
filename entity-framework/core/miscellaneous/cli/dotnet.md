---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="77cb1-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="77cb1-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="77cb1-103">As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="77cb1-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="77cb1-104">Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="77cb1-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="77cb1-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="77cb1-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="77cb1-106">Instale as ferramentas de linha de comando do EF Core .NET usando estas etapas:</span><span class="sxs-lookup"><span data-stu-id="77cb1-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="77cb1-107">Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="77cb1-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="77cb1-108">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="77cb1-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="77cb1-109">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="77cb1-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="77cb1-110">Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="77cb1-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="77cb1-111">Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="77cb1-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="77cb1-112">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="77cb1-112">Using the tools</span></span>
---------------
<span data-ttu-id="77cb1-113">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="77cb1-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="77cb1-114">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="77cb1-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="77cb1-115">O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="77cb1-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="77cb1-116">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="77cb1-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="77cb1-117">Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="77cb1-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="77cb1-118">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="77cb1-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="77cb1-119">--json</span><span class="sxs-lookup"><span data-stu-id="77cb1-119">--json</span></span>                           | <span data-ttu-id="77cb1-120">Mostra saída JSON.</span><span class="sxs-lookup"><span data-stu-id="77cb1-120">Show JSON output.</span></span>           |
| <span data-ttu-id="77cb1-121">-c</span><span class="sxs-lookup"><span data-stu-id="77cb1-121">-c</span></span> | <span data-ttu-id="77cb1-122">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="77cb1-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="77cb1-123">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="77cb1-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="77cb1-124">-p</span><span class="sxs-lookup"><span data-stu-id="77cb1-124">-p</span></span> | <span data-ttu-id="77cb1-125">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="77cb1-125">--project \<PROJECT></span></span>             | <span data-ttu-id="77cb1-126">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="77cb1-126">The project to use.</span></span>         |
| <span data-ttu-id="77cb1-127">-s</span><span class="sxs-lookup"><span data-stu-id="77cb1-127">-s</span></span> | <span data-ttu-id="77cb1-128">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="77cb1-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="77cb1-129">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="77cb1-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="77cb1-130">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="77cb1-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="77cb1-131">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="77cb1-131">The target framework.</span></span>       |
|    | <span data-ttu-id="77cb1-132">– configuração \<Configuração ></span><span class="sxs-lookup"><span data-stu-id="77cb1-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="77cb1-133">A configuração a ser usada.</span><span class="sxs-lookup"><span data-stu-id="77cb1-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="77cb1-134">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="77cb1-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="77cb1-135">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="77cb1-135">The runtime to use.</span></span>         |
| <span data-ttu-id="77cb1-136">-h</span><span class="sxs-lookup"><span data-stu-id="77cb1-136">-h</span></span> | <span data-ttu-id="77cb1-137">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="77cb1-137">--help</span></span>                           | <span data-ttu-id="77cb1-138">Mostra informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="77cb1-138">Show help information.</span></span>      |
| <span data-ttu-id="77cb1-139">-v</span><span class="sxs-lookup"><span data-stu-id="77cb1-139">-v</span></span> | <span data-ttu-id="77cb1-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="77cb1-140">--verbose</span></span>                        | <span data-ttu-id="77cb1-141">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="77cb1-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="77cb1-142">--no-color</span><span class="sxs-lookup"><span data-stu-id="77cb1-142">--no-color</span></span>                       | <span data-ttu-id="77cb1-143">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="77cb1-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="77cb1-144">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="77cb1-144">--prefix-output</span></span>                  | <span data-ttu-id="77cb1-145">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="77cb1-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="77cb1-146">Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.</span><span class="sxs-lookup"><span data-stu-id="77cb1-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="77cb1-147">Comandos</span><span class="sxs-lookup"><span data-stu-id="77cb1-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="77cb1-148">remoção de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-148">dotnet ef database drop</span></span>

<span data-ttu-id="77cb1-149">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77cb1-149">Drops the database.</span></span>

<span data-ttu-id="77cb1-150">Opções:</span><span class="sxs-lookup"><span data-stu-id="77cb1-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="77cb1-151">-f</span><span class="sxs-lookup"><span data-stu-id="77cb1-151">-f</span></span> | <span data-ttu-id="77cb1-152">-force</span><span class="sxs-lookup"><span data-stu-id="77cb1-152">--force</span></span>   | <span data-ttu-id="77cb1-153">Não confirme.</span><span class="sxs-lookup"><span data-stu-id="77cb1-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="77cb1-154">--dry-run</span><span class="sxs-lookup"><span data-stu-id="77cb1-154">--dry-run</span></span> | <span data-ttu-id="77cb1-155">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="77cb1-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="77cb1-156">atualização de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-156">dotnet ef database update</span></span>

<span data-ttu-id="77cb1-157">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="77cb1-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="77cb1-158">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="77cb1-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="77cb1-159">\<MIGRAÇÃO ></span><span class="sxs-lookup"><span data-stu-id="77cb1-159">\<MIGRATION></span></span> | <span data-ttu-id="77cb1-160">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="77cb1-160">The target migration.</span></span> <span data-ttu-id="77cb1-161">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="77cb1-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="77cb1-162">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="77cb1-163">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="77cb1-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="77cb1-164">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="77cb1-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="77cb1-165">lista de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="77cb1-166">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="77cb1-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="77cb1-167">scaffold de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="77cb1-168">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77cb1-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="77cb1-169">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="77cb1-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="77cb1-170">\<CONEXÃO ></span><span class="sxs-lookup"><span data-stu-id="77cb1-170">\<CONNECTION></span></span> | <span data-ttu-id="77cb1-171">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77cb1-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="77cb1-172">\<PROVEDOR ></span><span class="sxs-lookup"><span data-stu-id="77cb1-172">\<PROVIDER></span></span>   | <span data-ttu-id="77cb1-173">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="77cb1-173">The provider to use.</span></span> <span data-ttu-id="77cb1-174">(Por exemplo</span><span class="sxs-lookup"><span data-stu-id="77cb1-174">(E.g.</span></span> <span data-ttu-id="77cb1-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="77cb1-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="77cb1-176">Opções:</span><span class="sxs-lookup"><span data-stu-id="77cb1-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77cb1-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="77cb1-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="77cb1-178">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="77cb1-178">--data-annotations</span></span>                      | <span data-ttu-id="77cb1-179">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="77cb1-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="77cb1-180">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="77cb1-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="77cb1-181">-c</span><span class="sxs-lookup"><span data-stu-id="77cb1-181">-c</span></span>              | <span data-ttu-id="77cb1-182">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="77cb1-182">--context \<NAME></span></span>                       | <span data-ttu-id="77cb1-183">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="77cb1-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="77cb1-184">-f</span><span class="sxs-lookup"><span data-stu-id="77cb1-184">-f</span></span>              | <span data-ttu-id="77cb1-185">-force</span><span class="sxs-lookup"><span data-stu-id="77cb1-185">--force</span></span>                                 | <span data-ttu-id="77cb1-186">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="77cb1-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="77cb1-187">-o</span><span class="sxs-lookup"><span data-stu-id="77cb1-187">-o</span></span>              | <span data-ttu-id="77cb1-188">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="77cb1-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="77cb1-189">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="77cb1-189">The directory to put files in.</span></span> <span data-ttu-id="77cb1-190">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="77cb1-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="77cb1-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="77cb1-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="77cb1-192">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="77cb1-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="77cb1-193">-t</span><span class="sxs-lookup"><span data-stu-id="77cb1-193">-t</span></span>              | <span data-ttu-id="77cb1-194">-tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="77cb1-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="77cb1-195">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="77cb1-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="77cb1-196">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="77cb1-196">--use-database-names</span></span>                    | <span data-ttu-id="77cb1-197">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77cb1-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="77cb1-198">Adicionar migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-198">dotnet ef migrations add</span></span>

<span data-ttu-id="77cb1-199">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-199">Adds a new migration.</span></span>

<span data-ttu-id="77cb1-200">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="77cb1-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="77cb1-201">\<NOME ></span><span class="sxs-lookup"><span data-stu-id="77cb1-201">\<NAME></span></span> | <span data-ttu-id="77cb1-202">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-202">The name of the migration.</span></span> |

<span data-ttu-id="77cb1-203">Opções:</span><span class="sxs-lookup"><span data-stu-id="77cb1-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77cb1-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="77cb1-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="77cb1-205"><nobr>--output-dir \<PATH></nobr></span><span class="sxs-lookup"><span data-stu-id="77cb1-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="77cb1-206">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="77cb1-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="77cb1-207">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="77cb1-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="77cb1-208">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="77cb1-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="77cb1-209">lista de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-209">dotnet ef migrations list</span></span>

<span data-ttu-id="77cb1-210">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="77cb1-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="77cb1-211">Remova as migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="77cb1-212">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-212">Removes the last migration.</span></span>

<span data-ttu-id="77cb1-213">Opções:</span><span class="sxs-lookup"><span data-stu-id="77cb1-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="77cb1-214">-f</span><span class="sxs-lookup"><span data-stu-id="77cb1-214">-f</span></span> | <span data-ttu-id="77cb1-215">-force</span><span class="sxs-lookup"><span data-stu-id="77cb1-215">--force</span></span> | <span data-ttu-id="77cb1-216">Não verificar se a migração tiver sido aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77cb1-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="77cb1-217">script de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="77cb1-217">dotnet ef migrations script</span></span>

<span data-ttu-id="77cb1-218">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="77cb1-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="77cb1-219">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="77cb1-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="77cb1-220">\<FROM></span><span class="sxs-lookup"><span data-stu-id="77cb1-220">\<FROM></span></span> | <span data-ttu-id="77cb1-221">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="77cb1-221">The starting migration.</span></span> <span data-ttu-id="77cb1-222">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="77cb1-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="77cb1-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="77cb1-223">\<TO></span></span>   | <span data-ttu-id="77cb1-224">A migração final.</span><span class="sxs-lookup"><span data-stu-id="77cb1-224">The ending migration.</span></span> <span data-ttu-id="77cb1-225">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="77cb1-226">Opções:</span><span class="sxs-lookup"><span data-stu-id="77cb1-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="77cb1-227">-o</span><span class="sxs-lookup"><span data-stu-id="77cb1-227">-o</span></span> | <span data-ttu-id="77cb1-228">-saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="77cb1-228">--output \<FILE></span></span> | <span data-ttu-id="77cb1-229">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="77cb1-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="77cb1-230">-i</span><span class="sxs-lookup"><span data-stu-id="77cb1-230">-i</span></span> | <span data-ttu-id="77cb1-231">--idempotent</span><span class="sxs-lookup"><span data-stu-id="77cb1-231">--idempotent</span></span>     | <span data-ttu-id="77cb1-232">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="77cb1-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
