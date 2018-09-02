---
title: Como funciona a consulta – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: f1c23471bfbc998b2d4f9dc579d1404d6202e109
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993197"
---
# <a name="how-queries-work"></a>Como funciona a consulta

O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados. O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade.

## <a name="the-life-of-a-query"></a>A vida de uma consulta

Veja a seguir uma visão geral de alto nível do processo pelo qual cada consulta passa.

1. A consulta LINQ é processada pelo Entity Framework Core para criar uma representação que está pronta para ser processada pelo provedor de banco de dados
   1. O resultado é armazenado em cache para que esse processamento não precise ser feito sempre que a consulta for executada
2. O resultado é passado para o provedor de banco de dados
   1. O provedor do banco de dados identifica quais partes da consulta podem ser avaliadas no banco de dados
   2. Essas partes da consulta são convertidas em linguagem de consulta específica de banco de dados (por exemplo, SQL para um banco de dados relacional)
   3. Uma ou mais consultas são enviadas para o banco de dados e o conjunto de resultados retornado (resultados são valores do banco de dados, não as instâncias de entidade)
3. Para cada item no conjunto de resultados
   1. Se for um rastreamento de consulta, o EF verificará se os dados representam uma entidade já no controlador de alterações para a instância de contexto
      * Nesse caso, a entidade existente será retornada
      * Caso contrário, uma nova entidade será criada, o controle de alterações será configurado e a nova entidade será retornada
   2. Se for uma consulta sem rastreamento, o EF verificará se os dados representam uma entidade já no conjunto de resultados para essa consulta
      * Nesse caso, a entidade existente será retornada <sup>(1)</sup>
      * Caso contrário, uma nova entidade será criada e retornada

<sup>(1)</sup> As consultas sem acompanhamento usam referências fracas para controlar as entidades que já foram retornadas. Se um resultado anterior com a mesma identidade sai do escopo e a coleta de lixo é executada, você poderá receber uma nova instância da entidade.

## <a name="when-queries-are-executed"></a>Quando as consultas são executadas

Quando você chama operadores LINQ, está criando simplesmente uma representação na memória da consulta. A consulta é enviada somente para o banco de dados quando os resultados são consumidos.

As operações mais comuns que resultam na consulta que está sendo enviada para o banco de dados são:
* Iteração dos resultados em um loop `for`
* Como usar um operador como `ToList`, `ToArray`, `Single`, `Count`
* Associação de dados de resultados de uma consulta para uma interface de usuário

> [!WARNING]  
> **Sempre valide a entrada do usuário:** embora o EF forneça proteção contra ataques de injeção de SQL, ele não faz nenhuma validação geral de entrada. Portanto, se os valores forem passados para as APIs, usados em consultas LINQ, atribuídos para propriedades de entidade etc., vindos de uma fonte não confiável, a validação apropriada, de acordo com os requisitos do aplicativo, deverá ser executada. Isso inclui qualquer entrada do usuário usada para construir consultas dinamicamente. Mesmo ao usar o LINQ, se você está aceitando entrada do usuário para criar expressões, precisa para garantir que apenas as expressões pretendidas possam ser criadas.
