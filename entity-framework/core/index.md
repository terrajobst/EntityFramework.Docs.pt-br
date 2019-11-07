---
title: Visão geral do Entity Framework Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655615"
---
# <a name="entity-framework-core"></a>Entity Framework Core

O EF (Entity Framework) Core é uma versão leve, extensível, de [software livre](https://github.com/aspnet/EntityFrameworkCore) e multiplataforma da popular tecnologia de acesso a dados do Entity Framework.

O EF Core pode servir como um ORM (Mapeador de Objeto Relacional), permitindo que os desenvolvedores de .NET trabalhem com um banco de dados usando objetos do .NET e eliminando a necessidade de grande parte do código de acesso aos dados que eles geralmente precisam escrever.

O EF Core é compatível com vários mecanismos de banco de dados, consulte detalhes em [Provedores de Banco de Dados](providers/index.md).

## <a name="the-model"></a>O modelo

Com o EF Core, o acesso a dados é executado usando um modelo. Um modelo é composto por classes de entidade e um objeto de contexto que representa uma sessão com o banco de dados, o que permite consultar e salve dados. Consulte [Criar um modelo](modeling/index.md) para saber mais.

Você pode gerar um modelo de um banco de dados existente, codificar manualmente um modelo para coincidir com o banco de dados ou usar [migrações da classe EF](managing-schemas/migrations/index.md) para criar um banco de dados do modelo e, em seguida, desenvolvê-lo à medida que o modelo for alterado com o tempo.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Consultar

Instâncias de suas classes de entidade são recuperadas do banco de dados usando a LINQ (Consulta Integrada à Linguagem). Consulte [Consultar dados](querying/index.md) para saber mais.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Salvar Dados

Dados são criados, excluídos e modificados no banco de dados usando as instâncias de suas classes de entidade. Consulte [Salvar dados](saving/index.md) para saber mais.

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Próximas etapas

Para ver tutoriais introdutórios, confira [Introdução ao Entity Framework Core](get-started/index.md).
