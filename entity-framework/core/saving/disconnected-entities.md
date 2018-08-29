---
title: Entidades Desconectadas – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: a81b0a26fe98dcc1ddedc11aba2673338c8991e8
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447722"
---
# <a name="disconnected-entities"></a>Entidades desconectadas

Uma instância DbContext automaticamente controlará as entidades retornadas do banco de dados. As alterações feitas a essas entidades serão detectadas quando SaveChanges for chamado e o banco de dados será atualizado conforme o necessário. Veja [Salvamento Básico](basic.md) e [Dados Relacionados](related-data.md) para obter detalhes.

No entanto, às vezes, as entidades são consultadas usando uma instância de contexto e, em seguida, salvas usando uma instância diferente. Isso geralmente ocorre em cenários "desconectados", por exemplo, um aplicativo da Web onde as entidades são consultadas, enviadas ao cliente, modificadas, enviadas de volta para o servidor em uma solicitação e salvas. Nesse caso, o contexto da segunda instância precisa saber se as entidades são novas (devem ser inseridas) ou existentes (devem ser atualizadas).

> [!TIP]  
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) deste artigo no GitHub.

> [!TIP]
> O EF Core só pode controlar uma instância de qualquer entidade com um determinado valor de chave primária. A melhor maneira de evitar que esse seja um problema é usar um contexto de curta duração para cada unidade de trabalho, de modo que o contexto começa vazio, tem entidades anexadas a ele, salva essas entidades e, em seguida, o contexto é descartado.

## <a name="identifying-new-entities"></a>Identificando novas entidades

### <a name="client-identifies-new-entities"></a>O cliente identifica novas entidades

O caso mais simples para lidar com isso é quando o cliente informa ao servidor se a entidade é nova ou existente. Por exemplo, geralmente, a solicitação para inserir uma nova entidade é diferente da solicitação para atualizar uma entidade existente.

O restante desta seção aborda os casos onde é necessário determinar se deseja inserir ou atualizar de alguma outra maneira.

### <a name="with-auto-generated-keys"></a>Com as chaves geradas automaticamente

O valor de uma chave gerada automaticamente geralmente pode ser usado para determinar se uma entidade precisa ser inserida ou atualizada. Se a chave não tiver sido definida (ou seja, ela ainda tem o valor padrão CLR de nulo, zero etc.), a entidade deverá ser nova e precisa de inserção. Por outro lado, se o valor da chave tiver sido definido, ele já deverá ter sido salvo anteriormente e agora precisa ser atualizado. Em outras palavras, se a chave tem um valor, a entidade foi consultada, enviada para o cliente e agora volta para ser atualizada.

É fácil verificar se há uma chave não definida quando o tipo de entidade é desconhecido:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

No entanto, o EF também tem uma forma interna de fazer isso para qualquer tipo de entidade e o tipo de chave:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> As chaves são definidas assim que as entidades são controladas pelo contexto, mesmo se a entidade estiver no estado adicionado. Isso ajuda ao passar um gráfico de entidades e decidir o que fazer com cada, por exemplo, ao usar a API TrackGraph. O valor da chave somente deve ser usado da forma mostrada aqui _antes_ que alguma chamada seja feita para controlar a entidade.

### <a name="with-other-keys"></a>Com outras chaves

Algum outro mecanismo é necessário para identificar novas entidades quando os valores de chave não são gerados automaticamente. Há duas abordagens gerais para isso:
 * Consulta para a entidade
 * Passar um sinalizador do cliente

Para consultar para a entidade, use apenas o método de localização:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Está além do escopo deste documento mostrar o código completo para passar um sinalizador de um cliente. Em um aplicativo Web, isso geralmente significa fazer solicitações diferentes para diferentes ações, ou passar algum estado na solicitação e, em seguida, extraí-lo no controlador.

## <a name="saving-single-entities"></a>Como salvar entidades simples

Se ele for conhecido ou não, uma inserção ou atualização será necessária, em seguida, adicionar ou atualizar pode ser usado de forma apropriada:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

No entanto, se a entidade usar valores de chave gerada automaticamente, o método de atualização poderá ser usado para ambos os casos:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

O método de atualização normalmente marca a entidade para a atualização, não inserção. No entanto, se a entidade tem uma chave gerada automaticamente e nenhum valor de chave tiver sido definido, a entidade será automaticamente marcada para inserir.

> [!TIP]  
> Esse comportamento foi introduzido no EF Core 2.0. Para versões anteriores, sempre é necessário escolher explicitamente adicionar ou atualizar.

