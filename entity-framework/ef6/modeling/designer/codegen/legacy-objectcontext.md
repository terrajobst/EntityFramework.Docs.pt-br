---
title: Revertendo para ObjectContext em Entity Framework Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418654"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Revertendo para ObjectContext no Entity Framework Designer
Com a versão anterior do Entity Framework um modelo criado com o designer do EF geraria um contexto derivado de ObjectContext e classes de entidade derivadas de EntityObject.

Começando com o EF 4.1, recomendamos alternar para um modelo de geração de código que gera um contexto derivado de classes de entidade DbContext e POCO.

No Visual Studio 2012, você obtém o código DbContext gerado por padrão para todos os novos modelos criados com o designer do EF. Os modelos existentes continuarão a gerar o código baseado em ObjectContext, a menos que você decida alternar para o gerador de código baseado em DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Reverter para a geração de código de ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. desabilitar a geração de código DbContext

A geração das classes DbContext e POCO derivadas é tratada por dois arquivos. tt no projeto, se você expandir o arquivo. edmx no Gerenciador de soluções, verá esses arquivos. Exclua esses dois arquivos do seu projeto.

![Arquivos de geração de código](~/ef6/media/codegenfiles.png)

Se você estiver usando o VB.NET, precisará selecionar o botão **Mostrar todos os arquivos** para ver os arquivos aninhados.

![Mostrar todos os arquivos](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. reabilitar a geração de código de ObjectContext

Abra o modelo no designer do EF, clique com o botão direito do mouse em uma seção em branco da superfície de design e selecione **Propriedades**.

No janela Propriedades altere a **estratégia de geração de código** de **nenhum** para **padrão**.

![Estratégia de geração de código](~/ef6/media/codegenstrategy.png)
