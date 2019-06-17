---
title: Async de consulta e salve - o EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 8c72012be4b77ff31faf909bf02035865521a640
ms.sourcegitcommit: 7c5c5e09a4d2671d7461e027837966c4ff91e398
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2019
ms.locfileid: "67148499"
---
# <a name="async-query-and-save"></a><span data-ttu-id="b30b7-102">Async consultar e salvar</span><span class="sxs-lookup"><span data-stu-id="b30b7-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="b30b7-103">**EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b30b7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b30b7-104">Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.</span><span class="sxs-lookup"><span data-stu-id="b30b7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b30b7-105">EF6 introduziu o suporte de consulta assíncrona e salvar usando as [async e await palavras-chave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que foram introduzidas no .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="b30b7-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="b30b7-106">Embora nem todos os aplicativos podem se beneficiar de assincronia, ele pode ser usado para melhorar a escalabilidade de capacidade de resposta e o servidor do cliente ao lidar com execução longa, rede ou tarefas vinculado à e/S.</span><span class="sxs-lookup"><span data-stu-id="b30b7-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="b30b7-107">Quando realmente usar async</span><span class="sxs-lookup"><span data-stu-id="b30b7-107">When to really use async</span></span>

<span data-ttu-id="b30b7-108">A finalidade deste passo a passo é apresentar os conceitos de async, de forma que torna mais fácil de observar a diferença entre a execução do programa síncrono e assíncrono.</span><span class="sxs-lookup"><span data-stu-id="b30b7-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="b30b7-109">Este passo a passo não se destina para ilustrar a qualquer um dos principais cenários em que a programação assíncrona fornece benefícios.</span><span class="sxs-lookup"><span data-stu-id="b30b7-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="b30b7-110">Programação assíncrona é voltada principalmente liberar o thread gerenciado (código em execução .NET thread) atual faça outro trabalho enquanto ele aguarda uma operação que não requer nenhum tempo de computação de um thread gerenciado.</span><span class="sxs-lookup"><span data-stu-id="b30b7-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="b30b7-111">Por exemplo, enquanto o mecanismo de banco de dados está processando uma consulta não há nada a ser feito pelo código do .NET.</span><span class="sxs-lookup"><span data-stu-id="b30b7-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="b30b7-112">Em aplicativos de cliente (WinForms, WPF, etc.) o thread atual pode ser usado para manter a interface do usuário responsiva enquanto a operação assíncrona é executada.</span><span class="sxs-lookup"><span data-stu-id="b30b7-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="b30b7-113">Em aplicativos de servidor (ASP.NET, etc.) o thread pode ser usado para processar outras solicitações de entrada - isso pode reduzir o uso de memória e/ou aumentar a taxa de transferência do servidor.</span><span class="sxs-lookup"><span data-stu-id="b30b7-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="b30b7-114">Na maioria dos aplicativos usando o async não terão nenhum benefício perceptível e até mesmo pode ser prejudicial.</span><span class="sxs-lookup"><span data-stu-id="b30b7-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="b30b7-115">Use os testes, criação de perfil e senso comum para medir o impacto de assincronia no seu cenário específico antes de confirmá-lo.</span><span class="sxs-lookup"><span data-stu-id="b30b7-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="b30b7-116">Aqui estão mais alguns recursos para saber mais sobre async:</span><span class="sxs-lookup"><span data-stu-id="b30b7-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="b30b7-117">Visão geral do Brandon Bray de async/await no .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b30b7-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="b30b7-118">[Programação assíncrona](https://msdn.microsoft.com/library/hh191443.aspx) páginas na biblioteca do MSDN</span><span class="sxs-lookup"><span data-stu-id="b30b7-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="b30b7-119">[Como criar aplicativos de Web ASP.NET usando Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclui uma demonstração de taxa de transferência ampliada de servidores)</span><span class="sxs-lookup"><span data-stu-id="b30b7-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="b30b7-120">Criar o modelo</span><span class="sxs-lookup"><span data-stu-id="b30b7-120">Create the model</span></span>

<span data-ttu-id="b30b7-121">Estaremos usando o [fluxo de trabalho de Code First](~/ef6/modeling/code-first/workflows/new-database.md) para criar nosso modelo e gerar o banco de dados, no entanto, a funcionalidade assíncrona irá funcionar com todos os modelos do EF, incluindo aqueles criados com o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="b30b7-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="b30b7-122">Criar um aplicativo de Console e chamá-lo **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="b30b7-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="b30b7-123">Adicione o pacote EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="b30b7-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="b30b7-124">No Gerenciador de soluções, clique com botão direito no **AsyncDemo** projeto</span><span class="sxs-lookup"><span data-stu-id="b30b7-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="b30b7-125">Selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="b30b7-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="b30b7-126">Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote</span><span class="sxs-lookup"><span data-stu-id="b30b7-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="b30b7-127">Clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="b30b7-127">Click **Install**</span></span>
-   <span data-ttu-id="b30b7-128">Adicionar um **Model.cs** classe com a implementação a seguir</span><span class="sxs-lookup"><span data-stu-id="b30b7-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="b30b7-129">Criar um programa síncrono</span><span class="sxs-lookup"><span data-stu-id="b30b7-129">Create a synchronous program</span></span>

<span data-ttu-id="b30b7-130">Agora que temos um modelo do EF, vamos escrever um código que utiliza para executar algum acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="b30b7-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="b30b7-131">Substitua o conteúdo do **Program.cs** com o código a seguir</span><span class="sxs-lookup"><span data-stu-id="b30b7-131">Replace the contents of **Program.cs** with the following code</span></span>

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

