---
title: Migración de EF6 a requisitos de validación EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565349"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a>Antes de migrar de EF6 a EF Core: Validar los requisitos de la aplicación

Antes de comenzar el proceso de portabilidad, es importante validar que EF Core cumple los requisitos de acceso a los datos de la aplicación.

## <a name="missing-features"></a>Características que faltan

Asegúrese de que EF Core tenga todas las características que necesita para usar en la aplicación. Vea [comparación de características](../features.md) para obtener una comparación detallada de cómo el conjunto de características en EF Core se compara con EF6. Si faltan las características necesarias, asegúrese de que puede compensar la falta de estas características antes de migrar a EF Core.

## <a name="behavior-changes"></a>Cambios de comportamiento

Se trata de una lista no exhaustiva de algunos cambios de comportamiento entre EF6 y EF Core. Es importante tener esto en cuenta como el puerto de la aplicación, ya que pueden cambiar la forma en que se comporta la aplicación, pero no se mostrarán como errores de compilación después de cambiar a EF Core.

### <a name="dbsetaddattach-and-graph-behavior"></a>DbSet. Add/Attach y comportamiento del grafo

En EF6, llamar `DbSet.Add()` a en una entidad da como resultado una búsqueda recursiva de todas las entidades a las que se hace referencia en sus propiedades de navegación. Las entidades que se encuentren y a las que no se haya realizado el seguimiento en el contexto, también se marcarán como agregadas. `DbSet.Attach()`se comporta de la misma forma, salvo que todas las entidades se marcan como sin cambios.

**EF Core realiza una búsqueda recursiva similar, pero con algunas reglas ligeramente diferentes.**

*  La entidad raíz siempre está en el estado solicitado (agregado para `DbSet.Add` y sin cambios para `DbSet.Attach`).

*  **Para las entidades que se encuentran durante la búsqueda recursiva de propiedades de navegación:**

    *  **Si la clave principal de la entidad se ha generado en el almacén**

        * Si la clave principal no se establece en un valor, el estado se establece en Added. El valor de clave principal se considera "no establecido" si se le asigna el valor predeterminado de CLR para el tipo de propiedad ( `0` por `int`ejemplo, `string`para, `null` para, etc.).

        * Si la clave principal se establece en un valor, el estado se establece en Unchanged.

    *  Si la clave principal no se genera en la base de datos, la entidad se coloca en el mismo estado que la raíz.

### <a name="code-first-database-initialization"></a>Inicialización de Code First base de datos

**EF6 tiene una cantidad significativa de magia que se realiza en torno a la selección de la conexión de base de datos y la inicialización de la base de datos. Algunas de estas reglas incluyen:**

* Si no se realiza ninguna configuración, EF6 seleccionará una base de datos en SQL Express o LocalDb.

* Si una cadena de conexión con el mismo nombre que el contexto está en el `App/Web.config` archivo de aplicaciones, se usará esta conexión.

* Si la base de datos no existe, se crea.

* Si no existe ninguna tabla del modelo en la base de datos, se agrega el esquema del modelo actual a la base de datos. Si se habilitan las migraciones, se usan para crear la base de datos.

* Si la base de datos existe y EF6 creó previamente el esquema, se comprueba la compatibilidad del esquema con el modelo actual. Se produce una excepción si el modelo ha cambiado desde que se creó el esquema.

**EF Core no realiza ninguna de estas instrucciones mágicas.**

* La conexión de base de datos debe configurarse explícitamente en el código.

* No se realiza ninguna inicialización. Debe usar `DbContext.Database.Migrate()` para aplicar las Migraciones (o `DbContext.Database.EnsureCreated()` y `EnsureDeleted()` para crear o eliminar la base de datos sin usar migraciones).

### <a name="code-first-table-naming-convention"></a>Code First Convención de nomenclatura de tabla

EF6 ejecuta el nombre de la clase de entidad a través de un servicio de pluralización para calcular el nombre de tabla predeterminado al que está asignada la entidad.

EF Core usa el nombre de la `DbSet` propiedad en la que se expone la entidad en el contexto derivado. Si la entidad no tiene una `DbSet` propiedad, se utiliza el nombre de clase.
