---
title: Cadeias de conexão e modelos-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419293"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="602c7-102">Cadeias de conexão e modelos</span><span class="sxs-lookup"><span data-stu-id="602c7-102">Connection strings and models</span></span>
<span data-ttu-id="602c7-103">Este tópico aborda como Entity Framework descobre qual conexão de banco de dados usar e como você pode alterá-la.</span><span class="sxs-lookup"><span data-stu-id="602c7-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="602c7-104">Os modelos criados com Code First e o designer do EF são abordados neste tópico.</span><span class="sxs-lookup"><span data-stu-id="602c7-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="602c7-105">Normalmente, um aplicativo Entity Framework usa uma classe derivada de DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="602c7-106">Essa classe derivada chamará um dos construtores na classe base DbContext a ser controlada:</span><span class="sxs-lookup"><span data-stu-id="602c7-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="602c7-107">Como o contexto se conectará a um banco de dados — ou seja, como uma cadeia de conexão é encontrada/usada</span><span class="sxs-lookup"><span data-stu-id="602c7-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="602c7-108">Se o contexto usará calcular um modelo usando Code First ou carregar um modelo criado com o designer do EF</span><span class="sxs-lookup"><span data-stu-id="602c7-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="602c7-109">Opções avançadas adicionais</span><span class="sxs-lookup"><span data-stu-id="602c7-109">Additional advanced options</span></span>  

<span data-ttu-id="602c7-110">Os fragmentos a seguir mostram algumas das maneiras como os construtores DbContext podem ser usados.</span><span class="sxs-lookup"><span data-stu-id="602c7-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="602c7-111">Usar Code First com conexão por convenção</span><span class="sxs-lookup"><span data-stu-id="602c7-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="602c7-112">Se você não tiver feito nenhuma outra configuração em seu aplicativo, chamar o construtor sem parâmetros no DbContext fará com que o DbContext seja executado no modo de Code First com uma conexão de banco de dados criada por convenção.</span><span class="sxs-lookup"><span data-stu-id="602c7-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="602c7-113">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-113">For example:</span></span>  

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

