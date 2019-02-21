---
title: A engenharia reversa de – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459179"
---
# <a name="reverse-engineering"></a>Engenharia reversa

Engenharia reversa é o processo de scaffolding de classes de tipo de entidade e uma classe DbContext com base em um esquema de banco de dados. Ele pode ser realizado usando o `Scaffold-DbContext` comando das ferramentas do EF Core pacote Manager Console (PMC) ou o `dotnet ef dbcontext scaffold` comando das ferramentas de Interface de linha de comando (CLI) do .NET.

## <a name="installing"></a>Instalando o

Antes de engenharia reversa, você precisará instalar o [ferramentas PMC](xref:core/miscellaneous/cli/powershell) (somente Visual Studio) ou o [ferramentas da CLI](xref:core/miscellaneous/cli/dotnet). Consulte os links para obter detalhes.

Você também precisará instalar um apropriado [provedor de banco de dados](xref:core/providers/index) para o esquema de banco de dados que você deseja fazer engenharia reversa.

## <a name="connection-string"></a>Cadeia de caracteres de Conexão

O primeiro argumento para o comando é uma cadeia de caracteres de conexão ao banco de dados. As ferramentas usará essa cadeia de caracteres de conexão para ler o esquema de banco de dados.

Como você pode colocar entre aspas e escapar a cadeia de caracteres de conexão depende do shell do qual você está usando para executar o comando. Consulte a documentação do seu shell para obter informações específicas. Por exemplo, o PowerShell requer escapar o `$` de caractere, mas não `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuração e os segredos do usuário

Se você tiver um projeto ASP.NET Core, você pode usar o `Name=<connection-string>` sintaxe para ler a cadeia de caracteres de conexão de configuração.

Isso funciona bem com o [ferramenta Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) para manter sua senha do banco de dados separado de sua base de código.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nome do provedor

O segundo argumento é o nome do provedor. O nome do provedor normalmente é o mesmo nome do pacote NuGet do provedor.

## <a name="specifying-tables"></a>Especificação de tabelas

Todas as tabelas no esquema de banco de dados engenharia reversos eles em tipos de entidade por padrão. Você pode limitar quais tabelas engenharia reversos, eles especificando os esquemas e tabelas.

O `-Schemas` parâmetro no PMC e o `--schema` opção na CLI pode ser usada para incluir todas as tabelas em um esquema.

`-Tables` (PMC) e `--table` (CLI) pode ser usada para incluir tabelas específicas.

Para incluir várias tabelas no PMC, use uma matriz.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Para incluir várias tabelas na CLI, especifique a opção várias vezes.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Preserva nomes

Nomes de tabela e coluna são corrigidos para que corresponda melhor às convenções de nomenclatura .NET para tipos e propriedades por padrão. Especificando o `-UseDatabaseNames` PMC de inserção ou a `--use-database-names` opção na CLI desabilitará esse comportamento preserva os nomes de banco de dados original tanto quanto possível. Identificadores inválidos de .NET ainda serão corrigidos e sintetizados nomes como propriedades de navegação ainda estará em conformidade com as convenções de nomenclatura do .NET.

## <a name="fluent-api-or-data-annotations"></a>API Fluent ou anotações de dados

Tipos de entidade são configurados usando a API Fluent por padrão. Especificar `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) em vez disso, usar anotações de dados sempre que possível.

Por exemplo, usando a API Fluent irá dar suporte isso:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Ao usar anotações de dados irá dar suporte isso:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nome de DbContext

O nome da classe DbContext gerado por scaffolding será o nome do banco de dados com o sufixo *contexto* por padrão. Para especificar o outra, use `-Context` no PMC e `--context` na CLI.

## <a name="directories-and-namespaces"></a>Diretórios e namespaces

As classes de entidade e uma classe DbContext são gerados automaticamente no diretório raiz do projeto e usam o namespace do projeto padrão. Você pode especificar o diretório em que as classes são gerados automaticamente usando `-OutputDir` (PMC) ou `--output-dir` (CLI). O namespace será o namespace raiz além dos nomes de todos os subdiretórios no diretório raiz do projeto.

