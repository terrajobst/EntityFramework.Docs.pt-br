---
title: Capacidade de teste e o Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 0ddf72ab46e2d67dc8a9cf75cbd40430352c5210
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490526"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="52919-102">Capacidade de teste e o Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="52919-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="52919-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="52919-103">Scott Allen</span></span>

<span data-ttu-id="52919-104">Publicado em: Maio de 2010</span><span class="sxs-lookup"><span data-stu-id="52919-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="52919-105">Introdução</span><span class="sxs-lookup"><span data-stu-id="52919-105">Introduction</span></span>

<span data-ttu-id="52919-106">Este white paper descreve e demonstra como escrever código que podem ser testado com o ADO.NET Entity Framework 4.0 e o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="52919-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="52919-107">Este documento não tenta se concentrar em uma metodologia de teste específica, como o design orientado a testes (TDD) ou o design controlado por comportamento (BDD).</span><span class="sxs-lookup"><span data-stu-id="52919-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="52919-108">Em vez disso, este documento se concentra em como escrever código que usa o ADO.NET Entity Framework e ainda mais fácil isolar e testar de forma automática.</span><span class="sxs-lookup"><span data-stu-id="52919-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="52919-109">Vamos examinar os padrões comuns de design que facilitam a cenários de acesso de teste nos dados e ver como aplicar esses padrões ao usar a estrutura.</span><span class="sxs-lookup"><span data-stu-id="52919-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="52919-110">Também vamos examinar os recursos específicos do framework para ver como esses recursos podem trabalhar no código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="52919-111">O que é o código testável?</span><span class="sxs-lookup"><span data-stu-id="52919-111">What Is Testable Code?</span></span>

<span data-ttu-id="52919-112">A capacidade de verificar se uma parte do software usando testes de unidade automatizados oferece muitos benefícios desejáveis.</span><span class="sxs-lookup"><span data-stu-id="52919-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="52919-113">Todos sabem que bons testes reduzirá o número de defeitos de software em um aplicativo e o aumento de qualidade do aplicativo - mas ter testes de unidade em vigor vai muito além de apenas encontrar bugs.</span><span class="sxs-lookup"><span data-stu-id="52919-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="52919-114">Um conjunto de testes de unidade aprovados permite que uma equipe de desenvolvimento economizar tempo e permanece no controle de software que criam.</span><span class="sxs-lookup"><span data-stu-id="52919-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="52919-115">Uma equipe pode fazer alterações no código existente, o refactor, reestruturação e reestruturação de software para atender aos novos requisitos e adicionar novos componentes em um aplicativo ao mesmo tempo, sabendo que o conjunto de testes pode verificar o comportamento do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52919-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="52919-116">Testes de unidade são parte de um ciclo rápido feedback para facilitar a alteração e preservar a facilidade de manutenção de software, como complexidade aumenta.</span><span class="sxs-lookup"><span data-stu-id="52919-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="52919-117">Teste de unidade vem com um preço, no entanto.</span><span class="sxs-lookup"><span data-stu-id="52919-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="52919-118">Uma equipe deve investir o tempo para criar e manter os testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="52919-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="52919-119">A quantidade de esforço necessário para criar esses testes está diretamente relacionada para o **possibilidade de teste** do software subjacente.</span><span class="sxs-lookup"><span data-stu-id="52919-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="52919-120">Como é fácil testar o software?</span><span class="sxs-lookup"><span data-stu-id="52919-120">How easy is the software to test?</span></span> <span data-ttu-id="52919-121">Uma equipe de design de software com capacidade de teste em mente criará efetivos testes mais rápido do que a equipe que trabalham com software que não podem ser testado.</span><span class="sxs-lookup"><span data-stu-id="52919-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="52919-122">Com capacidade de teste em mente, a Microsoft projetou o ADO.NET Entity Framework 4.0 (EF4).</span><span class="sxs-lookup"><span data-stu-id="52919-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="52919-123">Isso não significa que os desenvolvedores escrever testes de unidade em relação ao código de estrutura em si.</span><span class="sxs-lookup"><span data-stu-id="52919-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="52919-124">Em vez disso, os objetivos de capacidade de teste para EF4 tornam fácil criar código testável que se baseia em cima do framework.</span><span class="sxs-lookup"><span data-stu-id="52919-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="52919-125">Antes de examinarmos exemplos específicos, vale a pena entender as qualidades de código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="52919-126">As qualidades de código testável</span><span class="sxs-lookup"><span data-stu-id="52919-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="52919-127">Código que é fácil de testar exibirão sempre pelo menos duas características.</span><span class="sxs-lookup"><span data-stu-id="52919-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="52919-128">Primeiro, que pode ser testado o código é fácil **observar**.</span><span class="sxs-lookup"><span data-stu-id="52919-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="52919-129">Dado um conjunto de entradas, deve ser fácil observar a saída do código.</span><span class="sxs-lookup"><span data-stu-id="52919-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="52919-130">Por exemplo, o seguinte método de teste é fácil porque o método retorna o resultado de um cálculo diretamente.</span><span class="sxs-lookup"><span data-stu-id="52919-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="52919-131">Um método de teste é difícil, se o método grava o valor computado em um soquete de rede, uma tabela de banco de dados ou um arquivo, como o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="52919-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="52919-132">O teste deve executar o trabalho adicional para recuperar o valor.</span><span class="sxs-lookup"><span data-stu-id="52919-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="52919-133">Em segundo lugar, o código testável é fácil **isolar**.</span><span class="sxs-lookup"><span data-stu-id="52919-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="52919-134">Vamos usar o pseudocódigo a seguir como um mau exemplo de código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="52919-135">O método é fácil observar – podemos passar uma apólice e verifique se que o valor retornado corresponde a um resultado esperado.</span><span class="sxs-lookup"><span data-stu-id="52919-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="52919-136">No entanto, para testar o método precisaremos instalar um banco de dados com o esquema correto e configurar o servidor SMTP, caso o método tenta enviar um email.</span><span class="sxs-lookup"><span data-stu-id="52919-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="52919-137">O teste de unidade apenas quer verificar se a lógica de cálculo dentro do método, mas o teste pode falhar porque o servidor de email está offline ou porque o servidor de banco de dados movido.</span><span class="sxs-lookup"><span data-stu-id="52919-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="52919-138">Ambas essas falhas estão relacionadas ao comportamento que deseja que o teste para verificar.</span><span class="sxs-lookup"><span data-stu-id="52919-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="52919-139">O comportamento é difícil de isolar.</span><span class="sxs-lookup"><span data-stu-id="52919-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="52919-140">Os desenvolvedores de software que lutam para escrever código testável geralmente se esforçar para manter uma separação de preocupações no código que escrevem.</span><span class="sxs-lookup"><span data-stu-id="52919-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="52919-141">O método acima deve se concentrar em cálculos de negócios e delegar os detalhes da implementação do banco de dados e email a outros componentes.</span><span class="sxs-lookup"><span data-stu-id="52919-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="52919-142">Robert Isso chama o princípio da responsabilidade única.</span><span class="sxs-lookup"><span data-stu-id="52919-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="52919-143">Um objeto deve encapsular uma responsabilidade única e estreita, como calcular o valor de uma política.</span><span class="sxs-lookup"><span data-stu-id="52919-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="52919-144">Todas as outras tarefas de banco de dados e notificação devem ser de responsabilidade de algum outro objeto.</span><span class="sxs-lookup"><span data-stu-id="52919-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="52919-145">Código escrito dessa maneira é mais fácil isolar porque ele se concentra em uma única tarefa.</span><span class="sxs-lookup"><span data-stu-id="52919-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="52919-146">No .NET, temos as abstrações, precisamos seguir o princípio da responsabilidade única e obter o isolamento.</span><span class="sxs-lookup"><span data-stu-id="52919-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="52919-147">Podemos usar as definições de interface e forçar o código para usar a abstração de interface em vez de um tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="52919-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="52919-148">Neste artigo, veremos como um método, como o exemplo de incorreto apresentado acima pode trabalhar com interfaces que *parecer* como eles se comunicará com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="52919-149">No entanto, no momento do teste, podemos pode substituir uma implementação fictícia que não se comunicar com o banco de dados, mas em vez disso, armazena dados na memória.</span><span class="sxs-lookup"><span data-stu-id="52919-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="52919-150">Essa implementação fictícia será isolar o código de problemas não relacionados no código de acesso a dados ou banco de dados na configuração.</span><span class="sxs-lookup"><span data-stu-id="52919-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="52919-151">Há benefícios adicionais de isolamento.</span><span class="sxs-lookup"><span data-stu-id="52919-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="52919-152">O cálculo de negócios no último método deve levar apenas alguns milissegundos para executar, mas o próprio teste pode ser executado por vários segundos como os saltos de código em torno de rede e fala para vários servidores.</span><span class="sxs-lookup"><span data-stu-id="52919-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="52919-153">Testes de unidade devem ser executado rapidamente para facilitar a pequenas alterações.</span><span class="sxs-lookup"><span data-stu-id="52919-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="52919-154">Testes de unidade devem também ser repetidos e não falha porque um componente não relacionado ao teste tem um problema.</span><span class="sxs-lookup"><span data-stu-id="52919-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="52919-155">Escrevendo código que seja fácil para observar e isolar significa que os desenvolvedores terão uma hora mais fácil de escrever testes para o código, gastar menos tempo aguardando testes a serem executados e mais importante, gastam menos tempo que rastrear bugs que não existem.</span><span class="sxs-lookup"><span data-stu-id="52919-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="52919-156">Esperamos que você pode avaliar os benefícios dos testes e entender as qualidades que exibe o código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="52919-157">Estamos prestes a abordar como escrever código que funciona com o EF4 para salvar dados em um banco de dados, permanecendo observáveis e fácil isolar, mas primeiro, limitarei nosso foco para discutir os designs que podem ser testados para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="52919-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="52919-158">Padrões de design para persistência de dados</span><span class="sxs-lookup"><span data-stu-id="52919-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="52919-159">Os dois exemplos incorretos apresentados anteriormente tinham muitas responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="52919-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="52919-160">O primeiro exemplo incorreto teria que realizar um cálculo *e* gravar em um arquivo.</span><span class="sxs-lookup"><span data-stu-id="52919-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="52919-161">O segundo exemplo ruim tinha que ler dados de um banco de dados *e* executam um cálculo de negócios *e* enviar email.</span><span class="sxs-lookup"><span data-stu-id="52919-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="52919-162">Criando métodos menores que separam questões e delegar a responsabilidade para outros componentes de você fazer grandes avanços na direção de escrever um código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="52919-163">O objetivo é criar a funcionalidade pela composição de ações de abstrações de pequenas e focadas.</span><span class="sxs-lookup"><span data-stu-id="52919-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="52919-164">Quando se trata de persistência de dados de pequeno e focadas abstrações que estamos procurando são tão comuns que eles já foram documentados como padrões de design.</span><span class="sxs-lookup"><span data-stu-id="52919-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="52919-165">Livro de Martin Fowler padrões de arquitetura de aplicações corporativas foi o primeiro trabalho para descrever esses padrões em versão impressa.</span><span class="sxs-lookup"><span data-stu-id="52919-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="52919-166">Forneceremos uma breve descrição desses padrões nas seções a seguir antes de mostrarmos como esses ADO.NET Entity Framework implementa e funciona com esses padrões.</span><span class="sxs-lookup"><span data-stu-id="52919-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="52919-167">O padrão de repositório</span><span class="sxs-lookup"><span data-stu-id="52919-167">The Repository Pattern</span></span>

