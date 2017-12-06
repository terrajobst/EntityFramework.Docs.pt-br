---
title: Package Manager Console (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a>Ferramentas do EF Core Package Manager Console
=====================================
As ferramentas de EF Core pacote Manager Console (PMC) são executadas dentro do Visual Studio usando o NuGet [Package Manager Console][2].
Essas ferramentas trabalhar com projetos do .NET Framework e o .NET Core.

> [!TIP]
> Não usando o Visual Studio? O [EF principais ferramentas de linha de comando] [ 1] ficam entre plataformas e são executados dentro de um prompt de comando.

<a name="installing-the-tools"></a>Instalando as ferramentas
--------------------
Instale as ferramentas do EF Core Package Manager Console instalando o pacote Microsoft.EntityFrameworkCore.Tools NuGet.
Você pode instalá-lo executando o seguinte comando em [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Se tudo funcionou corretamente, você deve ser capaz de executar este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Se seu projeto de inicialização tem como destino .NET Standard [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas.

> [!IMPORTANT]
> Se você estiver usando **Universal do Windows** ou **Xamarin**, mova seu código EF para uma biblioteca de classes .NET padrão e [destino entre uma estrutura com suporte] [ 3] antes de usar as ferramentas. Especifique a biblioteca de classe como seu projeto de inicialização.

<a name="using-the-tools"></a>Usando as ferramentas
---------------
Sempre que você chamar um comando, há dois projetos envolvidas:

O projeto de destino é onde todos os arquivos são adicionados (ou, em alguns casos removidos). O projeto de destino padrão é a **projeto padrão** selecionado no Console do Gerenciador de pacotes, mas também podem ser especificadas usando-parâmetro de projeto.

O projeto de inicialização é emulado pelas ferramentas durante a execução de código do projeto. O padrão é um **definir como projeto de inicialização** no Gerenciador de soluções. Ele também pode ser especificado usando o parâmetro - StartupProject.

Parâmetros comuns:

|                           |                             |
| ------------------------- | --------------------------- |
| -Contexto \<cadeia de caracteres >        | O DbContext para usar.       |
| -Projeto \<cadeia de caracteres >        | O projeto a usar.         |
| -StartupProject \<cadeia de caracteres > | O projeto de inicialização para usar. |
| -Verbose                  | Mostra saída detalhada.        |

Para mostrar informações de ajuda sobre um comando, use do PowerShell `Get-Help` comando.

> [!TIP]
> Os parâmetros de contexto, o projeto e StartupProject suportam a expansão da tabulação.

> [!TIP]
> Definir **env:ASPNETCORE_ENVIRONMENT** antes de executar para especificar o ambiente do ASP.NET Core.

<a name="commands"></a>Comandos
--------

### <a name="add-migration"></a>Adicionar-migração

Adiciona uma nova migração.

Parâmetros:

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| ***-Nome*** \<cadeia de caracteres >              | O nome da migração.                                                      |
| <nobr>-OutputDir \<cadeia de caracteres ></nobr>  | O diretório (e sub-namespace) a ser usado. Caminhos são relativas ao diretório do projeto. O padrão é "Migrações". |

> [!NOTE]
> Parâmetros em **negrito** são necessários e aqueles na *itálico* são posicional.

### <a name="drop-database"></a>Remover banco de dados

Descarta o banco de dados.

Parâmetros:

|          |                                                          |
| -------- | -------------------------------------------------------- |
| -WhatIf  | Mostrar qual banco de dados deve ser descartado, mas não solte-o. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtém informações sobre um tipo DbContext.

### <a name="remove-migration"></a>Remove-migração

Remove a última migração.

Parâmetros:

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| -Force | Não verificar se a migração tiver sido aplicada ao banco de dados. |

### <a name="scaffold-dbcontext"></a>Scaffold DbContext

Scaffolds um tipos DbContext e entidade para um banco de dados.

Parâmetros:

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <nobr>***-Conexão*** \<cadeia de caracteres ></nobr> | A cadeia de caracteres de conexão para o banco de dados.                                    |
| ***-Provedor*** \<cadeia de caracteres >                | O provedor a ser usado. (Por ex.: Microsoft.EntityFrameworkCore.SqlServer)       |
| -OutputDir \<cadeia de caracteres >                     | O diretório de colocar arquivos em. Caminhos são relativas ao diretório do projeto. |
| -Contexto \<cadeia de caracteres >                       | O nome do DbContext para gerar.                                    |
| -Esquemas \<String [] >                     | Os esquemas de tabelas para gerar tipos de entidade para.                       |
| -Tabelas \<String [] >                      | As tabelas para gerar tipos de entidade para.                                  |
| -DataAnnotations                         | Use atributos para configurar o modelo (onde for possível). Se omitido, somente a API fluente é usada. |
| -UseDatabaseNames                        | Use nomes de tabela e coluna diretamente do banco de dados.                    |
| -Force                                   | Substitua arquivos existentes.                                                 |

### <a name="script-migration"></a>Migração do script

Gera um script SQL de migrações.

Parâmetros:

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| *-From* \<cadeia de caracteres > | A migração inicial. O padrão é 0 (o banco de dados inicial).      |
| *-* \<Cadeia de caracteres >   | A migração final. O padrão é para a última migração.              |
| -Idempotente       | Gere um script que pode ser usado em um banco de dados em qualquer migração. |
| -Saída \<cadeia de caracteres > | O arquivo para gravar o resultado.                                   |

> [!TIP]
> Para, e expansão da tabulação oferece suporte a parâmetros de saída.

### <a name="update-database"></a>Atualizar banco de dados

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <nobr>*-Migração* \<cadeia de caracteres ></nobr> | A migração de destino. Se for '0', todas as migrações serão revertidas. O padrão é para a última migração. |

> [!TIP]
> O parâmetro de migração dá suporte a expansão da tabulação.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