Você também pode usar `-ContextDir` (PMC) e `--context-dir` (CLI) para gerar automaticamente a classe DbContext em um diretório separado das classes de tipo de entidade.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Como ele funciona

Engenharia reversa começa lendo o esquema de banco de dados. Ele lê as informações sobre tabelas, colunas, restrições e índices.

Em seguida, ele usa as informações de esquema para criar um modelo do EF Core. Tabelas são usadas para criar tipos de entidade; colunas são usadas para criar propriedades; e chaves estrangeiras são usadas para criar relações.

Por fim, o modelo é usado para gerar código. Os correspondente entidade classes, a API Fluent e dados anotações de tipo são gerados automaticamente para recriar o mesmo modelo de seu aplicativo.

## <a name="what-doesnt-work"></a>O que não funciona

Nem tudo sobre um modelo pode ser representado usando um esquema de banco de dados. Por exemplo, informações sobre **hierarquias de herança**, **tipos próprios**, e **divisão de tabela** não estão presentes no esquema de banco de dados. Por isso, essas construções serão nunca ser revertida.

Além disso, **alguns tipos de coluna** podem não ter suporte pelo provedor do EF Core. Essas colunas não incluídas no modelo.

O EF Core exige que cada tipo de entidade ter uma chave. Tabelas, no entanto, não é necessário especificar uma chave primária. **Tabelas sem uma chave primária** estão no momento, não com engenharia reversa.

Você pode definir **tokens de simultaneidade** em um modelo do EF Core para impedir que os dois usuários atualizando a mesma entidade ao mesmo tempo. Alguns bancos de dados têm um tipo especial para representar este tipo de coluna (por exemplo, rowversion no SQL Server) no caso, é possível reverter a engenharia essas informações; No entanto, outros tokens de simultaneidade será não ser a engenharia reversa.

## <a name="customizing-the-model"></a>Personalizando o modelo

O código gerado pelo EF Core é seu código. Fique à vontade para alterá-lo. Ele só será registrado se você fazer engenharia reversa do mesmo modelo novamente. Representa o código gerado por scaffolding *uma* modelo que pode ser usado para acessar o banco de dados, mas certamente não é o *somente* modelo que pode ser usado.

Personalize as classes de tipo de entidade e classe DbContext para atender às suas necessidades. Por exemplo, você pode optar por renomear tipos e propriedades, introduzir a hierarquias de herança ou dividir uma tabela em várias entidades. Você também pode remover os índices não exclusivos, sequências não utilizadas e propriedades de navegação, opcionais propriedades escalares e nomes de restrição do modelo.

Você também pode adicionar mais construtores, métodos, propriedades, etc. usando outra classe parcial em um arquivo separado. Essa abordagem funciona mesmo quando você pretende fazer engenharia reversa o modelo novamente.

## <a name="updating-the-model"></a>Atualizando o modelo

Depois de fazer alterações no banco de dados, você precisa atualizar o modelo do EF Core para refletir essas alterações. Se as alterações de banco de dados são simples, pode ser mais fácil alterar manualmente o modelo do EF Core. Por exemplo, renomear uma tabela ou coluna, remoção de uma coluna ou atualizando o tipo da coluna é triviais alterações a serem feitas no código.

Alterações mais significativas, no entanto, não são como tornar fácil manualmente. O modelo do banco de dados novamente usando um fluxo de trabalho comum é reverter a engenharia `-Force` (PMC) ou `--force` (CLI) para substituir o modelo existente com um atualizado.

Outro recurso normalmente solicitados é a capacidade de atualizar o modelo do banco de dados, preservando a personalização como renomeações, hierarquias de tipo, etc. Use o problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para acompanhar o progresso desse recurso.

> [!WARNING]
> Se a engenharia reversa de modelo do banco de dados novamente, quaisquer alterações feitas aos arquivos serão perdidas.
