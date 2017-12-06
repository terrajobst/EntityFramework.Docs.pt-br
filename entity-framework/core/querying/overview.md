---
title: Como as consultas trabalho - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Como funcionam as consultas

Entity Framework Core usa linguagem integrar LINQ (consulta) para consultar dados do banco de dados. LINQ permite que você use c# (ou .NET idioma de sua escolha) para escrever consultas com rigidez de tipos com base em suas classes derivadas de contexto e a entidade.

## <a name="the-life-of-a-query"></a>A vida útil de uma consulta

A seguir está uma visão geral de alto nível do processo de que cada consulta atravessa.

1. A consulta LINQ é processada pelo Entity Framework Core para criar uma representação que está pronta para ser processado pelo provedor de banco de dados
   1. O resultado é armazenado em cache para que esse processamento não precisa ser feito sempre que a consulta é executada
2. O resultado é passado para o provedor de banco de dados
   1. O provedor de banco de dados identifica quais partes da consulta podem ser avaliadas no banco de dados
   2. Essas partes da consulta são convertidos em linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional)
   3. Uma ou mais consultas são enviadas para o banco de dados e o conjunto de resultados retornado (resultados são valores do banco de dados, não as instâncias de entidade)
3. Para cada item no conjunto de resultados
   1. Se esse for um rastreamento de consulta, EF verifica se os dados representam uma entidade já no controlador de alterações para a instância de contexto
      * Nesse caso, a entidade existente será retornada
      * Caso contrário, uma nova entidade é criada, o controle de alterações estão configurado e a nova entidade for retornada
   2. Se essa é uma consulta de acompanhamento não, EF verifica se os dados representam uma entidade que já estão no conjunto de resultados para esta consulta
      * Se assim, a entidade existente for retornada <sup>(1)</sup>
      * Caso contrário, uma nova entidade é criada e retornada

<sup>(1) </sup> Consultas sem controle usam referências fracas para manter o controle de entidades que já foram retornadas. Se um resultado anterior com a mesma identidade sai do escopo e coleta de lixo é executado, você pode receber uma nova instância da entidade.

## <a name="when-queries-are-executed"></a>Quando as consultas são executadas

Quando você chama operadores LINQ, você está criando simplesmente uma representação na memória da consulta. A consulta é enviada somente para o banco de dados quando os resultados são consumidos.

As operações mais comuns que resultam na consulta que está sendo enviada para o banco de dados são:
* Iteração os resultados em um `for` loop
* Usando um operador como `ToList`, `ToArray`, `Single`,`Count`
* Associação de dados de resultados de uma consulta para uma interface de usuário

> [!WARNING]  
> **Sempre valide a entrada do usuário:** EF enquanto fornecem proteção contra ataques de injeção de SQL, ele não faz nenhuma validação geral de entrada. Portanto, se os valores sejam passados para APIs, usado em consultas LINQ, atribuídas para propriedades de entidade, etc., vêm de uma fonte não confiável, em seguida, validação apropriadas, por seus requisitos de aplicativo deve ser executado. Isso inclui qualquer entrada do usuário usada para construir dinamicamente consultas. Mesmo ao usar o LINQ, se você está aceitando entrada do usuário construir expressões que você precisa garantir que somente as expressões pretendidas pode ser criada.
