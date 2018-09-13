---
title: Especificación de SSDL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490355"
---
# <a name="ssdl-specification"></a>Especificación SSDL
El lenguaje de definición de esquemas de almacenamiento (SSDL) es un lenguaje basado en XML que describe el modelo de almacenamiento de una aplicación Entity Framework.

En una aplicación de Entity Framework, los metadatos del modelo de almacenamiento se carga desde un archivo .ssdl (escrito en SSDL) en una instancia de la System.Data.Metadata.Edm.StoreItemCollection y es accesible mediante el uso de métodos en el Clase System.Data.Metadata.Edm.MetadataWorkspace. Entity Framework usa los metadatos del modelo de almacenamiento para traducir las consultas con el modelo conceptual en comandos específicos del almacenamiento.

El Diseñador de Entity Framework (EF Designer) almacena información de modelo de almacenamiento en un archivo .edmx en tiempo de diseño. En tiempo de compilación Entity Designer usa información en un archivo .edmx para crear el archivo .ssdl que Entity Framework necesita en tiempo de ejecución.

Las versiones de SSDL se diferencian por los espacios de nombres XML.

| Versión SSDL | Namespace XML                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association (Elemento) (SSDL)

Un **asociación** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica las columnas de tabla que participan en una restricción foreign key en la base de datos subyacente. Dos elementos de extremo secundario necesario especifican las tablas en los extremos de la asociación y la multiplicidad en cada extremo. Un elemento ReferentialConstraint opcional especifica los extremos principales y dependientes de la asociación, así como las columnas que participan. Si no hay ningún **ReferentialConstraint** elemento está presente, se debe usar un elemento AssociationSetMapping para especificar las asignaciones de columnas para la asociación.

El **asociación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   End (exactamente dos)
-   ReferentialConstraint (cero o uno)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **asociación** elemento.

| Nombre de atributo | Es necesario | Valor                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Name**       | Sí         | El nombre de la restricción de clave externa correspondiente de la base de datos subyacente. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que se usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders**  restricción de clave externa:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a>AssociationSet (Elemento) (SSDL)

El **AssociationSet** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa entre dos tablas en la base de datos subyacente. Las columnas de tabla que participan en la restricción de clave externa se especifican en un elemento Association. El **asociación** elemento que corresponde a un determinado **AssociationSet** elemento se especifica en el **asociación** atributo de la **AssociationSet**  elemento.

Conjuntos de asociaciones SSDL se asignan a los conjuntos de asociaciones CSDL por un elemento AssociationSetMapping. Sin embargo, si establece la asociación CSDL para una determinada asociación CSDL se define mediante el uso de un elemento ReferentialConstraint, no existe la correspondiente **AssociationSetMapping** elemento es necesario. En este caso, si un **AssociationSetMapping** elemento está presente, las asignaciones que define serán reemplazadas por el **ReferentialConstraint** elemento.

El **AssociationSet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   End (cero o dos)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSet** elemento.

| Nombre de atributo  | Es necesario | Valor                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Name**        | Sí         | El nombre de la restricción de clave externa representada por el conjunto de asociaciones.                          |
| **Asociación** | Sí         | El nombre de la asociación que define las columnas que participan en la restricción de clave externa. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **AssociationSet** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **AssociationSet** elemento que representa el `FK_CustomerOrders` restricción foreign key en la base de datos subyacente:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType (elemento) (SSDL)

El **CollectionType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica que el tipo de valor devuelto de una función es una colección. El **CollectionType** es un elemento secundario del elemento ReturnType. El tipo de colección se especifica mediante el elemento secundario de tipo de fila:

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra una función que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a>CommandText (Elemento) (SSDL)

El **CommandText** en lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento Function que le permite definir una instrucción SQL que se ejecuta en la base de datos. El **CommandText** elemento le permite agregar funcionalidad similar a un procedimiento almacenado en la base de datos, pero define el **CommandText** elemento del modelo de almacenamiento.

El **CommandText** elemento no puede tener elementos secundarios. El cuerpo de la **CommandText** elemento debe ser una instrucción SQL válida para la base de datos subyacente.