<span data-ttu-id="52919-168">Fowler diz que um repositório "atua como mediador entre o domínio e os dados de camadas de mapeamento usando uma interface de coleção para acessar objetos de domínio".</span><span class="sxs-lookup"><span data-stu-id="52919-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="52919-169">A meta do padrão de repositório é isolar o código das minúcias de acesso a dados e, como vimos anteriormente isolamento é uma característica necessária para a capacidade de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="52919-170">A chave para o isolamento é como o repositório expõe objetos usando uma interface de coleção.</span><span class="sxs-lookup"><span data-stu-id="52919-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="52919-171">A lógica que você escreve para usar o repositório não tem ideia de como o repositório será materializar os objetos que você solicitar.</span><span class="sxs-lookup"><span data-stu-id="52919-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="52919-172">O repositório pode se comunicar com um banco de dados, ou ele pode simplesmente retornar objetos de uma coleção na memória.</span><span class="sxs-lookup"><span data-stu-id="52919-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="52919-173">Tudo o que seu código precisa saber é que o repositório é exibido manter a coleção, e você pode recuperar, adicionar e excluir objetos da coleção.</span><span class="sxs-lookup"><span data-stu-id="52919-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="52919-174">Em aplicativos .NET existentes um repositório concreto geralmente herda de uma interface genérica semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="52919-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="52919-175">Faremos algumas alterações na definição de interface quando podemos fornecer uma implementação para EF4, mas o conceito básico permanece o mesmo.</span><span class="sxs-lookup"><span data-stu-id="52919-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="52919-176">Código pode usar um repositório concreto que implementa essa interface para recuperar uma entidade pelo seu valor de chave primária, para recuperar uma coleção de entidades com base na avaliação de um predicado, ou simplesmente recuperar todas as entidades disponíveis.</span><span class="sxs-lookup"><span data-stu-id="52919-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="52919-177">O código também pode adicionar e remover entidades por meio da interface do repositório.</span><span class="sxs-lookup"><span data-stu-id="52919-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="52919-178">Dada uma objetos IRepository do funcionário, o código pode executar as operações a seguir.</span><span class="sxs-lookup"><span data-stu-id="52919-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="52919-179">Uma vez que o código está usando uma interface (IRepository do funcionário), podemos fornecer o código com diferentes implementações da interface.</span><span class="sxs-lookup"><span data-stu-id="52919-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="52919-180">Uma implementação pode ser uma implementação apoiada por EF4 e objetos persistentes em um banco de dados do Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="52919-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="52919-181">Uma implementação diferente (um que usamos durante o teste) pode apoio de um objeto de lista de funcionários na memória.</span><span class="sxs-lookup"><span data-stu-id="52919-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="52919-182">A interface ajudará a obter o isolamento no código.</span><span class="sxs-lookup"><span data-stu-id="52919-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="52919-183">Observe o IRepository&lt;T&gt; interface não expõe uma operação de salvamento.</span><span class="sxs-lookup"><span data-stu-id="52919-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="52919-184">Como podemos atualizar objetos existentes?</span><span class="sxs-lookup"><span data-stu-id="52919-184">How do we update existing objects?</span></span> <span data-ttu-id="52919-185">Você pode se deparar com definições de IRepository que incluem a operação de salvamento e implementações desses repositórios serão necessário persistir imediatamente um objeto no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="52919-186">No entanto, em muitos aplicativos não queremos manter objetos individualmente.</span><span class="sxs-lookup"><span data-stu-id="52919-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="52919-187">Em vez disso, queremos dar vida a objetos, talvez de diferentes repositórios, modificar esses objetos como parte de uma atividade de negócios e, em seguida, manter todos os objetos como parte de uma única operação atômica.</span><span class="sxs-lookup"><span data-stu-id="52919-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="52919-188">Felizmente, há um padrão para permitir que esse tipo de comportamento.</span><span class="sxs-lookup"><span data-stu-id="52919-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="52919-189">O padrão de unidade de trabalho</span><span class="sxs-lookup"><span data-stu-id="52919-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="52919-190">Fowler diz que uma unidade de trabalho "manterá uma lista de objetos afetados por uma transação comercial e coordena a gravação recusar alterações e a resolução de problemas de simultaneidade".</span><span class="sxs-lookup"><span data-stu-id="52919-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="52919-191">É responsabilidade da unidade de trabalho para controlar as alterações aos objetos podemos dar vida a partir de um repositório e manter quaisquer alterações feitas aos objetos quando dizemos que a unidade de trabalho para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="52919-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="52919-192">Também é responsabilidade da unidade de trabalho para levar os novos objetos que já adicionamos a todos os repositórios e inserir os objetos em um banco de dados e também gerenciar exclusão.</span><span class="sxs-lookup"><span data-stu-id="52919-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="52919-193">Se você já realizou qualquer trabalho com conjuntos de dados ADO.NET, em seguida, você já estará familiarizado com o padrão de unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="52919-194">Conjuntos de dados ADO.NET tinha a capacidade de controlar as nossas atualizações, exclusões e inserção de objetos DataRow e poderia (com a Ajuda de um TableAdapter) reconciliar todas as nossas alterações para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="52919-195">No entanto, objetos de conjunto de dados modelo um subconjunto desconectado do banco de dados subjacente.</span><span class="sxs-lookup"><span data-stu-id="52919-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="52919-196">O padrão de unidade de trabalho exibe o mesmo comportamento, mas funciona com objetos comerciais e objetos de domínio que são isolados do código de acesso a dados e não reconhece do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="52919-197">Uma abstração para modelar a unidade de trabalho no código do .NET pode parecer com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="52919-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="52919-198">Expondo as referências de repositório da unidade de trabalho, que podemos garantir uma única unidade de trabalho de objeto tem a capacidade de controlar todas as entidades materializadas durante uma transação comercial.</span><span class="sxs-lookup"><span data-stu-id="52919-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="52919-199">A implementação do método de confirmação para uma unidade real de trabalho é onde a toda a mágica acontece reconciliar as alterações de na memória com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="52919-200">Dada uma referência de IUnitOfWork, o código pode fazer alterações em objetos de negócios recuperados de um ou mais repositórios e salvar todas as alterações usando a operação de confirmação atômica.</span><span class="sxs-lookup"><span data-stu-id="52919-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="52919-201">O padrão de carregamento lento</span><span class="sxs-lookup"><span data-stu-id="52919-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="52919-202">Fowler usa o carregamento lento de nome para descrever "um objeto que não contém todos os dados você precisa, mas sabe como obtê-lo".</span><span class="sxs-lookup"><span data-stu-id="52919-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="52919-203">Carregamento lento transparente é um recurso importante para ter quando escrever código testável comercial e trabalhar com um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="52919-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="52919-204">Por exemplo, considere o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="52919-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="52919-205">Como a coleção de cartões de ponto é preenchida?</span><span class="sxs-lookup"><span data-stu-id="52919-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="52919-206">Existem duas possíveis respostas.</span><span class="sxs-lookup"><span data-stu-id="52919-206">There are two possible answers.</span></span> <span data-ttu-id="52919-207">Uma resposta é que o repositório de funcionário, quando for solicitado para buscar um funcionário, emite uma consulta para recuperar o funcionário junto com as informações do funcionário associado de cartão de ponto.</span><span class="sxs-lookup"><span data-stu-id="52919-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="52919-208">Bancos de dados relacionais isso geralmente requer uma consulta com uma cláusula JOIN e pode resultar em recuperar mais informações do que um aplicativo precisa.</span><span class="sxs-lookup"><span data-stu-id="52919-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="52919-209">E se o aplicativo nunca precisa tocar a propriedade de cartões de ponto?</span><span class="sxs-lookup"><span data-stu-id="52919-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="52919-210">Uma segunda resposta é carregar a propriedade de cartões de ponto "sob demanda".</span><span class="sxs-lookup"><span data-stu-id="52919-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="52919-211">Esse carregamento lento é implícita e transparente para a lógica de negócios, porque o código não chama APIs especiais para recuperar as informações de cartão de ponto.</span><span class="sxs-lookup"><span data-stu-id="52919-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="52919-212">O código pressupõe que as informações de cartão de ponto estão presentes quando necessário.</span><span class="sxs-lookup"><span data-stu-id="52919-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="52919-213">Há alguma mágica envolvida com o carregamento lento que geralmente envolve a interceptação de tempo de execução das invocações de método.</span><span class="sxs-lookup"><span data-stu-id="52919-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="52919-214">O código de interceptação é responsável por conversar com o banco de dados e recuperar informações de cartão de ponto, deixando a lógica de negócios livre para ser a lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="52919-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="52919-215">Essa mágica de carregamento lento permite que o código de negócios isolar próprio das operações de recuperação de dados e resulta em mais código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="52919-216">A desvantagem de um carregamento lento é que, quando um aplicativo *faz* precisa das informações de cartão de ponto de código executará uma consulta adicional.</span><span class="sxs-lookup"><span data-stu-id="52919-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="52919-217">Isso não é uma preocupação para muitos aplicativos, mas para aplicativos sensíveis ao desempenho ou aplicativos um loop por meio de um número de objetos de funcionário e execução de uma consulta para recuperar os cartões de tempo durante cada iteração do loop (um problema conhecido como N + 1 problema de consulta), o carregamento lento é uma operação de arrastar.</span><span class="sxs-lookup"><span data-stu-id="52919-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="52919-218">Nesses cenários, um aplicativo talvez queira carregar avidamente as informações de cartão de ponto da maneira mais eficiente possível.</span><span class="sxs-lookup"><span data-stu-id="52919-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="52919-219">Felizmente, veremos como EF4 dá suporte a ambas as cargas lentas implícitas e eficiente eager carrega como podemos mover para a próxima seção e implementar esses padrões.</span><span class="sxs-lookup"><span data-stu-id="52919-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="52919-220">Implementar os padrões com o Entity Framework</span><span class="sxs-lookup"><span data-stu-id="52919-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="52919-221">A boa notícia é que todos os padrões de design que descrevemos na última seção são simples de implementar com o EF4.</span><span class="sxs-lookup"><span data-stu-id="52919-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="52919-222">Para demonstrar, vamos usar um aplicativo simples do ASP.NET MVC para editar e exibir os funcionários e suas informações de cartão de ponto associado.</span><span class="sxs-lookup"><span data-stu-id="52919-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="52919-223">Vamos começar usando os seguinte "plain old CLR objects" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="52919-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="52919-224">Essas definições de classe mudarão ligeiramente conforme exploramos diferentes abordagens e os recursos do EF4, mas a intenção é manter essas classes como ignorados com persistência (PI) quanto possível.</span><span class="sxs-lookup"><span data-stu-id="52919-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="52919-225">Um objeto de PI não sabe *como*, ou até mesmo *se*, o estado em que ele contém reside dentro de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="52919-226">PI e POCOs andam de mãos dadas com o software que podem ser testado.</span><span class="sxs-lookup"><span data-stu-id="52919-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="52919-227">Objetos usando uma abordagem POCO são menos restritos, mais flexível e mais fáceis de testar porque eles podem operar sem um banco de dados presente.</span><span class="sxs-lookup"><span data-stu-id="52919-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="52919-228">Com os POCOs in-loco podemos criar um modelo de dados de entidade (EDM) no Visual Studio (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="52919-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="52919-229">Não usaremos o EDM para gerar código para nossas entidades.</span><span class="sxs-lookup"><span data-stu-id="52919-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="52919-230">Em vez disso, queremos usar as entidades que podemos carinhosamente criar manualmente.</span><span class="sxs-lookup"><span data-stu-id="52919-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="52919-231">Só usaremos o EDM para gerar nosso esquema de banco de dados e forneça os metadados do que ef4 precisa mapear objetos no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![o EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="52919-233">**Figura 1**</span><span class="sxs-lookup"><span data-stu-id="52919-233">**Figure 1**</span></span>

<span data-ttu-id="52919-234">Observação: se você quiser desenvolver o modelo EDM pela primeira vez, é possível limpar, gerar código POCO do EDM.</span><span class="sxs-lookup"><span data-stu-id="52919-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="52919-235">Você pode fazer isso com uma extensão do Visual Studio 2010 fornecida pela equipe de programabilidade de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="52919-236">Para baixar a extensão, inicie o Gerenciador de extensões no menu Ferramentas no Visual Studio e procurar a Galeria de modelos online "POCO" (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="52919-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="52919-237">Existem vários modelos POCO disponíveis para o EF.</span><span class="sxs-lookup"><span data-stu-id="52919-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="52919-238">Para obter mais informações sobre como usar o modelo, consulte " [instruções passo a passo: POCO modelo para o Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="52919-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![o EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="52919-240">**Figura 2**</span><span class="sxs-lookup"><span data-stu-id="52919-240">**Figure 2**</span></span>

<span data-ttu-id="52919-241">De que esse ponto de partida de POCO, exploraremos duas abordagens diferentes para código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="52919-242">A primeira abordagem eu chamo a abordagem EF porque aproveita abstrações da API do Entity Framework para implementar as unidades de trabalho e repositórios.</span><span class="sxs-lookup"><span data-stu-id="52919-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="52919-243">A segunda abordagem vamos criar nossas própria abstrações de repositório personalizado e, em seguida, veja as vantagens e desvantagens de cada abordagem.</span><span class="sxs-lookup"><span data-stu-id="52919-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="52919-244">Vamos começar Explorando a abordagem do EF.</span><span class="sxs-lookup"><span data-stu-id="52919-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="52919-245">Uma implementação centralizada do EF</span><span class="sxs-lookup"><span data-stu-id="52919-245">An EF Centric Implementation</span></span>

<span data-ttu-id="52919-246">Considere a seguinte ação do controlador de um projeto ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="52919-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="52919-247">A ação recupera um objeto de funcionário e retorna um resultado para exibir uma exibição detalhada do funcionário.</span><span class="sxs-lookup"><span data-stu-id="52919-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="52919-248">O código é testável?</span><span class="sxs-lookup"><span data-stu-id="52919-248">Is the code testable?</span></span> <span data-ttu-id="52919-249">Há pelo menos dois testes que precisamos para verificar o comportamento da ação.</span><span class="sxs-lookup"><span data-stu-id="52919-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="52919-250">Em primeiro lugar, gostaríamos de verificar se que a ação retorna a exibição correta – um teste simples.</span><span class="sxs-lookup"><span data-stu-id="52919-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="52919-251">Também queremos escrever um teste para verificar se a ação recupera o funcionário correto e gostaríamos de fazer isso sem executar o código para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="52919-252">Lembre-se de que queremos isolar o código em teste.</span><span class="sxs-lookup"><span data-stu-id="52919-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="52919-253">Isolamento garantirá que o teste não falha devido a um bug no código de acesso a dados ou banco de dados na configuração.</span><span class="sxs-lookup"><span data-stu-id="52919-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="52919-254">Se o teste falhar, podemos saberá que temos um bug na lógica do controlador e não em algum componente de sistema de nível inferior.</span><span class="sxs-lookup"><span data-stu-id="52919-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="52919-255">Para obter o isolamento, precisaremos algumas abstrações, como as interfaces são apresentadas anteriormente para repositórios e unidades de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="52919-256">Lembre-se de que o padrão de repositório foi projetado para mediar entre objetos de domínio e a camada de mapeamento de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="52919-257">Nesse cenário EF4 *está* os dados de mapeamento de camada e já fornece uma abstração de repositório como chamada IObjectSet&lt;T&gt; (do namespace Objects).</span><span class="sxs-lookup"><span data-stu-id="52919-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="52919-258">A definição da interface é semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="52919-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="52919-259">IObjectSet&lt;T&gt; atende aos requisitos de um repositório porque ele é semelhante a uma coleção de objetos (por meio de IEnumerable&lt;T&gt;) e fornece métodos para adicionar e remover objetos da coleção simulada.</span><span class="sxs-lookup"><span data-stu-id="52919-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="52919-260">Os métodos de anexar e desanexar exponham recursos adicionais da API do EF4.</span><span class="sxs-lookup"><span data-stu-id="52919-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="52919-261">Usar IObjectSet&lt;T&gt; como a interface para repositórios precisamos de uma unidade de abstração de trabalho para vincular repositórios juntos.</span><span class="sxs-lookup"><span data-stu-id="52919-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="52919-262">Uma implementação concreta desta interface se comunicará com o SQL Server e é fácil criar usando a classe ObjectContext do EF4.</span><span class="sxs-lookup"><span data-stu-id="52919-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="52919-263">A classe ObjectContext é a unidade real de trabalho na API do EF4.</span><span class="sxs-lookup"><span data-stu-id="52919-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="52919-264">Colocando um IObjectSet&lt;T&gt; vida é tão fácil quanto chamar o método CreateObjectSet do objeto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="52919-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="52919-265">Nos bastidores, o framework usará os metadados é fornecido no EDM para produzir um ObjectSet concreta&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="52919-266">Vamos acrescentar ao retornar o IObjectSet&lt;T&gt; porque ela ajuda a preservar a capacidade de teste no código do cliente de interface.</span><span class="sxs-lookup"><span data-stu-id="52919-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="52919-267">Essa implementação concreta é útil na produção, mas é necessário para se concentrar em como usaremos nosso abstração IUnitOfWork para facilitar os testes.</span><span class="sxs-lookup"><span data-stu-id="52919-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="52919-268">As duplicatas de teste</span><span class="sxs-lookup"><span data-stu-id="52919-268">The Test Doubles</span></span>

<span data-ttu-id="52919-269">Para isolar a ação do controlador, precisaremos a capacidade de alternar entre a unidade real de trabalho (apoiada por um ObjectContext) e uma unidade de double ou "falso" de teste de trabalho (executando operações na memória).</span><span class="sxs-lookup"><span data-stu-id="52919-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="52919-270">A abordagem comum para realizar esse tipo de troca é não permitir que o controlador MVC instanciar uma unidade de trabalho, mas em vez disso, passe a unidade de trabalho no controlador como um parâmetro de construtor.</span><span class="sxs-lookup"><span data-stu-id="52919-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="52919-271">O código acima é um exemplo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="52919-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="52919-272">Não permitimos que o controlador criar dependência-do-lo (a unidade de trabalho), mas injetar a dependência no controlador.</span><span class="sxs-lookup"><span data-stu-id="52919-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="52919-273">Em um projeto MVC é comum usar um alocador de controlador personalizado em combinação com um contêiner de inversão de controle (IoC) para automatizar a injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="52919-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="52919-274">Esses tópicos estão além do escopo deste artigo, mas você pode ler mais por seguindo as referências no final deste artigo.</span><span class="sxs-lookup"><span data-stu-id="52919-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="52919-275">Uma unidade falsa de implementação de trabalho que podemos usar para teste pode parecer semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="52919-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="52919-276">Observe que a unidade falsa de trabalho expõe uma propriedade de confirmada.</span><span class="sxs-lookup"><span data-stu-id="52919-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="52919-277">Às vezes é útil adicionar recursos a uma classe falsa que facilitam o teste.</span><span class="sxs-lookup"><span data-stu-id="52919-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="52919-278">Nesse caso, é fácil observar se o código confirma uma unidade de trabalho, verificando a propriedade confirmada.</span><span class="sxs-lookup"><span data-stu-id="52919-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="52919-279">Também precisaremos uma falsa IObjectSet&lt;T&gt; mantenha objetos por funcionário e o cartão de ponto na memória.</span><span class="sxs-lookup"><span data-stu-id="52919-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="52919-280">Podemos fornecer uma única implementação do uso de genéricos.</span><span class="sxs-lookup"><span data-stu-id="52919-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="52919-281">Essa duplicata de teste delega a maior parte de seu trabalho a um HashSet subjacente&lt;T&gt; objeto.</span><span class="sxs-lookup"><span data-stu-id="52919-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="52919-282">Observe que IObjectSet&lt;T&gt; requer uma restrição genérica impondo T como uma classe (um tipo de referência) e também força que implementam IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="52919-283">É fácil fazer com que uma coleção em memória são exibidos como um IQueryable&lt;T&gt; usando o operador LINQ padrão AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="52919-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="52919-284">Os testes</span><span class="sxs-lookup"><span data-stu-id="52919-284">The Tests</span></span>

<span data-ttu-id="52919-285">Testes de unidade tradicional usará uma única classe de teste para manter todos os testes para todas as ações em um único controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="52919-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="52919-286">Podemos escrever esses testes, ou qualquer tipo de teste de unidade, uso de memória de falsificações que criamos.</span><span class="sxs-lookup"><span data-stu-id="52919-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="52919-287">No entanto, para este artigo, que estamos evitará o monolítico abordagem de classe de teste e nossos testes para se concentrar em uma parte específica da funcionalidade de grupo em vez disso.</span><span class="sxs-lookup"><span data-stu-id="52919-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="52919-288">Por exemplo, "Criar novo funcionário" pode ser a funcionalidade que desejamos testar, portanto, usamos uma única classe de teste para verificar se a ação de controlador único responsável por criar um novo funcionário.</span><span class="sxs-lookup"><span data-stu-id="52919-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="52919-289">Há algum código de configuração comum que precisamos para todas essas classes de teste refinada.</span><span class="sxs-lookup"><span data-stu-id="52919-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="52919-290">Por exemplo, sempre precisamos criar nossos repositórios na memória e falsa unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="52919-291">Também precisamos uma instância do controlador de funcionário com a unidade falsa de trabalho injetado.</span><span class="sxs-lookup"><span data-stu-id="52919-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="52919-292">Compartilharemos este código de configuração comuns entre as classes de teste usando uma classe base.</span><span class="sxs-lookup"><span data-stu-id="52919-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="52919-293">A "mãe de objeto" usamos a classe base é um padrão comum para a criação de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="52919-294">Uma mãe de objeto contém métodos de fábrica para criar uma instância de entidades de teste para uso em vários acessórios de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="52919-295">Podemos usar o EmployeeControllerTestBase como a classe base para um número de Acessórios de teste (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="52919-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="52919-296">Cada acessório de teste irá testar uma ação de controlador específica.</span><span class="sxs-lookup"><span data-stu-id="52919-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="52919-297">Por exemplo, um acessório de teste irá se concentrar no teste de ação de criação usada durante uma solicitação HTTP GET (para exibir o modo de exibição para a criação de um funcionário), e um acessório diferente se concentra na ação de criação usada em uma solicitação HTTP POST (para levar as informações enviadas pela usuário para criar um funcionário).</span><span class="sxs-lookup"><span data-stu-id="52919-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="52919-298">Cada classe derivada só é responsável pela instalação necessária em seu contexto específico e fornecem as declarações necessárias para verificar os resultados de seu contexto de teste específico.</span><span class="sxs-lookup"><span data-stu-id="52919-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![o EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="52919-300">**Figura 3**</span><span class="sxs-lookup"><span data-stu-id="52919-300">**Figure 3**</span></span>

<span data-ttu-id="52919-301">O estilo de teste e a convenção de nomenclatura apresentado aqui não é necessário para o código testável – é apenas uma abordagem.</span><span class="sxs-lookup"><span data-stu-id="52919-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="52919-302">Figura 4 mostra os testes em execução no Jet cérebros Resharper plug-in do executor de teste para Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="52919-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![o EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="52919-304">**Figura 4**</span><span class="sxs-lookup"><span data-stu-id="52919-304">**Figure 4**</span></span>

<span data-ttu-id="52919-305">Com uma classe base para lidar com o código de configuração compartilhada, os testes de unidade para cada ação do controlador são pequeno e fácil de escrever.</span><span class="sxs-lookup"><span data-stu-id="52919-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="52919-306">Os testes serão executados rapidamente (já que estamos executando operações na memória) e não devem falhar devido a infraestrutura relacionada ou as questões ambientais (porque isola a unidade sob teste).</span><span class="sxs-lookup"><span data-stu-id="52919-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="52919-307">Nesses testes, a classe base faz a maior parte do trabalho de instalação.</span><span class="sxs-lookup"><span data-stu-id="52919-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="52919-308">Lembre-se de que o construtor de classe base cria o repositório na memória, uma unidade falsa de trabalho e uma instância da classe EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="52919-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="52919-309">A classe de teste deriva dessa classe base e enfoca as especificidades de testar o método Create.</span><span class="sxs-lookup"><span data-stu-id="52919-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="52919-310">Nesse caso, as especificações resumem as etapas "Organizar, agir e afirmar", que você verá em qualquer procedimento de teste de unidade:</span><span class="sxs-lookup"><span data-stu-id="52919-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="52919-311">Crie um objeto newEmployee para simular os dados de entrada.</span><span class="sxs-lookup"><span data-stu-id="52919-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="52919-312">Invocar a ação Create do EmployeeController e passe o newEmployee.</span><span class="sxs-lookup"><span data-stu-id="52919-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="52919-313">Verifique se que a ação de criação produz os resultados esperados (funcionário aparece no repositório).</span><span class="sxs-lookup"><span data-stu-id="52919-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="52919-314">O que criamos nos permite testar qualquer uma das ações EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="52919-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="52919-315">Por exemplo, ao escrever testes para a ação Index do controlador de funcionário de nós pode herdar da classe base de teste para estabelecer a mesma configuração de base para nossos testes.</span><span class="sxs-lookup"><span data-stu-id="52919-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="52919-316">Novamente a classe base criará o repositório na memória, a unidade falsa de trabalho e uma instância da EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="52919-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="52919-317">Os testes para a ação de índice só precisam se concentrar em invocar a ação de índice e teste as qualidades do modelo a ação retorna.</span><span class="sxs-lookup"><span data-stu-id="52919-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="52919-318">Os testes que estamos criando com o fakes na memória sejam orientados para testar o *estado* do software.</span><span class="sxs-lookup"><span data-stu-id="52919-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="52919-319">Por exemplo, ao testar a ação Create que queremos inspecionar o estado do repositório, depois executa a ação de criação – o repositório mantém o novo funcionário?</span><span class="sxs-lookup"><span data-stu-id="52919-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="52919-320">Mais tarde, examinaremos interação com base em testes.</span><span class="sxs-lookup"><span data-stu-id="52919-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="52919-321">Interação com base em testes perguntará se o código em teste invocado métodos adequados em nossos objetos e passado os parâmetros corretos.</span><span class="sxs-lookup"><span data-stu-id="52919-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="52919-322">Agora passaremos a tampa outro padrão de design – o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="52919-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="52919-323">O carregamento imediato e carregamento lento</span><span class="sxs-lookup"><span data-stu-id="52919-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="52919-324">Em algum momento na web do ASP.NET MVC aplicativo talvez que desejamos exibir informações de um funcionário e incluem o funcionário associado cartões de ponto.</span><span class="sxs-lookup"><span data-stu-id="52919-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="52919-325">Por exemplo, podemos ter uma exibição de resumo de cartão de ponto que mostra o nome do funcionário e o número total de cartões no sistema.</span><span class="sxs-lookup"><span data-stu-id="52919-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="52919-326">Há várias abordagens que podemos pode adotar para implementar esse recurso.</span><span class="sxs-lookup"><span data-stu-id="52919-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="52919-327">Projeção</span><span class="sxs-lookup"><span data-stu-id="52919-327">Projection</span></span>

<span data-ttu-id="52919-328">É uma abordagem mais fácil para criar o resumo construir um modelo dedicado para as informações que desejamos exibir no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="52919-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="52919-329">Nesse cenário, o modelo pode parecer semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="52919-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="52919-330">Observe que o EmployeeSummaryViewModel não é uma entidade – em outras palavras não é algo que desejamos manter no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="52919-331">Apenas vamos usar essa classe a movimentação de dados no modo de exibição de uma maneira fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="52919-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="52919-332">O modelo de exibição é como a transferência de dados de um DTO (objeto) porque ela contém nenhum comportamento (não há métodos) – apenas as propriedades.</span><span class="sxs-lookup"><span data-stu-id="52919-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="52919-333">As propriedades manterá os dados que necessários para mover.</span><span class="sxs-lookup"><span data-stu-id="52919-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="52919-334">É fácil criar uma instância de nesse modelo de exibição usando o operador de projeção padrão do LINQ – o operador Select.</span><span class="sxs-lookup"><span data-stu-id="52919-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="52919-335">Há dois recursos notáveis no código acima.</span><span class="sxs-lookup"><span data-stu-id="52919-335">There are two notable features to the above code.</span></span> <span data-ttu-id="52919-336">Primeiro – o código é fácil de testar porque ele ainda é fácil observar e isolar.</span><span class="sxs-lookup"><span data-stu-id="52919-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="52919-337">O operador Select funciona tão bem quanto em relação a nosso fakes na memória como faz em relação a unidade real de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="52919-338">O segundo recurso notável é como o código permite que o EF4 gerar uma consulta única e eficiente para reunir informações de funcionário e o cartão de ponto juntos.</span><span class="sxs-lookup"><span data-stu-id="52919-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="52919-339">Podemos carregou informações de funcionários e as informações de cartão de ponto no mesmo objeto sem usar quaisquer APIs especiais.</span><span class="sxs-lookup"><span data-stu-id="52919-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="52919-340">O código simplesmente expressa as informações que ele requer o uso de operadores LINQ padrão que funcionam em relação a fontes de dados na memória, bem como fontes de dados remotas.</span><span class="sxs-lookup"><span data-stu-id="52919-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="52919-341">EF4 foi capaz de converter as árvores de expressão geradas pela consulta LINQ e C\# compilador em uma consulta de T-SQL única e eficiente.</span><span class="sxs-lookup"><span data-stu-id="52919-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="52919-342">Há outras vezes quando não queremos trabalhar com um modelo de exibição ou o objeto DTO, mas com entidades reais.</span><span class="sxs-lookup"><span data-stu-id="52919-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="52919-343">Quando nós sabemos que precisamos de um funcionário *e* cartões do funcionário, é possível carregar rapidamente os dados relacionados de maneira eficiente e não invasiva.</span><span class="sxs-lookup"><span data-stu-id="52919-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="52919-344">Carregamento adiantado explícito</span><span class="sxs-lookup"><span data-stu-id="52919-344">Explicit Eager Loading</span></span>

<span data-ttu-id="52919-345">Quando desejamos carregar avidamente as informações de entidade relacionada, precisamos de algum mecanismo para lógica de negócios (ou nesse cenário, a lógica de ação do controlador) para expressar o seu desejo de repositório.</span><span class="sxs-lookup"><span data-stu-id="52919-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="52919-346">O ObjectQuery EF4&lt;T&gt; classe define um método Include para especificar os objetos relacionados a serem recuperados durante uma consulta.</span><span class="sxs-lookup"><span data-stu-id="52919-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="52919-347">Lembre-se de ObjectContext EF4 expõe entidades por meio do ObjectSet concreta&lt;T&gt; classe que herda de ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="52919-348">Se estivéssemos usando ObjectSet&lt;T&gt; referências em nossa ação de controlador, poderíamos escrever o código a seguir para especificar um carregamento adiantado das informações de cartão de ponto para cada funcionário.</span><span class="sxs-lookup"><span data-stu-id="52919-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="52919-349">No entanto, já que estamos tentando manter nosso código testável estamos não expondo ObjectSet&lt;T&gt; de fora da unidade real da classe de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="52919-350">Em vez disso, podemos contar com o IObjectSet&lt;T&gt; interface que é mais fácil forjar, mas IObjectSet&lt;T&gt; não define um método Include.</span><span class="sxs-lookup"><span data-stu-id="52919-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="52919-351">A beleza do LINQ é que podemos criar nosso próprios operador Include.</span><span class="sxs-lookup"><span data-stu-id="52919-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="52919-352">Observe que esse operador Include é definido como um método de extensão para IQueryable&lt;T&gt; em vez de IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="52919-353">Isso nos dá a capacidade de usar o método com uma grande variedade de tipos possíveis, incluindo IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;e ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="52919-354">No caso da sequência subjacente não é um ObjectQuery original do EF4&lt;T&gt;, não haverá nenhum dano e o operador Include é não operacional.</span><span class="sxs-lookup"><span data-stu-id="52919-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="52919-355">Se subjacente de sequência *está* um ObjectQuery&lt;T&gt; (ou derivado de ObjectQuery&lt;T&gt;), em seguida, EF4 será ver nosso requisito para obter dados adicionais e formular adequado do SQL consulta.</span><span class="sxs-lookup"><span data-stu-id="52919-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="52919-356">Com esse novo operador em vigor, pode solicitar explicitamente um carregamento adiantado das informações de cartão de ponto do repositório.</span><span class="sxs-lookup"><span data-stu-id="52919-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="52919-357">Quando executado em relação a um ObjectContext real, o código produz a seguinte consulta única.</span><span class="sxs-lookup"><span data-stu-id="52919-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="52919-358">A consulta reúne informações suficientes do banco de dados em uma única viagem para materializar os objetos de funcionários e preencher totalmente sua propriedade de cartões de ponto.</span><span class="sxs-lookup"><span data-stu-id="52919-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="52919-359">A boa notícia é que o código dentro do método de ação permanece totalmente testável.</span><span class="sxs-lookup"><span data-stu-id="52919-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="52919-360">Não precisamos fornecer todos os recursos adicionais para nosso falsificações suportar o operador Include.</span><span class="sxs-lookup"><span data-stu-id="52919-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="52919-361">A má notícia é que tivemos de usar o operador Include dentro o código que queremos manter os com ignorância de persistência.</span><span class="sxs-lookup"><span data-stu-id="52919-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="52919-362">Isso é um exemplo perfeito do tipo de vantagens e desvantagens, que você precisará avaliar ao compilar código testável.</span><span class="sxs-lookup"><span data-stu-id="52919-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="52919-363">Há ocasiões em que você precisa para permitir que o vazamento de preocupações de persistência fora a abstração de repositório para atender às metas de desempenho.</span><span class="sxs-lookup"><span data-stu-id="52919-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="52919-364">A alternativa para o carregamento adiantado é carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="52919-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="52919-365">Meios de carregamento lento fazemos *não* precisa nosso código de negócios para anunciar explicitamente o requisito para os dados associados.</span><span class="sxs-lookup"><span data-stu-id="52919-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="52919-366">Em vez disso, podemos usar nosso entidades no aplicativo e se os dados adicionais são necessárias do Entity Framework carrega os dados sob demanda.</span><span class="sxs-lookup"><span data-stu-id="52919-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="52919-367">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="52919-367">Lazy Loading</span></span>

<span data-ttu-id="52919-368">É fácil imaginar um cenário em que não sabemos o que precisará dados uma parte da lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="52919-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="52919-369">Talvez sabemos que a lógica precisa de um objeto de funcionário, mas podemos pode ramificar em diferentes caminhos de execução em que alguns desses caminhos exigem informações de cartão de ponto do funcionário e outros não.</span><span class="sxs-lookup"><span data-stu-id="52919-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="52919-370">Cenários como esse são perfeitos para o carregamento lento implícito porque dados magicamente aparecem em uma base conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="52919-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="52919-371">Carregamento lento, também conhecido como adiado carregando, colocar alguns requisitos em nossos objetos de entidade.</span><span class="sxs-lookup"><span data-stu-id="52919-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="52919-372">POCOs com ignorância de persistência true não enfrenta todos os requisitos da camada de persistência, mas ignorância de persistência true é praticamente impossível alcançar.</span><span class="sxs-lookup"><span data-stu-id="52919-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="52919-373">Em vez disso, medimos a ignorância de persistência em graus relativos.</span><span class="sxs-lookup"><span data-stu-id="52919-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="52919-374">Seria uma pena se for necessário herdar de uma classe base da persistência orientada ou usar uma coleção especializada para alcançar o carregamento lento em POCOs.</span><span class="sxs-lookup"><span data-stu-id="52919-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="52919-375">Felizmente, o EF4 tem uma solução menos intrusiva.</span><span class="sxs-lookup"><span data-stu-id="52919-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="52919-376">Praticamente não puder ser detectada</span><span class="sxs-lookup"><span data-stu-id="52919-376">Virtually Undetectable</span></span>

<span data-ttu-id="52919-377">Ao usar objetos POCO, EF4 pode gerar dinamicamente os proxies de tempo de execução para entidades.</span><span class="sxs-lookup"><span data-stu-id="52919-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="52919-378">Esses proxies invisivelmente encapsulam os POCOs materializados e fornecem serviços adicionais por meio da interceptação cada propriedade obtenham e operação para executar o trabalho adicional de definição.</span><span class="sxs-lookup"><span data-stu-id="52919-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="52919-379">Um desses serviços é o recurso de carregamento lento que estamos procurando.</span><span class="sxs-lookup"><span data-stu-id="52919-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="52919-380">Outro serviço é um mecanismo que pode gravar quando o programa altera os valores de propriedade de uma entidade de controle de alterações eficiente.</span><span class="sxs-lookup"><span data-stu-id="52919-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="52919-381">A lista de alterações é usada pelo ObjectContext durante o método SaveChanges para persistir as entidades modificadas usando comandos de atualização.</span><span class="sxs-lookup"><span data-stu-id="52919-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="52919-382">Para esses proxies funcionar, no entanto, eles precisam de uma maneira de se conectar ao get de propriedade e operações de conjunto em uma entidade e os proxies atingir esse objetivo, substituindo os membros virtuais.</span><span class="sxs-lookup"><span data-stu-id="52919-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="52919-383">Dessa forma, se quisermos ter o carregamento lento implícito e o controle de alterações eficiente é necessário voltar para nossa definições de classe POCO e marcar propriedades como virtuais.</span><span class="sxs-lookup"><span data-stu-id="52919-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="52919-384">Podemos ainda dizer que a entidade Employee é principalmente persistência com ignorância.</span><span class="sxs-lookup"><span data-stu-id="52919-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="52919-385">O único requisito é usar os membros virtuais, e isso não afeta a capacidade de teste do código.</span><span class="sxs-lookup"><span data-stu-id="52919-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="52919-386">Não precisamos derivar de uma classe base especial ou até mesmo usar uma coleção especial dedicada a carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="52919-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="52919-387">Como o código demonstra, qualquer classe que implementa ICollection&lt;T&gt; está disponível para armazenar entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="52919-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="52919-388">Também há uma pequena alteração que precisamos fazer dentro de nossa unidade de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="52919-389">É o carregamento lento *desativar* por padrão ao trabalhar diretamente com um objeto de ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="52919-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="52919-390">Há uma propriedade que podemos definir na propriedade ContextOptions para habilitar o carregamento adiado, e podemos definir essa propriedade dentro de nossa unidade real de trabalho se quisermos que habilite o carregamento lento em todos os lugares.</span><span class="sxs-lookup"><span data-stu-id="52919-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="52919-391">Com o carregamento lento implícito habilitado, o código do aplicativo pode usar um funcionário e o funcionário associados cartões permanecendo não reconhece o trabalho necessário para o EF carregar os dados extras.</span><span class="sxs-lookup"><span data-stu-id="52919-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="52919-392">Carregamento lento facilita escrever o código do aplicativo e com a mágica de proxy, o código permanece completamente testável.</span><span class="sxs-lookup"><span data-stu-id="52919-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="52919-393">Fakes na memória da unidade de trabalho podem simplesmente pré-carregar entidades falsas com dados associados quando necessário durante um teste.</span><span class="sxs-lookup"><span data-stu-id="52919-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="52919-394">Neste ponto, voltaremos nossa atenção desde a criação de repositórios usando IObjectSet&lt;T&gt; e examine abstrações para ocultar todos os sinais a estrutura de persistência.</span><span class="sxs-lookup"><span data-stu-id="52919-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="52919-395">Repositórios personalizados</span><span class="sxs-lookup"><span data-stu-id="52919-395">Custom Repositories</span></span>

<span data-ttu-id="52919-396">Quando, apresentamos o padrão de unidade de trabalho design primeiro neste artigo, que fornecemos um exemplo de código para a unidade de trabalho possível aparência.</span><span class="sxs-lookup"><span data-stu-id="52919-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="52919-397">Vamos apresentar essa ideia original usando o funcionário e o cenário de cartão de ponto de funcionário, com que temos trabalhado novamente.</span><span class="sxs-lookup"><span data-stu-id="52919-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="52919-398">A principal diferença entre essa unidade de trabalho e a unidade de trabalho que criamos na última seção é como a unidade de trabalho não usa abstrações do EF4 framework (não há nenhum IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="52919-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="52919-399">IObjectSet&lt;T&gt; funciona bem como uma interface de repositório, mas a API que ele expõe não pode alinhar perfeitamente com as necessidades do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52919-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="52919-400">Essa abordagem futura representaremos usando um IRepository personalizado de repositórios&lt;T&gt; abstração.</span><span class="sxs-lookup"><span data-stu-id="52919-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="52919-401">Muitos desenvolvedores que seguem o design controlado por teste, design controlado por comportamento e design de metodologias controlado por domínio preferem o IRepository&lt;T&gt; abordagem por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="52919-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="52919-402">Primeiro, o IRepository&lt;T&gt; interface representa uma camada "anticorrupção".</span><span class="sxs-lookup"><span data-stu-id="52919-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="52919-403">Conforme descrito por Eric Evans em seu livro de Design controlado por domínio uma camada anticorrupção mantém o código de domínio para fora da infraestrutura de APIs, como uma API de persistência.</span><span class="sxs-lookup"><span data-stu-id="52919-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="52919-404">Em segundo lugar, os desenvolvedores podem criar métodos para o repositório que atendem às necessidades exatas de um aplicativo (como descobertos durante a gravação de testes).</span><span class="sxs-lookup"><span data-stu-id="52919-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="52919-405">Por exemplo, podemos com frequência talvez seja necessário localizar uma única entidade usando um valor de ID, portanto, podemos adicionar um método FindById para a interface do repositório.</span><span class="sxs-lookup"><span data-stu-id="52919-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="52919-406">Nosso IRepository&lt;T&gt; definição será semelhante ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="52919-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="52919-407">Observe a removeremos voltar a usar um IQueryable&lt;T&gt; interface para expor as coleções de entidades.</span><span class="sxs-lookup"><span data-stu-id="52919-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="52919-408">IQueryable&lt;T&gt; permite árvores de expressão do LINQ para fluir para o provedor do EF4 e dê o provedor de uma visão holística da consulta.</span><span class="sxs-lookup"><span data-stu-id="52919-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="52919-409">Uma segunda opção seria retornar IEnumerable&lt;T&gt;, que significa que o provedor LINQ do EF4 só verão as expressões criadas dentro do repositório.</span><span class="sxs-lookup"><span data-stu-id="52919-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="52919-410">Qualquer agrupamento, ordenação e projeção feitas fora do repositório não serão compostos para o comando SQL enviado ao banco de dados, o que pode prejudicar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="52919-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="52919-411">Por outro lado, um repositório retornando somente IEnumerable&lt;T&gt; resultados serão nunca surpreendê-lo com um novo comando SQL.</span><span class="sxs-lookup"><span data-stu-id="52919-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="52919-412">Ambas as abordagens funcionará, e ambas as abordagens permanecem testáveis.</span><span class="sxs-lookup"><span data-stu-id="52919-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="52919-413">É simples fornecer uma única implementação do IRepository&lt;T&gt; interface usando a API do ObjectContext EF4 e genéricos.</span><span class="sxs-lookup"><span data-stu-id="52919-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="52919-414">O IRepository&lt;T&gt; abordagem nos dá algum controle adicional sobre nossas consultas como um cliente tem que invocar um método para obter a uma entidade.</span><span class="sxs-lookup"><span data-stu-id="52919-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="52919-415">Dentro do método poderia fornecemos operadores LINQ para impor restrições de aplicativo e verificações adicionais.</span><span class="sxs-lookup"><span data-stu-id="52919-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="52919-416">Observe que a interface possui duas restrições no parâmetro de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="52919-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="52919-417">A primeira restrição é que os contras de classe prejudicar exigido pelo ObjectSet&lt;T&gt;, e a segunda restrição força nossas entidades para implementar IEntity – uma abstração criada para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="52919-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="52919-418">A interface IEntity força entidades tenham uma propriedade de Id legível e pode, em seguida, usamos essa propriedade no método FindById.</span><span class="sxs-lookup"><span data-stu-id="52919-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="52919-419">IEntity é definido com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="52919-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="52919-420">IEntity poderia ser considerado uma violação de ignorância de persistência de pequeno, pois nossas entidades são necessários para implementar essa interface.</span><span class="sxs-lookup"><span data-stu-id="52919-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="52919-421">Lembre-se de ignorância de persistência é sobre as vantagens e desvantagens, e para muitos, a funcionalidade de FindById superam a restrição imposta pela interface.</span><span class="sxs-lookup"><span data-stu-id="52919-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="52919-422">A interface não tem impacto na capacidade de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="52919-423">Criando uma instância de um ativo IRepository&lt;T&gt; requer um ObjectContext EF4, portanto, uma unidade concreta de implementação de trabalho deve gerenciar a instanciação.</span><span class="sxs-lookup"><span data-stu-id="52919-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="52919-424">Usando o repositório personalizado</span><span class="sxs-lookup"><span data-stu-id="52919-424">Using the Custom Repository</span></span>

<span data-ttu-id="52919-425">Usar nosso repositório personalizado não é significativamente diferente de usar o repositório com base em IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="52919-426">Em vez de aplicar operadores LINQ diretamente a uma propriedade, primeiro precisamos invocar uma métodos do repositório para pegar um IQueryable&lt;T&gt; referência.</span><span class="sxs-lookup"><span data-stu-id="52919-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="52919-427">Observe que o operador Include personalizado que anteriormente implementamos irá funcionar sem alterações.</span><span class="sxs-lookup"><span data-stu-id="52919-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="52919-428">Método de FindById do repositório remove ações ao tentar recuperar uma única entidade lógica duplicada.</span><span class="sxs-lookup"><span data-stu-id="52919-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="52919-429">Não há nenhuma diferença significativa das duas abordagens, examinamos a capacidade de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="52919-430">Poderíamos fornecer implementações fictícias de IRepository&lt;T&gt; criando classes concretas apoiadas por HashSet&lt;funcionário&gt; , exatamente como o que fizemos na última seção.</span><span class="sxs-lookup"><span data-stu-id="52919-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="52919-431">No entanto, alguns desenvolvedores preferem usar objetos fictícios e simular as estruturas de objeto em vez de criar o fakes.</span><span class="sxs-lookup"><span data-stu-id="52919-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="52919-432">Vamos examinar usar objetos fictícios para nossa implementação de teste e discutir as diferenças entre as simulações e fakes na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="52919-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="52919-433">Testes com objetos fictícios</span><span class="sxs-lookup"><span data-stu-id="52919-433">Testing with Mocks</span></span>

<span data-ttu-id="52919-434">Há diferentes abordagens para criar o que o chama de Martin Fowler uma "duplicata de teste".</span><span class="sxs-lookup"><span data-stu-id="52919-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="52919-435">Um teste duplo (como um filme dublê) é um objeto que compilar para "coloque em funcionamento em" para o real, os objetos de produção durante os testes.</span><span class="sxs-lookup"><span data-stu-id="52919-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="52919-436">Os repositórios de na memória que criamos são duplicatas de teste para os repositórios que se comunicam com o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="52919-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="52919-437">Já vimos como usar essas duplicatas de teste durante os testes de unidade para isolar o código e manter testes em execução rápida.</span><span class="sxs-lookup"><span data-stu-id="52919-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="52919-438">As duplicatas de teste que criamos têm implementações reais, trabalhar.</span><span class="sxs-lookup"><span data-stu-id="52919-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="52919-439">Em segundo plano cada um armazena uma coleção concreta de objetos e eles serão adicionar e remover objetos dessa coleção, como podemos manipular o repositório durante um teste.</span><span class="sxs-lookup"><span data-stu-id="52919-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="52919-440">Alguns desenvolvedores como criar suas duplicatas de teste dessa maneira – com o código real e implementações de trabalho.</span><span class="sxs-lookup"><span data-stu-id="52919-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="52919-441">Essas duplicatas de teste são o que chamamos *fakes*.</span><span class="sxs-lookup"><span data-stu-id="52919-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="52919-442">Eles têm implementações de trabalho, mas eles não são reais para uso em produção.</span><span class="sxs-lookup"><span data-stu-id="52919-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="52919-443">O repository falso, na verdade, não será gravado no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="52919-444">O servidor SMTP falso, na verdade, não envia uma mensagem de email pela rede.</span><span class="sxs-lookup"><span data-stu-id="52919-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="52919-445">Simulações versus Fakes</span><span class="sxs-lookup"><span data-stu-id="52919-445">Mocks versus Fakes</span></span>

<span data-ttu-id="52919-446">Há outro tipo de teste duas vezes conhecido como um *simular*.</span><span class="sxs-lookup"><span data-stu-id="52919-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="52919-447">Enquanto o fakes têm implementações de trabalho, as simulações entram com nenhuma implementação.</span><span class="sxs-lookup"><span data-stu-id="52919-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="52919-448">Com a Ajuda de uma estrutura de objeto, podemos construir esses objetos fictícios em tempo de execução e usá-los como duplicatas de teste.</span><span class="sxs-lookup"><span data-stu-id="52919-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="52919-449">Nesta seção, que usaremos objetos fictícios Moq de software livre.</span><span class="sxs-lookup"><span data-stu-id="52919-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="52919-450">Aqui está um exemplo simples de como usar o Moq para criar dinamicamente um teste duplo para um repositório de funcionário.</span><span class="sxs-lookup"><span data-stu-id="52919-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="52919-451">Solicitamos Moq IRepository&lt;funcionário&gt; implementação e ele criará um dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="52919-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="52919-452">Obter o objeto que implementa o IRepository&lt;funcionário&gt; acessando a propriedade do objeto da imitação&lt;T&gt; objeto.</span><span class="sxs-lookup"><span data-stu-id="52919-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="52919-453">É esse objeto interno que podemos passar para os controladores, e eles não saberão se esta for uma duplicata de teste ou o repositório real.</span><span class="sxs-lookup"><span data-stu-id="52919-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="52919-454">Podemos invocar métodos no objeto assim como podemos seria invocar métodos em um objeto com uma implementação real.</span><span class="sxs-lookup"><span data-stu-id="52919-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="52919-455">Você deve estar se perguntando o que o repositório fictício fará quando invocamos o método Add.</span><span class="sxs-lookup"><span data-stu-id="52919-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="52919-456">Como não há nenhuma implementação por trás o objeto fictício, adicionar não fará nada.</span><span class="sxs-lookup"><span data-stu-id="52919-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="52919-457">Não há nenhuma coleção concreta por trás dos bastidores tivemos com os fakes que escrevemos, portanto, o funcionário é descartado.</span><span class="sxs-lookup"><span data-stu-id="52919-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="52919-458">O que o valor retornado de FindById?</span><span class="sxs-lookup"><span data-stu-id="52919-458">What about the return value of FindById?</span></span> <span data-ttu-id="52919-459">Nesse caso, o objeto fictício faz a única coisa que ele pode fazer, que é retornado um valor padrão.</span><span class="sxs-lookup"><span data-stu-id="52919-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="52919-460">Uma vez que estamos retornando um tipo de referência (um funcionário), o valor de retorno é um valor nulo.</span><span class="sxs-lookup"><span data-stu-id="52919-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="52919-461">As simulações podem parecer inútil; No entanto, há dois recursos mais as simulações que não falamos sobre.</span><span class="sxs-lookup"><span data-stu-id="52919-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="52919-462">Primeiro, o framework Moq registra todas as chamadas feitas no objeto fictício.</span><span class="sxs-lookup"><span data-stu-id="52919-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="52919-463">Posteriormente no código podemos fazer Moq se alguém chamado o método Add, ou se qualquer pessoa que o método FindById foi invocado.</span><span class="sxs-lookup"><span data-stu-id="52919-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="52919-464">Veremos mais tarde como podemos usar esse recurso de gravação de "caixa preta" em testes.</span><span class="sxs-lookup"><span data-stu-id="52919-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="52919-465">O segundo recurso excelente é como podemos usar Moq programar com um objeto de simulação *expectativas*.</span><span class="sxs-lookup"><span data-stu-id="52919-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="52919-466">Uma expectativa informa o objeto fictício como responder a qualquer interação determinada.</span><span class="sxs-lookup"><span data-stu-id="52919-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="52919-467">Por exemplo, podemos programar uma expectativa em nossa simulação e instruí-lo para retornar um objeto de funcionário quando alguém chama FindById.</span><span class="sxs-lookup"><span data-stu-id="52919-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="52919-468">A estrutura de Moq usa uma API de instalação e as expressões lambda programar essas expectativas.</span><span class="sxs-lookup"><span data-stu-id="52919-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="52919-469">Neste exemplo, podemos pedir Moq para criar dinamicamente um repositório e, em seguida, nós Programamos o repositório com uma expectativa.</span><span class="sxs-lookup"><span data-stu-id="52919-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="52919-470">A expectativa informa o objeto fictício para retornar um novo objeto de funcionário com um valor de Id de 5, quando alguém chama o método FindById passando um valor de 5.</span><span class="sxs-lookup"><span data-stu-id="52919-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="52919-471">Esse teste é aprovado e não precisamos criar uma implementação completa de IRepository falsa&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="52919-472">Vamos rever os testes que escrevemos antes e retrabalho-los para usar objetos fictícios em vez de fakes.</span><span class="sxs-lookup"><span data-stu-id="52919-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="52919-473">Assim como antes, vamos usar uma classe base para configurar as partes comuns de infraestrutura que precisamos para todos os testes do controlador.</span><span class="sxs-lookup"><span data-stu-id="52919-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="52919-474">O código de configuração principalmente permanece o mesmo.</span><span class="sxs-lookup"><span data-stu-id="52919-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="52919-475">Em vez de usar falsificações, usaremos o Moq para construir objetos fictícios.</span><span class="sxs-lookup"><span data-stu-id="52919-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="52919-476">Organiza a classe base para a unidade de simulação de trabalho retornar um repositório fictício quando o código chama a propriedade de funcionários.</span><span class="sxs-lookup"><span data-stu-id="52919-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="52919-477">O restante da instalação fictício ocorrerá dentro os acessórios de teste dedicado para cada cenário específico.</span><span class="sxs-lookup"><span data-stu-id="52919-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="52919-478">Por exemplo, o acessório de teste para a ação de índice irá configurar o repositório fictício para retornar uma lista de funcionários, quando a ação chama o método FindAll do repositório fictício.</span><span class="sxs-lookup"><span data-stu-id="52919-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="52919-479">Exceto para as expectativas, nossos testes semelhante para os testes que tínhamos antes.</span><span class="sxs-lookup"><span data-stu-id="52919-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="52919-480">No entanto, com a capacidade de gravação de uma estrutura fictícia pode aproximar a testes de um ângulo diferente.</span><span class="sxs-lookup"><span data-stu-id="52919-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="52919-481">Vamos examinar essa nova perspectiva na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="52919-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="52919-482">Estado versus testes de interação</span><span class="sxs-lookup"><span data-stu-id="52919-482">State versus Interaction Testing</span></span>

<span data-ttu-id="52919-483">Há diferentes técnicas que você pode usar para testar o software com objetos fictícios.</span><span class="sxs-lookup"><span data-stu-id="52919-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="52919-484">Uma abordagem é usar o estado com base em testes, que é o que fizemos neste documento até o momento.</span><span class="sxs-lookup"><span data-stu-id="52919-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="52919-485">Estado com base em declarações de teste faz sobre o estado do software.</span><span class="sxs-lookup"><span data-stu-id="52919-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="52919-486">No último teste podemos invocado um método de ação no controlador e fez uma asserção sobre o modelo, que ele deverá ser compilado.</span><span class="sxs-lookup"><span data-stu-id="52919-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="52919-487">Aqui estão alguns exemplos de estado de teste:</span><span class="sxs-lookup"><span data-stu-id="52919-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="52919-488">Verifique se que o repositório contém o novo objeto de funcionário após a execução de criação.</span><span class="sxs-lookup"><span data-stu-id="52919-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="52919-489">Verifique se que o modelo contém uma lista de todos os funcionários após a execução de índice.</span><span class="sxs-lookup"><span data-stu-id="52919-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="52919-490">Verifique se que o repositório não contém um determinado funcionário após a exclusão é executada.</span><span class="sxs-lookup"><span data-stu-id="52919-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="52919-491">Outra abordagem, você verá com objetos fictícios é verificar *interações*.</span><span class="sxs-lookup"><span data-stu-id="52919-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="52919-492">Enquanto o estado com base em declarações de teste faz sobre o estado de objetos, interação com base em declarações de teste faz sobre como os objetos interagem.</span><span class="sxs-lookup"><span data-stu-id="52919-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="52919-493">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="52919-493">For example:</span></span>

-   <span data-ttu-id="52919-494">Verifique se que o controlador invoca método do repositório quando executa Create.</span><span class="sxs-lookup"><span data-stu-id="52919-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="52919-495">Verifique se que o controlador invoca o método de FindAll do repositório quando o índice é executada.</span><span class="sxs-lookup"><span data-stu-id="52919-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="52919-496">Verifique se que o controlador invoca a unidade do método de confirmação do trabalho para salvar as alterações quando editar é executado.</span><span class="sxs-lookup"><span data-stu-id="52919-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="52919-497">Geralmente interação teste requer menos dados de teste, porque nós não são investigação dentro de coleções e verificando contagens.</span><span class="sxs-lookup"><span data-stu-id="52919-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="52919-498">Por exemplo, se nós sabemos que a ação detalhes invoca o método FindById de um repositório com o valor correto -, em seguida, a ação está provavelmente se comportando corretamente.</span><span class="sxs-lookup"><span data-stu-id="52919-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="52919-499">Podemos verificar esse comportamento sem configurar quaisquer dados de teste para retornar do FindById.</span><span class="sxs-lookup"><span data-stu-id="52919-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="52919-500">A instalação somente necessária no acessório de teste acima é a configuração fornecida pela classe base.</span><span class="sxs-lookup"><span data-stu-id="52919-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="52919-501">Quando invocamos a ação do controlador, Moq registrará as interações com o repositório fictício.</span><span class="sxs-lookup"><span data-stu-id="52919-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="52919-502">Usando a API de verificar de Moq, podemos fazer Moq se o controlador invocado FindById com o valor da ID adequado.</span><span class="sxs-lookup"><span data-stu-id="52919-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="52919-503">Se o controlador não invocou o método ou invocou o método com um valor de parâmetro inesperado, verifique se o método lançará uma exceção e o teste falhará.</span><span class="sxs-lookup"><span data-stu-id="52919-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="52919-504">Aqui está outro exemplo para verificar que a ação Create invoca confirmação na unidade de trabalho atual.</span><span class="sxs-lookup"><span data-stu-id="52919-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="52919-505">Um dos riscos com testes de interação é a tendência em especificar interações.</span><span class="sxs-lookup"><span data-stu-id="52919-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="52919-506">A capacidade do objeto fictício para registrar e verificar cada interação com o objeto fictício não significa que o teste deve tentar verificar cada interação.</span><span class="sxs-lookup"><span data-stu-id="52919-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="52919-507">Algumas interações são detalhes de implementação e você só deve verificar as interações *necessária* para satisfazer o teste atual.</span><span class="sxs-lookup"><span data-stu-id="52919-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="52919-508">A escolha entre objetos fictícios ou fakes amplamente depende do sistema que você está testando e sua conta pessoal (ou equipe) preferências.</span><span class="sxs-lookup"><span data-stu-id="52919-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="52919-509">Objetos fictícios podem reduzir drasticamente a quantidade de código que você precisa implementar duplicatas de teste, mas nem todo mundo se sente confortável de expectativas de programação e verificando as interações.</span><span class="sxs-lookup"><span data-stu-id="52919-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="52919-510">Conclusões</span><span class="sxs-lookup"><span data-stu-id="52919-510">Conclusions</span></span>

<span data-ttu-id="52919-511">Neste artigo, demonstrei várias abordagens para criar o código que podem ser testado usando o ADO.NET Entity Framework para persistência de dados.</span><span class="sxs-lookup"><span data-stu-id="52919-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="52919-512">Podemos aproveitar abstrações internas como IObjectSet&lt;T&gt;, ou criar nossas próprios abstrações, como IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="52919-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="52919-513">Em ambos os casos, o suporte a POCO no ADO.NET Entity Framework 4.0 permite que os consumidores dessas abstrações permaneça persistente com ignorância e altamente testável.</span><span class="sxs-lookup"><span data-stu-id="52919-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="52919-514">Recursos adicionais do EF4, como carregamento lento implícito permite que o serviço de negócios e aplicativo de código para trabalhar sem se preocupar com os detalhes de um repositório de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="52919-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="52919-515">Por fim, as abstrações que podemos criar são fáceis de serem simule ou falsifique dentro de testes de unidade, e podemos usar essas duplicatas de teste para atingir a execução rápida, altamente isolada e os testes confiáveis.</span><span class="sxs-lookup"><span data-stu-id="52919-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="52919-516">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="52919-516">Additional Resources</span></span>

-   <span data-ttu-id="52919-517">Robert, " [o princípio da responsabilidade única](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="52919-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="52919-518">Martin Fowler [catálogo de padrões](http://www.martinfowler.com/eaaCatalog/index.html) de *padrões de arquitetura de aplicativo empresarial*</span><span class="sxs-lookup"><span data-stu-id="52919-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="52919-519">Caprio Griffin, " [injeção de dependência](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="52919-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="52919-520">Blog de programação de dados, " [instruções passo a passo: desenvolvimento com o Entity Framework 4.0 orientado por testes](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="52919-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="52919-521">Blog de programação de dados, " [padrões usando o repositório e unidade de trabalho com o Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="52919-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="52919-522">Dave Astels, " [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="52919-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="52919-523">Aaron Jensen, " [apresentando as especificações de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="52919-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="52919-524">Eric Lee, " [BDD com MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="52919-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="52919-525">Eric Evans, " [Design controlado por domínio](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="52919-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="52919-526">Martin Fowler, " [Mocks não são Stubs](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="52919-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="52919-527">Martin Fowler, " [duplo de teste](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="52919-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="52919-528">Jeremy Miller, " [estado versus testes de interação](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="52919-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="52919-529">Moq</span><span class="sxs-lookup"><span data-stu-id="52919-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="52919-530">Biografia</span><span class="sxs-lookup"><span data-stu-id="52919-530">Biography</span></span>

<span data-ttu-id="52919-531">Scott Allen é um membro da equipe técnica na Pluralsight e fundador da OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="52919-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="52919-532">Em 15 anos de desenvolvimento de software comercial, Scott trabalhou em soluções para tudo, desde dispositivos incorporados de 8 bits para aplicativos web ASP.NET com altamente escalonáveis.</span><span class="sxs-lookup"><span data-stu-id="52919-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="52919-533">Você pode alcançar Scott em seu blog em OdeToCode ou no Twitter em [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="52919-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
