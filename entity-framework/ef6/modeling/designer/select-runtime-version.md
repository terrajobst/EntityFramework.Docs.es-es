---
title: Seleccionar versión de Entity Framework en tiempo de ejecución para los modelos EF diseñadores - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
caps.latest.revision: 3
ms.openlocfilehash: 75f7b4ed81528683801893c31de490ce15be6733
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122607"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Seleccionar versión de Entity Framework en tiempo de ejecución para los modelos de diseñador de EF
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

A partir de EF6 se agregó la siguiente pantalla para EF Designer para que pueda seleccionar la versión del tiempo de ejecución que desea especificar al crear un modelo. Si la versión más reciente de Entity Framework ya no está instalada en el proyecto, aparecerá la pantalla. Si ya está instalada la versión más reciente solo se usará de forma predeterminada.

![Pantalla](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Destinatarios EF6.x

Puede elegir EF6 desde la pantalla 'Elegir su versión' para agregar el tiempo de ejecución de EF6 a su proyecto. Una vez haya agregado EF6, deberá detener viendo esta pantalla en el proyecto actual.

EF6 se deshabilitará si ya tiene una versión anterior de EF instalado (ya que no puede dirigirse a varias versiones del tiempo de ejecución desde el mismo proyecto). Si no está habilitada la opción de EF6 a continuación, siga estos pasos para actualizar el proyecto a EF6:

1.  Haga doble clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes NuGet...**
2.  Seleccione **actualizaciones**
3.  Seleccione **EntityFramework** (asegúrese de que va a actualizar a la versión que desee)
4.  Haga clic en **Update**

 

## <a name="targeting-ef5x"></a>Destinatarios EF5.x

Puede elegir EF5 desde la pantalla 'Elegir su versión' para agregar el tiempo de ejecución de EF5 a su proyecto. Una vez haya agregado EF5, aún verá la pantalla con la opción de EF6 deshabilitada.

Si tiene una versión EF4.x del runtime instalada ya verá que la versión de EF que aparecen en la pantalla, en lugar de EF5. En esta situación puede actualizar a EF5 mediante los siguientes pasos:

1.  Seleccione **herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**
2.  Ejecute **Install-Package EntityFramework-versión 5.0.0**

 

## <a name="targeting-ef4x"></a>Destinatarios EF4.x

Puede instalar el tiempo de ejecución EF4.x al proyecto mediante los pasos siguientes:

1.  Seleccione **herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**
2.  Ejecute **Install-Package EntityFramework-versión 4.3.0**