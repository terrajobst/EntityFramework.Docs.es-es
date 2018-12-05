---
title: Propagación de datos - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857434"
---
# <a name="data-seeding"></a>Propagación de datos

Propagación de datos es el proceso de rellenar una base de datos con un conjunto inicial de datos.

Hay varias maneras de que esto puede realizarse en EF Core:
* Datos de inicialización del modelo
* Personalización de la migración manual
* Lógica de inicialización personalizado

## <a name="model-seed-data"></a>Datos de inicialización del modelo

> [!NOTE]
> Esta característica es nueva en EF Core 2.1.

A diferencia de en EF6 en EF Core, la propagación de datos puede asociarse con un tipo de entidad como parte de la configuración del modelo. A continuación, EF Core [migraciones](xref:core/managing-schemas/migrations/index) puede calcular automáticamente qué insertar, actualizar o eliminar la necesidad de las operaciones que se aplicará al actualizar la base de datos a una nueva versión del modelo.

> [!NOTE]
> Las migraciones solo considera que los cambios en el modelo al determinar qué operación debe realizarse para obtener los datos de inicialización en el estado deseado. Por lo tanto es posible que se perderán los cambios en los datos que se realiza fuera de las migraciones o se producirá un error.

Por ejemplo, se configurarán los datos de inicialización para un `Blog` en `OnModelCreating`:

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse:

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Si el tipo de entidad tiene propiedades en una clase anónima puede utilizarse para proporcionar los valores de estado de sombra:

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

Propiedad de entidad tipos pueden propagarse de forma similar:

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) para obtener más contexto.

Una vez que los datos se ha agregado al modelo, [migraciones](xref:core/managing-schemas/migrations/index) debe usarse para aplicar los cambios.

> [!TIP]
> Si tiene que aplicar las migraciones como parte de una implementación automatizada, puede [crear un script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) que puede obtener una vista previa antes de la ejecución.

Como alternativa, puede usar `context.Database.EnsureCreated()` para crear una nueva base de datos que contiene los datos de inicialización, por ejemplo, para una base de datos de prueba o cuando se usa el proveedor en memoria o cualquier base de datos sin relación. Observe que si ya existe la base de datos, `EnsureCreated()` ninguno, se actualizará los datos de esquema ni el valor de inicialización de la base de datos. Bases de datos relacionales no debería llamar a `EnsureCreated()` si tiene previsto usar migraciones.

Las migraciones se administra este tipo de datos de inicialización y la secuencia de comandos para actualizar los datos que ya está en la base de datos se debe generar sin necesidad de conectarse a la base de datos. Esto impone algunas restricciones:
* El valor de clave principal debe especificarse incluso si se genera normalmente por la base de datos. Se utilizará para detectar cambios en los datos entre las migraciones.
* Previamente los datos inicializados se quitará si se cambia la clave principal de ninguna manera.

Por lo tanto, esta característica resulta especialmente útil para datos estáticos que no se espera que cambie fuera de las migraciones y no dependen de cualquier otra cosa en la base de datos, por ejemplo los códigos postales.

Si su escenario incluye alguno de los siguientes, se recomienda usar lógica de inicialización personalizadas descrita en la última sección:
* Datos temporales para las pruebas
* Datos que dependen de estado de la base de datos
* Datos que tengan valores de clave que va a generar la base de datos, incluidas las entidades que utilizan las claves alternativas como la identidad
* Datos que requiere la transformación personalizada (que no está controlado por [conversiones de valor](xref:core/modeling/value-conversions)), por ejemplo, algunos algoritmos de hash de contraseña
* Datos que requieren las llamadas a API externa, como la creación de usuarios y roles de ASP.NET Core Identity

## <a name="manual-migration-customization"></a>Personalización de la migración manual

Cuando se agrega una migración los cambios a los datos especificados con `HasData` se transforman en llamadas a `InsertData()`, `UpdateData()`, y `DeleteData()`. Una manera de solucionar algunas de las limitaciones de `HasData` es agregar manualmente estas llamadas o [operaciones personalizadas](xref:core/managing-schemas/migrations/operations) para la migración en su lugar.

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a>Lógica de inicialización personalizado

Una manera sencilla y eficaz para realizar la propagación de datos es usar [ `DbContext.SaveChanges()` ](xref:core/saving/index) antes de la aplicación principal lógica comienza la ejecución.

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> El código de propagación no debe formar parte de la ejecución de aplicación normal ya que esto puede causar problemas de simultaneidad cuando varias instancias se ejecutan y también podría requerir que las aplicaciones deben tener permiso para modificar el esquema de base de datos.

Dependiendo de las restricciones de la implementación se puede ejecutar el código de inicialización de maneras diferentes:
* Ejecutar la aplicación de inicialización localmente
* Implementación de la aplicación de inicialización con la aplicación principal, invocar la rutina de inicialización y deshabilitación o eliminación de la aplicación de la inicialización.

Normalmente, esto puede automatizarse mediante [perfiles de publicación](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).
