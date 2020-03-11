---
title: Como configurar valores explícitos para propriedades geradas – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 43c4ab3c2a60645cdeff2a6cc40ce979f832f2fd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417565"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Como configurar valores explícitos para propriedades geradas

Uma propriedade gerada é uma propriedade cujo valor é gerado (EF ou banco de dados) quando a entidade é adicionada e/ou atualizada. Veja [Propriedades geradas](../modeling/generated-properties.md) para saber mais.

Pode haver situações em que você deseja definir um valor explícito para uma propriedade gerada, em vez de ter uma gerada.

> [!TIP]  
> Veja o [exemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) deste artigo no GitHub.

## <a name="the-model"></a>O modelo

O modelo usado neste artigo contém uma única entidade `Employee`.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Salvar um valor explícito durante adicionar

A propriedade `Employee.EmploymentStarted` está configurada para ter valores gerados pelo banco de dados para novas entidades (usando um valor padrão).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

O código a seguir insere dois funcionários no banco de dados.

* Para a primeira, nenhum valor é atribuído à propriedade `Employee.EmploymentStarted`; portanto, ele permanece definido como o valor padrão CLR para `DateTime`.
* Para o segundo, configuramos um valor explícito de `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

A saída mostra que o banco de dados gerou um valor para o primeiro funcionário e nosso valor explícito foi usado para o segundo.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Valores explícitos em colunas de IDENTITY do SQL Server

Por convenção a propriedade `Employee.EmployeeId` é uma coluna `IDENTITY` gerada pelo repositório.

Na maioria das situações, a abordagem mostrada acima funcionará para as propriedades chave. No entanto, para inserir valores explícitos em uma coluna `IDENTITY` do SQL Server, é necessário habilitar `IDENTITY_INSERT` manualmente antes de chamar `SaveChanges()`.

> [!NOTE]  
> Temos uma [solicitação de recurso](https://github.com/aspnet/EntityFramework/issues/703) em nossa lista de pendências para fazer isso automaticamente dentro do provedor do SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

A saída mostra que as ids fornecidas foram salvas no banco de dados.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Configurar um valor explícito durante a atualização

A propriedade `Employee.LastPayRaise` está configurada para ter valores gerados pelo banco de dados durante as atualizações.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Por padrão, o EF Core gerará uma exceção se você tentar salvar um valor explícito para uma propriedade que está configurada para ser gerada durante a atualização. Para evitar isso, você precisará passar para a API de metadados de nível inferior e definir o `AfterSaveBehavior` (conforme mostrado acima).

> [!NOTE]  
> **Alterações no EF Core 2.0:** nas versões anteriores, o comportamento de salvar depois foi controlado por meio do sinalizador `IsReadOnlyAfterSave`. Esse sinalizador foi obsoleto e substituído por `AfterSaveBehavior`.

Também há um gatilho no banco de dados para gerar valores para a coluna `LastPayRaise` durante operações `UPDATE`.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

O código a seguir aumenta o salário de dois funcionários no banco de dados.

* Para a primeira, nenhum valor é atribuído à propriedade `Employee.LastPayRaise`; portanto, ele permanece definido como nulo.
* Para o segundo, configuramos um valor explícito de uma semana atrás (antes da data do aumento de salário).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

A saída mostra que o banco de dados gerou um valor para o primeiro funcionário e nosso valor explícito foi usado para o segundo.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
