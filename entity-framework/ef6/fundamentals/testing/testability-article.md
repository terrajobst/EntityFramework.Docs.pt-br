---
title: Capacidade de teste e Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181583"
---
# <a name="testability-and-entity-framework-40"></a>Capacidade de teste e Entity Framework 4,0
Scott Allen

Checked Maio de 2010

## <a name="introduction"></a>Introdução

Este white paper descreve e demonstra como escrever código de teste com o ADO.NET Entity Framework 4,0 e o Visual Studio 2010. Este documento não tenta se concentrar em uma metodologia de teste específica, como o TDD (design controlado por testes) ou o BDD (design controlado por comportamento). Em vez disso, este artigo se concentrará em como escrever código que usa o ADO.NET Entity Framework, ainda assim, é fácil isolar e testar de maneira automatizada. Veremos os padrões de design comuns que facilitam o teste em cenários de acesso a dados e Confira como aplicar esses padrões ao usar a estrutura. Também examinaremos os recursos específicos da estrutura para ver como esses recursos podem funcionar no código de teste.

## <a name="what-is-testable-code"></a>O que é o código de teste?

A capacidade de verificar uma parte do software usando testes de unidade automatizados oferece muitos benefícios desejáveis. Todos sabem que bons testes reduzirão o número de defeitos de software em um aplicativo e aumentarão a qualidade do aplicativo, mas ter testes de unidade vai muito além da localização de bugs.

Um bom conjunto de testes de unidade permite que uma equipe de desenvolvimento Economize tempo e permaneça no controle do software que eles criam. Uma equipe pode fazer alterações no código existente, refatorar, recriar e reestruturar o software para atender aos novos requisitos e adicionar novos componentes a um aplicativo, tudo sabendo que o conjunto de testes pode verificar o comportamento do aplicativo. Os testes de unidade fazem parte de um rápido ciclo de comentários para facilitar a alteração e preservar a facilidade de manutenção do software à medida que a complexidade aumenta.

No entanto, o teste de unidade vem com um preço. Uma equipe deve investir tempo para criar e manter testes de unidade. A quantidade de esforço necessária para criar esses testes está diretamente relacionada à **capacidade de teste** do software subjacente. Quão fácil é o software para testar? Uma equipe que projeta software com capacidade de teste em mente criará testes efetivos mais rápido do que a equipe que trabalha com software não-retestado.

A Microsoft projetou a ADO.NET Entity Framework 4,0 (EF4) com a capacidade de teste em mente. Isso não significa que os desenvolvedores estejam escrevendo testes de unidade no próprio código da estrutura. Em vez disso, as metas de capacidade de teste para o EF4 facilitam a criação de código que se baseia na estrutura. Antes de examinarmos os exemplos específicos, vale a pena entender as qualidades do código de teste.

### <a name="the-qualities-of-testable-code"></a>As qualidades do código de teste

O código que é fácil de testar sempre exibirá pelo menos duas características. Primeiro, o código de teste é fácil de **observar**. Dado algum conjunto de entradas, deve ser fácil observar a saída do código. Por exemplo, o teste do método a seguir é fácil porque o método retorna diretamente o resultado de um cálculo.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

O teste de um método é difícil se o método gravar o valor calculado em um soquete de rede, uma tabela de banco de dados ou um arquivo como o código a seguir. O teste precisa executar trabalho adicional para recuperar o valor.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Em segundo lugar, o código de teste é fácil de **isolar**. Vamos usar o seguinte pseudocódigo como um exemplo inadequado de código de teste.

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

O método é fácil de observar – podemos passar uma apólice de seguro e verificar se o valor retornado corresponde a um resultado esperado. No entanto, para testar o método, precisaremos ter um banco de dados instalado com o esquema correto e configurar o servidor SMTP caso o método tente enviar um email.

O teste de unidade só deseja verificar a lógica de cálculo dentro do método, mas o teste pode falhar porque o servidor de email está offline ou porque o servidor de banco de dados foi movido. Essas duas falhas não estão relacionadas ao comportamento que o teste deseja verificar. O comportamento é difícil de isolar.

Os desenvolvedores de software que se esforçam para escrever código de teste geralmente buscam manter uma separação de preocupações no código que escrevem. O método acima deve se concentrar nos cálculos de negócios e delegar o banco de dados e os detalhes de implementação de email para outros componentes. Robert C. Martin chama esse princípio de responsabilidade única. Um objeto deve encapsular uma única responsabilidade estreita, como calcular o valor de uma política. Todos os outros trabalhos de banco de dados e de notificação devem ser de responsabilidade de algum outro objeto. O código escrito dessa maneira é mais fácil de isolar, pois concentra-se em uma única tarefa.

No .NET, temos as abstrações que precisamos para seguir o princípio de responsabilidade única e obter isolamento. Podemos usar definições de interface e forçar o código a usar a abstração de interface em vez de um tipo concreto. Mais adiante neste documento, veremos como um método como o exemplo inadequado apresentado acima pode funcionar com interfaces *parecidas* com o banco de dados. No momento do teste, no entanto, podemos substituir uma implementação fictícia que não se comunica com o banco de dados, mas em vez disso, ele mantém o dado na memória. Essa implementação fictícia isolará o código de problemas não relacionados no código de acesso a dados ou na configuração do banco de dado.

Há benefícios adicionais para isolamento. O cálculo de negócios no último método deve levar apenas alguns milissegundos para ser executado, mas o próprio teste pode ser executado por vários segundos, já que o código é saltos em toda a rede e se comunica com vários servidores. Os testes de unidade devem ser executados rapidamente para facilitar pequenas alterações. Os testes de unidade também devem ser repetíveis e não podem falhar porque um componente não relacionado ao teste tem um problema. Escrever código que seja fácil de observar e isolar significa que os desenvolvedores terão um tempo mais fácil de escrever testes para o código, gastar menos tempo aguardando a execução de testes e, o mais importante, gastar menos tempo controlando os bugs que não existem.

Espero que você aprecie os benefícios do teste e entenda as qualidades que o código que pode ser testado é exibido. Estamos prestes a abordar como escrever código que funciona com o EF4 para salvar dados em um banco de dado, enquanto restam observáveis e fáceis de isolar, mas primeiro vamos restringir nosso foco para discutir os designs de teste para acesso a dados.

