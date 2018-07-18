---
title: Migrações do Code First com um banco de dados - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
caps.latest.revision: 3
ms.openlocfilehash: 77e139a29bb4708b00fc6198a57780ce75197252
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120095"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrações do Code First com um banco de dados existente
> [!NOTE]
> **EF4.3 em diante, somente** -recursos, APIs, etc. discutidos nesta página foram introduzidas no Entity Framework 4.1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este artigo aborda o uso de migrações do Code First com um banco de dados existente, o que não foi criado pelo Entity Framework.

> [!NOTE]
> Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos. Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="screencasts"></a>Screencasts

Se, em vez disso, você teria assistir um screencast que leia este artigo, os vídeos a seguir abrangem o mesmo conteúdo deste artigo.

### <a name="video-one-migrations---under-the-hood"></a>Vídeo 1: "Migrações – nos bastidores"

[Neste screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) aborda como controla as migrações e usa as informações sobre o modelo para detectar alterações no modelo.

### <a name="video-two-migrations---existing-databases"></a>Vídeo 2: "Migrações – bancos de dados existentes"

Aproveitando os conceitos de vídeo anterior, [este screencast](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) aborda como habilitar e usar as migrações com um banco de dados existente.

## <a name="step-1-create-a-model"></a>Etapa 1: Criar um modelo

A primeira etapa será criar um modelo Code First que tem como alvo o banco de dados existente. O [Code First para um banco de dados](~/ef6/modeling/code-first/workflows/existing-database.md) tópico fornece orientações detalhadas sobre como fazer isso.

>[!NOTE]
> É importante seguir o restante das etapas neste tópico antes de fazer alterações ao seu modelo que exige alterações no esquema de banco de dados. As etapas a seguir exigem o modelo a ser em sincronia com o esquema de banco de dados.

## <a name="step-2-enable-migrations"></a>Etapa 2: Habilitar migrações

A próxima etapa é permitir migrações. Você pode fazer isso executando o **Enable-Migrations** comando no Console do Gerenciador de pacotes.

Esse comando será criar uma pasta na solução denominada migrações e colocar uma única classe dentro dela chamada de configuração. A classe de configuração é onde você configura as migrações para seu aplicativo, você pode encontrar mais informações sobre ela na [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) tópico.

## <a name="step-3-add-an-initial-migration"></a>Etapa 3: Adicionar uma migração inicial

Depois que as migrações foram criadas e aplicadas para o banco de dados local, que talvez você queira aplicá-las altera para outros bancos de dados. Por exemplo, seu banco de dados local pode ser um banco de dados de teste e, por fim, convém também se aplicam as alterações a um banco de dados de produção e/ou bancos de dados de teste de outros desenvolvedores. Há duas opções para esta etapa e você deve escolher aquele depende se o esquema de outros bancos de dados está vazio ou atualmente correspondem ao esquema de banco de dados local.

-   **Opção um: Use o esquema existente como ponto de partida.** Você deve usar essa abordagem quando outros bancos de dados que as migrações serão aplicadas a futuramente terão o mesmo esquema que atualmente tem um banco de dados local. Por exemplo, você pode usar isso se seu banco de dados de teste local no momento corresponde v1 do seu banco de dados de produção e mais tarde, você aplicará essas migrações para atualizar seu banco de dados de produção para v2.
-   **Opção dois: Use o banco de dados vazio como ponto de partida.** Você deve usar essa abordagem quando outros bancos de dados que as migrações serão aplicadas a futuramente estão vazios (ou ainda não existir). Por exemplo, você pode usar isso se você iniciar o desenvolvimento de seu aplicativo usando um banco de dados de teste, mas sem usar as migrações e você posteriormente desejará criar um banco de dados de produção do zero.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opção um: Use o esquema existente como ponto de partida

Migrações do Code First usa um instantâneo do modelo armazenado na migração mais recente para detectar as alterações no modelo (você pode encontrar informações detalhadas sobre isso em [migrações do Code First em ambientes de equipe](~/ef6/modeling/code-first/migrations/teams.md)). Já que vamos supor que os bancos de dados já tem o esquema do modelo atual, geramos uma migração (sem op) vazia que tem o modelo atual como um instantâneo.

1.  Execute o **Add-Migration InitialCreate – IgnoreChanges** comando no Console do Gerenciador de pacotes. Isso cria uma migração vazia com o modelo atual como um instantâneo.
2.  Execute o **Update-Database** comando no Console do Gerenciador de pacotes. Isso se aplicará a migração InitialCreate ao banco de dados. Uma vez que a migração real não contém todas as alterações, ele simplesmente adicionará uma linha para o \_ \_MigrationsHistory tabela que indica se essa migração já foi aplicada.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opção dois: Usar o banco de dados vazio como um ponto de partida

Nesse cenário, precisamos migrações para poder criar o banco de dados inteiro do zero – incluindo as tabelas que já estão presentes no nosso banco de dados local. Vamos gerar uma migração InitialCreate que inclui lógica para criar o esquema existente. Em seguida, faremos nosso banco de dados existente parecer que essa migração já foi aplicada.

