---
title: Migraciones de Code First en entornos de equipo - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
caps.latest.revision: 3
ms.openlocfilehash: 6a29fe3ab3aa6b9a51bea54706490f143e860a48
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122815"
---
# <a name="code-first-migrations-in-team-environments"></a>Migraciones de Code First en entornos de equipo
> [!NOTE]
> En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos. Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Tómese un café, debe leer todo el artículo

Los problemas en entornos de equipo son principalmente en torno a combinar las migraciones cuando dos programadores han generado las migraciones en su base de código local. Aunque los pasos para solucionar este son bastante simples, requieren que tenga un conocimiento sólido de funcionamiento de las migraciones. Por favor, simplemente se no ir directamente al final: dedicar tiempo a leer el artículo completo para asegurarse de que es correctas.

## <a name="some-general-guidelines"></a>Algunas instrucciones generales

Antes de profundizar en cómo administrar las migraciones de combinación generadas por varios desarrolladores, estas son algunas directrices generales para el éxito.

### <a name="each-team-member-should-have-a-local-development-database"></a>Cada miembro del equipo debe tener una base de datos de desarrollo local

Las migraciones usa el  **\_ \_MigrationsHistory** tabla para almacenar qué migraciones se aplicaron a la base de datos. Si tiene varios desarrolladores generar migraciones diferentes al intentar establecer como destino la misma base de datos (y, por tanto, comparten un  **\_ \_MigrationsHistory** tabla) las migraciones se va a obtener muy confundida.

Por supuesto, si tiene los miembros del equipo que no están generando las migraciones, no hay ningún problema en la necesidad de compartir una base de datos central de desarrollo.

### <a name="avoid-automatic-migrations"></a>Evitar las migraciones automáticas

La conclusión es que las migraciones automáticas inicialmente se muestren correctamente en entornos de equipo, pero en realidad simplemente no funcionan. Si desea saber por qué, siga leyendo: si no es así, a continuación, puede ir a la sección siguiente.

Las migraciones automáticas le permite tener el esquema de base de datos actualizado para que coincida con el modelo actual sin necesidad de generar archivos de código (migraciones basadas en código). Las migraciones automáticas podría funcionar muy bien en un entorno de equipo si había usado jamás solo y nunca genera las migraciones basadas en código. El problema es que las migraciones automáticas están limitadas y no controlan un número de operaciones, propiedad o columna cambia el nombre, mover datos a otra tabla, etcetera. Para administrar estos escenarios terminará generando las migraciones basadas en código (y el código con scaffolding de edición) que se mezclan entre los cambios que se controlan mediante las migraciones automáticas. Esto hace casi en imposible para combinar los cambios cuando dos de los desarrolladores protegen las migraciones.

## <a name="screencasts"></a>Presentaciones en pantalla

Si en su lugar, podría ver una presentación en pantalla que lea este artículo, los siguientes dos vídeos abarcan el mismo contenido que este artículo.

### <a name="video-one-migrations---under-the-hood"></a>Otra de vídeo: "Migraciones: aspectos técnicos"

[Esta presentación en pantalla](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) cubre cómo realiza un seguimiento de las migraciones y usa información sobre el modelo para detectar cambios en el modelo.

### <a name="video-two-migrations---team-environments"></a>Dos de vídeo: "Migraciones: entornos de equipo"

