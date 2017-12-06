---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="91dfb-102">Ferramentas de linha de comando do EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="91dfb-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="91dfb-103">As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="91dfb-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="91dfb-104">Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.</span><span class="sxs-lookup"><span data-stu-id="91dfb-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="91dfb-105">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="91dfb-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="91dfb-106">Instale as ferramentas de linha de comando do EF Core .NET usando estas etapas:</span><span class="sxs-lookup"><span data-stu-id="91dfb-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="91dfb-107">Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)</span><span class="sxs-lookup"><span data-stu-id="91dfb-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="91dfb-108">Execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="91dfb-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="91dfb-109">O projeto resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="91dfb-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="91dfb-110">Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="91dfb-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="91dfb-111">Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="91dfb-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="91dfb-112">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="91dfb-112">Using the tools</span></span>
---------------
<span data-ttu-id="91dfb-113">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="91dfb-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="91dfb-114">O projeto de destino é onde todos os arquivos são adicionados (ou, em alguns casos removidos).</span><span class="sxs-lookup"><span data-stu-id="91dfb-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="91dfb-115">O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="91dfb-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="91dfb-116">O projeto de inicialização é emulado pelas ferramentas durante a execução de código do projeto.</span><span class="sxs-lookup"><span data-stu-id="91dfb-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="91dfb-117">Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.</span><span class="sxs-lookup"><span data-stu-id="91dfb-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="91dfb-118">Opções comuns:</span><span class="sxs-lookup"><span data-stu-id="91dfb-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="91dfb-119">– o json</span><span class="sxs-lookup"><span data-stu-id="91dfb-119">--json</span></span>                           | <span data-ttu-id="91dfb-120">Mostra saída JSON.</span><span class="sxs-lookup"><span data-stu-id="91dfb-120">Show JSON output.</span></span>           |
| <span data-ttu-id="91dfb-121">-c</span><span class="sxs-lookup"><span data-stu-id="91dfb-121">-c</span></span> | <span data-ttu-id="91dfb-122">– contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="91dfb-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="91dfb-123">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="91dfb-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="91dfb-124">-P</span><span class="sxs-lookup"><span data-stu-id="91dfb-124">-p</span></span> | <span data-ttu-id="91dfb-125">– projeto \<projeto ></span><span class="sxs-lookup"><span data-stu-id="91dfb-125">--project \<PROJECT></span></span>             | <span data-ttu-id="91dfb-126">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="91dfb-126">The project to use.</span></span>         |
| <span data-ttu-id="91dfb-127">-s</span><span class="sxs-lookup"><span data-stu-id="91dfb-127">-s</span></span> | <span data-ttu-id="91dfb-128">– projeto de inicialização \<projeto ></span><span class="sxs-lookup"><span data-stu-id="91dfb-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="91dfb-129">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="91dfb-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="91dfb-130">– framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="91dfb-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="91dfb-131">A estrutura de destino.</span><span class="sxs-lookup"><span data-stu-id="91dfb-131">The target framework.</span></span>       |
|    | <span data-ttu-id="91dfb-132">– configuração \<Configuração ></span><span class="sxs-lookup"><span data-stu-id="91dfb-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="91dfb-133">A configuração para uso.</span><span class="sxs-lookup"><span data-stu-id="91dfb-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="91dfb-134">o tempo de execução – \<identificador ></span><span class="sxs-lookup"><span data-stu-id="91dfb-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="91dfb-135">O tempo de execução para usar.</span><span class="sxs-lookup"><span data-stu-id="91dfb-135">The runtime to use.</span></span>         |
| <span data-ttu-id="91dfb-136">-h</span><span class="sxs-lookup"><span data-stu-id="91dfb-136">-h</span></span> | <span data-ttu-id="91dfb-137">– Ajuda</span><span class="sxs-lookup"><span data-stu-id="91dfb-137">--help</span></span>                           | <span data-ttu-id="91dfb-138">Mostra informações de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="91dfb-138">Show help information.</span></span>      |
| <span data-ttu-id="91dfb-139">-v</span><span class="sxs-lookup"><span data-stu-id="91dfb-139">-v</span></span> | <span data-ttu-id="91dfb-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="91dfb-140">--verbose</span></span>                        | <span data-ttu-id="91dfb-141">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="91dfb-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="91dfb-142">– sem cor</span><span class="sxs-lookup"><span data-stu-id="91dfb-142">--no-color</span></span>                       | <span data-ttu-id="91dfb-143">Não colorir saída.</span><span class="sxs-lookup"><span data-stu-id="91dfb-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="91dfb-144">-saída de prefixo</span><span class="sxs-lookup"><span data-stu-id="91dfb-144">--prefix-output</span></span>                  | <span data-ttu-id="91dfb-145">Prefixo com o nível de saída.</span><span class="sxs-lookup"><span data-stu-id="91dfb-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="91dfb-146">Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.</span><span class="sxs-lookup"><span data-stu-id="91dfb-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="91dfb-147">Comandos</span><span class="sxs-lookup"><span data-stu-id="91dfb-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="91dfb-148">remoção de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-148">dotnet ef database drop</span></span>

