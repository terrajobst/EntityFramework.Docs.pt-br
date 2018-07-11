---
title: Console do Gerenciador de pacotes (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 0799b0cb7c5d837fdbb7a4af510a9a4d9d34ec1a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949032"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="c810e-102">Ferramentas do Console do Gerenciador de pacotes do EF Core</span><span class="sxs-lookup"><span data-stu-id="c810e-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="c810e-103">As ferramentas do EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando do NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="c810e-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="c810e-104">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c810e-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="c810e-105">Não usando o Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="c810e-105">Not using Visual Studio?</span></span> <span data-ttu-id="c810e-106">O [ferramentas de linha de comando do EF Core] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="c810e-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="c810e-107">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="c810e-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="c810e-108">Instale as ferramentas do Console do Gerenciador de pacotes do EF Core ao instalar o pacote NuGet entityframeworkcore.</span><span class="sxs-lookup"><span data-stu-id="c810e-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="c810e-109">Você pode instalá-lo executando o seguinte comando na [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="c810e-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="c810e-110">Se tudo funcionou corretamente, você deve ser capaz de executar este comando:</span><span class="sxs-lookup"><span data-stu-id="c810e-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="c810e-111">Se seu projeto de inicialização for destinado ao .NET Standard [direcionamento cruzado uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="c810e-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c810e-112">Se você estiver usando **Windows Universal** ou **Xamarin**, mova seu código do EF para uma biblioteca de classes .NET Standard e [direcionamento cruzado uma estrutura com suporte] [ 3] antes de usar as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="c810e-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="c810e-113">Especifique a biblioteca de classes como seu projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c810e-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="c810e-114">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="c810e-114">Using the tools</span></span>
---------------
<span data-ttu-id="c810e-115">Sempre que você invoca um comando, existem dois projetos envolvidos:</span><span class="sxs-lookup"><span data-stu-id="c810e-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="c810e-116">O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos).</span><span class="sxs-lookup"><span data-stu-id="c810e-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="c810e-117">O projeto de destino padrão é o **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificados usando o - parâmetro de projeto.</span><span class="sxs-lookup"><span data-stu-id="c810e-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="c810e-118">O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="c810e-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="c810e-119">O padrão é uma **definir como projeto de inicialização** no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="c810e-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="c810e-120">Ele também pode ser especificado usando o parâmetro - o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="c810e-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="c810e-121">Parâmetros comuns:</span><span class="sxs-lookup"><span data-stu-id="c810e-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="c810e-122">-O contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-122">-Context \<String></span></span>        | <span data-ttu-id="c810e-123">O DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="c810e-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="c810e-124">-Projeto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-124">-Project \<String></span></span>        | <span data-ttu-id="c810e-125">O projeto para usar.</span><span class="sxs-lookup"><span data-stu-id="c810e-125">The project to use.</span></span>         |
| <span data-ttu-id="c810e-126">O projeto de inicialização - \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-126">-StartupProject \<String></span></span> | <span data-ttu-id="c810e-127">O projeto de inicialização para usar.</span><span class="sxs-lookup"><span data-stu-id="c810e-127">The startup project to use.</span></span> |
| <span data-ttu-id="c810e-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="c810e-128">-Verbose</span></span>                  | <span data-ttu-id="c810e-129">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="c810e-129">Show verbose output.</span></span>        |

<span data-ttu-id="c810e-130">Para mostrar informações de ajuda sobre um comando, use o PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="c810e-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="c810e-131">Os parâmetros de contexto, o projeto e o projeto de inicialização oferecem suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="c810e-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="c810e-132">Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c810e-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="c810e-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="c810e-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="c810e-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="c810e-134">Add-Migration</span></span>

<span data-ttu-id="c810e-135">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-135">Adds a new migration.</span></span>

<span data-ttu-id="c810e-136">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c810e-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c810e-137">***-Name*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-137">***-Name*** \<String></span></span>             | <span data-ttu-id="c810e-138">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="c810e-139"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="c810e-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="c810e-140">O diretório (e sub-namespace) para usar.</span><span class="sxs-lookup"><span data-stu-id="c810e-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="c810e-141">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c810e-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="c810e-142">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="c810e-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="c810e-143">Parâmetros em **negrito** são necessárias e aqueles na *itálico* são posicionais.</span><span class="sxs-lookup"><span data-stu-id="c810e-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="c810e-144">Banco de dados de destino</span><span class="sxs-lookup"><span data-stu-id="c810e-144">Drop-Database</span></span>

<span data-ttu-id="c810e-145">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c810e-145">Drops the database.</span></span>

<span data-ttu-id="c810e-146">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c810e-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="c810e-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="c810e-147">-WhatIf</span></span> | <span data-ttu-id="c810e-148">Mostrar qual banco de dados deve ser descartado, mas não o remova.</span><span class="sxs-lookup"><span data-stu-id="c810e-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="c810e-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="c810e-149">Get-DbContext</span></span>

<span data-ttu-id="c810e-150">Obtém informações sobre um tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="c810e-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="c810e-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="c810e-151">Remove-Migration</span></span>

<span data-ttu-id="c810e-152">Remove a última migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-152">Removes the last migration.</span></span>

<span data-ttu-id="c810e-153">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c810e-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="c810e-154">-Force</span><span class="sxs-lookup"><span data-stu-id="c810e-154">-Force</span></span> | <span data-ttu-id="c810e-155">Reverta a migração, se ele tiver sido aplicado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c810e-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="c810e-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="c810e-156">Scaffold-DbContext</span></span>

<span data-ttu-id="c810e-157">Usa o Scaffold de um DbContext e tipos de entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c810e-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="c810e-158">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c810e-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="c810e-159"><nobr>***-Conexão*** \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="c810e-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="c810e-160">A cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c810e-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="c810e-161">***-Provedor*** \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="c810e-162">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="c810e-162">The provider to use.</span></span> <span data-ttu-id="c810e-163">(por exemplo, entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="c810e-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="c810e-164">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="c810e-165">O diretório para colocar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="c810e-165">The directory to put files in.</span></span> <span data-ttu-id="c810e-166">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c810e-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="c810e-167">-ContextDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="c810e-168">O diretório para colocar o arquivo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="c810e-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="c810e-169">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c810e-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="c810e-170">-O contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-170">-Context \<String></span></span>                       | <span data-ttu-id="c810e-171">O nome do DbContext para gerar.</span><span class="sxs-lookup"><span data-stu-id="c810e-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="c810e-172">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="c810e-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="c810e-173">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="c810e-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="c810e-174">-Tabelas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="c810e-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="c810e-175">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="c810e-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="c810e-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="c810e-176">-DataAnnotations</span></span>                         | <span data-ttu-id="c810e-177">Use atributos para configurar o modelo (quando possível).</span><span class="sxs-lookup"><span data-stu-id="c810e-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="c810e-178">Se omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="c810e-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="c810e-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="c810e-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="c810e-180">Use nomes de tabela e coluna diretamente do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c810e-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="c810e-181">-Force</span><span class="sxs-lookup"><span data-stu-id="c810e-181">-Force</span></span>                                   | <span data-ttu-id="c810e-182">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="c810e-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="c810e-183">Migração do script</span><span class="sxs-lookup"><span data-stu-id="c810e-183">Script-Migration</span></span>

<span data-ttu-id="c810e-184">Gera um script SQL de migrações.</span><span class="sxs-lookup"><span data-stu-id="c810e-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="c810e-185">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="c810e-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="c810e-186">*-From* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-186">*-From* \<String></span></span> | <span data-ttu-id="c810e-187">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="c810e-187">The starting migration.</span></span> <span data-ttu-id="c810e-188">O padrão é 0 (o banco de dados inicial).</span><span class="sxs-lookup"><span data-stu-id="c810e-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="c810e-189">*-Para* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-189">*-To* \<String></span></span>   | <span data-ttu-id="c810e-190">A migração final.</span><span class="sxs-lookup"><span data-stu-id="c810e-190">The ending migration.</span></span> <span data-ttu-id="c810e-191">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="c810e-192">-Idempotentes</span><span class="sxs-lookup"><span data-stu-id="c810e-192">-Idempotent</span></span>       | <span data-ttu-id="c810e-193">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="c810e-194">-Saída \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="c810e-194">-Output \<String></span></span> | <span data-ttu-id="c810e-195">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="c810e-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="c810e-196">Para, de, e os parâmetros de saída oferecem suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="c810e-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="c810e-197">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="c810e-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="c810e-198"><nobr>*-Migration* \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="c810e-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="c810e-199">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="c810e-199">The target migration.</span></span> <span data-ttu-id="c810e-200">Se for '0', todas as migrações serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="c810e-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="c810e-201">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="c810e-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="c810e-202">O parâmetro de migração dá suporte à expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="c810e-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
