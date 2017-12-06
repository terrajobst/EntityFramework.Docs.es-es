---
title: "Establecer valores explícitos para propiedades generadas - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Establecer valores explícitos para propiedades generadas

Una propiedad generada es una propiedad cuyo valor se genera (ya sea por EF o la base de datos) cuando se agrega o actualiza la entidad. Vea [genera propiedades](../modeling/generated-properties.md) para obtener más información.

Puede haber situaciones en la que desea establecer un valor explícito para una propiedad generada, en lugar de tener uno generado.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) en GitHub.

## <a name="the-model"></a>El modelo

El modelo utilizado en este artículo contiene una sola `Employee` entidad.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Guardar un valor explícito durante agregar

El `Employee.EmploymentStarted` propiedad está configurada para tener valores generados por la base de datos nuevas entidades (con un valor predeterminado).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

El código siguiente inserta a dos empleados en la base de datos.
* Para la primera, se asigna ningún valor a `Employee.EmploymentStarted` propiedad, por lo que permanece establecido en el valor predeterminado CLR para `DateTime`.
* Para el segundo, hemos configurado un valor explícito de `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Resultado muestra que la base de datos genera un valor para el primer empleado y se usó el valor explícito para la segunda.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Valores explícitos en columnas de identidad de SQL Server

Por convención la `Employee.EmployeeId` propiedad es un almacén genera `IDENTITY` columna.

Para la mayoría de los casos, el enfoque se muestra arriba funcionará para las propiedades de clave. Sin embargo, para insertar valores explícitos en un servidor SQL Server `IDENTITY` columna, debe habilitar manualmente `IDENTITY_INSERT` antes de llamar a `SaveChanges()`.

> [!NOTE]  
> Tenemos un [solicitud de característica](https://github.com/aspnet/EntityFramework/issues/703) en nuestro trabajo pendiente a hacerlo automáticamente el proveedor de SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Resultado muestra que se guardaron los identificadores proporcionados en la base de datos.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Establecer un valor explícito durante la actualización

El `Employee.LastPayRaise` propiedad está configurada para tener valores generados por la base de datos durante las actualizaciones.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> De forma predeterminada, el núcleo de EF se iniciará una excepción si intenta guardar un valor explícito para una propiedad que está configurado para que se generen durante la actualización. Para evitar esto, se necesitan lista desplegable a la API de metadatos de nivel inferior y el `AfterSaveBehavior` (tal y como se muestra arriba).

> [!NOTE]  
> **Cambios en EF Core 2.0:** en versiones anteriores se controla el comportamiento de guardar posterior mediante el `IsReadOnlyAfterSave` marca. Esta marca se ha obsoleta y se ha reemplazado por `AfterSaveBehavior`.

También hay un desencadenador en la base de datos para generar valores para la `LastPayRaise` columna durante `UPDATE` operaciones.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

El código siguiente aumenta el salario de los dos empleados de la base de datos.
* Para la primera, se asigna ningún valor a `Employee.LastPayRaise` propiedad, por lo que permanece establecido en null.
* Para el segundo, hemos configurado un valor explícito de una semana hace (generar back desde la opción de pago).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Resultado muestra que la base de datos genera un valor para el primer empleado y se usó el valor explícito para la segunda.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
