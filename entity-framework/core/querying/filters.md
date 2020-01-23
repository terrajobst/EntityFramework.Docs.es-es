---
title: 'Filtros de consulta global: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f4ee9b77411290249e763f9cb8492eea61803e91
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124397"
---
# <a name="global-query-filters"></a>Filtros de consulta global

> [!NOTE]
> Esta característica se incluyó por primera vez en EF Core 2.0.

Los filtros de consulta global son predicados de consulta LINQ (una expresión booleana que habitualmente se pasa al operador de consulta LINQ *Where*) aplicados a los tipos de entidad del modelo de metadatos (habitualmente en *OnModelCreating*). Estos filtros se aplican automáticamente a las consultas LINQ que implican a esos tipos de entidad, incluidos aquellos a los que se hace referencia de forma indirecta, por ejemplo mediante el uso de Include o de referencias de propiedad de navegación directas. Algunas aplicaciones comunes de esta característica son:

* **Eliminación temporal**: un tipo de entidad define una propiedad *IsDeleted*.
* **Servicios multiinquilino**: un tipo de entidad define una propiedad *TenantId*.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo usar los filtros de consulta global para implementar los comportamientos de consulta de multiinquilino y de eliminación temporal en un simple modelo de creación de blogs.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) de este artículo en GitHub.

En primer lugar, defina las entidades:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Tenga en cuenta la declaración de un campo _tenantId_ en la entidad _Blog_. Se usará para asociar cada instancia de blog con un inquilino específico. También hay definida una propiedad _IsDeleted_ en el tipo de entidad _Post_. Se usa para llevar un seguimiento de si una instancia _Post_ se eliminó de manera temporal. Es decir, la instancia se marca como eliminada sin quitar físicamente los datos subyacentes.

A continuación, configure los filtros de consulta en _OnModelCreating_ con la API `HasQueryFilter`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Las expresiones de predicado pasadas a las llamadas de _HasQueryFilter_ ahora se aplicarán automáticamente a cualquier consulta LINQ para esos tipos.

> [!TIP]
> Tenga en cuenta el uso de un campo en el nivel de instancia de DbContext: `_tenantId` se usa para establecer el inquilino actual. Los filtros de nivel de modelo usan el valor de la instancia de contexto correcta (es decir, la instancia que está ejecutando la consulta).

> [!NOTE]
> Actualmente no es posible definir varios filtros de consulta en la misma entidad. Solo se aplicará el último. Sin embargo, puede definir un único filtro con varias condiciones mediante el operador lógico _AND_ ([`&&` en C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).

## <a name="disabling-filters"></a>Deshabilitación de filtros

Los filtros se pueden deshabilitar para consultas LINQ individuales mediante el operador `IgnoreQueryFilters()`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Limitaciones

Los filtros de consulta global tienen las limitaciones siguientes:

* Solo se pueden definir filtros para el tipo de entidad raíz de una jerarquía de herencia.
