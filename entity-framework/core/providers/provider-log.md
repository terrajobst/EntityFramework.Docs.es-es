---
title: Registro de cambios en el impacto de proveedor - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 5da1043310e2858638c81a0654a9cab23e39c220
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250821"
---
# <a name="provider-impacting-changes"></a>Cambios en el impacto de proveedor

Esta página contiene vínculos para extraer las solicitudes realizadas en el repositorio de EF Core que puede exigir que los autores de otros proveedores de base de datos para reaccionar ante ellos. La intención es proporcionar un punto de partida para los autores de proveedores de base de datos de terceros existentes al actualizar su proveedor para una nueva versión.

Este registro estamos empezando con los cambios de 2.1 a 2.2. Antes de 2.1 se utilizaba el [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) y [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etiquetas en nuestros problemas y solicitudes de extracción.

## <a name="21-----22"></a>2.1---> 2.2

### <a name="test-only-changes"></a>Cambios sólo para pruebas

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Permitir delimitadores personalizable de SQL en las pruebas
  * Probar los cambios que permiten las comparaciones de punto flotante no estricta en BuiltInDataTypesTestBase
  * Cambios de prueba que permiten las pruebas de consulta pueden volver a usar con diferentes delimitadores SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Agregue DbFunction pruebas a las pruebas de la especificación relacional
  * Por ejemplo, que se pueden ejecutar estas pruebas en todos los proveedores de base de datos
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Limpieza de la prueba Async
  * Quitar `Wait` , las llamadas innecesarias async y cambiar el nombre de algunos métodos de prueba
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Unificar la infraestructura de registro de prueba
  * Agregar `CreateListLoggerFactory` y quitado algunos infraestructura de registro anteriores, lo que requiere con estas pruebas para reaccionar ante los proveedores
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Ejecutar más pruebas de consulta tanto de forma sincrónica y asincrónica
  * Los nombres de prueba y la factorización ha cambiado, lo que requerirá con estas pruebas para reaccionar ante los proveedores
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Cambiar el nombre durante la navegación en el modelo ComplexNavigations
  * Proveedores de uso de estas pruebas que deba reaccionar
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 : Devuelve el contexto para el grupo en lugar de eliminarlos en las pruebas funcionales
  * Este cambio incluye algunas pruebas de refactorización que quizá requiera proveedores reaccionar ante ellos


### <a name="test-and-product-code-changes"></a>Cambios de código de prueba y producto

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidar RelationalTypeMapping.Clone métodos
  * Se permiten cambios en el RelationalTypeMapping 2.1 para una simplificación de las clases derivadas. No creemos que esto era importantes en los proveedores, pero los proveedores pueden aprovechar las ventajas de este cambio en su tipo derivado asignar clases.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Consultas con nombre o etiquetadas
  * Agrega la infraestructura para el etiquetado de las consultas LINQ y tener esas etiquetas se muestran como comentarios en el código SQL. Esto puede requerir que los proveedores reaccionar ante la generación de SQL.
* https://github.com/aspnet/EntityFrameworkCore/pull/13115 -Compatibilidad con datos espaciales a través de interrupción
  * Permite que las asignaciones de tipos y miembros traductores registrarse fuera del proveedor
    * Los proveedores deben llamar a bases. FindMapping() en su implementación ITypeMappingSource para que funcione
  * Siga este patrón para agregar compatibilidad espacial en el proveedor que es coherente entre todos los proveedores.
* https://github.com/aspnet/EntityFrameworkCore/pull/13199 -Agregar depuración mejorada para la creación del proveedor de servicio
  * Permite DbContextOptionsExtensions implementar una nueva interfaz que puede ayudarle a entender por qué se recompile el proveedor de servicios interno
