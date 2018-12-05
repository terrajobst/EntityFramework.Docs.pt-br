---
title: Propagação de dados – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857423"
---
# <a name="data-seeding"></a>Propagação de dados

Propagação de dados é o processo de preenchimento de um banco de dados com um conjunto inicial de dados.

Há várias maneiras, que isso pode ser feito no EF Core:
* Dados de propagação de modelo
* Personalização da migração manual
* Lógica de inicialização personalizada

## <a name="model-seed-data"></a>Dados de propagação de modelo

> [!NOTE]
> Este recurso é novo no EF Core 2.1.

Diferentemente do EF6 no EF Core, a propagação de dados pode ser associada com um tipo de entidade como parte da configuração do modelo. Em seguida, o EF Core [migrações](xref:core/managing-schemas/migrations/index) pode calcular automaticamente o que inserir, atualizar ou excluir operações precisa ser aplicado ao atualizar o banco de dados para uma nova versão do modelo.

> [!NOTE]
> Migrações considera somente as alterações de modelo ao determinar qual operação deve ser executada para obter os dados de semente para o estado desejado. Portanto, quaisquer alterações de dados executadas fora de migrações podem ser perdidas ou causar um erro.

Por exemplo, isso irá configurar dados de semente para uma `Blog` em `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para adicionar entidades que têm uma relação de valores de chave estrangeira precisa ser especificado:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Se o tipo de entidade tem todas as propriedades em uma classe anônima pode ser usada para fornecer os valores de estado de sombra:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Propriedade entidade tipos podem ser propagados de maneira semelhante:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Consulte a [projeto de exemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obter mais contexto.

Depois que os dados tiverem sido adicionados ao modelo, [migrações](xref:core/managing-schemas/migrations/index) deve ser usado para aplicar as alterações.

> [!TIP]
> Se você precisar aplicar migrações como parte de uma implantação automatizada, você pode [criar um script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que pode ser visualizado antes da execução.

Como alternativa, você pode usar `context.Database.EnsureCreated()` para criar um novo banco de dados que contém os dados de semente, por exemplo, para um banco de dados de teste ou ao usar o provedor na memória ou qualquer banco de dados não-relação. Observe que, se o banco de dados já existe, `EnsureCreated()` não atualizará os dados de semente nem de esquema no banco de dados. Para bancos de dados relacionais, você não deve chamar `EnsureCreated()` se você planeja usar as migrações.

Esse tipo de dados de propagação é gerenciado pelas migrações e o script para atualizar os dados que já está no banco de dados precisa ser gerado sem se conectar ao banco de dados. Isso impõe algumas restrições:
* O valor de chave primária precisa ser especificado mesmo que ele normalmente é gerado pelo banco de dados. Ele será usado para detectar alterações de dados entre as migrações.
* Anteriormente dados propagados serão removidos se a chave primária é alterada de alguma forma.

Portanto, esse recurso é mais útil para dados estáticos que não tem deve ser alterada fora de migrações e não dependem mais nada no banco de dados, por exemplo CEPs.

Se seu cenário inclui qualquer um dos seguintes, é recomendável usar a lógica de inicialização personalizada descrita na última seção:
* Dados temporários para teste
* Dados que depende do estado do banco de dados
* Dados que precisam de valores de chave a ser gerado pelo banco de dados, incluindo as entidades que usam chaves alternativas como a identidade
* Dados que requerem transformação personalizada (que não é tratado por [conversões de valor](xref:core/modeling/value-conversions)), como um hash de senha
* Dados que exigem chamadas à API externa, como a criação de usuários e funções do ASP.NET Core Identity

## <a name="manual-migration-customization"></a>Personalização da migração manual

Quando uma migração é adicionada as alterações aos dados especificados com `HasData` são transformados em chamadas para `InsertData()`, `UpdateData()`, e `DeleteData()`. Uma maneira de trabalhar em torno de algumas das limitações das `HasData` é adicionar manualmente essas chamadas ou [operações personalizadas](xref:core/managing-schemas/migrations/operations) para a migração em vez disso.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Lógica de inicialização personalizada

Uma maneira simples e poderosa para realizar a propagação de dados é usar [ `DbContext.SaveChanges()` ](xref:core/saving/index) antes que o aplicativo principal lógica inicia a execução.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> O código de propagação não deve ser parte da execução do aplicativo normal, pois isso pode causar problemas de simultaneidade quando várias instâncias estão em execução e também exigiria que o aplicativo com permissão para modificar o esquema de banco de dados.

Dependendo das restrições de sua implantação, o código de inicialização pode ser executado de diferentes maneiras:
* Executando o aplicativo de inicialização localmente
* Implantando o aplicativo de inicialização com o aplicativo principal, invocando a rotina de inicialização e desabilitando ou removendo o aplicativo de inicialização.

Normalmente, isso pode ser automatizado por meio [perfis de publicação](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
