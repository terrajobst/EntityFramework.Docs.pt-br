---
title: EF Core ferramentas reference (Package Manager Console) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419764"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Referência - Package Manager Console no Visual Studio das ferramentas do Entity Framework Core

As ferramentas do Console de Gerenciador de pacote (PMC) para o Entity Framework Core realizar tarefas de desenvolvimento de tempo de design. Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar as migrações e gerar código para um modelo com base em um banco de dados existente. Os comandos são executados dentro do Visual Studio usando o [Package Manager Console](/nuget/tools/package-manager-console). Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.

Se você não estiver usando o Visual Studio, é recomendável o [ferramentas de linha de comando do EF Core](dotnet.md) em vez disso. As ferramentas da CLI são multiplataforma e execução dentro de um prompt de comando.

## <a name="installing-the-tools"></a>Instalando as ferramentas

Os procedimentos para instalar e atualizar as ferramentas são diferentes entre o ASP.NET Core 2.1 + e versões anteriores ou outros tipos de projeto.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core versão 2.1 e posterior

As ferramentas são incluídas automaticamente em um projeto ASP.NET Core 2.1 + porque o `Microsoft.EntityFrameworkCore.Tools` pacote está incluído na [metapacote Microsoft](/aspnet/core/fundamentals/metapackage-app).

Portanto, você não precisa fazer nada para instalar as ferramentas, mas você precisa:
* Restaure os pacotes antes de usar as ferramentas em um novo projeto.
* Instale um pacote para atualizar as ferramentas para uma versão mais recente.

Para certificar-se de que você obtenha a versão mais recente das ferramentas, é recomendável que você também pode fazer a seguinte etapa:

