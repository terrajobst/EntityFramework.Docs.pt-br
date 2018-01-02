---
title: "Provedores de Banco de Dados – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Provedores de Banco de Dados

O Entity Framework Core usa um modelo de provedor para permitir que o EF seja usado para acessar vários bancos de dados diferentes. Alguns conceitos são comuns à maioria dos bancos de dados e estão incluídos nos componentes principais do EF Core. Esses conceitos incluem expressar consultas no LINQ e em transações e lidar com alterações aos objetos depois que eles forem carregados do banco de dados. Alguns conceitos são específicos para um determinado provedor. Por exemplo, o provedor SQL Server permite configurar as tabelas com otimização de memória (um recurso específico do SQL Server). Outros conceitos são específicos de uma classe de provedores. Por exemplo, provedores do EF Core para bancos de dados relacionais são desenvolvidos com base na biblioteca `Microsoft.EntityFrameworkCore.Relational` comum, que fornece APIs para configurar mapeamentos de tabela e coluna, restrições de chave estrangeira etc.

Os provedores do EF Core são criados por uma variedade de origens. Nem todos os provedores são mantidos como parte do projeto do Entity Framework Core. Ao considerar um provedor de terceiros, avalie a qualidade, o licenciamento, o suporte etc. para garantir que ele cumpra os requisitos.
