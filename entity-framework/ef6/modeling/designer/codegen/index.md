---
title: Modelos de geração de código do Designer — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: 29e28dd4ebe0e5e6e3cddacb1d34202c2010f389
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994865"
---
# <a name="designer-code-generation-templates"></a>Modelos de geração de código do Designer
Quando você cria um modelo usando o Entity Framework Designer, suas classes e os contextos derivados são automaticamente gerados. Além da geração de código padrão, também fornecemos vários modelos que podem ser usados para personalizar o código gerado. Esses modelos são fornecidos como Modelos de Texto T4, permitindo que você os personalize, se necessário.

O código gerado por padrão depende de em qual versão do Visual Studio o modelo é criado:

-   Modelos criados no Visual Studio 2012 e 2013 gerarão classes de entidade POCO simples e um contexto derivado do DbContext simplificado.
-   Modelos criados no Visual Studio 2010 gerarão classes de entidade derivadas de EntityObject e um contexto derivado de ObjectContext.
  > [!NOTE]    
  > É recomendável alternar para o modelo Gerador de DbContext depois de adicionar seu modelo.

Esta página aborda os modelos disponíveis e, em seguida, fornece instruções para adicionar um modelo ao seu modelo.

## <a name="available-templates"></a>Modelos disponíveis

Os seguintes modelos são fornecidos pela equipe do Entity Framework:

### <a name="dbcontext-generator"></a>Gerador DbContext

Esse modelo gerará classes de entidade POCO simples e um contexto derivado de DbContext usando o EF6.
Esse é o modelo recomendado a menos que você tenha um motivo para usar um dos outros modelos listados abaixo.
Ele também será o modelo de geração de código obtido por padrão se você estiver usando versões recentes do Visual Studio (Visual Studio 2013 em diante): quando se cria um modelo, ele é usado por padrão e os arquivos T4 (.tt) são aninhados sob seu arquivo .edmx.

#### <a name="older-versions-of-visual-studio"></a>Versões mais antigas do Visual Studio
- **Visual Studio 2012:** para obter os modelos **EF 6.x DbContextGenerator**, você precisará instalar a versão mais recente do **Entity Framework Tools para Visual Studio**. Consulte a página [Obter o Entity Framework](~/ef6/fundamentals/install.md) para encontrar mais informações.
- **Visual Studio 2010:** os modelos **EF 6.x DbContextGenerator** não estão disponíveis para Visual Studio 2010.

#### <a name="dbcontext-generator-for-ef-5x"></a>Gerador DbContext para EF 5.x

Se você estiver usando uma versão mais antiga do pacote NuGet EntityFramework (um com versão principal 5), precisará usar o modelo **Gerador DbContext do EF 5.x**.

Se você estiver usando o Visual Studio 2013 ou 2012, este modelo já estará instalado.

Se você estiver usando o Visual Studio 2010, precisará selecionar a guia **Online** ao adicionar o modelo para baixá-lo da Galeria do Visual Studio. Como alternativa, você pode instalar o modelo diretamente da Galeria do Visual Studio previamente. Como os modelos são incluídos em versões posteriores do Visual Studio, as versões na galeria podem ser instaladas apenas no Visual Studio 2010.

