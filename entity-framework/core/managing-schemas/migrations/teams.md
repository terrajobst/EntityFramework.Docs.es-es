---
title: Migraciones en entornos de equipo - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228385"
---
<a name="migrations-in-team-environments"></a>Migraciones en entornos de equipo
===============================
Cuando se trabaja con las migraciones en entornos de equipo, debe poner atención en el archivo de instantánea del modelo. Este archivo puede indicarle si la migración de su compañero combina sin problemas con el suyo o si tiene que resolver un conflicto, vuelva a crear la migración antes de compartirlo.

<a name="merging"></a>Combinación
-------
Cuando se mezclan las migraciones de sus compañeros de equipo, puede aparecer conflictos en el archivo de instantánea del modelo. Si ambos cambios están relacionados, la combinación es trivial y las dos migraciones pueden coexistir. Por ejemplo, es posible que obtenga un conflicto de combinación en la configuración de tipo de entidad customer que tiene este aspecto:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Puesto que ambas propiedades deben existir en el modelo final, complete la combinación agregando ambas propiedades. En muchos casos, el sistema de control de versiones puede combinar estos cambios automáticamente.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

En estos casos, la migración y migración de su compañero son independientes entre sí. Dado que cualquiera de ellos podría aplicarse primero, no es necesario realizar ningún cambio adicional en la migración antes de compartirlos con su equipo.

<a name="resolving-conflicts"></a>Resolución de conflictos
-------------------
A veces se produzca un conflicto es true cuando se mezclan los modelos de instantánea. Por ejemplo, usted y su compañero de equipo es posible que cada uno de ellos ha cambiado el nombre la misma propiedad.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Si se produce este tipo de conflicto, volver a crear la migración para resolverlo. Siga estos pasos:

1. Anular la mezcla y la reversión al directorio de trabajo antes de la combinación
2. Quitar la migración (pero mantener los cambios de modelo)
3. Combinar los cambios de su compañero en el directorio de trabajo
4. Volver a agregar a la migración

Una vez hecho esto, se pueden aplicar las dos migraciones en el orden correcto. La migración se aplica en primer lugar, cambiar el nombre de la columna a *Alias*, a partir de entonces la migración cambia su nombre a *Username*.

La migración puede compartirse con seguridad con el resto del equipo.
