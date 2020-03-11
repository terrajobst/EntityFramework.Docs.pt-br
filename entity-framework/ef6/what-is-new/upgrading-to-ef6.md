---
title: Atualizando para o Entity Framework 6-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419650"
---
# <a name="upgrading-to-entity-framework-6"></a>Atualizando para o Entity Framework 6

Nas versões anteriores do EF, o código foi dividido entre as bibliotecas principais (principalmente System. Data. Entity. dll) enviadas como parte do .NET Framework e das bibliotecas OOB (basicamente, o EntityFramework. dll) enviadas em um pacote NuGet. O EF6 leva o código das bibliotecas principais e incorpora-o às bibliotecas OOB. Isso era necessário para permitir que o EF se tornasse código aberto e para que ele pudesse evoluir em um ritmo diferente do .NET Framework. A consequência disso é que os aplicativos precisarão ser recriados em relação aos tipos movidos.

Isso deve ser direto para aplicativos que usam DbContext, conforme fornecido no EF 4,1 e posterior. Um pouco mais de trabalho é necessário para aplicativos que fazem uso de ObjectContext, mas ainda não é difícil fazer.

Aqui está uma lista de verificação das coisas que você precisa fazer para atualizar um aplicativo existente para o EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. instalar o pacote NuGet do EF6

Você precisa atualizar para o novo tempo de execução do Entity Framework 6.

1. Clique com o botão direito do mouse em seu projeto e selecione **gerenciar pacotes NuGet...**  
2. Na guia **online** , selecione **EntityFramework** e clique em **instalar**  
   > [!NOTE]
   > Se uma versão anterior do pacote NuGet do EntityFramework tiver sido instalada, ela será atualizada para EF6.

Como alternativa, você pode executar o seguinte comando no console do Gerenciador de pacotes:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Verifique se as referências de assembly a System. Data. Entity. dll foram removidas

A instalação do pacote NuGet do EF6 deve remover automaticamente todas as referências a System. Data. Entity do seu projeto para você.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. alternar modelos do EF designer (EDMX) para usar a geração de código do EF 6. x

Se você tiver modelos criados com o designer do EF, precisará atualizar os modelos de geração de código para gerar o código compatível com EF6.

> [!NOTE]
> Atualmente, há apenas os modelos de gerador do EF 6. x DbContext disponíveis para o Visual Studio 2012 e 2013.

1. Excluir modelos de geração de código existentes. Esses arquivos normalmente serão nomeados **\<edmx_file_name\>. tt** e **\<edmx_file_name\>. Context.tt** e estar aninhado em seu arquivo EDMX em Gerenciador de soluções. Você pode selecionar os modelos em Gerenciador de Soluções e pressionar a tecla **del** para excluí-los.  
   > [!NOTE]
   > Em projetos de site, os modelos não serão aninhados em seu arquivo EDMX, mas listados junto com ele em Gerenciador de Soluções.  

   > [!NOTE]
   > Em projetos do VB.NET, você precisará habilitar ' Mostrar todos os arquivos ' para poder ver os arquivos de modelo aninhados.
2. Adicione o modelo de geração de código do EF 6. x apropriado. Abra seu modelo no designer do EF, clique com o botão direito do mouse na superfície de design e selecione **Adicionar item de geração de código...**
    - Se você estiver usando a API DbContext (recomendado), o **gerador de DbContext do EF 6. x** estará disponível na guia **dados** .  
      > [!NOTE]
      > Se você estiver usando o Visual Studio 2012, será necessário instalar as ferramentas do EF 6 para ter esse modelo. Consulte [obter Entity Framework](~/ef6/fundamentals/install.md) para obter detalhes.  

    - Se você estiver usando a API ObjectContext, precisará selecionar a guia **online** e procurar por gerador de **EntityObject do EF 6. x**.  
3. Se você aplicou personalizações aos modelos de geração de código, precisará reaplicá-las aos modelos atualizados.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. atualizar namespaces para os tipos principais do EF que estão sendo usados

Os namespaces dos tipos DbContext e Code First não foram alterados. Isso significa que para muitos aplicativos que usam o EF 4,1 ou posterior, você não precisará alterar nada.

Tipos como ObjectContext que estavam anteriormente em System. Data. Entity. dll foram movidos para novos namespaces. Isso significa que talvez seja necessário atualizar suas diretivas *using* ou *Import* para criar com base no EF6.

A regra geral para alterações de namespace é que qualquer tipo em System. Data. * é movido para System. Data. Entity. Core. *. Em outras palavras, basta inserir **Entity. Core.** após System. Data. Por exemplo:

- System. Data. EntityException = > System. Data. **Entity. Core**. EntityException  
- System. Data. Objects. ObjectContext = > System. Data. **Entity. Core**. Objects. ObjectContext  
- System. Data. Objects. DataClasses. RelationshipManager = > System. Data. **Entity. Core**. Objects. DataClasses. RelationshipManager  

Esses tipos estão nos namespaces *principais* porque não são usados diretamente para a maioria dos aplicativos baseados em DbContext. Alguns tipos que faziam parte de System. Data. Entity. dll ainda são usados normalmente e diretamente para aplicativos baseados em DbContext e, portanto, não foram movidos para os namespaces *centrais* . Estes são:

- System. Data. EntityState = > System. Data. **Entidade**. EntityState  
- System. Data. Objects. DataClasses. EdmFunctionAttribute = > System. Data. **Entity. DbFunctionAttribute**  
  > [!NOTE]
  > Esta classe foi renomeada; uma classe com o nome antigo ainda existe e funciona, mas agora está marcada como obsoleta.  
- System. Data. Objects. EntityFunctions = > System. Data. **Entity. DbFunctions**  
  > [!NOTE]
  > Esta classe foi renomeada; uma classe com o nome antigo ainda existe e funciona, mas agora está marcada como obsoleta.)  
- As classes espaciais (por exemplo, DbGeography, DbGeometry) foram movidas de System. Data. espacial = > System. Data. **Entidade**. Espacial

> [!NOTE]
> Alguns tipos no namespace System. Data estão em System. Data. dll que não é um assembly do EF. Esses tipos não foram movidos e, portanto, seus namespaces permanecem inalterados.
