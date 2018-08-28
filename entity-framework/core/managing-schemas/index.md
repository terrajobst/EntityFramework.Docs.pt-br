---
title: Gerenciar Esquemas de Banco de Dados – EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994379"
---
# <a name="managing-database-schemas"></a>Gerenciar Esquemas de Banco de Dados
O EF Core oferece duas maneiras principais de manter seu esquema de banco de dados e modelo do EF Core em sincronia. Para escolher entre as duas, decida se seu modelo do EF Core ou o esquema de banco de dados é a fonte da verdade.

Se quiser que seu modelo do EF Core seja a fonte da verdade, use [Migrações][1]. Uma vez que você pode fazer alterações ao seu modelo do EF Core, essa abordagem aplica incrementalmente as alterações de esquema correspondentes ao banco de dados de modo que permaneçam compatíveis com o modelo do EF Core.

Use [Engenharia Reversa][2] se quiser que o esquema de banco de dados seja a fonte da verdade. Essa abordagem permite que você faça o scaffold de um DbContext e das classes de tipo de entidade realizando engenharia reversa do esquema de banco de dados para um modelo do EF Core.

> [!NOTE]
> A opção [criar e remover APIs][3] também pode criar o esquema de banco de dados usando seu modelo do EF Core. No entanto, eles são principalmente para teste, criação de protótipos e outros cenários em que remover o banco de dados é aceitável.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
