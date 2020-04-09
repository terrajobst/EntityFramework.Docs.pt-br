---
title: Alterações significativas no EF Core 3.0 – EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 6e0c17a22b56b206f18e47f678e3e237d5c42375
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417457"
---
# <a name="breaking-changes-included-in-ef-core-30"></a>Alterações de quebra incluídas no EF Core 3.0

As seguintes alterações de API e comportamento têm o potencial de quebrar os aplicativos existentes ao atualizá-los para 3.0.0.
As alterações que esperamos que afetem apenas os provedores de banco de dados estão documentadas em [alterações de provedor](xref:core/providers/provider-log).

## <a name="summary"></a>Resumo

| **Mudança de ruptura**                                                                                               | **Impacto** |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [Consultas LINQ não são mais avaliadas no cliente](#linq-queries-are-no-longer-evaluated-on-the-client)         | Alta       |
| [O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0](#netstandard21) | Alta      |
| [A ferramenta de linha de comando do EF Core, dotnet ef, não faz mais parte do SDK do .NET Core](#dotnet-ef) | Alta      |
| [DetectChanges respeita os valores de chave gerados pelo repositório](#dc) | Alta      |
| [FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados](#fromsql) | Alta      |
| [Tipos de consulta são consolidados com tipos de entidade](#qt) | Alta      |
| [O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core](#no-longer) | Médio      |
| [Agora, as exclusões em cascata acontecem imediatamente por padrão](#cascade) | Médio      |
| [Carregamento ávido de entidades relacionadas agora acontece em uma única consulta](#eager-loading-single-query) | Médio      |
| [DeleteBehavior.Restrict tem uma semântica de limpeza](#deletebehavior) | Médio      |
| [A API de configuração para relações de tipo de propriedade mudou](#config) | Médio      |
| [Cada propriedade usa geração de chave de inteiro em memória independente](#each) | Médio      |
| [As consultas sem acompanhamento não executam mais a resolução de identidade](#notrackingresolution) | Médio      |
| [Alterações na API de metadados](#metadata-api-changes) | Médio      |
| [Alterações na API de metadados específicos do provedor](#provider) | Médio      |
| [UseRowNumberForPaging foi removido](#urn) | Médio      |
| [O método FromSql quando usado com o procedimento armazenado não pode ser composto](#fromsqlsproc) | Médio      |
| [Os métodos FromSql só podem ser especificados em raízes de consulta](#fromsql) | Baixo      |
| [~~A execução de consulta é registrada no nível da Depuração~~ Revertida](#qe) | Baixo      |
| [Valores de chave temporários não estão definidos em instâncias de entidade](#tkv) | Baixo      |
| [As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais](#de) | Baixo      |
| [Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade](#aes) | Baixo      |
| [Entidades de propriedade não podem ser consultadas sem que o proprietário use uma consulta de rastreamento](#owned-query) | Baixo      |
| [Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados](#ip) | Baixo      |
| [A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança](#fkp) | Baixo      |
| [Agora, a conexão de banco de dados será fechada se não for mais usada antes da conclusão do TransactionScope](#dbc) | Baixo      |
| [Os campos de suporte são usados por padrão](#backing-fields-are-used-by-default) | Baixo      |
| [Gerar se vários campos de suporte compatíveis são encontrados](#throw-if-multiple-compatible-backing-fields-are-found) | Baixo      |
| [Os nomes de propriedade somente de campo devem corresponder ao nome de campo](#field-only-property-names-should-match-the-field-name) | Baixo      |
| [AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache](#adddbc) | Baixo      |
| [AddEntityFramework* adiciona IMemoryCache com um limite de tamanho](#addentityframework-adds-imemorycache-with-a-size-limit) | Baixo      |
| [DbContext.Entry agora executa uma DetectChanges local](#dbe) | Baixo      |
| [As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão](#string-and-byte-array-keys-are-not-client-generated-by-default) | Baixo      |
| [ILoggerFactory agora é um serviço com escopo](#ilf) | Baixo      |
| [Os proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | Baixo      |
| [Agora, a criação excessiva de provedores de serviço internos é um erro por padrão](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | Baixo      |
| [Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres](#nbh) | Baixo      |
| [O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask](#rtnt) | Baixo      |
| [A anotação Relational:TypeMapping agora é apenas TypeMapping](#rtt) | Baixo      |
| [ToTable em um tipo derivado gera uma exceção](#totable-on-a-derived-type-throws-an-exception) | Baixo      |
| [O EF Core não envia mais pragma para imposição do FK SQLite](#pragma) | Baixo      |
| [Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3](#sqlite3) | Baixo      |
| [Os valores de Guid agora são armazenados como TEXTO no SQLite](#guid) | Baixo      |
| [Os valores Char agora são armazenados como TEXTO no SQLite](#char) | Baixo      |
| [As IDs de migração agora são geradas usando o calendário da cultura invariável](#migid) | Baixo      |
| [As informações de extensão/metadados foram removidas do IDbContextOptionsExtension](#xinfo) | Baixo      |
| [LogQueryPossibleExceptionWithAggregateOperator foi renomeado](#lqpe) | Baixo      |
| [Esclarecer a API para nomes da restrição de chave estrangeira](#clarify) | Baixo      |
| [IRelationalDatabaseCreator.HasTables/HasTablesAsync foram tornados públicos](#irdc2) | Baixo      |
| [Microsoft.EntityFrameworkCore.Design agora é um pacote de DevelopmentDependency](#dip) | Baixo      |
| [SQLitePCL.raw atualizado para a versão 2.0.0](#SQLitePCL) | Baixo      |
| [NetTopologySuite atualizado para a versão 2.0.0](#NetTopologySuite) | Baixo      |
| [Microsoft.Data.SqlClient é usado em vez de System.Data.SqlClient](#SqlClient) | Baixo      |
| [Várias relações ambíguas de autorreferência devem ser configuradas](#mersa) | Baixo      |
| [DbFunction.Schema sendo uma seqüência de configurações nulas ou vazias configura-o para estar no esquema padrão do modelo](#udf-empty-string) | Baixo      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Consultas LINQ não são mais avaliadas no cliente

[#14935 de problemas de rastreamento](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
Também veja[problema #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

**Comportamento antigo**

Antes da 3.0, quando o EF Core não podia converter uma expressão que fazia parte de uma consulta para SQL ou parâmetro, ela automaticamente avaliava a expressão no cliente.
Por padrão, a avaliação do cliente de expressões potencialmente dispendiosas apenas disparava um aviso.

**Novo comportamento**

A partir da 3.0, o EF Core só permite expressões na projeção de nível superior (a última chamada `Select()` na consulta) a ser avaliada no cliente.
Quando expressões em qualquer outra parte da consulta não podem ser convertidas em SQL ou parâmetro, uma exceção é lançada.

**Porque**

A avaliação automática de consultas do cliente permite que várias consultas sejam executadas, mesmo que partes importantes delas não possam ser convertidas.
Esse comportamento pode resultar em comportamento inesperado e potencialmente perigoso que pode se tornar evidente apenas em produção.
Por exemplo, uma condição em uma chamada `Where()` que não pode ser convertida pode fazer todas as linhas da tabela serem transferidas do servidor de banco de dados e o filtro ser aplicado no cliente.
Essa situação pode facilmente passar despercebidas se a tabela contém apenas algumas linhas no desenvolvimento, mas ter consequências sérias quando o aplicativo passa para a produção, em que a tabela pode conter milhões de linhas.
Avisos de avaliação do cliente também se mostraram muito fáceis de ignorar durante o desenvolvimento.

Além disso, a avaliação automática do cliente pode levar a problemas em que melhorar a conversão da consulta para expressões específicas causa alterações significativas não intencionais entre versões.

**Atenuações**

Se não for possível converter totalmente uma consulta, reescreva a consulta em uma forma que possa ser convertida ou use `AsEnumerable()`, `ToList()` ou semelhante para explicitamente trazer dados de volta para o cliente em um local em que possam ser processados usando o LINQ to Objects.

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a>O EF Core 3.0 tem como destino o .NET Standard 2.1 em vez do .NET Standard 2.0

[Problema de acompanhamento nº 15498](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

> [!IMPORTANT] 
> O EF Core 3.1 mira o .NET Standard 2.0 novamente. Isso traz de volta o suporte para .NET Framework.

**Comportamento antigo**

Antes do 3.0, o EF Core tinha como destino o .NET Standard 2.0 e era executado em todas as plataformas que dão suporte a esse padrão, incluindo o .NET Framework.

**Novo comportamento**

No 3.0 em diante, o EF Core tem como destino o .NET Standard 2.1 e será executado em todas as plataformas que dão suporte a esse padrão. Isso não inclui o .NET Framework.

**Porque**

Isso faz parte de uma decisão estratégica nas tecnologias .NET para concentrar a energia no .NET Core e em outras plataformas .NET modernas, como o Xamarin.

**Atenuações**

Use o EF Core 3.1.

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>O Entity Framework Core não faz mais parte da estrutura compartilhada do ASP.NET Core

[Anúncios de acompanhamento de problema nº 325](https://github.com/aspnet/Announcements/issues/325)

**Comportamento antigo**

Antes do ASP.NET Core 3.0, quando você adicionava uma referência de pacote a `Microsoft.AspNetCore.App` ou `Microsoft.AspNetCore.All`, ela incluiria o EF Core e alguns provedores de dados do EF Core, como o provedor do SQL Server.

**Novo comportamento**

A partir da 3.0, a estrutura compartilhada do ASP.NET Core não inclui o EF Core nem provedores de dados do EF Core.

**Porque**

Antes dessa alteração, obter o EF Core exigia diferentes etapas, dependendo de se o aplicativo tem como destino o ASP.NET Core e o SQL Server ou não. Além disso, atualizar o ASP.NET Core forçou a atualização do EF Core e do provedor do SQL Server, o que nem sempre é desejável.

Com essa alteração, a experiência de obter o EF Core é a mesmo em todos os provedores, implementações de .NET com suporte e tipos de aplicativos.
Os desenvolvedores agora também podem controlar exatamente quando o EF Core e os provedores de dados do EF Core são atualizados.

**Atenuações**

Para usar o EF Core em um aplicativo ASP.NET Core 3.0 ou qualquer outro aplicativo com suporte, adicione explicitamente uma referência de pacote ao provedor de banco de dados do EF Core que seu aplicativo usará.

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a>A ferramenta de linha de comando do EF Core, dotnet ef, não é parte do SDK do .NET Core

[Acompanhamento de problema n. 14016](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

**Comportamento antigo**

Antes da 3.0, a ferramenta `dotnet ef` foi incluída no SDK do .NET Core e ficou prontamente disponível para uso na linha de comando de qualquer projeto sem a necessidade de etapas adicionais. 

**Novo comportamento**

Da 3.0 em diante, o SDK do .NET não inclui a ferramenta `dotnet ef`, portanto, antes que você possa usá-la, você precisa instalá-la explicitamente como uma ferramenta local ou global. 

**Porque**

Essa alteração permite distribuir e atualizar `dotnet ef` como uma ferramenta de CLI do .NET regular no NuGet, consistente com o fato de que o EF Core 3.0 também é sempre distribuído como um pacote do NuGet.

**Atenuações**

Para conseguir gerenciar migrações ou scaffold no `DbContext`, instale `dotnet-ef` como uma ferramenta global:

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

Você também pode obtê-lo como uma ferramenta local quando você restaura as dependências de um projeto que o declara como uma dependência de ferramentas usando um [arquivo de manifesto de ferramenta](https://github.com/dotnet/cli/issues/10288).

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a>FromSql, ExecuteSql e ExecuteSqlAsync foram renomeados

[Acompanhamento de questões nº 10996](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

**Comportamento antigo**

Em versões do EF Core anteriores à 3.0, esses nomes de método eram sobrecarregados para trabalhar com uma cadeia de caracteres normal ou uma cadeia de caracteres que deveria ser interpolada em SQL e parâmetros.

**Novo comportamento**

Da versão 3.0 do EF Core em diante, use `FromSqlRaw`, `ExecuteSqlRaw`, e `ExecuteSqlRawAsync` para criar uma consulta parametrizada em que os parâmetros são passados separadamente da cadeia de consulta.
Por exemplo:

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` para criar uma consulta parametrizada em que os parâmetros são passados como parte de uma cadeia de consulta interpolada.
Por exemplo:

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

Observe que ambas as consultas acima produzirão o mesmo SQL parametrizado com os mesmos parâmetros SQL.

**Porque**

Sobrecargas de método como essa tornam muito fácil chamar acidentalmente o método de cadeia de caracteres bruta quando a intenção era para chamar o método de cadeia de caracteres interpolada e vice-versa.
Isso pode resultar em consultas não parametrizadas, quando deveriam ter sido.

**Atenuações**

Mude para usar os novos nomes de método.

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a>O método FromSql quando usado com o procedimento armazenado não pode ser composto

[#15392 de problemas de rastreamento](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

**Comportamento antigo**

Antes do EF Core 3.0, o método FromSql tentou detectar se o SQL passado pode ser composto. Ele fez avaliação do cliente quando o SQL não era compostável como um procedimento armazenado. A consulta a seguir funcionou executando o procedimento armazenado no servidor e fazendo FirstOrDefault no lado do cliente.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

**Novo comportamento**

Começando com o EF Core 3.0, o EF Core não tentará analisar o SQL. Então, se você estiver compondo depois de FromSqlRaw/FromSqlInterpolated, então o EF Core irá compor o SQL causando sub consulta. Então, se você estiver usando um procedimento armazenado com composição, então você terá uma exceção para sintaxe SQL inválida.

**Porque**

O EF Core 3.0 não suporta avaliação automática do cliente, pois era propenso a erros, como explicado [aqui](#linq-queries-are-no-longer-evaluated-on-the-client).

**Mitigação**

Se você estiver usando um procedimento armazenado no FromSqlRaw/FromSqlInterpolated, você sabe que ele não pode ser composto, para que você possa adicionar __AsEnumerable/AsAsyncEnumerable__ logo após a chamada do método FromSql para evitar qualquer composição no lado do servidor.

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a>Os métodos FromSql só podem ser especificados em raízes de consulta

[Acompanhamento de problema nº 15704](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

**Comportamento antigo**

Antes do EF Core 3.0, o método `FromSql` podia ser especificado em qualquer lugar na consulta.

**Novo comportamento**

A partir do EF Core 3.0, os novos métodos `FromSqlRaw` e `FromSqlInterpolated` (que substituíram o `FromSql`) só podem ser especificados em raízes de consultas, ou seja, diretamente no `DbSet<>`. A tentativa de especificá-los em qualquer outro lugar resultará em um erro de compilação.

**Porque**

Especificar `FromSql` em qualquer lugar em vez de `DbSet` não tinha nenhum significado adicional ou valor agregado e poderia causar ambiguidade em determinados cenários.

**Atenuações**

As invocações de `FromSql` devem ser movidas para estarem diretamente no `DbSet` ao qual se aplicam.

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a>As consultas sem acompanhamento não executam mais a resolução de identidade

[Problema de acompanhamento nº 13518](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

**Comportamento antigo**

Antes do EF Core 3.0, a mesma instância de entidade era usada para cada ocorrência de uma entidade com uma ID e um tipo especificados. Isso corresponde ao comportamento das consultas de acompanhamento. Por exemplo, esta consulta:

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
retornará a mesma instância de `Category` para cada `Product` associado à categoria especificada.

**Novo comportamento**

No EF Core 3.0 em diante, diferentes instâncias de entidade serão criadas quando uma entidade com uma ID e um tipo especificados for encontrada em locais diferentes no grafo retornado. Por exemplo, a consulta acima agora retornará uma nova instância de `Category` para cada `Product`, mesmo quando dois produtos estiverem associados à mesma categoria.

**Porque**

A resolução de identidade (ou seja, determinar que uma entidade tem o mesmo tipo e a mesma ID de uma entidade encontrada anteriormente) adiciona sobrecarga extra de desempenho e memória. Isso geralmente executa um contador para descobrir o motivo pelo qual as consultas sem acompanhamento são usadas em primeiro lugar. Além disso, embora a resolução de identidade às vezes possa ser útil, ela não é necessária se as entidades são serializadas e enviadas a um cliente, o que é comum para consultas sem acompanhamento.

**Atenuações**

Use uma consulta de acompanhamento se a resolução de identidade for necessária.

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a>~~A execução de consulta é registrada no nível da Depuração~~ Revertida

[Acompanhamento de problema nº 14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Revertemos essa alteração porque a nova configuração do EF Core 3.0 permite que o nível de log de qualquer evento seja especificado pelo aplicativo. Por exemplo, para alternar o registro em log do SQL para `Debug`, configure explicitamente o nível em `OnConfiguring` ou `AddDbContext`:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>Valores de chave temporários não estão definidos em instâncias de entidade

[Acompanhamento de problema nº 12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

**Comportamento antigo**

Antes do EF Core 3.0, valores temporários eram atribuídos a todas as propriedades de chave que mais tarde teriam um valor real gerado pelo banco de dados.
Normalmente, esses valores temporários eram grandes números negativos.

**Novo comportamento**

A partir da 3.0, o EF Core armazena o valor de chave temporária como parte das informações de acompanhamento da entidade e deixa a propriedade de chave em si inalterada.

**Porque**

Essa alteração foi feita para impedir que valores de chave temporários erroneamente se tornem permanente quando uma entidade anteriormente controlada por alguma instância `DbContext` for movida para outra instância `DbContext`. 

**Atenuações**

Aplicativos que atribuem valores de chave primária em chaves estrangeiras para formar associações entre entidades poderão depender do comportamento antigo se as chaves primárias forem geradas pelo repositório e pertencerem a entidades no estado `Added`.
Isso pode ser evitado:
* Não usando chaves geradas pelo repositório.
* Definindo propriedades de navegação para formar relações, em vez de definir valores de chave estrangeira.
* Obter os valores de chave temporários reais de informações de acompanhamento de entidade.
Por exemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` retornará o valor temporário, embora `blog.Id` em si não tenha sido definido.

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges respeita os valores de chave gerados pelo repositório

[Acompanhamento de problema nº 14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

**Comportamento antigo**

Antes do EF Core 3.0, uma entidade não controlada encontrada pelo `DetectChanges` seria controlada no estado `Added` e inserida como uma nova linha quando `SaveChanges` fosse chamado.

**Novo comportamento**

A partir do EF Core 3.0, se uma entidade estiver usando valores de chave gerados e algum valor de chave estiver definido, a entidade será rastreada no estado `Modified`.
Isso significa que se presume que uma linha para a entidade exista e ela será atualizada quando `SaveChanges` for chamado.
Se o valor da chave não for definido ou se o tipo de entidade não estiver usando as chaves geradas, a nova entidade ainda será rastreada como `Added` como nas versões anteriores.

**Porque**

Essa alteração foi feita para tornar mais fácil e consistente trabalhar com grafos de entidade desconectados ao usar chaves geradas pelo repositório.

**Atenuações**

Essa alteração poderá interromper um aplicativo se um tipo de entidade estiver configurado para usar chaves geradas, mas os valores de chave estiverem explicitamente definidos para novas instâncias.
A correção é configurar explicitamente as propriedades de chave para não usarem valores gerados.
Por exemplo, com a API fluente:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Ou com anotações de dados:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a>Exclusões em cascata acontecerão imediatamente por padrão

[Acompanhamento de problema nº 10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

**Comportamento antigo**

Antes da 3.0, as ações em cascata aplicadas do EF Core (excluindo entidades dependentes quando uma entidade de segurança obrigatória é excluída ou quando a relação com uma entidade de segurança obrigatória é interrompida) não aconteciam até que SaveChanges fosse chamado.

**Novo comportamento**

A partir da 3.0, o EF Core aplica ações em cascata assim que a condição de disparo é detectada.
Por exemplo, chamar `context.Remove()` para excluir uma entidade principal resultará em todos dependentes obrigatórios relacionados rastreados também serem definidos como `Deleted` imediatamente.

**Porque**

Essa alteração foi feita para melhorar a experiência para associação de dados e cenários em que é importante entender quais entidades serão excluídas de auditoria _antes de_ `SaveChanges` ser chamado.

**Atenuações**

O comportamento anterior pode ser restaurado por meio das configurações em `context.ChangeTracker`.
Por exemplo:

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a>Carregamento ávido de entidades relacionadas agora acontece em uma única consulta

[Problema de rastreamento #18022](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

**Comportamento antigo**

Antes do 3.0, o carregamento `Include` avidamente de navegação de cobrança via operadores fez com que várias consultas fossem geradas no banco de dados relacional, uma para cada tipo de entidade relacionada.

**Novo comportamento**

A partir do 3.0, o EF Core gera uma única consulta com JOINs em bancos de dados relacionais.

**Porque**

A emissão de várias consultas para implementar uma única consulta LINQ causou inúmeros problemas, incluindo desempenho negativo, pois várias rodadas de banco de dados eram necessárias e problemas de coerência de dados, pois cada consulta poderia observar um estado diferente do banco de dados.

**Atenuações**

Embora tecnicamente esta não seja uma mudança de ruptura, ela pode ter um efeito `Include` considerável no desempenho do aplicativo quando uma única consulta contém um grande número de operadores em navegaçãos de coleção. [Consulte este comentário](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) para obter mais informações e para reescrever consultas de forma mais eficiente.

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a>DeleteBehavior.Restrict tem uma semântica de limpeza

[Acompanhamento de questões nº 12661](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

**Comportamento antigo**

Antes do 3.0, `DeleteBehavior.Restrict` criava chaves estrangeiras no banco de dados com a semântica `Restrict`, mas também alterava a correção interna de maneira não óbvia.

**Novo comportamento**

No 3.0 em diante, `DeleteBehavior.Restrict` garante que as chaves estrangeiras são criadas com a semântica `Restrict` – ou seja, nenhuma cascata é gerada em violação de restrição – sem afetar também a correção interna do EF.

**Porque**

Essa alteração foi feita para melhorar a experiência do uso de `DeleteBehavior` de maneira intuitiva, sem efeitos colaterais inesperados.

**Atenuações**

O comportamento anterior pode ser restaurado usando `DeleteBehavior.ClientNoAction`.

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a>Tipos de consulta são consolidados com tipos de entidade

[Acompanhamento de problema nº 14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

**Comportamento antigo**

Antes do EF Core 3.0, [tipos de consulta](xref:core/modeling/keyless-entity-types) eram um meio de consultar dados que não definem uma chave primária de maneira estruturada.
Ou seja, um tipo de consulta era usado para mapear os tipos de entidade sem chaves (mais provavelmente de uma exibição, mas possivelmente de uma tabela), enquanto um tipo de entidade normal era usado quando uma chave estava disponível (mais provavelmente de uma tabela, mas possivelmente de um modo de exibição).

**Novo comportamento**

Um tipo de consulta agora se torna apenas um tipo de entidade sem uma chave primária.
Tipos de entidade sem chave têm a mesma funcionalidade que tipos de consulta nas versões anteriores.

**Porque**

Essa alteração foi feita para reduzir a confusão entre a finalidade dos tipos de consulta.
Especificamente, são tipos de entidade sem chave e inerentemente somente leitura devido a isso, mas não devem ser usados apenas porque um tipo de entidade precisa ser somente leitura.
Da mesma forma, geralmente são mapeados para modos de exibição, mas isso é só porque os modos de exibição geralmente não definem chaves.

**Atenuações**

As seguintes partes da API agora estão obsoletas:
* **`ModelBuilder.Query<>()`**- `ModelBuilder.Entity<>().HasNoKey()` Em vez disso, precisa ser chamado para marcar um tipo de entidade como não ter chaves.
Isso ainda não será configurado por convenção para evitar erros de configuração quando uma chave primária for esperada, mas não corresponder à convenção.
* **`DbQuery<>`**- `DbSet<>` Em vez disso, deve ser usado.
* **`DbContext.Query<>()`**- `DbContext.Set<>()` Em vez disso, deve ser usado.

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a>A API de configuração para relações de tipo de propriedade mudou

[Problema de rastreamento #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[problema de rastreamento #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[problema de rastreamento #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

**Comportamento antigo**

Antes do EF Core 3.0, a configuração da relação de propriedade era realizada diretamente após a chamada `OwnsOne` ou `OwnsMany`. 

**Novo comportamento**

A partir do EF Core 3.0, agora há uma API fluente para configurar uma propriedade de navegação para o proprietário usando `WithOwner()`.
Por exemplo:

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

A configuração relacionada à relação entre o proprietário e a propriedade agora deve ser encadeada após `WithOwner()` da mesma forma que como as outras relações são configuradas.
No entanto, a configuração para o tipo de propriedade em si ainda é encadeada após `OwnsOne()/OwnsMany()`.
Por exemplo:

```csharp
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

**Porque**

Essa alteração foi feita para criar uma separação mais clara entre configurar o tipo de propriedade em si e a _relação com_ o tipo de propriedade.
Isso, por sua vez, removerá a ambiguidade e a confusão quanto a métodos como `HasForeignKey`.

**Atenuações**

Altere a configuração de relações de tipo de propriedade para usar a nova superfície de API, conforme mostrado no exemplo acima.

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>As entidades dependentes compartilham a tabela com a entidade de segurança agora são opcionais

[Acompanhamento de questões nº 9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

**Comportamento antigo**

Considere o modelo a seguir:
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
Em versões do EF Core anteriores à 3.0, se `OrderDetails` fosse pertencente a `Order` ou explicitamente mapeado para a mesma tabela, uma instância `OrderDetails` seria sempre necessária ao adicionar um novo `Order`.


**Novo comportamento**

Da versão 3.0 em diante, o EF Core permite adicionar um `Order` sem um `OrderDetails` e mapeia todas as propriedades `OrderDetails`, exceto a chave primária para colunas que permitem valor nulo.
Ao realizar consultas, o EF Core definirá `OrderDetails` para `null` se qualquer uma de suas propriedades requeridas não tiver um valor ou se ele não tiver nenhuma propriedade necessária além da chave primária e todas as propriedades forem `null`.

**Atenuações**

Se seu modelo tem uma tabela de compartilhamento de dependentes com todas as colunas opcionais, mas a navegação apontando para ele não deve ser `null`, então o aplicativo deve ser modificado para lidar com casos quando a navegação for `null`. Se isso não for possível, uma propriedade necessária deverá ser adicionada ao tipo de entidade ou pelo menos uma propriedade deverá ter um valor não `null` atribuído a ela.

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a>Todas as entidades compartilhando uma tabela com uma coluna de token de simultaneidade precisam mapeá-lo para uma propriedade

[Acompanhamento de questões nº 14154](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

**Comportamento antigo**

Considere o modelo a seguir:
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
Em versões do EF Core anteriores à 3.0, se `OrderDetails` pertencer a `Order` ou for explicitamente mapeado para a mesma tabela, atualizar apenas `OrderDetails` não atualizará o valor `Version` no cliente e a próxima atualização falhará.


**Novo comportamento**

Da versão 3.0 em diante, o EF Core propaga o novo valor `Version` para `Order` se ele for proprietário de `OrderDetails`. Caso contrário, uma exceção é gerada durante a validação do modelo.

**Porque**

Essa alteração foi feita para evitar um valor de token de simultaneidade obsoleto quando apenas uma das entidades mapeadas para a mesma tabela é atualizada.

**Atenuações**

Todas as entidades que compartilham a tabela precisam incluir uma propriedade que é mapeada para a coluna do token de simultaneidade. É possível criar uma no estado de sombra:
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a>Entidades de propriedade não podem ser consultadas sem que o proprietário use uma consulta de rastreamento

[#18876 de problemas de rastreamento](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

**Comportamento antigo**

Antes do EF Core 3.0, as entidades de propriedade poderiam ser consultadas como qualquer outra navegação.

```csharp
context.People.Select(p => p.Address);
```

**Novo comportamento**

A partir do 3.0, o EF Core será jogado se uma consulta de rastreamento projetar uma entidade própria sem o proprietário.

**Porque**

Entidades próprias não podem ser manipuladas sem o proprietário, portanto, na grande maioria dos casos, consultando-as dessa forma é um erro.

**Atenuações**

Se a entidade própria deve ser rastreada para ser modificada de qualquer forma posteriormente, o proprietário deve ser incluído na consulta.

Caso contrário, `AsNoTracking()` adicione uma chamada:

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a>Agora, as propriedades herdadas de tipos não mapeados são mapeadas para uma única coluna para todos os tipos derivados

[Acompanhamento de questões nº 13998](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

**Comportamento antigo**

Considere o modelo a seguir:
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

Em versões do EF Core anteriores à 3.0, a propriedade `ShippingAddress` seria mapeada para separar as colunas para `BulkOrder` e `Order` por padrão.

**Novo comportamento**

Da versão 3.0 em diante, o EF Core cria apenas uma coluna para `ShippingAddress`.

**Porque**

O antigo comportamento era inesperado.

**Atenuações**

A propriedade ainda pode ser mapeada explicitamente para uma coluna separada sobre os tipos derivados:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>A convenção de propriedade de chave estrangeira não coincide mais com mesmo nome que a propriedade de entidade de segurança

[Acompanhamento de problema nº 13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

**Comportamento antigo**

Considere o modelo a seguir:
```csharp
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

**Novo comportamento**

Da versão 3.0 em diante, o EF Core não tentará usar propriedades para chaves estrangeiras por convenção se elas tiverem o mesmo nome que a propriedade de entidade de segurança.
Nome do tipo de entidade de segurança concatenado com o nome da propriedade de entidade de segurança e nome de navegação concatenado com padrões de nome de propriedade de entidade de segurança ainda são correspondidos.
Por exemplo:

```csharp
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

```csharp
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

**Porque**

Essa alteração foi feita para evitar definir erroneamente uma propriedade de chave primária no tipo de propriedade.

**Atenuações**

Caso a propriedade se destine a ser a chave estrangeira e, portanto, parte da chave primária, configure-a explicitamente como tal.

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a>A conexão de banco de dados agora será fechada se não for mais usada antes da conclusão do TransactionScope

[Acompanhamento de questões nº 14218](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

**Comportamento antigo**

Em versões do EF Core anteriores à 3.0, se o contexto abrir a conexão dentro de um `TransactionScope`, a conexão permanecerá aberta enquanto o `TransactionScope` atual estiver ativo.

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

**Novo comportamento**

Da versão 3.0 em diante, o EF Core fecha a conexão assim que termina de usá-la.

**Porque**

Essa alteração permite para usar vários contextos no mesmo `TransactionScope`. O novo comportamento também corresponde ao EF6.

**Atenuações**

Se a conexão precisar permanecer aberta, uma chamada explícita para `OpenConnection()` garantirá que o EF Core não a feche prematuramente:

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Cada propriedade usa geração de chave de inteiro em memória independente

[Acompanhamento de problema nº 6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

**Comportamento antigo**

Antes do EF Core 3.0, um gerador de valor compartilhado era usado para todas as propriedades de chave de inteiro em memória.

**Novo comportamento**

Cada propriedade de chave de inteiro a partir do EF Core 3.0 obtém seu próprio gerador de valor ao usar o banco de dados em memória.
Além disso, se o banco de dados for excluído, a geração de chave será redefinida para todas as tabelas.

**Porque**

Essa alteração foi feita para alinhar melhor a geração de chave em memória com a geração de chave do banco de dados real e para melhorar a capacidade de isolar testes uns dos outros ao usar o banco de dados em memória.

**Atenuações**

Isso pode interromper um aplicativo que depende da definição de valores específicos de chave em memória.
Considere, em vez disso, não depender de valores de chave específicos ou atualizar para coincidir com o novo comportamento.

### <a name="backing-fields-are-used-by-default"></a>Campos de suporte são usados por padrão

[Acompanhamento de problema nº 12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

**Comportamento antigo**

Antes da 3.0, mesmo que o campo de suporte para uma propriedade fosse conhecido, o EF Core ainda faria, por padrão, a leitura e a gravação do valor da propriedade usando os métodos getter e setter da propriedade.
A exceção a isso era a execução da consulta, em que o campo de suporte seria definido diretamente, se fosse conhecido.

**Novo comportamento**

Do EF Core 3.0 em diante, se o campo de suporte para uma propriedade é conhecido, o EF Core sempre lê e grava essa propriedade usando o campo de suporte.
Isso poderá causar uma interrupção de aplicativo se o aplicativo depender do comportamento adicional codificado para os métodos getter ou setter.

**Porque**

Essa alteração foi feita para impedir que o EF Core erroneamente acionasse a lógica de negócios por padrão, ao executar operações de banco de dados envolvendo as entidades.

**Atenuações**

O comportamento anterior a 3.0 pode ser restaurado pela configuração do modo de acesso da propriedade em `ModelBuilder`.
Por exemplo:

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Lançado se vários campos de suporte compatíveis forem encontrados

[Acompanhamento de problema nº 12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

**Comportamento antigo**

Antes do EF Core 3.0, se vários campos correspondessem às regras para localizar o campo de suporte de uma propriedade, então um campo seria escolhido com base em uma ordem de precedência.
Isso pode fazer o campo errado ser usado em casos ambíguos.

**Novo comportamento**

A partir do EF Core 3.0, se vários campos corresponderem à mesma propriedade, uma exceção será lançada.

**Porque**

Essa alteração foi feita para evitar usar silenciosamente um campo em detrimento de outro, quando apenas um puder ser correto.

**Atenuações**

As propriedades com campos de suporte ambíguos devem ter o campo a ser usado explicitamente especificado.
Por exemplo, usando a API fluente:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a>Os nomes de propriedade somente de campo devem corresponder ao nome de campo

**Comportamento antigo**

Antes do EF Core 3.0, uma propriedade poderia ser especificada por um valor de seqüência e se nenhuma propriedade com esse nome fosse encontrada no tipo .NET, o EF Core tentaria compará-lo a um campo usando regras de convenção.

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

**Novo comportamento**

No EF Core 3.0 em diante, uma propriedade somente de campo deve corresponder exatamente ao nome de campo.

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

**Porque**

Essa alteração foi feita para evitar o uso do mesmo campo para duas propriedades nomeadas de forma semelhante; também torna as regras de correspondência para propriedades somente de campo as mesmas propriedades mapeadas para propriedades CLR.

**Atenuações**

As propriedades somente de campo devem ter o mesmo nome do campo para o qual são mapeadas.
Em uma versão futura do EF Core após 3.0, planejamos reativar explicitamente a configuração de um nome de campo diferente do nome da propriedade (ver problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool não chama mais AddLogging e AddMemoryCache

[Acompanhamento de problema nº 14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

**Comportamento antigo**

Antes do EF Core `AddDbContext` 3.0, ligando ou `AddDbContextPool` também registraria serviços de registro e cache de memória com DI através de chamadas para [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Novo comportamento**

A partir do EF Core 3.0, `AddDbContext` e `AddDbContextPool` não registrarão mais esses serviços com a DI (injeção de dependência).

**Porque**

O EF Core 3.0 não requer que esses serviços estejam no contêiner de DI do aplicativo. No entanto, se `ILoggerFactory` estiver registrado no contêiner de DI do aplicativo, ele ainda será usado pelo EF Core.

**Atenuações**

Se seu aplicativo precisar desses serviços, registre-os explicitamente com o contêiner de DI usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) ou [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a>AddEntityFramework* adiciona IMemoryCache com um limite de tamanho

[problema de rastreamento #12905](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

**Comportamento antigo**

Antes do EF Core `AddEntityFramework*` 3.0, os métodos de chamada também registrariam serviços de cache de memória com DI sem um limite de tamanho.

**Novo comportamento**

A partir do EF `AddEntityFramework*` Core 3.0, registrará um serviço IMemoryCache com um limite de tamanho. Se outros serviços adicionados depois dependerem do IMemoryCache, eles podem atingir rapidamente o limite padrão, causando exceções ou desempenho degradado.

**Porque**

O uso do IMemoryCache sem limite pode resultar em uso de memória descontrolado se houver um bug na lógica de cache de consulta ou as consultas forem geradas dinamicamente. Ter um limite padrão atenua um potencial ataque do DoS.

**Atenuações**

Na maioria `AddEntityFramework*` dos casos, `AddDbContext` `AddDbContextPool` ligar não é necessário se ou é chamado também. Portanto, a melhor atenuação `AddEntityFramework*` é remover a chamada.

Se o aplicativo precisar desses serviços, registre uma implementação do IMemoryCache explicitamente com o contêiner DI de antemão usando [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry agora executa uma DetectChanges local

[Acompanhamento de problema nº 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

**Comportamento antigo**

Antes do EF Core 3.0, chamar `DbContext.Entry` faria alterações serem detectadas para todas as entidades rastreadas.
Isso garantia que o estado exposto em `EntityEntry` fosse atualizado.

**Novo comportamento**

A partir do EF Core 3.0, chamar `DbContext.Entry` agora tentará apenas detectar alterações na entidade determinada e quaisquer entidades principais rastreadas relacionadas a ela.
Isso significa que as alterações em outro lugar talvez não tenham sido detectadas chamando esse método, o que podia ter implicações no estado do aplicativo.

Observe que, se `ChangeTracker.AutoDetectChangesEnabled` for definido como `false`, até mesmo essa detecção de alteração local será desabilitada.

Outros métodos que causam detecção de alteração – por exemplo `ChangeTracker.Entries` e `SaveChanges` – ainda causam uma `DetectChanges` completa de todas as entidades de controladas.

**Porque**

Essa alteração foi feita para melhorar o desempenho padrão de usar `context.Entry`.

**Atenuações**

Chame `ChangeTracker.DetectChanges()` explicitamente antes de chamar `Entry` para garantir o comportamento pré-3.0.

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>As chaves de matriz de byte e cadeia de caracteres não são geradas pelo cliente por padrão

[Acompanhamento de problema nº 14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

**Comportamento antigo**

Antes do EF Core 3.0, as propriedades `string` e `byte[]` podiam ser usadas sem configurar explicitamente um valor não nulo.
Nesse caso, o valor da chave será gerado no cliente como um GUID, serializado em bytes para `byte[]`.

**Novo comportamento**

A partir do EF Core 3.0, uma exceção será gerada indicando que nenhum valor de chave foi definido.

**Porque**

Essa alteração foi feita porque os valores `string`/`byte[]` gerados pelo cliente costumam não ser úteis, e o comportamento padrão tornava difícil refletir sobre os valores de chave gerados de uma maneira comum.

**Atenuações**

O comportamento pré-3.0 pode ser obtido especificando explicitamente que as propriedades chave devem usar valores gerados caso nenhum outro valor não nulo seja definido.
Por exemplo, com a API fluente:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Ou com anotações de dados:

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory agora é um serviço com escopo

[Acompanhamento de problema nº 14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

**Comportamento antigo**

Antes do EF Core 3.0, `ILoggerFactory` era registrado como um serviço singleton.

**Novo comportamento**

A partir do EF Core 3.0, `ILoggerFactory` agora está registrado no escopo.

**Porque**

Essa alteração foi feita para permitir a associação de um agente com uma instância `DbContext`, que habilita outra funcionalidade e remove alguns casos de comportamento patológico como uma explosão de provedores de serviço interno.

**Atenuações**

Essa alteração não deverá afetar o código do aplicativo, a menos que esteja registrando e usando os serviços personalizados no provedor de serviço interno do EF Core.
Isso não é comum.
Nesses casos, a maioria das coisas ainda funcionará, mas qualquer serviço singleton que dependa de `ILoggerFactory` precisará ser alterado para obter o `ILoggerFactory` de maneira diferente.

Se você enfrentar situações como essa, registre um problema no [Rastreador de problemas do GitHub do EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para informar como você está usando `ILoggerFactory` para que possamos entender melhor como não interromper isso novamente no futuro.

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>Proxies de carregamento lento não presumem mais que as propriedades de navegação estejam totalmente carregadas

[Acompanhamento de problema nº 12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

**Comportamento antigo**

Antes do EF Core 3.0, quando `DbContext` era descartado, não havia como saber se uma propriedade de navegação fornecida em uma entidade obtida daquele contexto estava totalmente carregada ou não.
Os proxies, em vez disso, presumirão que uma navegação de referência foi carregada caso ela tenha um valor não nulo e que uma navegação de coleção foi carregada caso ela não esteja vazia.
Nesses casos, a tentativa de realizar um carregamento lento seria inoperante.

**Novo comportamento**

A partir do EF Core 3.0, proxies controlam de se uma propriedade de navegação é carregada ou não.
Isso significa tentar acessar uma propriedade de navegação carregada após o contexto ter sido descartado sempre será não operacional, mesmo quando a navegação carregada for nula ou vazia.
Por outro lado, a tentativa de acessar uma propriedade de navegação que não está carregada gerará uma exceção se o contexto for descartado, mesmo que a propriedade de navegação seja uma coleção não vazia.
Se essa situação ocorre, isso significa que o código do aplicativo está tentando usar o carregamento lento em uma hora inválida e o aplicativo deve ser alterado para não fazer isso.

**Porque**

Essa alteração foi feita para tornar o comportamento consistente e correto ao tentar realizar um carregamento lento em uma instância `DbContext` descartada.

**Atenuações**

Atualize o código do aplicativo para não tentar carregamento lento com um contexto descartado ou configure-o para ser inoperante, conforme descrito na mensagem de exceção.

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>Criação excessiva de provedores de serviço internos agora é um erro por padrão

[Acompanhamento de problema nº 10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

**Comportamento antigo**

Antes do EF Core 3.0, um aviso era registrado para um aplicativo, criando um número patológico de provedores de serviço internos.

**Novo comportamento**

A partir do EF Core 3.0, esse aviso agora é considerado e um erro e uma exceção são lançados. 

**Porque**

Essa alteração era feita para obter melhores códigos de aplicativo expondo esse caso patológico mais explicitamente.

**Atenuações**

A causa mais apropriada de ação ao encontrar esse erro é entender a causa raiz e parar de criar tantos provedores de serviço internos.
No entanto, o erro pode ser convertido de volta em um aviso (ou ignorado) por meio da configuração no `DbContextOptionsBuilder`.
Por exemplo:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Novo comportamento para HasOne/HasMany chamado com uma única cadeia de caracteres

[Acompanhamento de problema nº 9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

**Comportamento antigo**

Antes do EF Core 3.0, o código que chamava `HasOne` ou `HasMany` com uma única cadeia de caracteres era interpretado de forma confusa.
Por exemplo:
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

O código parece estar relacionando `Samurai` com algum outro tipo de entidade usando a propriedade de navegação `Entrance`, que pode ser privada.

Na realidade, esse código tenta criar um relacionamento com algum tipo de entidade chamado `Entrance` sem propriedade de navegação.

**Novo comportamento**

A partir do EF Core 3.0, o código acima agora faz o que deveria ter feito antes.

**Porque**

O comportamento antigo era muito confuso, especialmente ao ler o código de configuração e ao procurar erros.

**Atenuações**

Isso interromperá somente os aplicativos que estão explicitamente configurando relacionamentos usando cadeias de caracteres para nomes de tipos e não estão especificando a propriedade de navegação explicitamente.
Isso não é comum.
O comportamento anterior pode ser obtido explicitamente passando `null` para o nome da propriedade de navegação.
Por exemplo:

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a>O tipo de retorno para vários métodos assíncronos foi alterado de Task para ValueTask

[Acompanhamento de questões nº 15184](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

**Comportamento antigo**

Os seguintes métodos assíncronos anteriormente retornavam um `Task<T>`:

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` (e classes derivadas)

**Novo comportamento**

Os métodos mencionados anteriormente agora retornam um `ValueTask<T>` sobre o mesmo `T` que antes.

**Porque**

Essa alteração reduz o número de alocações de heap incorridas ao invocar esses métodos, melhorando o desempenho geral.

**Atenuações**

Aplicativos que estão apenas aguardando as APIs acima só precisam ser recompilados. Nenhuma alteração de fonte é necessária.
Um uso mais complexo (por exemplo, passando o `Task` retornado a `Task.WhenAny()`) normalmente exigem que o `ValueTask<T>` retornado seja convertido em um `Task<T>` chamando `AsTask()` nele.
Observe que isso anula a redução de alocação feita por essa alteração.

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>A anotação Relational:TypeMapping agora é apenas TypeMapping

[Acompanhamento de problema nº 9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

**Comportamento antigo**

O nome da anotação para anotações de mapeamento de tipo era "Relational:TypeMapping".

**Novo comportamento**

O nome de anotação para anotações de mapeamento de tipo agora é "TypeMapping".

**Porque**

Mapeamentos de tipo agora são usados mais do que apenas para provedores de banco de dados relacional.

**Atenuações**

Isso interromperá apenas aplicativos que acessam o mapeamento de tipo diretamente como uma anotação, o que não é comum.
A ação mais apropriada para a correção é usar a superfície de API para acessar mapeamentos de tipo, em vez de usar a anotação diretamente.

### <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable em um tipo derivado gera uma exceção 

[Acompanhamento de problema nº 11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

**Comportamento antigo**

Antes do EF Core 3.0, `ToTable()` chamado em um tipo derivado era ignorado, uma vez que a única estratégia de mapeamento de herança era TPH, em que isso não é válido. 

**Novo comportamento**

A partir do EF Core 3.0 e em preparação para a adição de suporte a TPT e TPC em uma versão posterior, `ToTable()` chamado em um tipo derivado agora gerará uma exceção para evitar uma alteração de mapeamento inesperada no futuro.

**Porque**

Atualmente, não é válido mapear um tipo derivado para uma tabela diferente.
Essa alteração evita interrupção no futuro, quando se torna algo válido a se fazer.

**Atenuações**

Remova quaisquer tentativas de mapear tipos derivados para outras tabelas.

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex substituído por HasIndex 

[Acompanhamento de problema nº 12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

**Comportamento antigo**

Antes do EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` fornecia uma forma de configurar as colunas usadas com `INCLUDE`.

**Novo comportamento**

A partir do EF Core 3.0, agora há suporte para usar `Include` em um índice no nível relacional.
Use `HasIndex().ForSqlServerInclude()`.

**Porque**

Essa alteração foi feita para consolidar a API para índices com `Include` em um local para todos os provedores de banco de dados.

**Atenuações**

Use a nova API, conforme mostrado acima.

### <a name="metadata-api-changes"></a>Alterações na API de metadados

[Acompanhamento de questões nº 214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Novo comportamento**

As seguintes propriedades foram convertidas em métodos de extensão:

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

**Porque**

Essa alteração simplifica a implementação das interfaces mencionados anteriormente.

**Atenuações**

Use os novos métodos de extensão.

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a>Alterações na API de metadados específicos do provedor

[Acompanhamento de questões nº 214](https://github.com/aspnet/EntityFrameworkCore/issues/214)

**Novo comportamento**

Os métodos de extensão específicos do provedor serão nivelados:

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

**Porque**

Essa alteração simplifica a implementação dos métodos de extensão mencionados anteriormente.

**Atenuações**

Use os novos métodos de extensão.

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>O EF Core não envia mais pragma para imposição do FK SQLite

[Acompanhamento de problema nº 12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

**Comportamento antigo**

Antes do EF Core 3.0, o EF Core enviava `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite era aberta.

**Novo comportamento**

A partir do EF Core 3.0, o EF Core não envia mais `PRAGMA foreign_keys = 1` quando uma conexão para o SQLite é aberta.

**Porque**

Essa alteração foi feita porque o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` por padrão, o que, por sua vez, significa que a imposição de FK é ativada por padrão e não precisa ser habilitada explicitamente sempre que uma conexão é aberta.

**Atenuações**

Chaves estrangeiras são habilitadas por padrão no SQLitePCLRaw.bundle_e_sqlite3, que é usado por padrão para o EF Core.
Para outros casos, chaves estrangeiras podem ser habilitadas especificando `Foreign Keys=True` em sua cadeia de conexão.

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite agora depende de SQLitePCLRaw.bundle_e_sqlite3

**Comportamento antigo**

Antes do EF Core 3.0, o EF Core era usado `SQLitePCLRaw.bundle_green`.

**Novo comportamento**

A partir do EF Core 3.0, o EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.

**Porque**

Essa alteração foi feita de modo que a versão do SQLite usada no iOS fosse consistente com outras plataformas.

**Atenuações**

Para usar a versão do SQLite nativa no iOS, configure `Microsoft.Data.Sqlite` para usar um pacote `SQLitePCLRaw` diferente.

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a>Os valores de Guid agora são armazenados como TEXTO no SQLite

[Acompanhamento de problema nº 15078](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

**Comportamento antigo**

Os valores de Guid foram previamente armazenados como valores de BLOB no SQLite.

**Novo comportamento**

Agora os valores de Guid são armazenados como TEXTO.

**Porque**

O formato binário de Guids não é padronizado. O armazenamento dos valores como TEXTO permite que o banco de dados seja mais compatível com outras tecnologias.

**Atenuações**

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

No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

O Microsoft.Data.Sqlite ainda pode ler valores de Guid das colunas BLOB e TEXTO; no entanto, como o formato padrão para parâmetros e constantes foi alterado, é provável que você precise tomar medidas para a maioria dos cenários que envolvem Guids.

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Os valores de char agora são armazenados como TEXTO no SQLite

[Problema de acompanhamento nº 15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

**Comportamento antigo**

Anteriormente, os valores de char eram armazenados como valores INTEIROS no SQLite. Por exemplo, o valor de char *A* era armazenado como o valor inteiro 65.

**Novo comportamento**

Agora, os valores de char são armazenados como TEXTO.

**Porque**

O armazenamento dos valores como TEXTO é mais natural e permite que o banco de dados seja mais compatível com outras tecnologias.

**Atenuações**

Você pode migrar os bancos de dados existentes para o novo formato executando um código SQL semelhante ao seguinte.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

No EF Core, você pode continuar usando o comportamento anterior, configurando um conversor de valor nessas propriedades.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

O Microsoft.Data.Sqlite também continua capaz de ler os valores de caractere das colunas de INTEIRO e de TEXTO, para que determinados cenários não exijam nenhuma ação.

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>As IDs de migração agora são geradas usando o calendário da cultura invariável

[Problema de acompanhamento nº 12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

**Comportamento antigo**

As IDs de migração eram geradas inadvertidamente, usando o calendário da cultura atual.

**Novo comportamento**

Agora, as IDs de migração sempre são geradas usando o calendário da cultura invariável (gregoriano).

**Porque**

A ordem das migrações é importante para a atualização do banco de dados ou a resolução de conflitos de mesclagem. O uso do calendário invariável evita os problemas de ordenação que podem ocorrer quando os membros da equipe têm calendários de sistemas diferentes.

**Atenuações**

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

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a>UseRowNumberForPaging foi removido

[Acompanhamento de problema nº 16400](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

**Comportamento antigo**

Antes do EF Core 3.0, o `UseRowNumberForPaging` podia ser usado para gerar SQL para paginação compatível com SQL Server 2008.

**Novo comportamento**

A partir do EF Core 3.0, o EF só gerará SQL para paginação que seja compatível apenas com as versões posteriores do SQL Server. 

**Porque**

Estamos fazendo essa alteração porque o [SQL Server 2008 não é mais um produto com suporte](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e a atualização desse recurso para trabalhar com as alterações de consulta feitas no EF Core 3.0 gera um trabalho significativo.

**Atenuações**

É recomendável atualizar para uma versão mais recente do SQL Server ou usar um nível de compatibilidade superior para que haja suporte para o SQL gerado. Assim, se isso não for possível, [comente no rastreamento do problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) com detalhes. Poderemos rever essa decisão com base nos comentários.

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a>As informações de extensão/metadados foram removidas do IDbContextOptionsExtension

[Acompanhamento de problema nº 16119](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

**Comportamento antigo**

`IDbContextOptionsExtension` continha métodos para fornecer metadados sobre a extensão.

**Novo comportamento**

Esses métodos foram movidos para uma nova classe base abstrata `DbContextOptionsExtensionInfo`, que é retornada da nova propriedade `IDbContextOptionsExtension.Info`.

**Porque**

Nas versões 2.0 a 3.0, precisamos adicionar ou alterar esses métodos várias vezes.
Dividi-los em uma nova classe base abstrata facilitará a realização desse tipo de alteração sem interromper as extensões existentes.

**Atenuações**

Atualize as extensões para que sigam o novo padrão.
Os exemplos são encontrados em várias implementações de `IDbContextOptionsExtension` em diferentes tipos de extensões no código-fonte do EF Core.

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a>LogQueryPossibleExceptionWithAggregateOperator foi renomeado

[Acompanhamento de problema nº 10985](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

**Mudar**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` foi renomeado para `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.

**Porque**

Alinha a nomeação deste evento de aviso com todos os outros eventos de aviso.

**Atenuações**

Use o novo nome. (Observe que o número da ID do evento não foi alterado.)

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a>Esclarecer a API para nomes de restrição de chave estrangeira

[Acompanhamento de problema nº 10730](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

**Comportamento antigo**

Antes do EF Core 3.0, os nomes de restrição de chave estrangeira eram chamados simplesmente de "nome". Por exemplo:

```csharp
var constraintName = myForeignKey.Name;
```

**Novo comportamento**

A partir do EF Core 3.0, os nomes de restrição de chave estrangeira são chamados de "nome de restrição". Por exemplo:

```csharp
var constraintName = myForeignKey.ConstraintName;
```

**Porque**

Essa alteração traz consistência à nomeação nessa área e também esclarece que esse é o nome de restrição de chave estrangeira, e não o nome da coluna ou da propriedade na qual a chave estrangeira está definida.

**Atenuações**

Use o novo nome.

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a>IRelationalDatabaseCreator.HasTables/HasTablesAsync tornaram-se públicos

[Acompanhamento de problema nº 15997](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

**Comportamento antigo**

Antes do EF Core 3.0, esses métodos eram protegidos.

**Novo comportamento**

A partir do EF Core 3.0, esses métodos são públicos.

**Porque**

Esses métodos são usados pelo EF para determinar se um banco de dados foi criado, mas está vazio. Isso também pode ser útil fora do EF ao determinar se as migrações serão ou não aplicadas.

**Atenuações**

Altere a acessibilidade de todas as substituições.

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a>Agora Microsoft.EntityFrameworkCore.Design é um pacote de DevelopmentDependency

[Acompanhamento de problema nº 11506](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

**Comportamento antigo**

Antes do EF Core 3.0, o Microsoft.EntityFrameworkCore.Design era um pacote regular do NuGet cujo assembly podia ser referenciado por projetos que dependiam dele.

**Novo comportamento**

A partir do EF Core 3.0, ele se tornou um pacote de DevelopmentDependency. Isso significa que a dependência não fluirá transitivamente para outros projetos, e que você não pode mais, por padrão, referenciar sua montagem.

**Porque**

Esse pacote destina-se para uso somente em tempo de design. Os aplicativos implantados não devem fazer referência a ele. Tornar o pacote um DevelopmentDependency reforça essa recomendação.

**Atenuações**

Se você precisar referenciar esse pacote para substituir o comportamento de tempo de design do EF Core, então você pode atualizar os metadados do item PackageReference em seu projeto.

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

Se o pacote estiver sendo referenciado transitivamente por meio de Microsoft.EntityFrameworkCore.Tools, você precisará adicionar um PackageReference explícito ao pacote para alterar seus metadados. Essa referência explícita deve ser adicionada a qualquer projeto em que os tipos do pacote sejam necessários.

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a>SQLitePCL.raw atualizado para a versão 2.0.0

[Acompanhamento de problema nº 14824](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

**Comportamento antigo**

Anteriormente, Microsoft.EntityFrameworkCore.Sqlite dependia da versão 1.1.12 do SQLitePCL.raw.

**Novo comportamento**

Atualizamos nosso pacote para depender da versão 2.0.0.

**Porque**

A versão 2.0.0 do SQLitePCL.raw é voltada para o .NET Standard 2.0. Anteriormente, era voltada ao .NET Standard 1.1, o que exigia um fechamento grande de pacotes transitivos para funcionar.

**Atenuações**

O SQLitePCL.raw versão 2.0.0 inclui algumas alterações de falha. Consulte as [notas de liberação](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obter detalhes.

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a>NetTopologySuite atualizado para a versão 2.0.0

[Acompanhamento do problema n.º 14825](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

**Comportamento antigo**

Os pacotes espaciais anteriormente dependiam da versão 1.15.1 do NetTopologySuite.

**Novo comportamento**

Atualizamos nosso pacote para depender da versão 2.0.0.

**Porque**

O objetivo da versão 2.0.0 do NetTopologySuite é abordar vários problemas de usabilidade encontrados pelos usuários do EF Core.

**Atenuações**

O NetTopologySuite versão 2.0.0 inclui algumas alterações significativas. Consulte as [notas de liberação](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obter detalhes.

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a>Microsoft.Data.SqlClient é usado em vez de System.Data.SqlClient

[#15636 de problemade rastreamento](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

**Comportamento antigo**

Microsoft.EntityFrameworkCore.SqlServer anteriormente dependia do System.Data.SqlClient.

**Novo comportamento**

Atualizamos nosso pacote para depender do Microsoft.Data.SqlClient.

**Porque**

Microsoft.Data.SqlClient é o driver de acesso de dados principal para o SQL Server daqui para frente, e System.Data.SqlClient não será mais o foco do desenvolvimento.
Alguns recursos importantes, como always encrypted, só estão disponíveis no Microsoft.Data.SqlClient.

**Atenuações**

Se o seu código tiver uma dependência direta do System.Data.SqlClient, você deve alterá-lo para referenciar o Microsoft.Data.SqlClient; como os dois pacotes mantêm um grau muito alto de compatibilidade com a API, este deve ser apenas uma simples alteração de pacote e namespace.

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a>Várias relações ambíguas de autorreferência devem ser configuradas 

[Acompanhamento de Problema nº 13573](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

**Comportamento antigo**

Um tipo de entidade com várias propriedades de navegação unidirecionais de autorreferência e correspondência de FKs foi configurado incorretamente como uma relação única. Por exemplo:

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

**Novo comportamento**

Esse cenário agora é detectado na criação de modelo e uma exceção é gerada indicando que o modelo é ambíguo.

**Porque**

O modelo resultante era ambíguo e provavelmente, em geral, estará errado para esse caso.

**Atenuações**

Use a configuração completa da relação. Por exemplo:

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a>DbFunction.Schema sendo uma seqüência de configurações nulas ou vazias configura-o para estar no esquema padrão do modelo

[#12757 de problemas de rastreamento](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

**Comportamento antigo**

Uma Função Db configurada com esquema como uma seqüência vazia foi tratada como função incorporada sem um esquema. Por exemplo, seguindo `DatePart` código, `DATEPART` mapeará a função CLR para a função incorporada no SqlServer.

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

**Novo comportamento**

Todos os mapeamentos dbFunction são considerados mapeados para funções definidas pelo usuário. Assim, o valor vazio da seqüência colocaria a função dentro do esquema padrão para o modelo. Que pode ser o esquema configurado explicitamente `modelBuilder.HasDefaultSchema()` através `dbo` de API fluente ou de outra forma.

**Porque**

Anteriormente, o esquema de estar vazio era uma maneira de tratar que a função é incorporada, mas essa lógica só é aplicável para sqlServer onde funções incorporadas não pertencem a nenhum esquema.

**Atenuações**

Configure a tradução do DbFunction manualmente para mapeá-la para uma função incorporada.

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