## <a name="design-patterns-for-data-persistence"></a>Padrões de design para persistência de dados

Os dois exemplos inadequados apresentados anteriormente tinham muitas responsabilidades. O primeiro exemplo inadequado tinha de executar um cálculo *e* gravar em um arquivo. O segundo exemplo inadequado tinha de ler dados de um banco de dado *e* executar um cálculo comercial *e* enviar email. Ao criar métodos menores que separam preocupações e delegam responsabilidade a outros componentes, você fará grandes progressos em direção à escrita de código de teste. O objetivo é criar funcionalidades ao compor ações de abstrações pequenas e focadas.

Quando se trata de persistência de dados, as abstrações pequenas e focadas que estamos procurando são tão comuns que foram documentadas como padrões de design. Os padrões de livro de Martin Fowler da arquitetura de aplicativos empresariais foram o primeiro trabalho a descrever esses padrões na impressão. Forneceremos uma breve descrição desses padrões nas seções a seguir antes de mostrarmos como essas ADO.NET Entity Framework implementam e funcionam com esses padrões.

### <a name="the-repository-pattern"></a>O Padrão de Repositório

O Fowler diz um repositório "medias between from the data Mapping Layers, usando uma interface do tipo coleção para acessar objetos de domínio". O objetivo do padrão de repositório é isolar o código do minúcias de acesso a dados e, como vimos anteriormente, o isolamento é uma característica necessária para a capacidade de teste.

A chave para o isolamento é como o repositório expõe objetos usando uma interface como coleção. A lógica que você escreve para usar o repositório não tem idéia de como o repositório irá materializar os objetos que você solicitar. O repositório pode se comunicar com um banco de dados ou pode simplesmente retornar objetos de uma coleção na memória. Tudo o que seu código precisa saber é que o repositório parece manter a coleção e você pode recuperar, adicionar e excluir objetos da coleção.

Em aplicativos .NET existentes, um repositório concreto geralmente herda de uma interface genérica como a seguinte:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Faremos algumas alterações na definição da interface quando fornecermos uma implementação para EF4, mas o conceito básico permanece o mesmo. O código pode usar um repositório concreto implementando essa interface para recuperar uma entidade por seu valor de chave primária, para recuperar uma coleção de entidades com base na avaliação de um predicado ou simplesmente recuperar todas as entidades disponíveis. O código também pode adicionar e remover entidades por meio da interface do repositório.

Dado um IRepository de objetos Employee, o código pode executar as seguintes operações.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Como o código está usando uma interface (IRepository de Employee), podemos fornecer o código com diferentes implementações da interface. Uma implementação pode ser uma implementação apoiada por EF4 e persistir objetos em um banco de dados Microsoft SQL Server. Uma implementação diferente (uma que usamos durante o teste) pode ser apoiada por uma lista na memória de objetos Employee. A interface ajudará a atingir o isolamento no código.

Observe que a interface IRepository @ no__t-0T @ no__t-1 não expõe uma operação de salvamento. Como atualizar os objetos existentes? Você pode se deparar com definições de IRepository que incluem a operação salvar, e as implementações desses repositórios precisarão manter imediatamente um objeto no banco de dados. No entanto, em muitos aplicativos, não queremos manter objetos individualmente. Em vez disso, queremos dar vida aos objetos, talvez de diferentes repositórios, modificar esses objetos como parte de uma atividade de negócios e, em seguida, manter todos os objetos como parte de uma única operação atômica. Felizmente, há um padrão para permitir esse tipo de comportamento.

### <a name="the-unit-of-work-pattern"></a>O padrão de unidade de trabalho

Fowler diz que uma unidade de trabalho "manterá uma lista de objetos afetados por uma transação de negócios e coordenará a gravação de alterações e a resolução de problemas de simultaneidade". É responsabilidade da unidade de trabalho controlar as alterações nos objetos que levamos à vida de um repositório e persistem todas as alterações feitas nos objetos quando dizemos a unidade de trabalho para confirmar as alterações. Também é responsabilidade da unidade de trabalho pegar os novos objetos que adicionamos a todos os repositórios e inserir os objetos em um banco de dados, além de gerenciar a exclusão.

Se você já tiver feito qualquer trabalho com conjuntos de ADO.NETs de trabalho, ainda estará familiarizado com o padrão de unidade de serviço. Os conjuntos de dados ADO.NET tinham a capacidade de controlar nossas atualizações, exclusões e inserção de objetos DataRow e poderiam (com a ajuda de um TableAdapter) reconciliar todas as alterações em um banco de dados. No entanto, os objetos DataSet modelam um subconjunto desconectado do banco de dados subjacente. O padrão de unidade de trabalho exibe o mesmo comportamento, mas trabalha com objetos comerciais e objetos de domínio que são isolados do código de acesso a dados e que não sabem do banco.

Uma abstração para modelar a unidade de trabalho no código .NET pode ser semelhante ao seguinte:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Ao expor referências de repositório da unidade de trabalho, podemos garantir que uma única unidade de objeto de trabalho tenha a capacidade de acompanhar todas as entidades materializadas durante uma transação de negócios. A implementação do método Commit para uma unidade real de trabalho é onde toda a mágica acontece para reconciliar as alterações na memória com o banco de dados. 

Dada uma referência de IUnitOfWork, o código pode fazer alterações nos objetos comerciais recuperados de um ou mais repositórios e salvar todas as alterações usando a operação de confirmação atômica.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>O padrão de carga lenta

Fowler usa a carga lenta de nome para descrever "um objeto que não contém todos os dados necessários, mas sabe como obtê-lo". O carregamento lento transparente é um recurso importante a ser criado ao escrever código comercial de teste e trabalhar com um banco de dados relacional. Como exemplo, considere o código a seguir.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Como a coleção de cartões de expostos é populada? Há duas respostas possíveis. Uma resposta é que o repositório de funcionários, quando solicitado a buscar um funcionário, emite uma consulta para recuperar o funcionário junto com as informações do cartão de horário associado do funcionário. Em bancos de dados relacionais, isso geralmente requer uma consulta com uma cláusula JOIN e pode resultar na recuperação de mais informações do que um aplicativo precisa. E se o aplicativo nunca precisar tocar na propriedade de cartões de esquers?

