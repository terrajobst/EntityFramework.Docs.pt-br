---
title: Cadeias de caracteres de Conexão e modelos - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: dce414ea84f13235691abf0dcadef5c743d90f9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994953"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="5efd5-102">Modelos e cadeias de caracteres de Conexão</span><span class="sxs-lookup"><span data-stu-id="5efd5-102">Connection strings and models</span></span>
<span data-ttu-id="5efd5-103">Este tópico aborda como o Entity Framework descobre qual conexão de banco de dados para usar e como você pode alterá-la.</span><span class="sxs-lookup"><span data-stu-id="5efd5-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="5efd5-104">Modelos criados com o Code First e o EF Designer são abordados neste tópico.</span><span class="sxs-lookup"><span data-stu-id="5efd5-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="5efd5-105">Normalmente, um aplicativo do Entity Framework usa uma classe derivada de DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="5efd5-106">Essa classe derivada chamará um dos construtores na classe base DbContext ao controle:</span><span class="sxs-lookup"><span data-stu-id="5efd5-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="5efd5-107">Como o contexto se conectará ao banco de dados — ou seja, como uma cadeia de caracteres de conexão é usado/encontrado</span><span class="sxs-lookup"><span data-stu-id="5efd5-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="5efd5-108">Se usará o contexto de calcular um modelo usando o Code First ou carregar um modelo criado com o EF Designer</span><span class="sxs-lookup"><span data-stu-id="5efd5-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="5efd5-109">Opções avançadas</span><span class="sxs-lookup"><span data-stu-id="5efd5-109">Additional advanced options</span></span>  

<span data-ttu-id="5efd5-110">Os fragmentos a seguir mostram que algumas maneiras dos construtores DbContext podem ser usadas.</span><span class="sxs-lookup"><span data-stu-id="5efd5-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="5efd5-111">Usar o Code First com conexão por convenção</span><span class="sxs-lookup"><span data-stu-id="5efd5-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="5efd5-112">Se você não tiver feito qualquer outra configuração em seu aplicativo, em seguida, chamar o construtor sem parâmetros em DbContext fará com que DbContext para executar no modo de Code First com uma conexão de banco de dados criado por convenção.</span><span class="sxs-lookup"><span data-stu-id="5efd5-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="5efd5-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="5efd5-114">Neste exemplo DbContext usa o nome qualificado de namespace de seu contexto derivado de class—Demo.EF.BloggingContext—as o nome do banco de dados e cria uma cadeia de conexão para este banco de dados usando o SQL Express ou LocalDB.</span><span class="sxs-lookup"><span data-stu-id="5efd5-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="5efd5-115">Se ambos estiverem instaladas, o SQL Express será usado.</span><span class="sxs-lookup"><span data-stu-id="5efd5-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="5efd5-116">Visual Studio 2010 inclui o SQL Express por padrão e o Visual Studio 2012 e posterior inclui LocalDB.</span><span class="sxs-lookup"><span data-stu-id="5efd5-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="5efd5-117">Durante a instalação, o pacote EntityFramework NuGet verifica qual servidor de banco de dados está disponível.</span><span class="sxs-lookup"><span data-stu-id="5efd5-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="5efd5-118">O pacote do NuGet, em seguida, atualizará o arquivo de configuração, definindo o servidor de banco de dados padrão que usa o Code First durante a criação de uma conexão por convenção.</span><span class="sxs-lookup"><span data-stu-id="5efd5-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="5efd5-119">Se estiver executando o SQL Express, ele será usado.</span><span class="sxs-lookup"><span data-stu-id="5efd5-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="5efd5-120">Se o SQL Express não está disponível, em seguida, LocalDB será registrado como o padrão em vez disso.</span><span class="sxs-lookup"><span data-stu-id="5efd5-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="5efd5-121">Nenhuma alteração é feita ao arquivo de configuração, se ela já contém uma configuração para a fábrica de conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="5efd5-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="5efd5-122">Usar o Code First com conexão por convenção e o nome do banco de dados especificado</span><span class="sxs-lookup"><span data-stu-id="5efd5-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="5efd5-123">Se você não tiver feito qualquer outra configuração em seu aplicativo, em seguida, chamar o construtor de cadeia de caracteres no DbContext com o nome do banco de dados que você deseja usar fará com que DbContext para executar no modo de Code First com uma conexão de banco de dados criado por convenção, o banco de dados de Esse nome.</span><span class="sxs-lookup"><span data-stu-id="5efd5-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="5efd5-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="5efd5-125">Neste exemplo DbContext usa "BloggingDatabase" como o nome do banco de dados e cria uma cadeia de conexão para este banco de dados usando o SQL Express (instalado com o Visual Studio 2010) ou LocalDB (instalado com o Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="5efd5-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="5efd5-126">Se ambos estiverem instaladas, o SQL Express será usado.</span><span class="sxs-lookup"><span data-stu-id="5efd5-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="5efd5-127">Usar o Code First com a cadeia de caracteres de conexão no arquivo app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="5efd5-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="5efd5-128">Você pode optar por colocar uma cadeia de caracteres de conexão em seu arquivo App. config ou Web. config.</span><span class="sxs-lookup"><span data-stu-id="5efd5-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="5efd5-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="5efd5-130">Isso é uma maneira fácil de informar ao DbContext para usar um servidor de banco de dados diferente do SQL Express ou LocalDB — o exemplo anterior Especifica um banco de dados do SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="5efd5-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="5efd5-131">Se o nome da cadeia de caracteres de conexão corresponder ao nome de seu contexto (com ou sem qualificação de namespace), em seguida, ele será encontrado pelo DbContext quando o construtor sem parâmetros é usado.</span><span class="sxs-lookup"><span data-stu-id="5efd5-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="5efd5-132">Se o nome de cadeia de caracteres de conexão é diferente do nome de seu contexto, em seguida, você pode dizer DbContext para usar essa conexão em modo de Code First, passando o nome de cadeia de caracteres de conexão para o construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="5efd5-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="5efd5-134">Como alternativa, você pode usar o formulário "nome =\<nome da cadeia de conexão\>" para a cadeia de caracteres passada para o construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="5efd5-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="5efd5-136">Essa forma faz explícita que você espera que a cadeia de caracteres de conexão a ser encontrado no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="5efd5-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="5efd5-137">Uma exceção será lançada se uma cadeia de caracteres de conexão com o nome fornecido não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="5efd5-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="5efd5-138">Banco de dados/Model First com a cadeia de caracteres de conexão no arquivo app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="5efd5-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="5efd5-139">Modelos criados com o EF Designer são diferentes de Code First em que seu modelo já existe e não é gerado a partir do código quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="5efd5-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="5efd5-140">O modelo normalmente existe como um arquivo EDMX em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5efd5-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="5efd5-141">O designer adicionará uma cadeia de caracteres de conexão de EF ao seu arquivo App. config ou Web. config.</span><span class="sxs-lookup"><span data-stu-id="5efd5-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="5efd5-142">Essa cadeia de caracteres de conexão é especial que contém informações sobre como localizar as informações em seu arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="5efd5-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="5efd5-143">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="5efd5-144">O EF Designer também irá gerar o código que informa ao DbContext para usar essa conexão, passando o nome de cadeia de caracteres de conexão para o construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="5efd5-145">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5efd5-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="5efd5-146">O DbContext sabe para carregar o modelo existente (em vez de usar o Code First para calculá-lo a partir do código) porque a cadeia de caracteres de conexão é uma cadeia de caracteres de conexão de EF que contém detalhes do modelo para usar.</span><span class="sxs-lookup"><span data-stu-id="5efd5-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="5efd5-147">Outras opções do construtor DbContext</span><span class="sxs-lookup"><span data-stu-id="5efd5-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="5efd5-148">A classe DbContext contém outros construtores e os padrões de uso que permitem alguns cenários mais avançados.</span><span class="sxs-lookup"><span data-stu-id="5efd5-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="5efd5-149">Alguns deles são:</span><span class="sxs-lookup"><span data-stu-id="5efd5-149">Some of these are:</span></span>  

