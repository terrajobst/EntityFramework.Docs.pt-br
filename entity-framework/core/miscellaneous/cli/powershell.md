---
title: Console do Gerenciador de pacotes (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 0799b0cb7c5d837fdbb7a4af510a9a4d9d34ec1a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949032"
---
<a name="ef-core-package-manager-console-tools"></a>Ferramentas do Console do Gerenciador de pacotes do EF Core
=====================================
As ferramentas do EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando do NuGet [Package Manager Console][2].
Essas ferramentas funcionam com projetos do .NET Framework e o do .NET Core.

> [!TIP]
> Não usando o Visual Studio? O [ferramentas de linha de comando do EF Core] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.

<a name="installing-the-tools"></a>Instalando as ferramentas
--------------------
Instale as ferramentas do Console do Gerenciador de pacotes do EF Core ao instalar o pacote NuGet entityframeworkcore.
Você pode instalá-lo executando o seguinte comando na [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Se tudo funcionou corretamente, você deve ser capaz de executar este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Se seu projeto de inicialização for destinado ao .NET Standard [direcionamento cruzado uma estrutura com suporte] [ 3] antes de usar as ferramentas.

> [!IMPORTANT]
> Se você estiver usando **Windows Universal** ou **Xamarin**, mova seu código do EF para uma biblioteca de classes .NET Standard e [direcionamento cruzado uma estrutura com suporte] [ 3] antes de usar as ferramentas. Especifique a biblioteca de classes como seu projeto de inicialização.

<a name="using-the-tools"></a>Usando as ferramentas
---------------
Sempre que você invoca um comando, existem dois projetos envolvidos:

O projeto de destino é aquele ao qual todos os arquivos são adicionados (ou, em alguns casos, removidos). O projeto de destino padrão é o **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificados usando o - parâmetro de projeto.

O projeto de inicialização é aquele emulado pelas ferramentas durante a execução do código do seu projeto. O padrão é uma **definir como projeto de inicialização** no Gerenciador de soluções. Ele também pode ser especificado usando o parâmetro - o projeto de inicialização.

Parâmetros comuns:

|                           |                             |
|:--------------------------|:----------------------------|
| -O contexto \<cadeia de caracteres >        | O DbContext usar.       |
| -Projeto \<cadeia de caracteres >        | O projeto para usar.         |
| O projeto de inicialização - \<cadeia de caracteres > | O projeto de inicialização para usar. |
| -Verbose                  | Mostra saída detalhada.        |

Para mostrar informações de ajuda sobre um comando, use o PowerShell `Get-Help` comando.

> [!TIP]
> Os parâmetros de contexto, o projeto e o projeto de inicialização oferecem suporte a expansão da tabulação.

> [!TIP]
> Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.

<a name="commands"></a>Comandos
--------

### <a name="add-migration"></a>Add-Migration

Adiciona uma nova migração.

Parâmetros:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Name*** \<cadeia de caracteres >             | O nome da migração.                                                                                       |
| <nobr>-OutputDir \<cadeia de caracteres ></nobr> | O diretório (e sub-namespace) para usar. Caminhos são relativos ao diretório do projeto. O padrão é "Migrações". |

> [!NOTE]
> Parâmetros em **negrito** são necessárias e aqueles na *itálico* são posicionais.

### <a name="drop-database"></a>Banco de dados de destino

Descarta o banco de dados.

Parâmetros:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Mostrar qual banco de dados deve ser descartado, mas não o remova. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtém informações sobre um tipo DbContext.

### <a name="remove-migration"></a>Remove-Migration

Remove a última migração.

Parâmetros:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Reverta a migração, se ele tiver sido aplicado ao banco de dados. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Usa o Scaffold de um DbContext e tipos de entidade para um banco de dados.

Parâmetros:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Conexão*** \<cadeia de caracteres ></nobr> | A cadeia de conexão para o banco de dados.                                                           |
| ***-Provedor*** \<cadeia de caracteres >                | O provedor a ser usado. (por exemplo, entityframeworkcore)                      |
| -OutputDir \<cadeia de caracteres >                     | O diretório para colocar os arquivos. Caminhos são relativos ao diretório do projeto.                      |
| -ContextDir \<cadeia de caracteres >                    | O diretório para colocar o arquivo de DbContext. Caminhos são relativos ao diretório do projeto.             |
| -O contexto \<cadeia de caracteres >                       | O nome do DbContext para gerar.                                                           |
| -Esquemas \<String [] >                     | Os esquemas de tabelas para gerar tipos de entidade para.                                              |
| -Tabelas \<String [] >                      | As tabelas para gerar tipos de entidade para.                                                         |
| -DataAnnotations                         | Use atributos para configurar o modelo (quando possível). Se omitido, somente a API fluente é usada. |
| -UseDatabaseNames                        | Use nomes de tabela e coluna diretamente do banco de dados.                                           |
| -Force                                   | Substitua arquivos existentes.                                                                        |

### <a name="script-migration"></a>Migração do script

Gera um script SQL de migrações.

Parâmetros:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<cadeia de caracteres > | A migração inicial. O padrão é 0 (o banco de dados inicial).      |
| *-Para* \<cadeia de caracteres >   | A migração final. O padrão é para a última migração.              |
| -Idempotentes       | Gere um script que pode ser usado em um banco de dados em qualquer migração. |
| -Saída \<cadeia de caracteres > | O arquivo para gravar o resultado.                                   |

> [!TIP]
> Para, de, e os parâmetros de saída oferecem suporte a expansão da tabulação.

### <a name="update-database"></a>Atualizar banco de dados

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<cadeia de caracteres ></nobr> | A migração de destino. Se for '0', todas as migrações serão revertidas. O padrão é para a última migração. |

> [!TIP]
> O parâmetro de migração dá suporte à expansão da tabulação.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