* Editar seu *. csproj* arquivo e adicione uma linha especificando a versão mais recente do [entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pacote. Por exemplo, o *. csproj* arquivo pode incluir um `ItemGroup` parecido com este:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Atualize as ferramentas quando você receber uma mensagem semelhante ao seguinte exemplo:

> A versão das ferramentas do EF Core '2.1.1-rtm-30846' é mais antiga do que o tempo de execução '2.1.3-rtm-32065'. Atualize as ferramentas para os últimos recursos e correções de bugs.

Para atualizar as ferramentas:
* Instale o SDK do .NET Core mais recentes.
* Atualize o Visual Studio para a versão mais recente.
* Editar o *. csproj* de arquivo para que ele inclua uma referência de pacote para o pacote de ferramentas mais recente, conforme mostrado anteriormente.

### <a name="other-versions-and-project-types"></a>Outras versões e tipos de projeto

Instalar as ferramentas do Console do Gerenciador de pacotes, executando o seguinte comando em **Package Manager Console**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Atualizar as ferramentas, executando o seguinte comando em **Package Manager Console**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Verificar a instalação

Verifique se que as ferramentas são instaladas executando este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```

A saída fica assim (ela não informa qual versão das ferramentas que você está usando):

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
* Compreenda a diferença entre o projeto de inicialização e de destino.
* Saiba como usar as ferramentas com as bibliotecas de classes .NET Standard.
* Para projetos ASP.NET Core, defina o ambiente.

### <a name="target-and-startup-project"></a>Projeto de inicialização e de destino

Os comandos se referir a um *project* e uma *projeto de inicialização*.

* O *project* também é conhecido como o *projeto de destino* porque é onde os comandos Adicionar ou remover arquivos. Por padrão, o **projeto padrão** selecionado no **Package Manager Console** é o projeto de destino. Você pode especificar um projeto diferente como projeto de destino usando o <nobr> `--project` </nobr> opção.

* O *projeto de inicialização* é aquele que as ferramentas de compilar e executar. Tem as ferramentas executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de caracteres de conexão de banco de dados e a configuração do modelo. Por padrão, o **projeto de inicialização** na **Gerenciador de soluções** é o projeto de inicialização. Você pode especificar um projeto diferente como projeto de inicialização usando o <nobr> `--startup-project` </nobr> opção.

O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto. Um cenário típico no qual eles são projetos separados é que quando:

* As classes de entidade e de contexto do EF Core estão em uma biblioteca de classes do .NET Core.
* Um aplicativo de console .NET Core ou aplicativo web faz referência a biblioteca de classes.

Também é possível [coloque o código de migrações em uma biblioteca de classes separada do contexto do EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Outras estruturas de destino

As ferramentas do Console do Gerenciador de pacotes funcionam com projetos do .NET Core ou .NET Framework. Os aplicativos que têm o modelo do EF Core em uma biblioteca de classes .NET Standard não podem ter um projeto de .NET Framework ou .NET Core. Por exemplo, isso é verdadeiro para aplicativos Xamarin e plataforma Universal do Windows. Nesses casos, você pode criar um projeto de aplicativo de console .NET Core ou .NET Framework cuja única finalidade é atuar como projeto de inicialização para as ferramentas. O projeto pode ser um projeto fictício, sem nenhum código real &mdash; só é necessário fornecer um destino para as ferramentas.

Por que é um projeto fictício necessário? Como mencionado anteriormente, as ferramentas de tem que executar o código do aplicativo em tempo de design. Para fazer isso, eles precisam usar o tempo de execução do .NET Core ou .NET Framework. Quando o modelo do EF Core está em um projeto que tem como alvo o .NET Core ou .NET Framework, as ferramentas do EF Core emprestam o tempo de execução do projeto. E não é possível fazer isso se o modelo do EF Core está em uma biblioteca de classes .NET Standard. O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs que devem dar suporte a implementações do .NET. Portanto, .NET Standard não é suficiente para as ferramentas do EF Core executar o código do aplicativo. O projeto fictício, que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar a biblioteca de classes .NET Standard.

### <a name="aspnet-core-environment"></a>Ambiente do ASP.NET Core

Para especificar o ambiente para projetos do ASP.NET Core, defina **env:ASPNETCORE_ENVIRONMENT** antes de executar comandos.

## <a name="common-parameters"></a>Parâmetros comuns

A tabela a seguir mostra os parâmetros que são comuns a todos os comandos do EF Core:

| Parâmetro                 | Descrição                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -O contexto \<cadeia de caracteres >        | A classe `DbContext` a ser usada. Nome de classe totalmente qualificado com namespaces ou única.  Se esse parâmetro for omitido, o EF Core encontra a classe de contexto. Se houver várias classes de contexto, esse parâmetro é necessário. |
| -Project \<String>        | O projeto de destino. Se esse parâmetro for omitido, o **projeto padrão** para **Package Manager Console** é usado como o projeto de destino.                                                                             |
| O projeto de inicialização - \<cadeia de caracteres > | O projeto de inicialização. Se esse parâmetro for omitido, o **projeto de inicialização** na **propriedades da solução** é usado como o projeto de destino.                                                                                 |
| -Verbose                  | Mostra saída detalhada.                                                                                                                                                                                                 |

Para mostrar informações de ajuda sobre um comando, use o PowerShell `Get-Help` comando.

> [!TIP]
> Os parâmetros de contexto, o projeto e o projeto de inicialização oferecem suporte a expansão da tabulação.

## <a name="add-migration"></a>Add-Migration

Adiciona uma nova migração.

Parâmetros:

| Parâmetro                         | Descrição                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Name \<String><nobr>       | O nome da migração. Isso é um parâmetro posicional e é necessário.                                              |
| <nobr>-OutputDir \<String></nobr> | O diretório (e sub-namespace) para usar. Caminhos são relativos ao diretório de projeto de destino. O padrão é "Migrações". |

## <a name="drop-database"></a>Banco de dados de destino

Descarta o banco de dados.

Parâmetros:

| Parâmetro | Descrição                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Mostrar qual banco de dados deve ser descartado, mas não o remova. |

## <a name="get-dbcontext"></a>Get-DbContext

Obtém informações sobre um `DbContext` tipo.

## <a name="remove-migration"></a>Remove-Migration

Remove a última migração (reverte as alterações de código que foram feitas para a migração).

Parâmetros:

| Parâmetro | Descrição                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Gera código para um `DbContext` e tipos de entidade para um banco de dados. Para que `Scaffold-DbContext` para gerar um tipo de entidade, a tabela de banco de dados deve ter uma chave primária.

Parâmetros:

| Parâmetro                          | Descrição                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Conexão \<cadeia de caracteres ></nobr> | A cadeia de conexão para o banco de dados. Para projetos do ASP.NET Core 2.x, o valor pode ser *nome =\<nome da cadeia de caracteres de conexão >*. Nesse caso, o nome vem de fontes de configuração que são configuradas para o projeto. Isso é um parâmetro posicional e é necessário. |
| <nobr>-Provedor \<cadeia de caracteres ></nobr>   | O provedor a ser usado. Normalmente isso é o nome do pacote do NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`. Isso é um parâmetro posicional e é necessário.                                                                                           |
| -OutputDir \<cadeia de caracteres >               | O diretório para colocar os arquivos. Caminhos são relativos ao diretório do projeto.                                                                                                                                                                                             |
| -ContextDir \<cadeia de caracteres >              | O diretório para colocar o `DbContext` de arquivo no. Caminhos são relativos ao diretório do projeto.                                                                                                                                                                              |
| -O contexto \<cadeia de caracteres >                 | O nome da `DbContext` classe gerar.                                                                                                                                                                                                                          |
| -Schemas \<String[]>               | Os esquemas de tabelas para gerar tipos de entidade para. Se esse parâmetro for omitido, todos os esquemas são incluídos.                                                                                                                                                             |
| -Tabelas \<String [] >                | As tabelas para gerar tipos de entidade para. Se esse parâmetro for omitido, todas as tabelas são incluídas.                                                                                                                                                                         |
| -DataAnnotations                   | Use atributos para configurar o modelo (quando possível). Se esse parâmetro for omitido, somente a API fluente é usada.                                                                                                                                                      |
| -UseDatabaseNames                  | Use nomes de tabela e coluna exatamente como aparecem no banco de dados. Se esse parâmetro for omitido, os nomes de banco de dados são alterados de acordo com mais de perto para convenções de estilo de nome em C#.                                                                                       |
| -Force                             | Substitua arquivos existentes.                                                                                                                                                                                                                                               |

Exemplo:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Exemplo que usa o Scaffold somente tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Migração do script

Gera um script SQL que aplica todas as alterações de uma migração selecionada para outra migração selecionada.

Parâmetros:

| Parâmetro                | Descrição                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-From* \<String>        | A migração inicial. As migrações podem ser identificadas por nome ou ID. O número de 0 é um caso especial que significa *antes da primeira migração*. O padrão é 0.                                                              |
| *-To* \<String>          | A migração final. O padrão é para a última migração.                                                                                                                                                                      |
| <nobr>-Idempotent</nobr> | Gere um script que pode ser usado em um banco de dados em qualquer migração.                                                                                                                                                         |
| -Saída \<cadeia de caracteres >        | O arquivo para gravar o resultado. Se esse parâmetro for omitido, o arquivo é criado com um nome gerado na mesma pasta, como arquivos de tempo de execução do aplicativo são criados, por exemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> Para, de, e os parâmetros de saída oferecem suporte a expansão da tabulação.

O exemplo a seguir cria um script para a migração de InitialCreate, usando o nome da migração.

```powershell
Script-Migration -To InitialCreate
```

O exemplo a seguir cria um script para todas as migrações após a migração InitialCreate, usando a ID de migração.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Atualizar banco de dados

Atualiza o banco de dados para a última migração ou para uma migração especificada.

| Parâmetro                           | Descrição                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<String></nobr> | A migração de destino. As migrações podem ser identificadas por nome ou ID. O número de 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações para ser revertido. Se nenhuma migração for especificada, o comando assume como padrão para a última migração. |

> [!TIP]
> O parâmetro de migração dá suporte à expansão da tabulação.

O exemplo a seguir reverte todas as migrações.

```powershell
Update-Database -Migration 0
```

Os exemplos a seguir atualiza o banco de dados para uma migração especificada. O primeiro usa o nome da migração e o segundo usa a ID de migração:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Recursos adicionais

* [Migrações](xref:core/managing-schemas/migrations/index)
* [Engenharia reversa](xref:core/managing-schemas/scaffolding)
