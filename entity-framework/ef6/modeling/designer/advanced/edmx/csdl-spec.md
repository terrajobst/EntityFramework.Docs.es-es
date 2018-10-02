---
title: Especificación de CSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 438af83b8a1ad51ee8414341181412e950d0e117
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460155"
---
# <a name="csdl-specification"></a>Especificación CSDL
El lenguaje de definición de esquemas conceptuales (CSDL) es un lenguaje basado en XML que describe las entidades, las relaciones y las funciones que conforman un modelo conceptual de una aplicación controlada por datos. Este modelo conceptual puede utilizarse por Entity Framework o WCF Data Services. Los metadatos descritos mediante CSDL se usan Entity Framework para asignar entidades y relaciones que se definen en un modelo conceptual a un origen de datos. Para obtener más información, consulte [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) y [especificación de MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL es la implementación de Entity Framework del Entity Data Model.

En una aplicación de Entity Framework, los metadatos del modelo conceptual se carga desde un archivo .csdl (escrito en CSDL) en una instancia de la System.Data.Metadata.Edm.EdmItemCollection y es accesible mediante el uso de métodos en el Clase System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework usa los metadatos del modelo conceptual para traducir las consultas con el modelo conceptual en comandos específicos del origen de datos.

EF Designer almacena información sobre el modelo conceptual en un archivo .edmx en tiempo de diseño. En tiempo de compilación, EF Designer usa información en un archivo .edmx para crear el archivo .csdl que Entity Framework necesita en tiempo de ejecución.

Las versiones de CSDL se diferencian por los espacios de nombres XML.

| Versión CSDL | Namespace XML                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association (Elemento) (CSDL)

Un **asociación** elemento define una relación entre dos tipos de entidad. Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad. La multiplicidad de un extremo de asociación puede tener un valor uno (1), cero o uno (0.. 1), o muchos (\*). Esta información se especifica en los dos elementos de End secundarios.

Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas, si estas se exponen en un tipo de entidad.

En una aplicación, una instancia de una asociación representa una asociación concreta entre las instancias de tipos de entidad. Las instancias de asociación se agrupan de manera lógica en un conjunto de asociaciones.

Un **asociación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   End (exactamente 2 elementos)
-   ReferentialConstraint (cero o un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **asociación** elemento.

| Nombre de atributo | Es necesario | Valor                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | Sí         | Nombre de la asociación. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación cuando las claves externas no se han expuesto en el **cliente** y  **Orden** tipos de entidad. El **multiplicidad** valores para cada **final** de la asociación indican que muchas **pedidos** se puede asociar un **cliente**, pero solo un **cliente** se puede asociar un **orden**. Además, el **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y se han cargado en ObjectContext se eliminarán. Si el **cliente** se elimina.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación cuando las claves externas se han expuesto en el **cliente** y  **Orden** tipos de entidad. Con claves externas expuestas, la relación entre las entidades se administra con un **ReferentialConstraint** elemento. Un elemento AssociationSetMapping correspondiente no es necesario asignar esta asociación al origen de datos.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a>AssociationSet (Elemento) (CSDL)

El **AssociationSet** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para las instancias de asociación del mismo tipo. Un conjunto de asociaciones proporciona una definición para agrupar las instancias de la asociación con objeto de que se puedan asignar a un origen de datos.  

El **AssociationSet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elementos)
-   End (exactamente dos elementos necesarios)
-   Elementos annotation (cero o más elementos)

El **asociación** atributo especifica el tipo de asociación que contiene un conjunto de asociaciones. Los conjuntos de entidades que constituyen los extremos de un conjunto de asociaciones se especifican con exactamente dos secundarios **final** elementos.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSet** elemento.

| Nombre de atributo  | Es necesario | Valor                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | Sí         | Nombre del conjunto de entidades. El valor de la **nombre** atributo no puede ser el mismo que el valor de la **asociación** atributo.                                 |
| **Asociación** | Sí         | Nombre completo de la asociación cuyas instancias contiene el conjunto de asociaciones. La asociación debe estar en el mismo espacio de nombres que el conjunto de asociaciones. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **AssociationSet** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento con dos **AssociationSet** elementos:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType (Elemento de CSDL)

El **CollectionType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) especifica que un parámetro de función o una función devuelve el tipo es una colección. El **CollectionType** elemento puede ser un elemento secundario del elemento de parámetro o el elemento ReturnType (función). Se puede especificar el tipo de colección mediante el uso del **tipo** atributo o uno de los elementos secundarios siguientes:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Un modelo no se validará si se especifica el tipo de una colección con ambos el **tipo** atributo y un elemento secundario.

 

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **CollectionType** elemento. Tenga en cuenta que el **DefaultValue**, **MaxLength**, **FixedLength**, **precisión**, **escala**,  **Unicode**, y **intercalación** atributos solo son aplicables a colecciones de **EDMSimpleTypes**.

