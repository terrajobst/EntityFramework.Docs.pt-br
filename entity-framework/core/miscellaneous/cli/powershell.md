---
title: Referência de ferramentas de EF Core (console do Gerenciador de pacotes)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811905"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="6944a-102">Referência de ferramentas de Entity Framework Core – console do Gerenciador de pacotes no Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6944a-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="6944a-103">As ferramentas do console do Gerenciador de pacotes (PMC) para Entity Framework Core executam tarefas de desenvolvimento em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="6944a-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="6944a-104">Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplicam migrações e geram código para um modelo com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="6944a-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="6944a-105">Os comandos são executados dentro do Visual Studio usando o [console do Gerenciador de pacotes](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6944a-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="6944a-106">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6944a-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="6944a-107">Se você não estiver usando o Visual Studio, recomendamos o [EF Core ferramentas de linha de comando](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="6944a-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="6944a-108">As ferramentas da CLI são de plataforma cruzada e executadas dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="6944a-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="6944a-109">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="6944a-109">Installing the tools</span></span>

<span data-ttu-id="6944a-110">Os procedimentos para instalar e atualizar as ferramentas diferem entre ASP.NET Core 2.1 + e versões anteriores ou outros tipos de projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="6944a-111">ASP.NET Core versão 2,1 e posterior</span><span class="sxs-lookup"><span data-stu-id="6944a-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="6944a-112">As ferramentas são incluídas automaticamente em um projeto ASP.NET Core 2.1 + porque o pacote `Microsoft.EntityFrameworkCore.Tools` está incluído no [metapacote Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6944a-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6944a-113">Portanto, você não precisa fazer nada para instalar as ferramentas, mas é necessário:</span><span class="sxs-lookup"><span data-stu-id="6944a-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="6944a-114">Restaure os pacotes antes de usar as ferramentas em um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="6944a-115">Instale um pacote para atualizar as ferramentas para uma versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="6944a-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="6944a-116">Para certificar-se de que você está obtendo a versão mais recente das ferramentas, recomendamos que você também faça a seguinte etapa:</span><span class="sxs-lookup"><span data-stu-id="6944a-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="6944a-117">Edite seu arquivo *. csproj* e adicione uma linha especificando a versão mais recente do pacote [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="6944a-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="6944a-118">Por exemplo, o arquivo *. csproj* pode incluir um `ItemGroup` semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="6944a-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="6944a-119">Atualize as ferramentas quando receber uma mensagem semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="6944a-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="6944a-120">A versão de ferramentas de EF Core ' 2.1.1-RTM-30846 ' é mais antiga que a do tempo de execução ' 2.1.3-RTM-32065 '.</span><span class="sxs-lookup"><span data-stu-id="6944a-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="6944a-121">Atualize as ferramentas para os recursos e correções de bug mais recentes.</span><span class="sxs-lookup"><span data-stu-id="6944a-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="6944a-122">Para atualizar as ferramentas:</span><span class="sxs-lookup"><span data-stu-id="6944a-122">To update the tools:</span></span>

* <span data-ttu-id="6944a-123">Instale o SDK do .NET Core mais recente.</span><span class="sxs-lookup"><span data-stu-id="6944a-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="6944a-124">Atualize o Visual Studio para a versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="6944a-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="6944a-125">Edite o arquivo *. csproj* para que ele inclua uma referência de pacote para o pacote de ferramentas mais recente, conforme mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="6944a-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="6944a-126">Outras versões e tipos de projeto</span><span class="sxs-lookup"><span data-stu-id="6944a-126">Other versions and project types</span></span>

<span data-ttu-id="6944a-127">Instale as ferramentas do console do Gerenciador de pacotes executando o seguinte comando no **console do Gerenciador de pacotes**:</span><span class="sxs-lookup"><span data-stu-id="6944a-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="6944a-128">Atualize as ferramentas executando o comando a seguir no **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="6944a-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="6944a-129">Verificar a instalação</span><span class="sxs-lookup"><span data-stu-id="6944a-129">Verify the installation</span></span>

<span data-ttu-id="6944a-130">Verifique se as ferramentas estão instaladas executando este comando:</span><span class="sxs-lookup"><span data-stu-id="6944a-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="6944a-131">A saída é parecida com esta (não informa qual versão das ferramentas você está usando):</span><span class="sxs-lookup"><span data-stu-id="6944a-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="6944a-132">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="6944a-132">Using the tools</span></span>

