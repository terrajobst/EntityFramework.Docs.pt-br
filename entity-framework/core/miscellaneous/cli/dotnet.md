---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a>Ferramentas de linha de comando do EF Core .NET
===============================
As ferramentas de linha de comando Entity Framework Core .NET são uma extensão de várias plataformas **dotnet** comando, que é parte do [.NET Core SDK][2].

> [!TIP]
> Se você estiver usando o Visual Studio, é recomendável [as ferramentas de PMC] [ 1] em vez disso, uma vez que eles fornecem uma experiência mais integrada.

<a name="installing-the-tools"></a>Instalando as ferramentas
--------------------
> [!NOTE]
> O núcleo do .NET SDK versão 2.1.300 e mais recente inclui **dotnet ef** comandos que são compatíveis com o EF Core 2.0 e versões posteriores. Portanto, se você estiver usando versões recentes do SDK de núcleo do .NET e o tempo de execução do núcleo do EF, nenhuma instalação é necessária e você pode ignorar o restante desta seção.
>
> Por outro lado, o **dotnet ef** ferramenta contida na versão do SDK do .NET Core 2.1.300 e mais recente não é compatível com a versão EF Core 1.0 e 1.1. Antes de você pode trabalhar com um projeto que usa essas versões anteriores do EF principal em um computador que tem o SDK do .NET Core 2.1.300 ou posterior esteja instalado, você também deve instalar a versão 2.1.200 ou mais antiga do SDK e configure o aplicativo para usar a versão antiga, modificando seu  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) arquivo. Normalmente, esse arquivo está incluído no diretório da solução (um acima do projeto). Em seguida, você poderá continuar com a instrução 2161DS abaixo.

Para versões anteriores do SDK do núcleo do .NET, você pode instalar as ferramentas de linha de comando do EF principais .NET usando estas etapas:

1. Edite o arquivo de projeto e adicionar Microsoft.EntityFrameworkCore.Tools.DotNet como um item de DotNetCliToolReference (veja abaixo)
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
> Uma referência de pacote com `PrivateAssets="All"` significa que ela não está exposta a projetos que fazem referência a este projeto, o que é especialmente útil para os pacotes que normalmente são usados somente durante o desenvolvimento.

Se você fez tudo certo, você poderá executar com êxito o comando a seguir em um prompt de comando.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Usando as ferramentas
---------------
Sempre que você chamar um comando, há dois projetos envolvidas:

O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos). O projeto de destino por padrão para o projeto no diretório atual, mas pode ser alterado usando o <nobr> **– projeto** </nobr> opção.

O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto. Ele também usa como padrão o projeto no diretório atual, mas pode ser alterado usando o **– projeto de inicialização** opção.

> [!NOTE]
> Por exemplo, atualizar o banco de dados do seu aplicativo web que tenha o EF Core instalado em um projeto diferente teria esta aparência: `dotnet ef database update --project {project-path}` (do diretório de aplicativo da web)

Opções comuns:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Mostra saída JSON.           |
| -c | – contexto \<DBCONTEXT >           | O DbContext para usar.       |
| -p | – projeto \<projeto >             | O projeto a usar.         |
| -s | – projeto de inicialização \<projeto >     | O projeto de inicialização para usar. |
|    | – framework \<FRAMEWORK >         | A estrutura de destino.       |
|    | – configuração \<Configuração > | A configuração a ser usada.   |
|    | o tempo de execução – \<identificador >          | O tempo de execução para usar.         |
| -h | – Ajuda                           | Mostra informações de Ajuda.      |
| -v | -verbose                        | Mostra saída detalhada.        |
|    | – sem cor                       | Não colorir saída.      |
|    | --prefix-output                  | Prefixo com o nível de saída.   |


> [!TIP]
> Para especificar o ambiente do ASP.NET Core, defina o **ASPNETCORE_ENVIRONMENT** variável de ambiente antes de executar.

<a name="commands"></a>Comandos
--------

### <a name="dotnet-ef-database-drop"></a>remoção de banco de dados de ef dotnet

Descarta o banco de dados.

Opções:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | -force   | Não confirme.                                           |
|    | -Execute | Mostrar qual banco de dados deve ser descartado, mas não solte-o. |

### <a name="dotnet-ef-database-update"></a>atualização de banco de dados de ef dotnet

Atualiza o banco de dados para uma migração especificada.

Argumentos:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRAÇÃO &GT; | A migração de destino. Se for 0, todas as migrações serão revertidas. O padrão é para a última migração. |

### <a name="dotnet-ef-dbcontext-info"></a>informações do dotnet ef dbcontext

Obtém informações sobre um tipo DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>lista de dbcontext ef dotnet

Lista os tipos disponíveis de DbContext.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffold de dbcontext ef dotnet

Scaffolds um tipos DbContext e entidade para um banco de dados.

Argumentos:

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| \<CONEXÃO &GT; | A cadeia de caracteres de conexão para o banco de dados.                              |
| \<PROVEDOR &GT;   | O provedor a ser usado. (por exemplo, Microsoft.EntityFrameworkCore.SqlServer) |

Opções:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --data-annotations                      | Use atributos para configurar o modelo (onde for possível). Se omitido, somente a API fluente é usada. |
| -c              | – contexto \<nome >                       | O nome do DbContext.                                                                       |
|                 | -dir contexto \<caminho >                   | O diretório para colocar o arquivo DbContext no. Caminhos são relativas ao diretório do projeto.             |
| -f              | -force                                 | Substitua arquivos existentes.                                                                        |
| -o              | -dir saída \<caminho >                    | O diretório de colocar arquivos em. Caminhos são relativas ao diretório do projeto.                      |
|                 | <nobr>--schema \<SCHEMA_NAME>...</nobr> | Os esquemas de tabelas para gerar tipos de entidade para.                                              |
| -t              | -tabela \<TABLE_NAME >...                | As tabelas para gerar tipos de entidade para.                                                         |
|                 | – use nomes de banco de dados                    | Use nomes de tabela e coluna diretamente do banco de dados.                                           |

### <a name="dotnet-ef-migrations-add"></a>Adicionar migrações de ef dotnet

Adiciona uma nova migração.

Argumentos:

|         |                            |
|:--------|:---------------------------|
| \<NOME &GT; | O nome da migração. |

Opções:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>-dir saída \<caminho ></nobr> | O diretório (e sub-namespace) a ser usado. Caminhos são relativas ao diretório do projeto. O padrão é "Migrações". |

### <a name="dotnet-ef-migrations-list"></a>lista de migrações ef dotnet

Lista as migrações disponíveis.

### <a name="dotnet-ef-migrations-remove"></a>Remova as migrações de ef dotnet

Remove a última migração.

Opções:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | -force | Reverta a migração se ela foi aplicada ao banco de dados. |

### <a name="dotnet-ef-migrations-script"></a>script de migrações ef dotnet

Gera um script SQL de migrações.

Argumentos:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<DE &GT; | A migração inicial. O padrão é 0 (o banco de dados inicial). |
| \<PARA &GT;   | A migração final. O padrão é para a última migração.         |

Opções:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | -saída \<arquivo > | O arquivo para gravar o resultado.                                   |
| -i | – idempotente     | Gere um script que pode ser usado em um banco de dados em qualquer migração. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
