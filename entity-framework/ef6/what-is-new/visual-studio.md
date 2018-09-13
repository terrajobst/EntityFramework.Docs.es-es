---
title: Versiones de Visual Studio - EF6
author: divega
ms.date: 07/05/2018
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: d104236ac5c8877da421ba10de9827f17937a9ec
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489926"
---
# <a name="visual-studio-releases"></a>Versiones de Visual Studio

Se recomienda usar siempre la versión más reciente de Visual Studio porque contiene las herramientas más recientes para. NET, NuGet y Entity Framework.
De hecho, los distintos ejemplos y tutoriales en la documentación de Entity Framework se suponen que utiliza una versión reciente de Visual Studio.

Es posible, sin embargo, para usar las versiones anteriores de Visual Studio con diferentes versiones de Entity Framework, siempre que tener en cuenta algunas diferencias:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15.7 y versiones más recientes

- Esta versión de Visual Studio incluye la versión más reciente de las herramientas de Entity Framework y el tiempo de ejecución de EF 6.2 y no requiere pasos de configuración adicionales.
Consulte [What ' s New](~/ef6/what-is-new/index.md) para obtener más información acerca de estas versiones.
- Adición de Entity Framework a los nuevos proyectos con las herramientas EF agregará automáticamente el paquete NuGet de EF 6.2.
Manualmente, puede instalar o actualizar a cualquier paquete NuGet de EF de disponibles en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección servidor de la cadena de conexión debe usar es "(localdb)\\MSSQLLocalDB".
Recuerde que debe usar una cadena textual con el prefijo `@` o doble barra diagonal inversa "\\\\" cuando se especifica una cadena de conexión en el código de C#.  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 para Visual Studio 2017 15.6

- Estas versiones de Visual Studio incluyen herramientas de Entity Framework y 6.1.3 en tiempo de ejecución.
Consulte [libera últimos](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información acerca de estas versiones.
- Agregar Entity Framework a los nuevos proyectos con las herramientas EF, EF 6.1.3 automáticamente se agregará el paquete de NuGet.
Manualmente, puede instalar o actualizar a cualquier paquete NuGet de EF de disponibles en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección servidor de la cadena de conexión debe usar es "(localdb)\\MSSQLLocalDB".
Recuerde que debe usar una cadena textual con el prefijo `@` o doble barra diagonal inversa "\\\\" cuando se especifica una cadena de conexión en el código de C#.  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Esta versión de Visual Studio incluye y una versión anterior de herramientas de Entity Framework y en tiempo de ejecución.
Se recomienda que actualice a Entity Framework Tools 6.1.3, mediante [el instalador](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibles en Microsoft Download Center.
Consulte [libera últimos](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información acerca de estas versiones.
- Adición de Entity Framework a los nuevos proyectos con las herramientas EF actualizadas agregará automáticamente el EF 6.1.3 paquete NuGet.
Manualmente, puede instalar o actualizar a cualquier paquete NuGet de EF de disponibles en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada MSSQLLocalDB.
La sección servidor de la cadena de conexión debe usar es "(localdb)\\MSSQLLocalDB".
Recuerde que debe usar una cadena textual con el prefijo `@` o doble barra diagonal inversa "\\\\" cuando se especifica una cadena de conexión en el código de C#.  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Esta versión de Visual Studio incluye y una versión anterior de herramientas de Entity Framework y en tiempo de ejecución.
Se recomienda que actualice a Entity Framework Tools 6.1.3, mediante [el instalador](https://www.microsoft.com/en-us/download/details.aspx?id=40762) disponibles en Microsoft Download Center.
Consulte [libera últimos](~/ef6/what-is-new/past-releases.md#ef-613) para obtener más información acerca de estas versiones.
- Adición de Entity Framework a los nuevos proyectos con las herramientas EF actualizadas agregará automáticamente el EF 6.1.3 paquete NuGet.
Manualmente, puede instalar o actualizar a cualquier paquete NuGet de EF de disponibles en línea.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es una instancia de LocalDB denominada v11.0.
La sección servidor de la cadena de conexión debe usar es "(localdb)\\v11.0".
Recuerde que debe usar una cadena textual con el prefijo `@` o doble barra diagonal inversa "\\\\" cuando se especifica una cadena de conexión en el código de C#.  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- La versión de Entity Framework Tools disponibles con esta versión de Visual Studio no es compatible con el tiempo de ejecución de Entity Framework 6 y no se puede actualizar.
- De forma predeterminada, las herramientas de Entity Framework agregará a los proyectos de Entity Framework 4.0.
Para crear aplicaciones con las versiones más recientes de EF, primero debe instalar el [extensión del Administrador de paquetes de NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- De forma predeterminada, todas las de generación de código en la versión de herramientas EF se basa en EntityObject y Entity Framework 4.
Se recomienda que cambie la generación de código que se base en DbContext y Entity Framework 5, al instalar las plantillas de generación de código de DbContext para [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) o [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- Una vez haya instalado las extensiones del Administrador de paquetes de NuGet, puede manualmente instalar o actualizar a cualquier paquete NuGet de EF de disponibles en línea y usar EF6 con Code First, que no requieren que un diseñador.
- De forma predeterminada, la instancia de SQL Server disponible con esta versión de Visual Studio es SQL Server Express denominada SQLEXPRESS.
La sección servidor de la cadena de conexión debe usar es ". \\SQLEXPRESS ".
Recuerde que debe usar una cadena textual con el prefijo `@` o doble barra diagonal inversa "\\\\" cuando se especifica una cadena de conexión en el código de C#.