- <span data-ttu-id="5efd5-150">Você pode usar a classe DbModelBuilder para criar um modelo Code First sem instanciar uma instância de DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="5efd5-151">O resultado disso é um objeto de DbModel.</span><span class="sxs-lookup"><span data-stu-id="5efd5-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="5efd5-152">Você pode passar esse objeto DbModel para um dos construtores DbContext quando estiver pronto para criar a instância de DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="5efd5-153">Você pode passar uma cadeia de caracteres de conexão completa para o DbContext em vez de apenas o nome de cadeia de caracteres de conexão ou de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5efd5-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="5efd5-154">Por padrão, essa cadeia de caracteres de conexão é usada com o provedor de.NET; Isso pode ser alterado definindo uma implementação diferente de IConnectionFactory no contexto. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="5efd5-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="5efd5-155">Você pode usar um objeto DbConnection existente, passando-o para um construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="5efd5-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="5efd5-156">Se o objeto de conexão é uma instância de EntityConnection, em seguida, o modelo especificado na conexão será usado em vez de calcular um modelo usando Code First.</span><span class="sxs-lookup"><span data-stu-id="5efd5-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="5efd5-157">Se o objeto é uma instância de algum outro tipo — por exemplo, SqlConnection — e em seguida, o contexto de usá-lo para o modo de Code First.</span><span class="sxs-lookup"><span data-stu-id="5efd5-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="5efd5-158">Você pode passar um ObjectContext existente para um construtor DbContext para criar um DbContext quebra automática de contexto existente.</span><span class="sxs-lookup"><span data-stu-id="5efd5-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="5efd5-159">Isso pode ser usado para aplicativos existentes que usam o ObjectContext, mas que desejam tirar proveito do DbContext em algumas partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5efd5-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
