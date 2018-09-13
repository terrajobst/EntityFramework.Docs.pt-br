---
title: .NET core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489603"
---
<a name="ef-core-net-command-line-tools"></a>Ferramentas de linha de comando do EF Core .NET
===============================
As ferramentas de linha de comando do Entity Framework Core .NET são uma extensão para a plataforma cruzada **dotnet** comando, que é parte do [SDK do .NET Core][2].

> [!TIP]
> Se você estiver usando o Visual Studio, recomendamos [as ferramentas de PMC] [ 1] em vez disso, já que eles fornecem uma experiência mais integrada.

<a name="installing-the-tools"></a>Instalando as ferramentas
--------------------
> [!NOTE]
> SDK do .NET Core 2.1.300 de versão e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores. Portanto, se você estiver usando versões recentes do SDK do .NET Core e o tempo de execução do EF Core, nenhuma instalação é necessária e você pode ignorar o restante desta seção.
>
> Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão do EF Core 1.0 e 1.1. Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF Core em um computador que tem o SDK do .NET Core 2.1.300 ou mais recente instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configurar o aplicativo para usar essa versão, modificando sua  [global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo. Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto). Em seguida, você pode prosseguir com a instrução 2161DS abaixo.

Para versões anteriores do SDK do .NET Core, você pode instalar as ferramentas de linha de comando do EF Core .NET usando as seguintes etapas:

1. Edite o arquivo de projeto e adicione entityframeworkcore como um item DotNetCliToolReference (veja abaixo)
2. Execute os seguintes comandos:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

O projeto resultante deve ter esta aparência:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Uma referência de pacote com `PrivateAssets="All"` significa que ele não é exposto aos projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados apenas durante o desenvolvimento.

Se você fez tudo certo, você deve ser capaz de executar com êxito o comando a seguir em um prompt de comando.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Usando as ferramentas
---------------
Sempre que você invoca um comando, existem dois projetos envolvidos:

O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos). O projeto de destino padrão é o projeto no diretório atual, mas pode ser alterado usando o <nobr> **`--project`** </nobr> opção.

O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto. Ele também assume como padrão o projeto no diretório atual, mas pode ser alterado usando o **`--startup-project`** opção.

> [!NOTE]
> Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha instalado em um projeto diferente do EF Core teria esta aparência: `dotnet ef database update --project {project-path}` (a partir do seu diretório de aplicativo web)

Opções comuns:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | Mostra a saída JSON.           |
| -c | `--context <DBCONTEXT>`           | O DbContext usar.       |
| -p | `--project <PROJECT>`             | O projeto para usar.         |
| -s | `--startup-project <PROJECT>`     | O projeto de inicialização para usar. |
|    | `--framework <FRAMEWORK>`         | A estrutura de destino.       |
|    | `--configuration <CONFIGURATION>` | A configuração a ser usada.   |
|    | `--runtime <IDENTIFIER>`          | O tempo de execução para usar.         |
| -h | `--help`                           | Mostre informações de Ajuda.      |
| -v | `--verbose`                        | Mostra saída detalhada.        |
|    | `--no-color`                       | Não colorir saída.      |
|    | `--prefix-output`                  | Prefixo com o nível de saída.   |


> [!TIP]
> Para especificar o ambiente do ASP.NET Core, defina as **ASPNETCORE_ENVIRONMENT** variável de ambiente antes da execução.

<a name="commands"></a>Comandos
--------

### <a name="dotnet-ef-database-drop"></a>lista de banco de dados do dotnet ef

Descarta o banco de dados.

Opções:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | Confirme se não.                                           |
|    | `--dry-run` | Mostrar qual banco de dados deve ser descartado, mas não o remova. |

### <a name="dotnet-ef-database-update"></a>atualização de banco de dados do dotnet ef

Atualiza o banco de dados para uma migração especificada.

Argumentos:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | A migração de destino. Se for 0, todas as migrações serão revertidas. O padrão é para a última migração. |

### <a name="dotnet-ef-dbcontext-info"></a>informações do dotnet ef dbcontext

Obtém informações sobre um tipo DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>lista do dotnet ef dbcontext

Lista os tipos disponíveis de DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffold do dotnet ef dbcontext

Usa o Scaffold de um DbContext e tipos de entidade para um banco de dados.

Argumentos:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | A cadeia de conexão para o banco de dados.                                      |
| `<PROVIDER>`   | O provedor a ser usado. (por exemplo, entityframeworkcore) |

Opções:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | Use atributos para configurar o modelo (quando possível). Se omitido, somente a API fluente é usada. |
| -c              | `--context <NAME>`                       | O nome do DbContext.                                                                       |
|                 | `--context-dir <PATH>`                   | O diretório para colocar o arquivo de DbContext. Caminhos são relativos ao diretório do projeto.             |
| -f              | `--force`                                 | Substitua arquivos existentes.                                                                        |
| -o              | `--output-dir <PATH>`                    | O diretório para colocar os arquivos. Caminhos são relativos ao diretório do projeto.                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Os esquemas de tabelas para gerar tipos de entidade para.                                              |
| -t              | `--table <TABLE_NAME>`...                | As tabelas para gerar tipos de entidade para.                                                         |
|                 | `--use-database-names`                    | Use nomes de tabela e coluna diretamente do banco de dados.                                           |

### <a name="dotnet-ef-migrations-add"></a>Adicionar migrações do ef dotnet

Adiciona uma nova migração.

Argumentos:

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | O nome da migração. |

Opções:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>`--output-dir <PATH>`</nobr> | O diretório (e sub-namespace) para usar. Caminhos são relativos ao diretório do projeto. O padrão é "Migrações". |

### <a name="dotnet-ef-migrations-list"></a>lista de migrações dotnet ef

Lista as migrações disponíveis.

### <a name="dotnet-ef-migrations-remove"></a>Remover de migrações do ef dotnet

Remove a última migração.

Opções:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | Reverta a migração, se ele tiver sido aplicado ao banco de dados. |

### <a name="dotnet-ef-migrations-script"></a>script de migrações dotnet ef

Gera um script SQL de migrações.

Argumentos:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | A migração inicial. O padrão é 0 (o banco de dados inicial). |
| `<TO>`   | A migração final. O padrão é para a última migração.         |

Opções:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | O arquivo para gravar o resultado.                                   |
| -i | `--idempotent`     | Gere um script que pode ser usado em um banco de dados em qualquer migração. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
