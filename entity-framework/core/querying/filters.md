---
title: Filtros de consulta global - Core EF
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>Filtros de consulta global

Los filtros de consulta global son predicados de consulta LINQ (normalmente se pasa una expresión booleana a LINQ *donde* operador de consulta) aplicado a tipos de entidad en el modelo de metadatos (normalmente en *OnModelCreating*). Estos filtros se aplican automáticamente a las consultas LINQ que afectan a los tipos de entidad, incluidas las referencias de propiedad de navegación directa o indirectamente, como hace referencia mediante el uso de inclusión de tipos de entidad. Algunas aplicaciones comunes de esta característica son:

* **Eliminar temporalmente** -un tipo de entidad define una *IsDeleted* propiedad.
* **La arquitectura multiempresa** -un tipo de entidad define una *TenantId* propiedad.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo usar filtros de consulta Global para implementar comportamientos de la consulta de eliminación de software y la arquitectura multiempresa en un modelo de creación de blogs simple.

> [!TIP]
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) en GitHub.

En primer lugar, defina las entidades:

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

Tenga en cuenta la declaración de un __tenantId_ campo el _Blog_ entidad. Esto se utilizará para asociar cada instancia de Blog específicos del inquilino. También es un _IsDeleted_ propiedad en el _Post_ tipo de entidad. Se utiliza esta opción para realizar un seguimiento de si una _Post_ instancia ha estado "eliminado". Es decir La instancia se marca como eliminado reemplazar quitar físicamente los datos subyacentes.

A continuación, configure los filtros de consulta de _OnModelCreating_ mediante el ```HasQueryFilter``` API.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

Las expresiones de predicado que se pasan a la _HasQueryFilter_ llamadas ahora se aplicarán automáticamente a las consultas LINQ para esos tipos.

> [!TIP]
> Tenga en cuenta el uso de un campo de nivel de instancia de DbContext: ```_tenantId``` usa para establecer el inquilino actual. Filtros de nivel de modelo usará el valor de la instancia de contexto correcto. Es decir La instancia que se está ejecutando la consulta.

## <a name="disabling-filters"></a>Deshabilitar filtros

Los filtros pueden deshabilitarse para consultas LINQ individuales mediante el uso de la ```IgnoreQueryFilters()``` operador.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitaciones

Los filtros de consulta global tienen las siguientes limitaciones:

* Los filtros no pueden contener referencias a las propiedades de navegación.
* Solo se pueden definir filtros para la raíz del tipo de entidad de una jerarquía de herencia.
