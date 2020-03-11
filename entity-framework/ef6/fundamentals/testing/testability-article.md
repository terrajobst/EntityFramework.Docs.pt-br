---
title: Capacidade de teste e Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416446"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="774c8-102">Capacidade de teste e Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="774c8-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="774c8-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="774c8-103">Scott Allen</span></span>

<span data-ttu-id="774c8-104">Publicado em: maio de 2010</span><span class="sxs-lookup"><span data-stu-id="774c8-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="774c8-105">Introdução</span><span class="sxs-lookup"><span data-stu-id="774c8-105">Introduction</span></span>

<span data-ttu-id="774c8-106">Este white paper descreve e demonstra como escrever código de teste com o ADO.NET Entity Framework 4,0 e o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="774c8-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="774c8-107">Este documento não tenta se concentrar em uma metodologia de teste específica, como o TDD (design controlado por testes) ou o BDD (design controlado por comportamento).</span><span class="sxs-lookup"><span data-stu-id="774c8-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="774c8-108">Em vez disso, este artigo se concentrará em como escrever código que usa o ADO.NET Entity Framework, ainda assim, é fácil isolar e testar de maneira automatizada.</span><span class="sxs-lookup"><span data-stu-id="774c8-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="774c8-109">Veremos os padrões de design comuns que facilitam o teste em cenários de acesso a dados e Confira como aplicar esses padrões ao usar a estrutura.</span><span class="sxs-lookup"><span data-stu-id="774c8-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="774c8-110">Também examinaremos os recursos específicos da estrutura para ver como esses recursos podem funcionar no código de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="774c8-111">O que é o código de teste?</span><span class="sxs-lookup"><span data-stu-id="774c8-111">What Is Testable Code?</span></span>

<span data-ttu-id="774c8-112">A capacidade de verificar uma parte do software usando testes de unidade automatizados oferece muitos benefícios desejáveis.</span><span class="sxs-lookup"><span data-stu-id="774c8-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="774c8-113">Todos sabem que bons testes reduzirão o número de defeitos de software em um aplicativo e aumentarão a qualidade do aplicativo, mas ter testes de unidade vai muito além da localização de bugs.</span><span class="sxs-lookup"><span data-stu-id="774c8-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="774c8-114">Um bom conjunto de testes de unidade permite que uma equipe de desenvolvimento Economize tempo e permaneça no controle do software que eles criam.</span><span class="sxs-lookup"><span data-stu-id="774c8-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="774c8-115">Uma equipe pode fazer alterações no código existente, refatorar, recriar e reestruturar o software para atender aos novos requisitos e adicionar novos componentes a um aplicativo, tudo sabendo que o conjunto de testes pode verificar o comportamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="774c8-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="774c8-116">Os testes de unidade fazem parte de um rápido ciclo de comentários para facilitar a alteração e preservar a facilidade de manutenção do software à medida que a complexidade aumenta.</span><span class="sxs-lookup"><span data-stu-id="774c8-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="774c8-117">No entanto, o teste de unidade vem com um preço.</span><span class="sxs-lookup"><span data-stu-id="774c8-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="774c8-118">Uma equipe deve investir tempo para criar e manter testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="774c8-119">A quantidade de esforço necessária para criar esses testes está diretamente relacionada à **capacidade de teste** do software subjacente.</span><span class="sxs-lookup"><span data-stu-id="774c8-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="774c8-120">Quão fácil é o software para testar?</span><span class="sxs-lookup"><span data-stu-id="774c8-120">How easy is the software to test?</span></span> <span data-ttu-id="774c8-121">Uma equipe que projeta software com capacidade de teste em mente criará testes efetivos mais rápido do que a equipe que trabalha com software não-retestado.</span><span class="sxs-lookup"><span data-stu-id="774c8-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="774c8-122">A Microsoft projetou a ADO.NET Entity Framework 4,0 (EF4) com a capacidade de teste em mente.</span><span class="sxs-lookup"><span data-stu-id="774c8-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="774c8-123">Isso não significa que os desenvolvedores estejam escrevendo testes de unidade no próprio código da estrutura.</span><span class="sxs-lookup"><span data-stu-id="774c8-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="774c8-124">Em vez disso, as metas de capacidade de teste para o EF4 facilitam a criação de código que se baseia na estrutura.</span><span class="sxs-lookup"><span data-stu-id="774c8-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="774c8-125">Antes de examinarmos os exemplos específicos, vale a pena entender as qualidades do código de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="774c8-126">As qualidades do código de teste</span><span class="sxs-lookup"><span data-stu-id="774c8-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="774c8-127">O código que é fácil de testar sempre exibirá pelo menos duas características.</span><span class="sxs-lookup"><span data-stu-id="774c8-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="774c8-128">Primeiro, o código de teste é fácil de **observar**.</span><span class="sxs-lookup"><span data-stu-id="774c8-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="774c8-129">Dado algum conjunto de entradas, deve ser fácil observar a saída do código.</span><span class="sxs-lookup"><span data-stu-id="774c8-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="774c8-130">Por exemplo, o teste do método a seguir é fácil porque o método retorna diretamente o resultado de um cálculo.</span><span class="sxs-lookup"><span data-stu-id="774c8-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="774c8-131">O teste de um método é difícil se o método gravar o valor calculado em um soquete de rede, uma tabela de banco de dados ou um arquivo como o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="774c8-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="774c8-132">O teste precisa executar trabalho adicional para recuperar o valor.</span><span class="sxs-lookup"><span data-stu-id="774c8-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="774c8-133">Em segundo lugar, o código de teste é fácil de **isolar**.</span><span class="sxs-lookup"><span data-stu-id="774c8-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="774c8-134">Vamos usar o seguinte pseudocódigo como um exemplo inadequado de código de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="774c8-135">O método é fácil de observar – podemos passar uma apólice de seguro e verificar se o valor retornado corresponde a um resultado esperado.</span><span class="sxs-lookup"><span data-stu-id="774c8-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="774c8-136">No entanto, para testar o método, precisaremos ter um banco de dados instalado com o esquema correto e configurar o servidor SMTP caso o método tente enviar um email.</span><span class="sxs-lookup"><span data-stu-id="774c8-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="774c8-137">O teste de unidade só deseja verificar a lógica de cálculo dentro do método, mas o teste pode falhar porque o servidor de email está offline ou porque o servidor de banco de dados foi movido.</span><span class="sxs-lookup"><span data-stu-id="774c8-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="774c8-138">Essas duas falhas não estão relacionadas ao comportamento que o teste deseja verificar.</span><span class="sxs-lookup"><span data-stu-id="774c8-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="774c8-139">O comportamento é difícil de isolar.</span><span class="sxs-lookup"><span data-stu-id="774c8-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="774c8-140">Os desenvolvedores de software que se esforçam para escrever código de teste geralmente buscam manter uma separação de preocupações no código que escrevem.</span><span class="sxs-lookup"><span data-stu-id="774c8-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="774c8-141">O método acima deve se concentrar nos cálculos de negócios e delegar o banco de dados e os detalhes de implementação de email para outros componentes.</span><span class="sxs-lookup"><span data-stu-id="774c8-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="774c8-142">Robert C. Martin chama esse princípio de responsabilidade única.</span><span class="sxs-lookup"><span data-stu-id="774c8-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="774c8-143">Um objeto deve encapsular uma única responsabilidade estreita, como calcular o valor de uma política.</span><span class="sxs-lookup"><span data-stu-id="774c8-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="774c8-144">Todos os outros trabalhos de banco de dados e de notificação devem ser de responsabilidade de algum outro objeto.</span><span class="sxs-lookup"><span data-stu-id="774c8-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="774c8-145">O código escrito dessa maneira é mais fácil de isolar, pois concentra-se em uma única tarefa.</span><span class="sxs-lookup"><span data-stu-id="774c8-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="774c8-146">No .NET, temos as abstrações que precisamos para seguir o princípio de responsabilidade única e obter isolamento.</span><span class="sxs-lookup"><span data-stu-id="774c8-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="774c8-147">Podemos usar definições de interface e forçar o código a usar a abstração de interface em vez de um tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="774c8-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="774c8-148">Mais adiante neste documento, veremos como um método como o exemplo inadequado apresentado acima pode funcionar com interfaces *parecidas* com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="774c8-149">No momento do teste, no entanto, podemos substituir uma implementação fictícia que não se comunica com o banco de dados, mas em vez disso, ele mantém o dado na memória.</span><span class="sxs-lookup"><span data-stu-id="774c8-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="774c8-150">Essa implementação fictícia isolará o código de problemas não relacionados no código de acesso a dados ou na configuração do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="774c8-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="774c8-151">Há benefícios adicionais para isolamento.</span><span class="sxs-lookup"><span data-stu-id="774c8-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="774c8-152">O cálculo de negócios no último método deve levar apenas alguns milissegundos para ser executado, mas o próprio teste pode ser executado por vários segundos, já que o código é saltos em toda a rede e se comunica com vários servidores.</span><span class="sxs-lookup"><span data-stu-id="774c8-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="774c8-153">Os testes de unidade devem ser executados rapidamente para facilitar pequenas alterações.</span><span class="sxs-lookup"><span data-stu-id="774c8-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="774c8-154">Os testes de unidade também devem ser repetíveis e não podem falhar porque um componente não relacionado ao teste tem um problema.</span><span class="sxs-lookup"><span data-stu-id="774c8-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="774c8-155">Escrever código que seja fácil de observar e isolar significa que os desenvolvedores terão um tempo mais fácil de escrever testes para o código, gastar menos tempo aguardando a execução de testes e, o mais importante, gastar menos tempo controlando os bugs que não existem.</span><span class="sxs-lookup"><span data-stu-id="774c8-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="774c8-156">Espero que você aprecie os benefícios do teste e entenda as qualidades que o código que pode ser testado é exibido.</span><span class="sxs-lookup"><span data-stu-id="774c8-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="774c8-157">Estamos prestes a abordar como escrever código que funciona com o EF4 para salvar dados em um banco de dado, enquanto restam observáveis e fáceis de isolar, mas primeiro vamos restringir nosso foco para discutir os designs de teste para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="774c8-158">Padrões de design para persistência de dados</span><span class="sxs-lookup"><span data-stu-id="774c8-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="774c8-159">Os dois exemplos inadequados apresentados anteriormente tinham muitas responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="774c8-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="774c8-160">O primeiro exemplo inadequado tinha de executar um cálculo *e* gravar em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="774c8-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="774c8-161">O segundo exemplo inadequado tinha de ler dados de um banco de dado *e* executar um cálculo comercial *e* enviar email.</span><span class="sxs-lookup"><span data-stu-id="774c8-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="774c8-162">Ao criar métodos menores que separam preocupações e delegam responsabilidade a outros componentes, você fará grandes progressos em direção à escrita de código de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="774c8-163">O objetivo é criar funcionalidades ao compor ações de abstrações pequenas e focadas.</span><span class="sxs-lookup"><span data-stu-id="774c8-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="774c8-164">Quando se trata de persistência de dados, as abstrações pequenas e focadas que estamos procurando são tão comuns que foram documentadas como padrões de design.</span><span class="sxs-lookup"><span data-stu-id="774c8-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="774c8-165">Os padrões de livro de Martin Fowler da arquitetura de aplicativos empresariais foram o primeiro trabalho a descrever esses padrões na impressão.</span><span class="sxs-lookup"><span data-stu-id="774c8-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="774c8-166">Forneceremos uma breve descrição desses padrões nas seções a seguir antes de mostrarmos como essas ADO.NET Entity Framework implementam e funcionam com esses padrões.</span><span class="sxs-lookup"><span data-stu-id="774c8-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="774c8-167">O Padrão de Repositório</span><span class="sxs-lookup"><span data-stu-id="774c8-167">The Repository Pattern</span></span>

