---
title: 'Portabilidad de EF6 a EF Core: validar los requisitos'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949119"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de portabilidad de EF6 a EF Core: validar los requisitos de la aplicación

Antes de iniciar el proceso de conversión es importante validar que EF Core cumple los requisitos de acceso de datos para la aplicación.

## <a name="missing-features"></a>Características que faltan

Asegúrese de que EF Core tiene todas las características que necesita para usar en la aplicación. Consulte [comparación de características](../features.md) para ver una comparación detallada de cómo se comparan el conjunto de características de EF Core a EF6. Si faltan las características necesarias, asegúrese de que puede compensar la falta de estas características antes de migrar a EF Core.

## <a name="behavior-changes"></a>Cambios de comportamiento

Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core. Es importante recordar esto en mente como el puerto de la aplicación tal como puede cambiar la forma en que se comporta la aplicación, pero no se mostrará como errores de compilación después de intercambiar a EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>Comportamiento DbSet.Add/Attach y graph

En EF6, una llamada a `DbSet.Add()` en los resultados de una entidad en una búsqueda recursiva para todas las entidades que se hace referencia en sus propiedades de navegación. Todas las entidades que se encuentran y ya no contarán según el contexto, son también se marca como agregado. `DbSet.Attach()` se comporta igual, salvo que todas las entidades se marcan como sin cambios.

**EF Core realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente distintas.**

*  La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y sin modificaciones para `DbSet.Attach`).

*  **Para las entidades que se encuentran durante la búsqueda recursiva de las propiedades de navegación:**

    *  **Si la clave principal de la entidad es generado de almacén**

        * Si la clave principal no se establece en un valor, el estado se establece a agregado. El valor de clave principal se considera "sin establecer" si se asigna el valor predeterminado CLR para el tipo de propiedad (por ejemplo, `0` para `int`, `null` para `string`, etcetera.).

        * Si la clave principal se establece en un valor, el estado se establece en sin cambios.

    *  Si la clave principal no es una base de datos generada, la entidad se coloca en el mismo estado que la raíz.

### <a name="code-first-database-initialization"></a>Código de inicialización primera base de datos

**EF6 tiene una cantidad significativa de magia que realiza en torno a la selección de la conexión de base de datos e inicializar la base de datos. Algunas de estas reglas incluyen:**

* Si no se realiza ninguna configuración, EF6 seleccionará una base de datos SQL Express o LocalDb.

* Si es una cadena de conexión con el mismo nombre que el contexto en las aplicaciones `App/Web.config` archivo, se usará esta conexión.

* Si la base de datos no existe, se crea.

* Si no existe ninguna de las tablas del modelo en la base de datos, se agrega el esquema para el modelo actual a la base de datos. Si están habilitadas las migraciones, se utilizan para crear la base de datos.

* Si existe la base de datos y EF6 había creado anteriormente en el esquema, se comprueba el esquema para la compatibilidad con el modelo actual. Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.

**EF Core no lleva a cabo cualquiera de esta instrucción mágica.**

* La conexión de base de datos debe configurarse explícitamente en el código.

* No se realiza ninguna inicialización. Debe usar `DbContext.Database.Migrate()` para aplicar las migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).

### <a name="code-first-table-naming-convention"></a>Convención de nomenclatura de primera tabla de código

EF6 se ejecuta el nombre de clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado asignado a la entidad.

EF Core usa el nombre de la `DbSet` propiedad que la entidad está expuesta en el contexto derivado. Si la entidad no tiene un `DbSet` se utiliza la propiedad y, a continuación, el nombre de clase.