- [Gerador DbContext do EF 5.x para C#](http://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [Gerador DbContext do EF 5.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [Gerador DbContext do EF 5.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [Gerador DbContext do EF 5.x para sites VB.NET](http://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>Gerador DbContext para EF 4.x

Se você estiver usando uma versão mais antiga do pacote NuGet EntityFramework (um com versão principal 4), precisará usar o modelo **Gerador DbContext do EF 4.x**. Isso pode ser encontrado na guia **Online** ao adicionar o modelo ou você pode instalar o modelo diretamente da Galeria do Visual Studio previamente.

- [Gerador DbContext do EF 4.x para C#](http://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Gerador DbContext do EF 4.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [Gerador DbContext do EF 4.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [Gerador DbContext do EF 4.x para sites VB.NET](http://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>Gerador EntityObject

Este modelo gerará classes de entidade derivadas de EntityObject e um contexto derivado de ObjectContext.

> [!NOTE]
> Considere usar o Gerador DbContext

O Gerador DbContext é o modelo recomendado no momento para novos aplicativos. O Gerador DbContext aproveita a API DbContext mais simples. O Gerador EntityObject continua disponível para dar suporte aos aplicativos existentes.

**Visual Studio 2010, 2012 &amp; 2013**

Você precisará selecionar a guia **Online** ao adicionar o modelo para baixá-lo da Galeria do Visual Studio. Como alternativa, você pode instalar o modelo diretamente da Galeria do Visual Studio previamente.

- [Gerador EntityObject do EF 6.x para C#](http://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Gerador EntityObject do EF 6.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [Gerador EntityObject do EF 6.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [Gerador EntityObject do EF 6.x para sites em VB.NET](http://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**Gerador EntityObject para EF 5.x**


Se você estiver usando o Visual Studio 2012 ou 2013, precisará selecionar a guia **Online** ao adicionar o modelo para baixá-lo da Galeria do Visual Studio. Como alternativa, você pode instalar o modelo diretamente da Galeria do Visual Studio previamente. Como os modelos são incluídos no Visual Studio 2010, as versões na galeria podem ser instaladas apenas no Visual Studio 2012 &amp; 2013.

- [Gerador EntityObject do EF 5.x para C#](http://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Gerador EntityObject do EF 5.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [Gerador EntityObject do EF 5.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [Gerador EntityObject do EF 5.x para sites em VB.NET](http://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Se você quiser apenas a geração de código do ObjectContext sem precisar editar o modelo, poderá [reverter para a geração de código do EntityObject](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Se você estiver usando o Visual Studio 2010, este modelo já estará instalado. Se você criar um modelo no Visual Studio 2010, ele será usado por padrão, mas os arquivos .tt não serão incluídos no projeto. Se quiser personalizar o modelo, será necessário adicioná-lo ao projeto.

### <a name="self-tracking-entities-ste-generator"></a>Gerador de STE (Entidades de Rastreamento Automático)

Este modelo gerará classes de Entidade de Rastreamento Automático e um contexto derivado do ObjectContext. Em um aplicativo do EF, um contexto é responsável por controlar as alterações nas entidades. No entanto, em cenários de N camadas, o contexto pode não estar disponível na camada que modifica as entidades. As entidades de rastreamento automático ajudam você a rastrear as alterações em qualquer camada. Para obter mais informações, consulte [Entidades de rastreamento automático](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Modelo STE não recomendado

Não recomendamos usar o modelo STE em novos aplicativos, ele continua disponível para dar suporte aos aplicativos existentes. Visite o [artigo de entidades desconectadas](~/ef6/fundamentals/disconnected-entities/index.md) para outras opções recomendadas para cenários de N camadas.

> [!NOTE]
> Não há nenhuma versão do EF 6.x do modelo STE.


> [!NOTE]
> Não há nenhuma versão do Visual Studio 2013 do modelo STE.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Se você estiver usando o Visual Studio 2012, precisará selecionar a guia **Online** ao adicionar o modelo para baixá-lo da Galeria do Visual Studio. Como alternativa, você pode instalar o modelo diretamente da Galeria do Visual Studio previamente. Como os modelos são incluídos no Visual Studio 2010, as versões na galeria podem ser instaladas apenas no Visual Studio 2012.

- [Gerador de STE do EF 5.x para C#](http://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [Gerador de STE do EF 5.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [Gerador de STE do EF 5.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [Gerador de STE do EF 5.x para sites em VB.NET](http://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010**

Se você estiver usando o Visual Studio 2010, este modelo já estará instalado.

### <a name="poco-entity-generator"></a>Gerador de entidade POCO

Este modelo gerará classes de entidade POCO e um contexto derivado do ObjectContext

> [!NOTE]
> Considere usar o Gerador DbContext

O Gerador DbContext é o modelo recomendado no momento para gerar classes POCO em novos aplicativos. O Gerador DbContext aproveita a nova API DbContext e pode gerar classes POCO mais simples. O Gerador de entidade POCO continua disponível para dar suporte aos aplicativos existentes.

> [!NOTE]
> Não há nenhuma versão do EF 5.x ou do EF 6.x do modelo STE.

> [!NOTE]
> Não há nenhuma versão do Visual Studio 2013 do modelo POCO.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Você precisará selecionar a guia **Online** ao adicionar o modelo para baixá-lo da Galeria do Visual Studio. Como alternativa, você pode instalar o modelo diretamente da Galeria do Visual Studio previamente.

- [Gerador POCO do EF 4.x para C#](http://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Gerador POCO do EF 4.x para sites em C#](http://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [Gerador POCO do EF 4.x para VB.NET](http://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [Gerador POCO do EF 4.x para sites em VB.NET](http://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>O que são os modelos de “sites”

Os modelos de "sites" (por exemplo, sites do **Gerador DbContext do EF 5.x para C\#**) se destinam ao uso em projetos de site criados via **Arquivo –&gt; Novo –&gt; Site...**. Esses são diferentes dos aplicativos Web, criados via **Arquivo –&gt; Novo –&gt; Projeto...**, que usam os modelos padrão. Fornecemos modelos separados porque o sistema de modelo de item no Visual Studio os exige.

## <a name="using-a-template"></a>Usando um modelo

Para começar a usar um modelo de geração de código, clique com o botão direito do mouse em uma área vazia na superfície de design no EF Designer e selecione **Adicionar Item de Geração de Código...**.

![Add_Code_Gen_Item](~/ef6/media/add-code-gen-item.png)

Se você já tiver instalado o modelo que deseja usar (ou se ele estava incluído no Visual Studio), ele estará disponível na seção **Código** ou **Dados** do menu esquerdo.

![Instalados](~/ef6/media/installed.png)

Se você ainda não tiver o modelo instalado, selecione **Online** no menu esquerdo e pesquise o modelo desejado.

![Pesquisar](~/ef6/media/search.png) 

Se você estiver usando o Visual Studio 2012, os novos arquivos .tt serão aninhados no arquivo .edmx.*

> [!NOTE]
> Para modelos criados no Visual Studio 2012, você precisará excluir os modelos usados para a geração de código padrão, caso contrário, as classes e o contexto serão gerados de forma duplicada. Os arquivos padrão são **&lt;nome do modelo&gt;.tt** e **&lt;nome do modelo&gt;.context.tt**. 

![VS2012_Templates](~/ef6/media/vs2012-templates.png)

Se você estiver usando o Visual Studio 2010, os arquivos tt serão adicionados diretamente ao seu projeto.  

![VS2010_Templates](~/ef6/media/vs2010-templates.png)
