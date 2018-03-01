---
title: Package Manager Console (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: aacf8c8564a3966db6202c9ff1c1c02a19a10814
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="95bbc-102">Ferramentas do EF Core Package Manager Console</span><span class="sxs-lookup"><span data-stu-id="95bbc-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="95bbc-103">As ferramentas de EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando o NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="95bbc-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="95bbc-104">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="95bbc-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="95bbc-105">Não usando o Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="95bbc-105">Not using Visual Studio?</span></span> <span data-ttu-id="95bbc-106">O [EF principais ferramentas de linha de comando] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="95bbc-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="95bbc-107">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="95bbc-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="95bbc-108">Instale as ferramentas do EF Core Package Manager Console instalando o pacote Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="95bbc-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="95bbc-109">Você pode instalá-lo executando o seguinte comando em [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="95bbc-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="95bbc-110">Se tudo funcionou corretamente, você deve ser capaz de executar este comando:</span><span class="sxs-lookup"><span data-stu-id="95bbc-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="95bbc-111">Se seu projeto de inicialização tem como destino .NET Standard [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="95bbc-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="95bbc-112">Se você estiver usando **Universal do Windows** ou **Xamarin**, mova seu código EF para uma biblioteca de classes .NET padrão e [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="95bbc-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="95bbc-113">Especifique a biblioteca de classe como seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="95bbc-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="95bbc-114">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="95bbc-114">Using the tools</span></span>
---------------
<span data-ttu-id="95bbc-115">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="95bbc-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="95bbc-116">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="95bbc-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="95bbc-117">O projeto de destino padrão é a **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificadas usando-parâmetro de projeto.</span><span class="sxs-lookup"><span data-stu-id="95bbc-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="95bbc-118">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="95bbc-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="95bbc-119">O padrão é um **definir como projeto de inicialização** no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="95bbc-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="95bbc-120">Ele também pode ser especificado usando o parâmetro - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="95bbc-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="95bbc-121">Parâmetros comuns:</span><span class="sxs-lookup"><span data-stu-id="95bbc-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="95bbc-122">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-122">-Context \<String></span></span>        | <span data-ttu-id="95bbc-123">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="95bbc-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="95bbc-124">-Projeto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-124">-Project \<String></span></span>        | <span data-ttu-id="95bbc-125">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="95bbc-125">The project to use.</span></span>         |
| <span data-ttu-id="95bbc-126">-StartupProject \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-126">-StartupProject \<String></span></span> | <span data-ttu-id="95bbc-127">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="95bbc-127">The startup project to use.</span></span> |
| <span data-ttu-id="95bbc-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="95bbc-128">-Verbose</span></span>                  | <span data-ttu-id="95bbc-129">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="95bbc-129">Show verbose output.</span></span>        |

<span data-ttu-id="95bbc-130">Para mostrar informações de ajuda sobre um comando, use do PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="95bbc-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="95bbc-131">Os parâmetros de contexto, o projeto e StartupProject suportam a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="95bbc-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="95bbc-132">Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95bbc-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="95bbc-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="95bbc-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="95bbc-134">Adicionar-migração</span><span class="sxs-lookup"><span data-stu-id="95bbc-134">Add-Migration</span></span>

<span data-ttu-id="95bbc-135">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-135">Adds a new migration.</span></span>

<span data-ttu-id="95bbc-136">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="95bbc-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95bbc-137">***-Name*** \<String></span><span class="sxs-lookup"><span data-stu-id="95bbc-137">***-Name*** \<String></span></span>             | <span data-ttu-id="95bbc-138">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="95bbc-139"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="95bbc-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="95bbc-140">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="95bbc-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="95bbc-141">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="95bbc-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="95bbc-142">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="95bbc-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="95bbc-143">Parâmetros em **negrito** são necessários e aqueles na *itálico* são posicional.</span><span class="sxs-lookup"><span data-stu-id="95bbc-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="95bbc-144">Remover banco de dados</span><span class="sxs-lookup"><span data-stu-id="95bbc-144">Drop-Database</span></span>

<span data-ttu-id="95bbc-145">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95bbc-145">Drops the database.</span></span>

<span data-ttu-id="95bbc-146">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="95bbc-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="95bbc-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="95bbc-147">-WhatIf</span></span> | <span data-ttu-id="95bbc-148">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="95bbc-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="95bbc-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="95bbc-149">Get-DbContext</span></span>

<span data-ttu-id="95bbc-150">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="95bbc-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="95bbc-151">Remove-migração</span><span class="sxs-lookup"><span data-stu-id="95bbc-151">Remove-Migration</span></span>

<span data-ttu-id="95bbc-152">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-152">Removes the last migration.</span></span>

<span data-ttu-id="95bbc-153">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="95bbc-153">Parameters:</span></span>

|        |                                                                       |
|:-------|:----------------------------------------------------------------------|
| <span data-ttu-id="95bbc-154">-Force</span><span class="sxs-lookup"><span data-stu-id="95bbc-154">-Force</span></span> | <span data-ttu-id="95bbc-155">Não verificar se a migração tiver sido aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95bbc-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="95bbc-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="95bbc-156">Scaffold-DbContext</span></span>

<span data-ttu-id="95bbc-157">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95bbc-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="95bbc-158">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="95bbc-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="95bbc-159"><nobr>***-Conexão*** \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="95bbc-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="95bbc-160">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95bbc-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="95bbc-161">***-Provedor*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="95bbc-162">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="95bbc-162">The provider to use.</span></span> <span data-ttu-id="95bbc-163">(Por exemplo</span><span class="sxs-lookup"><span data-stu-id="95bbc-163">(E.g.</span></span> <span data-ttu-id="95bbc-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="95bbc-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="95bbc-165">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="95bbc-166">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="95bbc-166">The directory to put files in.</span></span> <span data-ttu-id="95bbc-167">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="95bbc-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="95bbc-168">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-168">-Context \<String></span></span>                       | <span data-ttu-id="95bbc-169">O nome do DbContext para gerar.</span><span class="sxs-lookup"><span data-stu-id="95bbc-169">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="95bbc-170">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="95bbc-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="95bbc-171">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="95bbc-171">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="95bbc-172">-Tabelas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="95bbc-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="95bbc-173">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="95bbc-173">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="95bbc-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="95bbc-174">-DataAnnotations</span></span>                         | <span data-ttu-id="95bbc-175">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="95bbc-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="95bbc-176">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="95bbc-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="95bbc-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="95bbc-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="95bbc-178">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="95bbc-178">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="95bbc-179">-Force</span><span class="sxs-lookup"><span data-stu-id="95bbc-179">-Force</span></span>                                   | <span data-ttu-id="95bbc-180">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="95bbc-180">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="95bbc-181">Migração do script</span><span class="sxs-lookup"><span data-stu-id="95bbc-181">Script-Migration</span></span>

<span data-ttu-id="95bbc-182">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="95bbc-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="95bbc-183">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="95bbc-183">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="95bbc-184">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="95bbc-184">*-From* \<String></span></span> | <span data-ttu-id="95bbc-185">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="95bbc-185">The starting migration.</span></span> <span data-ttu-id="95bbc-186">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="95bbc-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="95bbc-187">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="95bbc-187">*-To* \<String></span></span>   | <span data-ttu-id="95bbc-188">A migração final.</span><span class="sxs-lookup"><span data-stu-id="95bbc-188">The ending migration.</span></span> <span data-ttu-id="95bbc-189">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="95bbc-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="95bbc-190">-Idempotent</span></span>       | <span data-ttu-id="95bbc-191">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="95bbc-192">-Saída \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="95bbc-192">-Output \<String></span></span> | <span data-ttu-id="95bbc-193">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="95bbc-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="95bbc-194">Para, e expansão da tabulação oferece suporte a parâmetros de saída.</span><span class="sxs-lookup"><span data-stu-id="95bbc-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="95bbc-195">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="95bbc-195">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="95bbc-196"><nobr>*-Migração* \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="95bbc-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="95bbc-197">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="95bbc-197">The target migration.</span></span> <span data-ttu-id="95bbc-198">Se for '0', todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="95bbc-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="95bbc-199">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="95bbc-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="95bbc-200">O parâmetro de migração dá suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="95bbc-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
