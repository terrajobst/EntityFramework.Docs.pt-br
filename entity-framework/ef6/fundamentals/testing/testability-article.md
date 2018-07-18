---
title: Capacidade de teste e o Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
caps.latest.revision: 3
ms.openlocfilehash: 61773f8a23ff54ddb78aeeb5656c669b12f91478
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119845"
---
# <a name="testability-and-entity-framework-40"></a>Capacidade de teste e o Entity Framework 4.0
Scott Allen

Publicado em: Maio de 2010

## <a name="introduction"></a>Introdução

Este white paper descreve e demonstra como escrever código que podem ser testado com o ADO.NET Entity Framework 4.0 e o Visual Studio 2010. Este documento não tenta se concentrar em uma metodologia de teste específica, como o design orientado a testes (TDD) ou o design controlado por comportamento (BDD). Em vez disso, este documento se concentra em como escrever código que usa o ADO.NET Entity Framework e ainda mais fácil isolar e testar de forma automática. Vamos examinar os padrões comuns de design que facilitam a cenários de acesso de teste nos dados e ver como aplicar esses padrões ao usar a estrutura. Também vamos examinar os recursos específicos do framework para ver como esses recursos podem trabalhar no código testável.

## <a name="what-is-testable-code"></a>O que é o código testável?

A capacidade de verificar se uma parte do software usando testes de unidade automatizados oferece muitos benefícios desejáveis. Todos sabem que bons testes reduzirá o número de defeitos de software em um aplicativo e o aumento de qualidade do aplicativo - mas ter testes de unidade em vigor vai muito além de apenas encontrar bugs.

Um conjunto de testes de unidade aprovados permite que uma equipe de desenvolvimento economizar tempo e permanece no controle de software que criam. Uma equipe pode fazer alterações no código existente, o refactor, reestruturação e reestruturação de software para atender aos novos requisitos e adicionar novos componentes em um aplicativo ao mesmo tempo, sabendo que o conjunto de testes pode verificar o comportamento do aplicativo. Testes de unidade são parte de um ciclo rápido feedback para facilitar a alteração e preservar a facilidade de manutenção de software, como complexidade aumenta.

Teste de unidade vem com um preço, no entanto. Uma equipe deve investir o tempo para criar e manter os testes de unidade. A quantidade de esforço necessário para criar esses testes está diretamente relacionada para o **possibilidade de teste** do software subjacente. Como é fácil testar o software? Uma equipe de design de software com capacidade de teste em mente criará efetivos testes mais rápido do que a equipe que trabalham com software que não podem ser testado.

Com capacidade de teste em mente, a Microsoft projetou o ADO.NET Entity Framework 4.0 (EF4). Isso não significa que os desenvolvedores escrever testes de unidade em relação ao código de estrutura em si. Em vez disso, os objetivos de capacidade de teste para EF4 tornam fácil criar código testável que se baseia em cima do framework. Antes de examinarmos exemplos específicos, vale a pena entender as qualidades de código testável.

### <a name="the-qualities-of-testable-code"></a>As qualidades de código testável

Código que é fácil de testar exibirão sempre pelo menos duas características. Primeiro, que pode ser testado o código é fácil **observar**. Dado um conjunto de entradas, deve ser fácil observar a saída do código. Por exemplo, o seguinte método de teste é fácil porque o método retorna o resultado de um cálculo diretamente.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Um método de teste é difícil, se o método grava o valor computado em um soquete de rede, uma tabela de banco de dados ou um arquivo, como o código a seguir. O teste deve executar o trabalho adicional para recuperar o valor.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Em segundo lugar, o código testável é fácil **isolar**. Vamos usar o pseudocódigo a seguir como um mau exemplo de código testável.

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

O método é fácil observar – podemos passar uma apólice e verifique se que o valor retornado corresponde a um resultado esperado. No entanto, para testar o método precisaremos instalar um banco de dados com o esquema correto e configurar o servidor SMTP, caso o método tenta enviar um email.

O teste de unidade apenas quer verificar se a lógica de cálculo dentro do método, mas o teste pode falhar porque o servidor de email está offline ou porque o servidor de banco de dados movido. Ambas essas falhas estão relacionadas ao comportamento que deseja que o teste para verificar. O comportamento é difícil de isolar.

Os desenvolvedores de software que lutam para escrever código testável geralmente se esforçar para manter uma separação de preocupações no código que escrevem. O método acima deve se concentrar em cálculos de negócios e delegar os detalhes da implementação do banco de dados e email a outros componentes. Robert Isso chama o princípio da responsabilidade única. Um objeto deve encapsular uma responsabilidade única e estreita, como calcular o valor de uma política. Todas as outras tarefas de banco de dados e notificação devem ser de responsabilidade de algum outro objeto. Código escrito dessa maneira é mais fácil isolar porque ele se concentra em uma única tarefa.

No .NET, temos as abstrações, precisamos seguir o princípio da responsabilidade única e obter o isolamento. Podemos usar as definições de interface e forçar o código para usar a abstração de interface em vez de um tipo concreto. Neste artigo, veremos como um método, como o exemplo de incorreto apresentado acima pode trabalhar com interfaces que *parecer* como eles se comunicará com o banco de dados. No entanto, no momento do teste, podemos pode substituir uma implementação fictícia que não se comunicar com o banco de dados, mas em vez disso, armazena dados na memória. Essa implementação fictícia será isolar o código de problemas não relacionados no código de acesso a dados ou banco de dados na configuração.

Há benefícios adicionais de isolamento. O cálculo de negócios no último método deve levar apenas alguns milissegundos para executar, mas o próprio teste pode ser executado por vários segundos como os saltos de código em torno de rede e fala para vários servidores. Testes de unidade devem ser executado rapidamente para facilitar a pequenas alterações. Testes de unidade devem também ser repetidos e não falha porque um componente não relacionado ao teste tem um problema. Escrevendo código que seja fácil para observar e isolar significa que os desenvolvedores terão uma hora mais fácil de escrever testes para o código, gastar menos tempo aguardando testes a serem executados e mais importante, gastam menos tempo que rastrear bugs que não existem.