<span data-ttu-id="b30b7-132">Esse código chama o **PerformDatabaseOperations** método que salva uma nova **Blog** para o banco de dados e, em seguida, recupera todos os **Blogs** do banco de dados e imprime-los para o **Console**.</span><span class="sxs-lookup"><span data-stu-id="b30b7-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="b30b7-133">Depois disso, o programa grava uma citação do dia para o **Console**.</span><span class="sxs-lookup"><span data-stu-id="b30b7-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="b30b7-134">Como o código é síncrono, podemos pode observar o seguinte fluxo de execução ao executar o programa:</span><span class="sxs-lookup"><span data-stu-id="b30b7-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="b30b7-135">**SaveChanges** começa a enviar por push a nova **Blog** ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="b30b7-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="b30b7-136">**SaveChanges** é concluída</span><span class="sxs-lookup"><span data-stu-id="b30b7-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="b30b7-137">Consulta para todos os **Blogs** é enviada para o banco de dados</span><span class="sxs-lookup"><span data-stu-id="b30b7-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="b30b7-138">Consulta retorna e os resultados são gravados em **Console**</span><span class="sxs-lookup"><span data-stu-id="b30b7-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="b30b7-139">Cota do dia é gravada em **Console**</span><span class="sxs-lookup"><span data-stu-id="b30b7-139">Quote of the day is written to **Console**</span></span>

![Sincronização de saída](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="b30b7-141">Tornando assíncrono</span><span class="sxs-lookup"><span data-stu-id="b30b7-141">Making it asynchronous</span></span>

<span data-ttu-id="b30b7-142">Agora que temos nosso programa em execução, podemos começar a fazer uso de novo async e await palavras-chave.</span><span class="sxs-lookup"><span data-stu-id="b30b7-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="b30b7-143">Fizemos as seguintes alterações ao Program.cs</span><span class="sxs-lookup"><span data-stu-id="b30b7-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="b30b7-144">Line 2: O usando a instrução para o **Entity** namespace nos dá acesso aos métodos de extensão assíncrono EF.</span><span class="sxs-lookup"><span data-stu-id="b30b7-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="b30b7-145">Linha 4: O usando a instrução para o **Tasks** namespace nos permite usar o **tarefa** tipo.</span><span class="sxs-lookup"><span data-stu-id="b30b7-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="b30b7-146">Linha 12 & 18: Capturamos como que monitora o andamento da tarefa **PerformSomeDatabaseOperations** (linha 12) e, em seguida, bloqueando a execução do programa para este de tarefas para uma vez concluída todo o trabalho para o programa é criado (linha 18).</span><span class="sxs-lookup"><span data-stu-id="b30b7-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="b30b7-147">Line 25: Temos o update **PerformSomeDatabaseOperations** a ser marcado como **async** e retornar um **tarefa**.</span><span class="sxs-lookup"><span data-stu-id="b30b7-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="b30b7-148">Line 35: Agora estamos chamando a versão assíncrona de SaveChanges e aguardando a conclusão de TI.</span><span class="sxs-lookup"><span data-stu-id="b30b7-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="b30b7-149">Line 42: Agora estamos ligando para a versão assíncrona de ToList e aguardando no resultado.</span><span class="sxs-lookup"><span data-stu-id="b30b7-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="b30b7-150">Para obter uma lista abrangente dos métodos de extensão disponíveis no namespace System, consulte a classe QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="b30b7-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="b30b7-151">*Você também precisará adicionar "using System" ao seu usando as instruções.*</span><span class="sxs-lookup"><span data-stu-id="b30b7-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="b30b7-152">Agora que o código é assíncrona, podemos pode observar um fluxo de execução diferente quando executamos o programa:</span><span class="sxs-lookup"><span data-stu-id="b30b7-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="b30b7-153">**SaveChanges** começa a enviar por push a nova **Blog** no banco de dados *depois que o comando é enviado ao banco de dados não mais de computação tempo é necessária no thread gerenciado atual. O **PerformDatabaseOperations** método retorna (embora não foi concluída em execução) e fluxo do programa no método principal continua.*</span><span class="sxs-lookup"><span data-stu-id="b30b7-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="b30b7-154">**Cota do dia é gravada Console**
    *porque não há nada mais trabalho a ser feito no método Main, o thread gerenciado é bloqueado em tempo de espera chamar até que a operação de banco de dados é concluída. Quando for concluído, o restante de nosso **PerformDatabaseOperations** será executado.*</span><span class="sxs-lookup"><span data-stu-id="b30b7-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="b30b7-155">**SaveChanges** é concluída</span><span class="sxs-lookup"><span data-stu-id="b30b7-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="b30b7-156">Consulta para todos os **Blogs** é enviado para o banco de dados *novamente, o thread gerenciado é livre para executar outras tarefas enquanto a consulta é processada no banco de dados. Uma vez que todos os outra execução for concluída, o thread apenas interromperá na chamada espera entanto.*</span><span class="sxs-lookup"><span data-stu-id="b30b7-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="b30b7-157">Consulta retorna e os resultados são gravados em **Console**</span><span class="sxs-lookup"><span data-stu-id="b30b7-157">Query returns and results are written to **Console**</span></span>

![Saída de Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="b30b7-159">O argumento</span><span class="sxs-lookup"><span data-stu-id="b30b7-159">The takeaway</span></span>

<span data-ttu-id="b30b7-160">Agora que vimos como é fácil fazer uso dos métodos de assíncrono do EF.</span><span class="sxs-lookup"><span data-stu-id="b30b7-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="b30b7-161">Embora as vantagens de async podem não ser muito aparentes com um aplicativo de console simples, essas mesmas estratégias podem ser aplicadas em situações em que as atividades de execução longa ou limite de rede podem caso contrário, bloqueia o aplicativo, ou fazer com que um grande número de threads para Aumente o volume de memória.</span><span class="sxs-lookup"><span data-stu-id="b30b7-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
