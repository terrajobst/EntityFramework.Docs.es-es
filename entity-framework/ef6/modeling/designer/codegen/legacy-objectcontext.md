---
title: Revertir a ObjectContext en Entity Framework Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415434"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Revertir a ObjectContext en Entity Framework Designer
Con la versión anterior de Entity Framework un modelo creado con EF Designer generaría un contexto derivado de ObjectContext y clases de entidad derivadas de EntityObject.

A partir de EF 4.1, se recomienda cambiar a una plantilla de generación de código que genera un contexto que se deriva de las clases de entidad DbContext y POCO.

En Visual Studio 2012, se obtiene el código DbContext generado de forma predeterminada para todos los nuevos modelos creados con el diseñador de EF. Los modelos existentes seguirán generando código basado en ObjectContext a menos que decida cambiar al generador de código basado en DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Revertir a la generación de código de ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. deshabilitar la generación de código DbContext

La generación de las clases DbContext y POCO derivadas se controla mediante dos archivos. TT en el proyecto. Si expande el archivo. edmx en el explorador de soluciones, verá estos archivos. Elimine ambos archivos del proyecto.

![Archivos de generación de código](~/ef6/media/codegenfiles.png)

Si usa VB.NET, debe seleccionar el botón **Mostrar todos los archivos** para ver los archivos anidados.

![Mostrar todos los archivos](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. volver a habilitar la generación de código de ObjectContext

Abra el modelo en el diseñador de EF, haga clic con el botón derecho en una sección en blanco de la superficie de diseño y seleccione **propiedades**.

En el ventana Propiedades cambie la **estrategia de generación de código** de **ninguno** a **predeterminado**.

![Estrategia de generación de código](~/ef6/media/codegenstrategy.png)
