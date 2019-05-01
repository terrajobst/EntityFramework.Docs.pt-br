---
ms.openlocfilehash: 79a2a10cae9f8a5541bca132e407d4abbe95e093
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929882"
---
# <a name="contributing-to-the-entity-framework-documentation"></a>Contribuindo para a documentação do Entity Framework

O processo de contribuir com artigos e exemplos de código para a documentação do Entity Framework é explicado abaixo. As contribuições podem ser tão simples quanto correções de erro de digitação ou tão complexas quanto novos artigos.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>Como fazer uma correção ou sugestão simples

Artigos são armazenados neste repositório como arquivos de Markdown. Para fazer uma alteração simples ao conteúdo de um arquivo Markdown, clique no link **Editar** no canto superior direito da janela do navegador. Pode ser necessário expandir a barra **Opções** para ver o link **Editar**. Siga as instruções para criar uma PR (solicitação de pull). A equipe EF vai examinar a PR a aprová-la ou sugerir alterações.

## <a name="how-to-make-a-more-complex-submission"></a>Como fazer um envio mais complexo

Você precisará de uma compreensão básica do [Git e do GitHub.com](https://guides.github.com/activities/hello-world/).

* Abra um [problema](https://github.com/aspnet/EntityFramework.Docs/issues/new) descrevendo o que você deseja fazer, como alterar um artigo existente ou criar um novo. Aguarde a aprovação da equipe EF antes de investir muito tempo.
* Crie fork do repositório [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) e crie um branch para suas alterações.
* Enviar uma PR (solicitação de pull) para mestre com suas alterações.
* Responda aos comentários de PR.

## <a name="markdown-syntax"></a>Sintaxe de markdown

Os artigos são escritos em [DFM (Markdown para DocFx)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), um superconjunto de [GFM (Markdown para GitHub)](https://guides.github.com/features/mastering-markdown/). Para obter exemplos de sintaxe do DFM e metadados para recursos de interface do usuário comumente usados na documentação do EF, confira [Metadados e Modelo de Markdown](https://github.com/dotnet/docs/blob/master/styleguide/template.md) no guia de estilo do repositório do .NET Core.

## <a name="folder-structure-conventions"></a>Convenções de estrutura de pasta

Imagens e outros conteúdos estáticos são armazenados em uma pasta `_static` dentro de cada área/pasta do site.

Exemplos de código são armazenados na pasta raiz do `samples`. São organizados em uma estrutura de pastas que imita a estrutura de documentação (encontrado sob a pasta raiz `entity-framework`).

## <a name="code-snippets"></a>Snippets de código

Artigos geralmente contêm snippets de código para ilustrar os pontos. O DFM permite que você copie o código para o arquivo de Markdown ou veja um arquivo de código separado. Preferimos usar arquivos de código separados sempre que possível para minimizar a chance de erros no código. Os arquivos de código devem ser armazenados no repositório usando a estrutura de pastas descrita acima para projetos de exemplo.

Aqui estão alguns exemplos de [sintaxe de snippet de código do DFM](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).

Para renderizar um arquivo de código inteiro como um snippet:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

Para renderizar uma parte de um arquivo como um snippet usando números de linha:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

Para snippets C#, faça referência a uma [região do C#](https://msdn.microsoft.com/library/9a1ybwek.aspx). Use regiões em vez de números de linha. Números de linha em um arquivo de código tendem a sofrer alterações e sair de sincronia com as referências de número de linha em Markdown. Regiões do C# podem ser aninhadas. Se você fizer referência à região externa, as diretivas internas `#region` e `#endregion` não serão renderizadas em um snippet.

Para renderizar uma região C# chamada "snippet_Example":

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

Para realçar as linhas selecionadas em um snippet renderizado (geralmente é renderizado como a cor da tela de fundo amarela):

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a>Testar alterações com o DocFX

Teste suas alterações com a [ferramenta de linha de comando do DocFX](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), que cria uma versão hospedada localmente do site. O DocFX não renderiza extensões de site e estilo criadas para docs.microsoft.com.

O DocFX requer o .NET Framework no Windows ou no Mono (para Linux ou macOS).

### <a name="windows-instructions"></a>Instruções do Windows

* Baixe e descompacte *docfx.zip* de [versões do DocFX](https://github.com/dotnet/docfx/releases).
* Adicione DocFX ao seu PATH.
* Em uma janela de linha de comando, navegue até o repositório clonado (que contém o arquivo *docfx.json*) e execute o seguinte comando:

   ``` console
   docfx -t default --serve
   ```

* Em um navegador, navegue até `http://localhost:8080`.

### <a name="mono-instructions"></a>Instruções do Mono

* Instalar o Mono por meio do Homebrew: `brew install mono`.
* Baixe a [versão mais recente do DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).
* Extrair para `\bin\docfx`.
* Criar um alias para **docfx**:

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* Execute **docfx** no repositório clonado para criar o site e **docfx-serve** para exibir o site em `http://localhost:8080`.

## <a name="voice-and-tone"></a>Voz e tom

Nossa meta é escrever uma documentação que possa ser facilmente compreendida pelo maior público possível. Para esse fim, estabelecemos diretrizes para estilo de escrita que pedimos que nossos colaboradores sigam. Para obter mais informações, confira [Diretrizes de voz e tom](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) no repositório do .NET Core.
