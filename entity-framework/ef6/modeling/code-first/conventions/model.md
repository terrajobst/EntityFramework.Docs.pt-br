---
title: Convenções de baseado em modelo - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c1f416ba599445c07bd946714e9b6208b62f5951
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996941"
---
# <a name="model-based-conventions"></a>Convenções de modelo
> [!NOTE]
> **EF6 em diante apenas**: os recursos, as APIs etc. discutidos nessa página foram introduzidos no Entity Framework 6. Se você estiver usando uma versão anterior, algumas ou todas as informações não se aplicarão.  

Convenções de modelo com base são um método avançado de configuração do modelo baseado na convenção. Na maioria dos cenários de [personalizado convenção de API do Code First no DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve ser usado. Uma compreensão da API DbModelBuilder para convenções é recomendada antes de usar as convenções de modelo com base.  

Convenções de modelo com base em permitem a criação de convenções que afetam as tabelas que não são configuráveis por meio de convenções de padrão e propriedades. Exemplos disso são colunas de discriminador de tabela por modelos de hierarquia e as colunas de associação independente.  

## <a name="creating-a-convention"></a>Criar uma convenção   

A primeira etapa na criação de uma convenção de modelo com base é escolher quando o pipeline a convenção de precisa ser aplicado ao modelo. Há dois tipos de convenções de modelo conceitual (C-Space) e Store (S-Space). Uma convenção de C-Space é aplicada ao modelo de que o aplicativo for compilado, enquanto uma convenção de S-Space é aplicada para a versão do modelo que representa o banco de dados e controles de coisas, como colunas como automaticamente gerados são nomeadas.  

Uma convenção de modelo é uma classe que se estende do IConceptualModelConvention ou IStoreModelConvention.  Essas interfaces que ambos aceitar um tipo genérico que pode ser do tipo MetadataItem que é usado para filtrar o tipo de dados que se aplica a convenção a.  

## <a name="adding-a-convention"></a>Adicionando uma convenção   

Convenções de modelo são adicionadas da mesma forma como as classes de convenções regular. No **OnModelCreating** método, adicione a convenção à lista de convenções para um modelo.  

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

Também é possível adicionar uma convenção em relação à outra convenção usando o Conventions.AddBefore\< \> ou Conventions.AddAfter\< \> métodos. Para obter mais informações sobre as convenções que aplica-se do Entity Framework, consulte a seção Observações.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Exemplo: Convenção de modelo do discriminador  

Renomear colunas geradas pelo EF é um exemplo de algo que você não pode fazer com as outras convenções de APIs.  Essa é uma situação em que usar as convenções de modelo é sua única opção.  

Um exemplo de como usar uma convenção de modelo com base em para configurar colunas geradas é Personalizando a maneira como colunas de discriminador são nomeadas.  Abaixo está um exemplo de uma convenção de modelo simples com base em que renomeia todas as colunas no modelo denominada "Discriminadora" para "EntityType".  Isso inclui colunas que o desenvolvedor simplesmente denominada "Discriminadora". Uma vez que a coluna "Discriminadora" é uma coluna gerada, que isso deve ser executado no espaço de S.  

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

## <a name="example-general-ia-renaming-convention"></a>Exemplo: IA geral renomeando convenção   

Outro exemplo mais complicado de convenções de modelo com base em ação é configurar o modo como os que associações independentes (IAs) são nomeadas.  Essa é uma situação em que as convenções de modelo são aplicáveis porque IAs são gerados pelo EF e não estão presentes no modelo que pode acessar a API do DbModelBuilder.  

Quando o EF gera um IA, ele cria uma coluna denominada EntityType_KeyName. Por exemplo, para uma associação chamada Customer com uma coluna de chave chamado CustomerId isso geraria uma coluna denominada Customer_CustomerId. As faixas de convenção a seguir o '\_' caractere fora do nome da coluna que é gerado para o IA.  

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
        for (int i = 0; i \< properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a>Estendendo as convenções de existentes   

Se você precisa escrever uma convenção que é semelhante a uma das convenções que o Entity Framework já se aplica ao seu modelo, que você pode estender sempre que a convenção para evitar a necessidade de reescrevê-lo a partir do zero.  Um exemplo disso é substituir a Id existente correspondente a convenção com uma personalizada.   Um benefício adicional para substituir a convenção de chave é que o método substituído será chamado apenas se não houver nenhuma chave já foi detectado ou configurado explicitamente. Uma lista de convenções que são usados pelo Entity Framework está disponível aqui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Em seguida, precisamos adicionar nossa nova convenção antes da convenção de chave existente. Depois de adicionarmos o CustomKeyDiscoveryConvention, podemos remover o IdKeyDiscoveryConvention.  Se não removemos o IdKeyDiscoveryConvention existente essa convenção ainda terão precedência sobre a convenção de descoberta de Id, pois ele é executado pela primeira vez, mas no caso em que nenhuma propriedade de "chave" for encontrada, a convenção de "id" será executado.  Podemos ver esse comportamento porque cada convenção vê o modelo conforme atualizado pela convenção de anterior (em vez de operar de forma independente e todos os que estão sendo combinados) para que se, por exemplo, uma convenção anterior atualizado para corresponder a algo de um nome de coluna interesse sua convenção personalizada (quando antes que o nome não era de interesse), em seguida, ela será aplicada a essa coluna.  

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

## <a name="notes"></a>Observações  

Uma lista de convenções que são aplicadas no momento pelo Entity Framework está disponível na documentação do MSDN aqui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Essa lista é extraída diretamente do nosso código-fonte.  O código-fonte para o Entity Framework 6 está disponível no [GitHub](https://github.com/aspnet/entityframework6/) e muitas das convenções usadas pelo Entity Framework são bons pontos de partida para o modelo personalizado com base em convenções.  