No hay atributos son aplicables a la **CommandText** elemento.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **función** elemento con un elemento secundario **CommandText** elemento. Exponer el **UpdateProductInOrder** funcionar como un método en la clase ObjectContext, impórtelo en el modelo conceptual.  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a>DefiningQuery (Elemento) (SSDL)

El **DefiningQuery** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) permite ejecutar una instrucción SQL directamente en la base de datos subyacente. El **DefiningQuery** elemento se utiliza normalmente como una vista de base de datos, pero la vista se define en el modelo de almacenamiento en lugar de la base de datos. La vista definida en un **DefiningQuery** elemento puede asignarse a un tipo de entidad en el modelo conceptual a través de un elemento EntitySetMapping. Estas asignaciones son de solo lectura.  

La sintaxis de SSDL siguiente muestra la declaración de un **EntitySet** seguido por el **DefiningQuery** elemento que contiene una consulta usada para recuperar la vista.

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

Puede usar los procedimientos almacenados en Entity Framework para habilitar escenarios de lectura y escritura a través de vistas. Puede usar una vista del origen de datos o una vista de Entity SQL como la tabla base para recuperar datos y para el procesamiento de cambios por procedimientos almacenados.

Puede usar el **DefiningQuery** elemento de destino de Microsoft SQL Server Compact 3.5. Aunque SQL Server Compact 3.5 no admite procedimientos almacenados, puede implementar una funcionalidad similar con el **DefiningQuery** elemento. Otro caso donde puede ser útil es en la creación de procedimientos almacenados para resolver una desigualdad entre los tipos de datos utilizados en el lenguaje de programación y los del origen de datos. Podría escribir un **DefiningQuery** que toma un cierto conjunto de parámetros y, a continuación, llama a un procedimiento almacenado con un conjunto diferente de parámetros, por ejemplo, un procedimiento almacenado que elimina los datos.

## <a name="dependent-element-ssdl"></a>Dependent (Elemento) (SSDL)

El **dependientes** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo dependiente de una restricción de clave externa (también llamada restricción referencial). El **dependientes** elemento especifica la columna (o columnas) en una tabla que hacen referencia a una columna de clave principal (o columnas). **PropertyRef** elementos especifican las columnas que se hace referencia. La entidad de seguridad de elemento especifica las columnas de clave principal que hacen referencia las columnas que se especifican en el **dependientes** elemento.

El **dependientes** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   PropertyRef (uno o más)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **dependientes** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Sí         | El mismo valor que el **rol** atributo (si se utiliza) del elemento final correspondiente; en caso contrario, el nombre de la tabla que contiene la columna de referencia. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **dependientes** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra un elemento Association que usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders** clave externa restricción. El **dependientes** elemento especifica el **CustomerId** columna de la **orden** tabla como el extremo dependiente de la restricción.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a>Documentation (Elemento) (SSDL)

El **documentación** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario.

El **documentación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   **Resumen**: una breve descripción del elemento primario. (cero o un elemento).
-   **LongDescription**: una descripción extensa del elemento primario. (cero o un elemento).

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **documentación** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **documentación** como un elemento secundario de un elemento EntityType.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a>End (Elemento) (SSDL)

El **final** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica la tabla y el número de filas de un extremo de una restricción foreign key en la base de datos subyacente. El **final** elemento puede ser un elemento secundario del elemento Association o el elemento AssociationSet. En cada caso, los posibles elementos secundarios y atributos aplicables son diferentes.

### <a name="end-element-as-a-child-of-the-association-element"></a>Elemento End como elemento secundario del elemento Association

Un **final** elemento (como un elemento secundario de la **asociación** elemento) especifica la tabla y el número de filas al final de una restricción foreign key con la **tipo** y **Multiplicidad** respectivamente. Los extremos de una restricción de clave externa se definen como parte de una asociación SSDL, la cual debe tener exactamente dos extremos.

Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   OnDelete (cero o un elemento)
-   Elementos annotation (cero o más elementos)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **asociación** elemento.

