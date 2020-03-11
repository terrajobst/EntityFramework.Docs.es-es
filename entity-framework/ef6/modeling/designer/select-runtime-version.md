---
title: 'Selección de Entity Framework versión en tiempo de ejecución para los modelos de EF Designer: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414996"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Selección de Entity Framework versión en tiempo de ejecución para los modelos EF Designer
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

A partir de EF6, se ha agregado la siguiente pantalla a EF Designer para que pueda seleccionar la versión del tiempo de ejecución al que desea dirigirse al crear un modelo. La pantalla aparecerá cuando la versión más reciente de Entity Framework no esté ya instalada en el proyecto. Si ya está instalada la versión más reciente, se usará de forma predeterminada.

![Pantalla](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Destino de EF6. x

Puede elegir EF6 en la pantalla "elegir su versión" para agregar el tiempo de ejecución de EF6 al proyecto. Una vez que haya agregado EF6, dejará de ver esta pantalla en el proyecto actual.

EF6 se deshabilitará si ya tiene instalada una versión anterior de EF (ya que no puede tener como destino varias versiones del tiempo de ejecución del mismo proyecto). Si la opción EF6 no está habilitada aquí, siga estos pasos para actualizar el proyecto a EF6:

1.  Haga clic con el botón derecho en el proyecto en Explorador de soluciones y seleccione **administrar paquetes NuGet...**
2.  Seleccionar **actualizaciones**
3.  Seleccione **EntityFramework** (Asegúrese de que se va a actualizar a la versión que quiera)
4.  Haga clic en **Update** (Actualizar).

 

## <a name="targeting-ef5x"></a>Destino de EF5. x

Puede elegir EF5 en la pantalla "elegir su versión" para agregar el tiempo de ejecución de EF5 al proyecto. Una vez que haya agregado EF5, seguirá viendo la pantalla con la opción EF6 deshabilitada.

Si ya tiene instalada una versión de EF4. x del Runtime, verá que la versión de EF aparece en la pantalla en lugar de EF5. En esta situación, puede actualizar a EF5 siguiendo estos pasos:

1.  Seleccione **herramientas-administrador de paquetes de la biblioteca de&gt;-&gt; consola del administrador de paquetes**
2.  Ejecute **Install-Package EntityFramework-version 5.0.0**

 

## <a name="targeting-ef4x"></a>Destino de EF4. x

Puede instalar el tiempo de ejecución de EF4. x en el proyecto mediante los pasos siguientes:

1.  Seleccione **herramientas-administrador de paquetes de la biblioteca de&gt;-&gt; consola del administrador de paquetes**
2.  Ejecute **Install-Package EntityFramework-version 4.3.0**
