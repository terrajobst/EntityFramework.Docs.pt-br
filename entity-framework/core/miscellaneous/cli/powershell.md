---
title: Referência de ferramentas de EF Core (console do Gerenciador de pacotes)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181340"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Referência de ferramentas de Entity Framework Core – console do Gerenciador de pacotes no Visual Studio

As ferramentas do console do Gerenciador de pacotes (PMC) para Entity Framework Core executam tarefas de desenvolvimento em tempo de design. Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplicam migrações e geram código para um modelo com base em um banco de dados existente. Os comandos são executados dentro do Visual Studio usando o [console do Gerenciador de pacotes](/nuget/tools/package-manager-console). Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.

Se você não estiver usando o Visual Studio, recomendamos o [EF Core ferramentas de linha de comando](dotnet.md) . As ferramentas da CLI são de plataforma cruzada e executadas dentro de um prompt de comando.

## <a name="installing-the-tools"></a>Instalando as ferramentas

Os procedimentos para instalar e atualizar as ferramentas diferem entre ASP.NET Core 2.1 + e versões anteriores ou outros tipos de projeto.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core versão 2,1 e posterior

As ferramentas são incluídas automaticamente em um projeto ASP.NET Core 2.1 + porque o pacote `Microsoft.EntityFrameworkCore.Tools` está incluído no [metapacote Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).

Portanto, você não precisa fazer nada para instalar as ferramentas, mas é necessário:
* Restaure os pacotes antes de usar as ferramentas em um novo projeto.
* Instale um pacote para atualizar as ferramentas para uma versão mais recente.

Para certificar-se de que você está obtendo a versão mais recente das ferramentas, recomendamos que você também faça a seguinte etapa:

* Edite seu arquivo *. csproj* e adicione uma linha especificando a versão mais recente do pacote [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) . Por exemplo, o arquivo *. csproj* pode incluir um `ItemGroup` semelhante a este:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Atualize as ferramentas quando receber uma mensagem semelhante ao exemplo a seguir:

> A versão de ferramentas de EF Core ' 2.1.1-RTM-30846 ' é mais antiga que a do tempo de execução ' 2.1.3-RTM-32065 '. Atualize as ferramentas para os recursos e correções de bug mais recentes.

Para atualizar as ferramentas:
* Instale o SDK do .NET Core mais recente.
* Atualize o Visual Studio para a versão mais recente.
* Edite o arquivo *. csproj* para que ele inclua uma referência de pacote para o pacote de ferramentas mais recente, conforme mostrado anteriormente.

### <a name="other-versions-and-project-types"></a>Outras versões e tipos de projeto

Instale as ferramentas do console do Gerenciador de pacotes executando o seguinte comando no **console do Gerenciador de pacotes**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Atualize as ferramentas executando o comando a seguir no **console do Gerenciador de pacotes**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Verificar a instalação

