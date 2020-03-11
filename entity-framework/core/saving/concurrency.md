---
title: Como tratar conflitos de simultaneidade – EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417586"
---
# <a name="handling-concurrency-conflicts"></a>Como tratar conflitos de simultaneidade

> [!NOTE]
> Esta página documenta como a simultaneidade funciona no EF Core e como lidar com conflitos de simultaneidade no seu aplicativo. Confira [Tokens de Simultaneidade](xref:core/modeling/concurrency) para obter detalhes sobre como configurar os tokens de simultaneidade no seu modelo.

> [!TIP]
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) deste artigo no GitHub.

_Simultaneidade do banco de dados_ se refere a situações nas quais vários processos ou usuários acessam ou alteram os mesmos dados em um banco de dados ao mesmo tempo. _Controle de simultaneidade_ se refere a mecanismos específicos usados para garantir a consistência dos dados na presença de alterações simultâneas.

O EF Core implementa o _controle de simultaneidade otimista_, o que significa que ele permitirá que vários processos ou usuários façam alterações de forma independente, sem a sobrecarga da sincronização ou bloqueio. Em uma situação ideal, essas alterações não irão interferir umas nas outras e elas poderão ser bem-sucedidas. Na pior das hipóteses, dois ou mais processos tentarão fazer alterações conflitantes e apenas uma delas terá êxito.

## <a name="how-concurrency-control-works-in-ef-core"></a>Como funciona o controle de simultaneidade no EF Core

As propriedades configuradas como tokens de simultaneidade são usadas para implementar um controle de simultaneidade otimista: sempre que uma operação de atualização ou exclusão é realizada durante `SaveChanges`, o valor do token de simultaneidade no banco de dados é comparado ao valor original lido pelo EF Core.

- Se os valores coincidirem, a operação pode ser concluída.
- Se os valores não coincidirem, o EF Core presume que outro usuário executou uma operação conflitante e anula a transação atual.

A situação quando outro usuário executou uma operação que conflita com a operação atual é conhecida como _conflito de simultaneidade_.

Os provedores do banco de dados são responsáveis por implementar a comparação dos valores do token de simultaneidade.

Em bancos de dados relacionais, o EF Core inclui uma verificação do valor do token de simultaneidade na cláusula `WHERE` de qualquer instrução `UPDATE` ou `DELETE`. Depois de executar as instruções, o EF Core lê o número de linhas que foram afetadas.

Se nenhuma linha for afetada, um conflito de simultaneidade será detectado e o EF Core gera `DbUpdateConcurrencyException`.

Por exemplo, é aconselhável configurar `LastName` em `Person` como um token de simultaneidade. Em seguida, qualquer operação de atualização em Pessoa incluirá a verificação de simultaneidade na cláusula `WHERE`:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Como resolver conflitos de simultaneidade

Continuando com o exemplo anterior, se um usuário tentar salvar algumas alterações em um `Person`, mas outro usuário já tiver alterado o `LastName`, uma exceção será gerada.

Neste ponto, o aplicativo pode simplesmente informar ao usuário que a atualização não teve êxito devido a alterações conflitantes e avançar. Mas, talvez seja desejável consultar o usuário para garantir que este registro represente a mesma pessoa e repetir a operação.

Este processo é um exemplo de como _resolver um conflito de simultaneidade_.

Resolver um conflito de simultaneidade envolve mesclar as alterações pendentes a partir do `DbContext` atual com os valores no banco de dados. Os valores que serão mesclados irão variar com base no aplicativo e poderão ser direcionados pela entrada do usuário.

**Há três conjuntos de valores disponíveis para ajudar a resolver um conflito de simultaneidade:**

- **Valores atuais** são os valores que o aplicativo estava tentando gravar no banco de dados.
- **Valores originais** são os valores que foram originalmente recuperados do banco de dados, antes de todas as edições feitas.
- **Valores do banco de dados** são os valores armazenados atualmente no banco de dados.

A abordagem geral para lidar com um conflito de simultaneidade é:

1. Capturar `DbUpdateConcurrencyException` durante `SaveChanges`.
2. Use `DbUpdateConcurrencyException.Entries` para preparar um novo conjunto de alterações para as entidades afetadas.
3. Atualize os valores originais do token de simultaneidade para refletir os valores atuais no banco de dados.
4. Tente novamente o processo até o conflito ocorrer.

No exemplo a seguir, `Person.FirstName` e `Person.LastName` são configurados como tokens de simultaneidade. Há um comentário `// TODO:` no local onde você inclui a lógica específica para o aplicativo para escolher o valor a ser salvo.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
