---
title: 'EF6 relaciones - EF Designer:'
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994653"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="eafc7-102">Relaciones - EF Designer</span><span class="sxs-lookup"><span data-stu-id="eafc7-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="eafc7-103">Esta página proporciona información sobre cómo configurar las relaciones en el modelo con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="eafc7-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="eafc7-104">Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="eafc7-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="eafc7-105">Las asociaciones definen las relaciones entre tipos de entidad en un modelo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="eafc7-106">En este tema se muestra cómo asignar las asociaciones con Entity Framework Designer (Diseñador de EF).</span><span class="sxs-lookup"><span data-stu-id="eafc7-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="eafc7-107">La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="eafc7-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="eafc7-109">Cuando se compila el modelo conceptual, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores.</span><span class="sxs-lookup"><span data-stu-id="eafc7-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="eafc7-110">Puede omitir estas advertencias porque después de elegir generar la base de datos del modelo, los errores desaparecerán.</span><span class="sxs-lookup"><span data-stu-id="eafc7-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="eafc7-111">Información general de las asociaciones</span><span class="sxs-lookup"><span data-stu-id="eafc7-111">Associations Overview</span></span>

<span data-ttu-id="eafc7-112">Al diseñar el modelo usando el Diseñador de EF, un archivo .edmx representa el modelo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="eafc7-113">En el archivo .edmx, un **asociación** elemento define una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="eafc7-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="eafc7-114">Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad.</span><span class="sxs-lookup"><span data-stu-id="eafc7-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="eafc7-115">La multiplicidad de un extremo de asociación puede tener un valor uno (1), cero o uno (0.. 1), o muchos (\*).</span><span class="sxs-lookup"><span data-stu-id="eafc7-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="eafc7-116">Esta información se especifica en dos secundarios **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="eafc7-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="eafc7-117">En tiempo de ejecución, las instancias de tipo de entidad en un extremo de una asociación pueden tener acceso a través de las propiedades de navegación o las claves externas (si se decide exponer las claves externas de las entidades).</span><span class="sxs-lookup"><span data-stu-id="eafc7-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="eafc7-118">Con claves externas expuestas, la relación entre las entidades se administra con un **ReferentialConstraint** elemento (elemento secundario de la **asociación** elemento).</span><span class="sxs-lookup"><span data-stu-id="eafc7-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="eafc7-119">Se recomienda que siempre exponer las claves externas para las relaciones en las entidades.</span><span class="sxs-lookup"><span data-stu-id="eafc7-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="eafc7-120">En muchos a muchos (\*:\*) no se puede agregar claves externas a las entidades.</span><span class="sxs-lookup"><span data-stu-id="eafc7-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="eafc7-121">En un \*:\* relación, la información de asociación se administra con un objeto independiente.</span><span class="sxs-lookup"><span data-stu-id="eafc7-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="eafc7-122">Para obtener información acerca de los elementos CSDL (**ReferentialConstraint**, **asociación**, etc.) vea el [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="eafc7-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="eafc7-123">Crear y eliminar las asociaciones</span><span class="sxs-lookup"><span data-stu-id="eafc7-123">Create and Delete Associations</span></span>

