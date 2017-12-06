---
title: Migrar desde EF6 EF Core - trasladar un modelo basado en EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Trasladar un modelo basado en EDMX de EF6 a EF Core

Núcleo EF no admite el formato de archivo EDMX para los modelos. La mejor opción migrar estos modelos, consiste en generar un nuevo modelo basado en código de la base de datos para la aplicación.

## <a name="install-ef-core-nuget-packages"></a>Instalar paquetes de NuGet de núcleo de EF

Instalar el `Microsoft.EntityFrameworkCore.Tools` paquete NuGet.

## <a name="regenerate-the-model"></a>Volver a generar el modelo

Ahora puede usar la funcionalidad de ingeniería inversa para crear un modelo basado en la base de datos existente.

Ejecute el siguiente comando en la consola de administrador de paquetes (Herramientas –> Administrador de paquetes de NuGet –> Package Manager Console). Vea [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para opciones de comando para aplicar la técnica scaffolding un subconjunto de tablas etcetera.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por ejemplo, este es el comando para aplicar la técnica scaffolding un modelo de la base de datos de creación de blogs en la instancia de SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Quitar EF6 modelo

Ahora se quitará el modelo EF6 desde la aplicación.

Es correcto dejar el paquete de NuGet EF6 (Entity Framework) instalado, como EF Core y EF6 pueden ser usado en paralelo en la misma aplicación. Sin embargo, si no se pretende usar EF6 en las áreas de la aplicación, a continuación, desinstalar el paquete le ayudarán a dar errores de compilación en fragmentos de código que requieren atención.

## <a name="update-your-code"></a>Actualizar el código

En este punto, es una cuestión de direccionamiento errores de compilación y revisión de código para ver si los cambios de comportamiento entre EF6 y EF Core afectarán al usuario.

## <a name="test-the-port"></a>Probar el puerto

Solo porque se compila la aplicación, no significa que se transfieren correctamente a EF Core. Debe probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento se vea afectado negativamente la aplicación.

> [!TIP]
> Vea [Introducción a EF principales en el núcleo de ASP.NET con una base de datos](xref:core/get-started/aspnetcore/existing-db) como una referencia adicional sobre cómo trabajar con una base de datos existente 
