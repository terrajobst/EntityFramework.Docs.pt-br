---
title: Package Manager Console (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="25d65-102">Ferramentas do EF Core Package Manager Console</span><span class="sxs-lookup"><span data-stu-id="25d65-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="25d65-103">As ferramentas de EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando o NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="25d65-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="25d65-104">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="25d65-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="25d65-105">Não usando o Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="25d65-105">Not using Visual Studio?</span></span> <span data-ttu-id="25d65-106">O [EF principais ferramentas de linha de comando] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="25d65-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="25d65-107">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="25d65-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="25d65-108">Instale as ferramentas do EF Core Package Manager Console instalando o pacote Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="25d65-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="25d65-109">Você pode instalá-lo executando o seguinte comando em [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="25d65-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="25d65-110">Se tudo funcionou corretamente, você deve ser capaz de executar este comando:</span><span class="sxs-lookup"><span data-stu-id="25d65-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="25d65-111">Se seu projeto de inicialização tem como destino .NET Standard [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="25d65-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25d65-112">Se você estiver usando **Universal do Windows** ou **Xamarin**, mova seu código EF para uma biblioteca de classes .NET padrão e [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="25d65-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="25d65-113">Especifique a biblioteca de classe como seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="25d65-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="25d65-114">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="25d65-114">Using the tools</span></span>
---------------
<span data-ttu-id="25d65-115">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="25d65-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="25d65-116">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="25d65-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="25d65-117">O projeto de destino padrão é a **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificadas usando-parâmetro de projeto.</span><span class="sxs-lookup"><span data-stu-id="25d65-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="25d65-118">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="25d65-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="25d65-119">O padrão é um **definir como projeto de inicialização** no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="25d65-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="25d65-120">Ele também pode ser especificado usando o parâmetro - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="25d65-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="25d65-121">Parâmetros comuns:</span><span class="sxs-lookup"><span data-stu-id="25d65-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="25d65-122">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-122">-Context \<String></span></span>        | <span data-ttu-id="25d65-123">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="25d65-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="25d65-124">-Projeto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-124">-Project \<String></span></span>        | <span data-ttu-id="25d65-125">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="25d65-125">The project to use.</span></span>         |
| <span data-ttu-id="25d65-126">-StartupProject \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-126">-StartupProject \<String></span></span> | <span data-ttu-id="25d65-127">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="25d65-127">The startup project to use.</span></span> |
| <span data-ttu-id="25d65-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="25d65-128">-Verbose</span></span>                  | <span data-ttu-id="25d65-129">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="25d65-129">Show verbose output.</span></span>        |

<span data-ttu-id="25d65-130">Para mostrar informações de ajuda sobre um comando, use do PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="25d65-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="25d65-131">Os parâmetros de contexto, o projeto e StartupProject suportam a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="25d65-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="25d65-132">Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25d65-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="25d65-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="25d65-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="25d65-134">Adicionar-migração</span><span class="sxs-lookup"><span data-stu-id="25d65-134">Add-Migration</span></span>

<span data-ttu-id="25d65-135">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-135">Adds a new migration.</span></span>

<span data-ttu-id="25d65-136">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25d65-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="25d65-137">***-Nome*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-137">***-Name*** \<String></span></span>             | <span data-ttu-id="25d65-138">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="25d65-139"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="25d65-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="25d65-140">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="25d65-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="25d65-141">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="25d65-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="25d65-142">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="25d65-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="25d65-143">Parâmetros em **negrito** são necessários e aqueles na *itálico* são posicional.</span><span class="sxs-lookup"><span data-stu-id="25d65-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="25d65-144">Remover banco de dados</span><span class="sxs-lookup"><span data-stu-id="25d65-144">Drop-Database</span></span>

<span data-ttu-id="25d65-145">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25d65-145">Drops the database.</span></span>

<span data-ttu-id="25d65-146">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25d65-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="25d65-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="25d65-147">-WhatIf</span></span> | <span data-ttu-id="25d65-148">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="25d65-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="25d65-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="25d65-149">Get-DbContext</span></span>

<span data-ttu-id="25d65-150">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="25d65-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="25d65-151">Remove-migração</span><span class="sxs-lookup"><span data-stu-id="25d65-151">Remove-Migration</span></span>

<span data-ttu-id="25d65-152">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-152">Removes the last migration.</span></span>

<span data-ttu-id="25d65-153">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25d65-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="25d65-154">-Force</span><span class="sxs-lookup"><span data-stu-id="25d65-154">-Force</span></span> | <span data-ttu-id="25d65-155">Reverta a migração se ela foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25d65-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="25d65-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="25d65-156">Scaffold-DbContext</span></span>

<span data-ttu-id="25d65-157">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25d65-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="25d65-158">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25d65-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="25d65-159"><nobr>***-Conexão*** \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="25d65-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="25d65-160">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25d65-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="25d65-161">***-Provedor*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="25d65-162">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="25d65-162">The provider to use.</span></span> <span data-ttu-id="25d65-163">(por exemplo,</span><span class="sxs-lookup"><span data-stu-id="25d65-163">(E.g.</span></span> <span data-ttu-id="25d65-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="25d65-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="25d65-165">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="25d65-166">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="25d65-166">The directory to put files in.</span></span> <span data-ttu-id="25d65-167">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="25d65-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="25d65-168">-ContextDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="25d65-169">O diretório para colocar o arquivo DbContext no.</span><span class="sxs-lookup"><span data-stu-id="25d65-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="25d65-170">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="25d65-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="25d65-171">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-171">-Context \<String></span></span>                       | <span data-ttu-id="25d65-172">O nome do DbContext para gerar.</span><span class="sxs-lookup"><span data-stu-id="25d65-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="25d65-173">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="25d65-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="25d65-174">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="25d65-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="25d65-175">-Tabelas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="25d65-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="25d65-176">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="25d65-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="25d65-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="25d65-177">-DataAnnotations</span></span>                         | <span data-ttu-id="25d65-178">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="25d65-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="25d65-179">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="25d65-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="25d65-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="25d65-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="25d65-181">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25d65-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="25d65-182">-Force</span><span class="sxs-lookup"><span data-stu-id="25d65-182">-Force</span></span>                                   | <span data-ttu-id="25d65-183">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="25d65-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="25d65-184">Migração do script</span><span class="sxs-lookup"><span data-stu-id="25d65-184">Script-Migration</span></span>

<span data-ttu-id="25d65-185">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="25d65-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="25d65-186">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="25d65-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="25d65-187">*-From* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-187">*-From* \<String></span></span> | <span data-ttu-id="25d65-188">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="25d65-188">The starting migration.</span></span> <span data-ttu-id="25d65-189">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="25d65-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="25d65-190">*-* \<Cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-190">*-To* \<String></span></span>   | <span data-ttu-id="25d65-191">A migração final.</span><span class="sxs-lookup"><span data-stu-id="25d65-191">The ending migration.</span></span> <span data-ttu-id="25d65-192">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="25d65-193">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="25d65-193">-Idempotent</span></span>       | <span data-ttu-id="25d65-194">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="25d65-195">-Saída \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="25d65-195">-Output \<String></span></span> | <span data-ttu-id="25d65-196">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="25d65-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="25d65-197">Para, e expansão da tabulação oferece suporte a parâmetros de saída.</span><span class="sxs-lookup"><span data-stu-id="25d65-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="25d65-198">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="25d65-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="25d65-199"><nobr>*-Migração* \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="25d65-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="25d65-200">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="25d65-200">The target migration.</span></span> <span data-ttu-id="25d65-201">Se for '0', todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="25d65-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="25d65-202">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="25d65-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="25d65-203">O parâmetro de migração dá suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="25d65-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
