---
title: Convenções de Code First personalizadas-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419223"
---
# <a name="custom-code-first-conventions"></a>Convenções de Code First personalizadas
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Ao usar Code First seu modelo é calculado de suas classes usando um conjunto de convenções. As [convenções de Code First](~/ef6/modeling/code-first/conventions/built-in.md) padrão determinam coisas como a propriedade que se torna a chave primária de uma entidade, o nome da tabela à qual uma entidade mapeia e em que precisão e escala uma coluna decimal tem por padrão.

Às vezes, essas convenções padrão não são ideais para seu modelo e você precisa contorná-las Configurando muitas entidades individuais usando as anotações de dados ou a API fluente. As convenções de Code First personalizadas permitem que você defina suas próprias convenções que fornecem padrões de configuração para seu modelo. Neste tutorial, exploraremos os diferentes tipos de convenções personalizadas e como criar cada uma delas.


## <a name="model-based-conventions"></a>Convenções baseadas em modelo

Esta página aborda a API DbModelBuilder para convenções personalizadas. Essa API deve ser suficiente para criar a maioria das convenções personalizadas. No entanto, também há a capacidade de criar convenções baseadas em modelo – convenções que manipulam o modelo final após sua criação – para lidar com cenários avançados. Para obter mais informações, consulte [convenções baseadas em modelos](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Nosso modelo

Vamos começar definindo um modelo simples que podemos usar com nossas convenções. Adicione as seguintes classes ao seu projeto.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Apresentando convenções personalizadas

Vamos escrever uma convenção que configure qualquer propriedade chamada Key para ser a chave primária para seu tipo de entidade.

As convenções são habilitadas no construtor de modelos, que pode ser acessado por meio da substituição de OnModelCreating no contexto. Atualize a classe ProductContext da seguinte maneira:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Agora, qualquer propriedade em nosso modelo chamado Key será configurada como a chave primária de qualquer entidade de sua parte.

Também poderíamos tornar nossas convenções mais específicas filtrando o tipo de propriedade que iremos configurar:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Isso configurará todas as propriedades chamadas Key para ser a chave primária de sua entidade, mas somente se forem um número inteiro.

Um recurso interessante do método IsKey é que ele é aditivo. Isso significa que, se você chamar IsKey em várias propriedades e todas elas se tornarão parte de uma chave composta. A única limitação disso é que, ao especificar várias propriedades para uma chave, você também deve especificar uma ordem para essas propriedades. Você pode fazer isso chamando o método HasColumnOrder como abaixo:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Esse código configurará os tipos em nosso modelo para ter uma chave composta que consiste na coluna de chave int e na coluna de nome da cadeia de caracteres. Se exibirmos o modelo no designer, ele ficaria assim:

![Chave composta](~/ef6/media/compositekey.png)

Outro exemplo de convenções de propriedade é configurar todas as propriedades DateTime em meu modelo para mapear para o tipo datetime2 em SQL Server em vez de DateTime. Você pode conseguir isso com o seguinte:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Classes de Convenção

Outra maneira de definir convenções é usar uma classe de Convenção para encapsular sua Convenção. Ao usar uma classe de Convenção, você cria um tipo que herda da classe de Convenção no namespace System. Data. Entity. ModelConfiguration. convenções.

Podemos criar uma classe de convenção com a Convenção datetime2 que mostramos anteriormente fazendo o seguinte:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Para instruir o EF a usar essa convenção, adicione-a à coleção de convenções no OnModelCreating, que, se você tiver trabalhado a seguir, com o passo a passos, terá a seguinte aparência:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Como você pode ver, adicionamos uma instância de nossa Convenção à coleção de convenções. A herança de Convenção fornece uma maneira conveniente de agrupar e compartilhar convenções entre equipes ou projetos. Você poderia, por exemplo, ter uma biblioteca de classes com um conjunto comum de convenções que todos os seus projetos de organizações usam.

 

## <a name="custom-attributes"></a>Atributos personalizados

Outro grande uso de convenções é permitir que novos atributos sejam usados ao configurar um modelo. Para ilustrar isso, vamos criar um atributo que possamos usar para marcar as propriedades de cadeia de caracteres como não-Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Agora, vamos criar uma Convenção para aplicar esse atributo ao nosso modelo:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Com essa convenção, podemos adicionar o atributo nonunicode a qualquer uma de nossas propriedades de cadeia de caracteres, o que significa que a coluna no banco de dados será armazenada como varchar em vez de nvarchar.

Uma coisa a ser observada sobre essa Convenção é que, se você colocar o atributo não Unicode em algo diferente de uma propriedade de cadeia de caracteres, ele gerará uma exceção. Ele faz isso porque você não pode configurar isunicode em qualquer tipo que não seja uma cadeia de caracteres. Se isso acontecer, você poderá tornar sua Convenção mais específica, para que ela filtre tudo que não seja uma cadeia de caracteres.

Embora a Convenção acima funcione para definir atributos personalizados, há outra API que pode ser muito mais fácil de usar, especialmente quando você deseja usar propriedades da classe Attribute.

Para este exemplo, vamos atualizar nosso atributo e alterá-lo para um atributo isunicode, portanto, ele tem esta aparência:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Assim que tivermos isso, podemos definir um bool em nosso atributo para informar à Convenção se uma propriedade deve ser Unicode ou não. Poderíamos fazer isso na Convenção que já temos acessando o ClrProperty da classe de configuração como esta:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Isso é fácil o suficiente, mas há uma maneira mais sucinta de conseguir isso usando o método com da API de convenções. O método with tem um parâmetro do tipo Func&lt;PropertyInfo, T&gt; que aceita o PropertyInfo da mesma forma que o método Where, mas é esperado que ele retorne um objeto. Se o objeto retornado for nulo, a propriedade não será configurada, o que significa que você pode filtrar Propriedades com ele, exatamente como, mas é diferente, pois ele também capturará o objeto retornado e o passará para o método configure. Isso funciona da seguinte maneira:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Os atributos personalizados não são o único motivo para usar o método com, ele é útil em qualquer lugar que você precise dizer sobre algo que você está filtrando ao configurar seus tipos ou propriedades.

 

## <a name="configuring-types"></a>Configurando tipos

Até agora, todas as nossas convenções foram para propriedades, mas há outra área da API de convenções para configurar os tipos em seu modelo. A experiência é semelhante às convenções que vimos até agora, mas as opções dentro da configuração estarão na entidade em vez de no nível de propriedade.

Uma das coisas com as quais as convenções de nível de tipo pode ser realmente útil é alterar a Convenção de nomenclatura de tabela, mapear para um esquema existente que seja diferente do padrão do EF ou criar um novo banco de dados com uma Convenção de nomenclatura diferente. Para fazer isso, primeiro precisamos de um método que possa aceitar o TypeInfo para um tipo em nosso modelo e retornar o que o nome da tabela para esse tipo deve ser:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Esse método usa um tipo e retorna uma cadeia de caracteres que usa letras minúsculas com sublinhados em vez de CamelCase. Em nosso modelo, isso significa que a classe ProductCategory será mapeada para uma tabela chamada Product\_Category em vez de ProductCategories.

Assim que tivermos esse método, podemos chamá-lo em uma convenção como esta:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Essa convenção configura todos os tipos em nosso modelo para mapear para o nome da tabela que é retornada do nosso método getTableName. Essa convenção é o equivalente a chamar o método ToTable para cada entidade no modelo usando a API Fluent.

Uma coisa a ser observada sobre isso é que, quando você chamar a tarefa, o EF usará a cadeia de caracteres que você fornece como o nome exato da tabela, sem nenhuma das pluralização que normalmente faria ao determinar os nomes das tabelas. É por isso que o nome da tabela de nossa Convenção é produto\_categoria em vez de categorias de\_de produto. Podemos resolver isso em nossa Convenção fazendo uma chamada para o serviço de pluralização por si mesmos.

No código a seguir, usaremos o recurso de [resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md) adicionado em EF6 para recuperar o serviço de pluralização que o EF teria usado e Pluralize nosso nome de tabela.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> A versão genérica do GetService é um método de extensão no namespace System. Data. Entity. Infrastructure. DependencyResolution, você precisará adicionar uma instrução using ao seu contexto a fim de usá-la.

### <a name="totable-and-inheritance"></a>Por tabela e herança

Outro aspecto importante da paraTable é que, se você mapear explicitamente um tipo para uma determinada tabela, poderá alterar a estratégia de mapeamento que o EF usará. Se você chamar natable para cada tipo em uma hierarquia de herança, passando o nome do tipo como o nome da tabela como fizemos acima, você alterará a estratégia de mapeamento padrão tabela por hierarquia (TPH) para tabela por tipo (TPT). A melhor maneira de descrever isso é whith um exemplo concreto:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Por padrão, o funcionário e o gerente são mapeados para a mesma tabela (funcionários) no banco de dados. A tabela conterá funcionários e gerentes com uma coluna discriminadora que informará qual tipo de instância está armazenado em cada linha. Esse é o mapeamento TPH, pois há uma única tabela para a hierarquia. No entanto, se você chamar outtable em ambas as classes, cada tipo será mapeado para sua própria tabela, também conhecida como TPT, já que cada tipo tem sua própria tabela.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

O código acima será mapeado para uma estrutura de tabela parecida com a seguinte:

![Exemplo de TPT](~/ef6/media/tptexample.jpg)

Você pode evitar isso e manter o mapeamento TPH padrão, de duas maneiras:

1.  Chame natable com o mesmo nome de tabela para cada tipo na hierarquia.
2.  Chame untable somente na classe base da hierarquia, em nosso exemplo que seria Employee.

 

## <a name="execution-order"></a>Ordem de execução

As convenções operam da última maneira com o WINS, o mesmo que a API fluente. Isso significa que, se você escrever duas convenções que configuram a mesma opção da mesma propriedade, então a última para executar o WINS. Por exemplo, no código abaixo, o comprimento máximo de todas as cadeias de caracteres é definido como 500, mas então configuramos todas as propriedades chamadas Name no modelo para ter um comprimento máximo de 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Como a Convenção para definir o comprimento máximo como 250 é após a que define todas as cadeias de caracteres como 500, todas as propriedades chamadas Name em nosso modelo terão um MaxLength de 250, enquanto quaisquer outras cadeias de caracteres, como descrições, seriam 500. Usar convenções dessa maneira significa que você pode fornecer uma Convenção geral para tipos ou propriedades em seu modelo e, em seguida, substituir-los para subconjuntos que são diferentes.

A API fluente e as anotações de dados também podem ser usadas para substituir uma convenção em casos específicos. Em nosso exemplo acima, se tivéssemos usado a API fluente para definir o comprimento máximo de uma propriedade, poderíamos colocá-la antes ou depois da Convenção, porque a API fluente mais específica ganhará sobre a Convenção de configuração mais geral.

 

## <a name="built-in-conventions"></a>Convenções internas

Como as convenções personalizadas podem ser afetadas pelas convenções de Code First padrão, pode ser útil adicionar convenções para execução antes ou depois de outra convenção. Para fazer isso, você pode usar os métodos AddBefore e AddAfter da coleção de convenções em seu DbContext derivado. O código a seguir adicionaria a classe de Convenção que criamos anteriormente para que ela seja executada antes da Convenção de descoberta de chave interna.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Esse será o mais usado ao adicionar convenções que precisam ser executadas antes ou depois das convenções internas, uma lista das convenções internas pode ser encontrada aqui: [System. Data. Entity. ModelConfiguration. Conventions namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).

Você também pode remover as convenções que você não deseja aplicar ao seu modelo. Para remover uma convenção, use o método remove. Aqui está um exemplo de como remover o PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
