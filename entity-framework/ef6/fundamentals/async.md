---
title: Consulta assíncrona e Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: ae578976ffc88b407ef0aaa0017935005bedd093
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921622"
---
# <a name="async-query-and-save"></a><span data-ttu-id="07907-102">Consulta assíncrona e salvar</span><span class="sxs-lookup"><span data-stu-id="07907-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="07907-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="07907-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="07907-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="07907-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="07907-105">O EF6 introduziu suporte para consulta assíncrona e salvar usando as [palavras-chave Async e Await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que foram introduzidas no .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="07907-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="07907-106">Embora nem todos os aplicativos possam se beneficiar da assincronia, eles podem ser usados para melhorar a capacidade de resposta do cliente e a escalabilidade do servidor ao manipular tarefas de longa execução, de rede ou de e/s.</span><span class="sxs-lookup"><span data-stu-id="07907-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="07907-107">Quando realmente usar Async</span><span class="sxs-lookup"><span data-stu-id="07907-107">When to really use async</span></span>

<span data-ttu-id="07907-108">A finalidade deste passo a passos é apresentar os conceitos assíncronos de uma forma que facilite a observação da diferença entre a execução assíncrona e síncrona do programa.</span><span class="sxs-lookup"><span data-stu-id="07907-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="07907-109">Este tutorial não se destina a ilustrar qualquer um dos principais cenários em que a programação assíncrona fornece benefícios.</span><span class="sxs-lookup"><span data-stu-id="07907-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="07907-110">A programação assíncrona se concentra principalmente em liberar o thread gerenciado atual (thread que executa o código .NET) para realizar outro trabalho enquanto aguarda uma operação que não requer nenhum tempo de computação de um thread gerenciado.</span><span class="sxs-lookup"><span data-stu-id="07907-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="07907-111">Por exemplo, embora o mecanismo de banco de dados esteja processando uma consulta, não há nada a ser feito pelo código .NET.</span><span class="sxs-lookup"><span data-stu-id="07907-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="07907-112">Em aplicativos cliente (WinForms, WPF, etc.), o thread atual pode ser usado para manter a interface do usuário responsiva enquanto a operação assíncrona é executada.</span><span class="sxs-lookup"><span data-stu-id="07907-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="07907-113">Em aplicativos de servidor (ASP.NET, etc.), o thread pode ser usado para processar outras solicitações de entrada-isso pode reduzir o uso de memória e/ou aumentar a taxa de transferência do servidor.</span><span class="sxs-lookup"><span data-stu-id="07907-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="07907-114">Na maioria dos aplicativos, o uso de Async não terá benefícios perceptíveis e até mesmo poderá ser prejudicial.</span><span class="sxs-lookup"><span data-stu-id="07907-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="07907-115">Use testes, criação de perfil e senso comum para medir o impacto de Async em seu cenário específico antes de confirmá-lo.</span><span class="sxs-lookup"><span data-stu-id="07907-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="07907-116">Veja mais alguns recursos para saber mais sobre Async:</span><span class="sxs-lookup"><span data-stu-id="07907-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="07907-117">Visão geral de Async/Await do Brandon Bray no .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="07907-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="07907-118">Páginas de [programação](https://msdn.microsoft.com/library/hh191443.aspx) assíncronas na biblioteca MSDN</span><span class="sxs-lookup"><span data-stu-id="07907-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="07907-119">[Como criar aplicativos Web ASP.NET usando Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclui uma demonstração do aumento da taxa de transferência do servidor)</span><span class="sxs-lookup"><span data-stu-id="07907-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="07907-120">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="07907-120">Create the model</span></span>

<span data-ttu-id="07907-121">Usaremos o [fluxo de trabalho Code First](~/ef6/modeling/code-first/workflows/new-database.md) para criar nosso modelo e gerar o banco de dados, no entanto, a funcionalidade assíncrona funcionará com todos os modelos do EF, incluindo aqueles criados com o designer do EF.</span><span class="sxs-lookup"><span data-stu-id="07907-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="07907-122">Criar um aplicativo de console e chamá-lo de **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="07907-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="07907-123">Adicionar o pacote NuGet do EntityFramework</span><span class="sxs-lookup"><span data-stu-id="07907-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="07907-124">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="07907-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="07907-125">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="07907-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="07907-126">Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="07907-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="07907-127">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="07907-127">Click **Install**</span></span>
-   <span data-ttu-id="07907-128">Adicionar uma classe **Model.cs** com a seguinte implementação</span><span class="sxs-lookup"><span data-stu-id="07907-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="07907-129">Criar um programa síncrono</span><span class="sxs-lookup"><span data-stu-id="07907-129">Create a synchronous program</span></span>

<span data-ttu-id="07907-130">Agora que temos um modelo do EF, vamos escrever um código que o use para executar algum acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="07907-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="07907-131">Substitua o conteúdo de **Program.cs** pelo código a seguir</span><span class="sxs-lookup"><span data-stu-id="07907-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="07907-132">Esse código chama o método **PerformDatabaseOperations** , que salva um novo **blog** no banco de dados e, em seguida, recupera todos os **Blogs** do banco de dados e os imprime no **console**do.</span><span class="sxs-lookup"><span data-stu-id="07907-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="07907-133">Depois disso, o programa grava uma cotação do dia para o **console**.</span><span class="sxs-lookup"><span data-stu-id="07907-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="07907-134">Como o código é síncrono, podemos observar o seguinte fluxo de execução quando executamos o programa:</span><span class="sxs-lookup"><span data-stu-id="07907-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="07907-135">**SaveChanges** começa a enviar por push o novo **blog** para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="07907-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="07907-136">**SaveChanges** é concluído</span><span class="sxs-lookup"><span data-stu-id="07907-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="07907-137">A consulta de todos os **Blogs** é enviada ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="07907-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="07907-138">Retornos de consulta e resultados são gravados no **console**</span><span class="sxs-lookup"><span data-stu-id="07907-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="07907-139">A cotação do dia é gravada no **console** do</span><span class="sxs-lookup"><span data-stu-id="07907-139">Quote of the day is written to **Console**</span></span>

![Saída de sincronização](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="07907-141">Tornando-o assíncrono</span><span class="sxs-lookup"><span data-stu-id="07907-141">Making it asynchronous</span></span>

<span data-ttu-id="07907-142">Agora que temos nosso programa em funcionamento, podemos começar a usar as novas palavras-chave Async e Await.</span><span class="sxs-lookup"><span data-stu-id="07907-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="07907-143">Fizemos as seguintes alterações no Program.cs</span><span class="sxs-lookup"><span data-stu-id="07907-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="07907-144">Linha 2: A instrução using para o namespace **System. Data. Entity** nos dá acesso aos métodos de extensão Async do EF.</span><span class="sxs-lookup"><span data-stu-id="07907-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="07907-145">Linha 4: A instrução using para o namespace **System. Threading. Tasks** nos permite usar o tipo de **tarefa** .</span><span class="sxs-lookup"><span data-stu-id="07907-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="07907-146">Linha 12 & 18: Estamos capturando como tarefa que monitora o progresso de **PerformSomeDatabaseOperations** (linha 12) e, em seguida, bloqueando a execução do programa para que essa tarefa seja concluída quando todo o trabalho do programa for concluído (linha 18).</span><span class="sxs-lookup"><span data-stu-id="07907-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="07907-147">Linha 25: Atualizamos **PerformSomeDatabaseOperations** para ser marcado como **Async** e retornar uma **tarefa**.</span><span class="sxs-lookup"><span data-stu-id="07907-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="07907-148">Line 35: Agora estamos chamando a versão assíncrona do SaveChanges e aguardando sua conclusão.</span><span class="sxs-lookup"><span data-stu-id="07907-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="07907-149">Linha 42: Agora estamos chamando a versão assíncrona do ToList e aguardando o resultado.</span><span class="sxs-lookup"><span data-stu-id="07907-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="07907-150">Para obter uma lista abrangente dos métodos de extensão disponíveis no namespace System. Data. Entity, consulte a classe QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="07907-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="07907-151">*Você também precisará adicionar "usando System. Data. Entity" às suas instruções using.*</span><span class="sxs-lookup"><span data-stu-id="07907-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="07907-152">Agora que o código é assíncrono, podemos observar um fluxo de execução diferente quando executamos o programa:</span><span class="sxs-lookup"><span data-stu-id="07907-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="07907-153">**SaveChanges** começa a enviar por push o novo **blog** para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="07907-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="07907-154">*Depois que o comando é enviado ao banco de dados, não é necessário mais tempo de computação no thread gerenciado atual. O método **PerformDatabaseOperations** retorna (mesmo que não tenha terminado a execução) e o fluxo do programa no método Main continua.*</span><span class="sxs-lookup"><span data-stu-id="07907-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="07907-155">**A cotação do dia é gravada no console do**</span><span class="sxs-lookup"><span data-stu-id="07907-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="07907-156">*Como não há mais trabalho a fazer no método Main, o thread gerenciado é bloqueado na chamada Wait até que a operação do banco de dados seja concluída. Após a conclusão, o restante de nosso **PerformDatabaseOperations** será executado.*</span><span class="sxs-lookup"><span data-stu-id="07907-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="07907-157">**SaveChanges** é concluído</span><span class="sxs-lookup"><span data-stu-id="07907-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="07907-158">A consulta de todos os **Blogs** é enviada ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="07907-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="07907-159">*Novamente, o thread gerenciado está livre para realizar outros trabalhos enquanto a consulta é processada no banco de dados. Como todas as outras execuções foram concluídas, o thread simplesmente será interrompido na chamada Wait.*</span><span class="sxs-lookup"><span data-stu-id="07907-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="07907-160">Retornos de consulta e resultados são gravados no **console**</span><span class="sxs-lookup"><span data-stu-id="07907-160">Query returns and results are written to **Console**</span></span>  

![Saída assíncrona](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="07907-162">O argumento</span><span class="sxs-lookup"><span data-stu-id="07907-162">The takeaway</span></span>

<span data-ttu-id="07907-163">Agora vimos como é fácil fazer uso dos métodos assíncronos do EF.</span><span class="sxs-lookup"><span data-stu-id="07907-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="07907-164">Embora as vantagens do Async não sejam muito aparentes com um simples aplicativo de console, essas mesmas estratégias podem ser aplicadas em situações em que as atividades de longa execução ou vinculadas à rede possam, de outra forma, bloquear o aplicativo ou fazer com que um grande número de threads aumentar a superfície da memória.</span><span class="sxs-lookup"><span data-stu-id="07907-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
