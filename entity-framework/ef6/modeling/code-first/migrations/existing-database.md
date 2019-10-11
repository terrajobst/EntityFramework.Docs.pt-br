---
title: Migrações do Code First com um banco de dados existente-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: eb7948eafb1322cabcf69b47bd5411f762fe8498
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182579"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migrações do Code First com um banco de dados existente
> [!NOTE]
> **Somente EF 4.3 em diante** – os recursos, as APIs, etc. abordados nesta página foram introduzidos no Entity Framework 4,1. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Este artigo aborda o uso de Migrações do Code First com um banco de dados existente, um que não foi criado por Entity Framework.

> [!NOTE]
> Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos. Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="screencasts"></a>Screencasts

Se você preferir assistir a um screencast do que ler este artigo, os dois vídeos a seguir abrangem o mesmo conteúdo deste artigo.

### <a name="video-one-migrations---under-the-hood"></a>Vídeo um: "Migrações-nos bastidores"

[Este screencast](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) aborda como as migrações acompanham e usam informações sobre o modelo para detectar alterações de modelo.

### <a name="video-two-migrations---existing-databases"></a>Vídeo dois: "Migrações-bancos de dados existentes"

Aproveitando os conceitos do vídeo anterior, [Este screencast](https://channel9.msdn.com/blogs/ef/migrations-existing-databases) aborda como habilitar e usar migrações com um banco de dados existente.

## <a name="step-1-create-a-model"></a>Etapa 1: Criar um modelo

Sua primeira etapa será criar um modelo de Code First que tenha como destino o banco de dados existente. O tópico [Code First para um banco de dados existente](~/ef6/modeling/code-first/workflows/existing-database.md) fornece orientações detalhadas sobre como fazer isso.

>[!NOTE]
> É importante seguir o restante das etapas neste tópico antes de fazer alterações no modelo que exijam alterações no esquema do banco de dados. As etapas a seguir exigem que o modelo esteja em sincronia com o esquema de banco de dados.

## <a name="step-2-enable-migrations"></a>Etapa 2: Habilitar migrações

A próxima etapa é habilitar as migrações. Você pode fazer isso executando o comando **Enable-Migrations** no console do Gerenciador de pacotes.

Esse comando criará uma pasta em sua solução chamada migrações e colocará uma única classe dentro dela chamada configuração. A classe de configuração é onde você configura as migrações para seu aplicativo, você pode obter mais informações sobre isso no tópico [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) .

## <a name="step-3-add-an-initial-migration"></a>Etapa 3: Adicionar uma migração inicial

Depois que as migrações tiverem sido criadas e aplicadas ao banco de dados local, talvez você também queira aplicar essas alterações a outros bancos. Por exemplo, seu banco de dados local pode ser um banco de dados de teste e, eventualmente, você também deseja aplicar as alterações a um banco de dados de produção e/ou outros desenvolvedores de teste. Há duas opções para essa etapa e a que você deve escolher depende se o esquema de quaisquer outros bancos de dados está vazio ou se corresponde atualmente ao esquema do banco de dados local.

-   **Option um: Usar esquema existente como ponto de partida.** Você deve usar essa abordagem quando outros bancos de dados aos quais as migrações serão aplicadas no futuro terão o mesmo esquema que o banco de dados local tem atualmente. Por exemplo, você pode usar isso se o banco de dados de teste local corresponder atualmente a v1 do seu banco de dados de produção e posteriormente você aplicará essas migrações para atualizar seu banco de dados de produção para v2.
-   **Option dois: Usar banco de dados vazio como ponto de partida.** Você deve usar essa abordagem quando outros bancos de dados aos quais as migrações serão aplicadas no futuro estiverem vazios (ou ainda não existirem). Por exemplo, você pode usar isso se você começou a desenvolver seu aplicativo usando um banco de dados de teste, mas sem usar migrações e posteriormente você desejará criar um banco de dados de produção do zero.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opção um: Usar esquema existente como ponto de partida

Migrações do Code First usa um instantâneo do modelo armazenado na migração mais recente para detectar alterações no modelo (você pode encontrar informações detalhadas sobre isso em [migrações do Code First em ambientes de equipe](~/ef6/modeling/code-first/migrations/teams.md)). Como vamos pressupor que os bancos de dados já tenham o esquema do modelo atual, geraremos uma migração vazia (sem op) que tem o modelo atual como um instantâneo.

1.  Execute o comando **Add-Migration InitialCreate – IgnoreChanges** no console do Gerenciador de pacotes. Isso cria uma migração vazia com o modelo atual como um instantâneo.
2.  Execute o comando **Update-Database** no console do Gerenciador de pacotes. Isso aplicará a migração InitialCreate ao banco de dados. Como a migração real não contém nenhuma alteração, ela simplesmente adicionará uma linha à tabela \_ @ no__t-1MigrationsHistory, indicando que essa migração já foi aplicada.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opção dois: Usar banco de dados vazio como ponto de partida

Nesse cenário, precisamos de migrações para poder criar todo o banco de dados do zero, incluindo as tabelas que já estão presentes em nosso banco de dados local. Vamos gerar uma migração InitialCreate que inclui a lógica para criar o esquema existente. Em seguida, vamos fazer com que nosso banco de dados existente pareça que essa migração já foi aplicada.

1.  Execute o comando **Add-Migration InitialCreate** no console do Gerenciador de pacotes. Isso cria uma migração para criar o esquema existente.
2.  Comente todos os códigos no método up da migração recém-criada. Isso nos permitirá ' Aplicar ' a migração para o banco de dados local sem tentar recriar todas as tabelas, etc. já existentes.
3.  Execute o comando **Update-Database** no console do Gerenciador de pacotes. Isso aplicará a migração InitialCreate ao banco de dados. Como a migração real não contém nenhuma alteração (porque as comentamos temporariamente), ela simplesmente adicionará uma linha à tabela \_ @ no__t-1MigrationsHistory, indicando que essa migração já foi aplicada.
4.  Retire o comentário do código no método up. Isso significa que, quando essa migração for aplicada a bancos de dados futuros, o esquema que já existia no banco de dados local será criado por migrações.

## <a name="things-to-be-aware-of"></a>Coisas que você deve conhecer

Há algumas coisas que você precisa conhecer ao usar migrações em um banco de dados existente.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Os nomes padrão/calculados podem não corresponder ao esquema existente

As migrações especificam explicitamente nomes para colunas e tabelas quando aplica Scaffold uma migração. No entanto, há outros objetos de banco de dados que as migrações calculam um nome padrão para o ao aplicar as migrações. Isso inclui índices e restrições Foreign Key. Ao direcionar um esquema existente, esses nomes calculados podem não corresponder ao que realmente existe no banco de dados.

Aqui estão alguns exemplos de quando você precisa estar ciente disso:

**If você usou ' opção um: Usar esquema existente como ponto de partida ' da etapa 3:**

-   Se as alterações futuras em seu modelo exigirem a alteração ou remoção de um dos objetos de banco de dados nomeados de forma diferente, será necessário modificar a migração com Scaffold para especificar o nome correto. As APIs de migrações têm um parâmetro de nome opcional que permite que você faça isso.
    Por exemplo, o esquema existente pode ter uma tabela post com uma coluna de chave estrangeira de blogid que tenha um índice chamado IndexFk @ no__t-0BlogId. No entanto, por padrão, as migrações esperam que esse índice fosse chamado de IX @ no__t-0BlogId. Se você fizer uma alteração no modelo que resulta no descarte desse índice, será necessário modificar a chamada com Scaffold DropIndex para especificar o nome do IndexFk @ no__t-0BlogId.

**If você usou ' opção dois: Usar banco de dados vazio como ponto de partida ' da etapa 3:**

-   A tentativa de executar o método down da migração inicial (ou seja, reverter para um banco de dados vazio) em seu banco de dados local pode falhar porque as migrações tentarão descartar índices e restrições FOREIGN KEY usando os nomes incorretos. Isso afetará apenas o banco de dados local, já que outros bancos de dado serão criados a partir do zero usando o método up da migração inicial.
    Se você quiser fazer o downgrade do banco de dados local existente para um estado vazio, será mais fácil fazê-lo manualmente, removendo o banco de dados ou descartando todas as tabelas. Após esse downgrade inicial, todos os objetos de banco de dados serão recriados com os nomes padrão, portanto, esse problema não se apresentará novamente.
-   Se as alterações futuras em seu modelo exigirem a alteração ou remoção de um dos objetos de banco de dados nomeados de forma diferente, isso não funcionará no banco de dados local existente – já que os nomes não corresponderão aos padrões. No entanto, ele funcionará em bancos de dados que foram criados ' do zero ', pois eles terão usado os nomes padrão escolhidos pelas migrações.
    Você pode fazer essas alterações manualmente em seu banco de dados local existente ou considerar que as migrações recriam seu banco de dados do zero, como ocorrerá em outras máquinas.
-   Bancos de dados criados com o uso do método up de sua migração inicial podem ser ligeiramente diferentes do banco de dado local, já que os nomes padrão calculados para índices e restrições Foreign Key serão usados. Você também pode acabar com índices extras, pois as migrações criarão índices em colunas de chave estrangeira por padrão – isso pode não ter sido o caso no banco de dados local original.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Nem todos os objetos de banco de dados são representados no modelo

Os objetos de banco de dados que não fazem parte do seu modelo não serão tratados pelas migrações. Isso pode incluir exibições, procedimentos armazenados, permissões, tabelas que não fazem parte de seu modelo, índices adicionais, etc.

Aqui estão alguns exemplos de quando você precisa estar ciente disso:

-   Independentemente da opção escolhida na ' etapa 3 ', se as alterações futuras em seu modelo exigirem a alteração ou descartar essas migrações de objetos adicionais não saberão fazer essas alterações. Por exemplo, se você remover uma coluna que tem um índice adicional, as migrações não saberão remover o índice. Você precisará adicioná-lo manualmente à migração do com Scaffold.
-   Se você usou ' opção dois: Usar um banco de dados vazio como ponto de partida ', esses objetos adicionais não serão criados pelo método up de sua migração inicial.
    Você pode modificar os métodos para cima e para baixo para cuidar desses objetos adicionais, se desejar. Para objetos que não têm suporte nativo na API de migrações – como exibições – você pode usar o método [SQL](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) para executar SQL bruto para criá-los/descartá-los.
