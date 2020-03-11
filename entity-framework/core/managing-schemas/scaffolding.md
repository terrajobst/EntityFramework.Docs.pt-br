---
title: Engenharia reversa-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416759"
---
# <a name="reverse-engineering"></a><span data-ttu-id="367f2-102">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="367f2-102">Reverse Engineering</span></span>

<span data-ttu-id="367f2-103">A engenharia reversa é o processo de classes de tipo de entidade scaffolding e uma classe DbContext baseada em um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="367f2-104">Ele pode ser executado usando o comando `Scaffold-DbContext` das ferramentas do console do Gerenciador de pacotes do EF Core (PMC) ou o comando `dotnet ef dbcontext scaffold` das ferramentas da CLI (interface de linha de comando) do .NET.</span><span class="sxs-lookup"><span data-stu-id="367f2-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="367f2-105">Instalando</span><span class="sxs-lookup"><span data-stu-id="367f2-105">Installing</span></span>

<span data-ttu-id="367f2-106">Antes da engenharia reversa, você precisará instalar as [ferramentas PMC](xref:core/miscellaneous/cli/powershell) (somente Visual Studio) ou as [ferramentas da CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="367f2-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="367f2-107">Consulte os links para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="367f2-107">See links for details.</span></span>

<span data-ttu-id="367f2-108">Você também precisará instalar um provedor de [banco de dados](xref:core/providers/index) apropriado para o esquema de banco de dados do qual deseja fazer engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="367f2-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="367f2-109">Cadeia de conexão</span><span class="sxs-lookup"><span data-stu-id="367f2-109">Connection string</span></span>

<span data-ttu-id="367f2-110">O primeiro argumento para o comando é uma cadeia de conexão para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="367f2-111">As ferramentas usarão essa cadeia de conexão para ler o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="367f2-112">A maneira de citar e escapar a cadeia de conexão depende de qual shell você está usando para executar o comando.</span><span class="sxs-lookup"><span data-stu-id="367f2-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="367f2-113">Consulte a documentação do Shell para obter informações específicas.</span><span class="sxs-lookup"><span data-stu-id="367f2-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="367f2-114">Por exemplo, o PowerShell exige que você escape o caractere `$`, mas não `\`.</span><span class="sxs-lookup"><span data-stu-id="367f2-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="367f2-115">Configuração e segredos do usuário</span><span class="sxs-lookup"><span data-stu-id="367f2-115">Configuration and User Secrets</span></span>

<span data-ttu-id="367f2-116">Se você tiver um projeto ASP.NET Core, poderá usar a sintaxe `Name=<connection-string>` para ler a cadeia de conexão da configuração.</span><span class="sxs-lookup"><span data-stu-id="367f2-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="367f2-117">Isso funciona bem com a [ferramenta Gerenciador de segredo](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) para manter a senha do banco de dados separada da base de código.</span><span class="sxs-lookup"><span data-stu-id="367f2-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="367f2-118">Nome do provedor</span><span class="sxs-lookup"><span data-stu-id="367f2-118">Provider name</span></span>

<span data-ttu-id="367f2-119">O segundo argumento é o nome do provedor.</span><span class="sxs-lookup"><span data-stu-id="367f2-119">The second argument is the provider name.</span></span> <span data-ttu-id="367f2-120">O nome do provedor geralmente é o mesmo que o nome do pacote NuGet do provedor.</span><span class="sxs-lookup"><span data-stu-id="367f2-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="367f2-121">Especificando tabelas</span><span class="sxs-lookup"><span data-stu-id="367f2-121">Specifying tables</span></span>

<span data-ttu-id="367f2-122">Todas as tabelas no esquema de banco de dados são revertidas com engenharia reversa em tipos de entidade por padrão.</span><span class="sxs-lookup"><span data-stu-id="367f2-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="367f2-123">Você pode limitar quais tabelas são revertidas com engenharia reversa especificando esquemas e tabelas.</span><span class="sxs-lookup"><span data-stu-id="367f2-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="367f2-124">O parâmetro `-Schemas` no PMC e a opção `--schema` na CLI podem ser usados para incluir cada tabela em um esquema.</span><span class="sxs-lookup"><span data-stu-id="367f2-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="367f2-125">`-Tables` (PMC) e `--table` (CLI) podem ser usados para incluir tabelas específicas.</span><span class="sxs-lookup"><span data-stu-id="367f2-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="367f2-126">Para incluir várias tabelas no PMC, use uma matriz.</span><span class="sxs-lookup"><span data-stu-id="367f2-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="367f2-127">Para incluir várias tabelas na CLI, especifique a opção várias vezes.</span><span class="sxs-lookup"><span data-stu-id="367f2-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="367f2-128">Preservando nomes</span><span class="sxs-lookup"><span data-stu-id="367f2-128">Preserving names</span></span>

<span data-ttu-id="367f2-129">Os nomes de tabela e coluna são corrigidos para corresponder melhor às convenções de nomenclatura do .NET para tipos e propriedades por padrão.</span><span class="sxs-lookup"><span data-stu-id="367f2-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="367f2-130">A especificação da opção de `-UseDatabaseNames` no PMC ou da `--use-database-names` na CLI desabilitará esse comportamento preservando os nomes de banco de dados originais o máximo possível.</span><span class="sxs-lookup"><span data-stu-id="367f2-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="367f2-131">Identificadores do .NET inválidos ainda serão fixos e nomes sintetizados como propriedades de navegação ainda estarão em conformidade com as convenções de nomenclatura do .NET.</span><span class="sxs-lookup"><span data-stu-id="367f2-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="367f2-132">API fluente ou anotações de dados</span><span class="sxs-lookup"><span data-stu-id="367f2-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="367f2-133">Os tipos de entidade são configurados usando a API fluente por padrão.</span><span class="sxs-lookup"><span data-stu-id="367f2-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="367f2-134">Especifique `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) para, em vez disso, usar as anotações de dados quando possível.</span><span class="sxs-lookup"><span data-stu-id="367f2-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="367f2-135">Por exemplo, usar a API fluente irá Scaffold isto:</span><span class="sxs-lookup"><span data-stu-id="367f2-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="367f2-136">Ao usar as anotações de dados, Scaffold:</span><span class="sxs-lookup"><span data-stu-id="367f2-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="367f2-137">Nome do DbContext</span><span class="sxs-lookup"><span data-stu-id="367f2-137">DbContext name</span></span>

