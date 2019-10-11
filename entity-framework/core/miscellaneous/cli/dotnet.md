---
title: Referência de ferramentas do EF Core (CLI do .NET) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: e5b42275aa575d711e1dcdf3d2ba3cb29a036727
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181251"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referência de ferramentas de Entity Framework Core-CLI .NET

As ferramentas da CLI (interface de linha de comando) para Entity Framework Core executar tarefas de desenvolvimento em tempo de design. Por exemplo, eles criam [migrações](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), aplicam migrações e geram código para um modelo com base em um banco de dados existente. Os comandos são uma extensão para o comando [dotnet](/dotnet/core/tools) de plataforma cruzada, que faz parte do [SDK do .NET Core](https://www.microsoft.com/net/core). Essas ferramentas funcionam com projetos do .NET Core.

Se você estiver usando o Visual Studio, recomendamos as [ferramentas de console do Gerenciador de pacotes](powershell.md) em vez disso:
* Eles trabalham automaticamente com o projeto atual selecionado no **console do Gerenciador de pacotes** sem exigir que você alterne manualmente os diretórios.
* Eles abrem automaticamente os arquivos gerados por um comando após a conclusão do comando.

## <a name="installing-the-tools"></a>Instalando as ferramentas

O procedimento de instalação depende do tipo de projeto e da versão:

* EF Core 3. x
* ASP.NET Core versão 2,1 e posterior
* EF Core 2. x
* EF Core 1. x

### <a name="ef-core-3x"></a>EF Core 3. x

* o `dotnet ef` deve ser instalado como uma ferramenta global ou local. A maioria dos desenvolvedores irá instalar `dotnet ef` como uma ferramenta global com o seguinte comando:

  ``` console
  dotnet tool install --global dotnet-ef
  ```

  Você também pode usar `dotnet ef` como ferramenta local. Para usá-lo como uma ferramenta local, restaure as dependências de um projeto que a declare como uma dependência de ferramentas usando um [arquivo de manifesto da ferramenta](https://github.com/dotnet/cli/issues/10288).

* Instale o [SDK do .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). O SDK deve ser instalado mesmo que você tenha a versão mais recente do Visual Studio.

* Instale o pacote `Microsoft.EntityFrameworkCore.Design` mais recente.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Instale o [SDK do .NET Core](https://www.microsoft.com/net/download/core)atual. O SDK precisa ser instalado, mesmo se você tiver a versão mais recente do Visual Studio 2017.

  Isso é tudo o que é necessário para o ASP.NET Core 2.1 + porque o pacote `Microsoft.EntityFrameworkCore.Design` está incluído no [metapacote Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2. x (não ASP.NET Core)

Os comandos `dotnet ef` são incluídos no SDK do .NET Core, mas para habilitar os comandos, você precisa instalar o pacote `Microsoft.EntityFrameworkCore.Design`.

* Instale o [SDK do .NET Core](https://www.microsoft.com/net/download/core)atual. O SDK deve ser instalado mesmo que você tenha a versão mais recente do Visual Studio.

* Instale o pacote mais recente de `Microsoft.EntityFrameworkCore.Design` estável.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1. x

* Instale o SDK do .NET Core versão 2.1.200. As versões posteriores não são compatíveis com as ferramentas da CLI para EF Core 1,0 e 1,1.

* Configure o aplicativo para usar a versão do SDK do 2.1.200 modificando seu arquivo [global. JSON](/dotnet/core/tools/global-json) . Esse arquivo é normalmente incluído no diretório da solução (um acima do projeto).

* Edite o arquivo de projeto e adicione `Microsoft.EntityFrameworkCore.Tools.DotNet` como um item `DotNetCliToolReference`. Especifique a versão mais recente do 1. x, por exemplo: 1.1.6. Consulte o exemplo de arquivo de projeto no final desta seção.

* Instale a versão mais recente do 1. x do pacote `Microsoft.EntityFrameworkCore.Design`, por exemplo:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Com as duas referências de pacote adicionadas, o arquivo de projeto tem uma aparência semelhante a esta:

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

  Uma referência de pacote com `PrivateAssets="All"` não é exposta a projetos que fazem referência a este projeto. Essa restrição é especialmente útil para pacotes que normalmente são usados apenas durante o desenvolvimento.

### <a name="verify-installation"></a>Verificar instalação

Execute os seguintes comandos para verificar se as ferramentas da CLI do EF Core estão instaladas corretamente:

  ``` Console
  dotnet restore
  dotnet ef
  ```

A saída do comando identifica a versão das ferramentas em uso:

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

Antes de usar as ferramentas, talvez seja necessário criar um projeto de inicialização ou definir o ambiente.

### <a name="target-project-and-startup-project"></a>Projeto de destino e projeto de inicialização

Os comandos referem-se a um *projeto* e um *projeto de inicialização*.

* O *projeto* também é conhecido como o *projeto de destino* porque é onde os comandos adicionam ou removem arquivos. Por padrão, o projeto no diretório atual é o projeto de destino. Você pode especificar um projeto diferente como projeto de destino usando a opção <nobr>`--project`</nobr> .

* O *projeto de inicialização* é aquele que as ferramentas criam e executam. As ferramentas precisam executar o código do aplicativo em tempo de design para obter informações sobre o projeto, como a cadeia de conexão do banco de dados e a configuração do modelo. Por padrão, o projeto no diretório atual é o projeto de inicialização. Você pode especificar um projeto diferente como projeto de inicialização usando a opção <nobr>`--startup-project`</nobr> .

O projeto de inicialização e o projeto de destino geralmente são o mesmo projeto. Um cenário típico em que são projetos separados é quando:

* O contexto de EF Core e as classes de entidade estão em uma biblioteca de classes do .NET Core.
* Um aplicativo de console do .NET Core ou aplicativo Web faz referência à biblioteca de classes.

Também é possível [colocar o código de migrações em uma biblioteca de classes separada do contexto de EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Outras estruturas de destino

As ferramentas da CLI funcionam com projetos do .NET Core e .NET Framework projetos. Os aplicativos que têm o modelo de EF Core em uma biblioteca de classes de .NET Standard podem não ter um projeto do .NET Core ou do .NET Framework. Por exemplo, isso é verdadeiro para aplicativos Xamarin e Plataforma Universal do Windows. Nesses casos, você pode criar um projeto de aplicativo de console do .NET Core cuja única finalidade é agir como projeto de inicialização para as ferramentas. O projeto pode ser um projeto fictício sem código real &mdash; é necessário apenas para fornecer um destino para as ferramentas.

Por que um projeto fictício é necessário? Conforme mencionado anteriormente, as ferramentas precisam executar o código do aplicativo em tempo de design. Para fazer isso, eles precisam usar o tempo de execução do .NET Core. Quando o modelo de EF Core está em um projeto direcionado ao .NET Core ou .NET Framework, as ferramentas de EF Core emprestam o tempo de execução do projeto. Eles não poderão fazer isso se o modelo de EF Core estiver em uma biblioteca de classes .NET Standard. O .NET Standard não é uma implementação real do .NET; é uma especificação de um conjunto de APIs para as quais as implementações do .NET devem dar suporte. Portanto .NET Standard não é suficiente para as ferramentas de EF Core executarem o código do aplicativo. O projeto fictício que você cria para usar como projeto de inicialização fornece uma plataforma de destino concreta na qual as ferramentas podem carregar o .NET Standard biblioteca de classes.

### <a name="aspnet-core-environment"></a>Ambiente de ASP.NET Core

Para especificar o ambiente para projetos de ASP.NET Core, defina a variável de ambiente **ASPNETCORE_ENVIRONMENT** antes de executar os comandos.

## <a name="common-options"></a>Opções comuns

|                   | Opção                            | Descrição                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Mostrar saída JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | A classe `DbContext` a ser usada. Nome da classe somente ou totalmente qualificado com namespaces.  Se essa opção for omitida, EF Core encontrará a classe de contexto. Se houver várias classes de contexto, essa opção será necessária.                                            |
| `-p`              | `--project <PROJECT>`             | Caminho relativo para a pasta do projeto de destino.  O valor padrão é a pasta atual.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Caminho relativo para a pasta do projeto de inicialização. O valor padrão é a pasta atual.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | O [moniker da estrutura de destino](/dotnet/standard/frameworks#supported-target-framework-versions) para a [estrutura de destino](/dotnet/standard/frameworks).  Use quando o arquivo de projeto especificar várias estruturas de destino e você desejar selecionar uma delas. |
|                   | `--configuration <CONFIGURATION>` | A configuração de compilação, por exemplo: `Debug` ou `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | O identificador do tempo de execução de destino para o qual restaurar os pacotes. Para obter uma lista de RIDs (Identificadores de Tempo de Execução), veja o [Catálogo de RIDs](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Mostrar informações de ajuda.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Mostrar saída detalhada.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Não colorir a saída.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Saída de prefixo com nível.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>Descarte de banco de dados dotnet EF

Remove o banco de dados.

Opções:

|                   | Opção                   | Descrição                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Não confirme.                                           |
|                   | <nobr>`--dry-run`</nobr> | Mostrar qual banco de dados seria descartado, mas não descartá-lo. |

## <a name="dotnet-ef-database-update"></a>atualização do banco de dados dotnet EF

Atualiza o banco de dados para a última migração ou para uma migração especificada.

Argumentos:

| Argumento      | Descrição                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | A migração de destino. As migrações podem ser identificadas por nome ou por ID. O número 0 é um caso especial que significa *antes da primeira migração* e faz com que todas as migrações sejam revertidas. Se nenhuma migração for especificada, o comando usa como padrão a última migração. |

Os exemplos a seguir atualizam o banco de dados para uma migração especificada. O primeiro usa o nome de migração e o segundo usa a ID de migração:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>informações do dotnet EF DbContext

Obtém informações sobre um tipo `DbContext`.

## <a name="dotnet-ef-dbcontext-list"></a>lista de DbContext de EF do dotnet

Lista os tipos `DbContext` disponíveis.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet EF DbContext Scaffold

Gera código para um `DbContext` e tipos de entidade para um banco de dados. Para que esse comando gere um tipo de entidade, a tabela de banco de dados deve ter uma chave primária.

Argumentos:

| Argumento       | Descrição                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | A cadeia de conexão para o banco de dados. Para projetos ASP.NET Core 2. x, o valor pode ser *Name = \<Name da cadeia de conexão >* . Nesse caso, o nome é proveniente das fontes de configuração que estão configuradas para o projeto. |
| `<PROVIDER>`   | O provedor a ser usado. Normalmente, esse é o nome do pacote NuGet, por exemplo: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Opções:

|                 | Opção                                   | Descrição                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Use atributos para configurar o modelo (sempre que possível). Se essa opção for omitida, somente a API fluente será usada.                                                                |
| `-c`            | `--context <NAME>`                       | O nome da classe `DbContext` a ser gerada.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | O diretório no qual colocar o arquivo de classe `DbContext`. Os caminhos são relativos ao diretório do projeto. Os namespaces são derivados dos nomes de pasta.                                 |
| `-f`            | `--force`                                | Substitui os arquivos existentes.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | O diretório no qual colocar os arquivos de classe de entidade. Os caminhos são relativos ao diretório do projeto.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Os esquemas de tabelas para os quais gerar tipos de entidade. Para especificar vários esquemas, repita `--schema` para cada um. Se essa opção for omitida, todos os esquemas serão incluídos.          |
| `-t`            | `--table <TABLE_NAME>`...                | As tabelas para as quais gerar tipos de entidade. Para especificar várias tabelas, repita `-t` ou `--table` para cada uma delas. Se essa opção for omitida, todas as tabelas serão incluídas.                |
|                 | `--use-database-names`                   | Use nomes de tabela e coluna exatamente como aparecem no banco de dados. Se essa opção for omitida, os nomes de banco de dados serão alterados C# para se adequar melhor às convenções de estilo de nome. |

O exemplo a seguir aplica Scaffold todos os esquemas e tabelas e coloca os novos arquivos na pasta *modelos* .

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

O exemplo a seguir aplica Scaffold apenas tabelas selecionadas e cria o contexto em uma pasta separada com um nome especificado:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Adicionar migrações do dotnet EF

Adiciona uma nova migração.

Argumentos:

| Argumento | Descrição                |
|:---------|:---------------------------|
| `<NAME>` | O nome da migração. |

Opções:

|                   | Opção                             | Descrição                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | O diretório (e o subnamespace) a ser usado. Os caminhos são relativos ao diretório do projeto. O padrão é "migrações". |

## <a name="dotnet-ef-migrations-list"></a>lista de migrações dotnet EF

Lista as migrações disponíveis.

## <a name="dotnet-ef-migrations-remove"></a>remoção de migrações dotnet EF

Remove a última migração (reverte as alterações de código que foram feitas para a migração).

Opções:

|                   | Opção    | Descrição                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Reverter a migração (reverter as alterações que foram aplicadas ao banco de dados). |

## <a name="dotnet-ef-migrations-script"></a>script de migrações dotnet EF

Gera um script SQL a partir de migrações.

Argumentos:

| Argumento | Descrição                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | A migração inicial. As migrações podem ser identificadas por nome ou por ID. O número 0 é um caso especial que significa *antes da primeira migração*. O padrão é 0. |
| `<TO>`   | A migração final. O padrão é a última migração.                                                                                                         |

Opções:

|                   | Opção            | Descrição                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | O arquivo no qual gravar o script.                                   |
| `-i`              | `--idempotent`    | Gere um script que possa ser usado em um banco de dados em qualquer migração. |

O exemplo a seguir cria um script para a migração InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

O exemplo a seguir cria um script para todas as migrações após a migração de InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Recursos adicionais

* [Migrações](xref:core/managing-schemas/migrations/index)
* [Engenharia reversa](xref:core/managing-schemas/scaffolding)
