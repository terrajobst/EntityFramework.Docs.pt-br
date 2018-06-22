---
title: Package Manager Console (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812554"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="dc290-102">Ferramentas do EF Core Package Manager Console</span><span class="sxs-lookup"><span data-stu-id="dc290-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="dc290-103">As ferramentas de EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando o NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="dc290-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="dc290-104">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc290-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="dc290-105">Não usando o Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="dc290-105">Not using Visual Studio?</span></span> <span data-ttu-id="dc290-106">O [EF principais ferramentas de linha de comando] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="dc290-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="dc290-107">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="dc290-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="dc290-108">Instale as ferramentas do EF Core Package Manager Console instalando o pacote Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="dc290-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="dc290-109">Você pode instalá-lo executando o seguinte comando em [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="dc290-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="dc290-110">Se tudo funcionou corretamente, você deve ser capaz de executar este comando:</span><span class="sxs-lookup"><span data-stu-id="dc290-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="dc290-111">Se seu projeto de inicialização tem como destino .NET Standard [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="dc290-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc290-112">Se você estiver usando **Universal do Windows** ou **Xamarin**, mova seu código EF para uma biblioteca de classes .NET padrão e [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="dc290-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="dc290-113">Especifique a biblioteca de classe como seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="dc290-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="dc290-114">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="dc290-114">Using the tools</span></span>
---------------
<span data-ttu-id="dc290-115">Sempre que você chamar um comando, há dois projetos envolvidas:</span><span class="sxs-lookup"><span data-stu-id="dc290-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="dc290-116">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="dc290-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="dc290-117">O projeto de destino padrão é a **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificadas usando-parâmetro de projeto.</span><span class="sxs-lookup"><span data-stu-id="dc290-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="dc290-118">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="dc290-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="dc290-119">O padrão é um **definir como projeto de inicialização** no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="dc290-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="dc290-120">Ele também pode ser especificado usando o parâmetro - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="dc290-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="dc290-121">Parâmetros comuns:</span><span class="sxs-lookup"><span data-stu-id="dc290-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="dc290-122">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-122">-Context \<String></span></span>        | <span data-ttu-id="dc290-123">O DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="dc290-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="dc290-124">-Projeto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-124">-Project \<String></span></span>        | <span data-ttu-id="dc290-125">O projeto a usar.</span><span class="sxs-lookup"><span data-stu-id="dc290-125">The project to use.</span></span>         |
| <span data-ttu-id="dc290-126">-StartupProject \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-126">-StartupProject \<String></span></span> | <span data-ttu-id="dc290-127">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="dc290-127">The startup project to use.</span></span> |
| <span data-ttu-id="dc290-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="dc290-128">-Verbose</span></span>                  | <span data-ttu-id="dc290-129">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="dc290-129">Show verbose output.</span></span>        |

<span data-ttu-id="dc290-130">Para mostrar informações de ajuda sobre um comando, use do PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="dc290-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="dc290-131">Os parâmetros de contexto, o projeto e StartupProject suportam a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="dc290-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="dc290-132">Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc290-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="dc290-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="dc290-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="dc290-134">Adicionar-migração</span><span class="sxs-lookup"><span data-stu-id="dc290-134">Add-Migration</span></span>

<span data-ttu-id="dc290-135">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-135">Adds a new migration.</span></span>

<span data-ttu-id="dc290-136">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="dc290-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc290-137">***-Nome*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-137">***-Name*** \<String></span></span>             | <span data-ttu-id="dc290-138">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="dc290-139"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="dc290-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="dc290-140">O diretório (e sub-namespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="dc290-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="dc290-141">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="dc290-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="dc290-142">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="dc290-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="dc290-143">Parâmetros em **negrito** são necessários e aqueles na *itálico* são posicional.</span><span class="sxs-lookup"><span data-stu-id="dc290-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="dc290-144">Remover banco de dados</span><span class="sxs-lookup"><span data-stu-id="dc290-144">Drop-Database</span></span>

<span data-ttu-id="dc290-145">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc290-145">Drops the database.</span></span>

<span data-ttu-id="dc290-146">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="dc290-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="dc290-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="dc290-147">-WhatIf</span></span> | <span data-ttu-id="dc290-148">Mostrar qual banco de dados deve ser descartado, mas não solte-o.</span><span class="sxs-lookup"><span data-stu-id="dc290-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="dc290-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="dc290-149">Get-DbContext</span></span>

<span data-ttu-id="dc290-150">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="dc290-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="dc290-151">Remove-migração</span><span class="sxs-lookup"><span data-stu-id="dc290-151">Remove-Migration</span></span>

<span data-ttu-id="dc290-152">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-152">Removes the last migration.</span></span>

<span data-ttu-id="dc290-153">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="dc290-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="dc290-154">-Force</span><span class="sxs-lookup"><span data-stu-id="dc290-154">-Force</span></span> | <span data-ttu-id="dc290-155">Reverta a migração se ela foi aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc290-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="dc290-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="dc290-156">Scaffold-DbContext</span></span>

<span data-ttu-id="dc290-157">Scaffolds um tipos DbContext e entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc290-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="dc290-158">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="dc290-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc290-159"><nobr>***-Conexão*** \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="dc290-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="dc290-160">A cadeia de caracteres de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc290-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="dc290-161">***-Provedor*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="dc290-162">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="dc290-162">The provider to use.</span></span> <span data-ttu-id="dc290-163">(por exemplo,</span><span class="sxs-lookup"><span data-stu-id="dc290-163">(E.g.</span></span> <span data-ttu-id="dc290-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="dc290-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="dc290-165">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="dc290-166">O diretório de colocar arquivos em.</span><span class="sxs-lookup"><span data-stu-id="dc290-166">The directory to put files in.</span></span> <span data-ttu-id="dc290-167">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="dc290-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="dc290-168">-ContextDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="dc290-169">O diretório para colocar o arquivo DbContext no.</span><span class="sxs-lookup"><span data-stu-id="dc290-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="dc290-170">Caminhos são relativas ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="dc290-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="dc290-171">-Contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-171">-Context \<String></span></span>                       | <span data-ttu-id="dc290-172">O nome do DbContext para gerar.</span><span class="sxs-lookup"><span data-stu-id="dc290-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="dc290-173">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="dc290-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="dc290-174">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="dc290-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="dc290-175">-Tabelas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="dc290-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="dc290-176">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="dc290-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="dc290-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="dc290-177">-DataAnnotations</span></span>                         | <span data-ttu-id="dc290-178">Use atributos para configurar o modelo (onde for possível).</span><span class="sxs-lookup"><span data-stu-id="dc290-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="dc290-179">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="dc290-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="dc290-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="dc290-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="dc290-181">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="dc290-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="dc290-182">-Force</span><span class="sxs-lookup"><span data-stu-id="dc290-182">-Force</span></span>                                   | <span data-ttu-id="dc290-183">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="dc290-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="dc290-184">Migração do script</span><span class="sxs-lookup"><span data-stu-id="dc290-184">Script-Migration</span></span>

<span data-ttu-id="dc290-185">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="dc290-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="dc290-186">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="dc290-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="dc290-187">*-From* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-187">*-From* \<String></span></span> | <span data-ttu-id="dc290-188">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="dc290-188">The starting migration.</span></span> <span data-ttu-id="dc290-189">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="dc290-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="dc290-190">*-* \<Cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-190">*-To* \<String></span></span>   | <span data-ttu-id="dc290-191">A migração final.</span><span class="sxs-lookup"><span data-stu-id="dc290-191">The ending migration.</span></span> <span data-ttu-id="dc290-192">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="dc290-193">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="dc290-193">-Idempotent</span></span>       | <span data-ttu-id="dc290-194">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="dc290-195">-Saída \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="dc290-195">-Output \<String></span></span> | <span data-ttu-id="dc290-196">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="dc290-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="dc290-197">Para, e expansão da tabulação oferece suporte a parâmetros de saída.</span><span class="sxs-lookup"><span data-stu-id="dc290-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="dc290-198">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="dc290-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc290-199"><nobr>*-Migração* \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="dc290-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="dc290-200">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="dc290-200">The target migration.</span></span> <span data-ttu-id="dc290-201">Se for '0', todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="dc290-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="dc290-202">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="dc290-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="dc290-203">O parâmetro de migração dá suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="dc290-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
