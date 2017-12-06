---
title: Migraciones en entornos de equipo - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migraciones en entornos de equipo
===============================
Cuando trabaje con migraciones en entornos de equipo, preste especial atención en el archivo de instantánea de modelo. Este archivo puede indicarle si migración de su compañero limpiamente combina con la suya de si tiene que resolver un conflicto mediante la creación de volver a la migración antes de compartirlo.

<a name="merging"></a>Combinación
-------
Cuando se mezclan las migraciones de sus compañeros de equipo, puede obtener conflictos en el archivo de instantánea de modelo. Si no están relacionadas con los cambios, la combinación es trivial y pueden coexistir dos migraciones. Por ejemplo, puede obtener un conflicto de combinación en la configuración del tipo de entidad customer este aspecto:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Puesto que ambas propiedades deben existir en el modelo final, completar la combinación mediante la adición de ambas propiedades. En muchos casos, el sistema de control de versiones puede combinar dichos cambios automáticamente.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

En estos casos, la migración y la migración de su compañero son independientes entre sí. Puesto que ninguna de ellas puede aplicarse en primer lugar, no es necesario realizar ningún cambio adicional a la migración antes de compartirla con su equipo.

<a name="resolving-conflicts"></a>Resolución de conflictos
-------------------
A veces se produzca un conflicto true cuando se mezclan los modelos de instantánea. Por ejemplo, usted y su compañero pueden cada uno de ellos ha cambiado el nombre la misma propiedad.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Si se produce este tipo de conflicto, volver a crear la migración para resolverlo. Siga estos pasos:

1. Anular la mezcla y la reversión en su directorio de trabajo antes de la combinación
2. Quitar la migración (pero conservar los cambios de modelo)
3. Combinar los cambios de su compañero en el directorio de trabajo
4. Vuelva a agregar la migración

Una vez hecho esto, pueden aplicarse las dos migraciones en el orden correcto. Su migración se aplica en primer lugar, cambiar el nombre de la columna a *Alias*, a partir de ahí la migración cambia el nombre a *nombre de usuario*.

La migración puede ser comparte de forma segura con el resto del equipo.
