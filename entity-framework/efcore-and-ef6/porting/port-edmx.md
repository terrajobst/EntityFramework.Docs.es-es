---
title: 'Portabilidad de EF6 a EF Core: trasladar un modelo basado en EDMX'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997416"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portabilidad de un modelo basado en EDMX de EF6 a EF Core

EF Core no es compatible con el formato de archivo EDMX para los modelos. Es la mejor opción para estos modelos, el puerto generar un nuevo modelo basado en código de la base de datos para la aplicación.

## <a name="install-ef-core-nuget-packages"></a>Instalar paquetes de NuGet de EF Core

Instalar el `Microsoft.EntityFrameworkCore.Tools` paquete NuGet.

## <a name="regenerate-the-model"></a>Volver a generar el modelo

Ahora puede usar la funcionalidad de ingeniería inversa para crear un modelo basado en la base de datos existente.

Ejecute el siguiente comando en la consola de administrador de paquetes (Herramientas –> Administrador de paquetes de NuGet –> Package Manager Console). Consulte [consola del Administrador de paquetes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opciones de comando aplicar scaffolding a un subconjunto de tablas etcetera.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por ejemplo, aquí es el comando para aplicar scaffolding a un modelo a partir de la base de datos de creación de blogs en la instancia de SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Quitar el modelo de EF6

El modelo de EF6 se quitarían ahora desde su aplicación.

Es correcto dejar el paquete NuGet de EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser utilizado por en paralelo en la misma aplicación. Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete para obtener errores de compilación en fragmentos de código que necesitan atención.

## <a name="update-your-code"></a>Actualizar el código

En este momento, es cuestión de direccionamiento de los errores de compilación y revisión de código para ver si los cambios de comportamiento entre EF6 y EF Core le afectará.

## <a name="test-the-port"></a>Probar el puerto

Simplemente porque se compila la aplicación, no significa que es portada correctamente a EF Core. Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento han afectado negativamente la aplicación.

> [!TIP]
> Consulte [Introducción a EF Core en ASP.NET Core con una base de datos](xref:core/get-started/aspnetcore/existing-db) para una referencia adicional sobre cómo trabajar con una base de datos existente 