<span data-ttu-id="774c8-168">O Fowler diz um repositório "medias between from the data Mapping Layers, usando uma interface do tipo coleção para acessar objetos de domínio".</span><span class="sxs-lookup"><span data-stu-id="774c8-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="774c8-169">O objetivo do padrão de repositório é isolar o código do minúcias de acesso a dados e, como vimos anteriormente, o isolamento é uma característica necessária para a capacidade de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="774c8-170">A chave para o isolamento é como o repositório expõe objetos usando uma interface como coleção.</span><span class="sxs-lookup"><span data-stu-id="774c8-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="774c8-171">A lógica que você escreve para usar o repositório não tem idéia de como o repositório irá materializar os objetos que você solicitar.</span><span class="sxs-lookup"><span data-stu-id="774c8-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="774c8-172">O repositório pode se comunicar com um banco de dados ou pode simplesmente retornar objetos de uma coleção na memória.</span><span class="sxs-lookup"><span data-stu-id="774c8-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="774c8-173">Tudo o que seu código precisa saber é que o repositório parece manter a coleção e você pode recuperar, adicionar e excluir objetos da coleção.</span><span class="sxs-lookup"><span data-stu-id="774c8-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="774c8-174">Em aplicativos .NET existentes, um repositório concreto geralmente herda de uma interface genérica como a seguinte:</span><span class="sxs-lookup"><span data-stu-id="774c8-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="774c8-175">Faremos algumas alterações na definição da interface quando fornecermos uma implementação para EF4, mas o conceito básico permanece o mesmo.</span><span class="sxs-lookup"><span data-stu-id="774c8-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="774c8-176">O código pode usar um repositório concreto implementando essa interface para recuperar uma entidade por seu valor de chave primária, para recuperar uma coleção de entidades com base na avaliação de um predicado ou simplesmente recuperar todas as entidades disponíveis.</span><span class="sxs-lookup"><span data-stu-id="774c8-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="774c8-177">O código também pode adicionar e remover entidades por meio da interface do repositório.</span><span class="sxs-lookup"><span data-stu-id="774c8-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="774c8-178">Dado um IRepository de objetos Employee, o código pode executar as seguintes operações.</span><span class="sxs-lookup"><span data-stu-id="774c8-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="774c8-179">Como o código está usando uma interface (IRepository de Employee), podemos fornecer o código com diferentes implementações da interface.</span><span class="sxs-lookup"><span data-stu-id="774c8-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="774c8-180">Uma implementação pode ser uma implementação apoiada por EF4 e persistir objetos em um banco de dados Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="774c8-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="774c8-181">Uma implementação diferente (uma que usamos durante o teste) pode ser apoiada por uma lista na memória de objetos Employee.</span><span class="sxs-lookup"><span data-stu-id="774c8-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="774c8-182">A interface ajudará a atingir o isolamento no código.</span><span class="sxs-lookup"><span data-stu-id="774c8-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="774c8-183">Observe que a interface IRepository&lt;T&gt; não expõe uma operação de salvamento.</span><span class="sxs-lookup"><span data-stu-id="774c8-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="774c8-184">Como atualizar os objetos existentes?</span><span class="sxs-lookup"><span data-stu-id="774c8-184">How do we update existing objects?</span></span> <span data-ttu-id="774c8-185">Você pode se deparar com definições de IRepository que incluem a operação salvar, e as implementações desses repositórios precisarão manter imediatamente um objeto no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="774c8-186">No entanto, em muitos aplicativos, não queremos manter objetos individualmente.</span><span class="sxs-lookup"><span data-stu-id="774c8-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="774c8-187">Em vez disso, queremos dar vida aos objetos, talvez de diferentes repositórios, modificar esses objetos como parte de uma atividade de negócios e, em seguida, manter todos os objetos como parte de uma única operação atômica.</span><span class="sxs-lookup"><span data-stu-id="774c8-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="774c8-188">Felizmente, há um padrão para permitir esse tipo de comportamento.</span><span class="sxs-lookup"><span data-stu-id="774c8-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="774c8-189">O padrão de unidade de trabalho</span><span class="sxs-lookup"><span data-stu-id="774c8-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="774c8-190">Fowler diz que uma unidade de trabalho "manterá uma lista de objetos afetados por uma transação de negócios e coordenará a gravação de alterações e a resolução de problemas de simultaneidade".</span><span class="sxs-lookup"><span data-stu-id="774c8-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="774c8-191">É responsabilidade da unidade de trabalho controlar as alterações nos objetos que levamos à vida de um repositório e persistem todas as alterações feitas nos objetos quando dizemos a unidade de trabalho para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="774c8-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="774c8-192">Também é responsabilidade da unidade de trabalho pegar os novos objetos que adicionamos a todos os repositórios e inserir os objetos em um banco de dados, além de gerenciar a exclusão.</span><span class="sxs-lookup"><span data-stu-id="774c8-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="774c8-193">Se você já tiver feito qualquer trabalho com conjuntos de ADO.NETs de trabalho, ainda estará familiarizado com o padrão de unidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="774c8-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="774c8-194">Os conjuntos de dados ADO.NET tinham a capacidade de controlar nossas atualizações, exclusões e inserção de objetos DataRow e poderiam (com a ajuda de um TableAdapter) reconciliar todas as alterações em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="774c8-195">No entanto, os objetos DataSet modelam um subconjunto desconectado do banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="774c8-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="774c8-196">O padrão de unidade de trabalho exibe o mesmo comportamento, mas trabalha com objetos comerciais e objetos de domínio que são isolados do código de acesso a dados e que não sabem do banco.</span><span class="sxs-lookup"><span data-stu-id="774c8-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="774c8-197">Uma abstração para modelar a unidade de trabalho no código .NET pode ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="774c8-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="774c8-198">Ao expor referências de repositório da unidade de trabalho, podemos garantir que uma única unidade de objeto de trabalho tenha a capacidade de acompanhar todas as entidades materializadas durante uma transação de negócios.</span><span class="sxs-lookup"><span data-stu-id="774c8-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="774c8-199">A implementação do método Commit para uma unidade real de trabalho é onde toda a mágica acontece para reconciliar as alterações na memória com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="774c8-200">Dada uma referência de IUnitOfWork, o código pode fazer alterações nos objetos comerciais recuperados de um ou mais repositórios e salvar todas as alterações usando a operação de confirmação atômica.</span><span class="sxs-lookup"><span data-stu-id="774c8-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="774c8-201">O padrão de carga lenta</span><span class="sxs-lookup"><span data-stu-id="774c8-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="774c8-202">Fowler usa a carga lenta de nome para descrever "um objeto que não contém todos os dados necessários, mas sabe como obtê-lo".</span><span class="sxs-lookup"><span data-stu-id="774c8-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="774c8-203">O carregamento lento transparente é um recurso importante a ser criado ao escrever código comercial de teste e trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="774c8-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="774c8-204">Como exemplo, considere o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="774c8-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="774c8-205">Como a coleção de cartões de expostos é populada?</span><span class="sxs-lookup"><span data-stu-id="774c8-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="774c8-206">Há duas respostas possíveis.</span><span class="sxs-lookup"><span data-stu-id="774c8-206">There are two possible answers.</span></span> <span data-ttu-id="774c8-207">Uma resposta é que o repositório de funcionários, quando solicitado a buscar um funcionário, emite uma consulta para recuperar o funcionário junto com as informações do cartão de horário associado do funcionário.</span><span class="sxs-lookup"><span data-stu-id="774c8-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="774c8-208">Em bancos de dados relacionais, isso geralmente requer uma consulta com uma cláusula JOIN e pode resultar na recuperação de mais informações do que um aplicativo precisa.</span><span class="sxs-lookup"><span data-stu-id="774c8-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="774c8-209">E se o aplicativo nunca precisar tocar na propriedade de cartões de esquers?</span><span class="sxs-lookup"><span data-stu-id="774c8-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="774c8-210">Uma segunda resposta é carregar a propriedade de cartões de esquers "sob demanda".</span><span class="sxs-lookup"><span data-stu-id="774c8-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="774c8-211">Esse carregamento lento é implícito e transparente para a lógica de negócios porque o código não invoca APIs especiais para recuperar informações de cartão de tempo.</span><span class="sxs-lookup"><span data-stu-id="774c8-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="774c8-212">O código pressupõe que as informações do cartão de tempo estejam presentes quando necessário.</span><span class="sxs-lookup"><span data-stu-id="774c8-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="774c8-213">Há alguma mágica envolvida com o carregamento lento que geralmente envolve a interceptação de tempo de execução de invocações de método.</span><span class="sxs-lookup"><span data-stu-id="774c8-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="774c8-214">O código de interceptação é responsável por conversar com o banco de dados e recuperar informações de cartão de tempo, deixando a lógica de negócios livre para ser lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="774c8-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="774c8-215">Essa mágica de carga lenta permite que o código comercial se isole de operações de recuperação de dados e resulta em um código mais contestável.</span><span class="sxs-lookup"><span data-stu-id="774c8-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="774c8-216">A desvantagem de uma carga lenta é que, quando um *aplicativo precisar* das informações do cartão de tempo, o código executará uma consulta adicional.</span><span class="sxs-lookup"><span data-stu-id="774c8-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="774c8-217">Isso não é uma preocupação para muitos aplicativos, mas para aplicativos sensíveis ao desempenho ou aplicativos que executam um loop por meio de um número de objetos Employee e a execução de uma consulta para recuperar cartões de tempo durante cada iteração do loop (um problema geralmente conhecido como N + 1 problema de consulta), o carregamento lento é um arrastar.</span><span class="sxs-lookup"><span data-stu-id="774c8-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="774c8-218">Nesses cenários, um aplicativo pode querer carregar cuidadosamente as informações de cartão de tempo da maneira mais eficiente possível.</span><span class="sxs-lookup"><span data-stu-id="774c8-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="774c8-219">Felizmente, veremos como o EF4 dá suporte a cargas lentas implícitas e cargas de rápido eficiência à medida que passamos para a próxima seção e implementamos esses padrões.</span><span class="sxs-lookup"><span data-stu-id="774c8-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="774c8-220">Implementando padrões com o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="774c8-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="774c8-221">A boa notícia é que todos os padrões de design descritos na última seção são simples de implementar com EF4.</span><span class="sxs-lookup"><span data-stu-id="774c8-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="774c8-222">Para demonstrar, vamos usar um aplicativo MVC simples do ASP.NET para editar e exibir funcionários e suas informações de cartão de ponto associadas.</span><span class="sxs-lookup"><span data-stu-id="774c8-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="774c8-223">Começaremos usando os seguintes "objetos antigos do CLR" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="774c8-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="774c8-224">Essas definições de classe serão ligeiramente alteradas à medida que explorarmos diferentes abordagens e recursos do EF4, mas a intenção é manter essas classes como a Persistence que desconhecem (PI) o mais possível.</span><span class="sxs-lookup"><span data-stu-id="774c8-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="774c8-225">Um objeto PI não sabe *como*, ou mesmo *se*, o estado que ele mantém reside dentro de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="774c8-226">PI e POCOs se encontram em mãos com software de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="774c8-227">Os objetos que usam uma abordagem POCO são menos restritos, mais flexíveis e mais fáceis de testar, pois eles podem operar sem um banco de dados presente.</span><span class="sxs-lookup"><span data-stu-id="774c8-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="774c8-228">Com o POCOs em vigor, podemos criar um Modelo de Dados de Entidade (EDM) no Visual Studio (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="774c8-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="774c8-229">Não usaremos o EDM para gerar código para nossas entidades.</span><span class="sxs-lookup"><span data-stu-id="774c8-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="774c8-230">Em vez disso, queremos usar as entidades que carinhosamentemos manualmente.</span><span class="sxs-lookup"><span data-stu-id="774c8-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="774c8-231">Usaremos apenas o EDM para gerar nosso esquema de banco de dados e fornecer os metadados que o EF4 precisa para mapear objetos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![test_01 EF](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="774c8-233">**Figura 1**</span><span class="sxs-lookup"><span data-stu-id="774c8-233">**Figure 1**</span></span>

<span data-ttu-id="774c8-234">Observação: se você quiser desenvolver o modelo do EDM primeiro, será possível gerar um código limpo e POCO do EDM.</span><span class="sxs-lookup"><span data-stu-id="774c8-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="774c8-235">Você pode fazer isso com uma extensão do Visual Studio 2010 fornecida pela equipe de programação de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="774c8-236">Para baixar a extensão, inicie o Gerenciador de extensões no menu ferramentas no Visual Studio e pesquise a galeria online de modelos para "POCO" (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="774c8-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="774c8-237">Há vários modelos de POCO disponíveis para o EF.</span><span class="sxs-lookup"><span data-stu-id="774c8-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="774c8-238">Para obter mais informações sobre como usar o modelo, consulte o [modelo "Walkthrough: poco para o Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="774c8-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![test_02 EF](~/ef6/media/eftest-02.png)

<span data-ttu-id="774c8-240">**Figura 2**</span><span class="sxs-lookup"><span data-stu-id="774c8-240">**Figure 2**</span></span>

<span data-ttu-id="774c8-241">Deste ponto de partida do POCO, exploraremos duas abordagens diferentes para o código que pode ser testado.</span><span class="sxs-lookup"><span data-stu-id="774c8-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="774c8-242">A primeira abordagem que chamo a abordagem do EF porque ela aproveita abstrações da API Entity Framework para implementar unidades de trabalho e repositórios.</span><span class="sxs-lookup"><span data-stu-id="774c8-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="774c8-243">Na segunda abordagem, criaremos nossas próprias abstrações de repositório personalizadas e, em seguida, veremos as vantagens e desvantagens de cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="774c8-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="774c8-244">Vamos começar explorando a abordagem do EF.</span><span class="sxs-lookup"><span data-stu-id="774c8-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="774c8-245">Uma implementação centrada no EF</span><span class="sxs-lookup"><span data-stu-id="774c8-245">An EF Centric Implementation</span></span>

<span data-ttu-id="774c8-246">Considere a seguinte ação do controlador de um projeto MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="774c8-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="774c8-247">A ação recupera um objeto Employee e retorna um resultado para exibir uma exibição detalhada do funcionário.</span><span class="sxs-lookup"><span data-stu-id="774c8-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="774c8-248">O código está sendo testado?</span><span class="sxs-lookup"><span data-stu-id="774c8-248">Is the code testable?</span></span> <span data-ttu-id="774c8-249">Há pelo menos dois testes que seriam necessários para verificar o comportamento da ação.</span><span class="sxs-lookup"><span data-stu-id="774c8-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="774c8-250">Primeiro, gostaríamos de verificar se a ação retorna o modo de exibição correto – um teste fácil.</span><span class="sxs-lookup"><span data-stu-id="774c8-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="774c8-251">Também queremos escrever um teste para verificar se a ação recupera o funcionário correto e gostaríamos de fazer isso sem executar o código para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="774c8-252">Lembre-se de que desejamos isolar o código em teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="774c8-253">O isolamento garantirá que o teste não falhe devido a um bug no código de acesso a dados ou na configuração do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="774c8-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="774c8-254">Se o teste falhar, sabemos que temos um bug na lógica do controlador e não em um componente de sistema de nível inferior.</span><span class="sxs-lookup"><span data-stu-id="774c8-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="774c8-255">Para obter o isolamento, precisaremos de algumas abstrações como as interfaces que apresentamos anteriormente para repositórios e unidades de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="774c8-256">Lembre-se de que o padrão de repositório foi projetado para mediar entre objetos de domínio e a camada de mapeamento de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="774c8-257">Neste cenário, EF4 *é* a camada de mapeamento de dados e já fornece uma abstração semelhante ao repositório chamada IObjectSet&lt;t&gt; (do namespace System. Data. Objects).</span><span class="sxs-lookup"><span data-stu-id="774c8-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="774c8-258">A definição da interface é parecida com a seguinte.</span><span class="sxs-lookup"><span data-stu-id="774c8-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="774c8-259">IObjectSet&lt;T&gt; atende aos requisitos de um repositório porque ele se assemelha a uma coleção de objetos (via IEnumerable&lt;T&gt;) e fornece métodos para adicionar e remover objetos da coleção simulada.</span><span class="sxs-lookup"><span data-stu-id="774c8-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="774c8-260">Os métodos Attach e Detach expõem recursos adicionais da API EF4.</span><span class="sxs-lookup"><span data-stu-id="774c8-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="774c8-261">Para usar IObjectSet&lt;T&gt; como a interface para repositórios, precisamos de uma unidade de abstração de trabalho para associar repositórios.</span><span class="sxs-lookup"><span data-stu-id="774c8-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="774c8-262">Uma implementação concreta dessa interface se comunicará com SQL Server e será fácil de criar usando a classe ObjectContext de EF4.</span><span class="sxs-lookup"><span data-stu-id="774c8-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="774c8-263">A classe ObjectContext é a unidade real de trabalho na API EF4.</span><span class="sxs-lookup"><span data-stu-id="774c8-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="774c8-264">Trazer um IObjectSet&lt;T&gt; vida é tão fácil quanto invocar o método CreateObjectSet do objeto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="774c8-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="774c8-265">Em segundo plano, a estrutura usará os metadados que fornecemos no EDM para produzir um ObjectSet concreto&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="774c8-266">Vamos usar o retorno da interface IObjectSet&lt;T&gt;, pois isso ajudará a preservar a capacidade de teste no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="774c8-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="774c8-267">Essa implementação concreta é útil na produção, mas precisamos nos concentrar em como usaremos nossa abstração IUnitOfWork para facilitar os testes.</span><span class="sxs-lookup"><span data-stu-id="774c8-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="774c8-268">As duplicatas de teste</span><span class="sxs-lookup"><span data-stu-id="774c8-268">The Test Doubles</span></span>

<span data-ttu-id="774c8-269">Para isolar a ação do controlador, precisaremos da capacidade de alternar entre a unidade real de trabalho (apoiada por um ObjectContext) e um duplo de teste ou uma unidade de trabalho "falsa" (executando operações na memória).</span><span class="sxs-lookup"><span data-stu-id="774c8-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="774c8-270">A abordagem comum para executar esse tipo de alternância é não permitir que o controlador MVC instancie uma unidade de trabalho, mas, em vez disso, passe a unidade de trabalho para o controlador como um parâmetro de construtor.</span><span class="sxs-lookup"><span data-stu-id="774c8-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="774c8-271">O código acima é um exemplo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="774c8-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="774c8-272">Não permitimos que o controlador criasse sua dependência (a unidade de trabalho), mas injeta a dependência no controlador.</span><span class="sxs-lookup"><span data-stu-id="774c8-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="774c8-273">Em um projeto MVC, é comum usar uma fábrica de controlador personalizado em combinação com um contêiner inversão de controle (IoC) para automatizar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="774c8-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="774c8-274">Esses tópicos estão além do escopo deste artigo, mas você pode ler mais seguindo as referências no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="774c8-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="774c8-275">Uma implementação de unidade de trabalho falsa que podemos usar para teste pode ser semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="774c8-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="774c8-276">Observe que a unidade de trabalho falsa expõe uma propriedade confirmada.</span><span class="sxs-lookup"><span data-stu-id="774c8-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="774c8-277">Às vezes, é útil adicionar recursos a uma classe falsa que facilita os testes.</span><span class="sxs-lookup"><span data-stu-id="774c8-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="774c8-278">Nesse caso, é fácil observar se o código confirma uma unidade de trabalho verificando a propriedade confirmada.</span><span class="sxs-lookup"><span data-stu-id="774c8-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="774c8-279">Também precisaremos de um IObjectSet falso&lt;&gt; para armazenar objetos de funcionário e de cartão de na memória.</span><span class="sxs-lookup"><span data-stu-id="774c8-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="774c8-280">Podemos fornecer uma única implementação usando genéricos.</span><span class="sxs-lookup"><span data-stu-id="774c8-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="774c8-281">Esse teste delega a maior parte de seu trabalho a um objeto HashSet subjacente&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="774c8-282">Observe que IObjectSet&lt;T&gt; requer uma restrição genérica que impõe T como uma classe (um tipo de referência) e também nos obriga a implementar IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="774c8-283">É fácil fazer com que uma coleção na memória apareça como um IQueryable&lt;T&gt; usando o operador LINQ padrão de consulta.</span><span class="sxs-lookup"><span data-stu-id="774c8-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="774c8-284">Os testes</span><span class="sxs-lookup"><span data-stu-id="774c8-284">The Tests</span></span>

<span data-ttu-id="774c8-285">Os testes de unidade tradicionais usarão uma única classe de teste para manter todos os testes de todas as ações em um único controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="774c8-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="774c8-286">Podemos escrever esses testes ou qualquer tipo de teste de unidade, usando as falsificações de memória que criamos.</span><span class="sxs-lookup"><span data-stu-id="774c8-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="774c8-287">No entanto, para este artigo, evitaremos a abordagem de classe de teste monolítica e, em vez disso, agruparemos nossos testes para se concentrar em uma parte específica da funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="774c8-288">  Por exemplo, "criar novo funcionário" pode ser a funcionalidade que queremos testar, portanto, usaremos uma única classe de teste para verificar a ação do controlador único responsável por criar um novo funcionário.</span><span class="sxs-lookup"><span data-stu-id="774c8-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="774c8-289">Há um código de instalação comum que precisamos para todas essas classes de teste refinadas.</span><span class="sxs-lookup"><span data-stu-id="774c8-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="774c8-290">Por exemplo, sempre precisamos criar nossos repositórios na memória e uma unidade de trabalho falsa.</span><span class="sxs-lookup"><span data-stu-id="774c8-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="774c8-291">Também precisamos de uma instância do controlador de funcionário com a unidade de trabalho falsa injetada.</span><span class="sxs-lookup"><span data-stu-id="774c8-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="774c8-292">Vamos compartilhar esse código de instalação comum em classes de teste usando uma classe base.</span><span class="sxs-lookup"><span data-stu-id="774c8-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="774c8-293">O "objeto mãe" que usamos na classe base é um padrão comum para a criação de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="774c8-294">Uma mãe do objeto contém métodos de fábrica para instanciar entidades de teste para uso em vários acessórios de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="774c8-295">Podemos usar o EmployeeControllerTestBase como a classe base para vários acessórios de teste (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="774c8-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="774c8-296">Cada acessório de teste testará uma ação de controlador específica.</span><span class="sxs-lookup"><span data-stu-id="774c8-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="774c8-297">Por exemplo, um acessório de teste se concentrará no teste da ação de criação usada durante uma solicitação HTTP GET (para exibir a exibição para criar um funcionário), e um acessório diferente se concentrará na ação de criação usada em uma solicitação HTTP POST (para obter informações enviadas pelo usuário para criar um funcionário).</span><span class="sxs-lookup"><span data-stu-id="774c8-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="774c8-298">Cada classe derivada é responsável apenas pela configuração necessária em seu contexto específico e para fornecer as asserções necessárias para verificar os resultados de seu contexto de teste específico.</span><span class="sxs-lookup"><span data-stu-id="774c8-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![test_03 EF](~/ef6/media/eftest-03.png)

<span data-ttu-id="774c8-300">**Figura 3**</span><span class="sxs-lookup"><span data-stu-id="774c8-300">**Figure 3**</span></span>

<span data-ttu-id="774c8-301">A Convenção de nomenclatura e o estilo de teste apresentados aqui não são necessários para o código de testes – é apenas uma abordagem.</span><span class="sxs-lookup"><span data-stu-id="774c8-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="774c8-302">A Figura 4 mostra os testes em execução no plug-in do executor de teste do Jet cérebro para o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="774c8-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![test_04 EF](~/ef6/media/eftest-04.png)

<span data-ttu-id="774c8-304">**Figura 4**</span><span class="sxs-lookup"><span data-stu-id="774c8-304">**Figure 4**</span></span>

<span data-ttu-id="774c8-305">Com uma classe base para lidar com o código de instalação compartilhado, os testes de unidade para cada ação do controlador são pequenos e fáceis de escrever.</span><span class="sxs-lookup"><span data-stu-id="774c8-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="774c8-306">Os testes serão executados rapidamente (já que estamos executando operações na memória) e não devem falhar devido a questões ambientais ou de infraestrutura não relacionadas (porque isolamos a unidade em teste).</span><span class="sxs-lookup"><span data-stu-id="774c8-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="774c8-307">Nesses testes, a classe base faz a maior parte do trabalho de configuração.</span><span class="sxs-lookup"><span data-stu-id="774c8-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="774c8-308">Lembre-se de que o construtor da classe base cria o repositório na memória, uma unidade de trabalho falsa e uma instância da classe EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="774c8-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="774c8-309">A classe de teste deriva dessa classe base e se concentra nas especificidades de teste do método Create.</span><span class="sxs-lookup"><span data-stu-id="774c8-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="774c8-310">Nesse caso, as especificações resumem as etapas de "organizar, agir e declarar" que você verá em qualquer procedimento de teste de unidade:</span><span class="sxs-lookup"><span data-stu-id="774c8-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="774c8-311">Crie um objeto newEmployee para simular os dados de entrada.</span><span class="sxs-lookup"><span data-stu-id="774c8-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="774c8-312">Invoque a ação de criação do EmployeeController e passe o newEmployee.</span><span class="sxs-lookup"><span data-stu-id="774c8-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="774c8-313">Verifique se a ação criar produz os resultados esperados (o funcionário aparece no repositório).</span><span class="sxs-lookup"><span data-stu-id="774c8-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="774c8-314">O que nós criamos nos permite testar qualquer uma das ações EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="774c8-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="774c8-315">Por exemplo, quando escrevemos testes para a ação de índice do controlador de funcionário, podemos herdar da classe base de teste para estabelecer a mesma configuração base para nossos testes.</span><span class="sxs-lookup"><span data-stu-id="774c8-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="774c8-316">Novamente, a classe base criará o repositório na memória, a unidade de trabalho falsa e uma instância do EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="774c8-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="774c8-317">Os testes para a ação de índice só precisam se concentrar na invocação da ação de índice e no teste das qualidades do modelo que a ação retorna.</span><span class="sxs-lookup"><span data-stu-id="774c8-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="774c8-318">Os testes que estamos criando com falsificações na memória são orientados em relação ao teste do *estado* do software.</span><span class="sxs-lookup"><span data-stu-id="774c8-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="774c8-319">Por exemplo, ao testar a ação criar, desejamos inspecionar o estado do repositório após a execução da ação de criação – o repositório mantém o novo funcionário?</span><span class="sxs-lookup"><span data-stu-id="774c8-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="774c8-320">Posteriormente, veremos o teste baseado em interação.</span><span class="sxs-lookup"><span data-stu-id="774c8-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="774c8-321">O teste baseado em interação perguntará se o código sob teste chamou os métodos adequados em nossos objetos e passou os parâmetros corretos.</span><span class="sxs-lookup"><span data-stu-id="774c8-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="774c8-322">Por enquanto, vamos mudar para a capa de outro padrão de design – a carga lenta.</span><span class="sxs-lookup"><span data-stu-id="774c8-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="774c8-323">Carregamento adiantado e carregamento lento</span><span class="sxs-lookup"><span data-stu-id="774c8-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="774c8-324">Em algum ponto do aplicativo Web ASP.NET MVC, talvez você queira exibir as informações de um funcionário e incluir os cartões de tempo associados do funcionário.</span><span class="sxs-lookup"><span data-stu-id="774c8-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="774c8-325">Por exemplo, pode haver uma exibição de resumo do cartão de tempo que mostra o nome do funcionário e o número total de cartões de tempo no sistema.</span><span class="sxs-lookup"><span data-stu-id="774c8-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="774c8-326">Há várias abordagens que podemos adotar para implementar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="774c8-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="774c8-327">Projeção</span><span class="sxs-lookup"><span data-stu-id="774c8-327">Projection</span></span>

<span data-ttu-id="774c8-328">Uma abordagem fácil para criar o resumo é construir um modelo dedicado às informações que desejamos exibir na exibição.</span><span class="sxs-lookup"><span data-stu-id="774c8-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="774c8-329">Nesse cenário, o modelo pode ser semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="774c8-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="774c8-330">Observe que o EmployeeSummaryViewModel não é uma entidade – em outras palavras, não é algo que desejamos persistir no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="774c8-331">Vamos usar essa classe para embaralhar dados na exibição de maneira fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="774c8-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="774c8-332">O modelo de exibição é como um DTO (objeto de transferência de dados) porque ele não contém nenhum comportamento (nenhum método) – apenas Propriedades.</span><span class="sxs-lookup"><span data-stu-id="774c8-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="774c8-333">As propriedades conterão os dados que precisamos mover.</span><span class="sxs-lookup"><span data-stu-id="774c8-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="774c8-334">É fácil instanciar esse modelo de exibição usando o operador de projeção padrão do LINQ – o operador SELECT.</span><span class="sxs-lookup"><span data-stu-id="774c8-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="774c8-335">Há dois recursos notáveis para o código acima.</span><span class="sxs-lookup"><span data-stu-id="774c8-335">There are two notable features to the above code.</span></span> <span data-ttu-id="774c8-336">Primeiro – o código é fácil de testar, pois ainda é fácil observar e isolar.</span><span class="sxs-lookup"><span data-stu-id="774c8-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="774c8-337">O operador Select também funciona bem em nossas falsificações na memória como faz com a unidade real de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="774c8-338">O segundo recurso notável é como o código permite que o EF4 gere uma consulta única e eficiente para montar as informações de funcionário e de cartão de tempo juntas.</span><span class="sxs-lookup"><span data-stu-id="774c8-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="774c8-339">Nós carregamos informações de funcionários e informações de cartão de tempo no mesmo objeto sem usar nenhuma API especial.</span><span class="sxs-lookup"><span data-stu-id="774c8-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="774c8-340">O código simplesmente expressou as informações necessárias usando operadores LINQ padrão que funcionam em fontes de dados na memória, bem como em fontes de dados remotas.</span><span class="sxs-lookup"><span data-stu-id="774c8-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="774c8-341">EF4 foi capaz de converter as árvores de expressão geradas pela consulta LINQ e o compilador C\# em uma consulta T-SQL única e eficiente.</span><span class="sxs-lookup"><span data-stu-id="774c8-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="774c8-342">Há outras ocasiões em que não queremos trabalhar com um modelo de exibição ou objeto DTO, mas com entidades reais.</span><span class="sxs-lookup"><span data-stu-id="774c8-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="774c8-343">Quando sabemos que precisamos de um funcionário *e* dos cartões de tempo do funcionário, podemos carregar os dados relacionados de maneira discreta e eficiente.</span><span class="sxs-lookup"><span data-stu-id="774c8-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="774c8-344">Carregamento adiantado explícito</span><span class="sxs-lookup"><span data-stu-id="774c8-344">Explicit Eager Loading</span></span>

<span data-ttu-id="774c8-345">Quando desejarmos carregar cuidadosamente as informações de entidade relacionadas, precisamos de um mecanismo de lógica comercial (ou, neste cenário, lógica de ação do controlador) para expressar seu desejo para o repositório.</span><span class="sxs-lookup"><span data-stu-id="774c8-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="774c8-346">A classe T&gt; do EF4 ObjectQuery&lt;define um método include para especificar os objetos relacionados a serem recuperados durante uma consulta.</span><span class="sxs-lookup"><span data-stu-id="774c8-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="774c8-347">Lembre-se de que o ObjectContext EF4 expõe entidades por meio da classe concreta&lt;T&gt; que herda de ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="774c8-348">  Se estivesse usando o ObjectSet&lt;T&gt; referências em nossa ação do controlador, poderíamos escrever o código a seguir para especificar um carregamento adiantado de informações de cartão de tempo para cada funcionário.</span><span class="sxs-lookup"><span data-stu-id="774c8-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="774c8-349">No entanto, como estamos tentando manter nosso code-testando, não estamos expondo o ObjectSet&lt;T&gt; de fora da classe real de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="774c8-350">Em vez disso, nós confiamos na interface IObjectSet&lt;T&gt;, que é mais fácil de ser falsificada, mas IObjectSet&lt;T&gt; não define um método include.</span><span class="sxs-lookup"><span data-stu-id="774c8-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="774c8-351">A beleza do LINQ é que podemos criar nosso próprio operador include.</span><span class="sxs-lookup"><span data-stu-id="774c8-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="774c8-352">Observe que esse operador include é definido como um método de extensão para IQueryable&lt;T&gt; em vez de IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="774c8-353">Isso nos dá a capacidade de usar o método com uma variedade maior de tipos possíveis, incluindo IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, objectquerer&lt;T&gt;e ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="774c8-354">No evento, a sequência subjacente não é uma EF4 de Object&gt;&lt;Query original, mas não há nenhum dano feito e o operador include é uma operação não operacional.</span><span class="sxs-lookup"><span data-stu-id="774c8-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="774c8-355">Se a sequência subjacente *for* uma objectquery&lt;t&gt; (ou derivada de objectquery&lt;t&gt;), EF4 verá nosso requisito para dados adicionais e formulará a consulta SQL apropriada.</span><span class="sxs-lookup"><span data-stu-id="774c8-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="774c8-356">Com esse novo operador em vigor, podemos solicitar explicitamente uma carga adiantada de informações de cartão de tempo do repositório.</span><span class="sxs-lookup"><span data-stu-id="774c8-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="774c8-357">Quando executado em um ObjectContext real, o código produz a seguinte consulta única.</span><span class="sxs-lookup"><span data-stu-id="774c8-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="774c8-358">A consulta coleta informações suficientes do banco de dados em uma viagem para materializar os objetos Employee e preencher totalmente sua propriedade Timecards.</span><span class="sxs-lookup"><span data-stu-id="774c8-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="774c8-359">A grande notícia é que o código dentro do método de ação permanece totalmente testado.</span><span class="sxs-lookup"><span data-stu-id="774c8-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="774c8-360">Não precisamos fornecer nenhum recurso adicional para nossas falsificações para dar suporte ao operador include.</span><span class="sxs-lookup"><span data-stu-id="774c8-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="774c8-361">A má notícia é que tivemos que usar o operador include dentro do código que queríamos manter a persistência que desconhecem.</span><span class="sxs-lookup"><span data-stu-id="774c8-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="774c8-362">Este é um exemplo primo do tipo de compensações que você precisará avaliar ao criar código de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="774c8-363">Há ocasiões em que você precisa deixar a persistência se preocupar com o vazamento fora da abstração do repositório para atender às metas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="774c8-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="774c8-364">A alternativa ao carregamento adiantado é o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="774c8-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="774c8-365">O carregamento lento significa que *não* precisamos do nosso código comercial para anunciar explicitamente o requisito de dados associados.</span><span class="sxs-lookup"><span data-stu-id="774c8-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="774c8-366">Em vez disso, usamos nossas entidades no aplicativo e, se forem necessários dados adicionais Entity Framework carregará os dados sob demanda.</span><span class="sxs-lookup"><span data-stu-id="774c8-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="774c8-367">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="774c8-367">Lazy Loading</span></span>

<span data-ttu-id="774c8-368">É fácil imaginar um cenário em que não sabemos quais dados uma parte da lógica comercial precisará.</span><span class="sxs-lookup"><span data-stu-id="774c8-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="774c8-369">Podemos saber que a lógica precisa de um objeto Employee, mas podemos ramificar em caminhos de execução diferentes em que alguns desses caminhos exigem informações de cartão de tempo do funcionário e outros não.</span><span class="sxs-lookup"><span data-stu-id="774c8-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="774c8-370">Cenários como esse são perfeitos para carregamento lento implícito porque os dados aparecem de forma mágica conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="774c8-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="774c8-371">O carregamento lento, também conhecido como carregamento adiado, coloca alguns requisitos em nossos objetos de entidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="774c8-372">POCOs com a verdadeira persistência ignorância não enfrentaria nenhum requisito da camada de persistência, mas a verdadeira persistência ignorância é praticamente impossível de alcançar.</span><span class="sxs-lookup"><span data-stu-id="774c8-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="774c8-373">  Em vez disso, medimos ignorância de persistência em graus relativos.</span><span class="sxs-lookup"><span data-stu-id="774c8-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="774c8-374">Seria uma pena se fosse necessário herdar de uma classe base orientada à persistência ou usar uma coleção especializada para obter o carregamento lento em POCOs.</span><span class="sxs-lookup"><span data-stu-id="774c8-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="774c8-375">Felizmente, o EF4 tem uma solução menos invasiva.</span><span class="sxs-lookup"><span data-stu-id="774c8-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="774c8-376">Praticamente não detectável</span><span class="sxs-lookup"><span data-stu-id="774c8-376">Virtually Undetectable</span></span>

<span data-ttu-id="774c8-377">Ao usar objetos POCO, o EF4 pode gerar dinamicamente proxies de tempo de execução para entidades.</span><span class="sxs-lookup"><span data-stu-id="774c8-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="774c8-378">Esses proxies envolvem invisivelmente o POCOs materializado e fornecem serviços adicionais interceptando cada propriedade Get e Set para executar trabalho adicional.</span><span class="sxs-lookup"><span data-stu-id="774c8-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="774c8-379">Um desses serviços é o recurso de carregamento lento que estamos procurando.</span><span class="sxs-lookup"><span data-stu-id="774c8-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="774c8-380">Outro serviço é um mecanismo de controle de alterações eficiente que pode registrar quando o programa altera os valores de propriedade de uma entidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="774c8-381">A lista de alterações é usada pelo ObjectContext durante o método SaveChanges para manter todas as entidades modificadas usando comandos UPDATE.</span><span class="sxs-lookup"><span data-stu-id="774c8-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="774c8-382">No entanto, para que esses proxies funcionem, eles precisam de uma maneira de se conectar às operações Get e set de propriedade em uma entidade, e os proxies atingem essa meta por meio da substituição de membros virtuais.</span><span class="sxs-lookup"><span data-stu-id="774c8-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="774c8-383">Portanto, se quisermos que o carregamento lento implícito e o controle de alterações eficiente, precisamos voltar para nossas definições de classe POCO e marcar propriedades como virtuais.</span><span class="sxs-lookup"><span data-stu-id="774c8-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="774c8-384">Ainda podemos dizer que a entidade Employee é, na maioria das vezes, que desconhecem persistence.</span><span class="sxs-lookup"><span data-stu-id="774c8-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="774c8-385">O único requisito é usar membros virtuais e isso não afeta a capacidade de teste do código.</span><span class="sxs-lookup"><span data-stu-id="774c8-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="774c8-386">Não precisamos derivar de nenhuma classe base especial ou até mesmo usar uma coleção especial dedicada ao carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="774c8-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="774c8-387">Como o código demonstra, qualquer classe implementando ICollection&lt;T&gt; está disponível para armazenar entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="774c8-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="774c8-388">Também há uma pequena alteração que precisamos fazer dentro de nossa unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="774c8-389">O carregamento lento é *desativado* por padrão ao trabalhar diretamente com um objeto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="774c8-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="774c8-390">Há uma propriedade que podemos definir na propriedade ContextOptions para habilitar o carregamento adiado e podemos definir essa propriedade dentro de nossa unidade real de trabalho se quisermos habilitar o carregamento lento em todos os lugares.</span><span class="sxs-lookup"><span data-stu-id="774c8-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="774c8-391">Com o carregamento lento implícito habilitado, o código do aplicativo pode usar um funcionário e os cartões de tempo associados do funcionário enquanto o tranqüilamente restante não conhece o trabalho necessário para o EF carregar os dados adicionais.</span><span class="sxs-lookup"><span data-stu-id="774c8-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="774c8-392">O carregamento lento torna o código do aplicativo mais fácil de escrever e, com a mágica do proxy, o código permanece completamente testado.</span><span class="sxs-lookup"><span data-stu-id="774c8-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="774c8-393">As falsificações na memória da unidade de trabalho podem simplesmente pré-carregar entidades falsas com dados associados quando necessário durante um teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="774c8-394">Neste ponto, vamos transformar nossa atenção na criação de repositórios usando IObjectSet&lt;T&gt; e examinar abstrações para ocultar todos os sinais da estrutura de persistência.</span><span class="sxs-lookup"><span data-stu-id="774c8-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="774c8-395">Repositórios personalizados</span><span class="sxs-lookup"><span data-stu-id="774c8-395">Custom Repositories</span></span>

<span data-ttu-id="774c8-396">Quando apresentamos inicialmente o padrão de design de unidade de trabalho neste artigo, fornecemos um código de exemplo para o que seria a unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="774c8-397">Vamos apresentar essa ideia original usando o cenário de cartão de horário de funcionário e funcionário com o qual trabalhamos.</span><span class="sxs-lookup"><span data-stu-id="774c8-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="774c8-398">A principal diferença entre essa unidade de trabalho e a unidade de trabalho que criamos na última seção é como essa unidade de trabalho não usa nenhuma abstração da estrutura EF4 (não há IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="774c8-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="774c8-399">IObjectSet&lt;T&gt; funciona bem como uma interface de repositório, mas a API que ele expõe pode não alinhar perfeitamente com as necessidades do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="774c8-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="774c8-400">Nesta próxima abordagem, representaremos repositórios usando uma abstração personalizada de IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="774c8-401">Muitos desenvolvedores que seguem o design controlado por testes, design controlado por comportamento e metodologias orientadas a domínio design preferem a abordagem IRepository&lt;T&gt; por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="774c8-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="774c8-402">Primeiro, a interface IRepository&lt;T&gt; representa uma camada de "anticorrupção".</span><span class="sxs-lookup"><span data-stu-id="774c8-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="774c8-403">Conforme descrito por Eric Evans em seu livro de design controlado por domínio, uma camada anticorrupção mantém seu código de domínio longe das APIs de infraestrutura, como uma API de persistência.</span><span class="sxs-lookup"><span data-stu-id="774c8-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="774c8-404">Em segundo lugar, os desenvolvedores podem criar métodos no repositório que atendam às necessidades exatas de um aplicativo (como descoberto durante a gravação de testes).</span><span class="sxs-lookup"><span data-stu-id="774c8-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="774c8-405">Por exemplo, em geral, podemos precisar localizar uma única entidade usando um valor de ID, para que possamos adicionar um método FindById à interface do repositório.</span><span class="sxs-lookup"><span data-stu-id="774c8-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="774c8-406">  Nossa definição IRepository&lt;T&gt; será parecida com a seguinte.</span><span class="sxs-lookup"><span data-stu-id="774c8-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="774c8-407">Observe que vamos voltar para usando uma interface IQueryable&lt;T&gt; para expor coleções de entidades.</span><span class="sxs-lookup"><span data-stu-id="774c8-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="774c8-408">IQueryable&lt;T&gt; permite que as árvores de expressões LINQ fluam para o provedor EF4 e fornecem ao provedor uma visão holística da consulta.</span><span class="sxs-lookup"><span data-stu-id="774c8-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="774c8-409">Uma segunda opção seria retornar IEnumerable&lt;T&gt;, o que significa que o provedor do EF4 LINQ só verá as expressões criadas dentro do repositório.</span><span class="sxs-lookup"><span data-stu-id="774c8-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="774c8-410">Qualquer agrupamento, ordenação e projeção feitos fora do repositório não será composto no comando SQL enviado ao banco de dados, o que pode prejudicar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="774c8-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="774c8-411">Por outro lado, um repositório que retorna apenas IEnumerable&lt;T&gt; resultados nunca surpreenderá você com um novo comando SQL.</span><span class="sxs-lookup"><span data-stu-id="774c8-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="774c8-412">Ambas as abordagens funcionarão e ambas as abordagens permanecerão em teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="774c8-413">É simples fornecer uma única implementação da interface IRepository&lt;T&gt; usando genéricos e a API ObjectContext do EF4.</span><span class="sxs-lookup"><span data-stu-id="774c8-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="774c8-414">A abordagem IRepository&lt;T&gt; nos dá algum controle adicional sobre nossas consultas, pois um cliente precisa invocar um método para chegar a uma entidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="774c8-415">Dentro do método, poderíamos fornecer verificações adicionais e operadores LINQ para impor restrições de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="774c8-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="774c8-416">Observe que a interface tem duas restrições no parâmetro de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="774c8-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="774c8-417">A primeira restrição é a classe contras que os&gt;&lt;contras de IEntity e a segunda restrição forçam as entidades a implementarem a criação de uma ou uma abstração criada para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="774c8-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="774c8-418">A interface IEntity força as entidades a terem uma propriedade de ID legível e, em seguida, podemos usar essa propriedade no método FindById.</span><span class="sxs-lookup"><span data-stu-id="774c8-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="774c8-419">IEntity é definido com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="774c8-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="774c8-420">IEntity poderia ser considerado uma pequena violação do ignorância de persistência, já que nossas entidades são necessárias para implementar essa interface.</span><span class="sxs-lookup"><span data-stu-id="774c8-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="774c8-421">Lembre-se de que a persistência ignorância é sobre as compensações e, para muitos, a funcionalidade do FindById superará a restrição imposta pela interface.</span><span class="sxs-lookup"><span data-stu-id="774c8-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="774c8-422">A interface não tem impacto na capacidade de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="774c8-423">A instanciação de um IRepository dinâmico&lt;T&gt; requer um ObjectContext EF4, portanto, uma implementação concreta da unidade de trabalho deve gerenciar a instanciação.</span><span class="sxs-lookup"><span data-stu-id="774c8-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="774c8-424">Usando o repositório personalizado</span><span class="sxs-lookup"><span data-stu-id="774c8-424">Using the Custom Repository</span></span>

<span data-ttu-id="774c8-425">Usar nosso repositório personalizado não é significativamente diferente de usar o repositório com base em IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="774c8-426">Em vez de aplicar operadores LINQ diretamente a uma propriedade, primeiro precisaremos invocar os métodos do repositório para obter uma referência IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="774c8-427">Observe que o operador de inclusão personalizado que implementamos anteriormente funcionará sem alteração.</span><span class="sxs-lookup"><span data-stu-id="774c8-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="774c8-428">O método FindById do repositório remove a lógica duplicada das ações que tentam recuperar uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="774c8-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="774c8-429">Não há nenhuma diferença significativa na capacidade de teste das duas abordagens que examinamos.</span><span class="sxs-lookup"><span data-stu-id="774c8-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="774c8-430">Poderíamos fornecer implementações falsas de IRepository&lt;T&gt; criando classes concretas apoiadas por HashSet&lt;funcionário&gt;-exatamente como o que fizemos na última seção.</span><span class="sxs-lookup"><span data-stu-id="774c8-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="774c8-431">No entanto, alguns desenvolvedores preferem usar objetos fictícios e estruturas de objeto fictícios em vez de criar falsificações.</span><span class="sxs-lookup"><span data-stu-id="774c8-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="774c8-432">Veremos como usar as simulações para testar nossa implementação e discutir as diferenças entre as simulações e as falsificações na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="774c8-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="774c8-433">Teste com simulações</span><span class="sxs-lookup"><span data-stu-id="774c8-433">Testing with Mocks</span></span>

<span data-ttu-id="774c8-434">Há diferentes abordagens para criar o que Martin Fowler chama de "duplicata de teste".</span><span class="sxs-lookup"><span data-stu-id="774c8-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="774c8-435">Um duplo de teste (como um filme stunt duplo) é um objeto que você cria para "entrar" para objetos reais, de produção durante os testes.</span><span class="sxs-lookup"><span data-stu-id="774c8-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="774c8-436">Os repositórios na memória que criamos são duplicatas de teste para os repositórios que falam com SQL Server.</span><span class="sxs-lookup"><span data-stu-id="774c8-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="774c8-437">Vimos como usar essas duplicatas de teste durante os testes de unidade para isolar o código e manter os testes em execução rápida.</span><span class="sxs-lookup"><span data-stu-id="774c8-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="774c8-438">As duplicatas de teste que criamos têm implementações reais e em funcionamento.</span><span class="sxs-lookup"><span data-stu-id="774c8-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="774c8-439">Nos bastidores, cada um armazena uma coleção concreta de objetos, e eles adicionarão e removerão objetos dessa coleção à medida que manipularmos o repositório durante um teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="774c8-440">Alguns desenvolvedores gostam de criar suas duplicatas de teste dessa maneira – com código real e implementações de trabalho.</span><span class="sxs-lookup"><span data-stu-id="774c8-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="774c8-441">  Essas duplicatas de teste são o que chamamos de *falsificações*.</span><span class="sxs-lookup"><span data-stu-id="774c8-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="774c8-442">Eles têm implementações de trabalho, mas não são reais o suficiente para uso em produção.</span><span class="sxs-lookup"><span data-stu-id="774c8-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="774c8-443">Na verdade, o repositório falso não grava no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="774c8-444">O servidor SMTP falso não envia realmente uma mensagem de email pela rede.</span><span class="sxs-lookup"><span data-stu-id="774c8-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="774c8-445">Simulações versus falsificações</span><span class="sxs-lookup"><span data-stu-id="774c8-445">Mocks versus Fakes</span></span>

<span data-ttu-id="774c8-446">Há outro tipo de duplicata de teste conhecida como uma *simulação*.</span><span class="sxs-lookup"><span data-stu-id="774c8-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="774c8-447">Embora as falsificações tenham implementações em funcionamento, as simulações vêm sem implementação.</span><span class="sxs-lookup"><span data-stu-id="774c8-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="774c8-448">Com a ajuda de uma estrutura de objetos fictício, construímos esses objetos fictícios em tempo de execução e os usamos como duplicatas de teste.</span><span class="sxs-lookup"><span data-stu-id="774c8-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="774c8-449">Nesta seção, usaremos a estrutura de simulação de código-fonte aberto MOQ.</span><span class="sxs-lookup"><span data-stu-id="774c8-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="774c8-450">Veja um exemplo simples de como usar MOQ para criar dinamicamente um Double de teste para um repositório de funcionários.</span><span class="sxs-lookup"><span data-stu-id="774c8-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="774c8-451">Pedimos MOQ para um IRepository&lt;funcionário&gt; implementação e ele cria um dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="774c8-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="774c8-452">Podemos chegar ao objeto que implementa o IRepository&lt;Employee&gt; acessando a propriedade Object do objeto fictício&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="774c8-453">É esse objeto interno que podemos passar para nossos controladores, e eles não saberão se esse é um duplo de teste ou o real Repository.</span><span class="sxs-lookup"><span data-stu-id="774c8-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="774c8-454">Podemos invocar métodos no objeto da mesma forma que invocamos métodos em um objeto com uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="774c8-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="774c8-455">Você deve estar se perguntando o que o repositório de simulações fará quando invocarmos o método Add.</span><span class="sxs-lookup"><span data-stu-id="774c8-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="774c8-456">Como não há nenhuma implementação por trás do objeto fictício, Add não faz nada.</span><span class="sxs-lookup"><span data-stu-id="774c8-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="774c8-457">Não há nenhuma coleção concreta nos bastidores, como tivemos com as falsificações que escrevemos, portanto, o funcionário é Descartado.</span><span class="sxs-lookup"><span data-stu-id="774c8-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="774c8-458">E quanto ao valor de retorno de FindById?</span><span class="sxs-lookup"><span data-stu-id="774c8-458">What about the return value of FindById?</span></span> <span data-ttu-id="774c8-459">Nesse caso, o objeto fictício faz a única coisa que ele pode fazer, que é retornar um valor padrão.</span><span class="sxs-lookup"><span data-stu-id="774c8-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="774c8-460">Como estamos retornando um tipo de referência (um funcionário), o valor de retorno é um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="774c8-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="774c8-461">As simulações podem soar inúteis; no entanto, há mais dois recursos de simulações sobre os quais não falamos.</span><span class="sxs-lookup"><span data-stu-id="774c8-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="774c8-462">Primeiro, a estrutura MOQ registra todas as chamadas feitas no objeto fictício.</span><span class="sxs-lookup"><span data-stu-id="774c8-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="774c8-463">Posteriormente, no código, podemos perguntar MOQ se alguém invocou o método Add ou se alguém invocou o método FindById.</span><span class="sxs-lookup"><span data-stu-id="774c8-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="774c8-464">Veremos mais adiante como podemos usar esse recurso de gravação de "caixa preta" em testes.</span><span class="sxs-lookup"><span data-stu-id="774c8-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="774c8-465">O segundo grande recurso é como podemos usar MOQ para programar um objeto fictício com *expectativas*.</span><span class="sxs-lookup"><span data-stu-id="774c8-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="774c8-466">Uma expectativa diz ao objeto fictício como responder a qualquer interação determinada.</span><span class="sxs-lookup"><span data-stu-id="774c8-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="774c8-467">Por exemplo, podemos programar uma expectativa em nossa simulação e pedir que ela retorne um objeto Employee quando alguém invocar FindById.</span><span class="sxs-lookup"><span data-stu-id="774c8-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="774c8-468">A estrutura MOQ usa uma API de instalação e expressões lambda para programar essas expectativas.</span><span class="sxs-lookup"><span data-stu-id="774c8-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="774c8-469">Neste exemplo, solicitamos que o MOQ crie um repositório dinamicamente e, em seguida, programamos o repositório com uma expectativa.</span><span class="sxs-lookup"><span data-stu-id="774c8-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="774c8-470">A expectativa diz ao objeto fictício para retornar um novo objeto Employee com um valor de ID 5 quando alguém invoca o método FindById passando um valor de 5.</span><span class="sxs-lookup"><span data-stu-id="774c8-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="774c8-471">Esse teste passa, e não precisamos criar uma implementação completa para falsificar IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="774c8-472">Vamos revisitar os testes que escrevemos anteriormente e reutilizá-los para usar simulações em vez de falsificações.</span><span class="sxs-lookup"><span data-stu-id="774c8-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="774c8-473">Assim como antes, usaremos uma classe base para configurar as partes comuns da infraestrutura de que precisamos para todos os testes do controlador.</span><span class="sxs-lookup"><span data-stu-id="774c8-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="774c8-474">O código de instalação permanece basicamente o mesmo.</span><span class="sxs-lookup"><span data-stu-id="774c8-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="774c8-475">Em vez de usar falsificações, usaremos MOQ para construir objetos fictícios.</span><span class="sxs-lookup"><span data-stu-id="774c8-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="774c8-476">A classe base organiza a unidade de simulação de trabalho para retornar um repositório fictício quando o código invoca a propriedade Employees.</span><span class="sxs-lookup"><span data-stu-id="774c8-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="774c8-477">O restante da configuração de simulação ocorrerá dentro dos acessórios de teste dedicados a cada cenário específico.</span><span class="sxs-lookup"><span data-stu-id="774c8-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="774c8-478">Por exemplo, o acessório de teste para a ação de índice configurará o repositório de simulações para retornar uma lista de funcionários quando a ação invocar o método FindAll do repositório fictício.</span><span class="sxs-lookup"><span data-stu-id="774c8-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="774c8-479">Exceto pelas expectativas, nossos testes são semelhantes aos testes que tínhamos antes.</span><span class="sxs-lookup"><span data-stu-id="774c8-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="774c8-480">No entanto, com a capacidade de gravação de uma estrutura fictícia, podemos abordar os testes de um ângulo diferente.</span><span class="sxs-lookup"><span data-stu-id="774c8-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="774c8-481">Veremos essa nova perspectiva na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="774c8-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="774c8-482">Teste de Estado versus interação</span><span class="sxs-lookup"><span data-stu-id="774c8-482">State versus Interaction Testing</span></span>

<span data-ttu-id="774c8-483">Há técnicas diferentes que você pode usar para testar o software com objetos fictícios.</span><span class="sxs-lookup"><span data-stu-id="774c8-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="774c8-484">Uma abordagem é usar o teste baseado em estado, que é o que fizemos neste documento até agora.</span><span class="sxs-lookup"><span data-stu-id="774c8-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="774c8-485">O teste baseado em estado faz asserções sobre o estado do software.</span><span class="sxs-lookup"><span data-stu-id="774c8-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="774c8-486">No último teste, invocamos um método de ação no controlador e fizemos uma asserção sobre o modelo que deveria criar.</span><span class="sxs-lookup"><span data-stu-id="774c8-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="774c8-487">Aqui estão alguns exemplos de estado de teste:</span><span class="sxs-lookup"><span data-stu-id="774c8-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="774c8-488">Verifique se o repositório contém o novo objeto Employee após a criação de execute.</span><span class="sxs-lookup"><span data-stu-id="774c8-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="774c8-489">Verifique se o modelo contém uma lista de todos os funcionários após a execução do índice.</span><span class="sxs-lookup"><span data-stu-id="774c8-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="774c8-490">Verifique se o repositório não contém um determinado funcionário após a execução da exclusão.</span><span class="sxs-lookup"><span data-stu-id="774c8-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="774c8-491">Outra abordagem que você verá com objetos fictícios é verificar as *interações*.</span><span class="sxs-lookup"><span data-stu-id="774c8-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="774c8-492">Embora o teste baseado em estado faça declarações sobre o estado dos objetos, o teste baseado em interação faz asserções sobre como os objetos interagem.</span><span class="sxs-lookup"><span data-stu-id="774c8-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="774c8-493">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="774c8-493">For example:</span></span>

-   <span data-ttu-id="774c8-494">Verifique se o controlador invoca o método Add do repositório quando CREATE executa.</span><span class="sxs-lookup"><span data-stu-id="774c8-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="774c8-495">Verifique se o controlador invoca o método FindAll do repositório quando o índice é executado.</span><span class="sxs-lookup"><span data-stu-id="774c8-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="774c8-496">Verifique se o controlador invoca a unidade do método de confirmação do trabalho para salvar as alterações quando a edição é executada.</span><span class="sxs-lookup"><span data-stu-id="774c8-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="774c8-497">O teste de interação geralmente requer menos dados de teste, porque não estamos na análise de coleções e verificando contagens.</span><span class="sxs-lookup"><span data-stu-id="774c8-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="774c8-498">Por exemplo, se soubermos que a ação detalhes invoca o método FindById do repositório com o valor correto, a ação provavelmente está se comportando corretamente.</span><span class="sxs-lookup"><span data-stu-id="774c8-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="774c8-499">Podemos verificar esse comportamento sem configurar dados de teste para retornar do FindById.</span><span class="sxs-lookup"><span data-stu-id="774c8-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="774c8-500">A única configuração necessária no acessório de teste acima é a configuração fornecida pela classe base.</span><span class="sxs-lookup"><span data-stu-id="774c8-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="774c8-501">Quando invocamos a ação do controlador, MOQ registrará as interações com o repositório fictício.</span><span class="sxs-lookup"><span data-stu-id="774c8-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="774c8-502">Usando a API Verify de MOQ, podemos perguntar MOQ se o controlador invocava FindById com o valor de ID apropriado.</span><span class="sxs-lookup"><span data-stu-id="774c8-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="774c8-503">Se o controlador não invocar o método ou invocar o método com um valor de parâmetro inesperado, o método Verify gerará uma exceção e o teste falhará.</span><span class="sxs-lookup"><span data-stu-id="774c8-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="774c8-504">Aqui está outro exemplo para verificar se a ação de criação invoca a confirmação na unidade de trabalho atual.</span><span class="sxs-lookup"><span data-stu-id="774c8-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="774c8-505">Um perigo do teste de interação é a tendência de especificar interações.</span><span class="sxs-lookup"><span data-stu-id="774c8-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="774c8-506">A capacidade do objeto fictício de registrar e verificar cada interação com o objeto fictício não significa que o teste deve tentar verificar cada interação.</span><span class="sxs-lookup"><span data-stu-id="774c8-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="774c8-507">Algumas interações são detalhes de implementação e você só deve verificar as interações *necessárias* para atender ao teste atual.</span><span class="sxs-lookup"><span data-stu-id="774c8-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="774c8-508">A escolha entre simulações ou falsificações depende em grande parte do sistema que você está testando e suas preferências pessoais (ou de equipe).</span><span class="sxs-lookup"><span data-stu-id="774c8-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="774c8-509">Os objetos fictícios podem reduzir drasticamente a quantidade de código de que você precisa para implementar duplicatas de teste, mas nem todos se sentem confortável em expectativas de programação e verificação de interações.</span><span class="sxs-lookup"><span data-stu-id="774c8-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="774c8-510">Conclusões</span><span class="sxs-lookup"><span data-stu-id="774c8-510">Conclusions</span></span>

<span data-ttu-id="774c8-511">Neste documento, demonstramos várias abordagens para criar um código que pode ser testado ao usar o Entity Framework ADO.NET para persistência de dados.</span><span class="sxs-lookup"><span data-stu-id="774c8-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="774c8-512">Podemos aproveitar abstrações internas como IObjectSet&lt;T&gt;ou criar nossas próprias abstrações, como IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="774c8-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="774c8-513">  Em ambos os casos, o suporte do POCO no ADO.NET Entity Framework 4,0 permite que os consumidores dessas abstrações permaneçam que desconhecem persistentes e altamente testados.</span><span class="sxs-lookup"><span data-stu-id="774c8-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="774c8-514">Recursos de EF4 adicionais, como carregamento lento implícito, permitem que o código de negócios e de serviço de aplicativo funcione sem se preocupar com os detalhes de um armazenamento de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="774c8-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="774c8-515">Por fim, as abstrações que criamos são fáceis de simular ou falsas dentro dos testes de unidade, e podemos usar essas duplicatas de teste para obter testes de execução rápida, altamente isolados e confiáveis.</span><span class="sxs-lookup"><span data-stu-id="774c8-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="774c8-516">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="774c8-516">Additional Resources</span></span>

-   <span data-ttu-id="774c8-517">Robert C. Martin, " [princípio de responsabilidade única](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="774c8-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="774c8-518">Martin Fowler, [Catálogo de padrões](https://www.martinfowler.com/eaaCatalog/index.html) de *padrões de arquitetura de aplicativos empresariais*</span><span class="sxs-lookup"><span data-stu-id="774c8-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="774c8-519">Griffin Caprio, " [injeção de dependência](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="774c8-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="774c8-520">Blog de programação de dados, " [passo a passos: desenvolvimento controlado por testes com o Entity Framework 4,0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="774c8-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="774c8-521">Blog de programação de dados, " [usando o repositório e os padrões de unidade de trabalho com o Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="774c8-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="774c8-522">Aaron Jensen, " [apresentando especificações de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="774c8-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="774c8-523">Eric Lee, " [BDD com MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="774c8-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="774c8-524">Eric Evans, " [design controlado por domínio](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="774c8-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="774c8-525">Martin Fowler, " [simulações não são stubs](https://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="774c8-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="774c8-526">Martin Fowler, " [duplicata de teste](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="774c8-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="774c8-527">MOQ</span><span class="sxs-lookup"><span data-stu-id="774c8-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="774c8-528">Sumi</span><span class="sxs-lookup"><span data-stu-id="774c8-528">Biography</span></span>

<span data-ttu-id="774c8-529">Scott Allen é membro da equipe técnica da Pluralsight e do fundador da OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="774c8-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="774c8-530">Em 15 anos de desenvolvimento de software comercial, Scott trabalhou em soluções para tudo, desde dispositivos incorporados de 8 bits até aplicativos Web ASP.NET altamente escalonáveis.</span><span class="sxs-lookup"><span data-stu-id="774c8-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="774c8-531">Você pode alcançar Scott em seu blog em OdeToCode ou no Twitter em [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="774c8-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
