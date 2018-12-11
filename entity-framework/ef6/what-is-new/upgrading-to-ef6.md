---
title: Actualizar a Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 711f1940080de27bd23cb8f641a5c7f2711dd65b
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182012"
---
# <a name="upgrading-to-entity-framework-6"></a>Actualizar a Entity Framework 6

En versiones anteriores de EF se dividió el código entre las bibliotecas de core (principalmente System.Data.Entity.dll) se incluye como parte de .NET Framework y las bibliotecas de (OOB) de fuera de banda (principalmente EntityFramework.dll) se incluye en un paquete de NuGet. EF6 toma el código de las bibliotecas de core y se incorpora a las bibliotecas OOB. Esto ha sido necesario para permitir que EF realizarse de código abierto y para que sea capaz de evolucionar a un ritmo diferente de .NET Framework. La consecuencia de esto es que las aplicaciones deben volver a generarse con los tipos que se ha movido.

Esto debe ser sencilla para las aplicaciones que hacen uso de DbContext como enviadas en EF 4.1 y versiones posteriores. Se necesita para las aplicaciones que hacen uso de ObjectContext un poco más trabajo, pero todavía no es difícil de hacer.

Le presentamos una lista de comprobación de las cosas que debe hacer para actualizar una aplicación existente a EF6.

## <a name="1-install-the-ef6-nuget-package"></a>1. Instale el paquete NuGet de EF6

Deberá actualizar a la nueva Entity Framework 6 en tiempo de ejecución.

1. Haga doble clic en el proyecto y seleccione **administrar paquetes NuGet...**  
2. En el **Online** ficha, seleccione **EntityFramework** y haga clic en **instalar**  
   > [!NOTE]
   > Si una versión anterior de EntityFramework NuGet se instaló el paquete se actualizará a EF6.

Como alternativa, puede ejecutar el siguiente comando desde la consola de administrador de paquetes:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Asegúrese de que se han quitado las referencias de ensamblado a System.Data.Entity.dll

Instalar el paquete NuGet de EF6 automáticamente debe quitar las referencias a System.Data.Entity desde el proyecto para usted.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Intercambiar los modelos de EF Designer (EDMX) para usar la generación de código EF 6.x

Si tiene cualquier modelo creado con el Diseñador de EF, deberá actualizar las plantillas de generación de código para generar código compatible con EF6.

> [!NOTE]
> Actualmente hay disponibles para Visual Studio 2012 y 2013 solo plantillas de generador de DbContext EF 6.x.

1. Eliminar plantillas de generación de código existentes. Estos archivos se suele denominar  **\<edmx_file_name\>.tt** y  **\<edmx_file_name\>. Context.tt** y estar anidado en el archivo edmx en el Explorador de soluciones. Puede seleccionar las plantillas en el Explorador de soluciones y presione la **SUPR** clave para eliminarlos.  
   > [!NOTE]
   > En proyectos de sitios Web las plantillas no se anidado en el archivo edmx, pero aparece junto a ella en el Explorador de soluciones.  

   > [!NOTE]
   > En los proyectos de VB.NET deberá habilitar "Mostrar todos los archivos' poder ver los archivos de plantilla anidada.
2. Agregue la plantilla de generación de código de adecuada EF 6.x. Abra el modelo de EF Designer, haga doble clic en la superficie de diseño y seleccione **Add Code Generation Item...**
    - Si está usando la API de DbContext (recomendado), a continuación, **EF 6.x generador de DbContext** estará disponible en el **datos** ficha.  
      > [!NOTE]
      > Si utiliza Visual Studio 2012, deberá instalar las herramientas de EF 6 para que esta plantilla. Consulte [obtener Entity Framework](~/ef6/fundamentals/install.md) para obtener más información.  

    - Si está utilizando la API de ObjectContext, a continuación, deberá seleccionar la **Online** pestaña y busque **EF 6.x generador de EntityObject**.  
3. Si aplica las personalizaciones a las plantillas de generación de código que deberá volver a aplicarlos a las plantillas actualizadas.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Actualice los espacios de nombres para cualquier tipo EF core que se va a usar

No han cambiado los espacios de nombres para tipos de DbContext y Code First. Esto significa que para muchas aplicaciones que usan EF 4.1 o posterior no debe cambiar nada.

Tipos como ObjectContext que anteriormente se encontraban en System.Data.Entity.dll se movieron a nuevos espacios de nombres. Esto significa que deba actualizar su *mediante* o *importación* directivas para la compilación EF6.

La regla general para los cambios del espacio de nombres es que cualquier tipo de System.Data.* se mueve al System.Data.Entity.Core.*. En otras palabras, sólo tiene que insertar **Entity.Core.** Después de System.Data. Por ejemplo:

- System.Data.EntityException = > System.Data. **Entity.Core**. EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core**. Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core**. Objects.DataClasses.RelationshipManager  

Estos tipos están en el *Core* espacios de nombres porque no se usan directamente para la mayoría de las aplicaciones basadas en DbContext. Algunos tipos que formaban parte de System.Data.Entity.dll todavía se utilizan con frecuencia y directamente para las aplicaciones basadas en DbContext y lo que no se han movido los *Core* espacios de nombres. Estos son:

- System.Data.EntityState = > System.Data. **Entidad**. Valor de EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Esta clase ha cambiado de nombre; una clase con el nombre antiguo todavía existe y funciona, pero ahora marcado como obsoleto.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Esta clase ha cambiado de nombre; una clase con el nombre antiguo todavía existe y funciona, pero ahora marcado como obsoleto.)  
- Clases espaciales (por ejemplo, DbGeography, DbGeometry) han movido desde System.Data.Spatial = > System.Data. **Entidad**. Espacial

> [!NOTE]
> Algunos tipos en el espacio de nombres System.Data se encuentran en System.Data.dll, que no es un ensamblado EF. Estos tipos no se han movido y por lo que sus espacios de nombres permanecen sin cambios.
