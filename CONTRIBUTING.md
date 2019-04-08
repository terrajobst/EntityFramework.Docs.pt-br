---
ms.openlocfilehash: 79a2a10cae9f8a5541bca132e407d4abbe95e093
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914098"
---
# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="d74f0-101">Contribuindo para a documentação do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d74f0-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="d74f0-102">O processo de contribuir com artigos e exemplos de código para a documentação do Entity Framework é explicado abaixo.</span><span class="sxs-lookup"><span data-stu-id="d74f0-102">The process of contributing articles and code samples to the Entity Framework documentation is explained below.</span></span> <span data-ttu-id="d74f0-103">As contribuições podem ser tão simples quanto correções de erro de digitação ou tão complexas quanto novos artigos.</span><span class="sxs-lookup"><span data-stu-id="d74f0-103">Contributions can be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="d74f0-104">Como fazer uma correção ou sugestão simples</span><span class="sxs-lookup"><span data-stu-id="d74f0-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="d74f0-105">Artigos são armazenados neste repositório como arquivos de Markdown.</span><span class="sxs-lookup"><span data-stu-id="d74f0-105">Articles are stored as Markdown files in this repository.</span></span> <span data-ttu-id="d74f0-106">Para fazer uma alteração simples ao conteúdo de um arquivo Markdown, clique no link **Editar** no canto superior direito da janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="d74f0-106">To make a simple change to the content of a Markdown file, click the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="d74f0-107">Pode ser necessário expandir a barra **Opções** para ver o link **Editar**.</span><span class="sxs-lookup"><span data-stu-id="d74f0-107">You might need to expand the **options** bar to see the **Edit** link.</span></span> <span data-ttu-id="d74f0-108">Siga as instruções para criar uma PR (solicitação de pull).</span><span class="sxs-lookup"><span data-stu-id="d74f0-108">Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="d74f0-109">A equipe EF vai examinar a PR a aprová-la ou sugerir alterações.</span><span class="sxs-lookup"><span data-stu-id="d74f0-109">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="d74f0-110">Como fazer um envio mais complexo</span><span class="sxs-lookup"><span data-stu-id="d74f0-110">How to make a more complex submission</span></span>