<span data-ttu-id="6944a-133">Antes de usar as ferramentas:</span><span class="sxs-lookup"><span data-stu-id="6944a-133">Before using the tools:</span></span>

* <span data-ttu-id="6944a-134">Entenda a diferença entre o destino e o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="6944a-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="6944a-135">Saiba como usar as ferramentas com .NET Standard bibliotecas de classe.</span><span class="sxs-lookup"><span data-stu-id="6944a-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="6944a-136">Para projetos ASP.NET Core, defina o ambiente.</span><span class="sxs-lookup"><span data-stu-id="6944a-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="6944a-137">Projeto de destino e inicialização</span><span class="sxs-lookup"><span data-stu-id="6944a-137">Target and startup project</span></span>

<span data-ttu-id="6944a-138">Os comandos referem-se a um *projeto* e um *projeto de inicialização*.</span><span class="sxs-lookup"><span data-stu-id="6944a-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="6944a-139">O *projeto* também é conhecido como o *projeto de destino* porque é onde os comandos adicionam ou removem arquivos.</span><span class="sxs-lookup"><span data-stu-id="6944a-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="6944a-140">Por padrão, o **projeto padrão** selecionado no **console do Gerenciador de pacotes** é o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="6944a-141">Você pode especificar um projeto diferente como projeto de destino usando a opção <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="6944a-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="6944a-142">O *projeto de inicialização* é aquele que as ferramentas criam e executam.</span><span class="sxs-lookup"><span data-stu-id="6944a-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="6944a-143">As ferramentas precisam executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de conexão do banco de dados e a configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="6944a-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="6944a-144">Por padrão, o **projeto de inicialização** no **Gerenciador de soluções** é o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="6944a-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="6944a-145">Você pode especificar um projeto diferente como projeto de inicialização usando a opção <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="6944a-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="6944a-146">O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="6944a-147">Um cenário típico em que são projetos separados é quando:</span><span class="sxs-lookup"><span data-stu-id="6944a-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="6944a-148">O contexto de EF Core e as classes de entidade estão em uma biblioteca de classes do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6944a-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="6944a-149">Um aplicativo de console do .NET Core ou aplicativo Web faz referência à biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="6944a-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="6944a-150">Também é possível [colocar o código de migrações em uma biblioteca de classes separada do contexto de EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="6944a-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="6944a-151">Outras estruturas de destino</span><span class="sxs-lookup"><span data-stu-id="6944a-151">Other target frameworks</span></span>

<span data-ttu-id="6944a-152">As ferramentas de console do Gerenciador de pacotes funcionam com projetos .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6944a-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="6944a-153">Os aplicativos que têm o modelo de EF Core em uma biblioteca de classes de .NET Standard podem não ter um projeto do .NET Core ou do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6944a-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="6944a-154">Por exemplo, isso é verdadeiro para aplicativos Xamarin e Plataforma Universal do Windows.</span><span class="sxs-lookup"><span data-stu-id="6944a-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="6944a-155">Nesses casos, você pode criar um projeto de aplicativo de console do .NET Core ou .NET Framework, cujo único propósito é agir como projeto de inicialização para as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="6944a-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="6944a-156">O projeto pode ser um projeto fictício sem código real &mdash; só é necessário fornecer um destino para as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="6944a-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="6944a-157">Por que um projeto fictício é necessário?</span><span class="sxs-lookup"><span data-stu-id="6944a-157">Why is a dummy project required?</span></span> <span data-ttu-id="6944a-158">Conforme mencionado anteriormente, as ferramentas precisam executar o código do aplicativo em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="6944a-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="6944a-159">Para fazer isso, eles precisam usar o .NET Core ou o .NET Framework Runtime.</span><span class="sxs-lookup"><span data-stu-id="6944a-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="6944a-160">Quando o modelo de EF Core está em um projeto direcionado ao .NET Core ou .NET Framework, as ferramentas de EF Core emprestam o tempo de execução do projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="6944a-161">Eles não poderão fazer isso se o modelo de EF Core estiver em uma biblioteca de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6944a-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="6944a-162">O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs para as quais as implementações do .NET devem dar suporte.</span><span class="sxs-lookup"><span data-stu-id="6944a-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="6944a-163">Portanto .NET Standard não é suficiente para as ferramentas de EF Core executarem o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6944a-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="6944a-164">O projeto fictício que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar o .NET Standard biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="6944a-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="6944a-165">Ambiente de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6944a-165">ASP.NET Core environment</span></span>

