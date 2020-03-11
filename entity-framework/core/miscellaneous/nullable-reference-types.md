---
title: Trabalhando com tipos de referência anuláveis-EF Core
author: roji
ms.date: 09/09/2019
ms.assetid: bde4e0ee-fba3-4813-a849-27049323d301
uid: core/miscellaneous/nullable-reference-types
ms.openlocfilehash: c16a475c363320cd18804a4efe78ccae1ae22f0d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416651"
---
# <a name="working-with-nullable-reference-types"></a>Trabalhando com tipos de referência anuláveis

C#8 introduziu um novo recurso chamado [tipos de referência anulável](/dotnet/csharp/tutorials/nullable-reference-types), permitindo que os tipos de referência sejam anotados, indicando se é válido para que eles contenham nulo ou não. Se você for novo nesse recurso, é recomendável que você se familiarize com ele lendo os C# documentos.

Esta página apresenta o suporte de EF Core para tipos de referência anuláveis e descreve as práticas recomendadas para trabalhar com eles.

## <a name="required-and-optional-properties"></a>Propriedades obrigatórias e opcionais

A documentação principal sobre as propriedades obrigatórias e opcionais e sua interação com tipos de referência anuláveis é a página de [Propriedades obrigatória e opcional](xref:core/modeling/entity-properties#required-and-optional-properties) . É recomendável começar lendo essa página primeiro.

> [!NOTE]
> Tenha cuidado ao habilitar tipos de referência anuláveis em um projeto existente: as propriedades do tipo de referência que foram previamente configuradas como opcionais agora serão configuradas conforme necessário, a menos que sejam anotadas explicitamente para permitir valor nulo. Ao gerenciar um esquema de banco de dados relacional, isso pode fazer com que as migrações sejam geradas, alterando a nulidade da coluna do banco de dados.

## <a name="dbcontext-and-dbset"></a>DbContext e DbSet

Quando tipos de referência anuláveis são habilitados, o C# compilador emite avisos para qualquer propriedade não anulável não inicializada, pois eles conteriam NULL. Como resultado, a prática comum de definir um `DbSet` não anulável em um contexto irá gerar um aviso. No entanto, EF Core sempre Inicializa todas as propriedades de `DbSet` em tipos derivados de DbContext, para que sejam garantidas nunca serem nulas, mesmo que o compilador não esteja ciente disso. Portanto, é recomendável manter suas propriedades `DbSet` não anuláveis, permitindo que você as acesse sem verificações nulas, e para silenciar os avisos do compilador, definindo-os explicitamente como NULL com a ajuda do operador NULL-tolerante (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/NullableReferenceTypesContext.cs?name=Context&highlight=3-4)]

## <a name="non-nullable-properties-and-initialization"></a>Propriedades e inicialização não anuláveis

Os avisos do compilador para tipos de referência não anuláveis não inicializados também são um problema para propriedades regulares em seus tipos de entidade. Em nosso exemplo acima, evitamos esses avisos usando a [Associação de Construtor](xref:core/modeling/constructors), um recurso que funciona perfeitamente com propriedades não anuláveis, garantindo que eles sejam sempre inicializados. No entanto, em alguns cenários, a associação de construtor não é uma opção: as propriedades de navegação, por exemplo, não podem ser inicializadas dessa maneira.

As propriedades de navegação necessárias apresentam uma dificuldade adicional: embora um dependente sempre exista para uma determinada entidade de segurança, ele pode ou não ser carregado por uma consulta específica, dependendo das necessidades nesse ponto do programa ([consulte os diferentes padrões para carregar dados](xref:core/querying/related-data)). Ao mesmo tempo, não é desejável tornar essas propriedades anuláveis, pois isso forçaria todo o acesso a elas para verificar se há algum valor nulo, mesmo se eles forem necessários.

Uma maneira de lidar com esses cenários é ter uma propriedade não anulável com um [campo de backup](xref:core/modeling/backing-field)anulável:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=10-17)]

Como a propriedade de navegação é não anulável, uma navegação necessária é configurada; e, desde que a navegação seja carregada corretamente, o dependente poderá ser acessado por meio da propriedade. No entanto, se a propriedade for acessada sem primeiro carregar adequadamente a entidade relacionada, um InvalidOperationException será gerado, pois o contrato de API foi usado incorretamente. Observe que o EF deve ser configurado para sempre acessar o campo de backup e não a propriedade, pois depende de ser capaz de ler o valor mesmo quando não definido; consulte a [documentação sobre como fazer isso](xref:core/modeling/backing-field) , e considere especificar `PropertyAccessMode.Field` para verificar se a configuração está correta.

Como alternativa terser, é possível simplesmente inicializar a propriedade como NULL com a ajuda do operador NULL-tolerante (!):

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Order.cs?range=19)]

Um valor nulo real nunca será observado, exceto como resultado de um bug de programação, por exemplo, acessar a propriedade de navegação sem carregar corretamente a entidade relacionada com antecedência.

> [!NOTE]
> As navegações de coleção, que contêm referências a várias entidades relacionadas, sempre devem ser não anuláveis. Uma coleção vazia significa que não existe nenhuma entidade relacionada, mas a lista em si nunca deve ser nula.

## <a name="navigating-and-including-nullable-relationships"></a>Navegando e incluindo relações anuláveis

Ao lidar com relações opcionais, é possível encontrar avisos do compilador em que uma exceção de referência nula real seria impossível. Ao traduzir e executar suas consultas LINQ, o EF Core garante que, se uma entidade relacionada opcional não existir, qualquer navegação nela será simplesmente ignorada, em vez de lançá-la. No entanto, o compilador não sabe disso EF Core garante e produz avisos como se a consulta LINQ fosse executada na memória, com LINQ to Objects. Como resultado, é necessário usar o operador NULL-tolerante (!) para informar ao compilador que um valor nulo real não é possível:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=46)]

Um problema semelhante ocorre quando se inclui vários níveis de relações entre navegações opcionais:

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Program.cs?range=36-39&highlight=2)]

Se você estiver fazendo isso muito, e os tipos de entidade em questão forem predominantemente (ou exclusivamente) usados em consultas EF Core, considere tornar as propriedades de navegação não anuláveis e configurá-las como opcionais por meio da API fluente ou das anotações de dados. Isso removerá todos os avisos do compilador enquanto mantém a relação opcional; no entanto, se suas entidades forem atravessadas fora do EF Core, você poderá observar valores nulos, embora as propriedades sejam anotadas como não anuláveis.

## <a name="limitations"></a>Limitações

* A engenharia reversa atualmente não dá suporte C# [ C# a 8 tipos de referência anuláveis (NRTs)](/dotnet/csharp/tutorials/nullable-reference-types): EF Core sempre gera código que assume que o recurso está desativado. Por exemplo, colunas de texto anuláveis serão com Scaffold como uma propriedade com o tipo `string`, não `string?`, com a API Fluent ou as anotações de dados usadas para configurar se uma propriedade é necessária ou não. Você pode editar o código com Scaffold e substituí-los C# por anotações de nulidade. O suporte do scaffolding para tipos de referência anuláveis é acompanhado pelo problema [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).
* A superfície da API pública do EF Core ainda não foi anotada quanto à nulidade (a API pública é "NULL-alheios"), o que, às vezes, é estranho usar quando o recurso NRT está ativado. Isso notavelmente inclui os operadores assíncronos LINQ expostos por EF Core, como [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_). Planejamos resolver isso para a versão 5,0.
