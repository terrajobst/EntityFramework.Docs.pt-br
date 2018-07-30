---
title: Atualizar para o Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
caps.latest.revision: 3
ms.openlocfilehash: d22f0d686e6b8e3922d37245386af7723bf08ba1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39120097"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="e8561-102">Atualizar para o Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e8561-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="e8561-103">Nas versões anteriores do EF o código foi dividido entre core (principalmente System.Data.Entity.dll) é fornecida como parte do .NET Framework e fora de banda (OOB) bibliotecas (principalmente EntityFramework) fornecidas em um pacote do NuGet.</span><span class="sxs-lookup"><span data-stu-id="e8561-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="e8561-104">EF6 usa o código de bibliotecas do core e ele incorpora as bibliotecas OOB.</span><span class="sxs-lookup"><span data-stu-id="e8561-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="e8561-105">Isso foi necessário para permitir que o EF sejam feitas de código-fonte aberto e para que elas sejam capazes de evoluir em um ritmo diferente do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e8561-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="e8561-106">A consequência disso é que aplicativos precisarão ser recriados com os tipos movidos.</span><span class="sxs-lookup"><span data-stu-id="e8561-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="e8561-107">Isso deve ser simples para aplicativos que usam DbContext como enviado no EF 4.1 e posterior.</span><span class="sxs-lookup"><span data-stu-id="e8561-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="e8561-108">É necessário para aplicativos que fazem uso de ObjectContext um pouco mais trabalho, mas ele ainda não é difícil fazer.</span><span class="sxs-lookup"><span data-stu-id="e8561-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="e8561-109">Aqui está uma lista de verificação das coisas que você precisa fazer para atualizar um aplicativo existente para o EF6.</span><span class="sxs-lookup"><span data-stu-id="e8561-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="e8561-110">1. Instale o pacote NuGet do EF6</span><span class="sxs-lookup"><span data-stu-id="e8561-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="e8561-111">Você precisará atualizar para o novo tempo de execução do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e8561-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="e8561-112">Clique com botão direito no seu projeto e selecione **gerenciar pacotes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="e8561-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="e8561-113">Sob o **Online** guia select **EntityFramework** e clique em **instalar**</span><span class="sxs-lookup"><span data-stu-id="e8561-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="e8561-114">Se uma versão anterior do pacote foi instalado o EntityFramework NuGet isso irá atualizá-lo para o EF6.</span><span class="sxs-lookup"><span data-stu-id="e8561-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="e8561-115">Como alternativa, você pode executar o comando a seguir no Console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="e8561-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="e8561-116">2. Certifique-se de que as referências de assembly para System.Data.Entity.dll são removidas</span><span class="sxs-lookup"><span data-stu-id="e8561-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="e8561-117">Instalar o pacote NuGet do EF6 automaticamente deve remover todas as referências a System do seu projeto para você.</span><span class="sxs-lookup"><span data-stu-id="e8561-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="e8561-118">3. Troque os modelos EF Designer (EDMX) para usar a geração de código do EF 6.x</span><span class="sxs-lookup"><span data-stu-id="e8561-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="e8561-119">Se você tiver todos os modelos criados com o EF Designer, você precisará atualizar os modelos de geração de código para gerar código compatível do EF6.</span><span class="sxs-lookup"><span data-stu-id="e8561-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="e8561-120">Atualmente, há apenas EF 6.x gerador DbContext modelos disponíveis para o Visual Studio 2012 e 2013.</span><span class="sxs-lookup"><span data-stu-id="e8561-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="e8561-121">Exclua modelos de geração de código existentes.</span><span class="sxs-lookup"><span data-stu-id="e8561-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="e8561-122">Esses arquivos geralmente serão denominados  **\<edmx_file_name\>. TT** e  **\<edmx_file_name\>. Context.TT** e estar aninhado em seu arquivo edmx no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="e8561-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="e8561-123">Você pode selecionar os modelos no Gerenciador de soluções e pressione a **/DEL** chave excluí-los.</span><span class="sxs-lookup"><span data-stu-id="e8561-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="e8561-124">Em projetos de Site os modelos serão não aninhados em seu arquivo edmx, mas listados juntamente com ele no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="e8561-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="e8561-125">Os projetos VB.NET, você precisará habilitar 'Show All Files' ser capaz de ver os arquivos de modelo aninhado.</span><span class="sxs-lookup"><span data-stu-id="e8561-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="e8561-126">Adicione o modelo de geração de código do apropriado EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="e8561-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="e8561-127">Abra seu modelo no Designer de EF, clique com botão direito na superfície do design e selecione **Adicionar Item de geração de código...**</span><span class="sxs-lookup"><span data-stu-id="e8561-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="e8561-128">Se você estiver usando a API DbContext (recomendado), em seguida, **EF 6.x gerador DbContext** estará disponível sob o **dados** guia.</span><span class="sxs-lookup"><span data-stu-id="e8561-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="e8561-129">Se você estiver usando o Visual Studio 2012, você precisará instalar as ferramentas do EF 6 para este modelo.</span><span class="sxs-lookup"><span data-stu-id="e8561-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="e8561-130">Ver [obter o Entity Framework](~/ef6/fundamentals/install.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="e8561-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="e8561-131">Se você estiver usando a API do ObjectContext, você precisará selecionar o **Online** guia e pesquise **EF 6.x gerador de EntityObject**.</span><span class="sxs-lookup"><span data-stu-id="e8561-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="e8561-132">Se você aplicou todas as personalizações para os modelos de geração de código, você precisará aplicá-los novamente para os modelos atualizados.</span><span class="sxs-lookup"><span data-stu-id="e8561-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="e8561-133">4. Atualizar os namespaces para alguns tipos do EF core que está sendo usados</span><span class="sxs-lookup"><span data-stu-id="e8561-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="e8561-134">Os namespaces para os tipos DbContext e Code First não foram alterados.</span><span class="sxs-lookup"><span data-stu-id="e8561-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="e8561-135">Isso significa que para muitos aplicativos que usam o EF 4.1 ou posterior não será preciso alterar nada.</span><span class="sxs-lookup"><span data-stu-id="e8561-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="e8561-136">Tipos como ObjectContext que estavam anteriormente no System.Data.Entity.dll foram movidos para os novos namespaces.</span><span class="sxs-lookup"><span data-stu-id="e8561-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="e8561-137">Isso significa que talvez você precise atualizar seu *usando* ou *importação* diretivas para compilar em relação a EF6.</span><span class="sxs-lookup"><span data-stu-id="e8561-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="e8561-138">A regra geral para as alterações de namespace é que qualquer tipo em System.Data.* é movido para System.Data.Entity.Core.*.</span><span class="sxs-lookup"><span data-stu-id="e8561-138">The general rule for namespace changes is that any type in System.Data.* is moved to System.Data.Entity.Core.*.</span></span> <span data-ttu-id="e8561-139">Em outras palavras, basta inserir **Entity.Core.**</span><span class="sxs-lookup"><span data-stu-id="e8561-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="e8561-140">Depois de System. Data.</span><span class="sxs-lookup"><span data-stu-id="e8561-140">after System.Data.</span></span> <span data-ttu-id="e8561-141">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="e8561-141">For example:</span></span>

- <span data-ttu-id="e8561-142">System.Data.EntityException = > System. Data. **Entity.Core.** EntityException</span><span class="sxs-lookup"><span data-stu-id="e8561-142">System.Data.EntityException => System.Data.**Entity.Core.** EntityException</span></span>  
- <span data-ttu-id="e8561-143">System.Data.Objects.ObjectContext = > System. Data. **Entity.Core.** Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="e8561-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core.** Objects.ObjectContext</span></span>  
- <span data-ttu-id="e8561-144">System.Data.Objects.DataClasses.RelationshipManager = > System. Data. **Entity.Core.** Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="e8561-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core.** Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="e8561-145">Esses tipos estão na *Core* namespaces porque eles não são usados diretamente para a maioria dos aplicativos baseados em DbContext.</span><span class="sxs-lookup"><span data-stu-id="e8561-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="e8561-146">Alguns tipos que faziam parte do System.Data.Entity.dll ainda são usados comumente e diretamente para aplicativos baseados em DbContext e portanto, não foram movidos para o *Core* namespaces.</span><span class="sxs-lookup"><span data-stu-id="e8561-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="e8561-147">Elas são:</span><span class="sxs-lookup"><span data-stu-id="e8561-147">These are:</span></span>

- <span data-ttu-id="e8561-148">System.Data.EntityState = > System. Data. **Entidade.** EntityState</span><span class="sxs-lookup"><span data-stu-id="e8561-148">System.Data.EntityState => System.Data.**Entity.** EntityState</span></span>  
- <span data-ttu-id="e8561-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > System. Data. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="e8561-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="e8561-150">Essa classe foi renomeada; uma classe com o nome antigo ainda existe e está funcionando, mas agora marcado como obsoleto.</span><span class="sxs-lookup"><span data-stu-id="e8561-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="e8561-151">System.Data.Objects.EntityFunctions = > System. Data. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="e8561-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="e8561-152">Essa classe foi renomeada; uma classe com o nome antigo ainda existe e está funcionando, mas ele agora marcado como obsoleto.)</span><span class="sxs-lookup"><span data-stu-id="e8561-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="e8561-153">Classes espaciais (por exemplo, DbGeography, DbGeometry) foram movidos do Spatial = > System. Data. **Entidade.** Espacial</span><span class="sxs-lookup"><span data-stu-id="e8561-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity.** Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="e8561-154">Alguns tipos no namespace System. Data estão na DLL que não é um assembly do EF.</span><span class="sxs-lookup"><span data-stu-id="e8561-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="e8561-155">Esses tipos não movidos e então seus namespaces permanecem inalterados.</span><span class="sxs-lookup"><span data-stu-id="e8561-155">These types have not moved and so their namespaces remain unchanged.</span></span>