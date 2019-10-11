---
title: Herramientas y extensiones - EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 0c9671eb77181d85cd493341cd1abf842d13fb0e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181269"
---
# <a name="ef-core-tools--extensions"></a>Herramientas y extensiones de EF Core

Estas herramientas y extensiones proporcionan más funcionalidades para Entity Framework Core 2.0 y versiones posteriores.

> [!IMPORTANT]  
> Las extensiones están compiladas a partir de una gran variedad de orígenes, por lo que su mantenimiento no está incluido en el proyecto Entity Framework Core. En lo que respecta a las extensiones de terceros y para asegurarse de que cumplan sus requisitos, no se olvide de evaluar la calidad, las licencias, la compatibilidad, el soporte técnico, etc.

## <a name="tools"></a>Herramientas

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro es una solución de modelado de entidad compatible con Entity Framework y Entity Framework Core. Permite definir fácilmente el modelo de entidad y asignarlo a la base de datos mediante Database First o Model First, de modo que pueda empezar a escribir consultas de inmediato.

[Sitio web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer es un potente diseñador ORM para ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access y LINQ to SQL. Permite diseñar modelos EF Core de forma visual mediante los enfoques Database First y Model First, así como generar código C# y de Visual Basic. 

[Sitio web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools es una extensión de Visual Studio 2017 que expone varias tareas de tiempo de diseño de EF Core en una interfaz de usuario sencilla. Incluye técnicas de ingeniería inversa de DbContext y clases de entidades a partir de bases de datos existentes y [DACPAC de SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), así como la administración de la migración de bases de datos y la visualización de modelos.

[Wiki de GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Editor visual de Entity Framework

El Editor de objetos visuales de Entity Framework es una extensión de Visual Studio que agrega un diseñador ORM que permite generar objetos visuales de las clases EF 6 y EF Core. El código se genera mediante plantillas T4, por lo que se puede personalizar. Asimismo, admite enumeraciones y asociaciones de herencia, unidireccionales y bidireccionales, y permite asignar colores a las clases y agregar bloques de texto para explicar ciertas partes del diseño que puedan resultar difíciles de comprender.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory es un motor de scaffolding para .NET Core que permite automatizar la generación de clases DbContext, entidades, opciones de configuración de la asignación y clases de repositorios de una base de datos SQL Server.

[Repositorio de GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>LoreSoft's Entity Framework Core Generator

Entity Framework Core Generator (efg) es una herramienta de la CLI de .NET Core que permite generar modelos EF Core a partir de una base de datos existente. Es similar a `dotnet ef dbcontext scaffold`, pero también admite la [regeneración](https://efg.loresoft.com/en/latest/regeneration/) de código seguro mediante el reemplazo de la región o el análisis de los archivos de asignación. Asimismo, la herramienta permite generar modelos de vista, efectuar la validación y crear el código de los asignadores de objetos. 

[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentación](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Extensiones

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Biblioteca de extensiones que permite registrar automáticamente los cambios en los datos realizados por EF Core e incluirlos en una tabla a modo de historial.

[Repositorio de GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Puerto de .NET Core y .NET Standard de System.Linq.Dynamic que incluye compatibilidad asincrónica con EF Core.
Elemento System.Linq.Dynamic (muestra de Microsoft) en el que se ilustra cómo construir consultas LINQ de forma dinámica a partir de expresiones de cadena, en lugar de utilizar el código.

[Repositorio de GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Extensión que permite almacenar los resultados de las consultas EF Core en una caché de segundo nivel. De este modo, al volver a ejecutar las mismas cadenas, se pueden recuperar los datos directamente de la caché sin acceder a la base de datos.

[Repositorio de GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Esta biblioteca permite recuperar los valores de una clave principal (incluidas las claves compuestas) de una entidad como un diccionario.

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Esta biblioteca habilita el acceso fuertemente tipado a los valores originales de las propiedades de las entidades. 

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Generator Console) es un generador de código muy sencillo que se basa en un proyecto de consola. Para generar el código, se ejecuta en .NET Core y utiliza cadenas C# interpoladas. Geco incluye un generador de modelos de ingeniería inversa para EF Core que admite la pluralización, la singularización y las plantillas editables. También proporciona un generador de scripts de datos semilla, un ejecutor de scripts y una herramienta para limpiar las bases de datos.

[Repositorio de GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore es una versión de la biblioteca LINQKit compatible con EF Core. LINQKit es un conjunto de extensiones gratuito para los usuarios avanzados de LINQ to SQL y Entity Framework. Proporciona funcionalidades avanzadas como la creación dinámica de expresiones de predicado y el uso de variables de expresiones en las subconsultas.  

[Repositorio de GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq amplía las características de proveedores LINQ como Entity Framework para permitir la reutilización de las funciones, la reescritura de las consultas y la creación de consultas dinámicas mediante selectores y predicados traducibles.

[Repositorio de GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Extensión de Microsoft.EntityFrameworkCore para admitir los repositorios, los patrones de unidades de trabajo y varias bases de datos compatibles con las transacciones distribuidas.

[Repositorio de GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensiones de EF Core para operaciones masivas (Insert, Update y Delete).

[Repositorio de GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Agrega pluralización de tiempo de diseño a EF Core.

[Repositorio de GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

Método de extensión sencillo que obtiene la instrucción SQL que EF Core generaría para una consulta LINQ determinada en casos simples. El uso del método ToSql está limitado a casos simples, dado que EF Core puede generar más de una instrucción SQL para una única consulta LINQ, así como distintas instrucciones SQL en función de los valores de los parámetros.

[Repositorio de GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Recuperación del atributo [Index] para EF Core (con extensión para la creación de modelos).

[Repositorio de GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Proporciona un contenedor alrededor del proveedor de la base de datos en memoria de EF Core. Así, hace que actúe de una forma más similar a la de un proveedor relacional.

[Repositorio de GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Implementación de compatibilidad temporal para EF Core.

[Repositorio de GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Caché de consulta de segundo nivel y alto rendimiento para EF Core.

[Repositorio de GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Amplía su DbContext con características como: Incluir filtro, Auditoría, Cache, Consulta de futuro, Batch Delete, Actualización por lotes y más.

[Sitio web](https://entityframework-plus.net/)
[Repositorio de GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Extensiones de Entity Framework

Extiende su DbContext con operaciones masivas de alto rendimiento: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge y más.

[Sitio web](https://entityframework-extensions.net/)

### <a name="reconciler"></a>Reconciliador

Actualice un gráfico de entidades en el almacén a uno específico mediante la inserción, actualización y eliminación de las entidades correspondientes.

[Repositorio de GitHub](https://github.com/jtheisen/reconciler)
