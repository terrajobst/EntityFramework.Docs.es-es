---
title: 'Relaciones: EF Designer: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415080"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="904bc-102">Relaciones: EF Designer</span><span class="sxs-lookup"><span data-stu-id="904bc-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="904bc-103">En esta página se proporciona información sobre cómo configurar las relaciones en el modelo mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="904bc-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="904bc-104">Para obtener información general sobre las relaciones en EF y cómo obtener acceso a los datos y manipularlos mediante relaciones, vea [relaciones & propiedades de navegación](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="904bc-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="904bc-105">Las asociaciones definen las relaciones entre los tipos de entidad de un modelo.</span><span class="sxs-lookup"><span data-stu-id="904bc-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="904bc-106">En este tema se muestra cómo asignar asociaciones con el Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="904bc-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="904bc-107">En la imagen siguiente se muestran las ventanas principales que se usan al trabajar con el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="904bc-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="904bc-109">Al crear el modelo conceptual, es posible que aparezcan advertencias sobre entidades y asociaciones no asignadas en la Lista de errores.</span><span class="sxs-lookup"><span data-stu-id="904bc-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="904bc-110">Puede omitir estas advertencias porque, después de elegir generar la base de datos a partir del modelo, los errores desaparecerán.</span><span class="sxs-lookup"><span data-stu-id="904bc-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="904bc-111">Información general sobre asociaciones</span><span class="sxs-lookup"><span data-stu-id="904bc-111">Associations Overview</span></span>

