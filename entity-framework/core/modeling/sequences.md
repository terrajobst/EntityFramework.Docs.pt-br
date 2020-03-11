---
title: Sequências-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417436"
---
# <a name="sequences"></a><span data-ttu-id="ba7e1-102">Sequências</span><span class="sxs-lookup"><span data-stu-id="ba7e1-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="ba7e1-103">As sequências são um recurso normalmente suportado apenas por bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="ba7e1-104">Se você estiver usando um banco de dados não relacional, como Cosmos, verifique a documentação do banco de dados ao gerar valores exclusivos.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="ba7e1-105">Uma sequência gera valores numéricos únicos e sequenciais no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="ba7e1-106">As sequências não são associadas a uma tabela específica, e várias tabelas podem ser configuradas para desenhar valores da mesma sequência.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="ba7e1-107">Uso básico</span><span class="sxs-lookup"><span data-stu-id="ba7e1-107">Basic usage</span></span>

<span data-ttu-id="ba7e1-108">Você pode configurar uma sequência no modelo e, em seguida, usá-la para gerar valores para propriedades:</span><span class="sxs-lookup"><span data-stu-id="ba7e1-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="ba7e1-109">Observe que o SQL específico usado para gerar um valor de uma sequência é específico ao banco de dados; o exemplo acima funciona em SQL Server, mas falhará em outros bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="ba7e1-110">Consulte a documentação do banco de dados específico para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="ba7e1-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="ba7e1-111">Definindo configurações de sequência</span><span class="sxs-lookup"><span data-stu-id="ba7e1-111">Configuring sequence settings</span></span>

<span data-ttu-id="ba7e1-112">Você também pode configurar aspectos adicionais da sequência, como seu esquema, valor inicial, incremento, etc.:</span><span class="sxs-lookup"><span data-stu-id="ba7e1-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