Verifique se as ferramentas estão instaladas executando este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```

A saída é parecida com esta (não informa qual versão das ferramentas você está usando):

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

## <a name="using-the-tools"></a>Usando as ferramentas

Antes de usar as ferramentas:
* Entenda a diferença entre o destino e o projeto de inicialização.
* Saiba como usar as ferramentas com .NET Standard bibliotecas de classe.
* Para projetos ASP.NET Core, defina o ambiente.

### <a name="target-and-startup-project"></a>Projeto de destino e inicialização

Os comandos referem-se a um *projeto* e um *projeto de inicialização*.

* O *projeto* também é conhecido como o *projeto de destino* porque é onde os comandos adicionam ou removem arquivos. Por padrão, o **projeto padrão** selecionado no **console do Gerenciador de pacotes** é o projeto de destino. Você pode especificar um projeto diferente como projeto de destino usando a opção <nobr>`--project`</nobr> .

* O *projeto de inicialização* é aquele que as ferramentas criam e executam. As ferramentas precisam executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de conexão do banco de dados e a configuração do modelo. Por padrão, o **projeto de inicialização** no **Gerenciador de soluções** é o projeto de inicialização. Você pode especificar um projeto diferente como projeto de inicialização usando a opção <nobr>`--startup-project`</nobr> .

O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto. Um cenário típico em que são projetos separados é quando:

* O contexto de EF Core e as classes de entidade estão em uma biblioteca de classes do .NET Core.
* Um aplicativo de console do .NET Core ou aplicativo Web faz referência à biblioteca de classes.

Também é possível [colocar o código de migrações em uma biblioteca de classes separada do contexto de EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Outras estruturas de destino

As ferramentas de console do Gerenciador de pacotes funcionam com projetos .NET Core ou .NET Framework. Os aplicativos que têm o modelo de EF Core em uma biblioteca de classes de .NET Standard podem não ter um projeto do .NET Core ou do .NET Framework. Por exemplo, isso é verdadeiro para aplicativos Xamarin e Plataforma Universal do Windows. Nesses casos, você pode criar um projeto de aplicativo de console do .NET Core ou .NET Framework, cujo único propósito é agir como projeto de inicialização para as ferramentas. O projeto pode ser um projeto fictício sem código real &mdash; é necessário apenas para fornecer um destino para as ferramentas.

Por que um projeto fictício é necessário? Conforme mencionado anteriormente, as ferramentas precisam executar o código do aplicativo em tempo de design. Para fazer isso, eles precisam usar o .NET Core ou o .NET Framework Runtime. Quando o modelo de EF Core está em um projeto direcionado ao .NET Core ou .NET Framework, as ferramentas de EF Core emprestam o tempo de execução do projeto. Eles não poderão fazer isso se o modelo de EF Core estiver em uma biblioteca de classes .NET Standard. O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs para as quais as implementações do .NET devem dar suporte. Portanto .NET Standard não é suficiente para as ferramentas de EF Core executarem o código do aplicativo. O projeto fictício que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar o .NET Standard biblioteca de classes.

### <a name="aspnet-core-environment"></a>Ambiente de ASP.NET Core

Para especificar o ambiente para projetos de ASP.NET Core, defina **env: ASPNETCORE_ENVIRONMENT** antes de executar os comandos.

## <a name="common-parameters"></a>Parâmetros comuns

A tabela a seguir mostra os parâmetros que são comuns a todos os comandos de EF Core:

| Parâmetro                 | Descrição                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -@No__t de contexto-0String >        | A classe `DbContext` a ser usada. Nome da classe somente ou totalmente qualificado com namespaces.  Se esse parâmetro for omitido, EF Core encontrará a classe de contexto. Se houver várias classes de contexto, esse parâmetro será necessário. |
| -Project \<String >        | O projeto de destino. Se esse parâmetro for omitido, o **projeto padrão** do **console do Gerenciador de pacotes** será usado como o projeto de destino.                                                                             |
| -StartupProject \<String > | O projeto de inicialização. Se esse parâmetro for omitido, o **projeto de inicialização** nas propriedades da **solução** será usado como o projeto de destino.                                                                                 |
| -Verbose                  | Mostrar saída detalhada.                                                                                                                                                                                                 |

Para mostrar informações de ajuda sobre um comando, use o comando `Get-Help` do PowerShell.

> [!TIP]
> Os parâmetros Context, Project e StartupProject dão suporte à expansão de tabulação.

## <a name="add-migration"></a>Adicionar e migrar

Adiciona uma nova migração.

Parâmetros:

| Parâmetro                         | Descrição                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr> \<String > <nobr>       | O nome da migração. Esse é um parâmetro posicional e é necessário.                                              |
| <nobr>-OutputDir \<String ></nobr> | O diretório (e o subnamespace) a ser usado. Os caminhos são relativos ao diretório do projeto de destino. O padrão é "migrações". |

## <a name="drop-database"></a>Remover banco de dados

Remove o banco de dados.

Parâmetros:

| Parâmetro | Descrição                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Mostrar qual banco de dados seria descartado, mas não descartá-lo. |

## <a name="get-dbcontext"></a>Get-DbContext

Obtém informações sobre um tipo `DbContext`.

## <a name="remove-migration"></a>Remove-Migration

Remove a última migração (reverte as alterações de código que foram feitas para a migração).

Parâmetros:

| Parâmetro | Descrição                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Gera código para um `DbContext` e tipos de entidade para um banco de dados. Para que `Scaffold-DbContext` gere um tipo de entidade, a tabela de banco de dados deve ter uma chave primária.

Parâmetros:

| Parâmetro                          | Descrição                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-@No__t de conexão-1String ></nobr> | A cadeia de conexão para o banco de dados. Para projetos ASP.NET Core 2. x, o valor pode ser *Name = \<Name da cadeia de conexão >* . Nesse caso, o nome é proveniente das fontes de configuração que estão configuradas para o projeto. Esse é um parâmetro posicional e é necessário. |
| <nobr>-Provedor \<String ></nobr>   | O provedor a ser usado. Normalmente, esse é o nome do pacote NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`. Esse é um parâmetro posicional e é necessário.                                                                                           |
| -OutputDir \<String >               | O diretório no qual colocar arquivos. Os caminhos são relativos ao diretório do projeto.                                                                                                                                                                                             |
| -ContextDir \<String >              | O diretório no qual colocar o arquivo `DbContext`. Os caminhos são relativos ao diretório do projeto.                                                                                                                                                                              |
| -@No__t de contexto-0String >                 | O nome da classe `DbContext` a ser gerada.                                                                                                                                                                                                                          |
| -Schemas \<String [] >               | Os esquemas de tabelas para os quais gerar tipos de entidade. Se esse parâmetro for omitido, todos os esquemas serão incluídos.                                                                                                                                                             |
| -Tables \<String [] >                | As tabelas para as quais gerar tipos de entidade. Se esse parâmetro for omitido, todas as tabelas serão incluídas.                                                                                                                                                                         |
| -DataAnnotations                   | Use atributos para configurar o modelo (sempre que possível). Se esse parâmetro for omitido, somente a API fluente será usada.                                                                                                                                                      |
| -UseDatabaseNames                  | Use nomes de tabela e coluna exatamente como aparecem no banco de dados. Se esse parâmetro for omitido, os nomes de banco de dados serão alterados C# para se adequar melhor às convenções de estilo de nome.                                                                                       |
| -Force                             | Substitui os arquivos existentes.                                                                                                                                                                                                                                               |

