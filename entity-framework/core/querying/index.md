---
title: Consultar Dados – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048878"
---
# <a name="querying-data"></a>Consultar Dados

O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados. O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade. Uma representação da consulta LINQ é passada para o provedor de banco de dados, para ser convertida em linguagem de consulta específica do banco de dados (por exemplo, SQL para um banco de dados relacional). Para obter mais informações sobre como uma consulta é processada, consulte [Como funciona a consulta](overview.md).
