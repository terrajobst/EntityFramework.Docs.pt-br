---
title: Testar componentes usando o EF Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655794"
---
# <a name="testing-components-using-ef-core"></a>Testar componentes usando o EF Core

Você talvez queira testar componentes usando algo que se aproxime da conexão ao banco de dados real sem a sobrecarga de operações de E/S de banco de dados reais.

Há duas opções principais para fazer isso:

* O [Modo em memória do SQLite](sqlite.md) permite escrever testes eficientes para um provedor que se comporte como um banco de dados relacional.
* [O provedor InMemory](in-memory.md) é um provedor leve que tem dependências mínimas, mas nem sempre se comporta como um banco de dados relacional.
