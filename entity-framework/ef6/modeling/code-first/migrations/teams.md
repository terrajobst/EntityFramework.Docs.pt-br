---
title: Migrações do Code First em ambientes de equipe - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 42f52e63fd6cfc1f02d6a721594f4a161eea9a7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997293"
---
# <a name="code-first-migrations-in-team-environments"></a>Migrações do Code First em ambientes de equipe
> [!NOTE]
> Este artigo pressupõe que você sabe como usar as migrações Code First em cenários básicos. Se você não fizer isso, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Tomar um café, você precisa ler este artigo inteiro

Os problemas em ambientes de equipe são principalmente em torno de mesclagem migrações quando dois desenvolvedores geraram migrações em sua base de código local. Enquanto as etapas para superar esses são bastante simples, eles exigem que você tenha uma compreensão sólida do funcionamento de migrações. Por favor, não apenas pular para o final – Reserve um tempo para ler o artigo inteiro para garantir que você tiver êxito.

## <a name="some-general-guidelines"></a>Algumas diretrizes gerais

Antes de nos aprofundar em como gerenciar as migrações de mesclagem geradas por vários desenvolvedores, aqui estão algumas diretrizes gerais para prepará-lo sucesso.

### <a name="each-team-member-should-have-a-local-development-database"></a>Cada membro da equipe deve ter um banco de dados de desenvolvimento local

As migrações usa o  **\_ \_MigrationsHistory** tabela para armazenar a quais migrações foram aplicadas ao banco de dados. Se você tiver vários desenvolvedores gerar migrações diferentes ao tentar o mesmo banco de dados de destino (e, portanto, compartilham uma  **\_ \_MigrationsHistory** tabela) migrações vai ficar muito confuso.

É claro que, se você tiver os membros da equipe que não estão gerando as migrações, há sem problemas fazendo com que eles compartilham um banco de dados de desenvolvimento central.

### <a name="avoid-automatic-migrations"></a>Evitar migrações automáticas

O resultado final é que as migrações automáticas inicialmente boas aparência em ambientes de equipe, mas na verdade eles simplesmente não funcionam. Se você quiser saber o porquê, continue lendo – caso contrário, em seguida, pule para a próxima seção.

As migrações automáticas permite que você tenha seu esquema de banco de dados atualizada de acordo com o modelo atual sem a necessidade de gerar arquivos de código (as migrações baseadas em código). As migrações automáticas funcionaria muito bem em um ambiente de equipe, se você apenas os utilizou e nunca gerado as migrações baseadas em código. O problema é que as migrações automáticas são limitadas e não lidar com um número de operações – renomeia propriedade/coluna, movimentação de dados para outra tabela, etc. Para tratar desses cenários, você acaba gerando as migrações baseadas em código (e editando o código gerado por scaffolding) que são misturados entre as alterações que são manipuladas pelo migrações automáticas. Isso é quase impossível para mesclar as alterações quando dois desenvolvedores fazer check-in migrações.

## <a name="screencasts"></a>Screencasts

Se, em vez disso, você teria assistir um screencast que leia este artigo, os vídeos a seguir abrangem o mesmo conteúdo deste artigo.

### <a name="video-one-migrations---under-the-hood"></a>Vídeo 1: "Migrações – nos bastidores"

[Neste screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) aborda como controla as migrações e usa as informações sobre o modelo para detectar alterações no modelo.

### <a name="video-two-migrations---team-environments"></a>Vídeo 2: "Migrações – ambientes de equipe"

