---
title: Manipulando conflitos de simultaneidade - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Manipulando conflitos de simultaneidade

> [!NOTE]
> Esta página documenta como simultaneidade funciona no núcleo do EF e como lidar com conflitos de simultaneidade em seu aplicativo. Consulte [Tokens de simultaneidade](xref:core/modeling/concurrency) para obter detalhes sobre como configurar os tokens de simultaneidade em seu modelo.

> [!TIP]
> Veja o [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) deste artigo no GitHub.

_Simultaneidade de banco de dados_ refere-se a situações em que vários processos ou usuários acessarem ou alterem os mesmos dados em um banco de dados ao mesmo tempo. _Controle de simultaneidade_ refere-se aos mecanismos específicos usados para garantir a consistência de dados na presença de alterações simultâneas.

Implementa Core EF _controle de simultaneidade otimista_, que significa que ela permitirá que vários processos ou usuários alterarem independentemente, sem a sobrecarga de sincronização ou de bloqueio. Em uma situação ideal, essas alterações não irá interferir com o outro e, portanto, poderá ser bem-sucedida. Na pior das hipóteses, dois ou mais processos tentará fazer alterações em conflito e apenas um deles deve ter êxito.

## <a name="how-concurrency-control-works-in-ef-core"></a>Como funciona o controle de simultaneidade no núcleo do EF

As propriedades configuradas como tokens de simultaneidade são usados para implementar o controle de simultaneidade otimista: sempre que uma operação de atualização ou exclusão é executada durante a `SaveChanges`, o valor do token de simultaneidade no banco de dados é comparado com o original valor lido por núcleo EF.

- Se os valores coincidirem, a operação pode ser concluída.
- Se os valores não coincidirem, o núcleo EF presume que outro usuário executou uma operação conflitante e anula a transação atual.

A situação em que outro usuário executou uma operação que está em conflito com a operação atual é conhecida como _conflito de simultaneidade_.

Provedores de banco de dados serão responsáveis por implementar a comparação de valores de token de simultaneidade.

Em bancos de dados relacionais EF Core inclui uma verificação para o valor do token de simultaneidade no `WHERE` cláusula de qualquer `UPDATE` ou `DELETE` instruções. Depois de executar as instruções, Core EF lê o número de linhas afetadas.

Se nenhuma linha for afetada, será detectado um conflito de simultaneidade e EF Core lança `DbUpdateConcurrencyException`.

Por exemplo, é aconselhável configurar `LastName` em `Person` um token de simultaneidade. Qualquer operação de atualização na pessoa incluirá a verificação de simultaneidade no `WHERE` cláusula:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Resolvendo conflitos de simultaneidade

Continuando com o exemplo anterior, se um usuário tenta salvar algumas alterações em um `Person`, mas outro usuário já foi alterado o `LastName` a uma exceção será lançada.

Neste ponto, o aplicativo simplesmente pode informar ao usuário que a atualização não teve êxito devido a alterações conflitantes e Avançar. Mas, talvez seja conveniente para solicitar ao usuário para garantir que este registro ainda representa a mesma pessoa real e tente a operação novamente.

Esse processo é um exemplo de _resolver um conflito de simultaneidade_.

Resolver um conflito de simultaneidade mesclando as alterações pendentes do atual `DbContext` com os valores no banco de dados. Quais valores são mesclados irão variar com base no aplicativo e podem ser direcionado pela entrada do usuário.

**Há três conjuntos de valores disponíveis para ajudar a resolver um conflito de simultaneidade:**

* **Valores atuais** são os valores que o aplicativo estava tentando gravar no banco de dados.

* **Valores originais** são os valores que foram originalmente recuperados do banco de dados, antes de todas as edições feitas.

* **Valores de banco de dados** são os valores atualmente armazenados no banco de dados.

A abordagem geral para lidar com um conflito de simultaneidade é:

1. Catch `DbUpdateConcurrencyException` durante `SaveChanges`.
2. Use `DbUpdateConcurrencyException.Entries` para preparar um novo conjunto de alterações para as entidades afetadas.
3. Atualize os valores originais do token de simultaneidade para refletir os valores atuais no banco de dados.
4. Repita o processo até que nenhum conflito ocorre.

No exemplo a seguir, `Person.FirstName` e `Person.LastName` são configurados como tokens de simultaneidade. Há um `// TODO:` comentário no local onde você incluir lógica específica do aplicativo para escolher o valor a ser salvo.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