<span data-ttu-id="d74f0-111">Você precisará de uma compreensão básica do [Git e do GitHub.com](https://guides.github.com/activities/hello-world/).</span><span class="sxs-lookup"><span data-stu-id="d74f0-111">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="d74f0-112">Abra um [problema](https://github.com/aspnet/EntityFramework.Docs/issues/new) descrevendo o que você deseja fazer, como alterar um artigo existente ou criar um novo.</span><span class="sxs-lookup"><span data-stu-id="d74f0-112">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="d74f0-113">Aguarde a aprovação da equipe EF antes de investir muito tempo.</span><span class="sxs-lookup"><span data-stu-id="d74f0-113">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="d74f0-114">Crie fork do repositório [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) e crie um branch para suas alterações.</span><span class="sxs-lookup"><span data-stu-id="d74f0-114">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="d74f0-115">Enviar uma PR (solicitação de pull) para mestre com suas alterações.</span><span class="sxs-lookup"><span data-stu-id="d74f0-115">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="d74f0-116">Responda aos comentários de PR.</span><span class="sxs-lookup"><span data-stu-id="d74f0-116">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="d74f0-117">Sintaxe de markdown</span><span class="sxs-lookup"><span data-stu-id="d74f0-117">Markdown syntax</span></span>

<span data-ttu-id="d74f0-118">Os artigos são escritos em [DFM (Markdown para DocFx)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), um superconjunto de [GFM (Markdown para GitHub)](https://guides.github.com/features/mastering-markdown/).</span><span class="sxs-lookup"><span data-stu-id="d74f0-118">Articles are written in [DocFx-flavored Markdown (DFM)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="d74f0-119">Para obter exemplos de sintaxe do DFM e metadados para recursos de interface do usuário comumente usados na documentação do EF, confira [Metadados e Modelo de Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) no guia de estilo do repositório do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d74f0-119">For examples of DFM syntax and metadata for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span>

## <a name="folder-structure-conventions"></a><span data-ttu-id="d74f0-120">Convenções de estrutura de pasta</span><span class="sxs-lookup"><span data-stu-id="d74f0-120">Folder structure conventions</span></span>

<span data-ttu-id="d74f0-121">Imagens e outros conteúdos estáticos são armazenados em uma pasta `_static` dentro de cada área/pasta do site.</span><span class="sxs-lookup"><span data-stu-id="d74f0-121">Images and other static content are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="d74f0-122">Exemplos de código são armazenados na pasta raiz do `samples`.</span><span class="sxs-lookup"><span data-stu-id="d74f0-122">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="d74f0-123">São organizados em uma estrutura de pastas que imita a estrutura de documentação (encontrado sob a pasta raiz `entity-framework`).</span><span class="sxs-lookup"><span data-stu-id="d74f0-123">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="d74f0-124">Snippets de código</span><span class="sxs-lookup"><span data-stu-id="d74f0-124">Code snippets</span></span>

<span data-ttu-id="d74f0-125">Artigos geralmente contêm snippets de código para ilustrar os pontos.</span><span class="sxs-lookup"><span data-stu-id="d74f0-125">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="d74f0-126">O DFM permite que você copie o código para o arquivo de Markdown ou veja um arquivo de código separado.</span><span class="sxs-lookup"><span data-stu-id="d74f0-126">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="d74f0-127">Preferimos usar arquivos de código separados sempre que possível para minimizar a chance de erros no código.</span><span class="sxs-lookup"><span data-stu-id="d74f0-127">Whenever possible, use separate code files to minimize the chance of errors in the code.</span></span> <span data-ttu-id="d74f0-128">Os arquivos de código devem ser armazenados no repositório usando a estrutura de pastas descrita acima para projetos de exemplo.</span><span class="sxs-lookup"><span data-stu-id="d74f0-128">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="d74f0-129">Aqui estão alguns exemplos de [sintaxe de snippet de código do DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span><span class="sxs-lookup"><span data-stu-id="d74f0-129">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="d74f0-130">Para renderizar um arquivo de código inteiro como um snippet:</span><span class="sxs-lookup"><span data-stu-id="d74f0-130">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="d74f0-131">Para renderizar uma parte de um arquivo como um snippet usando números de linha:</span><span class="sxs-lookup"><span data-stu-id="d74f0-131">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="d74f0-132">Para snippets C#, faça referência a uma [região do C#](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span><span class="sxs-lookup"><span data-stu-id="d74f0-132">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="d74f0-133">Use regiões em vez de números de linha.</span><span class="sxs-lookup"><span data-stu-id="d74f0-133">Use regions rather than line numbers.</span></span> <span data-ttu-id="d74f0-134">Números de linha em um arquivo de código tendem a sofrer alterações e sair de sincronia com as referências de número de linha em Markdown.</span><span class="sxs-lookup"><span data-stu-id="d74f0-134">Line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="d74f0-135">Regiões do C# podem ser aninhadas.</span><span class="sxs-lookup"><span data-stu-id="d74f0-135">C# regions can be nested.</span></span> <span data-ttu-id="d74f0-136">Se você fizer referência à região externa, as diretivas internas `#region` e `#endregion` não serão renderizadas em um snippet.</span><span class="sxs-lookup"><span data-stu-id="d74f0-136">If you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="d74f0-137">Para renderizar uma região C# chamada "snippet_Example":</span><span class="sxs-lookup"><span data-stu-id="d74f0-137">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="d74f0-138">Para realçar as linhas selecionadas em um snippet renderizado (geralmente é renderizado como a cor da tela de fundo amarela):</span><span class="sxs-lookup"><span data-stu-id="d74f0-138">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="d74f0-139">Testar alterações com o DocFX</span><span class="sxs-lookup"><span data-stu-id="d74f0-139">Test your changes with DocFX</span></span>

<span data-ttu-id="d74f0-140">Teste suas alterações com a [ferramenta de linha de comando do DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), que cria uma versão hospedada localmente do site.</span><span class="sxs-lookup"><span data-stu-id="d74f0-140">Test your changes with the [DocFX command-line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="d74f0-141">O DocFX não renderiza extensões de site e estilo criadas para docs.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="d74f0-141">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="d74f0-142">O DocFX requer o .NET Framework no Windows ou no Mono (para Linux ou macOS).</span><span class="sxs-lookup"><span data-stu-id="d74f0-142">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="d74f0-143">Instruções do Windows</span><span class="sxs-lookup"><span data-stu-id="d74f0-143">Windows instructions</span></span>

* <span data-ttu-id="d74f0-144">Baixe e descompacte *docfx.zip* de [versões do DocFX](https://github.com/dotnet/docfx/releases).</span><span class="sxs-lookup"><span data-stu-id="d74f0-144">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="d74f0-145">Adicione DocFX ao seu PATH.</span><span class="sxs-lookup"><span data-stu-id="d74f0-145">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="d74f0-146">Em uma janela de linha de comando, navegue até o repositório clonado (que contém o arquivo *docfx.json*) e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d74f0-146">In a command-line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="d74f0-147">Em um navegador, navegue até `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d74f0-147">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="d74f0-148">Instruções do Mono</span><span class="sxs-lookup"><span data-stu-id="d74f0-148">Mono instructions</span></span>

* <span data-ttu-id="d74f0-149">Instalar o Mono por meio do Homebrew: `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="d74f0-149">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="d74f0-150">Baixe a [versão mais recente do DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span><span class="sxs-lookup"><span data-stu-id="d74f0-150">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="d74f0-151">Extrair para `\bin\docfx`.</span><span class="sxs-lookup"><span data-stu-id="d74f0-151">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="d74f0-152">Criar um alias para **docfx**:</span><span class="sxs-lookup"><span data-stu-id="d74f0-152">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="d74f0-153">Execute **docfx** no repositório clonado para criar o site e **docfx-serve** para exibir o site em `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d74f0-153">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="d74f0-154">Voz e tom</span><span class="sxs-lookup"><span data-stu-id="d74f0-154">Voice and tone</span></span>

<span data-ttu-id="d74f0-155">Nossa meta é escrever uma documentação que possa ser facilmente compreendida pelo maior público possível.</span><span class="sxs-lookup"><span data-stu-id="d74f0-155">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="d74f0-156">Para esse fim, estabelecemos diretrizes para estilo de escrita que pedimos que nossos colaboradores sigam.</span><span class="sxs-lookup"><span data-stu-id="d74f0-156">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="d74f0-157">Para obter mais informações, confira [Diretrizes de voz e tom](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) no repositório do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d74f0-157">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