Creación de los conceptos descritos en el vídeo anterior, [esta presentación en pantalla](http://channel9.msdn.com/blogs/ef/migrations-team-environments) se tratan los problemas que surgen en un entorno de equipo y cómo solucionarlos.

## <a name="understanding-how-migrations-works"></a>Comprender el funcionamiento de las migraciones

La clave para usar correctamente las migraciones en un entorno de equipo básico comprender cómo realiza un seguimiento de las migraciones y usa información sobre el modelo para detectar cambios en el modelo.

### <a name="the-first-migration"></a>La primera migración

Cuando se agrega la primera migración al proyecto, ejecute algo parecido a **Add-Migration primer** en la consola de administrador de paquetes. A continuación, se ilustran los pasos de alto niveles que realiza este comando.

![FirstMigration](~/ef6/media/firstmigration.png)

El modelo actual se calcula desde el código (1). Los objetos de base de datos necesarios, a continuación, se calculan las diferencias de modelo (2): puesto que es la primera migración del modelo difieren solo usa un modelo vacío para la comparación. Los cambios necesarios se pasan al generador de código para compilar el código de migración necesaria (3) que, a continuación, se agrega a la solución de Visual Studio (4).

Además del código de migración real que se almacena en el archivo de código principal, las migraciones también genera algunos archivos adicionales de código subyacente. Estos archivos son metadatos que se usan por migraciones y no son algo que debe editar. Uno de estos archivos es un archivo de recursos (.resx) que contiene una instantánea del modelo en el momento en que se generó la migración. Verá cómo se utiliza en el paso siguiente.

En este momento probablemente se ejecutaría **Update-Database** para aplicar los cambios a la base de datos y, a continuación, vaya sobre la implementación de otras áreas de la aplicación.

### <a name="subsequent-migrations"></a>Migraciones posteriores

Más adelante, vuelva y realizar algunos cambios en el modelo: en nuestro ejemplo vamos a agregar un **Url** propiedad **Blog**. A continuación, podría emitir un comando como **Add-Migration AddUrl** para aplicar scaffolding a una migración para la base de datos correspondiente de aplicar los cambios. A continuación, se ilustran los pasos de alto niveles que realiza este comando.

![SecondMigration](~/ef6/media/secondmigration.png)

Igual que la última vez, el modelo actual se calcula a partir de código (1). Sin embargo, esta vez hay las migraciones existentes para que el modelo anterior se recupera de la migración más reciente (2). Estos dos modelos son comparar para buscar los cambios de la base de datos necesarios (3) y, a continuación, finalice el proceso como antes.

Este mismo proceso se usa para las migraciones adicionales que agregue al proyecto.

### <a name="why-bother-with-the-model-snapshot"></a>¿Por qué molestarse con la instantánea del modelo?

Quizás se pregunte por qué EF molesta con la instantánea del modelo: ¿por qué no solo vistazo a la base de datos. Si es así, siga leyendo. Si no le interesa, a continuación, puede omitir esta sección.

Hay una serie de motivos que EF mantiene alrededor de la instantánea del modelo:

-   Permite a desviarse del modelo EF de la base de datos. Estos cambios pueden realizarse directamente en la base de datos, o puede cambiar el código con scaffolding en las migraciones para realizar los cambios. Estos son algunos ejemplos de esto en la práctica:
    -   Desea agregar un insertadas y actualizadas para la columna a una o varias de las tablas pero no desea incluir estas columnas en el modelo de EF. Si las migraciones examinado la base de datos intentaría continuamente quitar estas columnas, cada vez que se ha aplicado scaffolding a una migración. Con la instantánea del modelo, EF detectará solo los cambios al modelo de confianza.
    -   Desea cambiar el cuerpo de un procedimiento almacenado usado para las actualizaciones debe para incluir algún registro. Si las migraciones mirado este procedimiento almacenado desde la base de datos podría pruebe y restablecer la definición que EF espera continuamente. Mediante el uso de la instantánea del modelo, EF aplicará solo la técnica scaffolding código para modificar el procedimiento almacenado cuando se cambia la forma del procedimiento en el modelo de EF.
    -   Estos mismos principios se aplican a la adición de índices adicionales, incluidas tablas adicionales en la base de datos, asignación de EF a una vista de base de datos que se encuentra a través de una tabla, etcetera.
-   El modelo de EF contiene algo más que la forma de la base de datos. Tener todo el modelo permite que las migraciones ver información acerca de las propiedades y clases en el modelo y cómo se asignan a las columnas y tablas. Esta información permite las migraciones a ser más inteligente en el código que aplica la técnica scaffolding. Por ejemplo, si cambia el nombre de la columna que se asigna una propiedad para las migraciones puede detectar el cambio de nombre por ver que es la misma propiedad: algo que no se puede realizar si solo tiene el esquema de base de datos. 

## <a name="what-causes-issues-in-team-environments"></a>¿Qué provoca problemas en entornos de equipo

El flujo de trabajo se trata en la anterior funciona muy bien de sección cuando haya un único desarrollador que trabaja en una aplicación. También funciona bien en un entorno de equipo si es la única persona que realizar cambios en el modelo. En este escenario puede realizar cambios en el modelo, genere las migraciones y enviarlas a su control de código fuente. Pueden sincronizar los cambios y ejecutar otros desarrolladores **Update-Database** que aplicar los cambios de esquema.

Iniciar problemas surgen cuando tiene varios desarrolladores realizar cambios en el modelo de EF y enviándolos al control de código fuente al mismo tiempo. Carece de EF es una manera de primera clase para combinar las migraciones locales con las migraciones que otro programador ha sometido a control de código fuente desde la última sincronización.

## <a name="an-example-of-a-merge-conflict"></a>Un ejemplo de un conflicto de combinación

Primero echemos un vistazo a un ejemplo concreto de este tipo de conflicto de fusión mediante combinación. Seguiremos en el ejemplo que analizamos anteriormente. Como una partida punto vamos a suponer que los cambios de la sección anterior se han protegido por el desarrollador original. Realizaremos un seguimiento de dos desarrolladores como hacen los cambios al código base.

Realizaremos un seguimiento el modelo de EF y las migraciones a través de una serie de cambios. Para un punto de partida, a los desarrolladores han sincronizado con el repositorio de control de código fuente, como se muestra en el gráfico siguiente.

![StartingPoint](~/ef6/media/startingpoint.png)

Desarrollador \#1 y desarrollador \#2 ahora realiza algunos cambios en el modelo de EF en su local código base. Desarrollador \#1 agrega un **clasificación** propiedad **Blog** – y genera un **AddRating** migración para aplicar los cambios a la base de datos. Desarrollador \#2 agrega un **lectores** propiedad **Blog** – y genera el correspondiente **AddReaders** migración. A los desarrolladores ejecutan **Update-Database**, para aplicar los cambios a sus bases de datos locales y, a continuación, continuar desarrollando la aplicación.

> [!NOTE]
> Las migraciones van precedidas de una marca de tiempo, por lo que nuestro gráfico que representa la migración AddReaders Developer \#2 viene después de la migración AddRating Developer \#1. Si developer \#1 o \#2 no generó el facilita la primera migración ninguna diferencia con los problemas de trabajar en un equipo o en el proceso de combinación que veremos en la sección siguiente.

![LocalChanges](~/ef6/media/localchanges.png)

Es un día de suerte para desarrollador \#1 cuando se produzcan al enviar sus cambios en primer lugar. Dado que ninguna otra persona ha protegido desde que sincroniza en su repositorio, solo pueden enviar sus cambios sin necesidad de realizar cualquier combinación.

![Enviar](~/ef6/media/submit.png)

Ahora es el momento para desarrollador \#2 para enviar. No son tan de suerte. Dado que otra persona ha enviado cambios desde que sincronizan, necesitan deslice hacia abajo los cambios y la combinación. El sistema de control de origen probablemente podrá combinar automáticamente los cambios en el nivel de código, ya que son muy simples. El estado de desarrollador \#local de 2 del repositorio después de la sincronización se muestra en el gráfico siguiente. 

![Incorporación de cambios](~/ef6/media/pull.png)

En esta fase para desarrolladores \#puede ejecutar 2 **Update-Database** que detectará la nueva **AddRating** migración (que no se ha aplicado al desarrollador \#de 2 base de datos) y aplicarla. Ahora el **clasificación** columna se agrega a la **Blogs** está sincronizada con el modelo de tabla y la base de datos.

Sin embargo, existen un par de problemas:

1.  Aunque **Update-Database** se aplicará la **AddRating** también generará una advertencia de migración: *no se puede actualizar la base de datos para que coincida con el modelo actual porque hay cambios pendientes y se deshabilitó la migración automática...*
    El problema es que la instantánea del modelo se almacena en la última migración (**AddReader**) falta el **clasificación** propiedad **Blog** (puesto que no era parte del modelo cuando el se ha generado la migración). En primer lugar, el código detecta que el modelo en la última migración no coincide con el modelo actual y genera la advertencia.
2.  Ejecutar la aplicación daría como resultado una excepción InvalidOperationException que indica que "*el model que respalda el contexto 'BloggingContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos"*
    Nuevamente, el problema es que la instantánea del modelo almacenada en la última migración no coincide con el modelo actual.
3.  Por último, esperaríamos ejecución **Add-Migration** ahora generaría una migración vacía (ya que no hay ningún cambio para aplicar a la base de datos). Sin embargo, dado que las migraciones se compara el modelo actual a uno desde la última migración (que falta el **clasificación** propiedad) aplicará realmente la técnica scaffolding otro **AddColumn** llamada para agregar en el **Clasificación** columna. Por supuesto, esta migración produciría un error durante la **Update-Database** porque el **clasificación** columna ya existe.

## <a name="resolving-the-merge-conflict"></a>Resolver los conflictos de combinación

La buena noticia es que no es demasiado difícil de tratar con la combinación manualmente, siempre que tenga una comprensión del funcionamiento de las migraciones. Por tanto, si ha omitido con antelación a esta sección... lo sentimos, que deberá volver y leer primero el resto del artículo.

Hay dos opciones, es el más fácil generar una migración en blanco que contenga el modelo actual correcto como una instantánea. La segunda opción consiste en actualizar la instantánea en la última migración que tiene el formato correcto instantánea del modelo. La segunda opción es un poco más difícil y no se puede usar en todos los escenarios, pero también es más limpio porque no implica agregar una migración adicional.

### <a name="option-1-add-a-blank-merge-migration"></a>Opción 1: Agregar una migración en blanco 'merge'

En esta opción se genere una migración en blanco únicamente con el fin de asegurarse de que la migración más reciente tiene la instantánea del modelo correcto almacenados en ella.

Esta opción puede utilizarse con independencia de que generó la última migración. En el ejemplo que hemos estado siguiendo Developer \#2 se ocupa de la fusión mediante combinación y le ha ocurrido a generar la última migración. Pero se pueden usar estos mismos pasos si desarrollador \#1 genera la última migración. Los pasos también se aplican si intervienen varias migraciones, simplemente hemos analizado dos para mantenerlo simple.

El proceso siguiente puede utilizarse para este enfoque, comenzando desde el momento en que se da cuenta de que los cambios que deben sincronizarse desde control de código fuente.

1.  Asegúrese de que se han escrito los cambios pendientes del modelo en la base de código local para una migración. Este paso garantiza que no pierda los cambios legítimos cuando llegue el momento para generar la migración en blanco.
2.  Sincronización con control de código fuente.
3.  Ejecute **Update-Database** para aplicar cualquier migración nueva que se han protegido otros desarrolladores.
    **
    *Nota: *** si no obtiene las advertencias del comando Update-Database, a continuación, se han producido ninguna nueva migración de otros desarrolladores y no hay ninguna necesidad de realizar cualquier combinación.*
4.  Ejecute **Add-Migration &lt;elegir\_un\_nombre&gt; – IgnoreChanges** (por ejemplo, **mezcla Add-Migration – IgnoreChanges**). Esto genera una migración con todos los metadatos (incluida una instantánea del modelo actual), pero se pasará por alto los cambios que se detecta al comparar los modelos actual a la instantánea en las últimas migraciones (lo que significa que obtendrá un espacio en blanco **seguridad** y **Abajo** método).
5.  Continuar desarrollando o enviar al control de código fuente (después de ejecutar sus pruebas unitarias por supuesto).

Este es el estado de desarrollador \#local de 2 del código base después de usar este enfoque.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Opción 2: Actualizar la instantánea del modelo en la última migración

Esta opción es muy similar a la opción 1, pero evita la migración en blanco adicionales – porque Seamos realistas, que desea que los archivos de código adicional en su solución.

**Este enfoque sólo es posible si la migración más reciente solo existe en la base de código local y no todavía se envió al control de código fuente (por ejemplo, si la última migración generada por el usuario que realiza la combinación)**. Editar los metadatos de las migraciones que otros desarrolladores haya aplicado a su base de datos de desarrollo, o incluso peor se aplica a una base de datos de producción, puede producir efectos laterales inesperados. Durante el proceso, vamos a revertir la última migración en nuestra base de datos local y volver a aplicarlo con los metadatos actualizados.

Aunque la última migración necesita simplemente ser en el código local base no existen restricciones para el número o el orden de las migraciones que lo continuar. Puede haber varias migraciones de varios desarrolladores diferentes y se aplican los mismos pasos, simplemente hemos analizado dos para mantenerlo simple.

El proceso siguiente puede utilizarse para este enfoque, comenzando desde el momento en que se da cuenta de que los cambios que deben sincronizarse desde control de código fuente.

1.  Asegúrese de que se han escrito los cambios pendientes del modelo en la base de código local para una migración. Este paso garantiza que no pierda los cambios legítimos cuando llegue el momento para generar la migración en blanco.
2.  Sincronizar con el control de código fuente.
3.  Ejecute **Update-Database** para aplicar cualquier migración nueva que se han protegido otros desarrolladores.
    **
    *Nota: *** si no obtiene las advertencias del comando Update-Database, a continuación, se han producido ninguna nueva migración de otros desarrolladores y no hay ninguna necesidad de realizar cualquier combinación.*
4.  Ejecute **Update-Database: TargetMigration &lt;segundo\_última\_migración&gt;**  (en el ejemplo que hemos estado siguiendo sería **Update-Database: TargetMigration AddRating**). Este roles hacer una copia de la base de datos al estado de la segunda última migración – eficazmente 'sin aplicar' la última migración desde la base de datos.
    **
    *Nota: *** este paso es necesario para que sea seguro editar los metadatos de la migración, ya que los metadatos también se almacenan en el \_ \_MigrationsHistoryTable de la base de datos. Se trata de por qué sólo se debe utilizar esta opción si la última migración únicamente está en la base de código local. Si otras bases de datos tenían la última migración aplicada también tendría que ellos revertir y volver a aplicar la última migración para actualizar los metadatos.* 
5.  Ejecute **Add-Migration &lt;completa\_nombre\_incluidos\_timestamp\_de\_última\_migración** &gt; (en el ejemplo nos hemos ido siguiendo sería algo parecido a **Add-Migration 201311062215252\_AddReaders**).
    **
    *Nota: *** debe incluir la marca de tiempo para que sepa que las migraciones que desea editar la migración existente en lugar de scaffolding uno nuevo.*
    Esto actualizará los metadatos de la última migración para que coincida con el modelo actual. Obtendrá la siguiente advertencia cuando se complete el comando, pero eso es exactamente lo que desea. "*Sólo el código de diseñador para la migración ' 201311062215252\_'AddReaders se vuelve a con scaffolding. Para volver a aplicar la técnica scaffolding de la migración completa, utilice el parámetro - Force. "*
6.  Ejecute **Update-Database** para volver a aplicar la migración más reciente con los metadatos actualizados.
7.  Continuar desarrollando o enviar al control de código fuente (después de ejecutar sus pruebas unitarias por supuesto).

Este es el estado de desarrollador \#local de 2 del código base después de usar este enfoque.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Resumen

Hay algunos desafíos al usar migraciones de Code First en un entorno de equipo. Sin embargo, un conocimiento básico del funcionamiento de las migraciones y algunos enfoques simple para resolver conflictos de combinación facilitan a superar estos desafíos.

El problema fundamental es incorrecto metadatos almacenados en la migración más reciente. Esto hace que Code First para detectar incorrectamente que el modelo actual y el esquema de base de datos no coinciden y para aplicar scaffolding a código incorrecto en la siguiente migración. Generando una migración en blanco con el modelo correcto, o bien actualizar los metadatos de la migración más reciente, se puede solucionar esta situación.
