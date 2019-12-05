---
title: Engenharia reversa-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824461"
---
# <a name="reverse-engineering"></a>Engenharia reversa

A engenharia reversa é o processo de classes de tipo de entidade scaffolding e uma classe DbContext baseada em um esquema de banco de dados. Ele pode ser executado usando o comando `Scaffold-DbContext` das ferramentas do console do Gerenciador de pacotes do EF Core (PMC) ou o comando `dotnet ef dbcontext scaffold` das ferramentas da CLI (interface de linha de comando) do .NET.

## <a name="installing"></a>Instalando o

Antes da engenharia reversa, você precisará instalar as [ferramentas PMC](xref:core/miscellaneous/cli/powershell) (somente Visual Studio) ou as [ferramentas da CLI](xref:core/miscellaneous/cli/dotnet). Consulte os links para obter detalhes.

Você também precisará instalar um provedor de [banco de dados](xref:core/providers/index) apropriado para o esquema de banco de dados do qual deseja fazer engenharia reversa.

## <a name="connection-string"></a>Cadeia de conexão

O primeiro argumento para o comando é uma cadeia de conexão para o banco de dados. As ferramentas usarão essa cadeia de conexão para ler o esquema de banco de dados.

A maneira de citar e escapar a cadeia de conexão depende de qual shell você está usando para executar o comando. Consulte a documentação do Shell para obter informações específicas. Por exemplo, o PowerShell exige que você escape o caractere `$`, mas não `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuração e segredos do usuário

Se você tiver um projeto ASP.NET Core, poderá usar a sintaxe `Name=<connection-string>` para ler a cadeia de conexão da configuração.

Isso funciona bem com a [ferramenta Gerenciador de segredo](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) para manter a senha do banco de dados separada da base de código.

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nome do provedor

O segundo argumento é o nome do provedor. O nome do provedor geralmente é o mesmo que o nome do pacote NuGet do provedor.

## <a name="specifying-tables"></a>Especificando tabelas

Todas as tabelas no esquema de banco de dados são revertidas com engenharia reversa em tipos de entidade por padrão. Você pode limitar quais tabelas são revertidas com engenharia reversa especificando esquemas e tabelas.

O parâmetro `-Schemas` no PMC e a opção `--schema` na CLI podem ser usados para incluir cada tabela em um esquema.

`-Tables` (PMC) e `--table` (CLI) podem ser usados para incluir tabelas específicas.

Para incluir várias tabelas no PMC, use uma matriz.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Para incluir várias tabelas na CLI, especifique a opção várias vezes.

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Preservando nomes

Os nomes de tabela e coluna são corrigidos para corresponder melhor às convenções de nomenclatura do .NET para tipos e propriedades por padrão. A especificação da opção de `-UseDatabaseNames` no PMC ou da `--use-database-names` na CLI desabilitará esse comportamento preservando os nomes de banco de dados originais o máximo possível. Identificadores do .NET inválidos ainda serão fixos e nomes sintetizados como propriedades de navegação ainda estarão em conformidade com as convenções de nomenclatura do .NET.

## <a name="fluent-api-or-data-annotations"></a>API fluente ou anotações de dados

Os tipos de entidade são configurados usando a API fluente por padrão. Especifique `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) para, em vez disso, usar as anotações de dados quando possível.

Por exemplo, usar a API fluente irá Scaffold isto:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Ao usar as anotações de dados, Scaffold:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nome do DbContext

O nome da classe DbContext com Scaffold será o nome do banco de dados com o *contexto* definido por padrão. Para especificar um diferente, use `-Context` em PMC e `--context` na CLI.

## <a name="directories-and-namespaces"></a>Diretórios e namespaces

As classes de entidade e uma classe DbContext são com Scaffold no diretório raiz do projeto e usam o namespace padrão do projeto. Você pode especificar o diretório em que as classes são com Scaffold usando `-OutputDir` (PMC) ou `--output-dir` (CLI). O namespace será o namespace raiz mais os nomes de qualquer subdiretório no diretório raiz do projeto.

Você também pode usar `-ContextDir` (PMC) e `--context-dir` (CLI) para Scaffold a classe DbContext em um diretório separado das classes de tipo de entidade.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Como funciona