| Nombre de atributo   | Es necesario | Valor                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Sí         | El nombre completo del conjunto de entidades SSDL que es el final de la restricción foreign key.                                                                                                                                                                                                                                                                                          |
| **Rol**         | No          | El valor de la **rol** atributo en la entidad de seguridad o dependiente del elemento del elemento ReferentialConstraint correspondiente (si se usa).                                                                                                                                                                                                                                             |
| **Multiplicidad** | Sí         | **1**, **de 0.. 1**, o **\*** según el número de filas que pueden estar al final de la restricción foreign key. <br/> **1** indica que existe exactamente una fila al final de la restricción de clave externa. <br/> **de 0.. 1** indica que existe una o ninguna fila al final de la restricción de clave externa. <br/> **\*** indica que el cero, uno o más filas hay al final de la restricción de clave externa. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que define el **FK\_CustomerOrders** restricción de clave externa. El **multiplicidad** valores especificados en cada **final** elemento indican que muchas filas en el **pedidos** tabla puede asociarse con una fila en la **a los clientes**  tabla, pero solo una fila en la **clientes** tabla puede asociarse con una fila en la **pedidos** tabla. Además, el **OnDelete** elemento indica que todas las filas de la **pedidos** tabla que hacen referencia a una fila determinada en el **clientes** se eliminará la tabla si la fila de el **clientes** se elimina la tabla.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Elemento End como elemento secundario del elemento AssociationSet

El **final** elemento (como un elemento secundario de la **AssociationSet** elemento) especifica una tabla en un extremo de una restricción foreign key de la base de datos subyacente.

Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   Elementos de anotación (cero o más)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **AssociationSet** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Sí         | El nombre del conjunto de entidades SSDL que es el final de la restricción foreign key.                                      |
| **Rol**       | No          | El valor de uno de los **rol** especificados en uno de los atributos **final** elemento del elemento Association correspondiente. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

#### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento con un **AssociationSet** elemento con dos **final** elementos:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a>EntityContainer (Elemento) (SSDL)

Un **EntityContainer** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) describe la estructura del origen de datos subyacente en una aplicación de Entity Framework: conjuntos de entidades SSDL (definidos en elementos EntitySet) representan las tablas en una base de datos, los tipos de entidad SSDL (definidos en elementos EntityType) representan filas de una tabla, y conjuntos de asociaciones (definidos en elementos de AssociationSet) representan restricciones de clave externa en una base de datos. Un contenedor de entidades del modelo de almacenamiento se asigna a un contenedor de entidades del modelo conceptual a través del elemento EntityContainerMapping.

Un **EntityContainer** elemento puede tener cero o un elemento de la documentación. Si un **documentación** elemento está presente, debe preceder a todos los demás elementos secundarios.

Un **EntityContainer** elemento puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):

-   EntitySet
-   AssociationSet
-   Elementos Annotation

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityContainer** elemento.

| Nombre de atributo | Es necesario | Valor                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del contenedor de entidades. Este nombre no puede contener puntos (.). |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityContainer** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento que define dos conjuntos de entidades y un conjunto de asociaciones. Observe que los nombres de los tipos de entidad y tipos de asociación están calificados mediante el nombre del espacio de nombres del modelo conceptual.

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a>EntitySet (Elemento) (SSDL)

Un **EntitySet** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una tabla o vista en la base de datos subyacente. Un elemento EntityType en SSDL representa una fila en la tabla o vista. El **EntityType** atributo de un **EntitySet** elemento especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL. La asignación entre un conjunto de entidades CSDL y un conjunto de entidades SSDL se especifica en un elemento EntitySetMapping.

El **EntitySet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   DefiningQuery (cero o un elemento)
-   Elementos Annotation

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySet** elemento.

> [!NOTE]
> Algunos atributos (no mostrados aquí) se pueden calificar con el **almacenar** alias. Estos atributos se utilizan el Asistente para actualizar modelo al actualizar un modelo.

| Nombre de atributo | Es necesario | Valor                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del conjunto de entidades.                                                              |
| **EntityType** | Sí         | El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias. |
| **Esquema**     | No          | El esquema de base de datos.                                                                     |
| **Table**      | No          | La tabla de base de datos.                                                                      |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntitySet** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainer** elemento que tiene dos **EntitySet** elementos y otra **AssociationSet** elemento:

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a>EntityType (Elemento) (SSDL)