Esperamos que você pode avaliar os benefícios dos testes e entender as qualidades que exibe o código testável. Estamos prestes a abordar como escrever código que funciona com o EF4 para salvar dados em um banco de dados, permanecendo observáveis e fácil isolar, mas primeiro, limitarei nosso foco para discutir os designs que podem ser testados para acesso a dados.

## <a name="design-patterns-for-data-persistence"></a>Padrões de design para persistência de dados

Os dois exemplos incorretos apresentados anteriormente tinham muitas responsabilidades. O primeiro exemplo incorreto teria que realizar um cálculo *e* gravar em um arquivo. O segundo exemplo ruim tinha que ler dados de um banco de dados *e* executam um cálculo de negócios *e* enviar email. Criando métodos menores que separam questões e delegar a responsabilidade para outros componentes de você fazer grandes avanços na direção de escrever um código testável. O objetivo é criar a funcionalidade pela composição de ações de abstrações de pequenas e focadas.

Quando se trata de persistência de dados de pequeno e focadas abstrações que estamos procurando são tão comuns que eles já foram documentados como padrões de design. Livro de Martin Fowler padrões de arquitetura de aplicações corporativas foi o primeiro trabalho para descrever esses padrões em versão impressa. Forneceremos uma breve descrição desses padrões nas seções a seguir antes de mostrarmos como esses ADO.NET Entity Framework implementa e funciona com esses padrões.

### <a name="the-repository-pattern"></a>O padrão de repositório

Fowler diz que um repositório "atua como mediador entre o domínio e os dados de camadas de mapeamento usando uma interface de coleção para acessar objetos de domínio". A meta do padrão de repositório é isolar o código das minúcias de acesso a dados e, como vimos anteriormente isolamento é uma característica necessária para a capacidade de teste.

A chave para o isolamento é como o repositório expõe objetos usando uma interface de coleção. A lógica que você escreve para usar o repositório não tem ideia de como o repositório será materializar os objetos que você solicitar. O repositório pode se comunicar com um banco de dados, ou ele pode simplesmente retornar objetos de uma coleção na memória. Tudo o que seu código precisa saber é que o repositório é exibido manter a coleção, e você pode recuperar, adicionar e excluir objetos da coleção.

Em aplicativos .NET existentes um repositório concreto geralmente herda de uma interface genérica semelhante ao seguinte:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Faremos algumas alterações na definição de interface quando podemos fornecer uma implementação para EF4, mas o conceito básico permanece o mesmo. Código pode usar um repositório concreto que implementa essa interface para recuperar uma entidade pelo seu valor de chave primária, para recuperar uma coleção de entidades com base na avaliação de um predicado, ou simplesmente recuperar todas as entidades disponíveis. O código também pode adicionar e remover entidades por meio da interface do repositório.

Dada uma objetos IRepository do funcionário, o código pode executar as operações a seguir.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Uma vez que o código está usando uma interface (IRepository do funcionário), podemos fornecer o código com diferentes implementações da interface. Uma implementação pode ser uma implementação apoiada por EF4 e objetos persistentes em um banco de dados do Microsoft SQL Server. Uma implementação diferente (um que usamos durante o teste) pode apoio de um objeto de lista de funcionários na memória. A interface ajudará a obter o isolamento no código.

Observe o IRepository&lt;T&gt; interface não expõe uma operação de salvamento. Como podemos atualizar objetos existentes? Você pode se deparar com definições de IRepository que incluem a operação de salvamento e implementações desses repositórios serão necessário persistir imediatamente um objeto no banco de dados. No entanto, em muitos aplicativos não queremos manter objetos individualmente. Em vez disso, queremos dar vida a objetos, talvez de diferentes repositórios, modificar esses objetos como parte de uma atividade de negócios e, em seguida, manter todos os objetos como parte de uma única operação atômica. Felizmente, há um padrão para permitir que esse tipo de comportamento.

### <a name="the-unit-of-work-pattern"></a>O padrão de unidade de trabalho

Fowler diz que uma unidade de trabalho "manterá uma lista de objetos afetados por uma transação comercial e coordena a gravação recusar alterações e a resolução de problemas de simultaneidade". É responsabilidade da unidade de trabalho para controlar as alterações aos objetos podemos dar vida a partir de um repositório e manter quaisquer alterações feitas aos objetos quando dizemos que a unidade de trabalho para confirmar as alterações. Também é responsabilidade da unidade de trabalho para levar os novos objetos que já adicionamos a todos os repositórios e inserir os objetos em um banco de dados e também gerenciar exclusão.

Se você já realizou qualquer trabalho com conjuntos de dados ADO.NET, em seguida, você já estará familiarizado com o padrão de unidade de trabalho. Conjuntos de dados ADO.NET tinha a capacidade de controlar as nossas atualizações, exclusões e inserção de objetos DataRow e poderia (com a Ajuda de um TableAdapter) reconciliar todas as nossas alterações para um banco de dados. No entanto, objetos de conjunto de dados modelo um subconjunto desconectado do banco de dados subjacente. O padrão de unidade de trabalho exibe o mesmo comportamento, mas funciona com objetos comerciais e objetos de domínio que são isolados do código de acesso a dados e não reconhece do banco de dados.

Uma abstração para modelar a unidade de trabalho no código do .NET pode parecer com o seguinte:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Expondo as referências de repositório da unidade de trabalho, que podemos garantir uma única unidade de trabalho de objeto tem a capacidade de controlar todas as entidades materializadas durante uma transação comercial. A implementação do método de confirmação para uma unidade real de trabalho é onde a toda a mágica acontece reconciliar as alterações de na memória com o banco de dados. 

Dada uma referência de IUnitOfWork, o código pode fazer alterações em objetos de negócios recuperados de um ou mais repositórios e salvar todas as alterações usando a operação de confirmação atômica.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>O padrão de carregamento lento

