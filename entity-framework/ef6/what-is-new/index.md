---
title: Novedades de EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198041"
---
# <a name="whats-new-in-ef6"></a>Novedades de EF6

Se recomienda encarecidamente usar la versión más reciente de Entity Framework para asegurarse de obtener las últimas características y la mayor estabilidad.
Pero es posible que deba usar una versión anterior o que quiera experimentar con las nuevas mejoras de la última versión preliminar.
Para instalar versiones concretas de EF, vea [Get Entity Framework](~/ef6/fundamentals/install.md) (Obtener Entity Framework).

## <a name="ef-630"></a>EF 6.3.0

El runtime de EF 6.3.0 para NuGet se publicó en septiembre de 2019. El objetivo principal de esta versión era facilitar la migración de las aplicaciones existentes que usan EF 6 a .NET Core 3.0. La comunidad también ha contribuido con varias correcciones de errores y mejoras. Consulte los problemas cerrados en cada [hito](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la versión 6.3.0 para información detallada. Estos son algunos de los más importantes:

- Compatibilidad con .NET Core 3.0.
  - El paquete EntityFramework ahora tiene como destino .NET Standard 2.1 además de .NET Framework 4.x.
  - Se han vuelto a escribir los comandos de migración para ejecutarlos fuera de proceso y que trabajen con proyectos de estilo de SDK.
- Compatibilidad con HierarchyId de SQL Server
- Compatibilidad mejorada con PackageReference de Roslyn y NuGet
- Se agregó la utilidad `ef6.exe` para habilitar, agregar, generar scripts y aplicar migraciones desde los ensamblados. Esto reemplaza a `migrate.exe`

### <a name="ef-designer-support"></a>Compatibilidad con el diseñador de EF

Actualmente no se admite el uso del diseñador de EF directamente en proyectos de .NET Core o .NET Standard. 

Puede solucionar esta limitación agregando el archivo EDMX y las clases generadas para las entidades y DbContext como archivos vinculados a un proyecto de .NET Core 3.0 o .NET Standard 2.1 en la misma solución.

Los archivos vinculados tendrían el aspecto siguiente:

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

Tenga en cuenta que el archivo EDMX está vinculado con la acción de compilación EntityDeploy. Se trata de una tarea especial de MSBuild (ahora incluida en el paquete EF 6.3) que se encarga de agregar el modelo EF en el ensamblado de destino como recursos incrustados (o de copiarlo como archivos en la carpeta de salida, en función de la configuración de procesamiento de artefactos de metadatos en EDMX). Para más información sobre cómo configurar esta configuración, consulte nuestro [ejemplo de .NET Core de EDMX](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Versiones anteriores

La página [Versiones anteriores](past-releases.md) contiene un archivo de todas las versiones anteriores de EF y de las características principales incluidas en cada versión.