Un **EntityType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una fila en una tabla o vista de la base de datos subyacente. Un elemento EntitySet de SSDL representa la tabla o vista en la que se producen las filas. El **EntityType** atributo de un **EntitySet** elemento especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL. La asignación entre un tipo de entidad SSDL y un tipo de entidad CSDL se especifica en un elemento EntityTypeMapping.

El **EntityType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o un elemento)
-   Clave (cero o un elemento)
-   Elementos Annotation

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityType** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del tipo de entidad. Este valor normalmente es igual que el nombre de la tabla en la que el tipo de entidad representa una fila. Este valor no puede contener ningún punto (.). |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con dos propiedades:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a>Function (Elemento) (SSDL)

El **función** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica un procedimiento almacenado que existe en la base de datos subyacente.

El **función** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   Parámetro (cero o más)
-   CommandText (cero o uno)
-   ReturnType (cero o más)
-   Elementos de anotación (cero o más)

Un devuelto de tipo para una función debe especificarse con el **ReturnType** elemento o el **ReturnType** atributo (ver abajo), pero no ambos.

Los procedimientos almacenados que se especifican en el modelo de almacenamiento se pueden importar en el modelo conceptual de una aplicación. Para obtener más información, consulte [realizar consultas con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/query.md). El **función** elemento también se puede usar para definir funciones personalizadas en el modelo de almacenamiento.  

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **función** elemento.

> [!NOTE]
> Algunos atributos (no mostrados aquí) se pueden calificar con el **almacenar** alias. Estos atributos se utilizan el Asistente para actualizar modelo al actualizar un modelo.

| Nombre de atributo             | Es necesario | Valor                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                   | Sí         | Nombre del procedimiento almacenado.                                                                                                                                                                                  |
| **ReturnType**             | No          | El tipo de valor devuelto del procedimiento almacenado.                                                                                                                                                                           |
| **Aggregate**              | No          | **True** si el procedimiento almacenado devuelve un valor agregado; de lo contrario **False**.                                                                                                                                  |
| **BuiltIn**                | No          | **True** si la función es una función integrada<sup>1</sup> función; en caso contrario **False**.                                                                                                                                  |
| **StoreFunctionName**      | No          | Nombre del procedimiento almacenado.                                                                                                                                                                                  |
| **NiladicFunction**        | No          | **True** si la función es una función niládica<sup>2</sup> función; **False** en caso contrario.                                                                                                                                   |
| **IsComposable**           | No          | **True** si la función es una composición<sup>3</sup> función; **False** en caso contrario.                                                                                                                                |
| **ParameterTypeSemantics** | No          | La enumeración que define la semántica de tipos que se utiliza para resolver sobrecargas de función. La enumeración se define en el manifiesto del proveedor por cada definición de función. El valor predeterminado es **AllowImplicitConversion**. |
| **Esquema**                 | No          | El nombre del esquema donde se define el procedimiento almacenado.                                                                                                                                                   |

<sup>1</sup> una función integrada es una función que se define en la base de datos. Para obtener información acerca de las funciones que se definen en el modelo de almacenamiento, vea el elemento CommandText (SSDL).

<sup>2</sup> una función niládica es una función que no acepta parámetros y, cuando se llama, no requiere paréntesis.

<sup>3</sup> dos funciones admiten composición si el resultado de una función puede ser la entrada para otra función.

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **función** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **función** elemento que corresponde a la **UpdateOrderQuantity** procedimiento almacenado. El procedimiento almacenado acepta dos parámetros y no devuelve ningún valor.

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a>Key (Elemento) (SSDL)

El **clave** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa la clave principal de una tabla en la base de datos subyacente. **Clave** es un elemento secundario de un elemento EntityType, que representa una fila de una tabla. La clave principal se define en el **clave** elemento haciendo referencia a uno o más elementos de propiedad que se definen en el **EntityType** elemento.

El **clave** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   PropertyRef (uno o más)
-   Elementos Annotation

No hay atributos son aplicables a la **clave** elemento.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con una clave que hace referencia a una propiedad:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a>OnDelete (Elemento) (SSDL)