Uma segunda resposta é carregar a propriedade de cartões de esquers "sob demanda". Esse carregamento lento é implícito e transparente para a lógica de negócios porque o código não invoca APIs especiais para recuperar informações de cartão de tempo. O código pressupõe que as informações do cartão de tempo estejam presentes quando necessário. Há alguma mágica envolvida com o carregamento lento que geralmente envolve a interceptação de tempo de execução de invocações de método. O código de interceptação é responsável por conversar com o banco de dados e recuperar informações de cartão de tempo, deixando a lógica de negócios livre para ser lógica de negócios. Essa mágica de carga lenta permite que o código comercial se isole de operações de recuperação de dados e resulta em um código mais contestável.

A desvantagem de uma carga lenta é que, quando um *aplicativo precisar* das informações do cartão de tempo, o código executará uma consulta adicional. Isso não é uma preocupação para muitos aplicativos, mas para aplicativos sensíveis ao desempenho ou aplicativos que executam um loop por meio de um número de objetos Employee e a execução de uma consulta para recuperar cartões de tempo durante cada iteração do loop (um problema geralmente conhecido como N + 1 problema de consulta), o carregamento lento é um arrastar. Nesses cenários, um aplicativo pode querer carregar cuidadosamente as informações de cartão de tempo da maneira mais eficiente possível.

Felizmente, veremos como o EF4 dá suporte a cargas lentas implícitas e cargas de rápido eficiência à medida que passamos para a próxima seção e implementamos esses padrões.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementando padrões com o Entity Framework

A boa notícia é que todos os padrões de design descritos na última seção são simples de implementar com EF4. Para demonstrar, vamos usar um aplicativo MVC simples do ASP.NET para editar e exibir funcionários e suas informações de cartão de ponto associadas. Começaremos usando os seguintes "objetos antigos do CLR" (POCOs). 

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

Essas definições de classe serão ligeiramente alteradas à medida que explorarmos diferentes abordagens e recursos do EF4, mas a intenção é manter essas classes como a Persistence que desconhecem (PI) o mais possível. Um objeto PI não sabe *como*, ou mesmo *se*, o estado que ele mantém reside dentro de um banco de dados. PI e POCOs se encontram em mãos com software de teste. Os objetos que usam uma abordagem POCO são menos restritos, mais flexíveis e mais fáceis de testar, pois eles podem operar sem um banco de dados presente.

Com o POCOs em vigor, podemos criar um Modelo de Dados de Entidade (EDM) no Visual Studio (consulte a Figura 1). Não usaremos o EDM para gerar código para nossas entidades. Em vez disso, queremos usar as entidades que carinhosamentemos manualmente. Usaremos apenas o EDM para gerar nosso esquema de banco de dados e fornecer os metadados que o EF4 precisa para mapear objetos no banco de dados.

![test_01 EF](~/ef6/media/eftest-01.jpg)

**Figura 1**

Observação: se você quiser desenvolver o modelo do EDM primeiro, será possível gerar um código limpo e POCO do EDM. Você pode fazer isso com uma extensão do Visual Studio 2010 fornecida pela equipe de programação de dados. Para baixar a extensão, inicie o Gerenciador de extensões no menu ferramentas no Visual Studio e pesquise a galeria online de modelos para "POCO" (consulte a Figura 2). Há vários modelos de POCO disponíveis para o EF. Para obter mais informações sobre como usar o modelo, consulte "[Walkthrough: Modelo POCO para o Entity Framework @ no__t-0 ".

![test_02 EF](~/ef6/media/eftest-02.png)

**Figura 2**

Deste ponto de partida do POCO, exploraremos duas abordagens diferentes para o código que pode ser testado. A primeira abordagem que chamo a abordagem do EF porque ela aproveita abstrações da API Entity Framework para implementar unidades de trabalho e repositórios. Na segunda abordagem, criaremos nossas próprias abstrações de repositório personalizadas e, em seguida, veremos as vantagens e desvantagens de cada abordagem. Vamos começar explorando a abordagem do EF.  

### <a name="an-ef-centric-implementation"></a>Uma implementação centrada no EF

Considere a seguinte ação do controlador de um projeto MVC do ASP.NET. A ação recupera um objeto Employee e retorna um resultado para exibir uma exibição detalhada do funcionário.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

O código está sendo testado? Há pelo menos dois testes que seriam necessários para verificar o comportamento da ação. Primeiro, gostaríamos de verificar se a ação retorna o modo de exibição correto – um teste fácil. Também queremos escrever um teste para verificar se a ação recupera o funcionário correto e gostaríamos de fazer isso sem executar o código para consultar o banco de dados. Lembre-se de que desejamos isolar o código em teste. O isolamento garantirá que o teste não falhe devido a um bug no código de acesso a dados ou na configuração do banco de dado. Se o teste falhar, sabemos que temos um bug na lógica do controlador e não em um componente de sistema de nível inferior.

Para obter o isolamento, precisaremos de algumas abstrações como as interfaces que apresentamos anteriormente para repositórios e unidades de trabalho. Lembre-se de que o padrão de repositório foi projetado para mediar entre objetos de domínio e a camada de mapeamento de dados. Neste cenário, EF4 *é* a camada de mapeamento de dados e já fornece uma abstração semelhante ao repositório denominada IObjectSet @ No__t-1T @ no__t-2 (do namespace System. Data. Objects). A definição da interface é parecida com a seguinte.

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

IObjectSet @ no__t-0T @ no__t-1 atende aos requisitos de um repositório porque ele se assemelha a uma coleção de objetos (via IEnumerable @ no__t-2T @ no__t-3) e fornece métodos para adicionar e remover objetos da coleção simulada. Os métodos Attach e Detach expõem recursos adicionais da API EF4. Para usar IObjectSet @ no__t-0T @ no__t-1 como a interface para repositórios, precisamos de uma unidade de abstração de trabalho para associar repositórios.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Uma implementação concreta dessa interface se comunicará com SQL Server e será fácil de criar usando a classe ObjectContext de EF4. A classe ObjectContext é a unidade real de trabalho na API EF4.

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