<span data-ttu-id="91dfb-149">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91dfb-149">Drops the database.</span></span>

<span data-ttu-id="91dfb-150">Opções:</span><span class="sxs-lookup"><span data-stu-id="91dfb-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="91dfb-151">-f</span><span class="sxs-lookup"><span data-stu-id="91dfb-151">-f</span></span> | <span data-ttu-id="91dfb-152">-force</span><span class="sxs-lookup"><span data-stu-id="91dfb-152">--force</span></span>   | <span data-ttu-id="91dfb-153">Não confirme.</span><span class="sxs-lookup"><span data-stu-id="91dfb-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="91dfb-154">-Execute</span><span class="sxs-lookup"><span data-stu-id="91dfb-154">--dry-run</span></span> | <span data-ttu-id="91dfb-155">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="91dfb-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="91dfb-156">atualização de banco de dados de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-156">dotnet ef database update</span></span>

<span data-ttu-id="91dfb-157">Atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="91dfb-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="91dfb-158">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="91dfb-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="91dfb-159">\<MIGRAÇÃO ></span><span class="sxs-lookup"><span data-stu-id="91dfb-159">\<MIGRATION></span></span> | <span data-ttu-id="91dfb-160">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="91dfb-160">The target migration.</span></span> <span data-ttu-id="91dfb-161">Se for 0, todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="91dfb-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="91dfb-162">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="91dfb-163">informações do dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="91dfb-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="91dfb-164">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="91dfb-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="91dfb-165">lista de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="91dfb-166">Lista os tipos disponíveis de DbContext.</span><span class="sxs-lookup"><span data-stu-id="91dfb-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="91dfb-167">scaffold de dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="91dfb-168">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91dfb-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="91dfb-169">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="91dfb-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="91dfb-170">\<CONEXÃO ></span><span class="sxs-lookup"><span data-stu-id="91dfb-170">\<CONNECTION></span></span> | <span data-ttu-id="91dfb-171">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91dfb-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="91dfb-172">\<PROVEDOR ></span><span class="sxs-lookup"><span data-stu-id="91dfb-172">\<PROVIDER></span></span>   | <span data-ttu-id="91dfb-173">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="91dfb-173">The provider to use.</span></span> <span data-ttu-id="91dfb-174">(Por ex.:</span><span class="sxs-lookup"><span data-stu-id="91dfb-174">(E.g.</span></span> <span data-ttu-id="91dfb-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="91dfb-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="91dfb-176">Opções:</span><span class="sxs-lookup"><span data-stu-id="91dfb-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="91dfb-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="91dfb-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="91dfb-178">– as anotações de dados</span><span class="sxs-lookup"><span data-stu-id="91dfb-178">--data-annotations</span></span>                | <span data-ttu-id="91dfb-179">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="91dfb-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="91dfb-180">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="91dfb-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="91dfb-181">-c</span><span class="sxs-lookup"><span data-stu-id="91dfb-181">-c</span></span>        |       <span data-ttu-id="91dfb-182">– contexto \<nome ></span><span class="sxs-lookup"><span data-stu-id="91dfb-182">--context \<NAME></span></span>                 | <span data-ttu-id="91dfb-183">O nome do DbContext.</span><span class="sxs-lookup"><span data-stu-id="91dfb-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="91dfb-184">-f</span><span class="sxs-lookup"><span data-stu-id="91dfb-184">-f</span></span>        |       <span data-ttu-id="91dfb-185">-force</span><span class="sxs-lookup"><span data-stu-id="91dfb-185">--force</span></span>                           | <span data-ttu-id="91dfb-186">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="91dfb-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="91dfb-187">-o</span><span class="sxs-lookup"><span data-stu-id="91dfb-187">-o</span></span>        |       <span data-ttu-id="91dfb-188">-dir saída \<caminho ></span><span class="sxs-lookup"><span data-stu-id="91dfb-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="91dfb-189">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="91dfb-189">The directory to put files in.</span></span> <span data-ttu-id="91dfb-190">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="91dfb-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="91dfb-191"><nobr>– esquema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="91dfb-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="91dfb-192">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="91dfb-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="91dfb-193">-t</span><span class="sxs-lookup"><span data-stu-id="91dfb-193">-t</span></span>        |       <span data-ttu-id="91dfb-194">-tabela \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="91dfb-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="91dfb-195">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="91dfb-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="91dfb-196">– use nomes de banco de dados</span><span class="sxs-lookup"><span data-stu-id="91dfb-196">--use-database-names</span></span>              | <span data-ttu-id="91dfb-197">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91dfb-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="91dfb-198">Adicionar migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-198">dotnet ef migrations add</span></span>