Fowler usa o carregamento lento de nome para descrever "um objeto que não contém todos os dados você precisa, mas sabe como obtê-lo". Carregamento lento transparente é um recurso importante para ter quando escrever código testável comercial e trabalhar com um banco de dados relacional. Por exemplo, considere o código a seguir.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Como a coleção de cartões de ponto é preenchida? Existem duas possíveis respostas. Uma resposta é que o repositório de funcionário, quando for solicitado para buscar um funcionário, emite uma consulta para recuperar o funcionário junto com as informações do funcionário associado de cartão de ponto. Bancos de dados relacionais isso geralmente requer uma consulta com uma cláusula JOIN e pode resultar em recuperar mais informações do que um aplicativo precisa. E se o aplicativo nunca precisa tocar a propriedade de cartões de ponto?

Uma segunda resposta é carregar a propriedade de cartões de ponto "sob demanda". Esse carregamento lento é implícita e transparente para a lógica de negócios, porque o código não chama APIs especiais para recuperar as informações de cartão de ponto. O código pressupõe que as informações de cartão de ponto estão presentes quando necessário. Há alguma mágica envolvida com o carregamento lento que geralmente envolve a interceptação de tempo de execução das invocações de método. O código de interceptação é responsável por conversar com o banco de dados e recuperar informações de cartão de ponto, deixando a lógica de negócios livre para ser a lógica de negócios. Essa mágica de carregamento lento permite que o código de negócios isolar próprio das operações de recuperação de dados e resulta em mais código testável.

A desvantagem de um carregamento lento é que, quando um aplicativo *faz* precisa das informações de cartão de ponto de código executará uma consulta adicional. Isso não é uma preocupação para muitos aplicativos, mas para aplicativos sensíveis ao desempenho ou aplicativos um loop por meio de um número de objetos de funcionário e execução de uma consulta para recuperar os cartões de tempo durante cada iteração do loop (um problema conhecido como N + 1 problema de consulta), o carregamento lento é uma operação de arrastar. Nesses cenários, um aplicativo talvez queira carregar avidamente as informações de cartão de ponto da maneira mais eficiente possível.

Felizmente, veremos como EF4 dá suporte a ambas as cargas lentas implícitas e eficiente eager carrega como podemos mover para a próxima seção e implementar esses padrões.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementar os padrões com o Entity Framework

A boa notícia é que todos os padrões de design que descrevemos na última seção são simples de implementar com o EF4. Para demonstrar, vamos usar um aplicativo simples do ASP.NET MVC para editar e exibir os funcionários e suas informações de cartão de ponto associado. Vamos começar usando os seguinte "plain old CLR objects" (POCOs). 

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

Essas definições de classe mudarão ligeiramente conforme exploramos diferentes abordagens e os recursos do EF4, mas a intenção é manter essas classes como ignorados com persistência (PI) quanto possível. Um objeto de PI não sabe *como*, ou até mesmo *se*, o estado em que ele contém reside dentro de um banco de dados. PI e POCOs andam de mãos dadas com o software que podem ser testado. Objetos usando uma abordagem POCO são menos restritos, mais flexível e mais fáceis de testar porque eles podem operar sem um banco de dados presente.

Com os POCOs in-loco podemos criar um modelo de dados de entidade (EDM) no Visual Studio (veja a Figura 1). Não usaremos o EDM para gerar código para nossas entidades. Em vez disso, queremos usar as entidades que podemos carinhosamente criar manualmente. Só usaremos o EDM para gerar nosso esquema de banco de dados e forneça os metadados do que ef4 precisa mapear objetos no banco de dados.

![eftest_01](~/ef6/media/eftest-01.jpg)

**Figura 1**

Observação: se você quiser desenvolver o modelo EDM pela primeira vez, é possível limpar, gerar código POCO do EDM. Você pode fazer isso com uma extensão do Visual Studio 2010 fornecida pela equipe de programabilidade de dados. Para baixar a extensão, inicie o Gerenciador de extensões no menu Ferramentas no Visual Studio e procurar a Galeria de modelos online "POCO" (consulte a Figura 2). Existem vários modelos POCO disponíveis para o EF. Para obter mais informações sobre como usar o modelo, consulte " [instruções passo a passo: POCO modelo para o Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![eftest_02](~/ef6/media/eftest-02.png)

**Figura 2**

De que esse ponto de partida de POCO, exploraremos duas abordagens diferentes para código testável. A primeira abordagem eu chamo a abordagem EF porque aproveita abstrações da API do Entity Framework para implementar as unidades de trabalho e repositórios. A segunda abordagem vamos criar nossas própria abstrações de repositório personalizado e, em seguida, veja as vantagens e desvantagens de cada abordagem. Vamos começar Explorando a abordagem do EF.  

### <a name="an-ef-centric-implementation"></a>Uma implementação centralizada do EF

Considere a seguinte ação do controlador de um projeto ASP.NET MVC. A ação recupera um objeto de funcionário e retorna um resultado para exibir uma exibição detalhada do funcionário.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

O código é testável? Há pelo menos dois testes que precisamos para verificar o comportamento da ação. Em primeiro lugar, gostaríamos de verificar se que a ação retorna a exibição correta – um teste simples. Também queremos escrever um teste para verificar se a ação recupera o funcionário correto e gostaríamos de fazer isso sem executar o código para consultar o banco de dados. Lembre-se de que queremos isolar o código em teste. Isolamento garantirá que o teste não falha devido a um bug no código de acesso a dados ou banco de dados na configuração. Se o teste falhar, podemos saberá que temos um bug na lógica do controlador e não em algum componente de sistema de nível inferior.

Para obter o isolamento, precisaremos algumas abstrações, como as interfaces são apresentadas anteriormente para repositórios e unidades de trabalho. Lembre-se de que o padrão de repositório foi projetado para mediar entre objetos de domínio e a camada de mapeamento de dados. Nesse cenário EF4 *está* os dados de mapeamento de camada e já fornece uma abstração de repositório como chamada IObjectSet&lt;T&gt; (do namespace Objects). A definição da interface é semelhante ao seguinte.

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

