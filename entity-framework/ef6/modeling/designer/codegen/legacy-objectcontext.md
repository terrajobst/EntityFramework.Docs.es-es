---
title: Revertir a un ObjectContext en Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122343"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Revertir a un ObjectContext en Entity Framework Designer
Con la versión anterior de Entity Framework, un modelo creado con EF Designer generaría un contexto que deriva de ObjectContext y clases de entidad que se derivan de EntityObject.

A partir de EF4.1 se recomienda usarlo en una plantilla de generación de código que genera un contexto de derivar las clases de entidad de DbContext y POCO.

En Visual Studio 2012, obtendrá código DbContext genera de forma predeterminada para todos los modelos nuevos creados con EF Designer. Los modelos existentes seguirá generando código en función de ObjectContext a menos que decida cambiar a generador de código en función de DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Volver a la generación de código de ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Deshabilitar la generación de código de DbContext

Generación de las clases derivadas de DbContext y POCO se controla mediante dos archivos .tt en el proyecto, si expande el archivo .edmx en el Explorador de soluciones, verá estos archivos. Elimine los dos archivos del proyecto.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

Si utilizas VB.NET deberá seleccionar la **mostrar todos los archivos** botón para ver los archivos anidados.

![ShowAllFiles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Volver a habilitar la generación de código de ObjectContext

Abierto modelar en EF Designer, haga doble clic en una sección vacía de la superficie de diseño y seleccione **propiedades**.

En el cambio de la ventana de propiedades el **Code Generation Strategy** desde **ninguno** a **predeterminado**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
