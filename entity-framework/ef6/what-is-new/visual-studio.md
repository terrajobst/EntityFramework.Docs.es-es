---
title: 'Versiones de Visual Studio: EF6'
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: 16bcdc6d0e7c5632d4f4c06ba285a7a666f24204
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414360"
---
# <a name="visual-studio-releases"></a>Versiones de Visual Studio

Se recomienda usar siempre la versión más reciente de Visual Studio, ya que contiene las herramientas más recientes para .NET, NuGet y Entity Framework.
De hecho, los distintos ejemplos y tutoriales de la documentación de Entity Framework suponen que está usando una versión reciente de Visual Studio.

Sin embargo, es posible usar versiones anteriores de Visual Studio con versiones diferentes de Entity Framework siempre que tenga en cuenta algunas diferencias:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 y versiones más recientes

- Esta versión de Visual Studio incluye la versión más reciente de las herramientas de Entity Framework y el tiempo de ejecución de EF 6,2 y no requiere pasos de configuración adicionales.
Vea [las novedades para](~/ef6/what-is-new/index.md) obtener más información sobre estas versiones.
- Al agregar Entity Framework a los nuevos proyectos con las herramientas de EF, se agregará automáticamente el paquete NuGet EF 6,2.
Puede instalar o actualizar manualmente cualquier paquete de NuGet de EF disponible en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección del servidor de la cadena de conexión que se debe usar es "(LocalDB)\\MSSQLLocalDB".
Recuerde usar una cadena textual con el prefijo `@` o la doble barra diagonal inversa "\\\\" al especificar una cadena de conexión en C# el código.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 a Visual Studio 2017 15,6

- Estas versiones de Visual Studio incluyen herramientas de Entity Framework y 6.1.3 en tiempo de ejecución.
Vea [versiones anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información sobre estas versiones.
- Al agregar Entity Framework a los nuevos proyectos con las herramientas de EF, se agregará automáticamente el paquete NuGet EF 6.1.3.
Puede instalar o actualizar manualmente cualquier paquete de NuGet de EF disponible en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección del servidor de la cadena de conexión que se debe usar es "(LocalDB)\\MSSQLLocalDB".
Recuerde usar una cadena textual con el prefijo `@` o la doble barra diagonal inversa "\\\\" al especificar una cadena de conexión en C# el código.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Esta versión de Visual Studio incluye y la versión anterior de las herramientas de Entity Framework y el tiempo de ejecución.
Se recomienda que actualice a Entity Framework Tools 6.1.3, mediante [el instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponible en el centro de descarga de Microsoft.
Vea [versiones anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información sobre estas versiones.
- Al agregar Entity Framework a los nuevos proyectos mediante las herramientas de EF actualizadas, se agregará automáticamente el paquete NuGet EF 6.1.3.
Puede instalar o actualizar manualmente cualquier paquete de NuGet de EF disponible en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección del servidor de la cadena de conexión que se debe usar es "(LocalDB)\\MSSQLLocalDB".
Recuerde usar una cadena textual con el prefijo `@` o la doble barra diagonal inversa "\\\\" al especificar una cadena de conexión en C# el código.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Esta versión de Visual Studio incluye y la versión anterior de las herramientas de Entity Framework y el tiempo de ejecución.
Se recomienda que actualice a Entity Framework Tools 6.1.3, mediante [el instalador](https://www.microsoft.com/download/details.aspx?id=40762) disponible en el centro de descarga de Microsoft.
Vea [versiones anteriores](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información sobre estas versiones.
- Al agregar Entity Framework a los nuevos proyectos mediante las herramientas de EF actualizadas, se agregará automáticamente el paquete NuGet EF 6.1.3.
Puede instalar o actualizar manualmente cualquier paquete de NuGet de EF disponible en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada v 11.0.
La sección del servidor de la cadena de conexión que se debe usar es "(LocalDB)\\v 11.0".
Recuerde usar una cadena textual con el prefijo `@` o la doble barra diagonal inversa "\\\\" al especificar una cadena de conexión en C# el código.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- La versión de Entity Framework Tools disponible con esta versión de Visual Studio no es compatible con el tiempo de ejecución de Entity Framework 6 y no se puede actualizar.
- De forma predeterminada, las herramientas de Entity Framework agregarán Entity Framework 4,0 a los proyectos.
Para crear aplicaciones con las versiones más recientes de EF, primero deberá instalar la extensión del administrador de [paquetes NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- De forma predeterminada, toda la generación de código en la versión de las herramientas de EF se basa en EntityObject y Entity Framework 4.
Se recomienda cambiar la generación de código para que se base en DbContext y Entity Framework 5, mediante la instalación de plantillas de generación de [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) código DbContext para o [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Una vez que haya instalado las extensiones del administrador de paquetes NuGet, puede instalar o actualizar manualmente cualquier paquete de NuGet de EF disponible en línea y usar EF6 con Code First, que no requiere un diseñador.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio se SQL Server Express denominada SQLEXPRESS.
La sección del servidor de la cadena de conexión que se debe usar es ".\\SQLEXPRESS ".
Recuerde usar una cadena textual con el prefijo `@` o la doble barra diagonal inversa "\\\\" al especificar una cadena de conexión en C# el código.
