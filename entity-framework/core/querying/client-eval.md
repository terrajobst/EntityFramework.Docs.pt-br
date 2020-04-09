---
title: Avaliação cliente vs. Servidor - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417756"
---
# <a name="client-vs-server-evaluation"></a>Avaliação cliente vs. Servidor

Como regra geral, o Entity Framework Core tenta avaliar uma consulta no servidor o máximo possível. O EF Core converte partes da consulta em parâmetros, que ele pode avaliar no lado do cliente. O restante da consulta (juntamente com os parâmetros gerados) é dado ao provedor de banco de dados para determinar a consulta de banco de dados equivalente para avaliar no servidor. O EF Core suporta a avaliação parcial do cliente na projeção de nível superior (essencialmente, a última chamada para `Select()`). Se a projeção de nível superior na consulta não puder ser traduzida para o servidor, o EF Core buscará quaisquer dados necessários do servidor e avaliará as partes restantes da consulta no cliente. Se o EF Core detectar uma expressão, em qualquer lugar que não seja a projeção de nível superior, que não pode ser traduzida para o servidor, então ele lança uma exceção de tempo de execução. Veja [como a consulta funciona](xref:core/querying/how-query-works) para entender como o EF Core determina o que não pode ser traduzido para servidor.

> [!NOTE]
> Antes da versão 3.0, o Entity Framework Core suportava a avaliação do cliente em qualquer lugar da consulta. Para obter mais informações, consulte a [seção versões anteriores](#previous-versions).

> [!TIP]
> Você pode ver a [amostra](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="client-evaluation-in-the-top-level-projection"></a>Avaliação do cliente na projeção de nível superior

No exemplo a seguir, um método auxiliar é usado para padronizar URLs para blogs, que são devolvidos a partir de um banco de dados SQL Server. Como o provedor do SQL Server não tem nenhuma ideia de como esse método é implementado, não é possível traduzi-lo para O SQL. Todos os outros aspectos da consulta são avaliados no banco `URL` de dados, mas a aprovação do retorno através desse método é feita no cliente.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Avaliação de clientes sem suporte

Embora a avaliação do cliente seja útil, pode resultar em um desempenho ruim às vezes. Considere a seguinte consulta, na qual o método auxiliar é agora usado em um filtro onde. Como o filtro não pode ser aplicado no banco de dados, todos os dados precisam ser puxados para a memória para aplicar o filtro no cliente. Com base no filtro e na quantidade de dados no servidor, a avaliação do cliente pode resultar em um desempenho ruim. Assim, o Entity Framework Core bloqueia essa avaliação de clientes e lança uma exceção de tempo de execução.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Avaliação explícita do cliente

Você pode precisar forçar a avaliação do cliente explicitamente em certos casos como seguir

- A quantidade de dados é pequena para que a avaliação no cliente não incorra em uma grande penalidade de desempenho.
- O operador LINQ que está sendo usado não tem tradução do lado do servidor.

Nesses casos, você pode optar explicitamente pela avaliação `AsEnumerable` do `ToList` `AsAsyncEnumerable` cliente `ToListAsync` ligando para métodos como ou (ou para assincronismo). Ao `AsEnumerable` usar você estaria transmitindo os `ToList` resultados, mas usar causaria buffering criando uma lista, que também leva memória adicional. Embora se você estiver enumerando várias vezes, então armazenar resultados em uma lista ajuda mais, já que há apenas uma consulta ao banco de dados. Dependendo do uso específico, você deve avaliar qual método é mais útil para o caso.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Vazamento potencial de memória na avaliação do cliente

Uma vez que a tradução e a compilação da consulta são caras, o EF Core armazena o plano de consulta compilado. O delegado armazenado em cache pode usar o código do cliente enquanto faz a avaliação do cliente de projeção de nível superior. O EF Core gera parâmetros para as partes avaliadas pelo cliente da árvore e reutiliza o plano de consulta substituindo os valores dos parâmetros. Mas certas constantes na árvore de expressão não podem ser convertidas em parâmetros. Se o delegado armazenado contém tais constantes, então esses objetos não podem ser coletados lixo, pois ainda estão sendo referenciados. Se tal objeto contiver um DbContext ou outros serviços nele, então ele pode fazer com que o uso da memória do aplicativo cresça com o tempo. Esse comportamento é geralmente um sinal de um vazamento de memória. O EF Core abre uma exceção sempre que se depara com constantes de um tipo que não pode ser mapeado usando o provedor de banco de dados atual. Causas comuns e suas soluções são as seguintes:

- **Usando um método de instância**: Ao usar métodos de ocorrência em uma projeção de cliente, a árvore de expressão contém uma constante da instância. Se o seu método não usar nenhum dado da instância, considere tornar o método estático. Se você precisar de dados de instância no corpo do método, então passe os dados específicos como um argumento para o método.
- **Passando argumentos constantes para o método**: `this` Este caso surge geralmente usando em um argumento para o método do cliente. Considere dividir o argumento em vários argumentos escalares, que podem ser mapeados pelo provedor de banco de dados.
- **Outras constantes**: Se uma constante é vista em qualquer outro caso, então você pode avaliar se a constante é necessária no processamento. Se for necessário ter a constante, ou se você não puder usar uma solução dos casos acima, então crie uma variável local para armazenar o valor e usar a variável local na consulta. O EF Core converterá a variável local em um parâmetro.

## <a name="previous-versions"></a>Versões anteriores

A seção a seguir se aplica às versões EF Core antes do 3.0.

Versões mais antigas do EF Core suportavam avaliação de clientes em qualquer parte da consulta - não apenas a projeção de nível superior. É por isso que consultas semelhantes a uma postada na seção [de avaliação de clientes sem suporte](#unsupported-client-evaluation) funcionaram corretamente. Como esse comportamento pode causar problemas de desempenho despercebidos, o EF Core registrou um aviso de avaliação do cliente. Para obter mais informações sobre como visualizar a saída de registro, consulte [Registrar](xref:core/miscellaneous/logging).

Opcionalmente, o EF Core permitiu que você alterasse o comportamento padrão para lançar uma exceção ou não fazer nada ao fazer avaliação do cliente (exceto na projeção). O comportamento de arremesso de exceção o tornaria semelhante ao comportamento em 3.0. Para alterar o comportamento, você precisa configurar avisos enquanto configura as `DbContext.OnConfiguring`opções `Startup.cs` para o seu contexto - normalmente em - ou em se você estiver usando ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
