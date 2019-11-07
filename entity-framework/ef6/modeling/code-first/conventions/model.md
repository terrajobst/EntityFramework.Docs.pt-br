---
title: Convenções baseadas em modelo-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656162"
---
# <a name="model-based-conventions"></a>Convenções baseadas em modelo
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

As convenções baseadas em modelo são um método avançado de configuração de modelo baseada em convenção. Para a maioria dos cenários, a [API de Convenção de Code First personalizada em DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve ser usada. É recomendável compreender a API DbModelBuilder para convenções antes de usar convenções baseadas em modelo.  

As convenções baseadas em modelo permitem a criação de convenções que afetam as propriedades e as tabelas que não são configuráveis por meio de convenções padrão. Exemplos deles são colunas discriminadoras em modelos de tabela por hierarquia e colunas de associação independentes.  

## <a name="creating-a-convention"></a>Criando uma Convenção   

A primeira etapa na criação de uma convenção baseada em modelo é escolher quando no pipeline a Convenção precisa ser aplicada ao modelo. Há dois tipos de convenções de modelo, conceitual (C-Space) e loja (S-Space). Uma Convenção de espaço C é aplicada ao modelo que o aplicativo cria, enquanto uma Convenção de S-Space é aplicada à versão do modelo que representa o banco de dados e controla itens como, por exemplo, como as colunas geradas automaticamente são nomeadas.  

Uma Convenção de modelo é uma classe que se estende de IConceptualModelConvention ou IStoreModelConvention.  Essas interfaces aceitam um tipo genérico que pode ser do tipo MetadataItem, que é usado para filtrar o tipo de dados ao qual a Convenção se aplica.  

## <a name="adding-a-convention"></a>Adicionando uma Convenção   

As convenções de modelo são adicionadas da mesma maneira que as classes de convenções regulares. No método **OnModelCreating** , adicione a Convenção à lista de convenções para um modelo.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

Uma convenção também pode ser adicionada em relação a outra convenção usando as convenções. addantes\<\> ou convenções. addapós\<métodos \>. Para obter mais informações sobre as convenções que Entity Framework se aplica, consulte a seção observações.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Exemplo: Convenção de modelo discriminador  

Renomear colunas geradas pelo EF é um exemplo de algo que você não pode fazer com as outras APIs de convenções.  Essa é uma situação em que o uso de convenções de modelo é sua única opção.  

Um exemplo de como usar uma convenção baseada em modelo para configurar colunas geradas é personalizar a maneira como as colunas discriminadoras são nomeadas.  Abaixo está um exemplo de uma convenção simples baseada em modelo que renomeia cada coluna no modelo chamado "discriminador" como "EntityType".  Isso inclui colunas que o desenvolvedor simplesmente chama "discriminador". Como a coluna "discriminador" é uma coluna gerada, isso precisa ser executado em S-Space.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Exemplo: Convenção de renomeação de IA geral   

Outro exemplo mais complicado de convenções baseadas em modelo em ação é configurar a maneira como as associações independentes (IAs) são nomeadas.  Essa é uma situação em que as convenções de modelo são aplicáveis porque o IAs é gerado pelo EF e não estão presentes no modelo que a API DbModelBuilder pode acessar.  

Quando o EF gera um IA, ele cria uma coluna chamada EntityType_KeyName. Por exemplo, para uma associação chamada Customer com uma coluna de chave chamada CustomerId, ela geraria uma coluna chamada Customer_CustomerId. A Convenção a seguir remove o caractere '\_' do nome da coluna que é gerado para IA.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i < properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Estendendo convenções existentes   

Se você precisar escrever uma convenção semelhante a uma das convenções que Entity Framework já se aplica ao modelo, você sempre poderá estender essa Convenção para evitar ter que reescrevê-la do zero.  Um exemplo disso é substituir a Convenção de correspondência de ID existente por uma personalizada.   Um benefício adicional para substituir a Convenção de chave é que o método substituído será chamado somente se não houver nenhuma chave já detectada ou configurada explicitamente. Uma lista de convenções que são usadas pelo Entity Framework está disponível aqui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

Em seguida, precisamos adicionar nossa nova Convenção antes da Convenção de chave existente. Depois de adicionarmos o CustomKeyDiscoveryConvention, podemos remover o IdKeyDiscoveryConvention.  Se não removermos o IdKeyDiscoveryConvention existente, essa Convenção ainda terá precedência sobre a Convenção de descoberta de ID, pois ela é executada primeiro, mas, no caso em que nenhuma propriedade "Key" for encontrada, a Convenção "ID" será executada.  Vemos esse comportamento porque cada convenção vê o modelo como atualizado pela Convenção anterior (em vez de operar nele de forma independente e tudo sendo combinado) para que, por exemplo, uma Convenção anterior tenha atualizado um nome de coluna para corresponder a algo de de interesse para sua Convenção personalizada (quando antes de o nome não ser interessante), ela será aplicada a essa coluna.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Anotações  

Uma lista de convenções atualmente aplicadas pelo Entity Framework está disponível na documentação do MSDN aqui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Essa lista é extraída diretamente do nosso código-fonte.  O código-fonte para o Entity Framework 6 está disponível no [GitHub](https://github.com/aspnet/entityframework6/) e muitas das convenções usadas pelo Entity Framework são bons pontos de partida para as convenções personalizadas baseadas em modelos.  
