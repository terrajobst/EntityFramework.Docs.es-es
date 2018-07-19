---
title: Migraciones de Code First con una base de datos existente - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
caps.latest.revision: 3
ms.openlocfilehash: 77e139a29bb4708b00fc6198a57780ce75197252
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122775"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Migraciones de Code First con una base de datos existente
> [!NOTE]
> **EF4.3 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 4.1. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

En este artículo se describe el uso de migraciones de Code First con una base de datos existente que no fue creado por Entity Framework.

> [!NOTE]
> En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos. Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="screencasts"></a>Presentaciones en pantalla

Si en su lugar, podría ver una presentación en pantalla que lea este artículo, los siguientes dos vídeos abarcan el mismo contenido que este artículo.

### <a name="video-one-migrations---under-the-hood"></a>Otra de vídeo: "Migraciones: aspectos técnicos"

[Esta presentación en pantalla](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) cubre cómo realiza un seguimiento de las migraciones y usa información sobre el modelo para detectar cambios en el modelo.

### <a name="video-two-migrations---existing-databases"></a>Dos de vídeo: "Migraciones: bases de datos existentes"

Creación de los conceptos descritos en el vídeo anterior, [esta presentación en pantalla](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) se explica cómo habilitar y usar las migraciones con una base de datos existente.

## <a name="step-1-create-a-model"></a>Paso 1: Crear un modelo

El primer paso será crear un modelo de Code First que tenga como destino de la base de datos existente. El [Code First para una base de datos](~/ef6/modeling/code-first/workflows/existing-database.md) tema proporciona instrucciones detalladas sobre cómo hacerlo.

>[!NOTE]
> Es importante que siga el resto de los pasos descritos en este tema antes de realizar cambios en el modelo que requieran cambios en el esquema de base de datos. Los pasos siguientes requieren que el modelo de estar sincronizado con el esquema de base de datos.

## <a name="step-2-enable-migrations"></a>Paso 2: Habilitar migraciones

El siguiente paso es habilitar migraciones. Puede hacerlo ejecutando el **Enable-Migrations** comando en la consola de administrador de paquetes.

Este comando se cree una carpeta en la solución llama a las migraciones y colocar una única clase dentro de él llamado configuración. La clase de configuración es donde se configuran las migraciones para su aplicación, puede encontrar más información al respecto en el [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) tema.

## <a name="step-3-add-an-initial-migration"></a>Paso 3: Agregar una migración inicial

Una vez que las migraciones se han creado y aplicado a la base de datos local que también puede aplicar estos cambia a otras bases de datos. Por ejemplo, la base de datos local puede ser una base de datos de prueba y es posible que deseas en última instancia también se aplican los cambios a una base de datos de producción o bases de datos de prueba de otros desarrolladores. Hay dos opciones para este paso y lo que debe elegir depende de si el esquema de otras bases de datos está vacío o no actualmente coincide con el esquema de la base de datos local.

-   **Opción uno: Usar un esquema existente como punto de partida.** Debe usar este enfoque cuando otras bases de datos que se aplicarán las migraciones a en el futuro tendrá el mismo esquema que tiene actualmente la base de datos local. Por ejemplo, se puede utilizar si la base de datos de prueba local actualmente coincide con la versión 1 de la base de datos de producción y posteriormente se aplicarán estas migraciones para actualizar la base de datos de producción a v2.
-   **Opción dos: Usar la base de datos vacía como punto de partida.** Debe usar este enfoque cuando otras bases de datos que se aplicarán las migraciones a en el futuro están vacíos (o no existe todavía). Por ejemplo, podría utilizar si a desarrollar su aplicación con una base de datos de prueba, pero sin usar migraciones y más tarde querrá crear una base de datos de producción desde el principio.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Opción uno: Usar un esquema existente como punto de partida

Migraciones de Code First utiliza una instantánea del modelo que se almacenan en la migración más reciente para detectar cambios en el modelo (puede encontrar información detallada acerca de esto en [migraciones de Code First en entornos de equipo](~/ef6/modeling/code-first/migrations/teams.md)). Puesto que vamos a suponer que las bases de datos ya tienen el esquema del modelo actual, se generará una migración (inefectiva) vacía que tiene el modelo actual como una instantánea.

1.  Ejecute el **InitialCreate Add-Migration – IgnoreChanges** comando en la consola de administrador de paquetes. Esto crea una migración vacía con el modelo actual como una instantánea.
2.  Ejecute el **Update-Database** comando en la consola de administrador de paquetes. Esto aplicará la migración InitialCreate a la base de datos. Puesto que la migración real no contiene ningún cambio, simplemente agregará una fila a la \_ \_MigrationsHistory tabla que indica que se ha aplicado esta migración.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Opción dos: Usar la base de datos vacía como punto de partida

En este escenario es necesario que las migraciones para poder crear la base de datos completa desde cero, incluidas las tablas que ya están presentes en nuestra base de datos local. Vamos a generar una migración InitialCreate que incluye la lógica para crear el esquema existente. A continuación, haremos nuestra base de datos existente parezca ya se ha aplicado esta migración.