<span data-ttu-id="367f2-138">O nome da classe DbContext com Scaffold será o nome do banco de dados com o *contexto* definido por padrão.</span><span class="sxs-lookup"><span data-stu-id="367f2-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="367f2-139">Para especificar um diferente, use `-Context` em PMC e `--context` na CLI.</span><span class="sxs-lookup"><span data-stu-id="367f2-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="367f2-140">Diretórios e namespaces</span><span class="sxs-lookup"><span data-stu-id="367f2-140">Directories and namespaces</span></span>

<span data-ttu-id="367f2-141">As classes de entidade e uma classe DbContext são com Scaffold no diretório raiz do projeto e usam o namespace padrão do projeto.</span><span class="sxs-lookup"><span data-stu-id="367f2-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="367f2-142">Você pode especificar o diretório em que as classes são com Scaffold usando `-OutputDir` (PMC) ou `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="367f2-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="367f2-143">O namespace será o namespace raiz mais os nomes de qualquer subdiretório no diretório raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="367f2-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="367f2-144">Você também pode usar `-ContextDir` (PMC) e `--context-dir` (CLI) para Scaffold a classe DbContext em um diretório separado das classes de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="367f2-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="367f2-145">Como ele funciona</span><span class="sxs-lookup"><span data-stu-id="367f2-145">How it works</span></span>

<span data-ttu-id="367f2-146">A engenharia reversa começa lendo o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="367f2-147">Ele lê informações sobre tabelas, colunas, restrições e índices.</span><span class="sxs-lookup"><span data-stu-id="367f2-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="367f2-148">Em seguida, ele usa as informações de esquema para criar um modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="367f2-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="367f2-149">As tabelas são usadas para criar tipos de entidade; as colunas são usadas para criar propriedades; e as chaves estrangeiras são usadas para criar relações.</span><span class="sxs-lookup"><span data-stu-id="367f2-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="367f2-150">Por fim, o modelo é usado para gerar código.</span><span class="sxs-lookup"><span data-stu-id="367f2-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="367f2-151">As classes de tipo de entidade, a API fluente e as anotações de dados correspondentes são com Scaffold para recriar o mesmo modelo a partir de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="367f2-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="367f2-152">Limitações</span><span class="sxs-lookup"><span data-stu-id="367f2-152">Limitations</span></span>