<span data-ttu-id="904bc-112">Al diseñar el modelo mediante el diseñador de EF, un archivo. edmx representa el modelo.</span><span class="sxs-lookup"><span data-stu-id="904bc-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="904bc-113">En el archivo. edmx, un elemento **Association** define una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="904bc-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="904bc-114">Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad.</span><span class="sxs-lookup"><span data-stu-id="904bc-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="904bc-115">La multiplicidad de un extremo de asociación puede tener un valor de uno (1), cero o uno (0. 1) o varios (\*).</span><span class="sxs-lookup"><span data-stu-id="904bc-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="904bc-116">Esta información se especifica en dos elementos **End** secundarios.</span><span class="sxs-lookup"><span data-stu-id="904bc-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="904bc-117">En tiempo de ejecución, se puede tener acceso a las instancias de tipo de entidad en un extremo de una asociación a través de las propiedades de navegación o las claves externas (si elige exponer las claves externas en las entidades).</span><span class="sxs-lookup"><span data-stu-id="904bc-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="904bc-118">Con las claves externas expuestas, la relación entre las entidades se administra con un elemento **ReferentialConstraint** (un elemento secundario del elemento **Association** ).</span><span class="sxs-lookup"><span data-stu-id="904bc-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="904bc-119">Se recomienda exponer siempre las claves externas para las relaciones de las entidades.</span><span class="sxs-lookup"><span data-stu-id="904bc-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="904bc-120">En varios a varios (\*:\*) no se pueden agregar claves externas a las entidades.</span><span class="sxs-lookup"><span data-stu-id="904bc-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="904bc-121">En una relación de \*:\*, la información de asociación se administra con un objeto independiente.</span><span class="sxs-lookup"><span data-stu-id="904bc-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="904bc-122">Para obtener información sobre los elementos CSDL (**ReferentialConstraint**, **Association**, etc.), vea la [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="904bc-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="904bc-123">Crear y eliminar asociaciones</span><span class="sxs-lookup"><span data-stu-id="904bc-123">Create and Delete Associations</span></span>

<span data-ttu-id="904bc-124">La creación de una asociación con el diseñador de EF actualiza el contenido del modelo del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="904bc-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="904bc-125">Después de crear una asociación, debe crear las asignaciones para la Asociación (que se describen más adelante en este tema).</span><span class="sxs-lookup"><span data-stu-id="904bc-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="904bc-126">En esta sección se supone que ya ha agregado las entidades con las que desea crear una asociación entre el modelo.</span><span class="sxs-lookup"><span data-stu-id="904bc-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="904bc-127">Para crear una asociación</span><span class="sxs-lookup"><span data-stu-id="904bc-127">To create an association</span></span>

1.  <span data-ttu-id="904bc-128">Haga clic con el botón secundario en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y seleccione **asociación.** .</span><span class="sxs-lookup"><span data-stu-id="904bc-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="904bc-129">Rellene la configuración de la asociación en el cuadro de diálogo **Agregar Asociación** .</span><span class="sxs-lookup"><span data-stu-id="904bc-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Agregar Asociación](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="904bc-131">Puede optar por no agregar propiedades de navegación ni propiedades de clave externa a las entidades en los extremos de la asociación si desactiva la **propiedad de navegación **y **agrega propiedades de clave externa al &lt;nombre de tipo de entidad&gt; **las casillas de entidad.</span><span class="sxs-lookup"><span data-stu-id="904bc-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="904bc-132">Si agrega solo una propiedad de navegación, la asociación se podrá recorrer en una única dirección.</span><span class="sxs-lookup"><span data-stu-id="904bc-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="904bc-133">Si no agrega ninguna propiedad de navegación, deberá agregar propiedades de clave externa para poder tener acceso a las entidades situadas en los extremos de la asociación.</span><span class="sxs-lookup"><span data-stu-id="904bc-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="904bc-134">Haga clic en  **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="904bc-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="904bc-135">Para eliminar una asociación</span><span class="sxs-lookup"><span data-stu-id="904bc-135">To delete an association</span></span>

<span data-ttu-id="904bc-136">Para eliminar una asociación, realice una de las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="904bc-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="904bc-137">Haga clic con el botón derecho en la asociación en la superficie del diseñador EF y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="904bc-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="904bc-138">O BIEN</span><span class="sxs-lookup"><span data-stu-id="904bc-138">OR -</span></span>

-   <span data-ttu-id="904bc-139">Seleccione una o más asociaciones y presione la tecla Supr.</span><span class="sxs-lookup"><span data-stu-id="904bc-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="904bc-140">Incluir propiedades de clave externa en las entidades (restricciones referenciales)</span><span class="sxs-lookup"><span data-stu-id="904bc-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="904bc-141">Se recomienda exponer siempre las claves externas para las relaciones de las entidades.</span><span class="sxs-lookup"><span data-stu-id="904bc-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="904bc-142">Entity Framework usa una restricción referencial para identificar que una propiedad actúa como clave externa de una relación.</span><span class="sxs-lookup"><span data-stu-id="904bc-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="904bc-143">Si activó la casilla ***Agregar propiedades de clave externa al &lt;nombre de tipo de entidad&gt; entidad*** al crear una relación, esta restricción referencial se agregó automáticamente.</span><span class="sxs-lookup"><span data-stu-id="904bc-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="904bc-144">Cuando se usa EF Designer para agregar o editar una restricción referencial, el diseñador de EF agrega o modifica un elemento de **ReferentialConstraint** en el contenido CSDL del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="904bc-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="904bc-145">Haga doble clic en la asociación que desee editar.</span><span class="sxs-lookup"><span data-stu-id="904bc-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="904bc-146">Aparecerá el cuadro de diálogo de **restricción referencial** .</span><span class="sxs-lookup"><span data-stu-id="904bc-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="904bc-147">En la lista desplegable **principal** , seleccione la entidad de entidad de seguridad en la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="904bc-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="904bc-148">Las propiedades de clave de la entidad se agregan a la lista de de la **clave principal** en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="904bc-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="904bc-149">En la lista desplegable  **dependiente** , seleccione la entidad dependiente en la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="904bc-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="904bc-150">Para cada clave principal que tenga una clave dependiente, seleccione una clave dependiente correspondiente en las listas desplegables de la columna de **clave dependiente** .</span><span class="sxs-lookup"><span data-stu-id="904bc-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Restricción de referencia](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="904bc-152">Haga clic en  **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="904bc-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="904bc-153">Crear y editar asignaciones de asociación</span><span class="sxs-lookup"><span data-stu-id="904bc-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="904bc-154">Puede especificar cómo se asigna una asociación a la base de datos en la ventana detalles de la **asignación** del diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="904bc-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="904bc-155">Solo puede asignar los detalles de las asociaciones que no tienen una restricción referencial especificada.</span><span class="sxs-lookup"><span data-stu-id="904bc-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="904bc-156">Si se especifica una restricción referencial, se incluye una propiedad de clave externa en la entidad y se pueden usar los detalles de asignación de la entidad para controlar a qué columna se asigna la clave externa.</span><span class="sxs-lookup"><span data-stu-id="904bc-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="904bc-157">Crear una asignación de asociación</span><span class="sxs-lookup"><span data-stu-id="904bc-157">Create an association mapping</span></span>

-   <span data-ttu-id="904bc-158">Haga clic con el botón secundario en una asociación en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="904bc-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="904bc-159">Esto muestra la asignación de asociación en la ventana detalles de la **asignación** .</span><span class="sxs-lookup"><span data-stu-id="904bc-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="904bc-160">Haga clic en **Agregar una tabla o vista**.</span><span class="sxs-lookup"><span data-stu-id="904bc-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="904bc-161">Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="904bc-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="904bc-162">Seleccione la tabla a la que se asignará la asociación.</span><span class="sxs-lookup"><span data-stu-id="904bc-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="904bc-163">La ventana detalles de la **asignación** muestra los extremos de la asociación y las propiedades clave del tipo de entidad en cada **extremo**.</span><span class="sxs-lookup"><span data-stu-id="904bc-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="904bc-164">Para cada propiedad de clave, haga clic en el campo **columna** y seleccione la columna a la que se asignará la propiedad.</span><span class="sxs-lookup"><span data-stu-id="904bc-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Detalles de la asignación 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="904bc-166">Editar una asignación de asociación</span><span class="sxs-lookup"><span data-stu-id="904bc-166">Edit an association mapping</span></span>

-   <span data-ttu-id="904bc-167">Haga clic con el botón secundario en una asociación en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="904bc-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="904bc-168">Esto muestra la asignación de asociación en la ventana detalles de la **asignación** .</span><span class="sxs-lookup"><span data-stu-id="904bc-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="904bc-169">Haga clic en **asignaciones para &lt;nombre de tabla&gt;** .</span><span class="sxs-lookup"><span data-stu-id="904bc-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="904bc-170">Aparece una lista desplegable que incluye todas las tablas del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="904bc-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="904bc-171">Seleccione la tabla a la que se asignará la asociación.</span><span class="sxs-lookup"><span data-stu-id="904bc-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="904bc-172">La ventana detalles de la **asignación** muestra los extremos de la asociación y las propiedades clave del tipo de entidad en cada extremo.</span><span class="sxs-lookup"><span data-stu-id="904bc-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="904bc-173">Para cada propiedad de clave, haga clic en el campo **columna** y seleccione la columna a la que se asignará la propiedad.</span><span class="sxs-lookup"><span data-stu-id="904bc-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="904bc-174">Editar y eliminar propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="904bc-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="904bc-175">Las propiedades de navegación son propiedades de acceso directo que se usan para buscar las entidades en los extremos de una asociación en un modelo.</span><span class="sxs-lookup"><span data-stu-id="904bc-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="904bc-176">Las propiedades de navegación se pueden crear al definir una asociación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="904bc-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="904bc-177">Para editar las propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="904bc-177">To edit navigation properties</span></span>

-   <span data-ttu-id="904bc-178">Seleccione una propiedad de navegación en la superficie del diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="904bc-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="904bc-179">La información sobre la propiedad de navegación se muestra en la ventana de  **propiedades** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="904bc-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="904bc-180">Cambie la configuración de propiedades en la ventana **propiedades** .</span><span class="sxs-lookup"><span data-stu-id="904bc-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="904bc-181">Para eliminar propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="904bc-181">To delete navigation properties</span></span>

-   <span data-ttu-id="904bc-182">Si las claves externas no se exponen en los tipos de entidad del modelo conceptual, la eliminación de una propiedad de navegación puede provocar que la asociación correspondiente solo se pueda recorrer en una dirección o incluso en ninguna.</span><span class="sxs-lookup"><span data-stu-id="904bc-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="904bc-183">Haga clic con el botón derecho en una propiedad de navegación en la superficie del diseñador EF y seleccione **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="904bc-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