IObjectSet&lt;T&gt; atende aos requisitos de um repositório porque ele é semelhante a uma coleção de objetos (por meio de IEnumerable&lt;T&gt;) e fornece métodos para adicionar e remover objetos da coleção simulada. Os métodos de anexar e desanexar exponham recursos adicionais da API do EF4. Usar IObjectSet&lt;T&gt; como a interface para repositórios precisamos de uma unidade de abstração de trabalho para vincular repositórios juntos.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Uma implementação concreta desta interface se comunicará com o SQL Server e é fácil criar usando a classe ObjectContext do EF4. A classe ObjectContext é a unidade real de trabalho na API do EF4.

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

Colocando um IObjectSet&lt;T&gt; vida é tão fácil quanto chamar o método CreateObjectSet do objeto ObjectContext. Nos bastidores, o framework usará os metadados é fornecido no EDM para produzir um ObjectSet concreta&lt;T&gt;. Vamos acrescentar ao retornar o IObjectSet&lt;T&gt; porque ela ajuda a preservar a capacidade de teste no código do cliente de interface.

Essa implementação concreta é útil na produção, mas é necessário para se concentrar em como usaremos nosso abstração IUnitOfWork para facilitar os testes.

### <a name="the-test-doubles"></a>As duplicatas de teste

Para isolar a ação do controlador, precisaremos a capacidade de alternar entre a unidade real de trabalho (apoiada por um ObjectContext) e uma unidade de double ou "falso" de teste de trabalho (executando operações na memória). A abordagem comum para realizar esse tipo de troca é não permitir que o controlador MVC instanciar uma unidade de trabalho, mas em vez disso, passe a unidade de trabalho no controlador como um parâmetro de construtor.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

O código acima é um exemplo de injeção de dependência. Não permitimos que o controlador criar dependência-do-lo (a unidade de trabalho), mas injetar a dependência no controlador. Em um projeto MVC é comum usar um alocador de controlador personalizado em combinação com um contêiner de inversão de controle (IoC) para automatizar a injeção de dependência. Esses tópicos estão além do escopo deste artigo, mas você pode ler mais por seguindo as referências no final deste artigo.

Uma unidade falsa de implementação de trabalho que podemos usar para teste pode parecer semelhante ao seguinte.

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

Observe que a unidade falsa de trabalho expõe uma propriedade de confirmada. Às vezes é útil adicionar recursos a uma classe falsa que facilitam o teste. Nesse caso, é fácil observar se o código confirma uma unidade de trabalho, verificando a propriedade confirmada.

Também precisaremos uma falsa IObjectSet&lt;T&gt; mantenha objetos por funcionário e o cartão de ponto na memória. Podemos fornecer uma única implementação do uso de genéricos.

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

Essa duplicata de teste delega a maior parte de seu trabalho a um HashSet subjacente&lt;T&gt; objeto. Observe que IObjectSet&lt;T&gt; requer uma restrição genérica impondo T como uma classe (um tipo de referência) e também força que implementam IQueryable&lt;T&gt;. É fácil fazer com que uma coleção em memória são exibidos como um IQueryable&lt;T&gt; usando o operador LINQ padrão AsQueryable.

### <a name="the-tests"></a>Os testes

Testes de unidade tradicional usará uma única classe de teste para manter todos os testes para todas as ações em um único controlador MVC. Podemos escrever esses testes, ou qualquer tipo de teste de unidade, uso de memória de falsificações que criamos. No entanto, para este artigo, que estamos evitará o monolítico abordagem de classe de teste e nossos testes para se concentrar em uma parte específica da funcionalidade de grupo em vez disso.  Por exemplo, "Criar novo funcionário" pode ser a funcionalidade que desejamos testar, portanto, usamos uma única classe de teste para verificar se a ação de controlador único responsável por criar um novo funcionário.

Há algum código de configuração comum que precisamos para todas essas classes de teste refinada. Por exemplo, sempre precisamos criar nossos repositórios na memória e falsa unidade de trabalho. Também precisamos uma instância do controlador de funcionário com a unidade falsa de trabalho injetado. Compartilharemos este código de configuração comuns entre as classes de teste usando uma classe base.

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

A "mãe de objeto" usamos a classe base é um padrão comum para a criação de dados de teste. Uma mãe de objeto contém métodos de fábrica para criar uma instância de entidades de teste para uso em vários acessórios de teste.

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

Podemos usar o EmployeeControllerTestBase como a classe base para um número de Acessórios de teste (veja a Figura 3). Cada acessório de teste irá testar uma ação de controlador específica. Por exemplo, um acessório de teste irá se concentrar no teste de ação de criação usada durante uma solicitação HTTP GET (para exibir o modo de exibição para a criação de um funcionário), e um acessório diferente se concentra na ação de criação usada em uma solicitação HTTP POST (para levar as informações enviadas pela usuário para criar um funcionário). Cada classe derivada só é responsável pela instalação necessária em seu contexto específico e fornecem as declarações necessárias para verificar os resultados de seu contexto de teste específico.

![eftest_03](~/ef6/media/eftest-03.png)

**Figura 3**

O estilo de teste e a convenção de nomenclatura apresentado aqui não é necessário para o código testável – é apenas uma abordagem. Figura 4 mostra os testes em execução no Jet cérebros Resharper plug-in do executor de teste para Visual Studio 2010.

![eftest_04](~/ef6/media/eftest-04.png)

**Figura 4**

Com uma classe base para lidar com o código de configuração compartilhada, os testes de unidade para cada ação do controlador são pequeno e fácil de escrever. Os testes serão executados rapidamente (já que estamos executando operações na memória) e não devem falhar devido a infraestrutura relacionada ou as questões ambientais (porque isola a unidade sob teste).

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

Nesses testes, a classe base faz a maior parte do trabalho de instalação. Lembre-se de que o construtor de classe base cria o repositório na memória, uma unidade falsa de trabalho e uma instância da classe EmployeeController. A classe de teste deriva dessa classe base e enfoca as especificidades de testar o método Create. Nesse caso, as especificações resumem as etapas "Organizar, agir e afirmar", que você verá em qualquer procedimento de teste de unidade:

-   Crie um objeto newEmployee para simular os dados de entrada.
-   Invocar a ação Create do EmployeeController e passe o newEmployee.
-   Verifique se que a ação de criação produz os resultados esperados (funcionário aparece no repositório).