El **OnDelete** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) refleja el comportamiento de la base de datos cuando se elimina una fila que participa en una restricción foreign key. Si la acción se establece en **Cascade**, a continuación, también se eliminarán las filas que hacen referencia a una fila que se va a eliminar. Si la acción se establece en **ninguno**, a continuación, no se eliminan las filas que hacen referencia a una fila que se va a eliminar. Un **OnDelete** elemento es un elemento secundario del elemento final.

Un **OnDelete** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **OnDelete** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Acción**     | Sí         | **CASCADE** o **ninguno**. (El valor **Restricted** es válido pero tiene el mismo comportamiento que **ninguno**.) |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **OnDelete** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que define el **FK\_CustomerOrders** restricción de clave externa. El **OnDelete** elemento indica que todas las filas de la **pedidos** tabla que hacen referencia a una fila determinada en el **clientes** se eliminará la tabla si la fila en la **Clientes** se elimina la tabla.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a>Parameter (Elemento) (SSDL)

El **parámetro** en lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento Function que especifica los parámetros para un procedimiento almacenado en la base de datos.

El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Documentación (cero o uno)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Sí         | Nombre del parámetro.                                                                                                                                                                                                      |
| **Type**       | Sí         | Tipo del parámetro.                                                                                                                                                                                                             |
| **Modo**       | No          | **En**, **Out**, o **InOut** dependiendo de si el parámetro es una entrada, salida o parámetro de entrada/salida.                                                                                                                |
| **MaxLength**  | No          | Longitud máxima permitida del parámetro.                                                                                                                                                                                            |
| **Precisión**  | No          | Precisión del parámetro.                                                                                                                                                                                                 |
| **Escalar**      | No          | Escala del parámetro.                                                                                                                                                                                                     |
| **SRID**       | No          | Identificador de referencia espacial de sistema. Válido únicamente para los parámetros de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **función** elemento que tiene dos **parámetro** elementos que especifican los parámetros de entrada:

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a>Principal (Elemento) (SSDL)

El **Principal** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo principal de una restricción de clave externa (también llamada restricción referencial). El **Principal** elemento especifica la columna de clave principal (o columnas) en una tabla que hace referencia a otra columna (o columnas). **PropertyRef** elementos especifican las columnas que se hace referencia. El elemento dependiente especifica columnas que hacen referencia las columnas de clave principal que se especifican en el **Principal** elemento.

El **Principal** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   PropertyRef (uno o más)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **Principal** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Sí         | El mismo valor que el **rol** atributo (si se utiliza) del elemento final correspondiente; en caso contrario, el nombre de la tabla que contiene la columna que se hace referencia. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Principal** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra un elemento Association que usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders** clave externa restricción. El **Principal** elemento especifica el **CustomerId** columna de la **cliente** tabla como el extremo principal de la restricción.

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a>Property (Elemento) (SSDL)

El **propiedad** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una columna de una tabla en la base de datos subyacente. **Propiedad** son elementos secundarios de los elementos EntityType, que representan las filas de una tabla. Cada **propiedad** elemento definido en un **EntityType** elemento representa una columna.

Un **propiedad** elemento no puede tener elementos secundarios.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                  | Sí         | El nombre de la columna correspondiente.                                                                                                                                                                                           |
| **Type**                  | Sí         | El tipo de la columna correspondiente.                                                                                                                                                                                           |
| **Que acepta valores null**              | No          | **True** (el valor predeterminado) o **False** dependiendo de si la columna correspondiente puede tener un valor null.                                                                                                                  |
| **DefaultValue**          | No          | El valor predeterminado de la columna correspondiente.                                                                                                                                                                                  |
| **MaxLength**             | No          | La longitud máxima de la columna correspondiente.                                                                                                                                                                                 |
| **FixedLength**           | No          | **True** o **False** dependiendo de si el valor de columna correspondiente se almacenarán como una cadena de longitud fija.                                                                                                              |
| **Precisión**             | No          | La precisión de la columna correspondiente.                                                                                                                                                                                      |
| **Escalar**                 | No          | La escala de la columna correspondiente.                                                                                                                                                                                          |
| **Unicode**               | No          | **True** o **False** dependiendo de si el valor de columna correspondiente se almacenarán como una cadena Unicode.                                                                                                                   |
| **intercalación**             | No          | Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.                                                                                                                                                   |
| **SRID**                  | No          | Identificador de referencia espacial de sistema. Válido solo para las propiedades de tipos espaciales. Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | No          | **Ninguno**, **identidad** (si el valor correspondiente de la columna es una identidad que se genera en la base de datos), o **calculado** (si el valor de columna correspondiente se calcula en la base de datos). No es válido para las propiedades de tipo de fila. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityType** elemento con dos secundarios **propiedad** elementos:

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a>PropertyRef (Elemento) (SSDL)

