---
title: EF Core ferramentas reference (CLI) do .NET - Core EF
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 05c5f89fc79556e72a7e629c147aa817fe7d1a6b
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286463"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referência de ferramentas do Entity Framework Core - CLI do .NET

As ferramentas de interface de linha de comando (CLI) para o Entity Framework Core realizar tarefas de desenvolvimento de tempo de design. Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar as migrações e gerar código para um modelo com base em um banco de dados existente. Os comandos são uma extensão para a plataforma cruzada [dotnet](/dotnet/core/tools) comando, que é parte do [SDK do .NET Core](https://www.microsoft.com/net/core). Essas ferramentas funcionam com projetos do .NET Core.

Se você estiver usando o Visual Studio, é recomendável o [ferramentas do Console do Gerenciador de pacotes](powershell.md) em vez disso:
* Eles funcionam automaticamente com o projeto atual selecionado na **Package Manager Console** sem exigir que você mude manualmente os diretórios.
* Eles abrem automaticamente os arquivos gerados por um comando após a conclusão do comando.

## <a name="installing-the-tools"></a>Instalando as ferramentas

O procedimento de instalação depende do tipo de projeto e de versão:

* EF Core 3.x
* ASP.NET Core versão 2.1 e posterior
* EF Core 2.x
* EF Core 1.x

### <a name="ef-core-3x"></a>EF Core 3.x

* `dotnet ef` deve ser instalado como uma ferramenta global ou local. A maioria dos desenvolvedores instalará `dotnet ef` como uma ferramenta global com o seguinte comando:

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

  Você também pode usar `dotnet ef` como ferramenta de local. Para usá-lo como uma ferramenta local, restaure as dependências de um projeto que declara-o como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).

* Instalar o [.NET Core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). O SDK deve ser instalado, mesmo se você tiver a versão mais recente do Visual Studio.