Trazer um IObjectSet @ no__t-0T @ no__t-1 à vida é tão fácil quanto invocar o método CreateObjectSet do objeto ObjectContext. Em segundo plano, a estrutura usará os metadados que fornecemos no EDM para produzir um ObjectSet concreto @ no__t-0T @ no__t-1. Vamos continuar com o retorno da interface IObjectSet @ no__t-0T @ no__t-1, pois isso ajudará a preservar a capacidade de teste no código do cliente.

Essa implementação concreta é útil na produção, mas precisamos nos concentrar em como usaremos nossa abstração IUnitOfWork para facilitar os testes.

### <a name="the-test-doubles"></a>As duplicatas de teste

Para isolar a ação do controlador, precisaremos da capacidade de alternar entre a unidade real de trabalho (apoiada por um ObjectContext) e um duplo de teste ou uma unidade de trabalho "falsa" (executando operações na memória). A abordagem comum para executar esse tipo de alternância é não permitir que o controlador MVC instancie uma unidade de trabalho, mas, em vez disso, passe a unidade de trabalho para o controlador como um parâmetro de construtor.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

O código acima é um exemplo de injeção de dependência. Não permitimos que o controlador criasse sua dependência (a unidade de trabalho), mas injeta a dependência no controlador. Em um projeto MVC, é comum usar uma fábrica de controlador personalizado em combinação com um contêiner inversão de controle (IoC) para automatizar a injeção de dependência. Esses tópicos estão além do escopo deste artigo, mas você pode ler mais seguindo as referências no final deste artigo.

Uma implementação de unidade de trabalho falsa que podemos usar para teste pode ser semelhante ao seguinte.

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

Observe que a unidade de trabalho falsa expõe uma propriedade confirmada. Às vezes, é útil adicionar recursos a uma classe falsa que facilita os testes. Nesse caso, é fácil observar se o código confirma uma unidade de trabalho verificando a propriedade confirmada.

Também precisaremos de uma falsificação IObjectSet @ no__t-0T @ no__t-1 para manter os objetos Employee e Timecard na memória. Podemos fornecer uma única implementação usando genéricos.

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

Esse teste delega a maior parte de seu trabalho a um objeto HashSet @ no__t-0T @ no__t-1 subjacente. Observe que IObjectSet @ no__t-0T @ no__t-1 requer uma restrição genérica que impõe T como uma classe (um tipo de referência) e também nos obriga a implementar IQueryable @ no__t-2T @ no__t-3. É fácil fazer com que uma coleção na memória apareça como um IQueryable @ no__t-0T @ no__t-1 usando o operador LINQ padrão de consulta.

### <a name="the-tests"></a>Os testes

Os testes de unidade tradicionais usarão uma única classe de teste para manter todos os testes de todas as ações em um único controlador MVC. Podemos escrever esses testes ou qualquer tipo de teste de unidade, usando as falsificações de memória que criamos. No entanto, para este artigo, evitaremos a abordagem de classe de teste monolítica e, em vez disso, agruparemos nossos testes para se concentrar em uma parte específica da funcionalidade.  Por exemplo, "criar novo funcionário" pode ser a funcionalidade que queremos testar, portanto, usaremos uma única classe de teste para verificar a ação do controlador único responsável por criar um novo funcionário.

Há um código de instalação comum que precisamos para todas essas classes de teste refinadas. Por exemplo, sempre precisamos criar nossos repositórios na memória e uma unidade de trabalho falsa. Também precisamos de uma instância do controlador de funcionário com a unidade de trabalho falsa injetada. Vamos compartilhar esse código de instalação comum em classes de teste usando uma classe base.

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

O "objeto mãe" que usamos na classe base é um padrão comum para a criação de dados de teste. Uma mãe do objeto contém métodos de fábrica para instanciar entidades de teste para uso em vários acessórios de teste.

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

Podemos usar o EmployeeControllerTestBase como a classe base para vários acessórios de teste (veja a Figura 3). Cada acessório de teste testará uma ação de controlador específica. Por exemplo, um acessório de teste se concentrará no teste da ação de criação usada durante uma solicitação HTTP GET (para exibir a exibição para criar um funcionário), e um acessório diferente se concentrará na ação de criação usada em uma solicitação HTTP POST (para obter informações enviadas pelo usuário para criar um funcionário). Cada classe derivada é responsável apenas pela configuração necessária em seu contexto específico e para fornecer as asserções necessárias para verificar os resultados de seu contexto de teste específico.

![test_03 EF](~/ef6/media/eftest-03.png)

**Figura 3**

A Convenção de nomenclatura e o estilo de teste apresentados aqui não são necessários para o código de testes – é apenas uma abordagem. A Figura 4 mostra os testes em execução no plug-in do executor de teste do Jet cérebro para o Visual Studio 2010.

![test_04 EF](~/ef6/media/eftest-04.png)

**Figura 4**

Com uma classe base para lidar com o código de instalação compartilhado, os testes de unidade para cada ação do controlador são pequenos e fáceis de escrever. Os testes serão executados rapidamente (já que estamos executando operações na memória) e não devem falhar devido a questões ambientais ou de infraestrutura não relacionadas (porque isolamos a unidade em teste).

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

Nesses testes, a classe base faz a maior parte do trabalho de configuração. Lembre-se de que o construtor da classe base cria o repositório na memória, uma unidade de trabalho falsa e uma instância da classe EmployeeController. A classe de teste deriva dessa classe base e se concentra nas especificidades de teste do método Create. Nesse caso, as especificações resumem as etapas de "organizar, agir e declarar" que você verá em qualquer procedimento de teste de unidade:

-   Crie um objeto newEmployee para simular os dados de entrada.
-   Invoque a ação de criação do EmployeeController e passe o newEmployee.
-   Verifique se a ação criar produz os resultados esperados (o funcionário aparece no repositório).

O que nós criamos nos permite testar qualquer uma das ações EmployeeController. Por exemplo, quando escrevemos testes para a ação de índice do controlador de funcionário, podemos herdar da classe base de teste para estabelecer a mesma configuração base para nossos testes. Novamente, a classe base criará o repositório na memória, a unidade de trabalho falsa e uma instância do EmployeeController. Os testes para a ação de índice só precisam se concentrar na invocação da ação de índice e no teste das qualidades do modelo que a ação retorna.

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

