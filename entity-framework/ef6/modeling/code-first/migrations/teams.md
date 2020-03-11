---
title: Migrações do Code First em ambientes de equipe-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: b3c4c35d636caf4ddd251dd78e026587abc57d42
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418885"
---
# <a name="code-first-migrations-in-team-environments"></a>Migrações do Code First em ambientes de equipe
> [!NOTE]
> Este artigo pressupõe que você saiba como usar Migrações do Code First em cenários básicos. Caso contrário, você precisará ler [migrações do Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Pegue um café, você precisa ler este artigo inteiro

Os problemas em ambientes de equipe estão em grande parte da mesclagem de migrações quando dois desenvolvedores geraram migrações em sua base de código local. Embora as etapas para solucioná-las sejam bem simples, elas exigem uma compreensão sólida de como as migrações funcionam. Não pule até o final – Reserve um tempo para ler o artigo inteiro para garantir que você tenha êxito.

## <a name="some-general-guidelines"></a>Algumas diretrizes gerais

Antes de nos aprofundarmos em como gerenciar as migrações de mesclagem geradas por vários desenvolvedores, aqui estão algumas diretrizes gerais para configurá-lo para o sucesso.

### <a name="each-team-member-should-have-a-local-development-database"></a>Cada membro da equipe deve ter um banco de dados de desenvolvimento local

As migrações usam o **\_\_tabela MigrationsHistory** para armazenar quais migrações foram aplicadas ao banco de dados. Se você tiver vários desenvolvedores gerando diferentes migrações ao tentar direcionar o mesmo banco de dados (e, portanto, compartilhar um **\_\_tabela MigrationsHistory** ), o ficará muito confuso.

É claro que, se você tiver membros da equipe que não estão gerando migrações, não há nenhum problema em compartilhar um banco de dados de desenvolvimento central.

### <a name="avoid-automatic-migrations"></a>Evitar migrações automáticas

O resultado final é que as migrações automáticas parecem ser boas inicialmente em ambientes de equipe, mas, na realidade, elas simplesmente não funcionam. Se você quiser saber por que, continue lendo – caso contrário, poderá pular para a próxima seção.

As migrações automáticas permitem que você tenha seu esquema de banco de dados atualizado para corresponder ao modelo atual sem a necessidade de gerar arquivos de código (migrações baseadas em código). As migrações automáticas funcionariam muito bem em um ambiente de equipe se você as utilizou apenas e nunca geraram nenhuma migração baseada em código. O problema é que as migrações automáticas são limitadas e não lidam com várias operações – renomeações de propriedade/coluna, movimentação de dados para outra tabela, etc. Para lidar com esses cenários, você acaba gerando migrações baseadas em código (e editando o código com scaffold) que são misturadas entre as alterações que são manipuladas por migrações automáticas. Isso o torna quase impossível de mesclar alterações quando dois desenvolvedores verificam as migrações.

## <a name="screencasts"></a>Screencasts

Se você preferir assistir a um screencast do que ler este artigo, os dois vídeos a seguir abrangem o mesmo conteúdo deste artigo.

### <a name="video-one-migrations---under-the-hood"></a>Vídeo um: "migrações-nos bastidores"

[Este screencast](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) aborda como as migrações acompanham e usam informações sobre o modelo para detectar alterações de modelo.

### <a name="video-two-migrations---team-environments"></a>Vídeo dois: "migrações-ambientes de equipe"

Aproveitando os conceitos do vídeo anterior, [Este screencast](https://channel9.msdn.com/blogs/ef/migrations-team-environments) aborda os problemas que surgem em um ambiente de equipe e como resolvê-los.

## <a name="understanding-how-migrations-works"></a>Compreendendo como funcionam as migrações

A chave para o uso bem-sucedido de migrações em um ambiente de equipe é uma compreensão básica de como as migrações acompanham e usam informações sobre o modelo para detectar alterações no modelo.

### <a name="the-first-migration"></a>A primeira migração

Ao adicionar a primeira migração ao seu projeto, você executa algo como **Add-Migration primeiro** no console do Gerenciador de pacotes. As etapas de alto nível que esse comando executa estão configuradas abaixo.

![Primeira migração](~/ef6/media/firstmigration.png)

O modelo atual é calculado a partir do seu código (1). Os objetos de banco de dados necessários são então calculados pelo modelo de diferença (2) – já que essa é a primeira migração que o modelo difere apenas usa um modelo vazio para a comparação. As alterações necessárias são passadas para o gerador de código para criar o código de migração necessário (3), que é então adicionado à sua solução do Visual Studio (4).

Além do código de migração real que é armazenado no arquivo de código principal, as migrações também geram alguns arquivos code-behind adicionais. Esses arquivos são metadados usados por migrações e não são algo que você deve editar. Um desses arquivos é um arquivo de recurso (. resx) que contém um instantâneo do modelo no momento em que a migração foi gerada. Você verá como isso é usado na próxima etapa.

Neste ponto, você provavelmente executaria **Update-Database** para aplicar suas alterações ao banco de dados e, em seguida, implementar outras áreas do seu aplicativo.

### <a name="subsequent-migrations"></a>Migrações subsequentes

Mais tarde, você voltará e fará algumas alterações em seu modelo – em nosso exemplo, adicionaremos uma propriedade de **URL** ao **blog**. Em seguida, você emitiria um comando como **Add-Migration addurl** para Scaffold uma migração para aplicar as alterações de banco de dados correspondentes. As etapas de alto nível que esse comando executa estão configuradas abaixo.

![Segunda migração](~/ef6/media/secondmigration.png)

Assim como na última vez, o modelo atual é calculado a partir do código (1). No entanto, desta vez há migrações existentes para que o modelo anterior seja recuperado da migração mais recente (2). Esses dois modelos são diferenciados para localizar as alterações de banco de dados necessárias (3) e, em seguida, o processo é concluído como antes.

Esse mesmo processo é usado para outras migrações que você adicionar ao projeto.

### <a name="why-bother-with-the-model-snapshot"></a>Por que se preocupar com o instantâneo do modelo?

Você deve estar se perguntando por que o EF nos dois com o instantâneo do modelo – por que não apenas examinar o banco de dados. Em caso afirmativo, continue lendo. Se você não estiver interessado, poderá ignorar esta seção.

Há vários motivos pelos quais o EF mantém o instantâneo do modelo:

-   Ele permite que seu banco de dados seja descompasso do modelo do EF. Essas alterações podem ser feitas diretamente no banco de dados ou você pode alterar o código com Scaffold em suas migrações para fazer as alterações. Aqui estão alguns exemplos disso na prática:
    -   Você deseja adicionar uma coluna inserted e Updated to a uma ou mais de suas tabelas, mas não deseja incluir essas colunas no modelo do EF. Se as migrações tiverem examinado o banco de dados, ele tentaria continuamente descartar essas colunas toda vez que você com Scaffold uma migração. Usando o instantâneo do modelo, o EF nunca detectará apenas alterações legítimas no modelo.
    -   Você deseja alterar o corpo de um procedimento armazenado usado para que as atualizações incluam algum log. Se as migrações examinarem esse procedimento armazenado do banco de dados, ele tentaria continuamente e redefini-lo de volta para a definição esperada pelo EF. Usando o instantâneo do modelo, o EF apenas Scaffold código para alterar o procedimento armazenado quando você altera a forma do procedimento no modelo do EF.
    -   Esses mesmos princípios se aplicam à adição de índices extras, incluindo tabelas extras em seu banco de dados, mapeando o EF para uma exibição de banco de dados que fica em uma tabela, etc.
-   O modelo do EF contém mais do que apenas a forma do banco de dados. Ter todo o modelo permite que as migrações examinem as informações sobre as propriedades e as classes em seu modelo e como elas são mapeadas para as colunas e tabelas. Essas informações permitem que as migrações sejam mais inteligentes no código que aplica Scaffold. Por exemplo, se você alterar o nome da coluna que uma propriedade mapeia para migrações pode detectar a renomeação, vendo que é a mesma propriedade – algo que não pode ser feito se você tiver apenas o esquema de banco de dados. 

## <a name="what-causes-issues-in-team-environments"></a>O que causa problemas em ambientes de equipe

O fluxo de trabalho abordado na seção anterior funciona muito bem quando você é um único desenvolvedor trabalhando em um aplicativo. Ele também funciona bem em um ambiente de equipe se você for a única pessoa fazendo alterações no modelo. Nesse cenário, você pode fazer alterações no modelo, gerar migrações e enviá-las ao controle do código-fonte. Outros desenvolvedores podem sincronizar suas alterações e executar **Update-Database** para que as alterações de esquema sejam aplicadas.

Os problemas começam a surgir quando você tem vários desenvolvedores fazendo alterações no modelo do EF e enviando ao controle do código-fonte ao mesmo tempo. O que o EF não tem é uma forma de primeira classe de mesclar suas migrações locais com migrações que outro desenvolvedor enviou ao controle do código-fonte desde a última sincronização.

## <a name="an-example-of-a-merge-conflict"></a>Um exemplo de um conflito de mesclagem

Primeiro, vamos dar uma olhada em um exemplo concreto desse conflito de mesclagem. Continuaremos com o exemplo que vimos anteriormente. Como ponto de partida, vamos supor que as alterações da seção anterior foram verificadas pelo desenvolvedor original. Acompanharemos dois desenvolvedores à medida que fizerem alterações na base de código.

Acompanharemos o modelo do EF e as migrações por meio de várias alterações. Para um ponto de partida, ambos os desenvolvedores sincronizaram com o repositório de controle do código-fonte, conforme descrito no gráfico a seguir.

![Ponto de partida](~/ef6/media/startingpoint.png)

O desenvolvedor \#1 e desenvolvedor \#2 agora faz algumas alterações no modelo do EF em sua base de código local. O desenvolvedor \#1 adiciona uma propriedade de **classificação** ao **blog** – e gera uma migração **addrating** para aplicar as alterações ao banco de dados. O desenvolvedor \#2 adiciona uma propriedade **leitores** ao **blog** – e gera a migração de **addreaders** correspondente. Ambos os desenvolvedores executam **Update-Database**, para aplicar as alterações aos seus bancos de dados locais e, em seguida, continuar desenvolvendo o aplicativo.

> [!NOTE]
> As migrações são prefixadas com um carimbo de data/hora, portanto, nosso gráfico representa que a migração de addreaders do desenvolvedor \#2 vem após a migração de addrating do desenvolvedor \#1. Se o desenvolvedor \#1 ou \#2 gerou a migração, primeiro não faz diferença com os problemas de trabalho em uma equipe, ou o processo para mesclá-los que veremos na próxima seção.

![Alterações locais](~/ef6/media/localchanges.png)

É um dia de sorte para o desenvolvedor \#1, pois eles acontecem para enviar suas alterações primeiro. Como ninguém mais fez check-in porque eles sincronizaram seu repositório, eles podem apenas enviar suas alterações sem executar nenhuma mesclagem.

![Enviar](~/ef6/media/submit.png)

Agora é hora para o desenvolvedor \#2 enviar. Eles não são tanto sorte. Como outra pessoa enviou alterações desde que elas foram sincronizadas, elas precisarão retirar as alterações e mesclar. O sistema de controle do código-fonte provavelmente será capaz de mesclar automaticamente as alterações no nível de código, pois elas são muito simples. O estado do repositório local do desenvolvedor \#2 após a sincronização é representado no gráfico a seguir. 

![Recepção](~/ef6/media/pull.png)

Neste estágio, o desenvolvedor \#2 pode executar **Update-Database** , que detectará a nova migração **addrating** (que não foi aplicada ao banco de dados do desenvolvedor \#2) e a aplicará. Agora, a coluna de **classificação** é adicionada à tabela **Blogs** e o banco de dados está em sincronia com o modelo.

No entanto, há alguns problemas:

1.  Embora **Update-Database** aplique a migração **addrating** , ele também emitirá um aviso: *não é possível atualizar o banco de dados para corresponder ao modelo atual porque há alterações pendentes e a migração automática está desabilitada...*
    O problema é que o instantâneo do modelo armazenado na última migração (**Addreader**) não tem a propriedade **rating** no **blog** (pois ele não era parte do modelo quando a migração foi gerada). Code First detecta que o modelo na última migração não corresponde ao modelo atual e gera o aviso.
2.  A execução do aplicativo resultaria em uma InvalidOperationException informando que "*o modelo que está fazendo backup do contexto ' BloggingContext ' foi alterado desde que o banco de dados foi criado. Considere o uso de Migrações do Code First para atualizar o banco de dados... "*
    Novamente, o problema é que o instantâneo do modelo armazenado na última migração não corresponde ao modelo atual.
3.  Por fim, esperamos que **a execução de Add-Migration** agora gere uma migração vazia (já que não há nenhuma alteração a ser aplicada ao banco de dados). Mas como as migrações comparam o modelo atual com o da última migração (que não tem a propriedade **rating** ), na verdade, ela Scaffold outra chamada **AddColumn** para adicionar na coluna de **classificação** . É claro que essa migração falhará durante **Update-Database** porque a coluna de **classificação** já existe.

## <a name="resolving-the-merge-conflict"></a>Resolvendo o conflito de mesclagem

A boa notícia é que não é muito difícil lidar com a mesclagem manualmente – desde que você tenha uma compreensão de como as migrações funcionam. Então, se você tiver pulado para esta seção... Você precisa voltar e ler o restante do artigo primeiro!

Há duas opções, a mais fácil é gerar uma migração em branco que tenha o modelo atual correto como um instantâneo. A segunda opção é atualizar o instantâneo na última migração para ter o instantâneo de modelo correto. A segunda opção é um pouco mais difícil e não pode ser usada em todos os cenários, mas também é mais limpa porque não envolve a adição de uma migração extra.

### <a name="option-1-add-a-blank-merge-migration"></a>Opção 1: adicionar uma migração em branco de "mesclagem"

Nessa opção, geramos uma migração em branco exclusivamente para fins de garantir que a migração mais recente tenha o instantâneo de modelo correto armazenado nela.

Essa opção pode ser usada independentemente de quem gerou a última migração. No exemplo, estamos seguindo o desenvolvedor \#2 está tomando cuidado com a mesclagem e eles ocorreram para gerar a última migração. Mas essas mesmas etapas podem ser usadas se o desenvolvedor \#1 gerou a última migração. As etapas também se aplicam se houver várias migrações envolvidas – nós apenas examinamos duas para simplificar.

O processo a seguir pode ser usado para essa abordagem, desde o momento em que você perceber que tem alterações que precisam ser sincronizadas do controle do código-fonte.

1.  Verifique se as alterações de modelo pendentes na sua base de código local foram gravadas em uma migração. Essa etapa garante que você não perca nenhuma alteração legítima quando chegar a hora de gerar a migração em branco.
2.  Sincronizar com controle do código-fonte.
3.  Execute **Update-Database** para aplicar as novas migrações nas quais os outros desenvolvedores fizeram check-in.
    **_Observação:_** *se você não receber nenhum aviso do comando Update-Database, não haverá nenhuma nova migração de outros desenvolvedores e não haverá necessidade de realizar nenhuma mesclagem.*
4.  Execute **o &lt;de adição de migração\_um nome de\_&gt; – IgnoreChanges** (por exemplo, **Merge-Migrate – IgnoreChanges**). Isso gera uma migração com todos os metadados (incluindo um instantâneo do modelo atual), mas ignorará as alterações detectadas ao comparar o modelo atual com o instantâneo nas últimas migrações (o que significa que você obtém um método **ativo** e **inativo** ).
5.  Continue desenvolvendo ou envie para o controle do código-fonte (depois de executar os testes de unidade, é claro).

Aqui está o estado da base de código local do desenvolvedor \#2 depois de usar essa abordagem.

![Migração de mesclagem](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Opção 2: atualizar o instantâneo do modelo na última migração

Essa opção é muito semelhante à opção 1, mas remove a migração em branco extra – porque vamos encarar os arquivos de código extras em sua solução.

**Essa abordagem só é viável se a migração mais recente existir apenas na sua base de código local e ainda não tiver sido enviada ao controle do código-fonte (por exemplo, se a última migração foi gerada pelo usuário que faz a mesclagem)** . Editar os metadados de migrações que outros desenvolvedores podem já ter aplicado ao seu banco de dados de desenvolvimento, ou ainda pior aplicado a um banco de dados de produção, pode resultar em efeitos colaterais inesperados. Durante o processo, vamos reverter a última migração em nosso banco de dados local e aplicá-la novamente com metadados atualizados.

Embora a última migração precise apenas estar na base de código local, não há restrições para o número ou a ordem das migrações que o executam. Pode haver várias migrações de vários desenvolvedores diferentes e as mesmas etapas se aplicam – nós apenas examinamos duas para simplificar.

O processo a seguir pode ser usado para essa abordagem, desde o momento em que você perceber que tem alterações que precisam ser sincronizadas do controle do código-fonte.

1.  Verifique se as alterações de modelo pendentes na sua base de código local foram gravadas em uma migração. Essa etapa garante que você não perca nenhuma alteração legítima quando chegar a hora de gerar a migração em branco.
2.  Sincronizar com o controle do código-fonte.
3.  Execute **Update-Database** para aplicar as novas migrações nas quais os outros desenvolvedores fizeram check-in.
    **_Observação:_** *se você não receber nenhum aviso do comando Update-Database, não haverá nenhuma nova migração de outros desenvolvedores e não haverá necessidade de realizar nenhuma mesclagem.*
4.  Execute **Update-Database – TargetMigration &lt;segundo\_última\_&gt;de migração** (no exemplo que estamos seguindo, seria **Update-Database – TargetMigration addrating**). Isso faz com que o banco de dados volte para o estado da segunda última migração – efetivamente ' reaplicando ' a última migração do banco de dados.
    **_Observação:_** *essa etapa é necessária para tornar seguro editar os metadados da migração, já que os metadados também são armazenados no \_\_MigrationsHistoryTable do banco de dados. É por isso que você só deve usar essa opção se a última migração for apenas em sua base de código local. Se outros bancos de dados tiverem a última migração aplicada, você também teria que redistribuí-los e reaplicar a última migração para atualizar os metadados.* 
5.  Execute **Add-Migration &lt;nome de\_completo\_incluindo\_carimbo de data/hora\_do\_última\_de migração** (no exemplo, estamos seguindo isso seria algo como **Add-migration 201311062215252&gt; addreaders**).\_
    **_Observação:_** *você precisa incluir o carimbo de data/hora para que as migrações saibam que você deseja editar a migração existente em vez de scaffolding uma nova.*
    Isso atualizará os metadados para a última migração para corresponder ao modelo atual. Você receberá o seguinte aviso quando o comando for concluído, mas isso é exatamente o que você deseja. "*Somente o código do designer para migração ' 201311062215252\_Addreaders ' foi recom Scaffold. Para rescaffoldr toda a migração, use o parâmetro-Force. "*
6.  Execute **Update-Database** para aplicar novamente a migração mais recente com os metadados atualizados.
7.  Continue desenvolvendo ou envie para o controle do código-fonte (depois de executar os testes de unidade, é claro).

Aqui está o estado da base de código local do desenvolvedor \#2 depois de usar essa abordagem.

![Metadados atualizados](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Resumo

Há alguns desafios ao usar Migrações do Code First em um ambiente de equipe. No entanto, uma compreensão básica de como as migrações funciona e algumas abordagens simples para resolver conflitos de mesclagem facilita superar esses desafios.

O problema fundamental são os metadados incorretos armazenados na última migração. Isso faz com que Code First detecte incorretamente que o modelo atual e o esquema de banco de dados não correspondem e Scaffold o código incorreto na próxima migração. Essa situação pode ser superada com a geração de uma migração em branco com o modelo correto ou a atualização dos metadados na migração mais recente.