Se a entidade não estiver usando as chaves geradas automaticamente, o aplicativo deverá decidir se a entidade deve ser inserida ou atualizada. Por exemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

As etapas aqui são:
* Se Localizar retornar nulo, isso significa que o banco de dados ainda não contém o blog com essa ID; portanto, chamamos Adicionar e a marcaremos para inserção.
* Se Localizar retornar uma entidade, ela existirá no banco de dados e o contexto agora é controlar a entidade existente
  * Em seguida, usamos SetValues para definir os valores de todas as propriedades nessa entidade para os estados que vieram do cliente.
  * A chamada SetValues marcará a entidade para ser atualizada conforme o necessário.

> [!TIP]  
> SetValues somente marcará como modificadas as propriedades que têm valores diferentes para aqueles na entidade controlada. Isso significa que, quando a atualização é enviada, somente as colunas que realmente foram alteradas serão atualizadas. (E, se nada foi alterado, nenhuma atualização será enviada).

## <a name="working-with-graphs"></a>Como trabalhar com gráficos

### <a name="identity-resolution"></a>Resolução de identidade

Conforme observado acima, o EF Core só pode controlar uma instância de qualquer entidade com um determinado valor de chave primária. Ao trabalhar com elementos gráficos, o gráfico deve ser criado idealmente de modo que essa constante seja mantida e o contexto deve ser usado para apenas uma unidade de trabalho. Se o gráfico contiver duplicatas, será necessário processar o gráfico antes de enviá-lo ao EF para consolidar várias instâncias em uma. Isso pode não ser trivial onde as instâncias têm valores e relações conflitantes, de modo que consolidar duplicatas deve ser feito assim que possível em seu pipeline de aplicativo para evitar a resolução de conflitos.

### <a name="all-newall-existing-entities"></a>Todas as entidades novas ou existentes

Um exemplo de como trabalhar com elementos gráficos é inserir ou atualizar um blog junto com sua coleção de postagens associadas. Se todas as entidades no gráfico tiverem que ser inseridas, ou todas tiverem que ser atualizadas, o processo será o mesmo descrito acima para entidades únicas. Por exemplo, um gráfico de blogs e postagens criados desta forma:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

pode ser inserido assim:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

A chamada para adicionar marcará o blog e todas as postagens a serem inseridas.

Da mesma forma, se todas as entidades em um gráfico precisarem ser atualizados, atualização pode ser usado:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

O blog e todas as suas postagens serão marcados para serem atualizados.

### <a name="mix-of-new-and-existing-entities"></a>Combinação de entidades novas e existentes

Com as chaves geradas automaticamente, a atualização pode novamente ser usada para inserções e atualizações, mesmo se o gráfico contiver uma mistura de entidades que exigem inserção e as que precisam de atualização:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

A atualização marcará qualquer entidade no gráfico, blog ou postagem para inserção se não tiver um conjunto de valores de chave, enquanto todas as outras entidades estejam marcadas para atualização.

Como antes, quando não estiver usando as chaves geradas automaticamente, uma consulta e algum processamento poderão ser usados:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Tratamento de exclusões

A exclusão pode ser complicada de lidar porque geralmente a ausência de uma entidade significa que ela deve ser excluída. Uma maneira de lidar com isso é usar "exclusões a quente", de modo que a entidade seja marcada como excluída, em vez de ser excluída de fato. Exclui e, em seguida, torna-se o mesmo que as atualizações. As exclusões a quente podem ser implementadas usando [filtros de consulta](xref:core/querying/filters).

Para exclusões verdadeiras, um padrão comum é usar uma extensão do padrão de consulta para executar o que é essencialmente uma diferença de gráfico. Por exemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Internamente, adicionar, anexar e atualizar usam percurso de gráfico com uma determinação feita para cada entidade como se ela deve ser marcada como adicionada (para inserir), modificada (para atualizar), inalterada (não fazer nada) ou excluída (para excluir). Esse mecanismo é exposto por meio da API TrackGraph. Por exemplo, vamos supor que, quando o cliente envia de volta um gráfico de entidades, ele define alguns sinalizadores em cada entidade indicando como ela deve ser tratada. O TrackGraph pode ser usado para processar esse sinalizador:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Os sinalizadores são mostrados apenas como parte da entidade para manter a simplicidade do exemplo. Normalmente, os sinalizadores devem fazer parte de um DTO ou algum outro estado incluído na solicitação.