| Nombre de atributo                                                          | Es necesario | Valor                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | No          | Tipo de la colección.                                                                                                                                                                                                      |
| **Que acepta valores null**                                                            | No          | **True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null. <br/> [!NOTE]                                                                                                                 |
| > En el CSDL v1, debe tener una propiedad de tipo complejo `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | No          | Valor predeterminado de la propiedad.                                                                                                                                                                                               |
| **MaxLength**                                                           | No          | Longitud máxima del valor de propiedad.                                                                                                                                                                                        |
| **FixedLength**                                                         | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.                                                                                                                           |
| **Precisión**                                                           | No          | Precisión del valor de propiedad.                                                                                                                                                                                             |
| **Escalar**                                                               | No          | Escala del valor de propiedad.                                                                                                                                                                                                 |
| **SRID**                                                                | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales.   Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.                                                                                                                                |
| **intercalación**                                                           | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                    |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra una función definida por modelo que utiliza un **CollectionType** elemento para especificar que la función devuelve una colección de **persona** tipos de entidad (como se especifica en el **ElementType** atributo).

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

El ejemplo siguiente muestra una función definida por modelo que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

El ejemplo siguiente muestra una función definida por modelo que usa el **CollectionType** elemento para especificar que la función acepta como parámetro una colección de **departamento** tipos de entidad.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a>ComplexType (Elemento) (CSDL)

Un **ComplexType** elemento define una estructura de datos que se compone de **EdmSimpleType** propiedades u otros tipos complejos.  Un tipo complejo puede ser una propiedad de un tipo de entidad o de otro tipo complejo. Un tipo complejo se parece a un tipo de entidad en que también define datos. Sin embargo, existen algunas diferencias clave entre los tipos complejos y los tipos de entidad:

-   Los tipos complejos no tienen identidades (o claves) y, por consiguiente, no pueden existir de forma independiente. Los tipos complejos solo pueden existir como propiedades de tipos de entidad u otros tipos complejos.
-   Tipos complejos no pueden participar en las asociaciones. Ni el final de una asociación puede ser un tipo complejo y, por tanto, no se puede definir las propiedades de navegación para los tipos complejos.
-   Una propiedad de tipo complejo no puede tener un valor nulo, aunque las propiedades escalares de un tipo complejo se pueden establecer cada una con el valor nulo.

Un **ComplexType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Propiedad (cero o más elementos)
-   Elementos annotation (cero o más elementos)

En la tabla siguiente se describe los atributos que se pueden aplicar a la **ComplexType** elemento.

| Nombre de atributo                                                                                                 | Es necesario | Valor                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nombre                                                                                                           | Sí         | El nombre del tipo complejo. El nombre de un tipo complejo no puede ser igual que el nombre de otro tipo complejo, tipo de entidad o asociación que esté dentro del ámbito del modelo. |
| BaseType                                                                                                       | No          | El nombre de otro tipo complejo que es el tipo base del tipo complejo que se define. <br/> [!NOTE]                                                                     |
| > Este atributo no es aplicable en CSDL v1. La herencia de los tipos complejos no se admite en esa versión. |             |                                                                                                                                                                                     |
| Abstracto                                                                                                       | No          | **True** o **False** (el valor predeterminado) dependiendo de si el tipo complejo es un tipo abstracto. <br/> [!NOTE]                                                                  |
| > Este atributo no es aplicable en CSDL v1. Los tipos complejos de esa versión no pueden ser tipos abstractos.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ComplexType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra un tipo complejo, **dirección**, con el **EdmSimpleType** propiedades **StreetAddress**, **Ciudad**,  **StateOrProvince**, **país**, y **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Para definir el tipo complejo **dirección** (anterior) como una propiedad de un tipo de entidad, debe declarar el tipo de propiedad en la definición de tipo de entidad. El ejemplo siguiente se muestra el **dirección** propiedad como un tipo complejo en un tipo de entidad (**Publisher**):

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression (Elemento) (CSDL)

El **DefiningExpression** elemento de lenguaje de definición de esquemas conceptuales (CSDL) contiene una expresión de Entity SQL que define una función en el modelo conceptual.  

> [!NOTE]
> Con fines de validación, un **DefiningExpression** elemento puede incluir contenido arbitrario. Sin embargo, Entity Framework generará una excepción en tiempo de ejecución si un **DefiningExpression** elemento no contiene SQL válido de entidad.

 

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **DefiningExpression** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se usa un **DefiningExpression** elemento para definir una función que devuelve el número de años transcurridos desde que se publicó un libro. El contenido de la **DefiningExpression** se escribe el elemento de Entity SQL.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent (Elemento) (CSDL)

El **dependientes** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario al elemento ReferentialConstraint y define el extremo dependiente de una restricción referencial. Un **ReferentialConstraint** elemento define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional. Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad. El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción. El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción. **PropertyRef** elementos se usan para especificar qué claves hacen referencia el extremo principal.

El **dependientes** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   PropertyRef (uno o más elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **dependientes** elemento.

| Nombre de atributo | Es necesario | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Sí         | Nombre del tipo de entidad del extremo dependiente de la asociación. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **dependientes** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que se va a utilizar como parte de la definición de la **PublishedBy** asociación. El **PublisherId** propiedad de la **libro** tipo de entidad constituye el extremo dependiente de la restricción referencial.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a>Documentation (Elemento) (CSDL)

El **documentación** elemento de lenguaje de definición de esquemas conceptuales (CSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario. En un archivo .edmx, cuando el **documentación** es un elemento secundario de un elemento que aparece como un objeto en la superficie de diseño del Diseñador de EF (por ejemplo, una entidad, asociación o propiedad), el contenido de la **documentación**  elemento aparecerá en Visual Studio **propiedades** ventana para el objeto.

El **documentación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   **Resumen**: una breve descripción del elemento primario. (cero o un elemento).
-   **LongDescription**: una descripción extensa del elemento primario. (cero o un elemento).
-   Elementos de anotación. (cero o más elementos).

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **documentación** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **documentación** como un elemento secundario de un elemento EntityType. Si el fragmento de código siguiente se encontraban en el CSDL contenido de un edmx archivo, el contenido de la **resumen** y **LongDescription** elementos aparecería en Visual Studio **propiedades** ventana al hacer clic en el `Customer` tipo de entidad.

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a>End (Elemento) (CSDL)

El **final** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento Association o el elemento AssociationSet. En cada caso, el rol de la **final** elemento es diferente y los atributos aplicables son diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento End como elemento secundario del elemento Association

Un **final** elemento (como un elemento secundario de la **asociación** elemento) identifica el tipo de entidad en un extremo de una asociación y el número de instancias del tipo de entidad que pueden existir en dicho extremo de una asociación. Los extremos de asociación se definen como parte de una asociación, y esta debe tener exactamente dos extremos. Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas si estas se exponen en un tipo de entidad.  

Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   OnDelete (cero o un elemento)
-   Elementos annotation (cero o más elementos)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **asociación** elemento.

| Nombre de atributo   | Es necesario | Valor                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Sí         | Nombre del tipo de entidad de un extremo de la asociación.                                                                                                                                                                                                                                                                                                                                                         |
| **Rol**         | No          | Nombre para el extremo de la asociación. Si no se proporciona ningún nombre, se usará el nombre del tipo de entidad del extremo de la asociación.                                                                                                                                                                                                                                                                                           |
| **Multiplicidad** | Sí         | **1**, **de 0.. 1**, o **\*** dependiendo del número de instancias del tipo de entidad que puede estar al final de la asociación. <br/> **1** indica esa instancia de tipo exactamente una entidad existe en el extremo de asociación. <br/> **de 0.. 1** indica que pueden existir cero o una instancia de tipo de entidad en el extremo de asociación. <br/> **\*** indica que el cero, uno o más instancias del tipo de entidad existen en el extremo de asociación. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación. El **multiplicidad** valores para cada **final** de la asociación indican que muchas **pedidos** se puede asociar un **cliente**, pero solo un **cliente** se puede asociar un **orden**. Además, el **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y que se han cargado en el ObjectContext será if se eliminó el **cliente** se elimina.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento End como elemento secundario del elemento AssociationSet

El **final** elemento especifica un extremo de un conjunto de asociaciones. El **AssociationSet** elemento debe contener dos **final** elementos. La información contenida en un **final** elemento se utiliza para asignar un conjunto de asociaciones a un origen de datos.

Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Elementos annotation (cero o más elementos)

> [!NOTE]
> Los elementos de anotación deben aparecer después de todos los demás elementos secundarios. Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.

 

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **AssociationSet** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sí         | El nombre de la **EntitySet** elemento que define un extremo del elemento primario **AssociationSet** elemento. El **EntitySet** elemento debe definirse en el mismo contenedor de entidades como elemento primario **AssociationSet** elemento. |
| **Rol**       | No          | Nombre del extremo del conjunto de asociaciones. Si el **rol** no se usa el atributo, el nombre del conjunto del extremo de asociación será el nombre del conjunto de entidades.                                                                   |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento con dos **AssociationSet** elementos, cada uno con dos **final** elementos:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer (Elemento) (CSDL)

El **EntityContainer** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para los conjuntos de entidades, conjuntos de asociaciones y las importaciones de función. Un contenedor de entidades del modelo conceptual se asigna a un contenedor de entidades del modelo de almacenamiento a través del elemento EntityContainerMapping. Un contenedor de entidades de modelo de almacenamiento describe la estructura de la base de datos: los conjuntos de entidades describen las tablas, los conjuntos de asociaciones describen las restricciones de clave externa y las importaciones de función describen los procedimientos almacenados en una base de datos.

Un **EntityContainer** elemento puede tener cero o un elemento de la documentación. Si un **documentación** elemento está presente, debe preceder a todos los **EntitySet**, **AssociationSet**, y **FunctionImport** elementos.

Un **EntityContainer** elemento puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Elementos Annotation

Puede ampliar un **EntityContainer** elemento para incluir el contenido de otro **EntityContainer** que está dentro del mismo espacio de nombres. Para incluir el contenido de otro **EntityContainer**, en la que hacen referencia a **EntityContainer** elemento, establezca el valor de la **extiende** atributo en el nombre de la  **EntityContainer** elemento que se va a incluir. Todos los elementos secundarios de los incluidos **EntityContainer** elemento se tratará como elementos secundarios de la referencia **EntityContainer** elemento.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **Using** elemento.

| Nombre de atributo | Es necesario | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Sí         | Nombre del contenedor de entidades.                               |
| **Extiende**    | No          | Nombre de otro contenedor de entidades dentro del mismo espacio de nombres. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityContainer** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento que define tres conjuntos de entidades y dos conjuntos de asociaciones.

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a>EntitySet (Elemento) (CSDL)

El **EntitySet** elemento de lenguaje de definición de esquemas conceptuales es un contenedor lógico para instancias de un tipo de entidad y las instancias de cualquier tipo que se deriva de ese tipo de entidad. La relación entre un tipo de entidad y un conjunto de entidades es análoga a la relación entre una fila y una tabla en una base de datos relacional. Igual que una fila, un tipo de entidad define un conjunto de datos relacionados y, lo mismo que una tabla, un conjunto de entidades contiene instancias de esa definición. Un conjunto de entidades proporciona una construcción para agrupar las instancias del tipo de entidad para que se pueden asignar a las estructuras de datos relacionadas en un origen de datos.  

Se pueden definir varios conjuntos de entidades para un tipo de entidad determinado.

> [!NOTE]
> EF Designer no admite los modelos conceptuales que contienen varios conjuntos de entidades por tipo.

 

El **EntitySet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Elemento Documentation (cero o un elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySet** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del conjunto de entidades.                                                              |
| **EntityType** | Sí         | El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntitySet** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento con tres **EntitySet** elementos:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

Es posible definir varios conjuntos de entidades por tipo (MEST). En el ejemplo siguiente se define un contenedor de entidades con dos conjuntos de entidades para el **libro** tipo de entidad:

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a>EntityType (Elemento) (CSDL)

El **EntityType** elemento representa la estructura de un concepto de nivel superior, como un cliente o un pedido, en un modelo conceptual. Un tipo de entidad es una plantilla para las instancias de los tipos de entidad de una aplicación. Cada plantilla contiene la información siguiente:

-   Un nombre único. (Requerido)
-   Una clave de entidad definida por una o varias propiedades. (Requerido)
-   Propiedades para el almacenamiento de datos. (Opcional)
-   Propiedades de navegación que permiten desplazarse de un extremo de la asociación al otro. (Opcional)

En una aplicación, una instancia de un tipo de entidad representa un objeto específico (como un cliente o un pedido concreto). Cada una de las instancias de un tipo de entidad debe tener una clave de entidad única dentro de un conjunto de entidades.

Dos instancias de tipo de entidad se consideran iguales solo si son del mismo tipo y los valores de sus claves de entidad son idénticos.

Un **EntityType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Clave (cero o un elemento)
-   Propiedad (cero o más elementos)
-   NavigationProperty (cero o más elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityType** elemento.

| Nombre de atributo                                                                                                                                  | Es necesario | Valor                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | Sí         | Nombre del tipo de entidad.                                                                     |
| **BaseType**                                                                                                                                    | No          | Nombre de otro tipo de entidad que es el tipo base del tipo de entidad que se define.  |
| **Abstracto**                                                                                                                                    | No          | **True** o **False**, dependiendo de si el tipo de entidad es un tipo abstracto.                 |
| **OpenType**                                                                                                                                    | No          | **True** o **False** dependiendo de si el tipo de entidad es un tipo de entidad abierto. <br/> [!NOTE] |
| > El **OpenType** el atributo solo es aplicable a los tipos de entidad que se definen en los modelos conceptuales que se usan con ADO.NET Data Services. |             |                                                                                                  |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con tres **propiedad** elementos y dos **NavigationProperty** elementos:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a>EnumType (elemento) (CSDL)

El **EnumType** elemento representa un tipo enumerado.

Un **EnumType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Miembro (cero o más elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EnumType** elemento.

| Nombre de atributo     | Es necesario | Valor                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | Sí         | Nombre del tipo de entidad.                                                                                                                                                                  |
| **IsFlags**        | No          | **True** o **False**, dependiendo de si se puede usar el tipo de enumeración como un conjunto de marcas. El valor predeterminado es **False.**.                                                                     |
| **UnderlyingType** | No          | **Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** o **Edm.SByte** definir el intervalo de valores del tipo.   Es el tipo subyacente predeterminado de elementos de la enumeración **Edm.Int32.**. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EnumType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EnumType** elemento con tres **miembro** elementos:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function (Elemento) (CSDL)

El **función** elemento de lenguaje de definición de esquemas conceptuales (CSDL) se usa para definir o declarar las funciones en el modelo conceptual. Una función se define mediante el uso de un elemento DefiningExpression.  

Un **función** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Parámetro (cero o más elementos)
-   DefiningExpression (elemento cero o uno)
-   ReturnType (función) (cero o un elemento)
-   Elementos annotation (cero o más elementos)

Un devuelto de tipo para una función debe especificarse con el **ReturnType** elemento (función) o el **ReturnType** atributo (ver abajo), pero no ambos. Los tipos de valores devueltos posibles son EdmSimpleType, tipo de entidad, tipo complejo, tipo de fila o tipo ref (o una colección de uno de estos tipos).

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **función** elemento.

| Nombre de atributo | Es necesario | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | Sí         | Nombre de la función.          |
| **ReturnType** | No          | El tipo devuelto por la función. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **función** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se usa un **función** elemento para definir una función que devuelve el número de años transcurridos desde que se contrató a un instructor.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport (Elemento) (CSDL)

El **FunctionImport** elemento de lenguaje de definición de esquemas conceptuales (CSDL) representa una función que está definido en el origen de datos pero está disponible para los objetos a través del modelo conceptual. Por ejemplo, un elemento de la función del modelo de almacenamiento puede usarse para representar un procedimiento almacenado en una base de datos. Un **FunctionImport** elemento en el modelo conceptual representa la función correspondiente en una aplicación de Entity Framework y se asigna a la función de modelo de almacenamiento mediante el elemento FunctionImportMapping. Cuando se llama a la función en la aplicación, el procedimiento almacenado correspondiente se ejecuta en la base de datos.

El **FunctionImport** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elementos)
-   Parámetro (cero o más elementos)
-   Elementos annotation (cero o más elementos)
-   ReturnType (FunctionImport) (cero o más elementos)

Una **parámetro** elemento debe definirse para cada parámetro que acepta la función.

Un devuelto de tipo para una función debe especificarse con el **ReturnType** (FunctionImport) (elemento) o el **ReturnType** atributo (ver abajo), pero no ambos. El valor de tipo de valor devuelto debe ser una colección de ComplexType, EntityType o EdmSimpleType.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **FunctionImport** elemento.

| Nombre de atributo   | Es necesario | Valor                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sí         | Nombre de la función importada.                                                                                                                                                                    |
| **ReturnType**   | No          | Tipo devuelto por la función. No use este atributo si la función no devuelve un valor. En caso contrario, el valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.        |
| **EntitySet**    | No          | Si la función devuelve una colección de entidades tipos, el valor de la **EntitySet** deben ser el conjunto de entidades al que pertenece la colección. En caso contrario, el **EntitySet** no debe usarse el atributo. |
| **IsComposable** | No          | Si el valor se establece en true, la función es que se pueden componer (Table-valued Function) y se puede usar en una consulta LINQ.  El valor predeterminado es **false**.                                                           |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **FunctionImport** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **FunctionImport** elemento que acepta un parámetro y devuelve una colección de tipos de entidad:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key (Elemento) (CSDL)

El **clave** elemento es un elemento secundario del elemento EntityType y define un *clave de entidad* (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad). Las propiedades que constituyen una entidad se eligen en tiempo de diseño. Los valores de las propiedades de clave de entidad deben identificar de forma inequívoca en tiempo de ejecución una instancia de tipo de entidad dentro de un conjunto de entidades. Las propiedades que constituyen una clave de entidad se deben elegir de tal forma que garanticen la unicidad de las instancias de un conjunto de entidades. El **clave** elemento define una clave de entidad haciendo referencia a una o varias de las propiedades de un tipo de entidad.

El **clave** elemento puede tener los elementos secundarios siguientes:

-   PropertyRef (uno o más elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **clave** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente define un tipo de entidad denominado **libro**. La clave de entidad se define haciendo referencia a la **ISBN** propiedad del tipo de entidad.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

El **ISBN** propiedad es una buena elección para la clave de entidad porque una biblioteca International Standard Book Number (ISBN) identifica de forma única un libro.

El ejemplo siguiente muestra un tipo de entidad (**autor**) que tiene una clave de entidad que consta de dos propiedades, **nombre** y **dirección**.

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

Uso de **nombre** y **dirección** para la entidad de clave es una opción razonable, ya que dos autores con el mismo nombre son vivan en la misma dirección. Sin embargo, esta opción no garantiza por completo la existencia de claves de entidad únicas en un conjunto de entidades. Agregar una propiedad, como **AuthorId**, que podría utilizarse para identificar de forma única un autor sería recomendable en este caso.

 

## <a name="member-element-csdl"></a>Elemento Member (CSDL)

El **miembro** elemento es un elemento secundario del elemento EnumType y define un miembro del tipo enumerado.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **FunctionImport** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del miembro.                                                                                                                                                                  |
| **Valor**      | No          | Valor del miembro. De forma predeterminada, el primer miembro tiene el valor 0 y el valor de cada enumerador sucesivo se incrementa en 1. Pueden existir varios miembros que tienen los mismos valores. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **FunctionImport** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EnumType** elemento con tres **miembro** elementos:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty (Elemento) (CSDL)

Un **NavigationProperty** elemento define una propiedad de navegación, que proporciona una referencia al otro extremo de una asociación. A diferencia de las propiedades definidas por el elemento de propiedad, las propiedades de navegación no definen la forma y características de datos. Proporcionan una manera de desplazarse por una asociación entre dos tipos de entidad.

Las propiedades de navegación son opcionales en los dos tipos de entidad de los extremos de una asociación. Si define una propiedad de navegación en un tipo de entidad del extremo de una asociación, no tiene que definir una propiedad de navegación en el tipo de entidad del otro extremo de la asociación.

El tipo de datos devuelto por una propiedad de navegación viene determinado por la multiplicidad de su extremo remoto de la asociación. Por ejemplo, supongamos que una propiedad de navegación, **OrdersNavProp**, existe en un **cliente** tipo de entidad y se desplaza una asociación uno a varios entre **cliente** y  **Orden**. Dado que el extremo remoto de la asociación para la propiedad de navegación tiene la multiplicidad muchas (\*), su tipo de datos es una colección (de **orden**). De forma similar, si una propiedad de navegación, **CustomerNavProp**, existe en el **orden** tipo de entidad, su tipo de datos sería **cliente** puesto que es la multiplicidad del extremo remoto uno (1).

Un **NavigationProperty** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **NavigationProperty** elemento.

| Nombre de atributo   | Es necesario | Valor                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sí         | Nombre de la propiedad de navegación.                                                                                                                                                                                                             |
| **Relación** | Sí         | Nombre de una asociación que se encuentra dentro del ámbito del modelo.                                                                                                                                                                                |
| **ToRole**       | Sí         | Extremo de la asociación en el que finaliza la navegación. El valor de la **ToRole** atributo debe ser el mismo que el valor de uno de los **rol** atributos definidos en uno de los extremos de asociación (definidos en el elemento AssociationEnd).       |
| **FromRole**     | Sí         | Extremo de la asociación desde el que comienza la navegación. El valor de la **FromRole** atributo debe ser el mismo que el valor de uno de los **rol** atributos definidos en uno de los extremos de asociación (definidos en el elemento AssociationEnd). |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **NavigationProperty** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se define un tipo de entidad (**libro**) con dos propiedades de navegación (**PublishedBy** y **WrittenBy**):

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a>OnDelete (Elemento) (CSDL)

El **OnDelete** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define el comportamiento que está conectado con una asociación. Si el **acción** atributo está establecido en **Cascade** en un extremo de una asociación, relacionados con los tipos de entidad en el otro extremo de la asociación se eliminan cuando se elimina el tipo de entidad del primer extremo. Si la asociación entre dos tipos de entidad es una relación de clave principal de clave principal, se elimina un objeto dependiente cargado cuando se elimina el objeto principal en el otro extremo de la asociación con independencia de la **OnDelete** especificación.  

> [!NOTE]
> El **OnDelete** elemento sólo afecta al comportamiento en tiempo de ejecución de una aplicación; no afecta al comportamiento del origen de datos. El comportamiento definido en el origen de datos debe ser igual que el definido en la aplicación.

 

Un **OnDelete** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **OnDelete** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Acción**     | Sí         | **CASCADE** o **ninguno**. Si **Cascade**, tipos de entidad dependiente se eliminarán cuando se elimina el tipo de entidad de seguridad. Si **ninguno**, tipos de entidad dependiente no se eliminará cuando se elimina el tipo de entidad de seguridad. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación. El **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y se han cargado en la clase ObjectContext se eliminará cuando el  **Cliente** se elimina.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter (Elemento) (CSDL)

El **parámetro** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento FunctionImport o el elemento Function.

### <a name="functionimport-element-application"></a>Aplicación para el elemento FunctionImport

Un **parámetro** elemento (como un elemento secundario de la **FunctionImport** elemento) se utiliza para definir los parámetros de entrada y salidos para las importaciones de función que se declaran en CSDL.

El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elementos)
-   Elementos annotation (cero o más elementos)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del parámetro.                                                                                                                                                                                                      |
| **Type**       | Sí         | Tipo del parámetro. El valor debe ser un **EDMSimpleType** o un tipo complejo que esté dentro del ámbito del modelo.                                                                                                             |
| **Modo**       | No          | **En**, **Out**, o **InOut** dependiendo de si el parámetro es una entrada, salida o parámetro de entrada/salida.                                                                                                                |
| **MaxLength**  | No          | Longitud máxima permitida del parámetro.                                                                                                                                                                                    |
| **Precisión**  | No          | Precisión del parámetro.                                                                                                                                                                                                 |
| **Escalar**      | No          | Escala del parámetro.                                                                                                                                                                                                     |
| **SRID**       | No          | Identificador de referencia espacial de sistema. Válido únicamente para los parámetros de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **FunctionImport** elemento con una **parámetro** elemento secundario. La función acepta un parámetro de entrada y devuelve una colección de tipos de entidad.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Aplicación para el elemento Function

Un **parámetro** elemento (como un elemento secundario de la **función** elemento) define parámetros para las funciones que se definen o se declara en un modelo conceptual.

El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   CollectionType (cero o un elemento)
-   ReferenceType (cero o un elemento)
-   RowType (cero o un elemento)

> [!NOTE]
> Solo uno de los **CollectionType**, **ReferenceType**, o **RowType** elementos pueden ser un elemento secundario de un **propiedad** elemento.

 

-   Elementos annotation (cero o más elementos)

> [!NOTE]
> Los elementos de anotación deben aparecer después de todos los demás elementos secundarios. Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.

 

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.

| Nombre de atributo   | Es necesario | Valor                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Sí         | Nombre del parámetro.                                                                                                                                                                                                      |
| **Type**         | No          | Tipo del parámetro. Un parámetro puede ser de cualquiera de los siguientes tipos (o colecciones de estos tipos): <br/> **EdmSimpleType** <br/> tipo de entidad <br/> tipo complejo <br/> tipo de fila <br/> tipo de referencia                             |
| **Que acepta valores null**     | No          | **True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un **null** valor.                                                                                                                          |
| **DefaultValue** | No          | Valor predeterminado de la propiedad.                                                                                                                                                                                              |
| **MaxLength**    | No          | Longitud máxima del valor de propiedad.                                                                                                                                                                                       |
| **FixedLength**  | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.                                                                                                                          |
| **Precisión**    | No          | Precisión del valor de propiedad.                                                                                                                                                                                            |
| **Escalar**        | No          | Escala del valor de propiedad.                                                                                                                                                                                                |
| **SRID**         | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.                                                                                                                               |
| **intercalación**    | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                   |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **función** elemento que utiliza uno **parámetro** elemento secundario para definir un parámetro de función.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal (Elemento) (CSDL)

El **Principal** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo principal de una restricción referencial. Un **ReferentialConstraint** elemento define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional. Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad. El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción. El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción. **PropertyRef** elementos se usan para especificar qué claves hace referencia el extremo dependiente.

El **Principal** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   PropertyRef (uno o más elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **Principal** elemento.

| Nombre de atributo | Es necesario | Valor                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Sí         | Nombre del tipo de entidad del extremo principal de la asociación. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Principal** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que forma parte de la definición de la **PublishedBy** asociación. El **Id** propiedad de la **Publisher** tipo de entidad constituye el extremo principal de la restricción referencial.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a>Property (Elemento) (CSDL)

El **propiedad** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento EntityType, ComplexType (elemento) o el elemento RowType.

### <a name="entitytype-and-complextype-element-applications"></a>Aplicaciones de elemento EntityType y ComplexType

**Propiedad** elementos (como elementos secundarios de **EntityType** o **ComplexType** elementos) definen la forma y características de los datos que contendrá una instancia de tipo de entidad o la instancia de tipo complejo . Las propiedades en un modelo conceptual son análogas a las propiedades que se definen en una clase. Del mismo modo que las propiedades en una clase definen la forma de la clase y proporcionan información sobre los objetos, las propiedades en un modelo conceptual definen la forma de un tipo de entidad y proporcionan información sobre las instancias del tipo de entidad.

El **propiedad** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Elemento Documentation (cero o un elementos)
-   Elementos annotation (cero o más elementos)

Las siguientes facetas se pueden aplicar a un **propiedad** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisión**, **escala**, **Unicode**, **intercalación**,  **ConcurrencyMode**. Las facetas son atributos XML que proporcionan información sobre cómo los valores de propiedad se almacenan en el almacén de datos.

> [!NOTE]
> Las facetas solo se pueden aplicar a propiedades de tipo **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.

| Nombre de atributo                                                         | Es necesario | Valor                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | Sí         | Nombre de la propiedad.                                                                                                                                                                                                       |
| **Type**                                                               | Sí         | Tipo del valor de propiedad. El tipo de valor de propiedad debe ser un **EDMSimpleType** o un tipo complejo (indicado por un nombre completo) que se encuentra dentro del ámbito del modelo.                                                 |
| **Que acepta valores null**                                                           | No          | **True** (el valor predeterminado) o <strong>False</strong> dependiendo de si la propiedad puede tener un valor null. <br/> [!NOTE]                                                                                                   |
| > En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | No          | Valor predeterminado de la propiedad.                                                                                                                                                                                              |
| **MaxLength**                                                          | No          | Longitud máxima del valor de propiedad.                                                                                                                                                                                       |
| **FixedLength**                                                        | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.                                                                                                                          |
| **Precisión**                                                          | No          | Precisión del valor de propiedad.                                                                                                                                                                                            |
| **Escalar**                                                              | No          | Escala del valor de propiedad.                                                                                                                                                                                                |
| **SRID**                                                               | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.                                                                                                                               |
| **intercalación**                                                          | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | No          | **Ninguno** (el valor predeterminado) o **Fixed**. Si el valor se establece en **Fixed**, el valor de propiedad se usará en las comprobaciones de simultaneidad optimista.                                                                                  |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con tres **propiedad** elementos:

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

El ejemplo siguiente se muestra un **ComplexType** elemento con cinco **propiedad** elementos:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>Aplicación de elemento RowType

**Propiedad** elementos (como los elementos secundarios de un **RowType** elemento) definen la forma y características de los datos que se pueden pasar o devolver desde una función definida por el modelo.  

El **propiedad** elemento puede tener exactamente uno de los elementos secundarios siguientes:

-   CollectionType
-   ReferenceType
-   RowType

El **propiedad** elemento puede tener cualquier número elementos de anotación de secundarios.

> [!NOTE]
> Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.

 

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.

| Nombre de atributo                                                     | Es necesario | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | Sí         | Nombre de la propiedad.                                                                                                                                                                                                       |
| **Type**                                                           | Sí         | Tipo del valor de propiedad.                                                                                                                                                                                                 |
| **Que acepta valores null**                                                       | No          | **True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null. <br/> [!NOTE]                                                                                                                |
| > En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | No          | Valor predeterminado de la propiedad.                                                                                                                                                                                              |
| **MaxLength**                                                      | No          | Longitud máxima del valor de propiedad.                                                                                                                                                                                       |
| **FixedLength**                                                    | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.                                                                                                                          |
| **Precisión**                                                      | No          | Precisión del valor de propiedad.                                                                                                                                                                                            |
| **Escalar**                                                          | No          | Escala del valor de propiedad.                                                                                                                                                                                                |
| **SRID**                                                           | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.                                                                                                                               |
| **intercalación**                                                      | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                   |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

#### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra **propiedad** elementos que se usan para definir la forma del tipo de valor devuelto de una función definida por el modelo.

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef (Elemento) (CSDL)

El **PropertyRef** elemento de lenguaje de definición de esquemas conceptuales (CSDL) hace referencia a una propiedad de un tipo de entidad para indicar que la propiedad realizará uno de los roles siguientes:

-   Parte de la clave de la entidad (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad). Uno o varios **PropertyRef** elementos se pueden utilizar para definir una clave de entidad.
-   El extremo dependiente o principal de una restricción referencial.

El **PropertyRef** elemento solo puede tener elementos annotation (cero o más) como elementos secundarios.

> [!NOTE]
> Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.

 

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **PropertyRef** elemento.

| Nombre de atributo | Es necesario | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Sí         | Nombre de la propiedad a la que se hace referencia. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **PropertyRef** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente define un tipo de entidad (**libro**). La clave de entidad se define haciendo referencia a la **ISBN** propiedad del tipo de entidad.

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

En el ejemplo siguiente, dos **PropertyRef** elementos se utilizan para indicar que dos propiedades (**Id** y **PublisherId**) son los extremos principales y dependientes de una referencial restricción.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType (Elemento) (CSDL)

El **ReferenceType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) especifica una referencia a un tipo de entidad. El **ReferenceType** elemento puede ser un elemento secundario de los siguientes elementos:

-   ReturnType (función)
-   Parámetro
-   CollectionType

El **ReferenceType** elemento se usa al definir un tipo de parámetro o valor devuelto para una función.

Un **ReferenceType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReferenceType** elemento.

| Nombre de atributo | Es necesario | Valor                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | Sí         | Nombre del tipo de entidad al que se hace referencia. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReferenceType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **ReferenceType** elemento que se utiliza como elemento secundario de un **parámetro** elemento en una función definida por modelo que acepta una referencia a un **persona** entidad tipo:

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

El ejemplo siguiente se muestra el **ReferenceType** elemento que se utiliza como elemento secundario de un **ReturnType** elemento (función) en una función definida por modelo que devuelve una referencia a un **persona**tipo de entidad:

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint (Elemento) (CSDL)

Un **ReferentialConstraint** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional. Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad. El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción. El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción.

Si una clave externa que se expone en un tipo de entidad hace referencia a una propiedad en otro tipo de entidad, el **ReferentialConstraint** elemento define una asociación entre los dos tipos de entidad. Dado que el **ReferentialConstraint** elemento proporciona información acerca de cómo dos tipos de entidad están relacionadas, ningún elemento AssociationSetMapping correspondiente es necesaria en el lenguaje de especificación de asignaciones (MSL). Una asociación entre dos tipos de entidad que no tienen claves externas expuestas debe tener su correspondiente **AssociationSetMapping** elemento con el fin de asignar la información de asociación al origen de datos.

Si una clave externa no se expone en un tipo de entidad, el **ReferentialConstraint** elemento solo puede definir una restricción de clave principal de clave principal entre el tipo de entidad y otro tipo de entidad.

Un **ReferentialConstraint** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Entidad de seguridad (exactamente un elemento)
-   Dependiente (exactamente un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

El **ReferentialConstraint** elemento puede tener cualquier número de atributos de anotación (atributos XML personalizados). Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que se va a utilizar como parte de la definición de la **PublishedBy** asociación.

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (función) (elemento) (CSDL)

El **ReturnType** elemento (función) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento Function. Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.

Devolver tipos pueden ser cualquiera **EdmSimpleType**, tipo de entidad, tipo complejo, tipo de fila, tipo ref o una colección de uno de estos tipos.

Se puede especificar el tipo de valor devuelto de una función con cualquiera el **tipo** atributo de la **ReturnType** elemento (función), o con uno de los elementos secundarios siguientes:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Un modelo no se validará si especifica una función de valor devuelto de tipo con ambos el **tipo** atributo de la **ReturnType** elemento (función) y uno de los elementos secundarios.

 

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReturnType** elemento (función).

| Nombre de atributo | Es necesario | Valor                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | No          | El tipo devuelto por la función. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** elemento (función). Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se usa un **función** elemento para definir una función que devuelve el número de años que lleva un libro en la impresión. Tenga en cuenta que se especifica el tipo de valor devuelto por la **tipo** atributo de un **ReturnType** elemento (función).

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) (elemento) (CSDL)

El **ReturnType** (FunctionImport) (elemento) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento FunctionImport. Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.

Devolver tipos pueden ser cualquier colección de tipo de entidad, tipo complejo, o **EdmSimpleType**,

Se especifica el tipo de valor devuelto de una función con el **tipo** atributo de la **ReturnType** (FunctionImport) (elemento).

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReturnType** (FunctionImport) (elemento).

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | No          | Tipo devuelto por la función. El valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.                                                                                      |
| **EntitySet**  | No          | Si la función devuelve una colección de entidades tipos, el valor de la **EntitySet** deben ser el conjunto de entidades al que pertenece la colección. En caso contrario, el **EntitySet** no debe usarse el atributo. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** (FunctionImport) (elemento). Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se usa un **FunctionImport** que devuelve los libros en pantalla y los publicadores. Tenga en cuenta que la función devuelve dos conjuntos de resultados y, por tanto, dos **ReturnType** se especifican los elementos (FunctionImport).

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType (Elemento) (CSDL)

Un **RowType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define una estructura sin nombre como un parámetro o tipo de valor devuelto para una función definida en el modelo conceptual.

Un **RowType** elemento puede ser el elemento secundario de los siguientes elementos:

-   CollectionType
-   Parámetro
-   ReturnType (función)

Un **RowType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Propiedad (uno o más)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **RowType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra una función definida por modelo que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a>Schema (Elemento) (CSDL)

El **esquema** elemento es el elemento raíz de una definición de modelo conceptual. Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo conceptual.

El **esquema** elemento puede contener cero o más de los elementos secundarios siguientes:

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   Asociación
-   ComplexType
-   Función

Un **esquema** elemento puede contener cero o un elemento de anotación.

> [!NOTE]
> El **función** elemento y los elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.

 

El **esquema** elemento utiliza la **Namespace** atributo para definir el espacio de nombres para los objetos de asociación, el tipo complejo y el tipo de entidad en un modelo conceptual. Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre. Los espacios de nombres pueden abarcar varios **esquema** varios archivos .csdl y elementos.

Un espacio de nombres del modelo conceptual es diferente del espacio de nombres XML de la **esquema** elemento. Un espacio de nombres del modelo conceptual (tal como se define por la **Namespace** atributo) es un contenedor lógico para tipos de entidad, tipos complejos y tipos de asociación. El espacio de nombres XML (indicado por el **xmlns** atributo) de un **esquema** elemento es el espacio de nombres predeterminado para los elementos secundarios y atributos de la **esquema** elemento. Espacios de nombres XML del formulario http://schemas.microsoft.com/ado/YYYY/MM/edm (donde AAAA y MM representan un año y mes, respectivamente) se reservan para CSDL. No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos se pueden aplicar a la **esquema** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espacio de nombres**  | Sí         | El espacio de nombres del modelo conceptual. El valor de la **Namespace** atributo se usa para formar el nombre completo de un tipo. Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres Simple.Example.Model, el nombre completo de la **EntityType** es SimpleExampleModel.Customer. <br/> No se puede usar las siguientes cadenas como valor para el **espacio de nombres** atributo: **sistema**, **transitorios**, o **Edm**. El valor de la **espacio de nombres** atributo no puede ser el mismo que el valor de la **espacio de nombres** atributo en el elemento de esquema de SSDL. |
| **Alias**      | No          | Un identificador usado en lugar del nombre del espacio de nombres. Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres Simple.Example.Model y el valor de la **Alias** atributo es *modelo*, puede usar Model.Customer como el nombre completo de la **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **esquema** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **esquema** elemento que contiene un **EntityContainer** elemento, dos **EntityType** elementos y otra **asociación** elemento.

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a>TypeRef (Elemento) (CSDL)

El **TypeRef** elemento de lenguaje de definición de esquemas conceptuales (CSDL) proporciona una referencia a un tipo con nombre existente. El **TypeRef** elemento puede ser un elemento secundario del elemento CollectionType, que se usa para especificar que una función tiene una colección como un parámetro o tipo de devolución.

Un **TypeRef** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **TypeRef** elemento. Tenga en cuenta que el **DefaultValue**, **MaxLength**, **FixedLength**, **precisión**, **escala**,  **Unicode**, y **intercalación** atributos solo son aplicables a **EDMSimpleTypes**.

| Nombre de atributo                                                     | Es necesario | Valor                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | No          | El nombre del tipo al que se hace referencia.                                                                                                                                                                                          |
| **Que acepta valores null**                                                       | No          | **True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null. <br/> [!NOTE]                                                                                                                |
| > En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | No          | Valor predeterminado de la propiedad.                                                                                                                                                                                              |
| **MaxLength**                                                      | No          | Longitud máxima del valor de propiedad.                                                                                                                                                                                       |
| **FixedLength**                                                    | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.                                                                                                                          |
| **Precisión**                                                      | No          | Precisión del valor de propiedad.                                                                                                                                                                                            |
| **Escalar**                                                          | No          | Escala del valor de propiedad.                                                                                                                                                                                                |
| **SRID**                                                           | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | No          | **True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.                                                                                                                               |
| **intercalación**                                                      | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                   |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra una función definida por modelo que usa el **TypeRef** elemento (como un elemento secundario de un **CollectionType** elemento) para especificar que la función acepta una colección de  **Departamento** tipos de entidad.

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a>Using (Elemento) (CSDL)

El **Using** elemento de lenguaje de definición de esquemas conceptuales (CSDL) importa el contenido de un modelo conceptual que existe en un espacio de nombres diferente. Estableciendo el valor de la **Namespace** atributo, puede hacer referencia a tipos de entidad, tipos complejos y tipos de asociación que se definen en otro modelo conceptual. Más de un **Using** elemento puede ser un elemento secundario de un **esquema** elemento.

> [!NOTE]
> El **Using** elemento en CSDL no funciona exactamente igual que un **mediante** instrucción en un lenguaje de programación. Mediante la importación de un espacio de nombres con un **mediante** instrucción en un lenguaje de programación, no afectan a los objetos en el espacio de nombres original. En CSDL, un espacio de nombres importado puede contener un tipo de entidad derivado de un tipo de entidad del espacio de nombres original. Esto puede afectar a los conjuntos de entidades declarados en el espacio de nombres original.

 

El **Using** elemento puede tener los elementos secundarios siguientes:

-   Documentación (cero o un elementos)
-   Elementos annotation (cero o más elementos)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos se pueden aplicar a la **Using** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espacio de nombres**  | Sí         | Nombre del espacio de nombres importado.                                                                                                                                                |
| **Alias**      | Sí         | Un identificador usado en lugar del nombre del espacio de nombres. Aunque este atributo es obligatorio, no es necesario usarlo en lugar del nombre del espacio de nombres para calificar los nombres de los objetos. |

 

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Using** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

 

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra el **Using** elemento que se usa para importar un espacio de nombres que está definida en otra parte. Tenga en cuenta que el espacio de nombres para el **esquema** elemento mostrado es `BooksModel`. El `Address` propiedad en el `Publisher` **EntityType** es un tipo complejo que se define en el `ExtendedBooksModel` espacio de nombres (importado con la **Using** elemento).

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a>Atributos de anotación (CSDL)

En el lenguaje de definición de esquemas conceptuales (CSDL), los atributos de anotación son atributos XML personalizados del modelo conceptual. Además de tener una estructura XML válida, los atributos de anotación deben cumplir las condiciones siguientes:

-   Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para CSDL.
-   Se pueden aplicar varios atributos de anotación a un elemento CSDL determinado.
-   Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.

Los atributos de anotación se pueden usar para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual. Pueden tener acceso a los metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con un atributo de anotación (**CustomAttribute**). El ejemplo también muestra un elemento Annotation aplicado al elemento de tipo de entidad.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

El siguiente código recupera los metadatos del atributo Annotation y los escribe en la consola:

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

El código anterior supone que el archivo `School.csdl` está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Elementos Annotation (CSDL)

Los elementos Annotation en el lenguaje de definición de esquemas conceptuales (CSDL) son los elementos XML personalizados del modelo conceptual. Además de tener una estructura XML válida, lo siguiente debe ser verdadero para los elementos Annotation:

-   Los elementos Annotation no deben estar en un espacio de nombres XML que esté reservado para CSDL.
-   Más de un elemento Annotation puede ser un elemento secundario de un elemento CSDL determinado.
-   Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.
-   Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento CSDL determinado.

Los elementos Annotation pueden utilizarse para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual. A partir de .NET Framework versión 4, pueden obtenerse metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con un elemento annotation (**CustomElement**). El ejemplo también muestra un atributo de anotación aplicado al elemento de tipo de entidad.

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

El siguiente código recupera los metadatos del elemento de anotación y los escribe en la consola:

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

El código anterior supone que el archivo School.csdl está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Tipos de modelos conceptuales (CSDL)

Lenguaje de definición de esquemas conceptuales (CSDL) admite un conjunto de tipos de datos primitivos abstractos, denominados **EDMSimpleTypes**, que definen las propiedades en un modelo conceptual. **EDMSimpleTypes** son objetos proxy para los tipos de datos primitivos que se admiten en el almacenamiento o el entorno de hospedaje.

En la tabla siguiente se enumeran los tipos de datos primitivos admitidos por CSDL. La tabla también enumeran las facetas que se pueden aplicar a cada **EDMSimpleType**.

| EDMSimpleType                    | Descripción                                                | Facetas aplicables                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | Contiene datos binarios.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Edm.Boolean**                  | Contiene el valor **true** o **false**.                  | Nullable, Default                                                        |
| **Edm.Byte**                     | Contiene un valor entero de 8 bits sin signo.                  | Precision, Nullable, Default                                             |
| **Edm.DateTime**                 | Representa una fecha y hora.                                | Precision, Nullable, Default                                             |
| **Edm.DateTimeOffset**           | Contiene una fecha y hora como un desplazamiento en minutos con respecto a GMT. | Precision, Nullable, Default                                             |
| **Edm.Decimal**                  | Contiene un valor numérico con una precisión y escala fijas.   | Precision, Nullable, Default                                             |
| **Edm.Double**                   | Contiene un número con 15 dígitos de precisión de punto flotante   | Precision, Nullable, Default                                             |
| **Edm.Float**                    | Contiene un número de punto flotante con una precisión de 7 dígitos.   | Precision, Nullable, Default                                             |
| **Edm.Guid**                     | Contiene un identificador único de 16 bytes.                      | Precision, Nullable, Default                                             |
| **Edm.Int16**                    | Contiene un valor entero de 16 bits con signo.                    | Precision, Nullable, Default                                             |
| **Edm.Int32**                    | Contiene un valor entero de 32 bits con signo.                    | Precision, Nullable, Default                                             |
| **Edm.Int64**                    | Contiene un valor entero de 64 bits con signo.                    | Precision, Nullable, Default                                             |
| **Edm.SByte**                    | Contiene un valor entero de 8 bits con signo.                     | Precision, Nullable, Default                                             |
| **Edm.String**                   | Contiene datos de caracteres.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **Edm.Time**                     | Contiene una hora del día.                                    | Precision, Nullable, Default                                             |
| **Edm.Geography**                |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.Geometry**                 |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Que acepta valores NULL, predeterminado, SRID                                                  |

## <a name="facets-csdl"></a>Facets (CSDL)

En el lenguaje de definición de esquemas conceptuales (CSDL), las facetas representan restricciones en las propiedades de tipos de entidad y tipos complejos. Las facetas aparecen como atributos XML en los elementos CSDL siguientes:

-   Property
-   TypeRef
-   Parámetro

En la tabla siguiente se describen las facetas que se admiten en CSDL. Todas las facetas son opcionales. Algunas facetas que se enumeran a continuación se usan por Entity Framework al generar una base de datos de un modelo conceptual.

> [!NOTE]
> Para obtener información acerca de los tipos de datos en un modelo conceptual, vea tipos de modelos conceptuales (CSDL).

| Faceta               | Descripción                                                                                                                                                                                                                                                   | Se aplica a                                                                                                                                                                                                                                                                                                                                                                           | Se utiliza para la generación de la base de datos | La usa el runtime. |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **intercalación**       | Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Sí                              | No                  |
| **ConcurrencyMode** | Indica que el valor de propiedad se debería utilizar para las comprobaciones de la simultaneidad optimista.                                                                                                                                                                    | Todos los **EDMSimpleType** propiedades                                                                                                                                                                                                                                                                                                                                                     | No                               | Sí                 |
| **Predetermiado**         | Especifica el valor predeterminado de la propiedad si no se proporciona ningún valor al crear las instancias.                                                                                                                                                                       | Todos los **EDMSimpleType** propiedades                                                                                                                                                                                                                                                                                                                                                     | Sí                              | Sí                 |
| **FixedLength**     | Especifica si la longitud del valor de propiedad puede variar.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Sí                              | No                  |
| **MaxLength**       | Especifica la longitud máxima del valor de propiedad.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Sí                              | No                  |
| **Que acepta valores null**        | Especifica si la propiedad puede tener un **null** valor.                                                                                                                                                                                                     | Todos los **EDMSimpleType** propiedades                                                                                                                                                                                                                                                                                                                                                     | Sí                              | Sí                 |
| **Precisión**       | Las propiedades de tipo **Decimal**, especifica el número de dígitos que puede tener un valor de propiedad. Las propiedades de tipo **tiempo**, **DateTime**, y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de segundos del valor de propiedad. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Sí                              | No                  |
| **Escalar**           | Especifica el número de dígitos que puede haber a la derecha del separador decimal para el valor de propiedad.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Sí                              | No                  |
| **SRID**            | Especifica el identificador de sistema de referencia espacial sistema. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | No                               | Sí                 |
| **Unicode**         | Indica si el valor de propiedad está almacenado como Unicode.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Sí                              | Sí                 |

>[!NOTE]
> Al generar una base de datos de un modelo conceptual, el Asistente para generar base de datos reconocerá el valor de la **StoreGeneratedPattern** atributo un **propiedad** elemento si encuentra en la siguiente espacio de nombres: http://schemas.microsoft.com/ado/2009/02/edm/annotation. Los valores admitidos para el atributo son **identidad** y **calculado**. Un valor de **identidad** generará una columna de base de datos con un valor de identidad que se genera en la base de datos. Un valor de **calculado** generará una columna con un valor que se calcula en la base de datos.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestran facetas aplicadas a las propiedades de un tipo de entidad:

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