El **PropertyRef** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) hace referencia a una propiedad definida en un elemento EntityType para indicar que la propiedad realizará uno de los roles siguientes:

-   Formar parte de la clave principal de la tabla que la **EntityType** representa. Uno o varios **PropertyRef** elementos se pueden utilizar para definir una clave principal. Para obtener más información, vea el elemento de clave.
-   Ser el extremo dependiente o principal de una restricción referencial. Para obtener más información, consulte ReferentialConstraint (elemento).

El **PropertyRef** elemento solo puede tener los elementos secundarios siguientes:

-   Documentación (cero o uno)
-   Elementos Annotation

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **PropertyRef** elemento.

| Nombre de atributo | Es necesario | Valor                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Sí         | Nombre de la propiedad a la que se hace referencia. |

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **PropertyRef** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **PropertyRef** elemento que se usa para definir una clave principal haciendo referencia a una propiedad que se define en un **EntityType** elemento.

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint (Elemento) (SSDL)

El **ReferentialConstraint** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa (también denominada una restricción de integridad referencial) en la base de datos subyacente. Los extremos principales y dependientes de la restricción se especifican respectivamente el principal y dependiente de los elementos secundarios. Se hace referencia a columnas que participan en los extremos principal y dependientes con elementos PropertyRef.

El **ReferentialConstraint** es un elemento secundario opcional del elemento Association. Si un **ReferentialConstraint** elemento no se usa para asignar la restricción foreign key que se especifica en el **asociación** elemento AssociationSetMapping al elemento que se debe usar para hacer esto.

El **ReferentialConstraint** elemento puede tener los elementos secundarios siguientes:

-   Documentación (cero o uno)
-   Entidad de seguridad (exactamente uno)
-   Dependiente (exactamente uno)
-   Elementos de anotación (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReferentialConstraint** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asociación** elemento que se usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders**  restricción de clave externa:

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a>ReturnType (elemento) (SSDL)

El **ReturnType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica el tipo de valor devuelto para una función que se define en un **función** elemento. Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.

Se especifica el tipo de valor devuelto de una función con el **tipo** atributo o la **ReturnType** elemento.

El **ReturnType** elemento puede tener los elementos secundarios siguientes:

- CollectionType (uno)  

> [!NOTE]
> Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** elemento. Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL. Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se usa un **función** que devuelve una colección de filas.

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a>RowType (elemento) (SSDL)

Un **RowType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) define una estructura sin nombre como un retorno de tipo para una función definida en el almacén.

Un **RowType** es el elemento secundario del **CollectionType** elemento:

Un **RowType** elemento puede tener los elementos secundarios siguientes:

- Propiedad (uno o más)  

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra una función de almacenamiento que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a>Schema (Elemento) (SSDL)

El **esquema** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es el elemento raíz de una definición de modelo de almacenamiento. Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo de almacenamiento.

El **esquema** elemento puede contener cero o más de los elementos secundarios siguientes:

-   Asociación
-   EntityType
-   EntityContainer
-   Función

El **esquema** elemento utiliza la **Namespace** atributo para definir el espacio de nombres para los objetos de asociación y tipo de entidad en un modelo de almacenamiento. Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre.