<span data-ttu-id="91dfb-199">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-199">Adds a new migration.</span></span>

<span data-ttu-id="91dfb-200">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="91dfb-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="91dfb-201">\<NOME ></span><span class="sxs-lookup"><span data-stu-id="91dfb-201">\<NAME></span></span> | <span data-ttu-id="91dfb-202">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-202">The name of the migration.</span></span> |

<span data-ttu-id="91dfb-203">Opções:</span><span class="sxs-lookup"><span data-stu-id="91dfb-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="91dfb-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="91dfb-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="91dfb-205"><nobr>-dir saída \<caminho ></nobr></span><span class="sxs-lookup"><span data-stu-id="91dfb-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="91dfb-206">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="91dfb-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="91dfb-207">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="91dfb-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="91dfb-208">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="91dfb-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="91dfb-209">lista de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-209">dotnet ef migrations list</span></span>

<span data-ttu-id="91dfb-210">Lista as migrações disponíveis.</span><span class="sxs-lookup"><span data-stu-id="91dfb-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="91dfb-211">Remova as migrações de ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="91dfb-212">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-212">Removes the last migration.</span></span>

<span data-ttu-id="91dfb-213">Opções:</span><span class="sxs-lookup"><span data-stu-id="91dfb-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="91dfb-214">-f</span><span class="sxs-lookup"><span data-stu-id="91dfb-214">-f</span></span> | <span data-ttu-id="91dfb-215">-force</span><span class="sxs-lookup"><span data-stu-id="91dfb-215">--force</span></span> | <span data-ttu-id="91dfb-216">Não verificar se a migração tiver sido aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="91dfb-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="91dfb-217">script de migrações ef dotnet</span><span class="sxs-lookup"><span data-stu-id="91dfb-217">dotnet ef migrations script</span></span>

<span data-ttu-id="91dfb-218">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="91dfb-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="91dfb-219">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="91dfb-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="91dfb-220">\<DE ></span><span class="sxs-lookup"><span data-stu-id="91dfb-220">\<FROM></span></span> | <span data-ttu-id="91dfb-221">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="91dfb-221">The starting migration.</span></span> <span data-ttu-id="91dfb-222">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="91dfb-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="91dfb-223">\<PARA ></span><span class="sxs-lookup"><span data-stu-id="91dfb-223">\<TO></span></span>   | <span data-ttu-id="91dfb-224">A migração final.</span><span class="sxs-lookup"><span data-stu-id="91dfb-224">The ending migration.</span></span> <span data-ttu-id="91dfb-225">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="91dfb-226">Opções:</span><span class="sxs-lookup"><span data-stu-id="91dfb-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="91dfb-227">-o</span><span class="sxs-lookup"><span data-stu-id="91dfb-227">-o</span></span> | <span data-ttu-id="91dfb-228">-saída \<arquivo ></span><span class="sxs-lookup"><span data-stu-id="91dfb-228">--output \<FILE></span></span> | <span data-ttu-id="91dfb-229">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="91dfb-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="91dfb-230">-i</span><span class="sxs-lookup"><span data-stu-id="91dfb-230">-i</span></span> | <span data-ttu-id="91dfb-231">– idempotente</span><span class="sxs-lookup"><span data-stu-id="91dfb-231">--idempotent</span></span>     | <span data-ttu-id="91dfb-232">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="91dfb-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
