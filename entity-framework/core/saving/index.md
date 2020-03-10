---
title: Salvar Dados – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413091"
---
# <a name="saving-data"></a>Salvar Dados

Cada instância de contexto tem um `ChangeTracker` que é responsável por manter o controle de alterações que precisam ser gravadas no banco de dados. Conforme você faz alterações a instâncias das suas classes de entidade, essas alterações são gravadas no `ChangeTracker` e então gravadas no banco de dados quando você chama `SaveChanges`. O provedor de banco de dados é responsável por converter as alterações em operações específicas do banco de dados (por exemplo, comandos `INSERT`, `UPDATE` e `DELETE` de um banco de dados relacional).
