---
title: Async de consulta e salve - o EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
caps.latest.revision: 3
ms.openlocfilehash: 655676029b3a4e42bbe257ff6091179930b1814e
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39119892"
---
# <a name="async-query-and-save"></a>Async consultar e salvar
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

EF6 introduziu o suporte de consulta assíncrona e salvar usando as [async e await palavras-chave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que foram introduzidas no .NET 4.5. Embora nem todos os aplicativos podem se beneficiar de assincronia, ele pode ser usado para melhorar a escalabilidade de capacidade de resposta e o servidor do cliente ao lidar com execução longa, rede ou tarefas vinculado à e/S.

## <a name="when-to-really-use-async"></a>Quando realmente usar async

A finalidade deste passo a passo é apresentar os conceitos de async, de forma que torna mais fácil de observar a diferença entre a execução do programa síncrono e assíncrono. Este passo a passo não se destina para ilustrar a qualquer um dos principais cenários em que a programação assíncrona fornece benefícios.

Programação assíncrona é voltada principalmente liberar o thread gerenciado (código em execução .NET thread) atual faça outro trabalho enquanto ele aguarda uma operação que não requer nenhum tempo de computação de um thread gerenciado. Por exemplo, enquanto o mecanismo de banco de dados está processando uma consulta não há nada a ser feito pelo código do .NET.

Em aplicativos de cliente (WinForms, WPF, etc.) o thread atual pode ser usado para manter a interface do usuário responsiva enquanto a operação assíncrona é executada. Em aplicativos de servidor (ASP.NET, etc.) o thread pode ser usado para processar outras solicitações de entrada - isso pode reduzir o uso de memória e/ou aumentar a taxa de transferência do servidor.

Na maioria dos aplicativos usando o async não terão nenhum benefício perceptível e até mesmo pode ser prejudicial. Use os testes, criação de perfil e senso comum para medir o impacto de assincronia no seu cenário específico antes de confirmá-lo.

Aqui estão mais alguns recursos para saber mais sobre async:

-   [Visão geral do Brandon Bray de async/await no .NET 4.5](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Programação assíncrona](https://msdn.microsoft.com/library/hh191443.aspx) páginas na biblioteca do MSDN
-   [Como criar aplicativos de Web ASP.NET usando Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclui uma demonstração de taxa de transferência ampliada de servidores)

## <a name="create-the-model"></a>Criar o modelo

Estaremos usando o [fluxo de trabalho de Code First](~/ef6/modeling/code-first/workflows/new-database.md) para criar nosso modelo e gerar o banco de dados, no entanto, a funcionalidade assíncrona irá funcionar com todos os modelos do EF, incluindo aqueles criados com o EF Designer.

-   Criar um aplicativo de Console e chamá-lo **AsyncDemo**
-   Adicione o pacote EntityFramework NuGet
    -   No Gerenciador de soluções, clique com botão direito no **AsyncDemo** projeto
    -   Selecione **gerenciar pacotes NuGet...**
    -   Na caixa de diálogo Gerenciar pacotes NuGet, selecione a **Online** guia e escolha o **EntityFramework** pacote
    -   Clique em **instalar**
-   Adicionar um **Model.cs** classe com a implementação a seguir

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

 

## <a name="create-a-synchronous-program"></a>Criar um programa síncrono

Agora que temos um modelo do EF, vamos escrever um código que utiliza para executar algum acesso a dados.

-   Substitua o conteúdo do **Program.cs** com o código a seguir

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

                Console.WriteLine();
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
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Esse código chama o **PerformDatabaseOperations** método que salva uma nova **Blog** para o banco de dados e, em seguida, recupera todos os **Blogs** do banco de dados e imprime-los para o **Console**. Depois disso, o programa grava uma citação do dia para o **Console**.

Como o código é síncrona, podemos pode observar o seguinte fluxo de execução ao executar o programa:

1.  **SaveChanges** começa a enviar por push a nova **Blog** ao banco de dados
2.  **SaveChanges** é concluída
3.  Consulta para todos os **Blogs** é enviada para o banco de dados
4.  Consulta retorna e os resultados são gravados em **Console**
5.  Cota do dia é gravada em **Console**

![SyncOutput](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Tornando assíncrono

Agora que temos nosso programa em execução, podemos começar a fazer uso de novo async e await palavras-chave. Fizemos as seguintes alterações ao Program.cs

1.  Linha 2: O usando a instrução para o **Entity** namespace nos dá acesso aos métodos de extensão assíncrono EF.
2.  Linha 4: O usando a instrução para o **Tasks** namespace nos permite usar o **tarefa** tipo.
3.  Linha 12 & 18: capturamos como que monitora o andamento da tarefa **PerformSomeDatabaseOperations** (linha 12) e, em seguida, bloqueando a execução do programa para este de tarefas para uma vez concluída todo o trabalho para o programa é criado (linha 18).
4.  Linha 25: Temos update **PerformSomeDatabaseOperations** a ser marcado como **async** e retornar um **tarefa**.
5.  Linha 35: Estamos agora chamando a versão assíncrona de SaveChanges e aguardando a conclusão de TI.
6.  Linha 42: Estamos agora chamando a versão assíncrona de ToList e aguardar o resultado.

Para obter uma lista abrangente dos métodos de extensão disponíveis no namespace System, consulte a classe QueryableExtensions. *Você também precisará adicionar "using System" ao seu usando as instruções.*

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

Agora que o código é assíncrona, podemos pode observar um fluxo de execução diferente quando executamos o programa:

1.  **SaveChanges** começa a enviar por push a nova **Blog** no banco de dados *depois que o comando é enviado ao banco de dados não mais de computação tempo é necessária no thread gerenciado atual. O **PerformDatabaseOperations** método retorna (embora não foi concluída em execução) e fluxo do programa no método principal continua.*
2.  **Cota do dia é gravada Console**
    *porque não há nada mais trabalho a ser feito no método Main, o thread gerenciado é bloqueado em tempo de espera chamar até que a operação de banco de dados é concluída. Quando for concluído, o restante de nosso **PerformDatabaseOperations** * será executado.
3.  **SaveChanges** é concluída
4.  Consulta para todos os **Blogs** é enviado para o banco de dados *novamente, o thread gerenciado é livre para executar outras tarefas enquanto a consulta é processada no banco de dados. Uma vez que todos os outra execução for concluída, o thread apenas interromperá na chamada espera entanto.*
5.  Consulta retorna e os resultados são gravados em **Console**

![AsyncOutput](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>O argumento

Agora que vimos como é fácil fazer uso dos métodos de assíncrono do EF. Embora as vantagens de async podem não ser muito aparentes com um aplicativo de console simples, essas mesmas estratégias podem ser aplicadas em situações em que as atividades de execução longa ou limite de rede podem caso contrário, bloqueia o aplicativo, ou fazer com que um grande número de threads para Aumente o volume de memória.
