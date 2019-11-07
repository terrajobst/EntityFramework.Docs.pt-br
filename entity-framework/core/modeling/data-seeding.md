---
title: Propagação de dados-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 0b11b6b3104b74e09c60c9c455e22f164df493c7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655763"
---
# <a name="data-seeding"></a>Propagação de dados

A propagação de dados é o processo de preenchimento de um banco de dados com um conjunto inicial.

Há várias maneiras que isso pode ser feito em EF Core:

* Dados de semente de modelo
* Personalização de migração manual
* Lógica de inicialização personalizada

## <a name="model-seed-data"></a>Dados de semente de modelo

> [!NOTE]
> Este recurso é novo no EF Core 2.1.

Ao contrário de EF6, no EF Core, os dados de propagação podem ser associados a um tipo de entidade como parte da configuração do modelo. Em seguida, EF Core [migrações](xref:core/managing-schemas/migrations/index) podem calcular automaticamente quais operações de inserção, atualização ou exclusão precisam ser aplicadas ao atualizar o banco de dados para uma nova versão do modelo.

> [!NOTE]
> As migrações só consideram alterações de modelo ao determinar qual operação deve ser executada para obter os dados de semente para o estado desejado. Assim, qualquer alteração nos dados executados fora das migrações pode ser perdida ou causar um erro.

Por exemplo, isso configurará os dados semente de um `Blog` no `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para adicionar entidades que têm uma relação, os valores de chave estrangeira precisam ser especificados:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Se o tipo de entidade tiver qualquer propriedade no estado de sombra, uma classe anônima poderá ser usada para fornecer os valores:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Tipos de entidade de propriedade podem ser propagados de maneira semelhante:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Consulte o [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obter mais contexto.

Depois que os dados tiverem sido adicionados ao modelo, as [migrações](xref:core/managing-schemas/migrations/index) deverão ser usadas para aplicar as alterações.

> [!TIP]
> Se você precisar aplicar migrações como parte de uma implantação automatizada, poderá [criar um script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que pode ser visualizado antes da execução.

Como alternativa, você pode usar o `context.Database.EnsureCreated()` para criar um novo banco de dados que contenha a semente, por exemplo, para um banco de dado de teste ou ao usar o provedor na memória ou qualquer banco de dados que não seja de relação. Observe que, se o banco de dados já existir, `EnsureCreated()` não atualizará o esquema nem os dados de semente no banco de dado. Para bancos de dados relacionais, você não deve chamar `EnsureCreated()` se planeja usar migrações.

### <a name="limitations-of-model-seed-data"></a>Limitações dos dados de semente do modelo

Esse tipo de dados de semente é gerenciado por migrações e o script para atualizar os dados que já estão no banco de dados precisa ser gerado sem se conectar ao banco de dado. Isso impõe algumas restrições:

* O valor da chave primária precisa ser especificado mesmo que seja geralmente gerado pelo banco de dados. Ele será usado para detectar alterações de dados entre as migrações.
* Os dados propagados anteriormente serão removidos se a chave primária for alterada de qualquer forma.

Portanto, esse recurso é mais útil para dados estáticos que não devem ser alterados fora das migrações e não dependem de mais nada no banco de dados, por exemplo, CEPs.

Se seu cenário incluir qualquer um dos seguintes, é recomendável usar a lógica de inicialização personalizada descrita na última seção:

* Dados temporários para teste
* Dados que dependem do estado do banco de dado
* Dados que precisam de valores de chave a serem gerados pelo banco de dado, incluindo entidades que usam chaves alternativas como a identidade
* Dados que exigem transformação personalizada (que não é manipulada por [conversões de valor](xref:core/modeling/value-conversions)), como alguns hashes de senha
* Dados que exigem chamadas para a API externa, como ASP.NET Core funções de identidade e criação de usuários

## <a name="manual-migration-customization"></a>Personalização de migração manual

Quando uma migração é adicionada, as alterações nos dados especificados com `HasData` são transformadas em chamadas para `InsertData()`, `UpdateData()`e `DeleteData()`. Uma maneira de trabalhar com algumas das limitações do `HasData` é adicionar manualmente essas chamadas ou [operações personalizadas](xref:core/managing-schemas/migrations/operations) à migração.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Lógica de inicialização personalizada

Uma maneira simples e eficiente de executar a propagação de dados é usar [`DbContext.SaveChanges()`](xref:core/saving/index) antes que a lógica do aplicativo principal comece a execução.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> O código de propagação não deve fazer parte da execução normal do aplicativo, pois isso pode causar problemas de simultaneidade quando várias instâncias estão em execução e também exigiria que o aplicativo tenha permissão para modificar o esquema de banco de dados.

Dependendo das restrições de sua implantação, o código de inicialização pode ser executado de maneiras diferentes:

* Executando o aplicativo de inicialização localmente
* Implantando o aplicativo de inicialização com o aplicativo principal, invocando a rotina de inicialização e desabilitando ou removendo o aplicativo de inicialização.

Normalmente, isso pode ser automatizado usando [perfis de publicação](/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