* <span data-ttu-id="367f2-153">Nem tudo sobre um modelo pode ser representado usando um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="367f2-154">Por exemplo, as informações sobre [**hierarquias de herança**](../modeling/inheritance.md), [**tipos de propriedade**](../modeling/owned-entities.md)e divisão de [**tabela**](../modeling/table-splitting.md) não estão presentes no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="367f2-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="367f2-155">Por isso, essas construções nunca serão revertidas com engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="367f2-155">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="367f2-156">Além disso, **alguns tipos de coluna** podem não ser suportados pelo provedor de EF Core.</span><span class="sxs-lookup"><span data-stu-id="367f2-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="367f2-157">Essas colunas não serão incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="367f2-157">These columns won't be included in the model.</span></span>
* <span data-ttu-id="367f2-158">Você pode definir [**tokens de simultaneidade**](../modeling/concurrency.md), em um modelo de EF Core para impedir que dois usuários atualizem a mesma entidade ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="367f2-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="367f2-159">Alguns bancos de dados têm um tipo especial para representar esse tipo de coluna (por exemplo, de SQL Server) nesse caso, podemos reverter a engenharia dessas informações; no entanto, outros tokens de simultaneidade não terão engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="367f2-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="367f2-160">[O C# recurso de tipo de referência anulável 8](/dotnet/csharp/tutorials/nullable-reference-types) não tem suporte no momento na engenharia reversa: C# EF Core sempre gera código que assume que o recurso está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="367f2-160">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="367f2-161">Por exemplo, colunas de texto anuláveis serão com Scaffold como uma propriedade com o tipo `string`, não `string?`, com a API Fluent ou as anotações de dados usadas para configurar se uma propriedade é necessária ou não.</span><span class="sxs-lookup"><span data-stu-id="367f2-161">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="367f2-162">Você pode editar o código com Scaffold e substituí-los C# por anotações de nulidade.</span><span class="sxs-lookup"><span data-stu-id="367f2-162">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="367f2-163">O suporte do scaffolding para tipos de referência anuláveis é acompanhado pelo problema [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span><span class="sxs-lookup"><span data-stu-id="367f2-163">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="367f2-164">Personalizando o modelo</span><span class="sxs-lookup"><span data-stu-id="367f2-164">Customizing the model</span></span>

<span data-ttu-id="367f2-165">O código gerado pelo EF Core é seu código.</span><span class="sxs-lookup"><span data-stu-id="367f2-165">The code generated by EF Core is your code.</span></span> <span data-ttu-id="367f2-166">Fique à vontade para alterá-la.</span><span class="sxs-lookup"><span data-stu-id="367f2-166">Feel free to change it.</span></span> <span data-ttu-id="367f2-167">Ela só será regenerada se você reverter a engenharia do mesmo modelo novamente.</span><span class="sxs-lookup"><span data-stu-id="367f2-167">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="367f2-168">O código com Scaffold representa *um* modelo que pode ser usado para acessar o banco de dados, mas certamente não é o *único* modelo que pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="367f2-168">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="367f2-169">Personalize as classes de tipo de entidade e a classe DbContext para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="367f2-169">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="367f2-170">Por exemplo, você pode optar por renomear tipos e propriedades, introduzir hierarquias de herança ou dividir uma tabela em várias entidades.</span><span class="sxs-lookup"><span data-stu-id="367f2-170">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="367f2-171">Você também pode remover índices não exclusivos, sequências não usadas e propriedades de navegação, Propriedades escalares opcionais e nomes de restrição do modelo.</span><span class="sxs-lookup"><span data-stu-id="367f2-171">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="367f2-172">Você também pode adicionar outros construtores, métodos, propriedades, etc.</span><span class="sxs-lookup"><span data-stu-id="367f2-172">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="367f2-173">usando outra classe parcial em um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="367f2-173">using another partial class in a separate file.</span></span> <span data-ttu-id="367f2-174">Essa abordagem funciona mesmo quando você pretende reverter a engenharia do modelo novamente.</span><span class="sxs-lookup"><span data-stu-id="367f2-174">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="367f2-175">Atualizando o modelo</span><span class="sxs-lookup"><span data-stu-id="367f2-175">Updating the model</span></span>

<span data-ttu-id="367f2-176">Depois de fazer alterações no banco de dados, talvez seja necessário atualizar seu modelo de EF Core para refletir essas alterações.</span><span class="sxs-lookup"><span data-stu-id="367f2-176">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="367f2-177">Se as alterações no banco de dados forem simples, talvez seja mais fácil apenas fazer as alterações manualmente no modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="367f2-177">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="367f2-178">Por exemplo, renomear uma tabela ou coluna, remover uma coluna ou atualizar o tipo de uma coluna são alterações triviais a serem feitas no código.</span><span class="sxs-lookup"><span data-stu-id="367f2-178">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="367f2-179">No entanto, alterações mais significativas não são fáceis de fazer manualmente.</span><span class="sxs-lookup"><span data-stu-id="367f2-179">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="367f2-180">Um fluxo de trabalho comum é reverter a engenharia do modelo do banco de dados novamente usando `-Force` (PMC) ou `--force` (CLI) para substituir o modelo existente por um atualizado.</span><span class="sxs-lookup"><span data-stu-id="367f2-180">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="367f2-181">Outro recurso comumente solicitado é a capacidade de atualizar o modelo do banco de dados enquanto preserva a personalização, como renomear, hierarquias de tipo, etc. Use o problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para acompanhar o progresso desse recurso.</span><span class="sxs-lookup"><span data-stu-id="367f2-181">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="367f2-182">Se você reverter a engenharia do modelo do banco de dados novamente, todas as alterações feitas nos arquivos serão perdidas.</span><span class="sxs-lookup"><span data-stu-id="367f2-182">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
