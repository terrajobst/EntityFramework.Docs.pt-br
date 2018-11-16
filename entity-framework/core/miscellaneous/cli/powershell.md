---
title: EF Core ferramentas reference (Package Manager Console) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 468698d1bbd17d4ad10b1b1601bfbc315a01c1ff
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688700"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="8a65b-102">Referência - Package Manager Console no Visual Studio das ferramentas do Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8a65b-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="8a65b-103">As ferramentas do Console de Gerenciador de pacote (PMC) para o Entity Framework Core realizar tarefas de desenvolvimento de tempo de design.</span><span class="sxs-lookup"><span data-stu-id="8a65b-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="8a65b-104">Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar as migrações e gerar código para um modelo com base em um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="8a65b-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="8a65b-105">Os comandos são executados dentro do Visual Studio usando o [Package Manager Console](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="8a65b-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="8a65b-106">Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a65b-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="8a65b-107">Se você não estiver usando o Visual Studio, é recomendável o [ferramentas de linha de comando do EF Core](dotnet.md) em vez disso.</span><span class="sxs-lookup"><span data-stu-id="8a65b-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="8a65b-108">As ferramentas da CLI são multiplataforma e execução dentro de um prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="8a65b-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="8a65b-109">Instalando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="8a65b-109">Installing the tools</span></span>