Os testes que estamos criando com falsificações na memória são orientados em relação ao teste do *estado* do software. Por exemplo, ao testar a ação criar, desejamos inspecionar o estado do repositório após a execução da ação de criação – o repositório mantém o novo funcionário?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Posteriormente, veremos o teste baseado em interação. O teste baseado em interação perguntará se o código sob teste chamou os métodos adequados em nossos objetos e passou os parâmetros corretos. Por enquanto, vamos mudar para a capa de outro padrão de design – a carga lenta.

## <a name="eager-loading-and-lazy-loading"></a>Carregamento adiantado e carregamento lento

Em algum ponto do aplicativo Web ASP.NET MVC, talvez você queira exibir as informações de um funcionário e incluir os cartões de tempo associados do funcionário. Por exemplo, pode haver uma exibição de resumo do cartão de tempo que mostra o nome do funcionário e o número total de cartões de tempo no sistema. Há várias abordagens que podemos adotar para implementar esse recurso.

### <a name="projection"></a>Projeção

Uma abordagem fácil para criar o resumo é construir um modelo dedicado às informações que desejamos exibir na exibição. Nesse cenário, o modelo pode ser semelhante ao seguinte.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Observe que o EmployeeSummaryViewModel não é uma entidade – em outras palavras, não é algo que desejamos persistir no banco de dados. Vamos usar essa classe para embaralhar dados na exibição de maneira fortemente tipada. O modelo de exibição é como um DTO (objeto de transferência de dados) porque ele não contém nenhum comportamento (nenhum método) – apenas Propriedades. As propriedades conterão os dados que precisamos mover. É fácil instanciar esse modelo de exibição usando o operador de projeção padrão do LINQ – o operador SELECT.

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

Há dois recursos notáveis para o código acima. Primeiro – o código é fácil de testar, pois ainda é fácil observar e isolar. O operador Select também funciona bem em nossas falsificações na memória como faz com a unidade real de trabalho.

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

O segundo recurso notável é como o código permite que o EF4 gere uma consulta única e eficiente para montar as informações de funcionário e de cartão de tempo juntas. Nós carregamos informações de funcionários e informações de cartão de tempo no mesmo objeto sem usar nenhuma API especial. O código simplesmente expressou as informações necessárias usando operadores LINQ padrão que funcionam em fontes de dados na memória, bem como em fontes de dados remotas. EF4 conseguiu converter as árvores de expressão geradas pela consulta LINQ e o compilador C @ no__t-0 em uma consulta T-SQL única e eficiente.

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

Há outras ocasiões em que não queremos trabalhar com um modelo de exibição ou objeto DTO, mas com entidades reais. Quando sabemos que precisamos de um funcionário *e* dos cartões de tempo do funcionário, podemos carregar os dados relacionados de maneira discreta e eficiente.

### <a name="explicit-eager-loading"></a>Carregamento adiantado explícito

Quando desejarmos carregar cuidadosamente as informações de entidade relacionadas, precisamos de um mecanismo de lógica comercial (ou, neste cenário, lógica de ação do controlador) para expressar seu desejo para o repositório. A classe EF4 ObjectQuery @ no__t-0T @ no__t-1 define um método include para especificar os objetos relacionados a serem recuperados durante uma consulta. Lembre-se de que o ObjectContext EF4 expõe entidades por meio da classe concreta ObjectSet @ no__t-0T @ no__t-1 que herda de ObjectQuery @ no__t-2T @ no__t-3.  Se estivesse usando ObjectSet @ no__t-0T @ no__t-1 referências em nossa ação de controlador, poderíamos escrever o código a seguir para especificar um carregamento adiantado de informações de cartão de tempo para cada funcionário.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

No entanto, como estamos tentando manter nosso código testável, não estamos expondo o ObjectSet @ no__t-0T @ no__t-1 de fora da classe real da aula de trabalho. Em vez disso, nós confiamos na interface IObjectSet @ no__t-0T @ no__t-1, que é mais fácil de ser falsificada, mas IObjectSet @ no__t-2T @ no__t-3 não define um método include. A beleza do LINQ é que podemos criar nosso próprio operador include.

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

Observe que esse operador include é definido como um método de extensão para IQueryable @ no__t-0T @ no__t-1 em vez de IObjectSet @ no__t-2T @ no__t-3. Isso nos dá a capacidade de usar o método com uma variedade maior de tipos possíveis, incluindo IQueryable @ no__t-0T @ no__t-1, IObjectSet @ no__t-2T @ no__t-3, ObjectQuery @ no__t-4T suporta @ no__t-5 e ObjectSet @ no__t-6T @ no__t-7. No caso de a sequência subjacente não ser uma EF4 original noquery @ no__t-0T @ no__t-1, não há nenhum dano feito e o operador include é um não operacional. Se a sequência subjacente *for* uma ObjectQuery @ No__t-1T @ no__t-2 (ou derivada de ObjectQuery @ No__t-3T @ no__t-4), o EF4 verá nosso requisito para dados adicionais e formulará a consulta SQL apropriada.

Com esse novo operador em vigor, podemos solicitar explicitamente uma carga adiantada de informações de cartão de tempo do repositório.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Quando executado em um ObjectContext real, o código produz a seguinte consulta única. A consulta coleta informações suficientes do banco de dados em uma viagem para materializar os objetos Employee e preencher totalmente sua propriedade Timecards.

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

A grande notícia é que o código dentro do método de ação permanece totalmente testado. Não precisamos fornecer nenhum recurso adicional para nossas falsificações para dar suporte ao operador include. A má notícia é que tivemos que usar o operador include dentro do código que queríamos manter a persistência que desconhecem. Este é um exemplo primo do tipo de compensações que você precisará avaliar ao criar código de teste. Há ocasiões em que você precisa deixar a persistência se preocupar com o vazamento fora da abstração do repositório para atender às metas de desempenho.

A alternativa ao carregamento adiantado é o carregamento lento. O carregamento lento significa que *não* precisamos do nosso código comercial para anunciar explicitamente o requisito de dados associados. Em vez disso, usamos nossas entidades no aplicativo e, se forem necessários dados adicionais Entity Framework carregará os dados sob demanda.

