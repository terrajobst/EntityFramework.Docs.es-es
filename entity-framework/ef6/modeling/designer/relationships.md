---
title: 'EF6 relaciones - EF Designer:'
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490688"
---
# <a name="relationships---ef-designer"></a>Relaciones - EF Designer
> [!NOTE]
> Esta página proporciona información sobre cómo configurar las relaciones en el modelo con EF Designer. Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md).

Las asociaciones definen las relaciones entre tipos de entidad en un modelo. En este tema se muestra cómo asignar las asociaciones con Entity Framework Designer (Diseñador de EF). La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Cuando se compila el modelo conceptual, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores. Puede omitir estas advertencias porque después de elegir generar la base de datos del modelo, los errores desaparecerán.

## <a name="associations-overview"></a>Información general de las asociaciones

Al diseñar el modelo usando el Diseñador de EF, un archivo .edmx representa el modelo. En el archivo .edmx, un **asociación** elemento define una relación entre dos tipos de entidad. Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad. La multiplicidad de un extremo de asociación puede tener un valor uno (1), cero o uno (0.. 1), o muchos (\*). Esta información se especifica en dos secundarios **final** elementos.

En tiempo de ejecución, las instancias de tipo de entidad en un extremo de una asociación pueden tener acceso a través de las propiedades de navegación o las claves externas (si se decide exponer las claves externas de las entidades). Con claves externas expuestas, la relación entre las entidades se administra con un **ReferentialConstraint** elemento (elemento secundario de la **asociación** elemento). Se recomienda que siempre exponer las claves externas para las relaciones en las entidades.

> [!NOTE]
> En muchos a muchos (\*:\*) no se puede agregar claves externas a las entidades. En un \*:\* relación, la información de asociación se administra con un objeto independiente.

Para obtener información acerca de los elementos CSDL (**ReferentialConstraint**, **asociación**, etc.) vea el [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Crear y eliminar las asociaciones

Crear una asociación con las actualizaciones de EF Designer en el contenido del archivo .edmx del modelo. Después de crear una asociación, debe crear las asignaciones para la asociación (descrito más adelante en este tema).

> [!NOTE]
> En esta sección se da por supuesto que ya ha agregado las entidades que desea crear una asociación entre el modelo.

### <a name="to-create-an-association"></a>Para crear una asociación

1.  Haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y seleccione **asociación...** .
2.  Rellene los valores para la asociación en el **Agregar asociación** cuadro de diálogo.

    ![Agregar asociación](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Opta por no agregar las propiedades de navegación o propiedades de clave externa a las entidades en los extremos de la asociación desactivando la ** propiedad de navegación ** y ** agregar propiedades de clave externa a la &lt;nombre de tipo de entidad&gt; entidad ** casillas de verificación. Si agrega solo una propiedad de navegación, la asociación se podrá recorrer en una única dirección. Si no agrega ninguna propiedad de navegación, deberá agregar propiedades de clave externa para poder tener acceso a las entidades situadas en los extremos de la asociación.
    
3.  Haga clic en **Aceptar**.

### <a name="to-delete-an-association"></a>Para eliminar una asociación

Para eliminar un asociación uno de los siguientes:

-   Haga clic en la asociación en el Diseñador de EF superficie y seleccione **eliminar**.

- OR:

-   Seleccione una o más asociaciones y presione la tecla Supr.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Incluir propiedades de clave externa en las entidades (restricciones referenciales)

Se recomienda que siempre exponer las claves externas para las relaciones en las entidades. Entity Framework usa una restricción referencial para identificar que una propiedad actúa como la clave externa de una relación.

Si ha activado el ***agregar propiedades de clave externa a la &lt;nombre de tipo de entidad&gt; entidad*** casilla de verificación al crear una relación, se agrega esta restricción referencial.

Cuando use EF Designer para agregar o editar una restricción referencial, EF Designer agrega o modifica un **ReferentialConstraint** elemento en el contenido CSDL del archivo .edmx.

-   Haga doble clic en la asociación que desee editar.
    El **restricción referencial** aparece el cuadro de diálogo.
-   Desde el **Principal** lista desplegable, seleccione la entidad de seguridad de la restricción referencial.
    Propiedades de clave de la entidad se agregan a la **clave de entidad** lista en el cuadro de diálogo.
-   Desde el **dependientes** lista desplegable, seleccione la entidad dependiente de la restricción referencial.
-   Para cada clave principal que tiene una clave dependiente, seleccione una clave dependiente correspondiente en las listas desplegables en el **clave dependiente** columna.

    ![Restricción de referencia](~/ef6/media/refconstraint.png)

-   Haga clic en **Aceptar**.

## <a name="create-and-edit-association-mappings"></a>Crear y editar asignaciones de asociación

Puede especificar cómo se asigna una asociación a la base de datos en el **detalles de Mapping** ventana de EF Designer.

> [!NOTE]
> Solo puede asignar los detalles de las asociaciones que no tiene una restricción referencial especificada. Si se especifica una restricción referencial, a continuación, se incluye una propiedad de clave externa en la entidad y puede usar los detalles de la asignación de la entidad para qué columna se asigna la clave externa para el control.

### <a name="create-an-association-mapping"></a>Crear una asignación de asociación

-   Haga clic en una asociación en la superficie de diseño y seleccione **asignación de tabla**.
    Esto muestra la asignación de asociación en el **detalles de Mapping** ventana.
-   Haga clic en **agregar una tabla o vista**.
    Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento. 
-   Seleccione la tabla a la que se asignará la asociación. 
    El **detalles de Mapping** ventana muestra los dos extremos de la asociación y las propiedades de clave para el tipo de entidad en cada **final**.
-   Para cada propiedad de clave, haga clic en el **columna** campo y seleccione la columna a la que se asignará la propiedad.

    ![Detalles de mapping 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Editar una asignación de asociación

-   Haga clic en una asociación en la superficie de diseño y seleccione **asignación de tabla**.
    Esto muestra la asignación de asociación en el **detalles de Mapping** ventana.
-   Haga clic en **se asigna a &lt;nombre de la tabla&gt;**.
    Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento. 
-   Seleccione la tabla a la que se asignará la asociación. 
    El **detalles de Mapping** ventana muestra los dos extremos de la asociación y las propiedades de clave para el tipo de entidad en cada extremo.
-   Para cada propiedad de clave, haga clic en el **columna** campo y seleccione la columna a la que se asignará la propiedad.

## <a name="edit-and-delete-navigation-properties"></a>Editar y eliminar propiedades de navegación

Propiedades de navegación son propiedades de acceso directo que se usan para localizar las entidades en los extremos de una asociación en un modelo. Las propiedades de navegación se pueden crear al definir una asociación entre dos tipos de entidad.

#### <a name="to-edit-navigation-properties"></a>Para editar las propiedades de navegación

-   Seleccione una propiedad de navegación en la superficie de EF Designer.
    Se muestra información acerca de la propiedad de navegación de Visual Studio **propiedades** ventana.
-   Cambiar los valores de propiedad en el **propiedades** ventana.

#### <a name="to-delete-navigation-properties"></a>Para eliminar las propiedades de navegación

-   Si las claves externas no se exponen en los tipos de entidad del modelo conceptual, la eliminación de una propiedad de navegación puede provocar que la asociación correspondiente solo se pueda recorrer en una dirección o incluso en ninguna.
-   Haga clic en una propiedad de navegación de EF Designer superficie y seleccione **eliminar**.
