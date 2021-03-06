---
title: Prueba de componentes mediante EF Core - EF Core
description: Diferentes métodos para probar aplicaciones que usan EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634249"
---
# <a name="testing-code-that-uses-ef-core"></a>Pruebas de código que usa EF Core

Para probar el código que accede a una base de datos, es necesario:
* Ejecutar consultas y actualizaciones en el mismo sistema de base de datos que se usa en producción.
* Ejecutar consultas y actualizaciones en algún otro sistema de base de datos más fácil de administrar.
* Usar dobles de prueba o algún otro mecanismo para evitar por completo el uso de una base de datos.

En este documento se describen las ventajas e inconvenientes de cada una de estas opciones y se muestra cómo usar EF Core con cada método.  

## <a name="all-database-providers-are-not-equal"></a>Todos los proveedores de bases de datos no son iguales

Es muy importante entender que EF Core no está diseñado para extraer cada aspecto del sistema de base de datos subyacente.
EF Core es un conjunto común de patrones y conceptos que se pueden usar con cualquier sistema de base de datos.
Así, los proveedores de bases de datos de EF Core basan el comportamiento y la funcionalidad específicos de base de datos en este marco común.
Esto permite a cada sistema de base de datos hacer lo que mejor se le da, a la vez que mantiene la homogeneidad, si fuera necesario, con otros sistemas de base de datos. 

Básicamente, esto significa que al cambiar de proveedor de base de datos, cambia el comportamiento de EF Core y no se puede esperar que la aplicación funcione correctamente a menos que tenga en cuenta de forma explícita todas las diferencias de comportamiento.
Dicho esto, en muchos casos esto funciona, ya que hay un alto grado de homogeneidad entre bases de datos relacionales.
Esto es bueno y malo.
Es bueno porque el cambio entre bases de datos puede ser relativamente fácil.
Es malo porque puede dar una falsa sensación de seguridad si la aplicación no se prueba por completo en el nuevo sistema de base de datos.  

## <a name="approach-1-production-database-system"></a>Enfoque 1: Sistema de base de datos de producción

Como se ha explicado en la sección anterior, la única manera de asegurarse de que se está probando lo que se ejecuta en producción es usar el mismo sistema de base de datos.
Por ejemplo, si la aplicación implementada usa SQL Azure, las pruebas también deben realizarse en SQL Azure.

Pero que cada desarrollador ejecute pruebas en SQL Azure mientras trabaja activamente en el código es lento y costoso.
Esto muestra la principal contrapartida de estos métodos: ¿cuándo resulta adecuado desviarse del sistema de base de datos de producción para mejorar la eficacia de las pruebas?

Afortunadamente, en este caso, la respuesta es bastante fácil: use la instancia local de SQL Server para las pruebas de desarrollador.
SQL Azure y SQL Server son muy similares, por lo que realizar las pruebas en SQL Server suele ser una contrapartida razonable.
Dicho esto, sigue siendo aconsejable ejecutar las pruebas en SQL Azure antes de pasar a producción.
 
### <a name="localdb"></a>LocalDb 

Todos los principales sistemas de base de datos tienen alguna forma de "edición para desarrolladores" para las pruebas locales.
SQL Server también tiene una característica denominada [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).
La principal ventaja de LocalDb es que inicia la instancia de base de datos a petición.
Esto evita que haya un servicio de base de datos ejecutándose en el equipo aunque no se estén ejecutando pruebas.

Pero LocalDb también plantea problemas:
* No admite todo lo que [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).
* No está disponible en Linux.
* Puede producir un retraso en la primera serie de pruebas cuando se inicia el servicio.

Personalmente, nunca me ha parecido un problema que haya un servicio de base de datos ejecutándose en el equipo de desarrollo y, en general, recomendaría usar Developer Edition.
Pero puede ser adecuado para algunas personas, especialmente en equipos de desarrollo menos potentes.  

## <a name="approach-2-sqlite"></a>Enfoque 2: SQLite

EF Core prueba el proveedor de SQL Server principalmente mediante su ejecución en una instancia local de SQL Server.
Estas pruebas ejecutan decenas de miles de consultas en un par de minutos en un equipo rápido.
Esto muestra que el uso del sistema de base de datos real puede ser una solución eficiente.
Es un mito que el uso de una base de datos más ligera sea la única manera de ejecutar pruebas rápidamente.

Dicho esto, ¿qué ocurre si, por cualquier motivo, no se pueden ejecutar pruebas en algo cercano al sistema de base de datos de producción?
La siguiente mejor opción es usar algo con funcionalidad similar.
Esto suele significar otra base de datos relacional, para lo que [SQLite](https://sqlite.org/index.html) es la opción obvia.

SQLite es una buena opción porque:
* Se ejecuta en proceso con la aplicación y, por tanto, tiene poca sobrecarga.
* Usa archivos simples creados automáticamente para bases de datos, por lo que no requiere administración de bases de datos.
* Tiene un modo en memoria que evita incluso la creación de archivos.

Pero recuerde que:
* SQLite inevitablemente no admite todo lo que el sistema de base de datos de producción.
* SQLite se comporta de forma diferente al sistema de base de datos de producción para algunas consultas.

Por lo tanto, si usa SQLite para algunas pruebas, asegúrese de probar también en el sistema de base de datos real.

Vea [Pruebas con SQLite](xref:core/miscellaneous/testing/sqlite) para obtener instrucciones específicas de EF Core. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Método 3: Base de datos en memoria de EF Core

EF Core incluye una base de datos en memoria que se usa para las pruebas internas del propio EF Core.
Esta base de datos en general **no es adecuada como sustituto para probar las aplicaciones que usan EF Core**. De manera específica:
* No es una base de datos relacional
* No admite transacciones
* No está optimizada para el rendimiento

Nada de esto es muy importante a la hora de probar elementos internos de EF Core, ya que se usa específicamente donde la base de datos es irrelevante para la prueba.
Por otro lado, estos aspectos tienden a ser muy importantes al probar una aplicación que usa EF Core.

## <a name="unit-testing"></a>Pruebas unitarias

Imagine que va a probar una parte de la lógica de negocio que pueda necesitar usar algunos datos de una base de datos, pero que no supone probar propiamente las interacciones de la base de datos.
Una opción es usar un [doble de prueba](https://en.wikipedia.org/wiki/Test_double) como simulacro o imitación.

Los dobles de prueba se usan para las pruebas internas de EF Core.
Pero nunca se intentan simular DbContext o IQueryable.
Hacerlo es difícil, engorroso y delicado.
**No lo haga.**

En su lugar, se usa la base de datos en memoria siempre que se realizan pruebas unitarias de algo que usa DbContext.
En este caso, el uso de la base de datos en memoria es adecuado porque la prueba no depende del comportamiento de la base de datos.
Pero no lo haga para probar consultas o actualizaciones reales de la base de datos.   

Vea [Pruebas con InMemory](xref:core/miscellaneous/testing/in-memory) para obtener instrucciones específicas de EF Core sobre el uso de la base de datos en memoria para las pruebas unitarias.