### <a name="lazy-loading"></a>Carregamento lento

É fácil imaginar um cenário em que não sabemos quais dados uma parte da lógica comercial precisará. Podemos saber que a lógica precisa de um objeto Employee, mas podemos ramificar em caminhos de execução diferentes em que alguns desses caminhos exigem informações de cartão de tempo do funcionário e outros não. Cenários como esse são perfeitos para carregamento lento implícito porque os dados aparecem de forma mágica conforme necessário.

O carregamento lento, também conhecido como carregamento adiado, coloca alguns requisitos em nossos objetos de entidade. POCOs com a verdadeira persistência ignorância não enfrentaria nenhum requisito da camada de persistência, mas a verdadeira persistência ignorância é praticamente impossível de alcançar.  Em vez disso, medimos ignorância de persistência em graus relativos. Seria uma pena se fosse necessário herdar de uma classe base orientada à persistência ou usar uma coleção especializada para obter o carregamento lento em POCOs. Felizmente, o EF4 tem uma solução menos invasiva.

### <a name="virtually-undetectable"></a>Praticamente não detectável

Ao usar objetos POCO, o EF4 pode gerar dinamicamente proxies de tempo de execução para entidades. Esses proxies envolvem invisivelmente o POCOs materializado e fornecem serviços adicionais interceptando cada propriedade Get e Set para executar trabalho adicional. Um desses serviços é o recurso de carregamento lento que estamos procurando. Outro serviço é um mecanismo de controle de alterações eficiente que pode registrar quando o programa altera os valores de propriedade de uma entidade. A lista de alterações é usada pelo ObjectContext durante o método SaveChanges para manter todas as entidades modificadas usando comandos UPDATE.

No entanto, para que esses proxies funcionem, eles precisam de uma maneira de se conectar às operações Get e set de propriedade em uma entidade, e os proxies atingem essa meta por meio da substituição de membros virtuais. Portanto, se quisermos que o carregamento lento implícito e o controle de alterações eficiente, precisamos voltar para nossas definições de classe POCO e marcar propriedades como virtuais.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Ainda podemos dizer que a entidade Employee é, na maioria das vezes, que desconhecem persistence. O único requisito é usar membros virtuais e isso não afeta a capacidade de teste do código. Não precisamos derivar de nenhuma classe base especial ou até mesmo usar uma coleção especial dedicada ao carregamento lento. Como demonstra o código, qualquer classe que implemente o ICollection @ no__t-0T @ no__t-1 está disponível para armazenar entidades relacionadas.

Também há uma pequena alteração que precisamos fazer dentro de nossa unidade de trabalho. O carregamento lento é *desativado* por padrão ao trabalhar diretamente com um objeto ObjectContext. Há uma propriedade que podemos definir na propriedade ContextOptions para habilitar o carregamento adiado e podemos definir essa propriedade dentro de nossa unidade real de trabalho se quisermos habilitar o carregamento lento em todos os lugares.

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

Com o carregamento lento implícito habilitado, o código do aplicativo pode usar um funcionário e os cartões de tempo associados do funcionário enquanto o tranqüilamente restante não conhece o trabalho necessário para o EF carregar os dados adicionais.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

O carregamento lento torna o código do aplicativo mais fácil de escrever e, com a mágica do proxy, o código permanece completamente testado. As falsificações na memória da unidade de trabalho podem simplesmente pré-carregar entidades falsas com dados associados quando necessário durante um teste.

Neste ponto, vamos transformar nossa atenção na criação de repositórios usando IObjectSet @ no__t-0T @ no__t-1 e examinar abstrações para ocultar todos os sinais da estrutura de persistência.

## <a name="custom-repositories"></a>Repositórios personalizados

Quando apresentamos inicialmente o padrão de design de unidade de trabalho neste artigo, fornecemos um código de exemplo para o que seria a unidade de trabalho. Vamos apresentar essa ideia original usando o cenário de cartão de horário de funcionário e funcionário com o qual trabalhamos.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

A principal diferença entre essa unidade de trabalho e a unidade de trabalho que criamos na última seção é como essa unidade de trabalho não usa nenhuma abstração da estrutura EF4 (não há IObjectSet @ no__t-0T @ no__t-1). IObjectSet @ no__t-0T @ no__t-1 funciona bem como uma interface de repositório, mas a API que ele expõe pode não estar perfeitamente alinhada com as necessidades do nosso aplicativo. Nesta próxima abordagem, representaremos repositórios usando uma abstração personalizada IRepository @ no__t-0T @ no__t-1.

Muitos desenvolvedores que seguem o design controlado por testes, design controlado por comportamento e metodologias orientadas a domínio preferem a abordagem IRepository @ no__t-0T @ no__t-1 por vários motivos. Primeiro, a interface IRepository @ no__t-0T @ no__t-1 representa uma camada de "anticorrupção". Conforme descrito por Eric Evans em seu livro de design controlado por domínio, uma camada anticorrupção mantém seu código de domínio longe das APIs de infraestrutura, como uma API de persistência. Em segundo lugar, os desenvolvedores podem criar métodos no repositório que atendam às necessidades exatas de um aplicativo (como descoberto durante a gravação de testes). Por exemplo, em geral, podemos precisar localizar uma única entidade usando um valor de ID, para que possamos adicionar um método FindById à interface do repositório.  Nossa definição IRepository @ no__t-0T @ no__t-1 será parecida com a seguinte.

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

Observe que vamos voltar ao uso de uma interface IQueryable @ no__t-0T @ no__t-1 para expor coleções de entidades. IQueryable @ no__t-0T @ no__t-1 permite que as árvores de expressões LINQ fluam para o provedor EF4 e fornecem ao provedor uma visão holística da consulta. Uma segunda opção seria retornar IEnumerable @ no__t-0T @ no__t-1, o que significa que o provedor do EF4 LINQ verá apenas as expressões criadas dentro do repositório. Qualquer agrupamento, ordenação e projeção feitos fora do repositório não será composto no comando SQL enviado ao banco de dados, o que pode prejudicar o desempenho. Por outro lado, um repositório que retorna apenas IEnumerable @ no__t-0T @ no__t-1 results nunca irá surpreender você com um novo comando SQL. Ambas as abordagens funcionarão e ambas as abordagens permanecerão em teste.