<span data-ttu-id="6944a-166">Para especificar o ambiente para projetos de ASP.NET Core, defina **env: ASPNETCORE_ENVIRONMENT** antes de executar os comandos.</span><span class="sxs-lookup"><span data-stu-id="6944a-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="6944a-167">Parâmetros comuns</span><span class="sxs-lookup"><span data-stu-id="6944a-167">Common parameters</span></span>

<span data-ttu-id="6944a-168">A tabela a seguir mostra os parâmetros que são comuns a todos os comandos de EF Core:</span><span class="sxs-lookup"><span data-stu-id="6944a-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="6944a-169">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-169">Parameter</span></span>                 | <span data-ttu-id="6944a-170">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-171">\<> de cadeia de caracteres de contexto</span><span class="sxs-lookup"><span data-stu-id="6944a-171">-Context \<String></span></span>        | <span data-ttu-id="6944a-172">A classe `DbContext` a ser usada.</span><span class="sxs-lookup"><span data-stu-id="6944a-172">The `DbContext` class to use.</span></span> <span data-ttu-id="6944a-173">Nome da classe somente ou totalmente qualificado com namespaces.</span><span class="sxs-lookup"><span data-stu-id="6944a-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="6944a-174">Se esse parâmetro for omitido, EF Core encontrará a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="6944a-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="6944a-175">Se houver várias classes de contexto, esse parâmetro será necessário.</span><span class="sxs-lookup"><span data-stu-id="6944a-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="6944a-176">-Cadeia de caracteres de \<de projeto ></span><span class="sxs-lookup"><span data-stu-id="6944a-176">-Project \<String></span></span>        | <span data-ttu-id="6944a-177">O projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-177">The target project.</span></span> <span data-ttu-id="6944a-178">Se esse parâmetro for omitido, o **projeto padrão** do **console do Gerenciador de pacotes** será usado como o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="6944a-179">-StartupProject \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="6944a-179">-StartupProject \<String></span></span> | <span data-ttu-id="6944a-180">O projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="6944a-180">The startup project.</span></span> <span data-ttu-id="6944a-181">Se esse parâmetro for omitido, o **projeto de inicialização** nas propriedades da **solução** será usado como o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="6944a-182">-Detalhado</span><span class="sxs-lookup"><span data-stu-id="6944a-182">-Verbose</span></span>                  | <span data-ttu-id="6944a-183">Mostrar saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="6944a-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="6944a-184">Para mostrar informações de ajuda sobre um comando, use o comando `Get-Help` do PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6944a-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="6944a-185">Os parâmetros Context, Project e StartupProject dão suporte à expansão de tabulação.</span><span class="sxs-lookup"><span data-stu-id="6944a-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="6944a-186">Adicionar e migrar</span><span class="sxs-lookup"><span data-stu-id="6944a-186">Add-Migration</span></span>

<span data-ttu-id="6944a-187">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-187">Adds a new migration.</span></span>

<span data-ttu-id="6944a-188">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6944a-188">Parameters:</span></span>

| <span data-ttu-id="6944a-189">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-189">Parameter</span></span>                         | <span data-ttu-id="6944a-190">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-191">\<> cadeia de caracteres de nome de <nobr><nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="6944a-192">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-192">The name of the migration.</span></span> <span data-ttu-id="6944a-193">Esse é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="6944a-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="6944a-194"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="6944a-195">O diretório (e o subnamespace) a ser usado.</span><span class="sxs-lookup"><span data-stu-id="6944a-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="6944a-196">Os caminhos são relativos ao diretório do projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="6944a-197">O padrão é "migrações".</span><span class="sxs-lookup"><span data-stu-id="6944a-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="6944a-198">Remover banco de dados</span><span class="sxs-lookup"><span data-stu-id="6944a-198">Drop-Database</span></span>

<span data-ttu-id="6944a-199">Remove o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6944a-199">Drops the database.</span></span>

<span data-ttu-id="6944a-200">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6944a-200">Parameters:</span></span>

| <span data-ttu-id="6944a-201">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-201">Parameter</span></span> | <span data-ttu-id="6944a-202">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="6944a-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="6944a-203">-WhatIf</span></span>   | <span data-ttu-id="6944a-204">Mostrar qual banco de dados seria descartado, mas não descartá-lo.</span><span class="sxs-lookup"><span data-stu-id="6944a-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="6944a-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="6944a-205">Get-DbContext</span></span>

<span data-ttu-id="6944a-206">Obtém informações sobre um tipo de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6944a-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="6944a-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="6944a-207">Remove-Migration</span></span>

