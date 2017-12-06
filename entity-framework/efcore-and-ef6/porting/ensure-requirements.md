---
title: "Migrar desde EF6 EF básica: validar los requisitos"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de migrar desde EF6 Core EF: validar los requisitos de su aplicación

Antes de empezar el proceso de migración es importante validar que EF Core cumple los requisitos de acceso de datos para la aplicación.

## <a name="missing-features"></a>Características que faltan

Asegúrese de que EF Core incluye todas las características que se debe usar en la aplicación. Vea [comparación de características](../features.md) para obtener una comparación detallada de cómo se comparan el conjunto de características de núcleo de EF con EF6. Si faltan las características necesarias, asegúrese de que es posible compensar la falta de estas características antes de migrar a EF Core.

## <a name="behavior-changes"></a>Cambios de comportamiento

Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core. Es importante tener en cuenta como el puerto de la aplicación tal y como puede cambiar la manera en que se comporta de la aplicación, pero no aparecerán como errores de compilación después de intercambiar a EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamiento de DbSet.Add/Attach y gráfico

En EF6, una llamada a `DbSet.Add()` en el resultado de una entidad en una búsqueda recursiva para todas las entidades que se hace referencia en sus propiedades de navegación. Todas las entidades que se encuentran y no tienen seguimiento por el contexto, son también se marca como agregado. `DbSet.Attach()`se comporta igual, salvo que todas las entidades se marcan como sin cambios.

**Núcleo EF realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente diferentes.**

*  La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y ha modificado para `DbSet.Attach`).

*  **Para las entidades que se encuentran durante la búsqueda recursiva de propiedades de navegación:**

    *  **Si la clave principal de la entidad es almacén generado**

        * Si la clave principal no se establece en un valor, el estado se establece a agregado. El valor de clave principal se considera "no configurada" si se asigna el valor predeterminado CLR para el tipo de propiedad (es decir, `0` para `int`, `null` para `string`, etcetera.).

        * Si la clave principal se establece en un valor, el estado se establece en sin cambios.

    *  Si la clave principal no está generado de la base de datos, la entidad se coloca en el mismo estado que la raíz.

### <a name="code-first-database-initialization"></a>Código de la primera inicialización de la base de datos

**EF6 tiene una cantidad significativa de magia realiza alrededor de la selección de la conexión de base de datos e inicializar la base de datos. Algunas de estas reglas incluyen:**

* Si no se realiza ninguna configuración, EF6 seleccionará una base de datos SQL Express o LocalDb.

* Si es una cadena de conexión con el mismo nombre que el contexto de las aplicaciones `App/Web.config` archivo, se usará esta conexión.

* Si la base de datos no existe, se crea.

* Si no existe ninguna de las tablas del modelo en la base de datos, el esquema para el modelo actual se agrega a la base de datos. Si las migraciones están habilitadas, se utilizan para crear la base de datos.

* Si existe la base de datos y EF6 había creado anteriormente el esquema, se comprueba el esquema para la compatibilidad con el modelo actual. Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.

**Núcleo EF no realiza ninguna este mágico.**

* La conexión de base de datos debe configurarse explícitamente en el código.

* No se realiza ninguna inicialización. Debe usar `DbContext.Database.Migrate()` para aplicar las migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).

### <a name="code-first-table-naming-convention"></a>Convención de nomenclatura de primera tabla de código

EF6 ejecuta el nombre de clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado que está asignada la entidad a.

EF Core utiliza el nombre de la `DbSet` propiedad que la entidad está expuesta en el contexto derivado. Si la entidad no tiene un `DbSet` se utiliza la propiedad y, a continuación, el nombre de clase.