<span data-ttu-id="8a65b-110">Os procedimentos para instalar e atualizar as ferramentas são diferentes entre o ASP.NET Core 2.1 + e versões anteriores ou outros tipos de projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="8a65b-111">ASP.NET Core versão 2.1 e posterior</span><span class="sxs-lookup"><span data-stu-id="8a65b-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="8a65b-112">As ferramentas são incluídas automaticamente em um projeto ASP.NET Core 2.1 + porque o `Microsoft.EntityFrameworkCore.Tools` pacote está incluído na [metapacote Microsoft](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8a65b-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="8a65b-113">Portanto, você não precisa fazer nada para instalar as ferramentas, mas você precisa:</span><span class="sxs-lookup"><span data-stu-id="8a65b-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="8a65b-114">Restaure os pacotes antes de usar as ferramentas em um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="8a65b-115">Instale um pacote para atualizar as ferramentas para uma versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="8a65b-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="8a65b-116">Para certificar-se de que você obtenha a versão mais recente das ferramentas, é recomendável que você também pode fazer a seguinte etapa:</span><span class="sxs-lookup"><span data-stu-id="8a65b-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="8a65b-117">Editar seu *. csproj* arquivo e adicione uma linha especificando a versão mais recente do [entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pacote.</span><span class="sxs-lookup"><span data-stu-id="8a65b-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="8a65b-118">Por exemplo, o *. csproj* arquivo pode incluir um `ItemGroup` parecido com este:</span><span class="sxs-lookup"><span data-stu-id="8a65b-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="8a65b-119">Atualize as ferramentas quando você receber uma mensagem semelhante ao seguinte exemplo:</span><span class="sxs-lookup"><span data-stu-id="8a65b-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="8a65b-120">A versão das ferramentas do EF Core '2.1.1-rtm-30846' é mais antiga do que o tempo de execução '2.1.3-rtm-32065'.</span><span class="sxs-lookup"><span data-stu-id="8a65b-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="8a65b-121">Atualize as ferramentas para os últimos recursos e correções de bugs.</span><span class="sxs-lookup"><span data-stu-id="8a65b-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="8a65b-122">Para atualizar as ferramentas:</span><span class="sxs-lookup"><span data-stu-id="8a65b-122">To update the tools:</span></span>
* <span data-ttu-id="8a65b-123">Instale o SDK do .NET Core mais recentes.</span><span class="sxs-lookup"><span data-stu-id="8a65b-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="8a65b-124">Atualize o Visual Studio para a versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="8a65b-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="8a65b-125">Editar o *. csproj* de arquivo para que ele inclua uma referência de pacote para o pacote de ferramentas mais recente, conforme mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8a65b-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="8a65b-126">Outras versões e tipos de projeto</span><span class="sxs-lookup"><span data-stu-id="8a65b-126">Other versions and project types</span></span>

<span data-ttu-id="8a65b-127">Instalar as ferramentas do Console do Gerenciador de pacotes, executando o seguinte comando em **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="8a65b-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="8a65b-128">Atualizar as ferramentas, executando o seguinte comando em **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="8a65b-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="8a65b-129">Verificar a instalação</span><span class="sxs-lookup"><span data-stu-id="8a65b-129">Verify the installation</span></span>

<span data-ttu-id="8a65b-130">Verifique se que as ferramentas são instaladas executando este comando:</span><span class="sxs-lookup"><span data-stu-id="8a65b-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="8a65b-131">A saída fica assim (ela não informa qual versão das ferramentas que você está usando):</span><span class="sxs-lookup"><span data-stu-id="8a65b-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="8a65b-132">Usando as ferramentas</span><span class="sxs-lookup"><span data-stu-id="8a65b-132">Using the tools</span></span>

<span data-ttu-id="8a65b-133">Antes de usar as ferramentas:</span><span class="sxs-lookup"><span data-stu-id="8a65b-133">Before using the tools:</span></span>
* <span data-ttu-id="8a65b-134">Compreenda a diferença entre o projeto de inicialização e de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="8a65b-135">Saiba como usar as ferramentas com as bibliotecas de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a65b-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="8a65b-136">Para projetos ASP.NET Core, defina o ambiente.</span><span class="sxs-lookup"><span data-stu-id="8a65b-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="8a65b-137">Projeto de inicialização e de destino</span><span class="sxs-lookup"><span data-stu-id="8a65b-137">Target and startup project</span></span>

<span data-ttu-id="8a65b-138">Os comandos se referir a um *project* e uma *projeto de inicialização*.</span><span class="sxs-lookup"><span data-stu-id="8a65b-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="8a65b-139">O *project* também é conhecido como o *projeto de destino* porque é onde os comandos Adicionar ou remover arquivos.</span><span class="sxs-lookup"><span data-stu-id="8a65b-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="8a65b-140">Por padrão, o **projeto padrão** selecionado no **Package Manager Console** é o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="8a65b-141">Você pode especificar um projeto diferente como projeto de destino usando o <nobr> `--project` </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="8a65b-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="8a65b-142">O *projeto de inicialização* é aquele que as ferramentas de compilar e executar.</span><span class="sxs-lookup"><span data-stu-id="8a65b-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="8a65b-143">Tem as ferramentas executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de caracteres de conexão de banco de dados e a configuração do modelo.</span><span class="sxs-lookup"><span data-stu-id="8a65b-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="8a65b-144">Por padrão, o **projeto de inicialização** na **Gerenciador de soluções** é o projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a65b-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="8a65b-145">Você pode especificar um projeto diferente como projeto de inicialização usando o <nobr> `--startup-project` </nobr> opção.</span><span class="sxs-lookup"><span data-stu-id="8a65b-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="8a65b-146">O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="8a65b-147">Um cenário típico no qual eles são projetos separados é que quando:</span><span class="sxs-lookup"><span data-stu-id="8a65b-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="8a65b-148">As classes de entidade e de contexto do EF Core estão em uma biblioteca de classes do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a65b-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="8a65b-149">Um aplicativo de console .NET Core ou aplicativo web faz referência a biblioteca de classes.</span><span class="sxs-lookup"><span data-stu-id="8a65b-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="8a65b-150">Também é possível [coloque o código de migrações em uma biblioteca de classes separada do contexto do EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="8a65b-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="8a65b-151">Outras estruturas de destino</span><span class="sxs-lookup"><span data-stu-id="8a65b-151">Other target frameworks</span></span>

<span data-ttu-id="8a65b-152">As ferramentas do Console do Gerenciador de pacotes funcionam com projetos do .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8a65b-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="8a65b-153">Os aplicativos que têm o modelo do EF Core em uma biblioteca de classes .NET Standard não podem ter um projeto de .NET Framework ou .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a65b-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="8a65b-154">Por exemplo, isso é verdadeiro para aplicativos Xamarin e plataforma Universal do Windows.</span><span class="sxs-lookup"><span data-stu-id="8a65b-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="8a65b-155">Nesses casos, você pode criar um projeto de aplicativo de console .NET Core ou .NET Framework cuja única finalidade é atuar como projeto de inicialização para as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="8a65b-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="8a65b-156">O projeto pode ser um projeto fictício, sem nenhum código real &mdash; só é necessário fornecer um destino para as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="8a65b-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="8a65b-157">Por que é um projeto fictício necessário?</span><span class="sxs-lookup"><span data-stu-id="8a65b-157">Why is a dummy project required?</span></span> <span data-ttu-id="8a65b-158">Como mencionado anteriormente, as ferramentas de tem que executar o código do aplicativo em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="8a65b-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="8a65b-159">Para fazer isso, eles precisam usar o tempo de execução do .NET Core ou .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8a65b-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="8a65b-160">Quando o modelo do EF Core está em um projeto que tem como alvo o .NET Core ou .NET Framework, as ferramentas do EF Core emprestam o tempo de execução do projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="8a65b-161">E não é possível fazer isso se o modelo do EF Core está em uma biblioteca de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a65b-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="8a65b-162">O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs que devem dar suporte a implementações do .NET.</span><span class="sxs-lookup"><span data-stu-id="8a65b-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="8a65b-163">Portanto, .NET Standard não é suficiente para as ferramentas do EF Core executar o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8a65b-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="8a65b-164">O projeto fictício, que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar a biblioteca de classes .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8a65b-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="8a65b-165">Ambiente do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a65b-165">ASP.NET Core environment</span></span>

<span data-ttu-id="8a65b-166">Para especificar o ambiente para projetos do ASP.NET Core, defina **env:ASPNETCORE_ENVIRONMENT** antes de executar comandos.</span><span class="sxs-lookup"><span data-stu-id="8a65b-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="8a65b-167">Parâmetros comuns</span><span class="sxs-lookup"><span data-stu-id="8a65b-167">Common parameters</span></span>

<span data-ttu-id="8a65b-168">A tabela a seguir mostra os parâmetros que são comuns a todos os comandos do EF Core:</span><span class="sxs-lookup"><span data-stu-id="8a65b-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="8a65b-169">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-169">Parameter</span></span>                 | <span data-ttu-id="8a65b-170">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-171">-O contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-171">-Context \<String></span></span>        | <span data-ttu-id="8a65b-172">O `DbContext` classe a ser usada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-172">The `DbContext` class to use.</span></span> <span data-ttu-id="8a65b-173">Nome de classe totalmente qualificado com namespaces ou única.</span><span class="sxs-lookup"><span data-stu-id="8a65b-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="8a65b-174">Se esse parâmetro for omitido, o EF Core encontra a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="8a65b-175">Se houver várias classes de contexto, esse parâmetro é necessário.</span><span class="sxs-lookup"><span data-stu-id="8a65b-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="8a65b-176">-Projeto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-176">-Project \<String></span></span>        | <span data-ttu-id="8a65b-177">O projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-177">The target project.</span></span> <span data-ttu-id="8a65b-178">Se esse parâmetro for omitido, o **projeto padrão** para **Package Manager Console** é usado como o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="8a65b-179">O projeto de inicialização - \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-179">-StartupProject \<String></span></span> | <span data-ttu-id="8a65b-180">O projeto de inicialização.</span><span class="sxs-lookup"><span data-stu-id="8a65b-180">The startup project.</span></span> <span data-ttu-id="8a65b-181">Se esse parâmetro for omitido, o **projeto de inicialização** na **propriedades da solução** é usado como o projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="8a65b-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="8a65b-182">-Verbose</span></span>                  | <span data-ttu-id="8a65b-183">Mostra saída detalhada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="8a65b-184">Para mostrar informações de ajuda sobre um comando, use o PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="8a65b-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="8a65b-185">Os parâmetros de contexto, o projeto e o projeto de inicialização oferecem suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="8a65b-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="8a65b-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="8a65b-186">Add-Migration</span></span>

<span data-ttu-id="8a65b-187">Adiciona uma nova migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-187">Adds a new migration.</span></span>

<span data-ttu-id="8a65b-188">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8a65b-188">Parameters:</span></span>

| <span data-ttu-id="8a65b-189">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-189">Parameter</span></span>                         | <span data-ttu-id="8a65b-190">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-191"><nobr>-Nome \<cadeia de caracteres ><nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="8a65b-192">O nome da migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-192">The name of the migration.</span></span> <span data-ttu-id="8a65b-193">Isso é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="8a65b-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="8a65b-194"><nobr>-OutputDir \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="8a65b-195">O diretório (e sub-namespace) para usar.</span><span class="sxs-lookup"><span data-stu-id="8a65b-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="8a65b-196">Caminhos são relativos ao diretório de projeto de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="8a65b-197">O padrão é "Migrações".</span><span class="sxs-lookup"><span data-stu-id="8a65b-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="8a65b-198">Banco de dados de destino</span><span class="sxs-lookup"><span data-stu-id="8a65b-198">Drop-Database</span></span>

<span data-ttu-id="8a65b-199">Descarta o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8a65b-199">Drops the database.</span></span>

<span data-ttu-id="8a65b-200">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8a65b-200">Parameters:</span></span>

| <span data-ttu-id="8a65b-201">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-201">Parameter</span></span> | <span data-ttu-id="8a65b-202">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="8a65b-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="8a65b-203">-WhatIf</span></span>   | <span data-ttu-id="8a65b-204">Mostrar qual banco de dados deve ser descartado, mas não o remova.</span><span class="sxs-lookup"><span data-stu-id="8a65b-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="8a65b-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="8a65b-205">Get-DbContext</span></span>

<span data-ttu-id="8a65b-206">Listas disponíveis `DbContext` tipos.</span><span class="sxs-lookup"><span data-stu-id="8a65b-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="8a65b-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="8a65b-207">Remove-Migration</span></span>

<span data-ttu-id="8a65b-208">Remove a última migração (reverte as alterações de código que foram feitas para a migração).</span><span class="sxs-lookup"><span data-stu-id="8a65b-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="8a65b-209">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8a65b-209">Parameters:</span></span>

| <span data-ttu-id="8a65b-210">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-210">Parameter</span></span> | <span data-ttu-id="8a65b-211">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-212">-Force</span><span class="sxs-lookup"><span data-stu-id="8a65b-212">-Force</span></span>    | <span data-ttu-id="8a65b-213">Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados).</span><span class="sxs-lookup"><span data-stu-id="8a65b-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="8a65b-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="8a65b-214">Scaffold-DbContext</span></span>

<span data-ttu-id="8a65b-215">Gera código para um `DbContext` e tipos de entidade para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8a65b-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="8a65b-216">Para que `Scaffold-DbContext` para gerar um tipo de entidade, a tabela de banco de dados deve ter uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="8a65b-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="8a65b-217">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8a65b-217">Parameters:</span></span>

| <span data-ttu-id="8a65b-218">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-218">Parameter</span></span>                          | <span data-ttu-id="8a65b-219">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-220"><nobr>-Conexão \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="8a65b-221">A cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8a65b-221">The connection string to the database.</span></span> <span data-ttu-id="8a65b-222">Para projetos do ASP.NET Core 2.x, o valor pode ser *nome =\<nome da cadeia de caracteres de conexão >*.</span><span class="sxs-lookup"><span data-stu-id="8a65b-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="8a65b-223">Nesse caso, o nome vem de fontes de configuração que são configuradas para o projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="8a65b-224">Isso é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="8a65b-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="8a65b-225"><nobr>-Provedor \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="8a65b-226">O provedor a ser usado.</span><span class="sxs-lookup"><span data-stu-id="8a65b-226">The provider to use.</span></span> <span data-ttu-id="8a65b-227">Normalmente isso é o nome do pacote do NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="8a65b-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="8a65b-228">Isso é um parâmetro posicional e é necessário.</span><span class="sxs-lookup"><span data-stu-id="8a65b-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="8a65b-229">-OutputDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-229">-OutputDir \<String></span></span>               | <span data-ttu-id="8a65b-230">O diretório para colocar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="8a65b-230">The directory to put files in.</span></span> <span data-ttu-id="8a65b-231">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="8a65b-232">-ContextDir \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-232">-ContextDir \<String></span></span>              | <span data-ttu-id="8a65b-233">O diretório para colocar o `DbContext` de arquivo no.</span><span class="sxs-lookup"><span data-stu-id="8a65b-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="8a65b-234">Caminhos são relativos ao diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="8a65b-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="8a65b-235">-O contexto \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-235">-Context \<String></span></span>                 | <span data-ttu-id="8a65b-236">O nome da `DbContext` classe gerar.</span><span class="sxs-lookup"><span data-stu-id="8a65b-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="8a65b-237">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="8a65b-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="8a65b-238">Os esquemas de tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="8a65b-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="8a65b-239">Se esse parâmetro for omitido, todos os esquemas são incluídos.</span><span class="sxs-lookup"><span data-stu-id="8a65b-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="8a65b-240">-Tabelas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="8a65b-240">-Tables \<String[]></span></span>                | <span data-ttu-id="8a65b-241">As tabelas para gerar tipos de entidade para.</span><span class="sxs-lookup"><span data-stu-id="8a65b-241">The tables to generate entity types for.</span></span> <span data-ttu-id="8a65b-242">Se esse parâmetro for omitido, todas as tabelas são incluídas.</span><span class="sxs-lookup"><span data-stu-id="8a65b-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="8a65b-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="8a65b-243">-DataAnnotations</span></span>                   | <span data-ttu-id="8a65b-244">Use atributos para configurar o modelo (quando possível).</span><span class="sxs-lookup"><span data-stu-id="8a65b-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="8a65b-245">Se esse parâmetro for omitido, somente a API fluente é usada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="8a65b-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="8a65b-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="8a65b-247">Use nomes de tabela e coluna exatamente como aparecem no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8a65b-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="8a65b-248">Se esse parâmetro for omitido, os nomes de banco de dados são alterados de acordo com mais de perto para convenções de estilo de nome em C#.</span><span class="sxs-lookup"><span data-stu-id="8a65b-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="8a65b-249">-Force</span><span class="sxs-lookup"><span data-stu-id="8a65b-249">-Force</span></span>                             | <span data-ttu-id="8a65b-250">Substitua arquivos existentes.</span><span class="sxs-lookup"><span data-stu-id="8a65b-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="8a65b-251">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="8a65b-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="8a65b-252">Exemplo que usa o Scaffold somente tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:</span><span class="sxs-lookup"><span data-stu-id="8a65b-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="8a65b-253">Migração do script</span><span class="sxs-lookup"><span data-stu-id="8a65b-253">Script-Migration</span></span>

<span data-ttu-id="8a65b-254">Gera um script SQL que aplica todas as alterações de uma migração selecionada para outra migração selecionada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="8a65b-255">Parâmetros:</span><span class="sxs-lookup"><span data-stu-id="8a65b-255">Parameters:</span></span>

| <span data-ttu-id="8a65b-256">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-256">Parameter</span></span>                | <span data-ttu-id="8a65b-257">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-258">*-From* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-258">*-From* \<String></span></span>        | <span data-ttu-id="8a65b-259">A migração inicial.</span><span class="sxs-lookup"><span data-stu-id="8a65b-259">The starting migration.</span></span> <span data-ttu-id="8a65b-260">As migrações podem ser identificadas por nome ou ID.</span><span class="sxs-lookup"><span data-stu-id="8a65b-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8a65b-261">O número de 0 é um caso especial que significa *antes da primeira migração*.</span><span class="sxs-lookup"><span data-stu-id="8a65b-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="8a65b-262">O padrão é 0.</span><span class="sxs-lookup"><span data-stu-id="8a65b-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="8a65b-263">*-Para* \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-263">*-To* \<String></span></span>          | <span data-ttu-id="8a65b-264">A migração final.</span><span class="sxs-lookup"><span data-stu-id="8a65b-264">The ending migration.</span></span> <span data-ttu-id="8a65b-265">O padrão é para a última migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="8a65b-266"><nobr>-Idempotentes</nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="8a65b-267">Gere um script que pode ser usado em um banco de dados em qualquer migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="8a65b-268">-Saída \<cadeia de caracteres ></span><span class="sxs-lookup"><span data-stu-id="8a65b-268">-Output \<String></span></span>        | <span data-ttu-id="8a65b-269">O arquivo para gravar o resultado.</span><span class="sxs-lookup"><span data-stu-id="8a65b-269">The file to write the result to.</span></span> <span data-ttu-id="8a65b-270">Se esse parâmetro for omitido, o arquivo é criado com um nome gerado na mesma pasta, como arquivos de tempo de execução do aplicativo são criados, por exemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="8a65b-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="8a65b-271">Para, de, e os parâmetros de saída oferecem suporte a expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="8a65b-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="8a65b-272">O exemplo a seguir cria um script para a migração de InitialCreate, usando o nome da migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="8a65b-273">O exemplo a seguir cria um script para todas as migrações após a migração InitialCreate, usando a ID de migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="8a65b-274">Atualizar banco de dados</span><span class="sxs-lookup"><span data-stu-id="8a65b-274">Update-Database</span></span>

<span data-ttu-id="8a65b-275">Atualiza o banco de dados para a última migração ou para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="8a65b-276">Parâmetro</span><span class="sxs-lookup"><span data-stu-id="8a65b-276">Parameter</span></span>                           | <span data-ttu-id="8a65b-277">Descrição</span><span class="sxs-lookup"><span data-stu-id="8a65b-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a65b-278"><nobr>*-Migration* \<cadeia de caracteres ></nobr></span><span class="sxs-lookup"><span data-stu-id="8a65b-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="8a65b-279">A migração de destino.</span><span class="sxs-lookup"><span data-stu-id="8a65b-279">The target migration.</span></span> <span data-ttu-id="8a65b-280">As migrações podem ser identificadas por nome ou ID.</span><span class="sxs-lookup"><span data-stu-id="8a65b-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="8a65b-281">O número de 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações para ser revertido.</span><span class="sxs-lookup"><span data-stu-id="8a65b-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="8a65b-282">Se nenhuma migração for especificada, o comando assume como padrão para a última migração.</span><span class="sxs-lookup"><span data-stu-id="8a65b-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="8a65b-283">O parâmetro de migração dá suporte à expansão da tabulação.</span><span class="sxs-lookup"><span data-stu-id="8a65b-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="8a65b-284">O exemplo a seguir reverte todas as migrações.</span><span class="sxs-lookup"><span data-stu-id="8a65b-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="8a65b-285">Os exemplos a seguir atualiza o banco de dados para uma migração especificada.</span><span class="sxs-lookup"><span data-stu-id="8a65b-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="8a65b-286">O primeiro usa o nome da migração e o segundo usa a ID de migração:</span><span class="sxs-lookup"><span data-stu-id="8a65b-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="8a65b-287">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="8a65b-287">Additional resources</span></span>

* [<span data-ttu-id="8a65b-288">Migrações</span><span class="sxs-lookup"><span data-stu-id="8a65b-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="8a65b-289">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="8a65b-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
