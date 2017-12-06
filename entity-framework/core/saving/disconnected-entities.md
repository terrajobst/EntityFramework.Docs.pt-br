---
title: Entidades desconectadas - Core EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a>Entidades desconectadas

Uma instância de DbContext automaticamente controlará entidades retornadas do banco de dados. As alterações feitas a essas entidades, em seguida, serão detectadas quando SaveChanges é chamado e o banco de dados será atualizado conforme necessário. Consulte [salvar básico](basic.md) e [dados relacionados](related-data.md) para obter detalhes.

No entanto, às vezes, entidades são consultadas usando uma instância de contexto e, em seguida, salva usando uma instância diferente. Isso geralmente ocorre em cenários "desconectados" como um aplicativo da web onde as entidades são consultadas, enviadas ao cliente, modificadas, enviadas de volta para o servidor em uma solicitação e salvos. Nesse caso, o contexto da segunda instância necessidades saber se as entidades são novo (deve ser inserido) ou existente (deve ser atualizado).

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) no GitHub.

## <a name="identifying-new-entities"></a>Identificar novas entidades

### <a name="client-identifies-new-entities"></a>Cliente identifica novas entidades

O caso mais simples para lidar com é quando o cliente informa ao servidor se a entidade é novo ou existente. Por exemplo, geralmente, a solicitação para inserir uma nova entidade é diferente da solicitação de atualização de uma entidade existente.

O restante desta seção aborda os casos onde necessário determinar se deseja inserir ou atualizar de alguma outra maneira.

### <a name="with-auto-generated-keys"></a>Com as chaves geradas automaticamente

O valor de uma chave gerada automaticamente geralmente pode ser usado para determinar se uma entidade precisa ser inserida ou atualizada. Se a chave não tiver sido definido (ou seja, ele ainda tem o valor padrão CLR de null, zero, etc.), em seguida, a entidade deve ser nova e precisa inserir. Por outro lado, se o valor da chave tiver sido definido, em seguida, ele deve ter já foi salvo anteriormente e agora precisa ser atualizado. Em outras palavras, se a chave tem um valor, em seguida, a entidade foi consultada, enviada para o cliente e tem agora volte a ser atualizado.

É fácil verificar se há uma chave não definida quando o tipo de entidade é desconhecido:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

No entanto, o EF também tem uma forma interna de fazer isso para qualquer tipo de entidade e o tipo de chave:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> As chaves são definidas como entidades são controladas pelo contexto, mesmo se a entidade está em estado adicionado. Isso ajuda ao passar um gráfico de entidades e decidir o que fazer com cada, tais como ao usar a API TrackGraph. O valor da chave deve ser usado somente da forma mostrada aqui _antes de_ é feita nenhuma chamada para controlar a entidade.

### <a name="with-other-keys"></a>Com outras chaves

Outro mecanismo é necessário para novas entidades de identidade quando os valores de chave não são gerados automaticamente. Há duas abordagens gerais para isso:
 * Consulta para a entidade
 * Passar um sinalizador do cliente

Para consultar para a entidade, use apenas o método de localização:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Está além do escopo deste documento para mostrar o código completo para transmitir um sinalizador de um cliente. Em um aplicativo web, geralmente isso significa fazer solicitações diferentes para diferentes ações, ou passando algum estado na solicitação, extraindo-o no controlador.

## <a name="saving-single-entities"></a>Salvando entidades simples

Se ele for conhecido ou não uma inserção ou atualização é necessária, em seguida, adicionar ou atualizar pode ser usado de forma apropriada:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

No entanto, se a entidade usa valores de chave gerada automaticamente, o método de atualização pode ser usado para ambos os casos:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

O método de atualização normalmente marca da entidade para a atualização, inserção não. No entanto, se a entidade tem uma chave gerada automaticamente e nenhum valor de chave foi definida, em seguida, em vez disso, a entidade é automaticamente marcada para inserir.

> [!TIP]  
> Esse comportamento foi introduzido no EF Core 2.0. Para versões anteriores sempre é necessário escolher explicitamente adicionar ou atualizar.

Se a entidade não está usando as chaves geradas automaticamente, o aplicativo deve decidir se a entidade deve ser inserida ou atualizada: por exemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

As etapas aqui são:
* Se adicionar localizar retorna null, e o banco de dados ainda não contiver o blog com essa ID, portanto, podemos chamar marcá-la para inserção.
* Se encontrar retorna uma entidade, em seguida, ele existe no banco de dados e o contexto agora está controlando a entidade existente
  * Em seguida, usamos o SetValues para definir os valores de todas as propriedades nessa entidade para os estados do cliente.
  * A chamada SetValues marcará a entidade a ser atualizada conforme necessário.

> [!TIP]  
> SetValues somente marcará como modificou as propriedades que têm valores diferentes para aqueles na entidade controlada. Isso significa que, quando a atualização é enviada, somente as colunas que realmente foram alterados sejam atualizadas. (E se nada foi alterado, em seguida, nenhuma atualização será enviada em todos os).

## <a name="working-with-graphs"></a>Trabalhando com gráficos

### <a name="all-newall-existing-entities"></a>Todas as entidades existentes do novo/all

Um exemplo de como trabalhar com elementos gráficos é inserir ou atualizar um blog junto com sua coleção de postagens associadas. Se todas as entidades no gráfico devem ser inseridas, ou todos devem ser atualizados, o processo é o mesmo descrito acima para entidades únicas. Por exemplo, um gráfico de blogs e postagens criadas desta forma:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

pode ser inserido como este:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

A chamada para adicionar marcará o blog e postagens a ser inserido.

Da mesma forma, se todas as entidades em um gráfico que precisam ser atualizados, em seguida, atualização pode ser usada:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

O blog e todas as suas mensagens serão marcadas para serem atualizados.

### <a name="mix-of-new-and-existing-entities"></a>Combinação de entidades novas e existentes

Com as chaves geradas automaticamente, atualização pode novamente ser usada para inserções e atualizações, mesmo que o gráfico contém uma mistura de entidades que exigem inserção e aqueles que precisam de atualização:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Atualização marcará a qualquer entidade no gráfico, blog ou post para inserção se não tiver um conjunto de valores de chave, enquanto todas as outras entidades são marcadas para atualização.

Como antes, quando não estiver usando as chaves geradas automaticamente, uma consulta e algum processamento podem ser usados:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Tratamento de exclusões

Exclusão pode ser complicada para tratar desde geralmente a ausência de uma entidade significa que deve ser excluído. Uma maneira de lidar com isso é usar "exclusões a quente", de modo que a entidade está marcada como excluído, em vez de, na verdade, está sendo excluído. Exclui e torna-se o mesmo que as atualizações. Exclusões a quente podem ser implementadas usando [filtros de consulta](xref:core/querying/filters).

Para exclusões true, um padrão comum é usar uma extensão do padrão de consulta para executar o que é essencialmente uma diferença de gráfico. Por exemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Internamente, adicionar, anexar e atualização usam percurso de gráfico com uma determinação feita para cada entidade como se ele deve ser marcado como adicionado (para inserir), modificado (para atualizar), inalterado (não fazer nada), ou excluídos (para excluir). Esse mecanismo é exposto por meio da API TrackGraph. Por exemplo, vamos supor que, quando o cliente envia de volta um gráfico de entidades ele define algumas sinalizador em cada entidade que indica como devem ser tratada. TrackGraph pode ser usado para processar esse sinalizador:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Os sinalizadores são mostrados apenas como parte da entidade para manter a simplicidade do exemplo. Normalmente, os sinalizadores seria parte de um DTO ou algum outro estado incluído na solicitação.
