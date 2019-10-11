---
title: Consulta assíncrona e Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181842"
---
# <a name="async-query-and-save"></a>Consulta assíncrona e salvar
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

O EF6 introduziu suporte para consulta assíncrona e salvar usando as [palavras-chave Async e Await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que foram introduzidas no .NET 4,5. Embora nem todos os aplicativos possam se beneficiar da assincronia, eles podem ser usados para melhorar a capacidade de resposta do cliente e a escalabilidade do servidor ao manipular tarefas de longa execução, de rede ou de e/s.

## <a name="when-to-really-use-async"></a>Quando realmente usar Async

A finalidade deste passo a passos é apresentar os conceitos assíncronos de uma forma que facilite a observação da diferença entre a execução assíncrona e síncrona do programa. Este tutorial não se destina a ilustrar qualquer um dos principais cenários em que a programação assíncrona fornece benefícios.

A programação assíncrona se concentra principalmente em liberar o thread gerenciado atual (thread que executa o código .NET) para realizar outro trabalho enquanto aguarda uma operação que não requer nenhum tempo de computação de um thread gerenciado. Por exemplo, embora o mecanismo de banco de dados esteja processando uma consulta, não há nada a ser feito pelo código .NET.

Em aplicativos cliente (WinForms, WPF, etc.), o thread atual pode ser usado para manter a interface do usuário responsiva enquanto a operação assíncrona é executada. Em aplicativos de servidor (ASP.NET, etc.), o thread pode ser usado para processar outras solicitações de entrada-isso pode reduzir o uso de memória e/ou aumentar a taxa de transferência do servidor.

Na maioria dos aplicativos, o uso de Async não terá benefícios perceptíveis e até mesmo poderá ser prejudicial. Use testes, criação de perfil e senso comum para medir o impacto de Async em seu cenário específico antes de confirmá-lo.

Veja mais alguns recursos para saber mais sobre Async:

-   [Visão geral de Async/Await do Brandon Bray no .NET 4,5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   Páginas de [programação assíncronas](https://msdn.microsoft.com/library/hh191443.aspx) na biblioteca MSDN
-   [Como criar aplicativos Web ASP.NET usando Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (inclui uma demonstração do aumento da taxa de transferência do servidor)

## <a name="create-the-model"></a>Criar o modelo

Usaremos o [fluxo de trabalho Code First](~/ef6/modeling/code-first/workflows/new-database.md) para criar nosso modelo e gerar o banco de dados, no entanto, a funcionalidade assíncrona funcionará com todos os modelos do EF, incluindo aqueles criados com o designer do EF.

-   Criar um aplicativo de console e chamá-lo de **AsyncDemo**
-   Adicionar o pacote NuGet do EntityFramework
    -   Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto **AsyncDemo**
    -   Selecione **gerenciar pacotes NuGet...**
    -   Na caixa de diálogo gerenciar pacotes NuGet, selecione a guia **online** e escolha o pacote do **EntityFramework**
    -   Clique em **instalar**
-   Adicionar uma classe **Model.cs** com a seguinte implementação

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

Agora que temos um modelo do EF, vamos escrever um código que o use para executar algum acesso a dados.

-   Substitua o conteúdo de **Program.cs** pelo código a seguir

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

Esse código chama o método **PerformDatabaseOperations** , que salva um novo **blog** no banco de dados e, em seguida, recupera todos os **Blogs** do banco de dados e os imprime no **console**do. Depois disso, o programa grava uma cotação do dia para o **console**.

Como o código é síncrono, podemos observar o seguinte fluxo de execução quando executamos o programa:

1.  **SaveChanges** começa a enviar por push o novo **blog** para o banco de dados
2.  **SaveChanges** é concluído
3.  A consulta de todos os **Blogs** é enviada ao banco de dados
4.  Retornos de consulta e resultados são gravados no **console**
5.  A cotação do dia é gravada no **console** do

![Saída de sincronização](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Tornando-o assíncrono

Agora que temos nosso programa em funcionamento, podemos começar a usar as novas palavras-chave Async e Await. Fizemos as seguintes alterações no Program.cs

1.  Linha 2: A instrução using para o namespace **System. Data. Entity** nos dá acesso aos métodos de extensão Async do EF.
2.  Linha 4: A instrução using para o namespace **System. Threading. Tasks** nos permite usar o tipo de **tarefa** .
3.  Linha 12 & 18: Estamos capturando como tarefa que monitora o progresso de **PerformSomeDatabaseOperations** (linha 12) e, em seguida, bloqueando a execução do programa para que essa tarefa seja concluída quando todo o trabalho do programa for concluído (linha 18).
4.  Linha 25: Atualizamos **PerformSomeDatabaseOperations** para ser marcado como **Async** e retornar uma **tarefa**.
5.  Line 35: Agora estamos chamando a versão assíncrona do SaveChanges e aguardando sua conclusão.
6.  Linha 42: Agora estamos chamando a versão assíncrona do ToList e aguardando o resultado.

Para obter uma lista abrangente dos métodos de extensão disponíveis no namespace System. Data. Entity, consulte a classe QueryableExtensions. *Você também precisará adicionar "usando System. Data. Entity" às suas instruções using.*

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

Agora que o código é assíncrono, podemos observar um fluxo de execução diferente quando executamos o programa:

1. **SaveChanges** começa a enviar por push o novo **blog** para o banco de dados  
    *Once o comando é enviado ao banco de dados, não é necessário mais tempo de computação no thread gerenciado atual. O método **PerformDatabaseOperations** retorna (mesmo que não tenha terminado a execução) e o fluxo do programa no método principal continua.*
2. **A cotação do dia é gravada no console do**  
    *Since não há mais trabalho a ser feito no método Main, o thread gerenciado é bloqueado na chamada Wait até que a operação do banco de dados seja concluída. Após a conclusão, o restante de nosso **PerformDatabaseOperations** será executado.*
3.  **SaveChanges** é concluído  
4.  A consulta de todos os **Blogs** é enviada ao banco de dados  
    *Again, o thread gerenciado é livre para realizar outro trabalho enquanto a consulta é processada no banco de dados. Como todas as outras execuções foram concluídas, o thread simplesmente será interrompido na chamada Wait.*
5.  Retornos de consulta e resultados são gravados no **console**  

![Saída assíncrona](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>O argumento

Agora vimos como é fácil fazer uso dos métodos assíncronos do EF. Embora as vantagens do Async não sejam muito aparentes com um simples aplicativo de console, essas mesmas estratégias podem ser aplicadas em situações em que as atividades de longa execução ou vinculadas à rede possam, de outra forma, bloquear o aplicativo ou fazer com que um grande número de threads aumentar a superfície da memória.
