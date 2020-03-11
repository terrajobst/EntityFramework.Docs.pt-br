---
title: Avaliação do cliente versus do servidor-EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417756"
---
# <a name="client-vs-server-evaluation"></a>Avaliação do cliente versus do servidor

Como regra geral, Entity Framework Core tenta avaliar uma consulta no servidor o máximo possível. EF Core converte partes da consulta em parâmetros, que podem ser avaliadas no lado do cliente. O resto da consulta (juntamente com os parâmetros gerados) é fornecido ao provedor de banco de dados para determinar a consulta de banco de dados equivalente a ser avaliada no servidor. O EF Core dá suporte à avaliação parcial do cliente na projeção de nível superior (essencialmente, a última chamada para `Select()`). Se a projeção de nível superior na consulta não puder ser convertida no servidor, EF Core obterá todos os dados necessários do servidor e avaliará as partes restantes da consulta no cliente. Se EF Core detectar uma expressão, em qualquer lugar diferente da projeção de nível superior, que não pode ser traduzida para o servidor, ele lançará uma exceção de tempo de execução. Veja [como a consulta funciona](xref:core/querying/how-query-works) para entender como EF Core determina o que não pode ser convertido no servidor.

> [!NOTE]
> Antes da versão 3,0, Entity Framework Core avaliação de cliente com suporte em qualquer lugar na consulta. Para obter mais informações, consulte a [seção versões anteriores](#previous-versions).

> [!TIP]
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="client-evaluation-in-the-top-level-projection"></a>Avaliação do cliente na projeção de nível superior

No exemplo a seguir, um método auxiliar é usado para padronizar URLs para Blogs, que são retornados de um banco de dados SQL Server. Como o provedor de SQL Server não tem informações sobre como esse método é implementado, não é possível convertê-lo em SQL. Todos os outros aspectos da consulta são avaliados no banco de dados, mas passar o `URL` retornado por esse método é feito no cliente.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Avaliação de cliente sem suporte

Embora a avaliação do cliente seja útil, isso pode resultar em um desempenho insatisfatório às vezes. Considere a consulta a seguir, na qual o método auxiliar agora é usado em um filtro Where. Como o filtro não pode ser aplicado no banco de dados do, todos eles precisam ser retirados da memória para aplicar o filtro no cliente. Com base no filtro e na quantidade de dados no servidor, a avaliação do cliente pode resultar em baixo desempenho. Portanto Entity Framework Core bloqueia essa avaliação do cliente e gera uma exceção de tempo de execução.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Avaliação explícita do cliente

Talvez seja necessário forçar a avaliação do cliente explicitamente em determinados casos, como a seguir

- A quantidade de dados é pequena para que a avaliação no cliente não incorra em uma enorme penalidade de desempenho.
- O operador LINQ que está sendo usado não tem tradução do lado do servidor.

Nesses casos, você pode explicitamente optar pela avaliação do cliente chamando métodos como `AsEnumerable` ou `ToList` (`AsAsyncEnumerable` ou `ToListAsync` para Async). Usando `AsEnumerable` você estaria transmitindo os resultados, mas usar `ToList` causaria o armazenamento em buffer criando uma lista, que também usa memória adicional. No entanto, se você estiver enumerando várias vezes, armazenar os resultados em uma lista ajuda mais, pois há apenas uma consulta para o banco de dados. Dependendo do uso específico, você deve avaliar qual método é mais útil para o caso.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Possível vazamento de memória na avaliação do cliente

Como a conversão de consultas e a compilação são caras, EF Core armazena em cache o plano de consulta compilado. O delegado em cache pode usar o código do cliente enquanto faz a avaliação do cliente da projeção de nível superior. EF Core gera parâmetros para as partes avaliadas pelo cliente da árvore e reutiliza o plano de consulta substituindo os valores de parâmetro. Mas determinadas constantes na árvore de expressão não podem ser convertidas em parâmetros. Se o delegado em cache contiver essas constantes, esses objetos não poderão ser coletados pelo lixo, pois ainda estão sendo referenciados. Se esse objeto contiver um DbContext ou outros serviços nele, isso poderá fazer com que o uso da memória do aplicativo cresça ao longo do tempo. Esse comportamento geralmente é um sinal de perda de memória. EF Core gera uma exceção sempre que se encontra entre constantes de um tipo que não podem ser mapeadas usando o provedor de banco de dados atual. As causas comuns e suas soluções são as seguintes:

- **Usando um método de instância**: ao usar métodos de instância em uma projeção de cliente, a árvore de expressão contém uma constante da instância. Se o método não usar nenhum dado da instância, considere tornar o método estático. Se você precisar de dados de instância no corpo do método, passe os dados específicos como um argumento para o método.
- **Passando argumentos constantes para o método**: esse caso ocorre geralmente usando `this` em um argumento para o método de cliente. Considere dividir o argumento em vários argumentos escalares, que podem ser mapeados pelo provedor de banco de dados.
- **Outras constantes**: se uma constante for proveniente de qualquer outro caso, você poderá avaliar se a constante é necessária no processamento. Se for necessário ter a constante ou se você não puder usar uma solução dos casos acima, crie uma variável local para armazenar o valor e use a variável local na consulta. EF Core converterá a variável local em um parâmetro.

## <a name="previous-versions"></a>Versões anteriores

A seção a seguir aplica-se às versões EF Core anteriores a 3,0.

Versões mais antigas do EF Core com suporte para avaliação do cliente em qualquer parte da consulta – não apenas na projeção de nível superior. É por isso que as consultas semelhantes a uma publicadas na seção de [avaliação do cliente sem suporte](#unsupported-client-evaluation) funcionaram corretamente. Como esse comportamento pode causar problemas de desempenho não percebidos, EF Core registrada em log um aviso de avaliação do cliente. Para obter mais informações sobre como exibir a saída de log, consulte [Logging](xref:core/miscellaneous/logging).

Opcionalmente EF Core permite que você altere o comportamento padrão para gerar uma exceção ou não fazer nada ao fazer a avaliação do cliente (exceto para na projeção). O comportamento de lançamento de exceção o tornaria semelhante ao comportamento em 3,0. Para alterar o comportamento, você precisa configurar avisos enquanto configura as opções para seu contexto, normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
