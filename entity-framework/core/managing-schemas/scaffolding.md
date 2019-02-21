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
# <a name="reverse-engineering"></a><span data-ttu-id="daa50-102">Engenharia reversa</span><span class="sxs-lookup"><span data-stu-id="daa50-102">Reverse Engineering</span></span>

<span data-ttu-id="daa50-103">Engenharia reversa é o processo de scaffolding de classes de tipo de entidade e uma classe DbContext com base em um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="daa50-104">Ele pode ser realizado usando o `Scaffold-DbContext` comando das ferramentas do EF Core pacote Manager Console (PMC) ou o `dotnet ef dbcontext scaffold` comando das ferramentas de Interface de linha de comando (CLI) do .NET.</span><span class="sxs-lookup"><span data-stu-id="daa50-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="daa50-105">Instalando o</span><span class="sxs-lookup"><span data-stu-id="daa50-105">Installing</span></span>

<span data-ttu-id="daa50-106">Antes de engenharia reversa, você precisará instalar o [ferramentas PMC](xref:core/miscellaneous/cli/powershell) (somente Visual Studio) ou o [ferramentas da CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="daa50-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="daa50-107">Consulte os links para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="daa50-107">See links for details.</span></span>

<span data-ttu-id="daa50-108">Você também precisará instalar um apropriado [provedor de banco de dados](xref:core/providers/index) para o esquema de banco de dados que você deseja fazer engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="daa50-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="daa50-109">Cadeia de caracteres de Conexão</span><span class="sxs-lookup"><span data-stu-id="daa50-109">Connection string</span></span>

<span data-ttu-id="daa50-110">O primeiro argumento para o comando é uma cadeia de caracteres de conexão ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="daa50-111">As ferramentas usará essa cadeia de caracteres de conexão para ler o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="daa50-112">Como você pode colocar entre aspas e escapar a cadeia de caracteres de conexão depende do shell do qual você está usando para executar o comando.</span><span class="sxs-lookup"><span data-stu-id="daa50-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="daa50-113">Consulte a documentação do seu shell para obter informações específicas.</span><span class="sxs-lookup"><span data-stu-id="daa50-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="daa50-114">Por exemplo, o PowerShell requer escapar o `$` de caractere, mas não `\`.</span><span class="sxs-lookup"><span data-stu-id="daa50-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="daa50-115">Configuração e os segredos do usuário</span><span class="sxs-lookup"><span data-stu-id="daa50-115">Configuration and User Secrets</span></span>

<span data-ttu-id="daa50-116">Se você tiver um projeto ASP.NET Core, você pode usar o `Name=<connection-string>` sintaxe para ler a cadeia de caracteres de conexão de configuração.</span><span class="sxs-lookup"><span data-stu-id="daa50-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="daa50-117">Isso funciona bem com o [ferramenta Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) para manter sua senha do banco de dados separado de sua base de código.</span><span class="sxs-lookup"><span data-stu-id="daa50-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="daa50-118">Nome do provedor</span><span class="sxs-lookup"><span data-stu-id="daa50-118">Provider name</span></span>

<span data-ttu-id="daa50-119">O segundo argumento é o nome do provedor.</span><span class="sxs-lookup"><span data-stu-id="daa50-119">The second argument is the provider name.</span></span> <span data-ttu-id="daa50-120">O nome do provedor normalmente é o mesmo nome do pacote NuGet do provedor.</span><span class="sxs-lookup"><span data-stu-id="daa50-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="daa50-121">Especificação de tabelas</span><span class="sxs-lookup"><span data-stu-id="daa50-121">Specifying tables</span></span>

<span data-ttu-id="daa50-122">Todas as tabelas no esquema de banco de dados engenharia reversos eles em tipos de entidade por padrão.</span><span class="sxs-lookup"><span data-stu-id="daa50-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="daa50-123">Você pode limitar quais tabelas engenharia reversos, eles especificando os esquemas e tabelas.</span><span class="sxs-lookup"><span data-stu-id="daa50-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="daa50-124">O `-Schemas` parâmetro no PMC e o `--schema` opção na CLI pode ser usada para incluir todas as tabelas em um esquema.</span><span class="sxs-lookup"><span data-stu-id="daa50-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="daa50-125">`-Tables` (PMC) e `--table` (CLI) pode ser usada para incluir tabelas específicas.</span><span class="sxs-lookup"><span data-stu-id="daa50-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="daa50-126">Para incluir várias tabelas no PMC, use uma matriz.</span><span class="sxs-lookup"><span data-stu-id="daa50-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="daa50-127">Para incluir várias tabelas na CLI, especifique a opção várias vezes.</span><span class="sxs-lookup"><span data-stu-id="daa50-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="daa50-128">Preserva nomes</span><span class="sxs-lookup"><span data-stu-id="daa50-128">Preserving names</span></span>

<span data-ttu-id="daa50-129">Nomes de tabela e coluna são corrigidos para que corresponda melhor às convenções de nomenclatura .NET para tipos e propriedades por padrão.</span><span class="sxs-lookup"><span data-stu-id="daa50-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="daa50-130">Especificando o `-UseDatabaseNames` PMC de inserção ou a `--use-database-names` opção na CLI desabilitará esse comportamento preserva os nomes de banco de dados original tanto quanto possível.</span><span class="sxs-lookup"><span data-stu-id="daa50-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="daa50-131">Identificadores inválidos de .NET ainda serão corrigidos e sintetizados nomes como propriedades de navegação ainda estará em conformidade com as convenções de nomenclatura do .NET.</span><span class="sxs-lookup"><span data-stu-id="daa50-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="daa50-132">API Fluent ou anotações de dados</span><span class="sxs-lookup"><span data-stu-id="daa50-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="daa50-133">Tipos de entidade são configurados usando a API Fluent por padrão.</span><span class="sxs-lookup"><span data-stu-id="daa50-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="daa50-134">Especificar `-DataAnnotations` (PMC) ou `--data-annotations` (CLI) em vez disso, usar anotações de dados sempre que possível.</span><span class="sxs-lookup"><span data-stu-id="daa50-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="daa50-135">Por exemplo, usando a API Fluent irá dar suporte isso:</span><span class="sxs-lookup"><span data-stu-id="daa50-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="daa50-136">Ao usar anotações de dados irá dar suporte isso:</span><span class="sxs-lookup"><span data-stu-id="daa50-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="daa50-137">Nome de DbContext</span><span class="sxs-lookup"><span data-stu-id="daa50-137">DbContext name</span></span>

<span data-ttu-id="daa50-138">O nome da classe DbContext gerado por scaffolding será o nome do banco de dados com o sufixo *contexto* por padrão.</span><span class="sxs-lookup"><span data-stu-id="daa50-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="daa50-139">Para especificar o outra, use `-Context` no PMC e `--context` na CLI.</span><span class="sxs-lookup"><span data-stu-id="daa50-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="daa50-140">Diretórios e namespaces</span><span class="sxs-lookup"><span data-stu-id="daa50-140">Directories and namespaces</span></span>

<span data-ttu-id="daa50-141">As classes de entidade e uma classe DbContext são gerados automaticamente no diretório raiz do projeto e usam o namespace do projeto padrão.</span><span class="sxs-lookup"><span data-stu-id="daa50-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="daa50-142">Você pode especificar o diretório em que as classes são gerados automaticamente usando `-OutputDir` (PMC) ou `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="daa50-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="daa50-143">O namespace será o namespace raiz além dos nomes de todos os subdiretórios no diretório raiz do projeto.</span><span class="sxs-lookup"><span data-stu-id="daa50-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="daa50-144">Você também pode usar `-ContextDir` (PMC) e `--context-dir` (CLI) para gerar automaticamente a classe DbContext em um diretório separado das classes de tipo de entidade.</span><span class="sxs-lookup"><span data-stu-id="daa50-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="daa50-145">Como ele funciona</span><span class="sxs-lookup"><span data-stu-id="daa50-145">How it works</span></span>

<span data-ttu-id="daa50-146">Engenharia reversa começa lendo o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="daa50-147">Ele lê as informações sobre tabelas, colunas, restrições e índices.</span><span class="sxs-lookup"><span data-stu-id="daa50-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="daa50-148">Em seguida, ele usa as informações de esquema para criar um modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="daa50-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="daa50-149">Tabelas são usadas para criar tipos de entidade; colunas são usadas para criar propriedades; e chaves estrangeiras são usadas para criar relações.</span><span class="sxs-lookup"><span data-stu-id="daa50-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="daa50-150">Por fim, o modelo é usado para gerar código.</span><span class="sxs-lookup"><span data-stu-id="daa50-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="daa50-151">Os correspondente entidade classes, a API Fluent e dados anotações de tipo são gerados automaticamente para recriar o mesmo modelo de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="daa50-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="daa50-152">O que não funciona</span><span class="sxs-lookup"><span data-stu-id="daa50-152">What doesn't work</span></span>

<span data-ttu-id="daa50-153">Nem tudo sobre um modelo pode ser representado usando um esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="daa50-154">Por exemplo, informações sobre **hierarquias de herança**, **tipos próprios**, e **divisão de tabela** não estão presentes no esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="daa50-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="daa50-155">Por isso, essas construções serão nunca ser revertida.</span><span class="sxs-lookup"><span data-stu-id="daa50-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="daa50-156">Além disso, **alguns tipos de coluna** podem não ter suporte pelo provedor do EF Core.</span><span class="sxs-lookup"><span data-stu-id="daa50-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="daa50-157">Essas colunas não incluídas no modelo.</span><span class="sxs-lookup"><span data-stu-id="daa50-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="daa50-158">O EF Core exige que cada tipo de entidade ter uma chave.</span><span class="sxs-lookup"><span data-stu-id="daa50-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="daa50-159">Tabelas, no entanto, não é necessário especificar uma chave primária.</span><span class="sxs-lookup"><span data-stu-id="daa50-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="daa50-160">**Tabelas sem uma chave primária** estão no momento, não com engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="daa50-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="daa50-161">Você pode definir **tokens de simultaneidade** em um modelo do EF Core para impedir que os dois usuários atualizando a mesma entidade ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="daa50-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="daa50-162">Alguns bancos de dados têm um tipo especial para representar este tipo de coluna (por exemplo, rowversion no SQL Server) no caso, é possível reverter a engenharia essas informações; No entanto, outros tokens de simultaneidade será não ser a engenharia reversa.</span><span class="sxs-lookup"><span data-stu-id="daa50-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="daa50-163">Personalizando o modelo</span><span class="sxs-lookup"><span data-stu-id="daa50-163">Customizing the model</span></span>

<span data-ttu-id="daa50-164">O código gerado pelo EF Core é seu código.</span><span class="sxs-lookup"><span data-stu-id="daa50-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="daa50-165">Fique à vontade para alterá-lo.</span><span class="sxs-lookup"><span data-stu-id="daa50-165">Feel free to change it.</span></span> <span data-ttu-id="daa50-166">Ele só será registrado se você fazer engenharia reversa do mesmo modelo novamente.</span><span class="sxs-lookup"><span data-stu-id="daa50-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="daa50-167">Representa o código gerado por scaffolding *uma* modelo que pode ser usado para acessar o banco de dados, mas certamente não é o *somente* modelo que pode ser usado.</span><span class="sxs-lookup"><span data-stu-id="daa50-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="daa50-168">Personalize as classes de tipo de entidade e classe DbContext para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="daa50-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="daa50-169">Por exemplo, você pode optar por renomear tipos e propriedades, introduzir a hierarquias de herança ou dividir uma tabela em várias entidades.</span><span class="sxs-lookup"><span data-stu-id="daa50-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="daa50-170">Você também pode remover os índices não exclusivos, sequências não utilizadas e propriedades de navegação, opcionais propriedades escalares e nomes de restrição do modelo.</span><span class="sxs-lookup"><span data-stu-id="daa50-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="daa50-171">Você também pode adicionar mais construtores, métodos, propriedades, etc.</span><span class="sxs-lookup"><span data-stu-id="daa50-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="daa50-172">usando outra classe parcial em um arquivo separado.</span><span class="sxs-lookup"><span data-stu-id="daa50-172">using another partial class in a separate file.</span></span> <span data-ttu-id="daa50-173">Essa abordagem funciona mesmo quando você pretende fazer engenharia reversa o modelo novamente.</span><span class="sxs-lookup"><span data-stu-id="daa50-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="daa50-174">Atualizando o modelo</span><span class="sxs-lookup"><span data-stu-id="daa50-174">Updating the model</span></span>

<span data-ttu-id="daa50-175">Depois de fazer alterações no banco de dados, você precisa atualizar o modelo do EF Core para refletir essas alterações.</span><span class="sxs-lookup"><span data-stu-id="daa50-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="daa50-176">Se as alterações de banco de dados são simples, pode ser mais fácil alterar manualmente o modelo do EF Core.</span><span class="sxs-lookup"><span data-stu-id="daa50-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="daa50-177">Por exemplo, renomear uma tabela ou coluna, remoção de uma coluna ou atualizando o tipo da coluna é triviais alterações a serem feitas no código.</span><span class="sxs-lookup"><span data-stu-id="daa50-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="daa50-178">Alterações mais significativas, no entanto, não são como tornar fácil manualmente.</span><span class="sxs-lookup"><span data-stu-id="daa50-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="daa50-179">O modelo do banco de dados novamente usando um fluxo de trabalho comum é reverter a engenharia `-Force` (PMC) ou `--force` (CLI) para substituir o modelo existente com um atualizado.</span><span class="sxs-lookup"><span data-stu-id="daa50-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="daa50-180">Outro recurso normalmente solicitados é a capacidade de atualizar o modelo do banco de dados, preservando a personalização como renomeações, hierarquias de tipo, etc. Use o problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para acompanhar o progresso desse recurso.</span><span class="sxs-lookup"><span data-stu-id="daa50-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="daa50-181">Se a engenharia reversa de modelo do banco de dados novamente, quaisquer alterações feitas aos arquivos serão perdidas.</span><span class="sxs-lookup"><span data-stu-id="daa50-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