Exemplo:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Exemplo que aplica Scaffold apenas tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Script – migração

Gera um script SQL que aplica todas as alterações de uma migração selecionada para outra migração selecionada.

Parâmetros:

| Parâmetro                | Descrição                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-De* \<String >        | A migração inicial. As migrações podem ser identificadas por nome ou por ID. O número 0 é um caso especial que significa *antes da primeira migração*. O padrão é 0.                                                              |
| *-Para* \<String >          | A migração final. O padrão é a última migração.                                                                                                                                                                      |
| <nobr>-Idempotent</nobr> | Gere um script que possa ser usado em um banco de dados em qualquer migração.                                                                                                                                                         |
| -Output \<String >        | O arquivo no qual o resultado será gravado. Se esse parâmetro for omitido, o arquivo será criado com um nome gerado na mesma pasta que os arquivos de tempo de execução do aplicativo são criados, por exemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* . |

> [!TIP]
> Os parâmetros para, de e saída dão suporte à expansão de tabulação.

O exemplo a seguir cria um script para a migração InitialCreate, usando o nome de migração.

```powershell
Script-Migration -To InitialCreate
```

O exemplo a seguir cria um script para todas as migrações após a migração de InitialCreate, usando a ID de migração.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Atualizar banco de dados

Atualiza o banco de dados para a última migração ou para uma migração especificada.

| Parâmetro                           | Descrição                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr> *-Migração* \<String ></nobr> | A migração de destino. As migrações podem ser identificadas por nome ou por ID. O número 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações sejam revertidas. Se nenhuma migração for especificada, o comando usa como padrão a última migração. |

> [!TIP]
> O parâmetro de migração oferece suporte à expansão de tabulação.

O exemplo a seguir reverte todas as migrações.

```powershell
Update-Database -Migration 0
```

Os exemplos a seguir atualizam o banco de dados para uma migração especificada. O primeiro usa o nome de migração e o segundo usa a ID de migração:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Recursos adicionais

* [Migrações](xref:core/managing-schemas/migrations/index)
* [Engenharia reversa](xref:core/managing-schemas/scaffolding)