O que criamos nos permite testar qualquer uma das ações EmployeeController. Por exemplo, ao escrever testes para a ação Index do controlador de funcionário de nós pode herdar da classe base de teste para estabelecer a mesma configuração de base para nossos testes. Novamente a classe base criará o repositório na memória, a unidade falsa de trabalho e uma instância da EmployeeController. Os testes para a ação de índice só precisam se concentrar em invocar a ação de índice e teste as qualidades do modelo a ação retorna.

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

Os testes que estamos criando com o fakes na memória sejam orientados para testar o *estado* do software. Por exemplo, ao testar a ação Create que queremos inspecionar o estado do repositório, depois executa a ação de criação – o repositório mantém o novo funcionário?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Mais tarde, examinaremos interação com base em testes. Interação com base em testes perguntará se o código em teste invocado métodos adequados em nossos objetos e passado os parâmetros corretos. Agora passaremos a tampa outro padrão de design – o carregamento lento.

## <a name="eager-loading-and-lazy-loading"></a>O carregamento imediato e carregamento lento

Em algum momento na web do ASP.NET MVC aplicativo talvez que desejamos exibir informações de um funcionário e incluem o funcionário associado cartões de ponto. Por exemplo, podemos ter uma exibição de resumo de cartão de ponto que mostra o nome do funcionário e o número total de cartões no sistema. Há várias abordagens que podemos pode adotar para implementar esse recurso.

### <a name="projection"></a>Projeção

É uma abordagem mais fácil para criar o resumo construir um modelo dedicado para as informações que desejamos exibir no modo de exibição. Nesse cenário, o modelo pode parecer semelhante ao seguinte.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Observe que o EmployeeSummaryViewModel não é uma entidade – em outras palavras não é algo que desejamos manter no banco de dados. Apenas vamos usar essa classe a movimentação de dados no modo de exibição de uma maneira fortemente tipada. O modelo de exibição é como a transferência de dados de um DTO (objeto) porque ela contém nenhum comportamento (não há métodos) – apenas as propriedades. As propriedades manterá os dados que necessários para mover. É fácil criar uma instância de nesse modelo de exibição usando o operador de projeção padrão do LINQ – o operador Select.

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

Há dois recursos notáveis no código acima. Primeiro – o código é fácil de testar porque ele ainda é fácil observar e isolar. O operador Select funciona tão bem quanto em relação a nosso fakes na memória como faz em relação a unidade real de trabalho.

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

O segundo recurso notável é como o código permite que o EF4 gerar uma consulta única e eficiente para reunir informações de funcionário e o cartão de ponto juntos. Podemos carregou informações de funcionários e as informações de cartão de ponto no mesmo objeto sem usar quaisquer APIs especiais. O código simplesmente expressa as informações que ele requer o uso de operadores LINQ padrão que funcionam em relação a fontes de dados na memória, bem como fontes de dados remotas. EF4 foi capaz de converter as árvores de expressão geradas pela consulta LINQ e C\# compilador em uma consulta de T-SQL única e eficiente.

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

Há outras vezes quando não queremos trabalhar com um modelo de exibição ou o objeto DTO, mas com entidades reais. Quando nós sabemos que precisamos de um funcionário *e* cartões do funcionário, é possível carregar rapidamente os dados relacionados de maneira eficiente e não invasiva.

### <a name="explicit-eager-loading"></a>Carregamento adiantado explícito

Quando desejamos carregar avidamente as informações de entidade relacionada, precisamos de algum mecanismo para lógica de negócios (ou nesse cenário, a lógica de ação do controlador) para expressar o seu desejo de repositório. O ObjectQuery EF4&lt;T&gt; classe define um método Include para especificar os objetos relacionados a serem recuperados durante uma consulta. Lembre-se de ObjectContext EF4 expõe entidades por meio do ObjectSet concreta&lt;T&gt; classe que herda de ObjectQuery&lt;T&gt;.  Se estivéssemos usando ObjectSet&lt;T&gt; referências em nossa ação de controlador, poderíamos escrever o código a seguir para especificar um carregamento adiantado das informações de cartão de ponto para cada funcionário.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

No entanto, já que estamos tentando manter nosso código testável estamos não expondo ObjectSet&lt;T&gt; de fora da unidade real da classe de trabalho. Em vez disso, podemos contar com o IObjectSet&lt;T&gt; interface que é mais fácil forjar, mas IObjectSet&lt;T&gt; não define um método Include. A beleza do LINQ é que podemos criar nosso próprios operador Include.

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

Observe que esse operador Include é definido como um método de extensão para IQueryable&lt;T&gt; em vez de IObjectSet&lt;T&gt;. Isso nos dá a capacidade de usar o método com uma grande variedade de tipos possíveis, incluindo IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;e ObjectSet&lt;T&gt;. No caso da sequência subjacente não é um ObjectQuery original do EF4&lt;T&gt;, não haverá nenhum dano e o operador Include é não operacional. Se subjacente de sequência *está* um ObjectQuery&lt;T&gt; (ou derivado de ObjectQuery&lt;T&gt;), em seguida, EF4 será ver nosso requisito para obter dados adicionais e formular adequado do SQL consulta.

Com esse novo operador em vigor, pode solicitar explicitamente um carregamento adiantado das informações de cartão de ponto do repositório.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Quando executado em relação a um ObjectContext real, o código produz a seguinte consulta única. A consulta reúne informações suficientes do banco de dados em uma única viagem para materializar os objetos de funcionários e preencher totalmente sua propriedade de cartões de ponto.

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

A boa notícia é que o código dentro do método de ação permanece totalmente testável. Não precisamos fornecer todos os recursos adicionais para nosso falsificações suportar o operador Include. A má notícia é que tivemos de usar o operador Include dentro o código que queremos manter os com ignorância de persistência. Isso é um exemplo perfeito do tipo de vantagens e desvantagens, que você precisará avaliar ao compilar código testável. Há ocasiões em que você precisa para permitir que o vazamento de preocupações de persistência fora a abstração de repositório para atender às metas de desempenho.