Un espacio de nombres del modelo de almacenamiento es diferente del espacio de nombres XML de la **esquema** elemento. Un espacio de nombres del modelo de almacenamiento (como se define en el **espacio de nombres** atributo) es un contenedor lógico para tipos de entidad y tipos de asociación. El espacio de nombres XML (indicado por el **xmlns** atributo) de un **esquema** elemento es el espacio de nombres predeterminado para los elementos secundarios y atributos de la **esquema** elemento. Espacios de nombres XML del formulario http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (donde AAAA y MM representan un año y mes, respectivamente) se reservan para SSDL. No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos se pueden aplicar a la **esquema** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espacio de nombres**             | Sí         | El espacio de nombres del modelo de almacenamiento. El valor de la **Namespace** atributo se usa para formar el nombre completo de un tipo. Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres ExampleModel.Store, el nombre completo de la **EntityType** es ExampleModel.Store.Customer. <br/> No se puede usar las siguientes cadenas como valor para el **espacio de nombres** atributo: **sistema**, **transitorios**, o **Edm**. El valor de la **espacio de nombres** atributo no puede ser el mismo que el valor de la **espacio de nombres** atributo en el elemento de esquema de CSDL. |
| **Alias**                 | No          | Un identificador usado en lugar del nombre del espacio de nombres. Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres ExampleModel.Store y el valor de la **Alias** atributo es *StorageModel*, puede usar StorageModel.Customer como nombre completo de la **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Proveedor**              | Sí         | El proveedor de datos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Sí         | Un token que indica al proveedor qué manifiesto del proveedor debe devolver. No se define ningún formato para el token. El proveedor define los valores para el token. Para obtener información acerca de los tokens de manifiesto de proveedor de SQL Server, vea SqlClient para Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **esquema** elemento que contiene un **EntityContainer** elemento, dos **EntityType** elementos y otra **asociación** elemento.

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a>Atributos de anotación

Los atributos de anotación (Annotation) del lenguaje de definición de esquemas de almacenamiento (SSDL) son atributos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre los elementos del modelo de almacenamiento. Además de tener una estructura XML válida, las siguientes restricciones se aplican a los atributos de anotación:

-   Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para SSDL.
-   Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.

Se pueden aplicar varios atributos de anotación a un elemento SSDL determinado. Pueden tener acceso a los metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra un elemento EntityType que tiene un atributo de anotación aplicado a la **OrderId** propiedad. El ejemplo también muestra un elemento de anotación que se agrega a la **EntityType** elemento.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a>Annotation (Elementos) (SSDL)

Los elementos Annotation del lenguaje de definición de esquemas de almacenamiento (SSDL) son elementos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre el modelo de almacenamiento. Además de tener una estructura XML válida, las siguientes restricciones se aplican a los elementos Annotation:

-   Los elementos Annotation no deben estar en un espacio de nombres de XML que esté reservado para SSDL.
-   Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.
-   Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento SSDL determinado.

Más de un elemento Annotation puede ser un elemento secundario de un elemento SSDL determinado. A partir de .NET Framework versión 4, pueden obtenerse metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra un elemento EntityType que tiene un elemento annotation (**CustomElement**). El ejemplo también muestra un atributo de anotación aplicado a la **OrderId** propiedad.

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a>Facetas (SSDL)

Las facetas en lenguaje de definición de esquemas de almacenamiento (SSDL) representan restricciones de tipos de columnas que se especifican en elementos de propiedad. Las facetas se implementan como atributos XML en **propiedad** elementos.

En la tabla siguiente se describen las facetas que se admiten en SSDL:

| Faceta           | Descripción                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **intercalación**   | Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.                                                                                                             |
| **FixedLength** | Especifica si la longitud del valor de la columna puede variar.                                                                                                                                                                                                  |
| **MaxLength**   | Especifica la longitud máxima del valor de la columna.                                                                                                                                                                                                           |
| **Precisión**   | Las propiedades de tipo **Decimal**, especifica el número de dígitos que puede tener un valor de propiedad. Las propiedades de tipo **tiempo**, **DateTime**, y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de segundos del valor de columna. |
| **Escalar**       | Especifica el número de dígitos que puede haber a la derecha del separador de decimales para el valor de la columna.                                                                                                                                                                      |
| **Unicode**     | Indica si el valor de la columna está almacenado como Unicode.                                                                                                                                                                                                    |