<span data-ttu-id="eafc7-124">Crear una asociación con las actualizaciones de EF Designer en el contenido del archivo .edmx del modelo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="eafc7-125">Después de crear una asociación, debe crear las asignaciones para la asociación (descrito más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="eafc7-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="eafc7-126">En esta sección se da por supuesto que ya ha agregado las entidades que desea crear una asociación entre el modelo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="eafc7-127">Para crear una asociación</span><span class="sxs-lookup"><span data-stu-id="eafc7-127">To create an association</span></span>

1.  <span data-ttu-id="eafc7-128">Haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y seleccione **asociación...** .</span><span class="sxs-lookup"><span data-stu-id="eafc7-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="eafc7-129">Rellene los valores para la asociación en el **Agregar asociación** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="eafc7-131">Opta por no agregar las propiedades de navegación o propiedades de clave externa a las entidades en los extremos de la asociación desactivando la ** propiedad de navegación ** y ** agregar propiedades de clave externa a la &lt;nombre de tipo de entidad&gt; entidad ** casillas de verificación.</span><span class="sxs-lookup"><span data-stu-id="eafc7-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="eafc7-132">Si agrega solo una propiedad de navegación, la asociación se podrá recorrer en una única dirección.</span><span class="sxs-lookup"><span data-stu-id="eafc7-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="eafc7-133">Si no agrega ninguna propiedad de navegación, deberá agregar propiedades de clave externa para poder tener acceso a las entidades situadas en los extremos de la asociación.</span><span class="sxs-lookup"><span data-stu-id="eafc7-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="eafc7-134">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="eafc7-135">Para eliminar una asociación</span><span class="sxs-lookup"><span data-stu-id="eafc7-135">To delete an association</span></span>

<span data-ttu-id="eafc7-136">Para eliminar un asociación uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="eafc7-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="eafc7-137">Haga clic en la asociación en el Diseñador de EF superficie y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="eafc7-138">OR:</span><span class="sxs-lookup"><span data-stu-id="eafc7-138">OR -</span></span>

-   <span data-ttu-id="eafc7-139">Seleccione una o más asociaciones y presione la tecla Supr.</span><span class="sxs-lookup"><span data-stu-id="eafc7-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="eafc7-140">Incluir propiedades de clave externa en las entidades (restricciones referenciales)</span><span class="sxs-lookup"><span data-stu-id="eafc7-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="eafc7-141">Se recomienda que siempre exponer las claves externas para las relaciones en las entidades.</span><span class="sxs-lookup"><span data-stu-id="eafc7-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="eafc7-142">Entity Framework usa una restricción referencial para identificar que una propiedad actúa como la clave externa de una relación.</span><span class="sxs-lookup"><span data-stu-id="eafc7-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="eafc7-143">Si ha activado el ***agregar propiedades de clave externa a la &lt;nombre de tipo de entidad&gt; entidad*** casilla de verificación al crear una relación, se agrega esta restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="eafc7-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="eafc7-144">Cuando use EF Designer para agregar o editar una restricción referencial, EF Designer agrega o modifica un **ReferentialConstraint** elemento en el contenido CSDL del archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="eafc7-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="eafc7-145">Haga doble clic en la asociación que desee editar.</span><span class="sxs-lookup"><span data-stu-id="eafc7-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="eafc7-146">El **restricción referencial** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="eafc7-147">Desde el **Principal** lista desplegable, seleccione la entidad de seguridad de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="eafc7-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="eafc7-148">Propiedades de clave de la entidad se agregan a la **clave de entidad** lista en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="eafc7-149">Desde el **dependientes** lista desplegable, seleccione la entidad dependiente de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="eafc7-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="eafc7-150">Para cada clave principal que tiene una clave dependiente, seleccione una clave dependiente correspondiente en las listas desplegables en el **clave dependiente** columna.</span><span class="sxs-lookup"><span data-stu-id="eafc7-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="eafc7-152">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="eafc7-153">Crear y editar asignaciones de asociación</span><span class="sxs-lookup"><span data-stu-id="eafc7-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="eafc7-154">Puede especificar cómo se asigna una asociación a la base de datos en el **detalles de Mapping** ventana de EF Designer.</span><span class="sxs-lookup"><span data-stu-id="eafc7-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="eafc7-155">Solo puede asignar los detalles de las asociaciones que no tiene una restricción referencial especificada.</span><span class="sxs-lookup"><span data-stu-id="eafc7-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="eafc7-156">Si se especifica una restricción referencial, a continuación, se incluye una propiedad de clave externa en la entidad y puede usar los detalles de la asignación de la entidad para qué columna se asigna la clave externa para el control.</span><span class="sxs-lookup"><span data-stu-id="eafc7-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="eafc7-157">Crear una asignación de asociación</span><span class="sxs-lookup"><span data-stu-id="eafc7-157">Create an association mapping</span></span>

-   <span data-ttu-id="eafc7-158">Haga clic en una asociación en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="eafc7-159">Esto muestra la asignación de asociación en el **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="eafc7-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="eafc7-160">Haga clic en **agregar una tabla o vista**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="eafc7-161">Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento. </span><span class="sxs-lookup"><span data-stu-id="eafc7-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="eafc7-162">Seleccione la tabla a la que se asignará la asociación. </span><span class="sxs-lookup"><span data-stu-id="eafc7-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="eafc7-163">El **detalles de Mapping** ventana muestra los dos extremos de la asociación y las propiedades de clave para el tipo de entidad en cada **final**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="eafc7-164">Para cada propiedad de clave, haga clic en el **columna** campo y seleccione la columna a la que se asignará la propiedad.</span><span class="sxs-lookup"><span data-stu-id="eafc7-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="eafc7-166">Editar una asignación de asociación</span><span class="sxs-lookup"><span data-stu-id="eafc7-166">Edit an association mapping</span></span>

-   <span data-ttu-id="eafc7-167">Haga clic en una asociación en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="eafc7-168">Esto muestra la asignación de asociación en el **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="eafc7-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="eafc7-169">Haga clic en **se asigna a &lt;nombre de la tabla&gt;**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="eafc7-170">Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento. </span><span class="sxs-lookup"><span data-stu-id="eafc7-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="eafc7-171">Seleccione la tabla a la que se asignará la asociación. </span><span class="sxs-lookup"><span data-stu-id="eafc7-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="eafc7-172">El **detalles de Mapping** ventana muestra los dos extremos de la asociación y las propiedades de clave para el tipo de entidad en cada extremo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="eafc7-173">Para cada propiedad de clave, haga clic en el **columna** campo y seleccione la columna a la que se asignará la propiedad.</span><span class="sxs-lookup"><span data-stu-id="eafc7-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="eafc7-174">Editar y eliminar propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="eafc7-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="eafc7-175">Propiedades de navegación son propiedades de acceso directo que se usan para localizar las entidades en los extremos de una asociación en un modelo.</span><span class="sxs-lookup"><span data-stu-id="eafc7-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="eafc7-176">Las propiedades de navegación se pueden crear al definir una asociación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="eafc7-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="eafc7-177">Para editar las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="eafc7-177">To edit navigation properties</span></span>

-   <span data-ttu-id="eafc7-178">Seleccione una propiedad de navegación en la superficie de EF Designer.</span><span class="sxs-lookup"><span data-stu-id="eafc7-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="eafc7-179">Se muestra información acerca de la propiedad de navegación de Visual Studio **propiedades** ventana.</span><span class="sxs-lookup"><span data-stu-id="eafc7-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="eafc7-180">Cambiar los valores de propiedad en el **propiedades** ventana.</span><span class="sxs-lookup"><span data-stu-id="eafc7-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="eafc7-181">Para eliminar las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="eafc7-181">To delete navigation properties</span></span>

-   <span data-ttu-id="eafc7-182">Si las claves externas no se exponen en los tipos de entidad del modelo conceptual, la eliminación de una propiedad de navegación puede provocar que la asociación correspondiente solo se pueda recorrer en una dirección o incluso en ninguna.</span><span class="sxs-lookup"><span data-stu-id="eafc7-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="eafc7-183">Haga clic en una propiedad de navegación de EF Designer superficie y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="eafc7-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
