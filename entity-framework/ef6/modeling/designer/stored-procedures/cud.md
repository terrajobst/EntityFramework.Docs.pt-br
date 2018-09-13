---
title: Designer CUD procedimentos armazenados – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489551"
---
# <a name="designer-cud-stored-procedures"></a>Procedimentos armazenados de CUD Designer
Este passo a passo mostram como mapear o criar\\inserir, atualizar e excluir operações (CUD) de um tipo de entidade para procedimentos armazenados usando o Entity Framework Designer (Designer de EF).  Por padrão, o Entity Framework gera automaticamente as instruções SQL para as operações de CUD, mas você também pode mapear procedimentos armazenados para essas operações.  

Observe que o Code First não suporta o mapeamento para procedimentos armazenados ou funções. No entanto, você pode chamar procedimentos armazenados ou funções por meio do método System.Data.Entity.DbSet.SqlQuery. Por exemplo:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Considerações ao mapear as operações de CUD de procedimentos armazenados

Ao mapear as operações de CUD para procedimentos armazenados, as seguintes considerações se aplicam: 

-   Se uma das operações CUD estiver mapeando para um procedimento armazenado, mapeie todos eles. Se você não mapear todos os três, as operações não mapeadas falhará se executado e uma **UpdateException** será lançada.
-   Você deve mapear cada parâmetro do procedimento armazenado para as propriedades da entidade.
-   Se o servidor gera o valor de chave primária para a linha inserida, você deve mapear esse valor para a propriedade de chave da entidade. No exemplo a seguir, o **InsertPerson** procedimento armazenado retorna a chave primária recentemente criada como parte do conjunto de resultados do procedimento armazenado. A chave primária é mapeada para a chave de entidade (**PersonID**) usando o **&lt;adicionar associações de resultado&gt;** recurso do EF Designer.
-   As chamadas de procedimento armazenado são mapeados 1:1 com as entidades no modelo conceitual. Por exemplo, se você implementar uma hierarquia de herança no modelo conceitual e mapeie o CUD procedimentos armazenados para o **pai** (base) e o **filho** entidades (derivadas), salvando o **Filho** alterações chamará somente o **filho**de procedimentos armazenados, ele não disparará a **pai**do armazenados chamadas de procedimentos.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

-   Abra o Visual Studio 2012.
-   Selecione **arquivo -&gt; New -&gt; projeto**
-   No painel esquerdo, clique em **Visual C#\#** e, em seguida, selecione o **Console** modelo.
-   Insira **CUDSProcsSample** como o nome.
-   Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

-   O nome do projeto no Gerenciador de soluções e selecione **Add -&gt; Novo Item**.
-   Selecione **dados** no menu esquerdo e selecione **modelo de dados de entidade ADO.NET** no painel de modelos.
-   Insira **CUDSProcs.edmx** para o nome de arquivo e clique **Add**.
-   Na caixa de diálogo Escolher conteúdo do modelo, selecione **gerar a partir do banco de dados**e, em seguida, clique em **próxima**.
-   Clique em **nova Conexão**. Na caixa de diálogo Propriedades de Conexão, insira o nome do servidor (por exemplo, **(localdb)\\mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e, em seguida, Clique em **Okey**.
    A caixa de diálogo Escolher sua Conexão de dados é atualizada com sua configuração de conexão de banco de dados.
-   Na escolha seu objetos de banco de dados caixa de diálogo do **tabelas** nó, selecione o **pessoa** tabela.
-   Além disso, selecione os seguintes procedimentos armazenados sob a **procedimentos armazenados e funções** nó: **DeletePerson**, **InsertPerson**, e **UpdatePerson** . 
-   Começando com o Visual Studio 2012, o EF Designer oferece suporte a importação em massa de procedimentos armazenados. O **importação selecionado procedimentos armazenados e funções no modelo de entidade** é marcada por padrão. Como neste exemplo estamos têm procedimentos armazenados que inserir, atualizarem e excluir tipos de entidade, vamos não quiser importá-los e será desmarque essa caixa de seleção. 

    ![Importar S Procs](~/ef6/media/importsprocs.jpg)

-   Clique em **Finalizar**.
    O Designer de EF, que fornece uma superfície de design para editar seu modelo, é exibido.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapear a entidade de pessoa para procedimentos armazenados

-   Clique com botão direito do **pessoa** tipo de entidade e selecione **mapeamento de procedimento armazenado**.
-   Os mapeamentos de procedimento armazenado são exibidos na **Mapping Details** janela.
-   Clique em  **&lt;selecione Inserir função&gt;**.
    O campo se torna uma lista suspensa dos procedimentos armazenados no modelo de armazenamento que podem ser mapeados para tipos de entidade no modelo conceitual.
    Selecione **InsertPerson** na lista suspensa.
-   Mapeamentos padrão entre os parâmetros de procedimento armazenado e as propriedades da entidade são exibidos. Observe que as setas indicam a direção de mapeamento: valores de propriedade são fornecidos para os parâmetros de procedimento armazenado.
-   Clique em  **&lt;Adicionar associação de resultado&gt;**.
-   Tipo de **NewPersonID**, o nome do parâmetro retornado pelo **InsertPerson** procedimento armazenado. Certifique-se de não digite espaços à esquerda ou à direita.
-   Pressione **ENTER**.
-   Por padrão, **NewPersonID** é mapeado para a chave de entidade **PersonID**. Observe que uma seta indica a direção do mapeamento: O valor da coluna de resultado é fornecido para a propriedade.

    ![Detalhes de mapeamento](~/ef6/media/mappingdetails.png)

-   Clique em **&lt;Selecionar função de atualização&gt;** e selecione **UpdatePerson** na lista suspensa.
-   Mapeamentos padrão entre os parâmetros de procedimento armazenado e as propriedades da entidade são exibidos.
-   Clique em **&lt;Selecionar função de exclusão&gt;** e selecione **DeletePerson** na lista suspensa.
-   Mapeamentos padrão entre os parâmetros de procedimento armazenado e as propriedades da entidade são exibidos.

Inserir, atualizar e excluir operações do **pessoa** tipo de entidade agora são mapeados para procedimentos armazenados.

Se você quiser habilitar a verificação de concorrência ao atualizar ou excluir uma entidade com procedimentos armazenados, use uma das seguintes opções:

-   Use uma **saída** afetados de parâmetro para retornar o número de linhas do procedimento armazenado e verifique se o **linhas afetadas parâmetro** caixa de seleção ao lado do nome do parâmetro. Se o valor retornado é zero quando a operação é chamada, uma [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) será lançada.
-   Verifique as **usar o valor Original** caixa de seleção ao lado de uma propriedade que você deseja usar para verificação de simultaneidade. Quando uma tentativa de atualização, o valor da propriedade que foi originalmente leitura do banco de dados será usado ao gravar dados de volta para o banco de dados. Se o valor não coincide com o valor no banco de dados, uma **OptimisticConcurrencyException** será lançada.

## <a name="use-the-model"></a>Usar o modelo

Abra o **Program.cs** do arquivo onde o **Main** método é definido. Adicione o seguinte código na função principal.

O código cria um novo **pessoa** objeto, em seguida, atualiza o objeto e, por fim, exclui o objeto.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   Compile e execute o aplicativo. O programa produz a saída a seguir *
    >[!NOTE]
> PersonID é gerado automaticamente pelo servidor, portanto, você provavelmente verá um número diferente *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Se você estiver trabalhando com a versão Ultimate do Visual Studio, você pode usar o Intellitrace com o depurador para ver as instruções SQL que são executadas.

![IntelliTrace](~/ef6/media/intellitrace.png)
