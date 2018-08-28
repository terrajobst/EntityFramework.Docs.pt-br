---
title: Convenções de personalizadas do Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995822"
---
# <a name="custom-code-first-conventions"></a>Convenções de primeiro código personalizado
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Ao usar o Code First seu modelo é calculado a partir de suas classes usando um conjunto de convenções. O padrão [convenções de Code First](~/ef6/modeling/code-first/conventions/built-in.md) determinar coisas como qual propriedade torna-se a chave primária de uma entidade, o nome da tabela de uma entidade é mapeada para e quais precisão e escala de uma coluna decimal tem por padrão.

Às vezes, essas convenções padrão não são ideais para seu modelo, e você precisará solucioná-los por meio da configuração muitas entidades individuais usando Data Annotations ou a API Fluent. Custom Code First Conventions permitem que você definir suas próprias convenções que fornecem os padrões de configuração para o seu modelo. Neste passo a passo, iremos explorar os diferentes tipos de convenções personalizadas e como criar cada um deles.


## <a name="model-based-conventions"></a>Convenções de modelo

Esta página aborda a API DbModelBuilder para convenções personalizadas. Essa API deve ser suficiente para a maioria das convenções personalizadas de criação. No entanto, há também a capacidade de criar convenções baseado em modelo - convenções que manipulam o modelo final depois de criado, para lidar com cenários avançados. Para obter mais informações, consulte [convenções de modelo](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Nosso modelo

Vamos começar definindo um modelo simple que podemos usar com convenções. Adicione as seguintes classes ao seu projeto.

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

 

## <a name="introducing-custom-conventions"></a>Introdução ao convenções personalizadas

Vamos escrever uma convenção que configura a qualquer propriedade de chave para ser a chave primária para seu tipo de entidade nomeada.

Convenções são habilitadas no construtor de modelo, que pode ser acessado por meio da substituição OnModelCreating no contexto. Atualize a classe ProductContext da seguinte maneira:

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

Agora, qualquer propriedade em nosso modelo de chave nomeada será configurado como a chave primária de qualquer entidade sua parte.

Também podemos fazer convenções mais específicas por meio da filtragem no tipo de propriedade, vamos configurar:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Isso irá configurar todas as propriedades de chamada de chave para ser o principal chave da sua entidade, mas somente se eles são um número inteiro.

Um recurso interessante do método IsKey é que ele é aditiva. Que significa que, se você chamar IsKey em várias propriedades e eles se torna parte de uma chave composta. Uma limitação para isso é que, ao especificar várias propriedades para uma chave você deve também especificar uma ordem para essas propriedades. Você pode fazer isso chamando o HasColumnOrder método como abaixo:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Esse código irá configurar os tipos em nosso modelo para ter uma chave composta que consiste a coluna de chave int e a coluna de nome de cadeia de caracteres. Se podemos exibir o modelo no designer de ela teria esta aparência:

![compositeKey](~/ef6/media/compositekey.png)

Outro exemplo de convenções de propriedade é configurar todas as propriedades de data e hora no meu modelo para mapear para o tipo de datetime2 no SQL Server em vez de datetime. Você pode obter isso com o seguinte:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Classes de convenção

Outra maneira de definir as convenções é usar uma convenção de classe para encapsular sua convenção. Ao usar uma classe de convenção, em seguida, crie um tipo que herda da classe no namespace System.Data.Entity.ModelConfiguration.Conventions convenção.

Podemos criar uma classe de convenção com a convenção de datetime2 que mostramos anteriormente, fazendo o seguinte:

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

Para informar ao EF para usar essa convenção que adicioná-lo à coleção convenções em OnModelCreating, que se você vem acompanhando junto com o passo a passo terá esta aparência:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Como você pode ver, podemos adicionar uma instância de nossa convenção para a coleção de convenções. Herdando de convenção fornece uma maneira conveniente de agrupar e compartilhar convenções entre projetos ou equipes. Por exemplo, você teria uma biblioteca de classes com um conjunto comum de convenções que todos os seus departamentos projetos usam.

 

## <a name="custom-attributes"></a>Atributos personalizados

Outro ótimo uso das convenções é habilitar novos atributos a ser usado ao configurar um modelo. Para ilustrar isso, vamos criar um atributo que podemos usar para marcar as propriedades de cadeia de caracteres como não-Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Agora, vamos criar uma convenção para aplicar esse atributo com o nosso modelo:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Com essa convenção, podemos adicionar o atributo NonUnicode a qualquer uma das nossas propriedades de cadeia de caracteres, o que significa que a coluna no banco de dados será armazenado como varchar em vez de nvarchar.

Uma coisa a observar sobre essa convenção é que, se você colocar o atributo NonUnicode em algo diferente de uma propriedade de cadeia de caracteres, em seguida, ele lançará uma exceção. Isso ocorre porque você não pode configurar IsUnicode em qualquer tipo que não seja uma cadeia de caracteres. Se isso acontecer, em seguida, você pode fazer sua convenção de mais específico, para que ela filtra tudo o que não é uma cadeia de caracteres.

Embora a convenção acima funciona para definir atributos personalizados é outra API pode ser muito mais fácil de usar, especialmente quando você quiser usar as propriedades da classe de atributos.

Para este exemplo, vamos atualizar nossos atributos e alterá-lo a um atributo IsUnicode, para que fique assim:

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

Assim que tivermos isso, podemos definir um bool em nosso atributo para informar a convenção ou não uma propriedade deve ser Unicode. Podemos fazer isso na convenção de que já temos, acessando o ClrProperty da classe de configuração como este:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Isso é bastante simples, mas há uma maneira mais sucinta de conseguir isso usando Having método das convenções de API. Tendo o método tem um parâmetro do tipo Func&lt;PropertyInfo, T&gt; que aceita o PropertyInfo igual Where método, mas é esperado para retornar um objeto. Se o objeto retornado é nulo, em seguida, a propriedade não será configurada, que significa que você pode filtrar as propriedades com ele, assim como Where, mas é diferente que também irá capturar o objeto retornado e passá-lo para o método Configure. Isso funciona como o seguinte:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Atributos personalizados não são o único motivo para a necessidade de usar o método, ele é útil em qualquer lugar em que você precisa pensar sobre algo que você está filtrando ao configurar seus tipos ou propriedades.

 

## <a name="configuring-types"></a>Configurando tipos de

Até agora todas as convenções foram para propriedades, mas há outra área das convenções de API para configurar os tipos em seu modelo. A experiência é semelhante às convenções que vimos até agora, mas as opções dentro configurar estarão na entidade em vez da propriedade nível.

Uma das coisas que as convenções de nível de tipo podem ser muito útil para está mudando a convenção de nomenclatura tabela, para mapear para um esquema existente que é diferente do padrão EF ou criar um novo banco de dados com uma convenção de nomenclatura diferente. Para fazer isso primeiro precisamos de um método que pode aceitar o TypeInfo para um tipo em nosso modelo e o que deve ser o nome da tabela para esse tipo de retorno:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Esse método usa um tipo e retorna uma cadeia de caracteres que use letras minúsculas com sublinhados em vez de CamelCase. Em nosso modelo, isso significa que a classe ProductCategory será mapeada para uma tabela chamada produto\_categoria, em vez de ProductCategories.

Assim que tivermos esse método podemos pode chamá-lo em uma convenção de como esta:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Essa convenção configura todos os tipos em nosso modelo para mapear para o nome da tabela que é retornado do nosso método GetTableName. Essa convenção é o equivalente a chamar o método ToTable para cada entidade no modelo usando a API Fluent.

Uma coisa a observar sobre isso é que, quando você chama EF ToTable levará a cadeia de caracteres que você fornece como o nome da tabela exata, sem qualquer da pluralização que faria normalmente, ao determinar os nomes de tabela. Por produto, é o nome da tabela de nossa convenção\_categoria, em vez de produto\_categorias. Podemos pode resolver que em nossa convenção, fazendo uma chamada para o serviço de pluralização sozinhos.

O código a seguir, nós usaremos o [resolução de dependência](~/ef6/fundamentals/configuring/dependency-resolution.md) recurso adicionado no EF6 para recuperar o serviço de pluralização teria que usar o EF e Pluralizar o nome da tabela.

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
> A versão genérica de GetService é um método de extensão no namespace System.Data.Entity.Infrastructure.DependencyResolution, você precisará adicionar uma instrução para o contexto para usá-lo.

### <a name="totable-and-inheritance"></a>ToTable e herança

Outro aspecto importante de ToTable é que, se você mapear explicitamente um tipo para uma determinada tabela, em seguida, você pode alterar a estratégia de mapeamento que usará o EF. Se você chamar ToTable para cada tipo em uma hierarquia de herança, passando o nome do tipo como o nome da tabela, como fizemos acima, em seguida, você irá alterar a estratégia de mapeamento de tabela por hierarquia (TPH) padrão para a tabela por tipo (TPT). A melhor maneira de descrever isso é um exemplo concreto de whith:

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

Por padrão, funcionário e gerente são mapeados para a mesma tabela (funcionários) no banco de dados. A tabela conterá os funcionários e gerentes com uma coluna de discriminador que lhe dirá que tipo de instância é armazenado em cada linha. Isso é o mapeamento TPH pois não há uma única tabela para a hierarquia. No entanto, se você chamar ToTable em ambas as classe, em seguida, cada tipo será em vez disso, ser mapeado para sua própria tabela, também conhecido como TPT, já que cada tipo tem sua própria tabela.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

O código acima será mapeado para uma estrutura de tabela é semelhante ao seguinte:

![tptExample](~/ef6/media/tptexample.jpg)

Você pode evitar isso e manter o mapeamento TPH padrão, de duas maneiras:

1.  Chame ToTable com o mesmo nome de tabela para cada tipo na hierarquia.
2.  Chame ToTable apenas na classe base da hierarquia, em nosso exemplo que seria funcionário.

 

## <a name="execution-order"></a>Ordem de execução

Convenções de operam em uma maneira de wins último, o mesmo que a API Fluent. Isso significa é que, se você escrever duas convenções que configuram a mesma opção da mesma propriedade e, em seguida, o último para executar o wins. Por exemplo, no código a seguir, o comprimento máximo de todas as cadeias de caracteres é definido como 500, mas, em seguida, configuramos propriedades de todas as chamadas de nome no modelo para ter um comprimento máximo de 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Como é a convenção para definir o comprimento máximo para 250 posterior àquela que define todas as cadeias de caracteres para 500, todas as propriedades chamadas Name em nosso modelo terá um MaxLength de 250 enquanto outra cadeia de caracteres, como descrições, seria 500. Usando convenções dessa maneira significa que você pode fornecer uma convenção geral para tipos ou propriedades em seu modelo e, em seguida, substitui-las para subconjuntos são diferentes.

A API Fluent e anotações de dados também podem ser usadas para substituir uma convenção em casos específicos. Em nosso exemplo acima se tivéssemos usado a API Fluent para definir o comprimento máximo de uma propriedade, em seguida, poderia ter ele é colocado antes ou depois que a convenção, porque a API Fluent mais específicos vencerá a convenção de configuração mais gerais.

 

## <a name="built-in-conventions"></a>Convenções internas

Como convenções personalizadas podem ser afetadas por convenções Code First padrão, ele pode ser útil adicionar as convenções a serem executados antes ou após outra convenção. Para fazer isso, você pode usar os métodos de coleção convenções AddBefore e AddAfter no seu DbContext derivado. O código a seguir seria adicionar a classe de convenção que criamos anteriormente para que ele seja executado antes da compilação na convenção de descoberta da chave.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Isso vai ser mais usados ao adicionar as convenções que precisam ser executados antes ou depois das convenções internas, uma lista das convenções internas pode ser encontrada aqui: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Você também pode remover as convenções que você deseja não aplicadas ao seu modelo. Para remover uma convenção, use o método Remove. Aqui está um exemplo de remoção de PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
