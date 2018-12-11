---
title: Atualizar para o Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 711f1940080de27bd23cb8f641a5c7f2711dd65b
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182001"
---
# <a name="upgrading-to-entity-framework-6"></a>Atualizar para o Entity Framework 6

Nas versões anteriores do EF o código foi dividido entre core (principalmente System.Data.Entity.dll) é fornecida como parte do .NET Framework e fora de banda (OOB) bibliotecas (principalmente EntityFramework) fornecidas em um pacote do NuGet. EF6 usa o código de bibliotecas do core e ele incorpora as bibliotecas OOB. Isso foi necessário para permitir que o EF sejam feitas de código-fonte aberto e para que elas sejam capazes de evoluir em um ritmo diferente do .NET Framework. A consequência disso é que aplicativos precisarão ser recriados com os tipos movidos.

Isso deve ser simples para aplicativos que usam DbContext como enviado no EF 4.1 e posterior. É necessário para aplicativos que fazem uso de ObjectContext um pouco mais trabalho, mas ele ainda não é difícil fazer.

Aqui está uma lista de verificação das coisas que você precisa fazer para atualizar um aplicativo existente para o EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Instale o pacote NuGet do EF6

Você precisará atualizar para o novo tempo de execução do Entity Framework 6.

1. Clique com botão direito no seu projeto e selecione **gerenciar pacotes NuGet...**  
2. Sob o **Online** guia select **EntityFramework** e clique em **instalar**  
   > [!NOTE]
   > Se uma versão anterior do pacote foi instalado o EntityFramework NuGet isso irá atualizá-lo para o EF6.

Como alternativa, você pode executar o comando a seguir no Console do Gerenciador de pacotes:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Certifique-se de que as referências de assembly para System.Data.Entity.dll são removidas

Instalar o pacote NuGet do EF6 automaticamente deve remover todas as referências a System do seu projeto para você.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Troque os modelos EF Designer (EDMX) para usar a geração de código do EF 6.x

Se você tiver todos os modelos criados com o EF Designer, você precisará atualizar os modelos de geração de código para gerar código compatível do EF6.

> [!NOTE]
> Atualmente, há apenas EF 6.x gerador DbContext modelos disponíveis para o Visual Studio 2012 e 2013.

1. Exclua modelos de geração de código existentes. Esses arquivos geralmente serão denominados  **\<edmx_file_name\>. TT** e  **\<edmx_file_name\>. Context.TT** e estar aninhado em seu arquivo edmx no Gerenciador de soluções. Você pode selecionar os modelos no Gerenciador de soluções e pressione a **/DEL** chave excluí-los.  
   > [!NOTE]
   > Em projetos de Site os modelos serão não aninhados em seu arquivo edmx, mas listados juntamente com ele no Gerenciador de soluções.  

   > [!NOTE]
   > Os projetos VB.NET, você precisará habilitar 'Show All Files' ser capaz de ver os arquivos de modelo aninhado.
2. Adicione o modelo de geração de código do apropriado EF 6.x. Abra seu modelo no Designer de EF, clique com botão direito na superfície do design e selecione **Adicionar Item de geração de código...**
    - Se você estiver usando a API DbContext (recomendado), em seguida, **EF 6.x gerador DbContext** estará disponível sob o **dados** guia.  
      > [!NOTE]
      > Se você estiver usando o Visual Studio 2012, você precisará instalar as ferramentas do EF 6 para este modelo. Ver [obter o Entity Framework](~/ef6/fundamentals/install.md) para obter detalhes.  

    - Se você estiver usando a API do ObjectContext, você precisará selecionar o **Online** guia e pesquise **EF 6.x gerador de EntityObject**.  
3. Se você aplicou todas as personalizações para os modelos de geração de código, você precisará aplicá-los novamente para os modelos atualizados.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Atualizar os namespaces para alguns tipos do EF core que está sendo usados

Os namespaces para os tipos DbContext e Code First não foram alterados. Isso significa que para muitos aplicativos que usam o EF 4.1 ou posterior não será preciso alterar nada.

Tipos como ObjectContext que estavam anteriormente no System.Data.Entity.dll foram movidos para os novos namespaces. Isso significa que talvez você precise atualizar seu *usando* ou *importação* diretivas para compilar em relação a EF6.

A regra geral para as alterações de namespace é que qualquer tipo em System.Data.* é movido para System.Data.Entity.Core.*. Em outras palavras, basta inserir **Entity.Core.** Depois de System. Data. Por exemplo:

- System.Data.EntityException = > System. Data. **Entity.Core**. EntityException  
- System.Data.Objects.ObjectContext = > System. Data. **Entity.Core**. Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System. Data. **Entity.Core**. Objects.DataClasses.RelationshipManager  

Esses tipos estão na *Core* namespaces porque eles não são usados diretamente para a maioria dos aplicativos baseados em DbContext. Alguns tipos que faziam parte do System.Data.Entity.dll ainda são usados comumente e diretamente para aplicativos baseados em DbContext e portanto, não foram movidos para o *Core* namespaces. Elas são:

- System.Data.EntityState = > System. Data. **Entidade**. EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System. Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Essa classe foi renomeada; uma classe com o nome antigo ainda existe e está funcionando, mas agora marcado como obsoleto.  
- System.Data.Objects.EntityFunctions = > System. Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Essa classe foi renomeada; uma classe com o nome antigo ainda existe e está funcionando, mas ele agora marcado como obsoleto.)  
- Classes espaciais (por exemplo, DbGeography, DbGeometry) foram movidos do Spatial = > System. Data. **Entidade**. Espacial

> [!NOTE]
> Alguns tipos no namespace System. Data estão na DLL que não é um assembly do EF. Esses tipos não movidos e então seus namespaces permanecem inalterados.
