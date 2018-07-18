---
title: 'Compatibilidad con el proveedor de tipos espaciales: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
caps.latest.revision: 3
ms.openlocfilehash: 76020e2a3127b1026a5cb8f032686cc8ce9c0c5f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121895"
---
# <a name="provider-support-for-spatial-types"></a>Proveedor de compatibilidad para tipos espaciales
Entity Framework admite trabajar con datos espaciales a través de las clases de DbGeography o DbGeometry. Estas clases se basan en la funcionalidad específica de la base de datos que ofrece el proveedor de Entity Framework. No todos los proveedores admiten datos espaciales y aquellos que lo hacen pueden tener requisitos previos adicionales, como la instalación de los ensamblados de tipo espacial. Para obtener más información sobre la compatibilidad con proveedor de tipos espaciales se proporciona a continuación.  

Información adicional sobre cómo usar tipos espaciales en una aplicación puede encontrarse en dos tutoriales, uno para Code First y otro para Database First o Model First:  

- [Tipos de datos espaciales en el código en primer lugar](~/ef6/modeling/code-first/data-types/spatial.md)  
- [Tipos de datos espaciales en el Diseñador de EF](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Versiones EF que admiten tipos espaciales  

Compatibilidad con tipos espaciales se introdujo en EF5. Sin embargo, en EF5 tipos espaciales solo se admiten cuando la aplicación tiene como destino y se ejecuta en .NET 4.5.  

A partir de EF6 admiten los tipos espaciales para las aplicaciones destinadas a .NET 4 y .NET 4.5.  

## <a name="ef-providers-that-support-spatial-types"></a>Proveedores EF que admiten tipos espaciales  

### <a name="ef5"></a>EF5  

Los proveedores de Entity Framework para EF5 que somos conscientes de que son tipos espaciales de soporte técnico:  

- Proveedor de Microsoft SQL Server  
    - Este proveedor se incluye como parte de EF5.  
    - Este proveedor depende de algunas bibliotecas adicionales de bajo nivel que quizá deba instalarse: consulte a continuación para obtener más información.  
- [Devart dotConnect para Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Se trata de un proveedor de terceros de Devart.  

Si necesita un proveedor de EF5 que admite tipos espaciales, a continuación, por favor, ponga en contacto con y estaremos encantados de agregarlo a esta lista.  

### <a name="ef6"></a>EF6  

Los proveedores de Entity Framework para EF6 que somos conscientes de que son tipos espaciales de soporte técnico:  

- Proveedor de Microsoft SQL Server  
    - Este proveedor se incluye como parte de EF6.  
    - Este proveedor depende de algunas bibliotecas adicionales de bajo nivel que quizá deba instalarse: consulte a continuación para obtener más información.  
- [Devart dotConnect para Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Se trata de un proveedor de terceros de Devart.  

Si necesita un proveedor de EF6 que admite tipos espaciales, a continuación, por favor, ponga en contacto con y estaremos encantados de agregarlo a esta lista.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Requisitos previos para tipos espaciales con Microsoft SQL Server  

Compatibilidad espacial de SQL Server depende de los tipos específicos de SQL Server de bajo nivel, SqlGeography y SqlGeometry. Estos tipos residen en el ensamblado Microsoft.SqlServer.Types.dll, y no se distribuye este ensamblado como parte de EF o como parte de .NET Framework.  

Cuando se instala Visual Studio a menudo también instalará una versión de SQL Server, y esto incluye la instalación de Microsoft.SqlServer.Types.dll.  

Si SQL Server no está instalado en el equipo donde desea usar tipos espaciales o tipos espaciales se excluyeron de la instalación de SQL Server, deberá instalarlos manualmente. Los tipos se pueden instalar mediante `SQLSysClrTypes.msi`, que forma parte del Feature Pack de Microsoft SQL Server. Tipos espaciales son SQL Server específica de la versión, por lo que recomendamos [busque "Feature Pack de SQL Server"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) de Microsoft Download Center, a continuación, seleccione y descargue la opción que corresponda a la versión de SQL Server que se va a usar.
