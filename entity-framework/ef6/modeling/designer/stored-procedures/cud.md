---
title: Procedimentos armazenados do designer CUD – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813557"
---
# <a name="designer-cud-stored-procedures"></a>Procedimentos armazenados do designer CUD

Esta explicação passo a passo mostra como mapear as operações Create @ no__t-0insert, Update e Delete (CUD) de um tipo de entidade para procedimentos armazenados usando o Entity Framework Designer (EF designer).  Por padrão, o Entity Framework gera automaticamente as instruções SQL para as operações CUD, mas você também pode mapear procedimentos armazenados para essas operações.  

Observe que Code First não dá suporte ao mapeamento para procedimentos armazenados ou funções. No entanto, você pode chamar procedimentos armazenados ou funções usando o método System. Data. Entity. DbSet. SQLQuery. Por exemplo:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Considerações ao mapear as operações de CUD para procedimentos armazenados

Ao mapear as operações de CUD para procedimentos armazenados, as seguintes considerações se aplicam:

- Se você estiver mapeando uma das operações CUD para um procedimento armazenado, mapeie todas elas. Se você não mapear todos os três, as operações não mapeadas falharão se executadas e uma **updateexception** will ser lançada.
- Você deve mapear todos os parâmetros do procedimento armazenado para as propriedades da entidade.
- Se o servidor gerar o valor de chave primária para a linha inserida, você deverá mapear esse valor de volta para a propriedade de chave da entidade. No exemplo a seguir, o procedimento **InsertPerson** stored retorna a chave primária recém-criada como parte do conjunto de resultados do procedimento armazenado. A chave primária é mapeada para a chave de entidade (**PersonID**) usando as **associações de resultado &lt;Add @ no__t-3** FEATURE do designer do EF.
- As chamadas de procedimento armazenado são mapeadas 1:1 com as entidades no modelo conceitual. Por exemplo, se você implementar uma hierarquia de herança em seu modelo conceitual e, em seguida, mapear os procedimentos armazenados do CUD para as entidades **pai** (base) e **filho** (derivado), salvar as alterações **filho** chamará somente o **filho**' os procedimentos armazenados de s, não dispararão as chamadas de procedimentos armazenados do **pai**.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir esta explicação passo a passo, será necessário:

- Uma versão recente do Visual Studio.
- O [banco de dados de exemplo da escola](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar o projeto

- Abra o Visual Studio 2012.
- Selecione **arquivo-&gt; Projeto New-&gt;**
- No painel esquerdo, clique em **Visual C @ no__t-1**e selecione o modelo de **console** .
- Insira **CUDSProcsSample** As o nome.
- Selecione **OK**.

## <a name="create-a-model"></a>Criar um modelo

- Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e selecione **Adicionar-&gt; novo item**.
- Selecione **dados** no menu à esquerda e, em seguida, selecione **ADO.NET modelo de dados de entidade** no painel modelos.
- Digite **CUDSProcs. edmx** para o nome do arquivo e clique em **Adicionar**.
- Na caixa de diálogo escolher conteúdo do modelo, selecione **gerar do banco de dados**e clique em **Avançar**.
- Clique em **nova conexão**. Na caixa de diálogo Propriedades da conexão, digite o nome do servidor (por exemplo, **(\\LocalDB) mssqllocaldb**), selecione o método de autenticação, digite **School** para o nome do banco de dados e clique em **OK**.
    A caixa de diálogo escolher sua conexão de dados é atualizada com a configuração de conexão de banco de dado.
- Na caixa de diálogo escolher seu objeto de banco de dados, nas **tabelas** node, selecione a tabela **Person** .
- Além disso, selecione os seguintes procedimentos armazenados no nó **procedimentos armazenados e funções** : **DeletePerson**, **InsertPerson**e **UpdatePerson**.
- A partir do Visual Studio 2012, o designer do EF dá suporte à importação em massa de procedimentos armazenados. As **funções e os procedimentos armazenados selecionados no modelo de entidade** são verificados por padrão. Como neste exemplo temos procedimentos armazenados que inserem, atualizam e excluem tipos de entidade, não queremos importá-los e Desmarcarei essa caixa de seleção.

    ![Importar os procs](~/ef6/media/importsprocs.jpg)

- Clique em **concluir**.
    O designer do EF, que fornece uma superfície de design para editar seu modelo, é exibido.

## <a name="map-the-person-entity-to-stored-procedures"></a>Mapear a entidade Person para procedimentos armazenados

- Clique com o botão direito do mouse no tipo **Person** entity e selecione **mapeamento de procedimento armazenado**.
- Os mapeamentos de procedimento armazenado são exibidos nos **detalhes de mapeamento** window.
- Clique em **&lt;Select inserir função @ no__t-2**.
    O campo torna-se uma lista suspensa dos procedimentos armazenados no modelo de armazenamento que podem ser mapeados para tipos de entidade no modelo conceitual.
    Selecione **InsertPerson** from a lista suspensa.
- Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos. Observe que as setas indicam a direção do mapeamento: Os valores de propriedade são fornecidos para parâmetros de procedimento armazenado.
- Clique em **Associação de resultado &lt;Add @ no__t-2**.
- Digite **NewPersonID**, o nome do parâmetro retornado pelo procedimento **InsertPerson** stored. Certifique-se de não digitar espaços à esquerda ou à direita.
- Pressione **Enter**.
- Por padrão, **NewPersonID** is mapeado para a chave de entidade **PersonID**. Observe que uma seta indica a direção do mapeamento: O valor da coluna de resultado é fornecido para a propriedade.

    ![Detalhes do mapeamento](~/ef6/media/mappingdetails.png)

- Clique em **&lt;Select função de atualização @ no__t-2** and selecione **UpdatePerson** from a lista suspensa resultante.
- Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos.
- Clique em **&lt;Select excluir função @ no__t-2** and selecione **DeletePerson** from a lista suspensa resultante.
- Os mapeamentos padrão entre parâmetros de procedimento armazenado e propriedades de entidade são exibidos.

As operações de inserção, atualização e exclusão do tipo **Person** entity agora são mapeadas para procedimentos armazenados.

Se você quiser habilitar a verificação de simultaneidade ao atualizar ou excluir uma entidade com procedimentos armazenados, use uma das seguintes opções:

- Use um parâmetro de **saída** para retornar o número de linhas afetadas do procedimento armazenado e verifique o **parâmetro de linhas afetado** checkbox ao lado do nome do parâmetro. Se o valor retornado for zero quando a operação for chamada, um  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will será lançado.
- Marque a caixa de seleção **usar valor original** ao lado de uma propriedade que você deseja usar para verificação de simultaneidade. Quando uma atualização for tentada, o valor da propriedade que foi originalmente lida do banco de dados será usado ao gravar os dados de volta no banco de dado. Se o valor não corresponder ao valor no banco de dados, um **OptimisticConcurrencyException** will será lançado.

## <a name="use-the-model"></a>Usar o modelo

Abra o arquivo **Program.cs** em que o método **Main** está definido. Adicione o código a seguir à função main.

O código cria um novo objeto **Person** , depois atualiza o objeto e, por fim, exclui o objeto.

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

- Compile e execute o aplicativo. O programa produz a seguinte saída *

> [!NOTE]
> PersonID é gerado automaticamente pelo servidor, portanto, você provavelmente verá um número diferente *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Se você estiver trabalhando com a versão final do Visual Studio, poderá usar o IntelliTrace com o depurador para ver as instruções SQL que são executadas.

![IntelliTrace](~/ef6/media/intellitrace.png)
