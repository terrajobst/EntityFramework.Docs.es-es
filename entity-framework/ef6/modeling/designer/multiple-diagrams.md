---
title: 'Varios diagramas por modelo: EF6'
author: divega
ms.date: 2016-10-23
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: f27afbd3b44ff3eb8044ab960f10b2856a603ee3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993783"
---
# <a name="multiple-diagrams-per-model"></a>Varios diagramas por modelo
> [!NOTE]
> **EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

Esta página y el vídeo se muestra cómo dividir un modelo en varios diagramas mediante el Diseñador de Entity Framework (EF Designer). Es posible que desee usar esta característica cuando el modelo se vuelve demasiado grande para ver o editar.

En versiones anteriores de EF Designer podría tener sólo un diagrama por el archivo EDMX. A partir de Visual Studio 2012, puede utilizar el Diseñador de EF para dividir el archivo EDMX en varios diagramas.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo muestra cómo dividir un modelo en varios diagramas mediante el Diseñador de Entity Framework (EF Designer). Es posible que desee usar esta característica cuando el modelo se vuelve demasiado grande para ver o editar.

**Presentado por**: Julia Kornich

**Vídeo**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Información general del diseñador EF

Cuando se crea un modelo usando el Asistente del Diseñador de EF Entity Data Model, se crea un archivo .edmx y se agrega a la solución. Este archivo define la forma de las entidades y cómo se asignan a la base de datos.

EF Designer consta de los siguientes componentes:

-   Una superficie de diseño visual para modificar el modelo. Puede crear, modificar o eliminar entidades y asociaciones.
-   Un **Explorador de modelos** ventana que proporciona vistas de árbol del modelo.  Las entidades y sus asociaciones se encuentran en el *\[ModelName\]* carpeta. Las tablas de base de datos y restricciones que se encuentran en el  *\[ModelName\]*. Carpeta de Store.
-   Un **detalles de Mapping** ventana para ver y editar asignaciones. Puede asignar tipos de entidad o asociaciones a tablas, columnas y procedimientos almacenados de base de datos. 

Cuando finalice el Asistente para Entity Data Model, se abre automáticamente la ventana de superficie de diseño visual. Si el Explorador de modelos no está visible, haga clic en la principal superficie de diseño y seleccione **Explorador de modelos**.

Captura de pantalla siguiente muestra un archivo .edmx abierto en EF Designer. La captura de pantalla muestra la superficie de diseño visual (a la izquierda) y el **Explorador de modelos** ventana (a la derecha).

![EFDesigner2](~/ef6/media/efdesigner2.png)

Para deshacer una operación que se realiza en EF Designer, haga clic en Ctrl-Z.

## <a name="working-with-diagrams"></a>Trabajar con diagramas

De forma predeterminada el Diseñador de EF crea un diagrama llamado Diagram1. Si tiene un diagrama con un gran número de entidades y asociaciones, se enlazarán más conveniente dividirla lógicamente. A partir de Visual Studio 2012, puede ver el modelo conceptual en varios diagramas.   

A medida que agrega nuevos diagramas aparecen en la carpeta de diagramas en la ventana Explorador de modelos. Para cambiar el nombre de un diagrama: seleccione el diagrama en la ventana Explorador de modelos, haga clic una vez en el nombre y escriba el nuevo nombre.  También puede haga clic en el nombre del diagrama y seleccione **cambiar el nombre**.

El nombre del diagrama se muestra junto al nombre del archivo .edmx, en el editor de Visual Studio. Por ejemplo Model1.edmx\[Diagram1\].

![Nombrediagrama](~/ef6/media/diagramname.png)

El contenido de diagramas (forma y color de las entidades y asociaciones) se almacena en el. archivo edmx.diagram. Para ver este archivo, seleccione el Explorador de soluciones y expandir el archivo .edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

No debe editar el. edmx.diagram archivos manualmente, el contenido de este archivo quizás se sobrescriben con EF Designer.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>División de entidades y asociaciones en un nuevo diagrama

Puede seleccionar las entidades en el diagrama existente (mantenga presionada MAYÚS para seleccionar varias entidades). Haga clic en el botón secundario del mouse y seleccione **mover al nuevo diagrama**. Se crea el nuevo diagrama y las entidades seleccionadas y sus asociaciones se mueven al diagrama.

Como alternativa, puede haga clic en la carpeta de diagramas en el Explorador de modelos y seleccione **Agregar nuevo diagrama.** A continuación, puede arrastrar y colocar las entidades desde la carpeta tipos de entidad en el Explorador de modelos a la superficie de diseño.

También puede cortar o copiar las entidades (mediante las teclas Ctrl-X o presione Ctrl-C) de un diagrama y pegar (con clave CTRL+v) en el otro. Si el diagrama en el que va a pegar una entidad ya contiene una entidad con el mismo nombre, se creará una nueva entidad y se agregan al modelo.  Por ejemplo: diagrama2 contiene la entidad Department. A continuación, pegar otro departamento en diagrama2. La entidad Department1 se crea y agrega al modelo conceptual.   

Para incluir las entidades relacionadas en un diagrama, haga clic la entidad y seleccione **relacionados incluyen**. Esto hará una copia de las entidades relacionadas y asociaciones en el diagrama especificado.

## <a name="changing-the-color-of-entities"></a>Cambiar el Color de las entidades

Además de dividir un modelo en varios diagramas, también puede cambiar los colores de las entidades.

Para cambiar el color, seleccione una entidad (o varias entidades) en la superficie de diseño. A continuación, haga clic en el botón secundario del mouse y seleccione **propiedades**. En la ventana Propiedades, seleccione la **Color de relleno** propiedad. Especifique el color con un nombre de color válidos (por ejemplo, rojo) o un RGB válido (por ejemplo, 255, 128, 128). 

![Color](~/ef6/media/color.png)

## <a name="summary"></a>Resumen

En este tema hemos examinado cómo dividir un modelo en varios diagramas y también cómo especificar un color diferente para una entidad mediante el Diseñador de Entity Framework. 
