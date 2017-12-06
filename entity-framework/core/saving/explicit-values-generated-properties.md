---
title: "Definindo valores explícitos para propriedades geradas - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Definir valores explícitos para as propriedades geradas

Uma propriedade gerada é uma propriedade cujo valor é gerado (o EF ou o banco de dados) quando a entidade é adicionada e/ou atualizada. Consulte [propriedades gerado](../modeling/generated-properties.md) para obter mais informações.

Pode haver situações em que você deseja definir um valor explícito para uma propriedade gerada, em vez de ter gerado.

> [!TIP]  
> Você pode exibir este artigo [exemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) no GitHub.

## <a name="the-model"></a>O modelo

O modelo usado neste artigo contém uma única `Employee` entidade.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Salvar um valor explícito durante adicionar

O `Employee.EmploymentStarted` está configurado para ter valores gerados pelo banco de dados para novas entidades (usando um valor padrão).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

O código a seguir insere dois funcionários no banco de dados.
* Para a primeira, nenhum valor é atribuído a `Employee.EmploymentStarted` propriedade, portanto, ele permanece definido como o valor padrão CLR `DateTime`.
* Para o segundo, configuramos um valor explícito de `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Saída mostra que o banco de dados gerou um valor para o primeiro funcionário e nosso valor explícito foi usado para a segunda.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Valores explícitos em colunas de identidade do SQL Server

Por convenção o `Employee.EmployeeId` propriedade é um repositório gerado `IDENTITY` coluna.

Na maioria das situações, a abordagem mostrada acima funcionará para propriedades de chave. No entanto, para inserir valores explícitos em um SQL Server `IDENTITY` coluna, é necessário habilitar manualmente `IDENTITY_INSERT` antes de chamar `SaveChanges()`.

> [!NOTE]  
> Temos um [solicitação de recurso](https://github.com/aspnet/EntityFramework/issues/703) em nossa lista de pendências para fazer isso automaticamente dentro do provedor SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Saída mostra que as ids fornecidas foram salvos no banco de dados.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Definir um valor explícito durante a atualização

O `Employee.LastPayRaise` está configurado para ter valores gerados pelo banco de dados durante as atualizações.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Por padrão, o núcleo de EF lançará uma exceção se você tentar salvar um valor explícito para uma propriedade que está configurado para ser gerado durante a atualização. Para evitar isso, você precisa passar para os metadados de nível inferior API e definir o `AfterSaveBehavior` (conforme mostrado acima).

> [!NOTE]  
> **Alterações no EF Core 2.0:** nas versões anteriores, o comportamento de salvar After foi controlado por meio de `IsReadOnlyAfterSave` sinalizador. Esse sinalizador foi obsoleto e substituído por `AfterSaveBehavior`.

Também há um gatilho no banco de dados para gerar valores para o `LastPayRaise` coluna durante `UPDATE` operações.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

O código a seguir aumenta o salário de dois funcionários no banco de dados.
* Para o primeiro, nenhum valor é atribuído à `Employee.LastPayRaise` propriedade, portanto, ele permanece definido como null.
* Para o segundo, configuramos um valor explícito de uma semana atrás (elevar back desde o pagamento).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Saída mostra que o banco de dados gerou um valor para o primeiro funcionário e nosso valor explícito foi usado para a segunda.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