É simples fornecer uma única implementação da interface IRepository @ no__t-0T @ no__t-1 usando genéricos e a API ObjectContext EF4.

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

A abordagem IRepository @ no__t-0T @ no__t-1 fornece um controle adicional sobre nossas consultas, pois um cliente precisa invocar um método para chegar a uma entidade. Dentro do método, poderíamos fornecer verificações adicionais e operadores LINQ para impor restrições de aplicativo. Observe que a interface tem duas restrições no parâmetro de tipo genérico. A primeira restrição é a classe contras contras de contras de no__t-0T @ no__t-1, e a segunda restrição força nossas entidades a implementarem IEntity – uma abstração criada para o aplicativo. A interface IEntity força as entidades a terem uma propriedade de ID legível e, em seguida, podemos usar essa propriedade no método FindById. IEntity é definido com o código a seguir.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity poderia ser considerado uma pequena violação do ignorância de persistência, já que nossas entidades são necessárias para implementar essa interface. Lembre-se de que a persistência ignorância é sobre as compensações e, para muitos, a funcionalidade do FindById superará a restrição imposta pela interface. A interface não tem impacto na capacidade de teste.

A instanciação de um IRepository @ no__t-0T @ no__t-1 ao vivo requer um ObjectContext EF4, portanto, uma implementação de unidade concreta de trabalho deve gerenciar a instanciação.

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

### <a name="using-the-custom-repository"></a>Usando o repositório personalizado

Usar nosso repositório personalizado não é significativamente diferente de usar o repositório com base em IObjectSet @ no__t-0T @ no__t-1. Em vez de aplicar operadores LINQ diretamente a uma propriedade, primeiro precisaremos chamar um dos métodos do repositório para obter uma referência IQueryable @ no__t-0T @ no__t-1.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Observe que o operador de inclusão personalizado que implementamos anteriormente funcionará sem alteração. O método FindById do repositório remove a lógica duplicada das ações que tentam recuperar uma única entidade.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Não há nenhuma diferença significativa na capacidade de teste das duas abordagens que examinamos. Poderíamos fornecer implementações falsas de IRepository @ no__t-0T @ no__t-1 Criando classes concretas apoiadas por HashSet @ no__t-2Employee @ no__t-3-exatamente como fizemos na última seção. No entanto, alguns desenvolvedores preferem usar objetos fictícios e estruturas de objeto fictícios em vez de criar falsificações. Veremos como usar as simulações para testar nossa implementação e discutir as diferenças entre as simulações e as falsificações na próxima seção.

### <a name="testing-with-mocks"></a>Teste com simulações

Há diferentes abordagens para criar o que Martin Fowler chama de "duplicata de teste". Um duplo de teste (como um filme stunt duplo) é um objeto que você cria para "entrar" para objetos reais, de produção durante os testes. Os repositórios na memória que criamos são duplicatas de teste para os repositórios que falam com SQL Server. Vimos como usar essas duplicatas de teste durante os testes de unidade para isolar o código e manter os testes em execução rápida.

As duplicatas de teste que criamos têm implementações reais e em funcionamento. Nos bastidores, cada um armazena uma coleção concreta de objetos, e eles adicionarão e removerão objetos dessa coleção à medida que manipularmos o repositório durante um teste. Alguns desenvolvedores gostam de criar suas duplicatas de teste dessa maneira – com código real e implementações de trabalho.  Essas duplicatas de teste são o que chamamos de *falsificações*. Eles têm implementações de trabalho, mas não são reais o suficiente para uso em produção. Na verdade, o repositório falso não grava no banco de dados. O servidor SMTP falso não envia realmente uma mensagem de email pela rede.

### <a name="mocks-versus-fakes"></a>Simulações versus falsificações

Há outro tipo de duplicata de teste conhecida como uma *simulação*. Embora as falsificações tenham implementações em funcionamento, as simulações vêm sem implementação. Com a ajuda de uma estrutura de objetos fictício, construímos esses objetos fictícios em tempo de execução e os usamos como duplicatas de teste. Nesta seção, usaremos a estrutura de simulação de código-fonte aberto MOQ. Veja um exemplo simples de como usar MOQ para criar dinamicamente um Double de teste para um repositório de funcionários.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Pedimos MOQ para uma implementação IRepository @ no__t-0Employee @ no__t-1 e ele cria um dinamicamente. Podemos acessar o objeto que implementa IRepository @ no__t-0Employee @ no__t-1 acessando a propriedade Object do objeto fictício @ no__t-2T @ no__t-3. É esse objeto interno que podemos passar para nossos controladores, e eles não saberão se esse é um duplo de teste ou o real Repository. Podemos invocar métodos no objeto da mesma forma que invocamos métodos em um objeto com uma implementação real.

Você deve estar se perguntando o que o repositório de simulações fará quando invocarmos o método Add. Como não há nenhuma implementação por trás do objeto fictício, Add não faz nada. Não há nenhuma coleção concreta nos bastidores, como tivemos com as falsificações que escrevemos, portanto, o funcionário é Descartado. E quanto ao valor de retorno de FindById? Nesse caso, o objeto fictício faz a única coisa que ele pode fazer, que é retornar um valor padrão. Como estamos retornando um tipo de referência (um funcionário), o valor de retorno é um valor nulo.

As simulações podem soar inúteis; no entanto, há mais dois recursos de simulações sobre os quais não falamos. Primeiro, a estrutura MOQ registra todas as chamadas feitas no objeto fictício. Posteriormente, no código, podemos perguntar MOQ se alguém invocou o método Add ou se alguém invocou o método FindById. Veremos mais adiante como podemos usar esse recurso de gravação de "caixa preta" em testes.

O segundo grande recurso é como podemos usar MOQ para programar um objeto fictício com *expectativas*. Uma expectativa diz ao objeto fictício como responder a qualquer interação determinada. Por exemplo, podemos programar uma expectativa em nossa simulação e pedir que ela retorne um objeto Employee quando alguém invocar FindById. A estrutura MOQ usa uma API de instalação e expressões lambda para programar essas expectativas.

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