* Instale o versão mais recente `Microsoft.EntityFrameworkCore.Design` pacote.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Instalar o atual [SDK do .NET Core](https://www.microsoft.com/net/download/core). O SDK precisa ser instalado, mesmo se você tiver a versão mais recente do Visual Studio 2017.

  Isso é tudo o que é necessário para o ASP.NET Core 2.1 + porque o `Microsoft.EntityFrameworkCore.Design` pacote está incluído na [metapacote Microsoft](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (não no ASP.NET Core)

O `dotnet ef` comandos forem incluídos no SDK do .NET Core, mas para habilitar os comandos que você precisa instalar o `Microsoft.EntityFrameworkCore.Design` pacote.

* Instalar o atual [SDK do .NET Core](https://www.microsoft.com/net/download/core). O SDK deve ser instalado, mesmo se você tiver a versão mais recente do Visual Studio.

* Instalar o estável mais recente `Microsoft.EntityFrameworkCore.Design` pacote.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Instale o .NET Core SDK versão 2.1.200. Em versões posteriores não são compatíveis com as ferramentas CLI para o EF Core 1.0 e 1.1.

* Configurar o aplicativo para usar o 2.1.200 SDK versão modificando seus [global. JSON](/dotnet/core/tools/global-json) arquivo. Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto).

* Edite o arquivo de projeto e adicione `Microsoft.EntityFrameworkCore.Tools.DotNet` como um `DotNetCliToolReference` item. Especifique a versão mais recente do 1.x, por exemplo: 1.1.6. Consulte o exemplo de arquivo de projeto no final desta seção.

* Instale a versão 1.x mais recente do `Microsoft.EntityFrameworkCore.Design` empacotar, por exemplo:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Com as duas referências de pacote adicionadas, o arquivo de projeto é semelhante a:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Uma referência de pacote com `PrivateAssets="All"` não é exposta aos projetos que fazem referência a este projeto. Essa restrição é especialmente útil para os pacotes que normalmente são usados apenas durante o desenvolvimento.

### <a name="verify-installation"></a>Verificar a instalação

Execute os seguintes comandos para verificar se as ferramentas da CLI do EF Core estão corretamente instaladas:

  ``` Console
  dotnet restore
  dotnet ef
  ```

A saída do comando identifica a versão das ferramentas do uso:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Usando as ferramentas

Antes de usar as ferramentas, você talvez precise criar um projeto de inicialização ou defina o ambiente.

### <a name="target-project-and-startup-project"></a>Projeto de destino e projeto de inicialização

Os comandos se referir a um *project* e uma *projeto de inicialização*.

* O *project* também é conhecido como o *projeto de destino* porque é onde os comandos Adicionar ou remover arquivos. Por padrão, o projeto no diretório atual é o projeto de destino. Você pode especificar um projeto diferente como projeto de destino usando o <nobr> `--project` </nobr> opção.

* O *projeto de inicialização* é aquele que as ferramentas de compilar e executar. Tem as ferramentas executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de caracteres de conexão de banco de dados e a configuração do modelo. Por padrão, o projeto no diretório atual é o projeto de inicialização. Você pode especificar um projeto diferente como projeto de inicialização usando o <nobr> `--startup-project` </nobr> opção.

O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto. Um cenário típico no qual eles são projetos separados é que quando:

* As classes de entidade e de contexto do EF Core estão em uma biblioteca de classes do .NET Core.
* Um aplicativo de console .NET Core ou aplicativo web faz referência a biblioteca de classes.

Também é possível [coloque o código de migrações em uma biblioteca de classes separada do contexto do EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Outras estruturas de destino

As ferramentas da CLI funcionam com projetos do .NET Core e projetos do .NET Framework. Os aplicativos que têm o modelo do EF Core em uma biblioteca de classes .NET Standard não podem ter um projeto de .NET Framework ou .NET Core. Por exemplo, isso é verdadeiro para aplicativos Xamarin e plataforma Universal do Windows. Nesses casos, você pode criar um projeto de aplicativo de console .NET Core cuja única finalidade é atuar como projeto de inicialização para as ferramentas. O projeto pode ser um projeto fictício, sem nenhum código real &mdash; só é necessário fornecer um destino para as ferramentas.

Por que é um projeto fictício necessário? Como mencionado anteriormente, as ferramentas de tem que executar o código do aplicativo em tempo de design. Para fazer isso, eles precisam usar o tempo de execução do .NET Core. Quando o modelo do EF Core está em um projeto que tem como alvo o .NET Core ou .NET Framework, as ferramentas do EF Core emprestam o tempo de execução do projeto. E não é possível fazer isso se o modelo do EF Core está em uma biblioteca de classes .NET Standard. O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs que devem dar suporte a implementações do .NET. Portanto, .NET Standard não é suficiente para as ferramentas do EF Core executar o código do aplicativo. O projeto fictício, que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar a biblioteca de classes .NET Standard.

### <a name="aspnet-core-environment"></a>Ambiente do ASP.NET Core

Para especificar o ambiente para projetos do ASP.NET Core, defina as **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar comandos.

## <a name="common-options"></a>Opções comuns

|                   | Opção                            | Descrição                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Mostra a saída JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | A classe `DbContext` a ser usada. Nome de classe totalmente qualificado com namespaces ou única.  Se essa opção for omitida, o EF Core encontrará a classe de contexto. Se houver várias classes de contexto, essa opção é necessária.                                            |
| `-p`              | `--project <PROJECT>`             | Caminho relativo à pasta do projeto do projeto de destino.  Valor padrão é a pasta atual.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Caminho relativo à pasta do projeto do projeto de inicialização. Valor padrão é a pasta atual.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | O [Moniker da estrutura de destino](/dotnet/standard/frameworks#supported-target-framework-versions) para o [estrutura de destino](/dotnet/standard/frameworks).  Use quando o arquivo de projeto especifica várias estruturas de destino, e você deseja selecionar um deles. |
|                   | `--configuration <CONFIGURATION>` | A configuração de build, por exemplo: `Debug` ou `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | O identificador do tempo de execução para restaurar os pacotes para o destino. Para obter uma lista de RIDs (Identificadores de Tempo de Execução), veja o [Catálogo de RIDs](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Mostre informações de Ajuda.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Mostra saída detalhada.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Não colorir saída.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Prefixo com o nível de saída.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>lista de banco de dados do dotnet ef

Descarta o banco de dados.

Opções:

|                   | Opção                   | Descrição                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Confirme se não.                                           |
|                   | <nobr>`--dry-run`</nobr> | Mostrar qual banco de dados deve ser descartado, mas não o remova. |

## <a name="dotnet-ef-database-update"></a>atualização de banco de dados do dotnet ef

Atualiza o banco de dados para a última migração ou para uma migração especificada.

Argumentos:

| Argumento      | Descrição                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | A migração de destino. As migrações podem ser identificadas por nome ou ID. O número de 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações para ser revertido. Se nenhuma migração for especificada, o comando assume como padrão para a última migração. |

Os exemplos a seguir atualiza o banco de dados para uma migração especificada. O primeiro usa o nome da migração e o segundo usa a ID de migração:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informações do dotnet ef dbcontext

Obtém informações sobre um `DbContext` tipo.

## <a name="dotnet-ef-dbcontext-list"></a>lista do dotnet ef dbcontext

Listas disponíveis `DbContext` tipos.

## <a name="dotnet-ef-dbcontext-scaffold"></a>scaffold do dotnet ef dbcontext

Gera código para um `DbContext` e tipos de entidade para um banco de dados. Para este comando para gerar um tipo de entidade que, a tabela de banco de dados deve ter uma chave primária.

Argumentos:

| Argumento       | Descrição                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | A cadeia de conexão para o banco de dados. Para projetos do ASP.NET Core 2.x, o valor pode ser *nome =\<nome da cadeia de caracteres de conexão >* . Nesse caso, o nome vem de fontes de configuração que são configuradas para o projeto. |
| `<PROVIDER>`   | O provedor a ser usado. Normalmente isso é o nome do pacote do NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Opções:

|                 | Opção                                   | Descrição                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Use atributos para configurar o modelo (quando possível). Se essa opção for omitida, somente a API fluente é usada.                                                                |
| `-c`            | `--context <NAME>`                       | O nome da `DbContext` classe gerar.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | O diretório para colocar o `DbContext` arquivo de classe no. Caminhos são relativos ao diretório do projeto. Namespaces são derivados dos nomes de pasta.                                 |
| `-f`            | `--force`                                | Substitua arquivos existentes.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | O diretório para colocar os arquivos de classe de entidade. Caminhos são relativos ao diretório do projeto.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Os esquemas de tabelas para gerar tipos de entidade para. Para especificar vários esquemas, repita `--schema` para cada uma delas. Se essa opção for omitida, todos os esquemas são incluídos.          |
| `-t`            | `--table <TABLE_NAME>`...                | As tabelas para gerar tipos de entidade para. Para especificar várias tabelas, repita `-t` ou `--table` para cada uma delas. Se essa opção for omitida, todas as tabelas são incluídas.                |
|                 | `--use-database-names`                   | Use nomes de tabela e coluna exatamente como aparecem no banco de dados. Se essa opção for omitida, nomes de banco de dados são alterados de acordo com mais de perto para convenções de estilo de nome em C#. |

O exemplo a seguir usa o Scaffold de todos os esquemas e tabelas e coloca os novos arquivos na *modelos* pasta.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

O exemplo a seguir usa o Scaffold somente tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Adicionar migrações do ef dotnet

Adiciona uma nova migração.

Argumentos:

| Argumento | Descrição                |
|:---------|:---------------------------|
| `<NAME>` | O nome da migração. |

Opções:

|                   | Opção                             | Descrição                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | O diretório (e sub-namespace) para usar. Caminhos são relativos ao diretório do projeto. O padrão é "Migrações". |

## <a name="dotnet-ef-migrations-list"></a>lista de migrações dotnet ef

Lista as migrações disponíveis.

## <a name="dotnet-ef-migrations-remove"></a>Remover de migrações do ef dotnet

Remove a última migração (reverte as alterações de código que foram feitas para a migração).

Opções:

|                   | Opção    | Descrição                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados). |

## <a name="dotnet-ef-migrations-script"></a>script de migrações dotnet ef

Gera um script SQL de migrações.

Argumentos:

| Argumento | Descrição                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | A migração inicial. As migrações podem ser identificadas por nome ou ID. O número de 0 é um caso especial que significa *antes da primeira migração*. O padrão é 0. |
| `<TO>`   | A migração final. O padrão é para a última migração.                                                                                                         |

Opções:

|                   | Opção            | Descrição                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | O arquivo para gravar o script.                                   |
| `-i`              | `--idempotent`    | Gere um script que pode ser usado em um banco de dados em qualquer migração. |

O exemplo a seguir cria um script para a migração de InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

O exemplo a seguir cria um script para todas as migrações após a migração InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Recursos adicionais

* [Migrações](xref:core/managing-schemas/migrations/index)
* [Engenharia reversa](xref:core/managing-schemas/scaffolding)