<span data-ttu-id="6944a-208">Remove a última migração (reverte as alterações de código que foram feitas para a migração).</span><span class="sxs-lookup"><span data-stu-id="6944a-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="6944a-209">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6944a-209">Parameters:</span></span>

| <span data-ttu-id="6944a-210">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-210">Parameter</span></span> | <span data-ttu-id="6944a-211">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-212">-Force</span><span class="sxs-lookup"><span data-stu-id="6944a-212">-Force</span></span>    | <span data-ttu-id="6944a-213">Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados).</span><span class="sxs-lookup"><span data-stu-id="6944a-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="6944a-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="6944a-214">Scaffold-DbContext</span></span>

<span data-ttu-id="6944a-215">Gera código para um `DbContext` e tipos de entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6944a-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="6944a-216">Para que `Scaffold-DbContext` gere um tipo de entidade, a tabela de banco de dados deve ter uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="6944a-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="6944a-217">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6944a-217">Parameters:</span></span>

| <span data-ttu-id="6944a-218">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-218">Parameter</span></span>                          | <span data-ttu-id="6944a-219">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-220"><nobr>-Cadeia de caracteres de \<de conexão ></nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="6944a-221">A cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6944a-221">The connection string to the database.</span></span> <span data-ttu-id="6944a-222">Para projetos ASP.NET Core 2. x, o valor pode ser *nome =\<nome da cadeia de conexão >* .</span><span class="sxs-lookup"><span data-stu-id="6944a-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="6944a-223">Nesse caso, o nome é proveniente das fontes de configuração que estão configuradas para o projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="6944a-224">Esse é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="6944a-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="6944a-225"><nobr>-Cadeia de caracteres de \<do provedor ></nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="6944a-226">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="6944a-226">The provider to use.</span></span> <span data-ttu-id="6944a-227">Normalmente, esse é o nome do pacote NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="6944a-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="6944a-228">Esse é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="6944a-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="6944a-229">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="6944a-229">-OutputDir \<String></span></span>               | <span data-ttu-id="6944a-230">O diretório no qual colocar arquivos.</span><span class="sxs-lookup"><span data-stu-id="6944a-230">The directory to put files in.</span></span> <span data-ttu-id="6944a-231">Os caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6944a-232">-ContextDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="6944a-232">-ContextDir \<String></span></span>              | <span data-ttu-id="6944a-233">O diretório no qual colocar o arquivo de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6944a-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="6944a-234">Os caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="6944a-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="6944a-235">\<> de cadeia de caracteres de contexto</span><span class="sxs-lookup"><span data-stu-id="6944a-235">-Context \<String></span></span>                 | <span data-ttu-id="6944a-236">O nome da classe de `DbContext` a ser gerada.</span><span class="sxs-lookup"><span data-stu-id="6944a-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="6944a-237">-Schemas \<cadeia de caracteres [] ></span><span class="sxs-lookup"><span data-stu-id="6944a-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="6944a-238">Os esquemas de tabelas para os quais gerar tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6944a-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="6944a-239">Se esse parâmetro for omitido, todos os esquemas serão incluídos.</span><span class="sxs-lookup"><span data-stu-id="6944a-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="6944a-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6944a-240">-Tables \<String[]></span></span>                | <span data-ttu-id="6944a-241">As tabelas para as quais gerar tipos de entidade.</span><span class="sxs-lookup"><span data-stu-id="6944a-241">The tables to generate entity types for.</span></span> <span data-ttu-id="6944a-242">Se esse parâmetro for omitido, todas as tabelas serão incluídas.</span><span class="sxs-lookup"><span data-stu-id="6944a-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="6944a-243">-Annotations</span><span class="sxs-lookup"><span data-stu-id="6944a-243">-DataAnnotations</span></span>                   | <span data-ttu-id="6944a-244">Use atributos para configurar o modelo (sempre que possível).</span><span class="sxs-lookup"><span data-stu-id="6944a-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="6944a-245">Se esse parâmetro for omitido, somente a API fluente será usada.</span><span class="sxs-lookup"><span data-stu-id="6944a-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="6944a-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="6944a-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="6944a-247">Use nomes de tabela e coluna exatamente como aparecem no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6944a-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="6944a-248">Se esse parâmetro for omitido, os nomes de banco de dados serão alterados C# para se adequar melhor às convenções de estilo de nome.</span><span class="sxs-lookup"><span data-stu-id="6944a-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="6944a-249">-Force</span><span class="sxs-lookup"><span data-stu-id="6944a-249">-Force</span></span>                             | <span data-ttu-id="6944a-250">Substitui os arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="6944a-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="6944a-251">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="6944a-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6944a-252">Exemplo que aplica Scaffold apenas tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:</span><span class="sxs-lookup"><span data-stu-id="6944a-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="6944a-253">Script – migração</span><span class="sxs-lookup"><span data-stu-id="6944a-253">Script-Migration</span></span>

