---
title: Como funciona a consulta – EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136236"
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
   3. Uma consulta é enviada para o banco de dados e o conjunto de resultados retornado (os resultados são valores do banco de dados, não instâncias da entidade)
3. Para cada item no conjunto de resultados
   1. Se for um rastreamento de consulta, o EF verificará se os dados representam uma entidade já no controlador de alterações para a instância de contexto
      * Nesse caso, a entidade existente será retornada
      * Caso contrário, uma nova entidade será criada, o controle de alterações será configurado e a nova entidade será retornada
   2. Se esta é uma consulta sem rastreamento, então uma nova entidade é sempre criada e devolvida

## <a name="when-queries-are-executed"></a>Quando as consultas são executadas

Quando você chama operadores LINQ, está criando simplesmente uma representação na memória da consulta. A consulta é enviada somente para o banco de dados quando os resultados são consumidos.

As operações mais comuns que resultam na consulta que está sendo enviada para o banco de dados são:

* Iteração dos resultados em um loop `for`
* Usando um operador `ToList` `ToArray`como `Single` `Count` , , ou as sobrecargas assíncronas equivalentes

> [!WARNING]  
> **Sempre validar a entrada do usuário:** enquanto o EF Core protege contra ataques de injeção SQL usando parâmetros e ignorando literais em consultas, ele não valida entradas. A validação apropriada, de acordo com os requisitos do aplicativo, deve ser realizada antes que os valores de fontes não confiáveis sejam usados em consultas LINQ, atribuídos às propriedades da entidade ou passados para outras APIs do Núcleo EF. Isso inclui qualquer entrada do usuário usada para construir consultas dinamicamente. Mesmo ao usar o LINQ, se você está aceitando entrada do usuário para criar expressões, precisa para garantir que apenas as expressões pretendidas possam ser criadas.