<span data-ttu-id="602c7-114">Neste exemplo, DbContext usa o nome qualificado do namespace da classe de contexto derivada — demo. EF. BloggingContext — como o nome do banco de dados e cria uma cadeia de conexão para esse banco de dados usando o SQL Express ou o LocalDB.</span><span class="sxs-lookup"><span data-stu-id="602c7-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="602c7-115">Se ambos estiverem instalados, o SQL Express será usado.</span><span class="sxs-lookup"><span data-stu-id="602c7-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="602c7-116">O Visual Studio 2010 inclui o SQL Express por padrão e o Visual Studio 2012 e posterior inclui o LocalDB.</span><span class="sxs-lookup"><span data-stu-id="602c7-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="602c7-117">Durante a instalação, o pacote NuGet do EntityFramework verifica qual servidor de banco de dados está disponível.</span><span class="sxs-lookup"><span data-stu-id="602c7-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="602c7-118">O pacote NuGet atualizará o arquivo de configuração definindo o servidor de banco de dados padrão que o Code First usa ao criar uma conexão por convenção.</span><span class="sxs-lookup"><span data-stu-id="602c7-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="602c7-119">Se o SQL Express estiver em execução, ele será usado.</span><span class="sxs-lookup"><span data-stu-id="602c7-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="602c7-120">Se o SQL Express não estiver disponível, o LocalDB será registrado como o padrão.</span><span class="sxs-lookup"><span data-stu-id="602c7-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="602c7-121">Nenhuma alteração será feita no arquivo de configuração se ele já contiver uma configuração para a fábrica de conexões padrão.</span><span class="sxs-lookup"><span data-stu-id="602c7-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="602c7-122">Usar Code First com conexão por convenção e nome de banco de dados especificado</span><span class="sxs-lookup"><span data-stu-id="602c7-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="602c7-123">Se você não tiver feito nenhuma outra configuração em seu aplicativo, chamar o construtor de cadeia de caracteres em DbContext com o nome do banco de dados que você deseja usar fará com que o DbContext seja executado no modo de Code First com uma conexão de banco de dados criada por convenção ao banco de dados de Esse nome.</span><span class="sxs-lookup"><span data-stu-id="602c7-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="602c7-124">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="602c7-125">Neste exemplo, DbContext usa "BloggingDatabase" como o nome do banco de dados e cria uma cadeia de conexão para esse banco de dados usando o SQL Express (instalado com o Visual Studio 2010) ou o LocalDB (instalado com o Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="602c7-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="602c7-126">Se ambos estiverem instalados, o SQL Express será usado.</span><span class="sxs-lookup"><span data-stu-id="602c7-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="602c7-127">Usar Code First com cadeia de conexão no arquivo app. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="602c7-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="602c7-128">Você pode optar por colocar uma cadeia de conexão em seu arquivo app. config ou Web. config.</span><span class="sxs-lookup"><span data-stu-id="602c7-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="602c7-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="602c7-130">Essa é uma maneira fácil de dizer ao DbContext para usar um servidor de banco de dados diferente do SQL Express ou LocalDB — o exemplo acima Especifica um banco de dados do SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="602c7-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="602c7-131">Se o nome da cadeia de conexão corresponder ao nome do seu contexto (com ou sem qualificação de namespace), ele será encontrado por DbContext quando o construtor sem parâmetros for usado.</span><span class="sxs-lookup"><span data-stu-id="602c7-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="602c7-132">Se o nome da cadeia de conexão for diferente do nome do seu contexto, você poderá dizer ao DbContext para usar essa conexão no modo de Code First passando o nome da cadeia de conexão para o Construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="602c7-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="602c7-134">Como alternativa, você pode usar a forma "Name =\<nome da cadeia de conexão\>" para a cadeia de caracteres passada para o Construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="602c7-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="602c7-136">Esse formulário torna explícito que você espera que a cadeia de conexão seja encontrada no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="602c7-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="602c7-137">Uma exceção será gerada se uma cadeia de conexão com o nome fornecido não for encontrada.</span><span class="sxs-lookup"><span data-stu-id="602c7-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="602c7-138">Banco de dados/Model First com cadeia de conexão no arquivo app. config/Web. config</span><span class="sxs-lookup"><span data-stu-id="602c7-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="602c7-139">Os modelos criados com o designer do EF são diferentes dos Code First em que o modelo já existe e não é gerado a partir do código quando o aplicativo é executado.</span><span class="sxs-lookup"><span data-stu-id="602c7-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="602c7-140">O modelo normalmente existe como um arquivo EDMX em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="602c7-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="602c7-141">O designer adicionará uma cadeia de conexão do EF ao arquivo app. config ou Web. config.</span><span class="sxs-lookup"><span data-stu-id="602c7-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="602c7-142">Essa cadeia de conexão é especial, pois contém informações sobre como localizar as informações em seu arquivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="602c7-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="602c7-143">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-143">For example:</span></span>  

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

<span data-ttu-id="602c7-144">O designer do EF também gerará código que informa ao DbContext para usar essa conexão, passando o nome da cadeia de conexão para o Construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="602c7-145">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="602c7-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="602c7-146">O DbContext sabe carregar o modelo existente (em vez de usar Code First para calculá-lo do código), pois a cadeia de conexão é uma cadeia de conexão do EF que contém detalhes do modelo a ser usado.</span><span class="sxs-lookup"><span data-stu-id="602c7-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="602c7-147">Outras opções do Construtor DbContext</span><span class="sxs-lookup"><span data-stu-id="602c7-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="602c7-148">A classe DbContext contém outros construtores e padrões de uso que habilitam Alguns cenários mais avançados.</span><span class="sxs-lookup"><span data-stu-id="602c7-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="602c7-149">Alguns deles são:</span><span class="sxs-lookup"><span data-stu-id="602c7-149">Some of these are:</span></span>  

- <span data-ttu-id="602c7-150">Você pode usar a classe DbModelBuilder para criar um modelo de Code First sem instanciar uma instância DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="602c7-151">O resultado disso é um objeto DbModel.</span><span class="sxs-lookup"><span data-stu-id="602c7-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="602c7-152">Em seguida, você pode passar esse objeto DbModel para um dos construtores DbContext quando estiver pronto para criar sua instância DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="602c7-153">Você pode passar uma cadeia de conexão completa para DbContext em vez de apenas o banco de dados ou o nome da cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="602c7-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="602c7-154">Por padrão, essa cadeia de conexão é usada com o provedor System. Data. SqlClient; Isso pode ser alterado definindo uma implementação diferente de IConnectionFactory no contexto. Database. DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="602c7-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="602c7-155">Você pode usar um objeto DbConnection existente passando-o para um Construtor DbContext.</span><span class="sxs-lookup"><span data-stu-id="602c7-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="602c7-156">Se o objeto de conexão for uma instância de EntityConnection, o modelo especificado na conexão será usado em vez de calcular um modelo usando Code First.</span><span class="sxs-lookup"><span data-stu-id="602c7-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="602c7-157">Se o objeto for uma instância de algum outro tipo — por exemplo, SqlConnection —, o contexto o usará para o modo de Code First.</span><span class="sxs-lookup"><span data-stu-id="602c7-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="602c7-158">Você pode passar um ObjectContext existente para um Construtor DbContext a fim de criar um DbContext encapsulando o contexto existente.</span><span class="sxs-lookup"><span data-stu-id="602c7-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="602c7-159">Isso pode ser usado para aplicativos existentes que usam ObjectContext, mas que desejam aproveitar o DbContext em algumas partes do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="602c7-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
