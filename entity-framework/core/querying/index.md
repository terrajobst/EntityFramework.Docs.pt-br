---
title: Consultar Dados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993528"
---
# <a name="querying-data"></a>Consultar Dados

O Entity Framework Core usa LINQ (Consulta Integrada à Linguagem) para consultar dados do banco de dados. O LINQ permite que você use C# (ou a linguagem .NET de sua escolha) para escrever consultas fortemente tipadas com base em suas classes derivadas de contexto e entidade. Uma representação da consulta LINQ é passada para o provedor de banco de dados, para ser convertida em linguagem de consulta específica do banco de dados (por exemplo, SQL para um banco de dados relacional). Para obter mais informações sobre como uma consulta é processada, consulte [Como funciona a consulta](overview.md).