A engenharia reversa começa lendo o esquema de banco de dados. Ele lê informações sobre tabelas, colunas, restrições e índices.

Em seguida, ele usa as informações de esquema para criar um modelo de EF Core. As tabelas são usadas para criar tipos de entidade; as colunas são usadas para criar propriedades; e as chaves estrangeiras são usadas para criar relações.

Por fim, o modelo é usado para gerar código. As classes de tipo de entidade, a API fluente e as anotações de dados correspondentes são com Scaffold para recriar o mesmo modelo a partir de seu aplicativo.

## <a name="limitations"></a>Limitações

* Nem tudo sobre um modelo pode ser representado usando um esquema de banco de dados. Por exemplo, as informações sobre [**hierarquias de herança**](../modeling/inheritance.md), [**tipos de propriedade**](../modeling/owned-entities.md)e divisão de [**tabela**](../modeling/table-splitting.md) não estão presentes no esquema de banco de dados. Por isso, essas construções nunca serão revertidas com engenharia reversa.
* Além disso, **alguns tipos de coluna** podem não ser suportados pelo provedor de EF Core. Essas colunas não serão incluídas no modelo.
* Você pode definir [**tokens de simultaneidade**](../modeling/concurrency.md), em um modelo de EF Core para impedir que dois usuários atualizem a mesma entidade ao mesmo tempo. Alguns bancos de dados têm um tipo especial para representar esse tipo de coluna (por exemplo, de SQL Server) nesse caso, podemos reverter a engenharia dessas informações; no entanto, outros tokens de simultaneidade não terão engenharia reversa.
* [O C# recurso de tipo de referência anulável 8](/dotnet/csharp/tutorials/nullable-reference-types) não tem suporte no momento na engenharia reversa: C# EF Core sempre gera código que assume que o recurso está desabilitado. Por exemplo, colunas de texto anuláveis serão com Scaffold como uma propriedade com o tipo `string`, não `string?`, com a API Fluent ou as anotações de dados usadas para configurar se uma propriedade é necessária ou não. Você pode editar o código com Scaffold e substituí-los C# por anotações de nulidade. O suporte do scaffolding para tipos de referência anuláveis é acompanhado pelo problema [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).

## <a name="customizing-the-model"></a>Personalizando o modelo

O código gerado pelo EF Core é seu código. Fique à vontade para alterá-la. Ela só será regenerada se você reverter a engenharia do mesmo modelo novamente. O código com Scaffold representa *um* modelo que pode ser usado para acessar o banco de dados, mas certamente não é o *único* modelo que pode ser usado.

Personalize as classes de tipo de entidade e a classe DbContext para atender às suas necessidades. Por exemplo, você pode optar por renomear tipos e propriedades, introduzir hierarquias de herança ou dividir uma tabela em várias entidades. Você também pode remover índices não exclusivos, sequências não usadas e propriedades de navegação, Propriedades escalares opcionais e nomes de restrição do modelo.

Você também pode adicionar outros construtores, métodos, propriedades, etc. usando outra classe parcial em um arquivo separado. Essa abordagem funciona mesmo quando você pretende reverter a engenharia do modelo novamente.

## <a name="updating-the-model"></a>Atualizando o modelo

Depois de fazer alterações no banco de dados, talvez seja necessário atualizar seu modelo de EF Core para refletir essas alterações. Se as alterações no banco de dados forem simples, talvez seja mais fácil apenas fazer as alterações manualmente no modelo de EF Core. Por exemplo, renomear uma tabela ou coluna, remover uma coluna ou atualizar o tipo de uma coluna são alterações triviais a serem feitas no código.

No entanto, alterações mais significativas não são fáceis de fazer manualmente. Um fluxo de trabalho comum é reverter a engenharia do modelo do banco de dados novamente usando `-Force` (PMC) ou `--force` (CLI) para substituir o modelo existente por um atualizado.

Outro recurso comumente solicitado é a capacidade de atualizar o modelo do banco de dados enquanto preserva a personalização, como renomear, hierarquias de tipo, etc. Use o problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para acompanhar o progresso desse recurso.

> [!WARNING]
> Se você reverter a engenharia do modelo do banco de dados novamente, todas as alterações feitas nos arquivos serão perdidas.