Aproveitando os conceitos de vídeo anterior, [este screencast](http://channel9.msdn.com/blogs/ef/migrations-team-environments) aborda os problemas que surgem em um ambiente de equipe e como resolvê-los.

## <a name="understanding-how-migrations-works"></a>Compreender como funciona a migrações

A chave para usar com êxito as migrações em um ambiente de equipe é básico Noções básicas sobre como as migrações acompanha e usa informações sobre o modelo para detectar alterações no modelo.

### <a name="the-first-migration"></a>A primeira migração

Quando você adiciona a primeira migração ao seu projeto, executar algo como **Add-Migration primeiro** no Console do Gerenciador de pacotes. As etapas de alto níveis que executa esse comando são mostradas abaixo.

![FirstMigration](~/ef6/media/firstmigration.png)

O modelo atual é calculado a partir do código (1). Os objetos de banco de dados necessários, em seguida, são calculados pelo diferem de modelo (2) – como essa é a primeira migração modelo diferem apenas usa um modelo vazio para a comparação. As alterações necessárias são passadas para o gerador de código para compilar o código de migração necessárias (3), que é então adicionado à sua solução do Visual Studio (4).

Além do código de migração real é armazenado no arquivo de código principal, as migrações também gera alguns arquivos de code-behind adicionais. Esses arquivos são metadados que são usados pelas migrações e não são algo que você deve editar. Um desses arquivos é um arquivo de recurso (. resx) que contém um instantâneo do modelo no momento em que a migração foi gerada. Você verá como isso é usado na próxima etapa.

Neste ponto, você provavelmente executaria **Update-Database** para aplicar suas alterações no banco de dados e, em seguida, vá sobre a implementação de outras áreas do seu aplicativo.

### <a name="subsequent-migrations"></a>Migrações subsequentes

Posteriormente, voltar e fazer algumas alterações ao seu modelo – em nosso exemplo, adicionaremos uma **Url** propriedade **Blog**. Você poderia, em seguida, emitir um comando, como **Add-Migration AddUrl** para criar o scaffolding de uma migração para o banco de dados correspondente de aplicar as alterações. As etapas de alto níveis que executa esse comando são mostradas abaixo.

![SecondMigration](~/ef6/media/secondmigration.png)

Assim como a última vez, o modelo atual é calculado a partir de código (1). No entanto, desta vez há migrações existentes para que o modelo anterior é recuperado da migração (2) mais recente. Esses dois modelos são foi comparado para localizar as alterações de banco de dados necessário (3) e, em seguida, o processo for concluído como antes.

Esse mesmo processo é usado para as migrações adicionais que você adiciona ao projeto.

### <a name="why-bother-with-the-model-snapshot"></a>Por que se preocupar com o instantâneo do modelo?

Você pode estar se perguntando por que o EF incomoda muito com o instantâneo do modelo – por que não simplesmente olhar o banco de dados. Nesse caso, continue lendo. Se você não estiver interessado, em seguida, você pode ignorar esta seção.

Há inúmeros motivos que EF mantém o instantâneo em torno do modelo:

-   Ele permite que seu banco de dados incorretos do modelo de EF. Essas alterações podem ser feitas diretamente no banco de dados, ou você pode alterar o código gerado por scaffolding em suas migrações para fazer as alterações. Aqui estão alguns exemplos na prática:
    -   Você deseja adicionar um Inserted e atualizada a coluna a um ou mais de suas tabelas, mas você não deseja incluir essas colunas no modelo do EF. Se as migrações examinou o banco de dados que ele continuamente tentar descartar essas colunas sempre gerada por scaffolding uma migração. Usando o modelo de instantâneo, EF só irá detectar alterações legítimas para o modelo.
    -   Você deseja alterar o corpo de um procedimento armazenado usado para atualizações para incluir algum registro em log. Se as migrações observou esse procedimento armazenado do banco de dados ele seria continuamente tente e redefini-la de volta para a definição de espera do EF. Ao usar o instantâneo do modelo, o EF só irá dar suporte código para alterar o procedimento armazenado, quando você altera a forma do procedimento no modelo do EF.
    -   Esses mesmos princípios se aplicam a adição de índices adicionais, incluindo as tabelas adicionais no banco de dados, mapeando o EF para uma exibição de banco de dados que se encontra em uma tabela, etc.
-   O modelo do EF contém mais do que apenas a forma do banco de dados. Todo o modelo permite que as migrações, examine informações sobre as propriedades e classes no seu modelo e como eles são mapeados para as tabelas e colunas. Essas informações permitem que as migrações ser mais inteligente no código que ele usa o Scaffold. Por exemplo, se você alterar o nome da coluna que uma propriedade é mapeada para migrações pode detectar a renomeação por ver que é a mesma propriedade – algo que não pode ser feito se você tiver apenas o esquema de banco de dados. 

## <a name="what-causes-issues-in-team-environments"></a>O que causa problemas em ambientes de equipe

O fluxo de trabalho é abordado em funciona a seção anterior muito bem quando você for um único desenvolvedor trabalhando em um aplicativo. Ele também funciona bem em um ambiente de equipe se você for a única pessoa que fazer alterações no modelo. Nesse cenário, você pode fazer alterações no modelo, gerar migrações e enviá-los para o controle de origem. Outros desenvolvedores podem sincronizar suas alterações e executar **Update-Database** as alterações de esquema aplicado.

Problemas começam a surgir quando você tiver vários desenvolvedores possam fazer alterações no modelo do EF e enviando ao controle de origem ao mesmo tempo. Não tem o EF é uma maneira de primeira classe para mesclar suas migrações locais com as migrações que outro desenvolvedor enviado ao controle de origem, pois sincronizado pela última vez.

## <a name="an-example-of-a-merge-conflict"></a>Um exemplo de um conflito de mesclagem

Primeiro vamos examinar um exemplo concreto de tal um conflito de mesclagem. Continuaremos em com de exemplo que vimos anteriormente. Como uma partida aponte vamos supor que as alterações da seção anterior foram check-in pelo desenvolvedor do original. Vamos rastrear dois desenvolvedores conforme eles fazem alterações no código base.

Vamos rastrear o modelo do EF e as migrações por meio de um número de alterações. Para um ponto de partida, ambos os desenvolvedores têm sincronizados com o repositório de controle de origem, conforme ilustrado no gráfico a seguir.

![StartingPoint](~/ef6/media/startingpoint.png)

Developer \#1 e o desenvolvedor \#2 agora faz algumas alterações no modelo do EF no seu código local base. Developer \#1 adiciona um **Rating** propriedade a ser **Blog** – e gera um **AddRating** migração para aplicar as alterações no banco de dados. Developer \#2 adiciona uma **leitores** propriedade **Blog** – e gera correspondente **AddReaders** migração. Execute os dois desenvolvedores **Update-Database**, para aplicar as alterações em seus bancos de dados locais e, em seguida, continuar a desenvolver o aplicativo.

> [!NOTE]
> As migrações são prefixadas com um carimbo de hora, portanto, nosso gráfico representa que a migração AddReaders do desenvolvedor \#2 vem após a migração AddRating de desenvolvedor \#1. Se developer \#1 ou \#2 não gerado a torna a primeira migração nenhuma diferença para os problemas de trabalhar em uma equipe ou o processo para mesclá-las que veremos na próxima seção.

![LocalChanges](~/ef6/media/localchanges.png)

Ele é um dia de sorte para desenvolvedor \#1 conforme acontecem enviar suas alterações primeiro. Porque nenhuma outra pessoa fez check-in, pois eles sincronizados seu repositório, eles apenas podem enviar suas alterações sem executar qualquer mesclagem.

![Enviar](~/ef6/media/submit.png)

Agora é hora de desenvolvedor \#2 para enviar. Eles não tiveram tanta sorte. Porque alguém enviou as alterações, pois eles sincronizados, precisará puxar as alterações e mesclagem. O sistema de controle do código-fonte provavelmente será capaz de mesclar automaticamente as alterações no nível do código, pois são muito simples. O estado do desenvolvedor \#2 local do repositório após a sincronização é ilustrada no gráfico a seguir. 

![Pull](~/ef6/media/pull.png)

Neste estágio Developer \#2 podem executar **Update-Database** que irá detectar o novo **AddRating** migração (que não tenha sido aplicado ao desenvolvedor \#do 2 banco de dados) e aplicá-lo. Agora o **Rating** coluna é adicionada à **Blogs** tabela e o banco de dados está em sincronia com o modelo.

No entanto, existem dois problemas:

1.  Embora **Update-Database** aplicará os **AddRating** migração também acionará um aviso: *não é possível atualizar o banco de dados de acordo com o modelo atual porque existem alterações pendentes e a migração automática está desabilitada...*
    O problema é que o instantâneo do modelo é armazenado na última migração (**AddReader**) está faltando o **classificação** propriedade no **Blog** (desde que não faz parte do modelo de quando o migração foi gerada). Código primeiro detecta que o modelo em que a última migração não corresponde ao modelo atual e gera o aviso.
2.  Executando o aplicativo pode resultar em uma InvalidOperationException informando que "*o modelo fazendo o contexto de 'BloggingContext' foi alterado desde que o banco de dados foi criado. Considere o uso de migrações do Code First para atualizar o banco de dados..."*
    Novamente, o problema é que o instantâneo do modelo armazenado na última migração não corresponde ao modelo atual.
3.  Por fim, esperaríamos em execução **Add-Migration** agora geraria uma migração vazia (pois não há nenhuma alteração a ser aplicado ao banco de dados). Mas porque as migrações compara o modelo atual para aquela da última migração (que está faltando a **Rating** propriedade), na verdade, ele será scaffold outro **AddColumn** chamada para adicionar o **Rating** coluna. Obviamente, essa migração falhar durante **Update-Database** porque o **classificação** coluna já existe.

## <a name="resolving-the-merge-conflict"></a>Resolver o conflito de mesclagem

A boa notícia é que não é muito difícil lidar com a mesclagem manualmente – desde que você tenha uma compreensão do funcionamento de migrações. Portanto, se você tiver ignorado em frente a esta seção... Desculpe, você precisa voltar e ler o restante do artigo primeiro!

Há duas opções, é a maneira mais fácil gerar uma migração em branco que tem o modelo correto atual como um instantâneo. A segunda opção é atualizar o instantâneo da última migração para tem o formato de instantâneo do modelo. A segunda opção é um pouco mais difícil e não pode ser usada em todos os cenários, mas também é mais limpa porque ela não envolve a adição de uma migração extra.

### <a name="option-1-add-a-blank-merge-migration"></a>Opção 1: Adicionar uma migração em branco 'merge'

Nessa opção nós geramos uma migração em branco exclusivamente para a finalidade de verificar se a migração mais recente tem o instantâneo do modelo correto armazenados nela.

Essa opção pode ser usada independentemente de quem gerou a última migração. No exemplo, vem acompanhando desenvolvedor \#2 é cuidar da mesclagem e que aconteceram gerar a última migração. Mas essas mesmas etapas podem ser usadas se desenvolvedor \#1 gerado a última migração. As etapas também se aplicam se houver várias migrações envolvidos – apenas usamos dois para manter a simplicidade.

O processo a seguir pode ser usado para essa abordagem, a partir do momento em que você perceba que você tem alterações que precisam ser sincronizados a partir do controle do código-fonte.

1.  Certifique-se de que as alterações do modelo pendentes em sua base de código local tem sido escritas para uma migração. Essa etapa garante que você não perca quaisquer alterações legítimas quando chega o momento para gerar a migração em branco.
2.  Sincronizar com o controle de origem.
3.  Execute **Update-Database** para aplicar quaisquer migrações novo que outros desenvolvedores tem check-in.
    **
    *Observação: * * * se você não obtém todos os avisos do comando Update-Database não havia nenhuma nova migração de outros desenvolvedores e não é necessário para executar qualquer ainda mais a mesclagem.*
4.  Execute **Add-Migration &lt;escolher\_um\_nome&gt; – IgnoreChanges** (por exemplo, **Add-Migration mesclar – IgnoreChanges**). Isso gera uma migração com todos os metadados (incluindo um instantâneo do modelo atual), mas ignorará todas as alterações que ele detecta ao comparar os modelos atuais para o instantâneo nas migrações último (que significa que você obtenha um espaço em branco **backup** e **Para baixo** método).
5.  Continuar a desenvolver ou envie ao controle de origem (depois de executar seu testes de unidade é claro).

Aqui está o estado do desenvolvedor \#2 local do código base depois de usar essa abordagem.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Opção 2: Atualizar o instantâneo do modelo em que a última migração

Essa opção é muito semelhante à opção 1, mas remove a migração de em branco extras porque vamos encarar os fatos, quem quer arquivos de código extra em sua solução.

**Essa abordagem só é viável se a última migração existe apenas na sua base de código local e ainda não foi enviada ao controle de origem (por exemplo, se a última migração foi gerada pelo usuário que está fazendo a mesclagem)**. Editar os metadados de migrações que outros desenvolvedores já aplicado ao seu banco de dados de desenvolvimento – ou até mesmo pior aplicado a um banco de dados de produção – pode resultar em efeitos colaterais inesperados. Durante o processo, vamos para reverter a última migração em nosso banco de dados local e aplique-o novamente com metadados atualizados.

Enquanto a última migração precisa apenas ser no código local base não existem restrições para o número ou a ordem das migrações que continuar a ele. Pode haver várias migrações de vários desenvolvedores diferentes e as mesmas etapas se aplicam – apenas usamos dois para manter a simplicidade.

O processo a seguir pode ser usado para essa abordagem, a partir do momento em que você perceba que você tem alterações que precisam ser sincronizados a partir do controle do código-fonte.

1.  Certifique-se de que as alterações do modelo pendentes em sua base de código local tem sido escritas para uma migração. Essa etapa garante que você não perca quaisquer alterações legítimas quando chega o momento para gerar a migração em branco.
2.  Sincronizar com o controle do código-fonte.
3.  Execute **Update-Database** para aplicar quaisquer migrações novo que outros desenvolvedores tem check-in.
    **
    *Observação: * * * se você não obtém todos os avisos do comando Update-Database não havia nenhuma nova migração de outros desenvolvedores e não é necessário para executar qualquer ainda mais a mesclagem.*
4.  Execute **Update-Database – TargetMigration &lt;segundo\_última\_migração&gt;**  (o exemplo estamos vem acompanhando, isso seria **Update-Database – TargetMigration AddRating**). Essa migração – efetivamente 'não aplicável' da última funções de banco de dados de volta para o estado do segundo a última migração do banco de dados.
    **
    *Observação: * * * esta etapa é necessária para torná-lo seguro editar os metadados da migração, pois os metadados também são armazenados na \_ \_MigrationsHistoryTable do banco de dados. É por isso você só deve usar essa opção se a última migração só estiver em sua base de código local. Se outros bancos de dados tinham a última migração aplicada você também teria revertê-las e aplicar novamente a última migração para atualizar os metadados.* 
5.  Execute **Add-Migration &lt;completa\_nome\_incluindo\_timestamp\_de\_última\_migração** &gt; (no exemplo podemos vem acompanhando isso seria algo como **Add-Migration 201311062215252\_AddReaders**).
    **
    *Observação: * * * você precisa incluir o carimbo de hora para que as migrações Saiba que você deseja editar a migração existente em vez de gerar automaticamente um novo.*
    Isso atualizará os metadados para a última migração coincidir com o modelo atual. Você obterá o seguinte aviso quando o comando for concluído, mas é exatamente o que você deseja. "*Somente o código de Designer para a migração ' 201311062215252\_'AddReaders foi gerado por scaffolding novamente. Para a migração de toda o scaffold novamente, use o parâmetro - Force. "*
6.  Execute **Update-Database** aplicar novamente a migração mais recente com os metadados atualizados.
7.  Continuar a desenvolver ou envie ao controle de origem (depois de executar seu testes de unidade é claro).

Aqui está o estado do desenvolvedor \#2 local do código base depois de usar essa abordagem.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Resumo

Há alguns desafios ao usar migrações do Code First em um ambiente de equipe. No entanto, um entendimento básico de como funciona a migrações e algumas abordagens simples para resolver conflitos de mesclagem tornam fácil superar esses desafios.

O problema fundamental é incorreto metadados armazenados na migração mais recente. Isso faz com que o Code First para detectar incorretamente que o modelo atual e o esquema de banco de dados não coincidem e criar o scaffolding de um código incorreto na próxima migração. Essa situação pode ser superada pela geração de uma migração em branco com o modelo correto ou atualizando os metadados na migração mais recente.