1.  Execute o **Add-Migration InitialCreate** comando no Console do Gerenciador de pacotes. Isso cria uma migração para criar o esquema existente.
2.  Comente todo o código no método de cima da migração recém-criado. Isso nos permitirá ' Aplicar ' a migração para o banco de dados local sem tentar recriar todas as tabelas etc. que já existem.
3.  Execute o **Update-Database** comando no Console do Gerenciador de pacotes. Isso se aplicará a migração InitialCreate ao banco de dados. Como a migração real não contém todas as alterações (porque estamos temporariamente comentado-out), ele simplesmente adicionará uma linha para o \_ \_MigrationsHistory tabela que indica se essa migração já foi aplicada.
4.  Cancelar os comentários de código no método Up. Isso significa que, quando essa migração é aplicada a bancos de dados futuros, o esquema que já existia no banco de dados local será criado pelas migrações.

## <a name="things-to-be-aware-of"></a>Coisas a serem consideradas

Há algumas coisas que você precisa estar ciente ao usar as migrações em relação a um banco de dados existente.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Os nomes padrão/calculado podem não corresponder ao esquema existente

Migrações especifica explicitamente os nomes das colunas e tabelas quando ele usa o Scaffold de migrações. No entanto, há outros objetos de banco de dados que as migrações calcula um nome padrão para ao aplicar as migrações. Isso inclui índices e restrições de chave estrangeira. Ao direcionar um esquema existente, esses nomes calculados podem não corresponder ao que realmente existe em seu banco de dados.

Aqui estão alguns exemplos de quando você precisa estar atento isso:

**Se você usou ' uma opção: usar o esquema existente como um ponto de partida ' da etapa 3:**

-   Se precisam de alterações futuras em seu modelo alterando ou removendo um dos objetos de banco de dados que é chamado de forma diferente, você precisará modificar a migração gerado por scaffolding para especificar o nome correto. As APIs de migrações têm um parâmetro de nome opcional que permite que você faça isso.
    Por exemplo, o esquema existente pode ter uma tabela de Post com uma coluna de chave estrangeira BlogId que tem um índice chamado IndexFk\_BlogId. No entanto, por padrão as migrações esperaria esse índice seja nomeado IX\_BlogId. Se você fizer uma alteração em seu modelo que resulta em remoção desse índice, você precisará modificar a chamada DropIndex gerado por scaffolding para especificar o IndexFk\_nome BlogId.

**Se você usou ' opção dois: usar o banco de dados vazio como um ponto de partida ' da etapa 3:**

-   Tentativa de executar o método de busca da migração inicial (isto é, reverter para um banco de dados vazio) no banco de dados local pode falhar porque as migrações tentará descartar índices e restrições de chave estrangeira usando os nomes incorretos. Isso afetará apenas o banco de dados local, pois outros bancos de dados serão criados do zero usando o método de cima da migração inicial.
    Se você quiser fazer o downgrade de seu banco de dados local existente para um estado vazio é mais fácil de fazer isso manualmente, por descartar o banco de dados ou descartar todas as tabelas. Após esse downgrade inicial de todos os objetos de banco de dados serão recriados com os nomes padrão, portanto, esse problema não apresentará novamente.
-   Se precisam de alterações futuras em seu modelo alterando ou removendo um dos objetos de banco de dados que é chamado de forma diferente, isso não funcionará em relação a seu banco de dados local existente – desde que os nomes não coincidem com os padrões. No entanto, ele funcionará em bancos de dados que foram criados 'a partir do zero', pois eles serão usados os nomes padrão escolhidos pelo migrações.
    Você pode fazer essas alterações manualmente no seu banco de dados local existente, ou faça com que as migrações de recriar o banco de dados a partir do zero – quanto em outros computadores.
-   Bancos de dados criados usando o método de backup de sua migração inicial podem diferir ligeiramente do banco de dados local, desde os nomes padrão calculado para índices e restrições de chave estrangeira serão usadas. Você também poderá terminar com índices adicionais como migrações criará índices em colunas de chave estrangeira por padrão – isso pode não ter sido o caso no seu banco de dados local original.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Nem todos os objetos de banco de dados são representados no modelo

Objetos de banco de dados que não fazem parte do seu modelo não serão manipulados por migrações. Isso pode incluir exibições, procedimentos armazenados, permissões, tabelas que não fazem parte de seu modelo, índices adicionais, etc.

Aqui estão alguns exemplos de quando você precisa estar atento isso:

-   Independentemente da opção escolhida na etapa 3 se precisam de alterações futuras em seu modelo, alterar ou descartar esses objetos adicionais que as migrações não saberá para fazer essas alterações. Por exemplo, se você descartar uma coluna que tem um índice adicional nele, as migrações não saberá para descartar o índice. Você precisará adicionar isso manualmente para a migração gerado por scaffolding.
-   Se você usou ' opção dois: usar o banco de dados vazio como um ponto de partida ', estes objetos adicionais não serão criados pelo método de cima da sua migração inicial.
    Você pode modificar cima e para baixo de métodos para cuidar desses objetos adicionais se desejar. Para objetos que não são suportados nativamente na API de migrações – como modos de exibição – você pode usar o [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) método para executar o SQL bruto para criar/remover eles.
