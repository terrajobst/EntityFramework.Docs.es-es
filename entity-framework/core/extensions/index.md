---
title: Herramientas y extensiones - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 1edc7a20f54b2d26f899c93e98dfaf6d62c29f86
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490732"
---
# <a name="ef-core-tools--extensions"></a>Herramientas y extensiones de EF Core

Las herramientas y extensiones proporcionan funcionalidad adicional para Entity Framework Core.

> [!IMPORTANT]  
> Las extensiones se compilan a partir de diversos orígenes y no se mantienen como parte del proyecto Entity Framework Core. Al considerar una extensión de terceros, evalúe la calidad, las licencias, la compatibilidad, el soporte técnico, etc., a fin de asegurarse de que cumple sus requisitos.

## <a name="tools"></a>Herramientas

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro es una solución de modelado de entidad compatible con Entity Framework y Entity Framework Core. Permite definir fácilmente el modelo de entidad y asignarlo a la base de datos mediante Database First o Model First, de modo que pueda empezar a escribir consultas de inmediato.

[Sitio web](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer es un potente diseñador ORM para ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access y LINQ to SQL. Puede usar los enfoques Database First y Model First para diseñar el modelo ORM y generar el código de C# o Visual Basic .NET para él. Introduce nuevos enfoques para diseñar modelos ORM, aumenta la productividad y facilita el desarrollo de aplicaciones de base de datos.

[Sitio web](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Extensión de Visual Studio 2017+. Puede usar técnicas de ingeniería inversa de DbContext y clases POCO de una base de datos existente o un proyecto de SQL Server Database, y visualizar e inspeccionar DbContext de varias maneras.

[Wiki de GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Editor visual de Entity Framework

Una extensión de Visual Studio 2017 que agrega un diseñador ORM para el diseño visual de las clases de Entity Framework 6, Core 2.0 y Core 2.1. El código se genera mediante plantillas T4, por lo que se puede personalizar completamente para adaptarlo según las necesidades. Se admiten asociaciones de herencia, unidireccionales y bidireccionales, además de enumeraciones, la capacidad de asignar colores a las clases y la adición de bloques de texto para explicar posibles partes difíciles del diseño.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a>Extensiones

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Complemento de Microsoft.EntityFrameworkCore para admitir automáticamente el registro del historial de cambios de datos.

[Repositorio de GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Extensiones de LINQ dinámicas para Microsoft.EntityFrameworkCore que agrega compatibilidad asincrónica.

 [Repositorio de GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Intento de capturar algunos procedimientos recomendados en una API que admite las pruebas, incluido un pequeño marco de trabajo para buscar consultas de N+1.

[Repositorio de GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Biblioteca de almacenamiento en caché de segundo nivel. El almacenamiento en caché de segundo nivel es una caché de consulta. Los resultados de los comandos de EF se almacenarán en la memoria caché, para que los mismos comandos de EF recuperen los datos de la memoria caché en lugar de ejecutarlos de nuevo en la base de datos.

[Repositorio de GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Carga y guarda los gráficos de toda la entidad desasociada (la entidad con sus listas y entidades secundarias). Se inspira en [GraphDiff](https://github.com/refactorthis/GraphDiff/). La idea es agregar también algunos complementos para simplificar ciertas tareas repetitivas, como la auditoría y la paginación.

[Repositorio de GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Recupera la clave principal (incluidas las claves compuestas) de cualquier entidad como un diccionario.

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Contenedores de extensión reactiva para observables activos de entidades de Entity Framework.

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Agrega desencadenadores a las entidades con eventos de inserción, actualización y eliminación. Hay tres eventos para cada uno: antes, después y en caso de error.

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Obtiene acceso con tipo al valor OriginalValue de las propiedades de la entidad. Se admiten las propiedades simples y complejas, pero no la navegación ni las colecciones.

[Repositorio de GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco proporciona un generador de modelos inversos compatible con la pluralización/singularización y plantillas editables basadas en cadenas interpoladas de C# 6.0 que se ejecutan en .NET Core. También proporciona un generador de scripts de inicialización con scripts de mezcla de SQL y un ejecutor de scripts.

[Repositorio de GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore es un conjunto gratuito de extensiones para usuarios avanzados de LINQ to SQL y EntityFrameworkCore. Es compatible con Include(...) e IDbAsync.

[Repositorio de GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore proporciona extensiones útiles para usar proveedores LINQ como Entity Framework que solo admiten un subconjunto pequeño de funciones de .NET, reutilizar funciones, reescribir consultas (e incluso convertirlas en null-safe) y compilar consultas dinámicas mediante el uso de selectores y predicados traducibles.

[Repositorio de GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Complemento de Microsoft.EntityFrameworkCore para admitir el repositorio, el patrón de unidades de trabajo y varias base de datos con compatibilidad con transacciones distribuidas.

[Repositorio de GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Carga diferida para EF Core 1.1.

[Repositorio de GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Extensiones de EntityFrameworkCore para operaciones masivas (Insert, Update y Delete).

[Repositorio de GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Agrega pluralización de tiempo de diseño a EF Core.

[Repositorio de GitHub](https://github.com/bricelam/EFCore.Pluralizer)