A alternativa para o carregamento adiantado é carregamento lento. Meios de carregamento lento fazemos *não* precisa nosso código de negócios para anunciar explicitamente o requisito para os dados associados. Em vez disso, podemos usar nosso entidades no aplicativo e se os dados adicionais são necessárias do Entity Framework carrega os dados sob demanda.

### <a name="lazy-loading"></a>Carregamento lento

É fácil imaginar um cenário em que não sabemos o que precisará dados uma parte da lógica de negócios. Talvez sabemos que a lógica precisa de um objeto de funcionário, mas podemos pode ramificar em diferentes caminhos de execução em que alguns desses caminhos exigem informações de cartão de ponto do funcionário e outros não. Cenários como esse são perfeitos para o carregamento lento implícito porque dados magicamente aparecem em uma base conforme necessário.

Carregamento lento, também conhecido como adiado carregando, colocar alguns requisitos em nossos objetos de entidade. POCOs com ignorância de persistência true não enfrenta todos os requisitos da camada de persistência, mas ignorância de persistência true é praticamente impossível alcançar.  Em vez disso, medimos a ignorância de persistência em graus relativos. Seria uma pena se for necessário herdar de uma classe base da persistência orientada ou usar uma coleção especializada para alcançar o carregamento lento em POCOs. Felizmente, o EF4 tem uma solução menos intrusiva.

### <a name="virtually-undetectable"></a>Praticamente não puder ser detectada

Ao usar objetos POCO, EF4 pode gerar dinamicamente os proxies de tempo de execução para entidades. Esses proxies invisivelmente encapsulam os POCOs materializados e fornecem serviços adicionais por meio da interceptação cada propriedade obtenham e operação para executar o trabalho adicional de definição. Um desses serviços é o recurso de carregamento lento que estamos procurando. Outro serviço é um mecanismo que pode gravar quando o programa altera os valores de propriedade de uma entidade de controle de alterações eficiente. A lista de alterações é usada pelo ObjectContext durante o método SaveChanges para persistir as entidades modificadas usando comandos de atualização.

Para esses proxies funcionar, no entanto, eles precisam de uma maneira de se conectar ao get de propriedade e operações de conjunto em uma entidade e os proxies atingir esse objetivo, substituindo os membros virtuais. Dessa forma, se quisermos ter o carregamento lento implícito e o controle de alterações eficiente é necessário voltar para nossa definições de classe POCO e marcar propriedades como virtuais.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Podemos ainda dizer que a entidade Employee é principalmente persistência com ignorância. O único requisito é usar os membros virtuais, e isso não afeta a capacidade de teste do código. Não precisamos derivar de uma classe base especial ou até mesmo usar uma coleção especial dedicada a carregamento lento. Como o código demonstra, qualquer classe que implementa ICollection&lt;T&gt; está disponível para armazenar entidades relacionadas.

Também há uma pequena alteração que precisamos fazer dentro de nossa unidade de trabalho. É o carregamento lento *desativar* por padrão ao trabalhar diretamente com um objeto de ObjectContext. Há uma propriedade que podemos definir na propriedade ContextOptions para habilitar o carregamento adiado, e podemos definir essa propriedade dentro de nossa unidade real de trabalho se quisermos que habilite o carregamento lento em todos os lugares.

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

Com o carregamento lento implícito habilitado, o código do aplicativo pode usar um funcionário e o funcionário associados cartões permanecendo não reconhece o trabalho necessário para o EF carregar os dados extras.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Carregamento lento facilita escrever o código do aplicativo e com a mágica de proxy, o código permanece completamente testável. Fakes na memória da unidade de trabalho podem simplesmente pré-carregar entidades falsas com dados associados quando necessário durante um teste.

Neste ponto, voltaremos nossa atenção desde a criação de repositórios usando IObjectSet&lt;T&gt; e examine abstrações para ocultar todos os sinais a estrutura de persistência.

## <a name="custom-repositories"></a>Repositórios personalizados

Quando, apresentamos o padrão de unidade de trabalho design primeiro neste artigo, que fornecemos um exemplo de código para a unidade de trabalho possível aparência. Vamos apresentar essa ideia original usando o funcionário e o cenário de cartão de ponto de funcionário, com que temos trabalhado novamente.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

