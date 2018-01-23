---
title: Carregamento de dados - Core EF relacionados ao
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a>Carregamento de dados relacionados

Entity Framework Core permite que você use as propriedades de navegação em seu modelo para carregar as entidades relacionadas. Há três padrões de S/RM comuns usados para carregar os dados relacionados.
* **Carregamento adiantado** significa que os dados relacionados são carregados do banco de dados como parte da consulta inicial.
* **Carregamento explícito** significa que os dados relacionados explicitamente carregados do banco de dados em um momento posterior.
* **Carregamento preguiçoso** significa que os dados relacionados são carregados transparente do banco de dados quando a propriedade de navegação é acessada. Carregamento preguiçoso ainda não é possível com o EF Core.

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) deste artigo no GitHub.

## <a name="eager-loading"></a>Carregamento adiantado

Você pode usar o `Include` método para especificar dados relacionados a serem incluídos nos resultados da consulta. No exemplo a seguir, os blogs que são retornados nos resultados terão seus `Posts` propriedade preenchida com as postagens relacionadas.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core será corrigir automaticamente o propriedades de navegação para outras entidades que foram previamente carregadas para a instância de contexto. Dessa forma, mesmo se você não incluir explicitamente os dados para uma propriedade de navegação, a propriedade ainda pode ser populada se algumas ou todas as entidades relacionadas foram carregadas anteriormente.


Você pode incluir dados relacionados de várias relações em uma única consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Incluindo vários níveis

Você pode fazer uma busca detalhada por meio de relações para incluir vários níveis de dados relacionados usando o `ThenInclude` método. O exemplo a seguir carrega todos os blogs, suas postagens relacionadas e autor de cada postagem.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> As versões atuais do Visual Studio oferecem opções de conclusão de código incorreto e pode causar expressões corretas para ser sinalizadas com erros de sintaxe ao usar o `ThenInclude` método após uma propriedade de navegação da coleção. Este é um sintoma de um bug de IntelliSense controlado no https://github.com/dotnet/roslyn/issues/8237. É seguro ignorar esses erros de sintaxe artificiais desde que o código está correto e pode ser compilado com êxito. 

É possível encadear chamadas múltiplas para `ThenInclude` para continuar incluindo mais níveis de dados relacionados.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Você pode combinar tudo isso para incluir dados relacionados de vários níveis e várias raízes na mesma consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Você talvez queira incluir várias entidades relacionadas para uma das entidades que está sendo incluída. Por exemplo, ao consultar `Blog`s, que você incluir `Posts` e deseja incluir o `Author` e `Tags` do `Posts`. Para fazer isso, você precisa especificar cada incluem a partir da raiz do caminho. Por exemplo, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`. Isso não significa que você obterá junções redundantes, na maioria dos casos que EF serão consolidados associações durante a geração de SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignorado inclui

Se você alterar a consulta para que ele não retorna instâncias do tipo de entidade que a consulta foi iniciada com o, os operadores de inclusão são ignorados.

No exemplo a seguir, os operadores de inclusão se baseiam o `Blog`, mas, em seguida, o `Select` operador é usado para alterar a consulta para retornar um tipo anônimo. Nesse caso, os operadores de inclusão não têm efeito.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Por padrão, o núcleo de EF registrará um aviso quando inclui operadores são ignorados. Consulte [log](../miscellaneous/logging.md) para obter mais informações sobre como exibir a saída de log. Você pode alterar o comportamento quando um operador include é ignorado para gerar ou não faça nada. Isso é feito ao configurar as opções para o contexto - normalmente em `DbContext.OnConfiguring`, ou em `Startup.cs` se você estiver usando o ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Carregamento explícito

> [!NOTE]  
> Esse recurso foi introduzido no EF Core 1.1.

Você pode carregar explicitamente uma propriedade de navegação por meio de `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Você pode carregar também explicitamente uma propriedade de navegação executando uma consulta separada que retorna as entidades relacionadas. Se o controle de alterações está habilitada, ao carregar uma entidade, Core EF automaticamente definir as propriedades de navegação da entidade para se referir a qualquer entidade que já carregada recentemente carregado e definir as propriedades de navegação das entidades já carregado para referir-se a entidade carregado recentemente.

### <a name="querying-related-entities"></a>Consultando entidades relacionadas

Você também pode obter uma consulta LINQ que representa o conteúdo de uma propriedade de navegação.

Isso permite que você faça coisas como a execução de um operador de agregação sobre as entidades relacionadas sem carregá-los na memória.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Você também pode filtrar quais entidades relacionadas são carregadas na memória.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Carregamento preguiçoso

Carregamento preguiçoso ainda não é suportado por núcleo EF. Você pode exibir o [item de carregamento lento em nossa lista de pendências](https://github.com/aspnet/EntityFramework/issues/3797) para controlar esse recurso.

## <a name="related-data-and-serialization"></a>Serialização e dados relacionados

Como propriedades de navegação de automaticamente a correção do EF principal será você pode acabar com ciclos em seu gráfico de objeto. Por exemplo, o carregamento de um blog e está relacionado postagens resultará em um objeto de blog que referencia uma coleção de postagens. Cada essas postagens terá uma referência de volta para o blog.

Algumas estruturas de serialização não permitir que esses ciclos. Por exemplo, Json.NET lançará a exceção a seguir se encontrou um ciclo.

> Newtonsoft.Json.JsonSerializationException: Self referenciando loop detectado para a propriedade 'Blog' com tipo 'MyApplication.Models.Blog'.

Se você estiver usando o ASP.NET Core, você pode configurar Json.NET para ignorar ciclos que encontrar no gráfico de objeto. Isso é feito o `ConfigureServices(...)` método `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
