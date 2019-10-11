---
title: Migrar de EF6 a EF Core-Porting de un modelo basado en EDMX-EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182063"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Portabilidad de un modelo basado en EF6 EDMX a EF Core

EF Core no admite el formato de archivo EDMX para los modelos. La mejor opción para portar estos modelos es generar un nuevo modelo basado en código a partir de la base de datos de la aplicación.

## <a name="install-ef-core-nuget-packages"></a>Instalación de EF Core paquetes NuGet

Instale el paquete NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Regeneración del modelo

Ahora puede usar la funcionalidad de ingeniería inversa para crear un modelo basado en la base de datos existente.

Ejecute el siguiente comando en la consola del administrador de paquetes (Herramientas – > Administrador de paquetes NuGet – > consola del administrador de paquetes). Vea [consola del administrador de paquetes (Visual Studio)](../../core/miscellaneous/cli/powershell.md) para ver las opciones de comando para aplicar scaffolding a un subconjunto de tablas, etc.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Por ejemplo, este es el comando para aplicar scaffolding a un modelo a partir de la base de datos de blogs en la SQL Server instancia de LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Quitar el modelo EF6

Ahora se quitará el modelo EF6 de la aplicación.

Es adecuado dejar el paquete NuGet de EF6 (EntityFramework) instalado, ya que EF Core y EF6 se pueden usar en paralelo en la misma aplicación. Sin embargo, si no va a usar EF6 en ninguna de las áreas de la aplicación, la desinstalación del paquete le ayudará a dar errores de compilación en fragmentos de código que requieren atención.

## <a name="update-your-code"></a>Actualización del código

En este punto, es cuestión de solucionar los errores de compilación y revisar el código para ver si el comportamiento cambia entre EF6 y EF Core afectará a su caso.

## <a name="test-the-port"></a>Prueba del puerto

El hecho de que la aplicación se compile, no significa que se haya trasladado correctamente a EF Core. Tendrá que probar todas las áreas de la aplicación para asegurarse de que ninguno de los cambios de comportamiento afecte negativamente a la aplicación.