A principal diferença entre essa unidade de trabalho e a unidade de trabalho que criamos na última seção é como a unidade de trabalho não usa abstrações do EF4 framework (não há nenhum IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; funciona bem como uma interface de repositório, mas a API que ele expõe não pode alinhar perfeitamente com as necessidades do nosso aplicativo. Essa abordagem futura representaremos usando um IRepository personalizado de repositórios&lt;T&gt; abstração.

Muitos desenvolvedores que seguem o design controlado por teste, design controlado por comportamento e design de metodologias controlado por domínio preferem o IRepository&lt;T&gt; abordagem por vários motivos. Primeiro, o IRepository&lt;T&gt; interface representa uma camada "anticorrupção". Conforme descrito por Eric Evans em seu livro de Design controlado por domínio uma camada anticorrupção mantém o código de domínio para fora da infraestrutura de APIs, como uma API de persistência. Em segundo lugar, os desenvolvedores podem criar métodos para o repositório que atendem às necessidades exatas de um aplicativo (como descobertos durante a gravação de testes). Por exemplo, podemos com frequência talvez seja necessário localizar uma única entidade usando um valor de ID, portanto, podemos adicionar um método FindById para a interface do repositório.  Nosso IRepository&lt;T&gt; definição será semelhante ao seguinte.

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

Observe a removeremos voltar a usar um IQueryable&lt;T&gt; interface para expor as coleções de entidades. IQueryable&lt;T&gt; permite árvores de expressão do LINQ para fluir para o provedor do EF4 e dê o provedor de uma visão holística da consulta. Uma segunda opção seria retornar IEnumerable&lt;T&gt;, que significa que o provedor LINQ do EF4 só verão as expressões criadas dentro do repositório. Qualquer agrupamento, ordenação e projeção feitas fora do repositório não serão compostos para o comando SQL enviado ao banco de dados, o que pode prejudicar o desempenho. Por outro lado, um repositório retornando somente IEnumerable&lt;T&gt; resultados serão nunca surpreendê-lo com um novo comando SQL. Ambas as abordagens funcionará, e ambas as abordagens permanecem testáveis.

É simples fornecer uma única implementação do IRepository&lt;T&gt; interface usando a API do ObjectContext EF4 e genéricos.

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

O IRepository&lt;T&gt; abordagem nos dá algum controle adicional sobre nossas consultas como um cliente tem que invocar um método para obter a uma entidade. Dentro do método poderia fornecemos operadores LINQ para impor restrições de aplicativo e verificações adicionais. Observe que a interface possui duas restrições no parâmetro de tipo genérico. A primeira restrição é que os contras de classe prejudicar exigido pelo ObjectSet&lt;T&gt;, e a segunda restrição força nossas entidades para implementar IEntity – uma abstração criada para o aplicativo. A interface IEntity força entidades tenham uma propriedade de Id legível e pode, em seguida, usamos essa propriedade no método FindById. IEntity é definido com o código a seguir.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity poderia ser considerado uma violação de ignorância de persistência de pequeno, pois nossas entidades são necessários para implementar essa interface. Lembre-se de ignorância de persistência é sobre as vantagens e desvantagens, e para muitos, a funcionalidade de FindById superam a restrição imposta pela interface. A interface não tem impacto na capacidade de teste.

Criando uma instância de um ativo IRepository&lt;T&gt; requer um ObjectContext EF4, portanto, uma unidade concreta de implementação de trabalho deve gerenciar a instanciação.

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

Usar nosso repositório personalizado não é significativamente diferente de usar o repositório com base em IObjectSet&lt;T&gt;. Em vez de aplicar operadores LINQ diretamente a uma propriedade, primeiro precisamos invocar uma métodos do repositório para pegar um IQueryable&lt;T&gt; referência.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Observe que o operador Include personalizado que anteriormente implementamos irá funcionar sem alterações. Método de FindById do repositório remove ações ao tentar recuperar uma única entidade lógica duplicada.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Não há nenhuma diferença significativa das duas abordagens, examinamos a capacidade de teste. Poderíamos fornecer implementações fictícias de IRepository&lt;T&gt; criando classes concretas apoiadas por HashSet&lt;funcionário&gt; , exatamente como o que fizemos na última seção. No entanto, alguns desenvolvedores preferem usar objetos fictícios e simular as estruturas de objeto em vez de criar o fakes. Vamos examinar usar objetos fictícios para nossa implementação de teste e discutir as diferenças entre as simulações e fakes na próxima seção.

### <a name="testing-with-mocks"></a>Testes com objetos fictícios

Há diferentes abordagens para criar o que o chama de Martin Fowler uma "duplicata de teste". Um teste duplo (como um filme dublê) é um objeto que compilar para "coloque em funcionamento em" para o real, os objetos de produção durante os testes. Os repositórios de na memória que criamos são duplicatas de teste para os repositórios que se comunicam com o SQL Server. Já vimos como usar essas duplicatas de teste durante os testes de unidade para isolar o código e manter testes em execução rápida.

As duplicatas de teste que criamos têm implementações reais, trabalhar. Em segundo plano cada um armazena uma coleção concreta de objetos e eles serão adicionar e remover objetos dessa coleção, como podemos manipular o repositório durante um teste. Alguns desenvolvedores como criar suas duplicatas de teste dessa maneira – com o código real e implementações de trabalho.  Essas duplicatas de teste são o que chamamos *fakes*. Eles têm implementações de trabalho, mas eles não são reais para uso em produção. O repository falso, na verdade, não será gravado no banco de dados. O servidor SMTP falso, na verdade, não envia uma mensagem de email pela rede.

### <a name="mocks-versus-fakes"></a>Simulações versus Fakes

Há outro tipo de teste duas vezes conhecido como um *simular*. Enquanto o fakes têm implementações de trabalho, as simulações entram com nenhuma implementação. Com a Ajuda de uma estrutura de objeto, podemos construir esses objetos fictícios em tempo de execução e usá-los como duplicatas de teste. Nesta seção, que usaremos objetos fictícios Moq de software livre. Aqui está um exemplo simples de como usar o Moq para criar dinamicamente um teste duplo para um repositório de funcionário.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Solicitamos Moq IRepository&lt;funcionário&gt; implementação e ele criará um dinamicamente. Obter o objeto que implementa o IRepository&lt;funcionário&gt; acessando a propriedade do objeto da imitação&lt;T&gt; objeto. É esse objeto interno que podemos passar para os controladores, e eles não saberão se esta for uma duplicata de teste ou o repositório real. Podemos invocar métodos no objeto assim como podemos seria invocar métodos em um objeto com uma implementação real.

Você deve estar se perguntando o que o repositório fictício fará quando invocamos o método Add. Como não há nenhuma implementação por trás o objeto fictício, adicionar não fará nada. Não há nenhuma coleção concreta por trás dos bastidores tivemos com os fakes que escrevemos, portanto, o funcionário é descartado. O que o valor retornado de FindById? Nesse caso, o objeto fictício faz a única coisa que ele pode fazer, que é retornado um valor padrão. Uma vez que estamos retornando um tipo de referência (um funcionário), o valor de retorno é um valor nulo.

As simulações podem parecer inútil; No entanto, há dois recursos mais as simulações que não falamos sobre. Primeiro, o framework Moq registra todas as chamadas feitas no objeto fictício. Posteriormente no código podemos fazer Moq se alguém chamado o método Add, ou se qualquer pessoa que o método FindById foi invocado. Veremos mais tarde como podemos usar esse recurso de gravação de "caixa preta" em testes.

O segundo recurso excelente é como podemos usar Moq programar com um objeto de simulação *expectativas*. Uma expectativa informa o objeto fictício como responder a qualquer interação determinada. Por exemplo, podemos programar uma expectativa em nossa simulação e instruí-lo para retornar um objeto de funcionário quando alguém chama FindById. A estrutura de Moq usa uma API de instalação e as expressões lambda programar essas expectativas.

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

Neste exemplo, podemos pedir Moq para criar dinamicamente um repositório e, em seguida, nós Programamos o repositório com uma expectativa. A expectativa informa o objeto fictício para retornar um novo objeto de funcionário com um valor de Id de 5, quando alguém chama o método FindById passando um valor de 5. Esse teste é aprovado e não precisamos criar uma implementação completa de IRepository falsa&lt;T&gt;.

Vamos rever os testes que escrevemos antes e retrabalho-los para usar objetos fictícios em vez de fakes. Assim como antes, vamos usar uma classe base para configurar as partes comuns de infraestrutura que precisamos para todos os testes do controlador.

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

O código de configuração principalmente permanece o mesmo. Em vez de usar falsificações, usaremos o Moq para construir objetos fictícios. Organiza a classe base para a unidade de simulação de trabalho retornar um repositório fictício quando o código chama a propriedade de funcionários. O restante da instalação fictício ocorrerá dentro os acessórios de teste dedicado para cada cenário específico. Por exemplo, o acessório de teste para a ação de índice irá configurar o repositório fictício para retornar uma lista de funcionários, quando a ação chama o método FindAll do repositório fictício.

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

Exceto para as expectativas, nossos testes semelhante para os testes que tínhamos antes. No entanto, com a capacidade de gravação de uma estrutura fictícia pode aproximar a testes de um ângulo diferente. Vamos examinar essa nova perspectiva na próxima seção.

### <a name="state-versus-interaction-testing"></a>Estado versus testes de interação

Há diferentes técnicas que você pode usar para testar o software com objetos fictícios. Uma abordagem é usar o estado com base em testes, que é o que fizemos neste documento até o momento. Estado com base em declarações de teste faz sobre o estado do software. No último teste podemos invocado um método de ação no controlador e fez uma asserção sobre o modelo, que ele deverá ser compilado. Aqui estão alguns exemplos de estado de teste:

-   Verifique se que o repositório contém o novo objeto de funcionário após a execução de criação.
-   Verifique se que o modelo contém uma lista de todos os funcionários após a execução de índice.
-   Verifique se que o repositório não contém um determinado funcionário após a exclusão é executada.

Outra abordagem, você verá com objetos fictícios é verificar *interações*. Enquanto o estado com base em declarações de teste faz sobre o estado de objetos, interação com base em declarações de teste faz sobre como os objetos interagem. Por exemplo:

-   Verifique se que o controlador invoca método do repositório quando executa Create.
-   Verifique se que o controlador invoca o método de FindAll do repositório quando o índice é executada.
-   Verifique se que o controlador invoca a unidade do método de confirmação do trabalho para salvar as alterações quando editar é executado.

Geralmente interação teste requer menos dados de teste, porque nós não são investigação dentro de coleções e verificando contagens. Por exemplo, se nós sabemos que a ação detalhes invoca o método FindById de um repositório com o valor correto -, em seguida, a ação está provavelmente se comportando corretamente. Podemos verificar esse comportamento sem configurar quaisquer dados de teste para retornar do FindById.

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

A instalação somente necessária no acessório de teste acima é a configuração fornecida pela classe base. Quando invocamos a ação do controlador, Moq registrará as interações com o repositório fictício. Usando a API de verificar de Moq, podemos fazer Moq se o controlador invocado FindById com o valor da ID adequado. Se o controlador não invocou o método ou invocou o método com um valor de parâmetro inesperado, verifique se o método lançará uma exceção e o teste falhará.

Aqui está outro exemplo para verificar que a ação Create invoca confirmação na unidade de trabalho atual.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Um dos riscos com testes de interação é a tendência em especificar interações. A capacidade do objeto fictício para registrar e verificar cada interação com o objeto fictício não significa que o teste deve tentar verificar cada interação. Algumas interações são detalhes de implementação e você só deve verificar as interações *necessária* para satisfazer o teste atual.

A escolha entre objetos fictícios ou fakes amplamente depende do sistema que você está testando e sua conta pessoal (ou equipe) preferências. Objetos fictícios podem reduzir drasticamente a quantidade de código que você precisa implementar duplicatas de teste, mas nem todo mundo se sente confortável de expectativas de programação e verificando as interações.

## <a name="conclusions"></a>Conclusões

Neste artigo, demonstrei várias abordagens para criar o código que podem ser testado usando o ADO.NET Entity Framework para persistência de dados. Podemos aproveitar abstrações internas como IObjectSet&lt;T&gt;, ou criar nossas próprios abstrações, como IRepository&lt;T&gt;.  Em ambos os casos, o suporte a POCO no ADO.NET Entity Framework 4.0 permite que os consumidores dessas abstrações permaneça persistente com ignorância e altamente testável. Recursos adicionais do EF4, como carregamento lento implícito permite que o serviço de negócios e aplicativo de código para trabalhar sem se preocupar com os detalhes de um repositório de dados relacional. Por fim, as abstrações que podemos criar são fáceis de serem simule ou falsifique dentro de testes de unidade, e podemos usar essas duplicatas de teste para atingir a execução rápida, altamente isolada e os testes confiáveis.

### <a name="additional-resources"></a>Recursos adicionais

-   Robert, " [o princípio da responsabilidade única](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler [catálogo de padrões](http://www.martinfowler.com/eaaCatalog/index.html) de *padrões de arquitetura de aplicativo empresarial*
-   Caprio Griffin, " [injeção de dependência](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog de programação de dados, " [instruções passo a passo: desenvolvimento com o Entity Framework 4.0 orientado por testes](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Blog de programação de dados, " [padrões usando o repositório e unidade de trabalho com o Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels, " [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen, " [apresentando as especificações de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [BDD com MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [Design controlado por domínio](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler, " [Mocks não são Stubs](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler, " [duplo de teste](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, " [estado versus testes de interação](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   Moq, \<http://code.google.com/p/moq/>

### <a name="biography"></a>Biografia

Scott Allen é um membro da equipe técnica na Pluralsight e fundador da OdeToCode.com. Em 15 anos de desenvolvimento de software comercial, Scott trabalhou em soluções para tudo, desde dispositivos incorporados de 8 bits para aplicativos web ASP.NET com altamente escalonáveis. Você pode alcançar Scott em seu blog em OdeToCode ou no Twitter em \< http://twitter.com/OdeToCode>.