1.  Ejecute el **Add-Migration InitialCreate** comando en la consola de administrador de paquetes. Esto crea una migración para crear el esquema existente.
2.  Comente todo el código en el método de copia de seguridad de la migración recién creado. Esto nos permitirá 'Aplicar' la migración a la base de datos local sin intentar volver a crear todas las tablas etc. que ya existen.
3.  Ejecute el **Update-Database** comando en la consola de administrador de paquetes. Esto aplicará la migración InitialCreate a la base de datos. Puesto que la migración real no contiene los cambios (ya hemos comentado temporalmente marcándolas), simplemente agregará una fila a la \_ \_MigrationsHistory tabla que indica que se ha aplicado esta migración.
4.  Quitar los comentarios del código en el método de copia de seguridad. Esto significa que, cuando se aplica esta migración a futuras bases de datos, se creará el esquema que ya existía en la base de datos local por migraciones.

## <a name="things-to-be-aware-of"></a>Cosas a tener en cuenta

Hay algunas cosas que debe tener en cuenta al usar las migraciones en una base de datos existente.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>/ Calcula de forma predeterminada los nombres no pueden coincidir con el esquema existente

Cuando aplica la técnica scaffolding las migraciones, migraciones especifica explícitamente los nombres de columnas y tablas. Sin embargo, hay otros objetos de base de datos que las migraciones se calcula un nombre predeterminado para al aplicar las migraciones. Esto incluye los índices y restricciones de clave externa. Cuando el destino es un esquema existente, estos nombres calculados pueden no coincidir con lo que realmente existe en la base de datos.

Estos son algunos ejemplos de cuando deba tener en cuenta esto:

**Si ha utilizado ' una opción: usar un esquema existente como punto de partida ' del paso 3:**

-   Si los cambios futuros en el modelo necesitan cambiar o quitar uno de los objetos de base de datos que es un nombre diferente, deberá modificar la migración con scaffold para especificar el nombre correcto. Las API de migraciones tienen un parámetro de nombre opcional que le permite hacer esto.
    Por ejemplo, el esquema existente puede tener una tabla de entrada con una columna de clave externa de BlogId que tiene un índice llamado IndexFk\_BlogId. Sin embargo, de forma predeterminada las migraciones esperaría este índice se denomine IX\_BlogId. Si realiza un cambio en el modelo que da como resultado la eliminación de este índice, deberá modificar la técnica scaffolding llamada DropIndex para especificar el IndexFk\_BlogId nombre.

**Si ha utilizado ' opción dos: usar base de datos vacío como punto de partida ' del paso 3:**

-   Puede producir un error al intentar ejecutar el método abajo de la migración inicial (es decir, es, a una base de datos vacía) en la base de datos local porque las migraciones intentará quitar índices y restricciones de clave externa con los nombres incorrectos. Esto solo afectará su base de datos local desde otras bases de datos se creará desde cero mediante el método de copia de seguridad de la migración inicial.
    Si desea cambiar la base de datos local existente en un estado vacío es más fácil hacerlo manualmente, ya sea quitando la base de datos o quitar todas las tablas. Después de este cambio inicial que se volverá a todos los objetos de base de datos con los nombres predeterminados, por lo que este problema no presentará propio nuevo.
-   Si los cambios futuros en el modelo necesitan cambiar o quitar uno de los objetos de base de datos que es un nombre diferente, esto no funcionará en la base de datos local existente: puesto que los nombres no coinciden con los valores predeterminados. Sin embargo, funcionará en bases de datos que se crearon 'desde el principio', ya que habrá utilizaban los nombres predeterminados elegidos por migraciones.
    Se podría realizar estas modificaciones manualmente en la base de datos local existente, o considere la opción de migraciones de volver a crear la base de datos desde cero: igual que en otras máquinas.
-   Bases de datos creadas mediante el método de copia de seguridad de la migración inicial pueden diferir ligeramente de la base de datos local desde los nombres predeterminados calculados para los índices y restricciones de clave externa que se usará. Es posible que también terminará con índices adicionales como las migraciones creará los índices en columnas de clave externa de forma predeterminada, esto puede no haber sido el caso de la base de datos local original.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>No todos los objetos de base de datos se representan en el modelo

No se controlarán los objetos de base de datos que no forman parte del modelo mediante migraciones. Esto puede incluir vistas, procedimientos almacenados, los permisos, las tablas que no forman parte de su modelo, los índices adicionales, etc.

Estos son algunos ejemplos de cuando deba tener en cuenta esto:

-   Independientemente de la opción que eligió en el 'Paso 3,' si los cambios futuros en el modelo necesitan cambiar o quitar estos objetos adicionales que para realizar estos cambios no sabrá migraciones. Por ejemplo, si quita una columna que tiene un índice adicional en ella, las migraciones no sabrá para quitar el índice. Deberá agregarlo manualmente a la migración con scaffolding.
-   Si ha utilizado ' opción dos: usar base de datos vacío como punto de partida ', estos objetos adicionales no se creará el método de copia de seguridad de la migración inicial.
    Puede modificar arriba y hacia abajo de los métodos que se encargue de estos objetos adicionales si lo desea. Para los objetos que no se admiten de forma nativa en la API de migraciones – como vistas: puede usar el [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) método para ejecutar SQL sin procesar para crear y colocar de ellos.
