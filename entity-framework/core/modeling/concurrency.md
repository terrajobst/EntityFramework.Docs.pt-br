---
title: Tokens de simultaneidade - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidade

Se uma propriedade é configurada como um token de simultaneidade EF irá verificar se nenhum outro usuário modificou esse valor no banco de dados ao salvar alterações no registro. EF usa um padrão de simultaneidade otimista, que significa que ela será assumem que o valor tenha sido alterada e tentar salvar os dados, mas geram se encontra que o valor foi alterado.

Por exemplo, talvez você queira configurar `LastName` em `Person` um token de simultaneidade. Isso significa que, se um usuário tenta salvar algumas alterações em um `Person`, mas outro usuário alterou o `LastName` , em seguida, uma exceção será lançada. Isso pode ser desejável para que seu aplicativo pode solicitar ao usuário para garantir que este registro ainda representa a mesma pessoa real antes de salvar suas alterações.

> [!NOTE]
> Esta página documenta como configurar tokens de simultaneidade. Consulte [tratamento simultaneidade](../saving/concurrency.md) para obter exemplos de como usar a simultaneidade otimista em seu aplicativo.

## <a name="how-concurrency-tokens-work-in-ef"></a>Como os tokens de simultaneidade funcionam no EF

Armazenamentos de dados podem impor tokens de simultaneidade, verificando se qualquer registro que está sendo atualizada ou excluída ainda tem o mesmo valor para o token de simultaneidade que foi atribuído quando o contexto originalmente carregou os dados do banco de dados.

Por exemplo, os bancos de dados relacionais fazer isso, incluindo o token de simultaneidade no `WHERE` cláusula de qualquer `UPDATE` ou `DELETE` comandos e verificando o número de linhas afetadas. Se o token de simultaneidade ainda corresponde a uma linha será atualizada. Se o valor no banco de dados foi alterada, nenhuma linha é atualizada.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como tokens de simultaneidade.

## <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar uma propriedade como um token de simultaneidade.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versão de carimbo de hora/linha

Um carimbo de hora é uma propriedade em que um novo valor é gerado pelo banco de dados sempre que uma linha é inserida ou atualizada. A propriedade também é tratada como um token de simultaneidade. Isso garante que você receberá uma exceção se ninguém tiver modificado uma linha que você está tentando atualizar desde consultada para os dados.

Como isso é obtido é até o provedor de banco de dados que está sendo usado. Para o SQL Server, carimbo de hora é geralmente usado em uma *byte []* propriedade, que será de instalação como um *ROWVERSION* coluna no banco de dados.

### <a name="conventions"></a>Convenções

Por convenção, as propriedades nunca são configuradas como os carimbos de hora.

### <a name="data-annotations"></a>Anotações de dados

Você pode usar as anotações de dados para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluente

Você pode usar a API fluente para configurar uma propriedade como um carimbo de hora.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