<span data-ttu-id="6944a-254">Gera um script SQL que aplica todas as alterações de uma migração selecionada para outra migração selecionada.</span><span class="sxs-lookup"><span data-stu-id="6944a-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="6944a-255">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="6944a-255">Parameters:</span></span>

| <span data-ttu-id="6944a-256">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-256">Parameter</span></span>                | <span data-ttu-id="6944a-257">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-258">*-De* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="6944a-258">*-From* \<String></span></span>        | <span data-ttu-id="6944a-259">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="6944a-259">The starting migration.</span></span> <span data-ttu-id="6944a-260">As migrações podem ser identificadas por nome ou por ID.</span><span class="sxs-lookup"><span data-stu-id="6944a-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6944a-261">O número 0 é um caso especial que significa *antes da primeira migração*.</span><span class="sxs-lookup"><span data-stu-id="6944a-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="6944a-262">O padrão é 0.</span><span class="sxs-lookup"><span data-stu-id="6944a-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="6944a-263">*-Para* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="6944a-263">*-To* \<String></span></span>          | <span data-ttu-id="6944a-264">A migração final.</span><span class="sxs-lookup"><span data-stu-id="6944a-264">The ending migration.</span></span> <span data-ttu-id="6944a-265">O padrão é a última migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="6944a-266"><nobr>-Idempotente</nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="6944a-267">Gere um script que possa ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="6944a-268">-\<cadeia de caracteres de saída ></span><span class="sxs-lookup"><span data-stu-id="6944a-268">-Output \<String></span></span>        | <span data-ttu-id="6944a-269">O arquivo no qual o resultado será gravado.</span><span class="sxs-lookup"><span data-stu-id="6944a-269">The file to write the result to.</span></span> <span data-ttu-id="6944a-270">Se esse parâmetro for omitido, o arquivo será criado com um nome gerado na mesma pasta que os arquivos de tempo de execução do aplicativo são criados, por exemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="6944a-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="6944a-271">Os parâmetros para, de e saída dão suporte à expansão de tabulação.</span><span class="sxs-lookup"><span data-stu-id="6944a-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="6944a-272">O exemplo a seguir cria um script para a migração InitialCreate, usando o nome de migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="6944a-273">O exemplo a seguir cria um script para todas as migrações após a migração de InitialCreate, usando a ID de migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="6944a-274">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="6944a-274">Update-Database</span></span>

<span data-ttu-id="6944a-275">Atualiza o banco de dados para a última migração ou para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="6944a-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="6944a-276">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="6944a-276">Parameter</span></span>                           | <span data-ttu-id="6944a-277">Descrição</span><span class="sxs-lookup"><span data-stu-id="6944a-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6944a-278"><nobr> *-\<de cadeia de caracteres de migração* ></nobr></span><span class="sxs-lookup"><span data-stu-id="6944a-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="6944a-279">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="6944a-279">The target migration.</span></span> <span data-ttu-id="6944a-280">As migrações podem ser identificadas por nome ou por ID.</span><span class="sxs-lookup"><span data-stu-id="6944a-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6944a-281">O número 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações sejam revertidas.</span><span class="sxs-lookup"><span data-stu-id="6944a-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="6944a-282">Se nenhuma migração for especificada, o comando usa como padrão a última migração.</span><span class="sxs-lookup"><span data-stu-id="6944a-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="6944a-283">O parâmetro de migração oferece suporte à expansão de tabulação.</span><span class="sxs-lookup"><span data-stu-id="6944a-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="6944a-284">O exemplo a seguir reverte todas as migrações.</span><span class="sxs-lookup"><span data-stu-id="6944a-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="6944a-285">Os exemplos a seguir atualizam o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="6944a-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="6944a-286">O primeiro usa o nome de migração e o segundo usa a ID de migração:</span><span class="sxs-lookup"><span data-stu-id="6944a-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="6944a-287">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6944a-287">Additional resources</span></span>

* [<span data-ttu-id="6944a-288">Migrações</span><span class="sxs-lookup"><span data-stu-id="6944a-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="6944a-289">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="6944a-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
