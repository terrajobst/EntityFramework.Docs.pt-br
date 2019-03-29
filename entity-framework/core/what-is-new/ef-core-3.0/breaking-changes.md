---
title: Alterações significativas no EF Core 3.0 – EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7ed55d4cae36f6b25059a5b218db4b0d5e2fb266
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419738"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Alterações da falha incluídas no EF Core 3.0 (atualmente em versão prévia)

> [!IMPORTANT]
> Lembre-se de que os conjuntos de recursos e agendamentos de versões futuras sempre estão sujeitos a alterações. Tentaremos manter esta página atualizada, mas ela pode não refletir nossos planos mais recentes.

As seguintes alterações de comportamento e API têm o potencial de interromper os aplicativos desenvolvidos para EF Core 2.2. x ao atualizá-los para 3.0.0.
As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](../../providers/provider-log.md).
Interrupções em novos recursos introduzidos de uma versão prévia 3.0 para outra versão prévia 3.0 não estão documentadas aqui.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Consultas LINQ não são mais avaliadas no cliente

[Acompanhamento de problema nº 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Confira também o problema nº 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.
Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.

**Comportamento novo**

A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.
Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.

**Por que**

A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.
Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.
Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.
Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.
Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.

Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.

**Mitigações**

Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core

[Anúncios de acompanhamento de problema nº 325](https://github.com/aspnet/Announcements/issues/325)

Essa alteração foi introduzida no ASP.NET Core 3.0, versão prévia 1. 

**Comportamento antigo**

Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.

**Comportamento novo**

A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.

**Por que**

Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não. Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.

Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.
Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.

**Mitigações**

Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.

## <a name="query-execution-is-logged-at-debug-level"></a>A execução de consulta é registrada no nível de Depuração

[Acompanhamento de problema nº 14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, a execução de consultas e outros comandos era registrada em log no nível `Info`.

**Comportamento novo**

A partir do EF Core 3.0, o registro em log da execução do comando/SQL está no nível `Debug`.

**Por que**

Essa alteração foi feita para reduzir o ruído no nível de log de `Info`.

**Mitigações**

Esse evento de registro em log é definido por `RelationalEventId.CommandExecuting` com a ID de evento 20100.
Para fazer novamente o log do SQL no nível `Info`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`.
Por exemplo:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Valores de chave temporários não estão definidos em instâncias de entidade

[Acompanhamento de problema nº 12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.

**Comportamento antigo**

Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.
Normalmente, esses valores temporários eram grandes números negativos.

**Comportamento novo**

A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.

**Por que**

Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`. 

**Mitigações**

Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.
Isso pode ser evitado:
* Não usando chaves geradas pelo repositório.
* Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.
* Obter os valores de chave temporários reais de informações de acompanhamento de entidade.
Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.

## <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges respeita os valores de chave gerados pelo repositório

[Acompanhamento de problema nº 14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.

**Comportamento novo**

A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.
Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.
Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.

**Por que**

Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.

**Mitigações**

Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.
A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.
Por exemplo, com a API fluente:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Ou com anotações de dados:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Exclusões em cascata acontecerão imediatamente por padrão

[Acompanhamento de problema nº 10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.

**Comportamento novo**

A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.
Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.

**Por que**

Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.

**Mitigações**

O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangedTracker`.
Por exemplo:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>Tipos de consulta são consolidados com tipos de entidade

[Acompanhamento de problema nº 14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/query-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.
Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).

**Comportamento novo**

Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.
Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.

**Por que**

Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.
Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.
Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.

**Mitigações**

As seguintes partes da API agora estão obsoletas:
* **`ModelBuilder.Query<>()`** – Em vez disso, `ModelBuilder.Entity<>().HasNoKey()` precisa ser chamado para marcar um tipo de entidade como não tendo chaves.
Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.
* **`DbQuery<>`** – Em vez disso, `DbSet<>` deve ser usado.
* **`DbContext.Query<>()`** – Em vez disso, `DbContext.Set<>()` deve ser usado.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>A API de configuração para relações de tipo de propriedade mudou

[Acompanhamento de problema nº 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Acompanhamento de problema nº 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Acompanhamento de problema nº 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`. 

**Comportamento novo**

A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.
Por exemplo:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.
No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.
Por exemplo:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Além disso, chamar `Entity()`, `HasOne()` ou `Set()` com um destino de tipo próprio agora lançará uma exceção.

**Por que**

Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.
Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.

**Mitigações**

Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança

[Acompanhamento de problema nº 13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Considere o modelo a seguir:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
Antes do EF Core 3.0, a propriedade `CustomerId` seria usada para a chave estrangeira por convenção.
No entanto, se `Order` for um tipo de propriedade, isso também tornará `CustomerId` a chave primária, e essa normalmente não é a expectativa.

**Comportamento novo**

A partir da 3.0, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.
Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.
Por exemplo:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Por que**

Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.

**Mitigações**

Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Cada propriedade usa geração de chave de inteiro em memória independente

[Acompanhamento de problema nº 6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.

**Comportamento novo**

Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.
Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.

**Por que**

Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.

**Mitigações**

Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.
Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.

## <a name="backing-fields-are-used-by-default"></a>Campos de suporte são usados por padrão

[Acompanhamento de problema nº 12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.

**Comportamento antigo**

Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.
A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.

**Comportamento novo**

A partir do EF Core 3.0, se o campo de suporte para uma propriedade for conhecido, ele sempre lerá e gravará aquela propriedade usando o campo de suporte.
Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.

**Por que**

Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.

**Mitigações**

O comportamento pré-3.0 pode ser restaurado configurando o modo de acesso de propriedade na API fluente modelBuilder.
Por exemplo:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Lançado se vários campos de suporte compatíveis forem encontrados

[Acompanhamento de problema nº 12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.
Isso pode fazer o campo errado ser usado em casos ambíguos.

**Comportamento novo**

A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.

**Por que**

Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.

**Mitigações**

As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.
Por exemplo, usando a API fluente:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache

[Acompanhamento de problema nº 14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, chamar `AddDbContext` ou `AddDbContextPool` também registrava o log e serviços de cache de memória com a DI por meio de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Comportamento novo**

A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).

**Por que**

O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo. No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.

**Mitigações**

Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry agora executa uma DetectChanges local

[Acompanhamento de problema nº 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.
Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.

**Comportamento novo**

A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.
Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.

Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.

Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.

**Por que**

Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.

**Mitigações**

Chame `ChgangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão

[Acompanhamento de problema nº 14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.
Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.

**Comportamento novo**

A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.

**Por que**

Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.

**Mitigações**

O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.
Por exemplo, com a API fluente:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Ou com anotações de dados:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory agora é um serviço com escopo

[Acompanhamento de problema nº 14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.

**Comportamento novo**

A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.

**Por que**

Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.

**Mitigações**

Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.
Isso não é comum.
Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.

Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo mesclado em IDbContextOptionsExtension

[Acompanhamento de problema nº 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

`IDbContextOptionsExtensionWithDebugInfo` era uma interface opcional adicional estendida de `IDbContextOptionsExtension` para evitar fazer uma alteração da falha à interface durante o ciclo de versão 2. x.

**Comportamento novo**

As interfaces agora são mescladas em conjunto em `IDbContextOptionsExtension`.

**Por que**

Essa alteração foi feita porque as interfaces são conceitualmente uma.

**Mitigações**

Quaisquer implementações de `IDbContextOptionsExtension` precisarão ser atualizadas para dar suporte ao novo membro.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas

[Acompanhamento de problema nº 12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.
Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.
Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.

**Comportamento novo**

A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.
Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.
Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.
Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.

**Por que**

Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.

**Mitigações**

Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Criação excessiva de provedores de serviço internos agora é um erro por padrão

[Acompanhamento de problema nº 10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.

**Comportamento novo**

A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados. 

**Por que**

Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.

**Mitigações**

A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.
No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.
Por exemplo:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres

[Acompanhamento de problema nº 9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Essa alteração será introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.
Por exemplo:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.

Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.

**Comportamento novo**

A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.

**Por que**

O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.

**Mitigações**

Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.
Isso não é comum.
O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.
Por exemplo:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>A anotação Relational:TypeMapping agora é apenas TypeMapping

[Acompanhamento de problema nº 9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 2.

**Comportamento antigo**

O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".

**Comportamento novo**

O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".

**Por que**

Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.

**Mitigações**

Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.
A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable em um tipo derivado gera uma exceção 

[Acompanhamento de problema nº 11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido. 

**Comportamento novo**

A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.

**Por que**

Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.
Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.

**Mitigações**

Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex substituído por HasIndex 

[Acompanhamento de problema nº 12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.

**Comportamento novo**

A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.
Use `HasIndex().ForSqlServerInclude()`.

**Por que**

Essa alteração foi feita para consolidar a API para índices com `Includes` em um local para todos os provedores de banco de dados.

**Mitigações**

Use a nova API, conforme mostrado acima.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>O EF Core não envia mais pragma para imposição do FK SQLite

[Acompanhamento de problema nº 12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 3.

**Comportamento antigo**

Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.

**Comportamento novo**

A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.

**Por que**

Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.

**Mitigações**

Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.
Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3

**Comportamento antigo**

Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.

**Comportamento novo**

A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.

**Por que**

Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.

**Mitigações**

Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Os valores de Guid agora são armazenados como TEXTO no SQLite

[Acompanhamento de problema nº 15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Os valores de Guid foram previamente armazenados como valores BLOB no SQLite.

**Comportamento novo**

Agora, os valores de Guid são armazenados como TEXTO.

**Por que**

O formato binário de Guids não é padronizado. O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.

**Mitigações**

Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Os valores de char agora são armazenados como TEXTO no SQLite

[Problema de acompanhamento nº 15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite. Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.

**Comportamento novo**

Agora, os valores de char são armazenados como TEXTO.

**Por que**

O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.

**Mitigações**

Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

No EF Core, você pode continuar usando o comportamento anterior configurando um conversor de valor nessas propriedades.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>As IDs de migração agora são geradas usando o calendário da cultura invariável

[Problema de acompanhamento nº 12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

As IDs de migração eram geradas inadvertidamente usando o calendário da cultura atual.

**Comportamento novo**

Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).

**Por que**

A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem. O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.

**Mitigações**

Essa alteração afeta todos aqueles que usam um calendário não gregoriano no qual o ano é maior do que o do calendário gregoriano (como o calendário budista tailandês). As IDs de migração existentes precisarão ser atualizadas para que as novas migrações sejam ordenadas após as migrações existentes.

A ID da migração pode ser encontrada no atributo Migração nos arquivos de designer das migrações.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

A tabela de histórico de Migrações também precisa ser atualizada.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator foi renomeado

[Acompanhamento de problema nº 10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.

**Alteração**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Por que**

Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.

**Mitigações**

Use o novo nome. (Observe que o número da ID do evento não foi alterado.)

## <a name="clarify-api-for-foreign-key-constraint-names"></a>Esclarecer a API para nomes de restrição de chave estrangeira

[Acompanhamento de problema nº 10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

Essa alteração foi introduzida no EF Core 3.0 – versão prévia 4.

**Comportamento antigo**

Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome". Por exemplo:

```C#
var constraintName = myForeignKey.Name;
```

**Comportamento novo**

A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição". Por exemplo:

```C#
var constraintName = myForeignKey.ConstraintName;
```

**Por que**

Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.

**Mitigações**

Use o novo nome.
