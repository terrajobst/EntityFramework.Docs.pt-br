---
title: Selecionando a versão de tempo de execução do Entity Framework para modelos do EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998236"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Selecionando a versão de tempo de execução do Entity Framework para modelos do EF Designer
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.

Começando com o EF6 a tela a seguir foi adicionada ao Designer de EF para que você possa selecionar a versão do tempo de execução que você deseja direcionar ao criar um modelo. A tela será exibida quando a versão mais recente do Entity Framework já não estiver instalada no projeto. Se a versão mais recente já está instalada. ele apenas será usado por padrão.

![Tela](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Direcionamento EF6.x

Você pode escolher o EF6 na tela 'Escolha sua versão' para adicionar o tempo de execução do EF6 ao seu projeto. Depois que você adicionou o EF6, você irá parar vendo essa tela no projeto atual.

EF6 será desabilitada se você já tiver uma versão mais antiga do EF instalado (já que não é possível direcionar várias versões do tempo de execução do mesmo projeto). Se a opção do EF6 não está habilitada aqui, siga estas etapas para atualizar seu projeto para o EF6:

1.  Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar pacotes NuGet...**
2.  Selecione **atualizações**
3.  Selecione **EntityFramework** (Verifique se ele vai atualizá-lo para a versão desejada)
4.  Clique em **atualização**

 

## <a name="targeting-ef5x"></a>Direcionamento EF5.x

Você pode escolher o EF5 na tela 'Escolha sua versão' para adicionar o tempo de execução do EF5 para o seu projeto. Depois que você adicionou o EF5, você ainda verá a tela com a opção de EF6 desabilitada.

Se você tiver uma versão EF4.x do tempo de execução já instalado, em seguida, você verá que a versão do EF listado na tela em vez de EF5. Nessa situação, você pode atualizar para o EF5 usando as seguintes etapas:

1.  Selecione **ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**
2.  Executar **Install-Package EntityFramework – versão 5.0.0**

 

## <a name="targeting-ef4x"></a>Direcionamento EF4.x

Você pode instalar o tempo de execução EF4.x ao seu projeto usando as seguintes etapas:

1.  Selecione **ferramentas -&gt; Gerenciador de pacotes de biblioteca -&gt; Package Manager Console**
2.  Executar **Install-Package EntityFramework – versão 4.3.0**