Neste exemplo, solicitamos que o MOQ crie um repositório dinamicamente e, em seguida, programamos o repositório com uma expectativa. A expectativa diz ao objeto fictício para retornar um novo objeto Employee com um valor de ID 5 quando alguém invoca o método FindById passando um valor de 5. Esse teste passa, e não precisamos criar uma implementação completa para falsificar IRepository @ no__t-0T @ no__t-1.

Vamos revisitar os testes que escrevemos anteriormente e reutilizá-los para usar simulações em vez de falsificações. Assim como antes, usaremos uma classe base para configurar as partes comuns da infraestrutura de que precisamos para todos os testes do controlador.

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

O código de instalação permanece basicamente o mesmo. Em vez de usar falsificações, usaremos MOQ para construir objetos fictícios. A classe base organiza a unidade de simulação de trabalho para retornar um repositório fictício quando o código invoca a propriedade Employees. O restante da configuração de simulação ocorrerá dentro dos acessórios de teste dedicados a cada cenário específico. Por exemplo, o acessório de teste para a ação de índice configurará o repositório de simulações para retornar uma lista de funcionários quando a ação invocar o método FindAll do repositório fictício.

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

Exceto pelas expectativas, nossos testes são semelhantes aos testes que tínhamos antes. No entanto, com a capacidade de gravação de uma estrutura fictícia, podemos abordar os testes de um ângulo diferente. Veremos essa nova perspectiva na próxima seção.

### <a name="state-versus-interaction-testing"></a>Teste de Estado versus interação

Há técnicas diferentes que você pode usar para testar o software com objetos fictícios. Uma abordagem é usar o teste baseado em estado, que é o que fizemos neste documento até agora. O teste baseado em estado faz asserções sobre o estado do software. No último teste, invocamos um método de ação no controlador e fizemos uma asserção sobre o modelo que deveria criar. Aqui estão alguns exemplos de estado de teste:

-   Verifique se o repositório contém o novo objeto Employee após a criação de execute.
-   Verifique se o modelo contém uma lista de todos os funcionários após a execução do índice.
-   Verifique se o repositório não contém um determinado funcionário após a execução da exclusão.

Outra abordagem que você verá com objetos fictícios é verificar as *interações*. Embora o teste baseado em estado faça declarações sobre o estado dos objetos, o teste baseado em interação faz asserções sobre como os objetos interagem. Por exemplo:

-   Verifique se o controlador invoca o método Add do repositório quando CREATE executa.
-   Verifique se o controlador invoca o método FindAll do repositório quando o índice é executado.
-   Verifique se o controlador invoca a unidade do método de confirmação do trabalho para salvar as alterações quando a edição é executada.

O teste de interação geralmente requer menos dados de teste, porque não estamos na análise de coleções e verificando contagens. Por exemplo, se soubermos que a ação detalhes invoca o método FindById do repositório com o valor correto, a ação provavelmente está se comportando corretamente. Podemos verificar esse comportamento sem configurar dados de teste para retornar do FindById.

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

A única configuração necessária no acessório de teste acima é a configuração fornecida pela classe base. Quando invocamos a ação do controlador, MOQ registrará as interações com o repositório fictício. Usando a API Verify de MOQ, podemos perguntar MOQ se o controlador invocava FindById com o valor de ID apropriado. Se o controlador não invocar o método ou invocar o método com um valor de parâmetro inesperado, o método Verify gerará uma exceção e o teste falhará.

Aqui está outro exemplo para verificar se a ação de criação invoca a confirmação na unidade de trabalho atual.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Um perigo do teste de interação é a tendência de especificar interações. A capacidade do objeto fictício de registrar e verificar cada interação com o objeto fictício não significa que o teste deve tentar verificar cada interação. Algumas interações são detalhes de implementação e você só deve verificar as interações *necessárias* para atender ao teste atual.

A escolha entre simulações ou falsificações depende em grande parte do sistema que você está testando e suas preferências pessoais (ou de equipe). Os objetos fictícios podem reduzir drasticamente a quantidade de código de que você precisa para implementar duplicatas de teste, mas nem todos se sentem confortável em expectativas de programação e verificação de interações.

## <a name="conclusions"></a>Conclusões

Neste documento, demonstramos várias abordagens para criar um código que pode ser testado ao usar o Entity Framework ADO.NET para persistência de dados. Podemos aproveitar abstrações internas, como IObjectSet @ no__t-0T @ no__t-1, ou criar nossas próprias abstrações, como IRepository @ no__t-2T @ no__t-3.  Em ambos os casos, o suporte do POCO no ADO.NET Entity Framework 4,0 permite que os consumidores dessas abstrações permaneçam que desconhecem persistentes e altamente testados. Recursos de EF4 adicionais, como carregamento lento implícito, permitem que o código de negócios e de serviço de aplicativo funcione sem se preocupar com os detalhes de um armazenamento de dados relacional. Por fim, as abstrações que criamos são fáceis de simular ou falsas dentro dos testes de unidade, e podemos usar essas duplicatas de teste para obter testes de execução rápida, altamente isolados e confiáveis.

### <a name="additional-resources"></a>Recursos adicionais

-   Robert C. Martin, " [princípio de responsabilidade única](https://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler, [Catálogo de padrões](https://www.martinfowler.com/eaaCatalog/index.html) de *padrões de arquitetura de aplicativos empresariais*
-   Griffin Caprio, " [injeção de dependência](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog de programação de dados, "[Walkthrough: Desenvolvimento controlado por testes com o Entity Framework 4.0 @ no__t-0 ".
-   Blog de programação de dados, " [usando o repositório e os padrões de unidade de trabalho com o Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Aaron Jensen, " [apresentando especificações de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [BDD com MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [design controlado por domínio](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler, " [simulações não são stubs](https://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler, " [duplicata de teste](https://martinfowler.com/bliki/TestDouble.html)"
-   [MOQ](https://code.google.com/p/moq/)

### <a name="biography"></a>Sumi

Scott Allen é membro da equipe técnica da Pluralsight e do fundador da OdeToCode.com. Em 15 anos de desenvolvimento de software comercial, Scott trabalhou em soluções para tudo, desde dispositivos incorporados de 8 bits até aplicativos Web ASP.NET altamente escalonáveis. Você pode acessar Scott em seu blog em OdeToCode ou no Twitter em [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).
