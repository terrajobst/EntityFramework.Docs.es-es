---
title: Introducción rápida a Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913504"
---
# <a name="entity-framework-6-quick-overview"></a>Introducción rápida a Entity Framework 6

Entity Framework 6 (EF6) es un asignador relacional de objetos (O/RM) probado para .NET con muchos años de desarrollo de características y estabilización.

EF6 implementa muchas características de O/RM populares:
- Asignación de clases de entidad con "omisión de persistencia" (lo que también se conoce como "POCO", u objeto CLR estándar) que no depende de ningún tipo de EF
- Seguimiento de cambios automático
- Resolución de identidad y unidad de trabajo
- Carga diligente, diferida y explícita
- Traducción de consultas fuertemente tipadas con LINQ (Language Integrated Query) 
- Capacidades de asignación enriquecidas que incluyen compatibilidad con:
  - Herencia (tabla por jerarquía, tabla por tipo y tabla por clase concreta)
  - Tipos complejos
  - Procedimientos almacenados
- Un diseñador visual para crear modelos de entidad.
- Una experiencia de "Code First" que admite la creación de modelos de entidad mediante la escritura de código.
- Los modelos pueden generarse a partir de bases de datos existentes y luego editarse manualmente, o bien se pueden crear desde cero y luego usarse para generar nuevas bases de datos.
- Integración con modelos de aplicación de .NET Framework, incluido ASP.NET, y mediante enlace de datos, con WPF y WinForms.
- Conectividad de base de datos basada en ADO.NET y varios proveedores disponibles para conectarse a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.

Como O/RM, EF6 reduce la discordancia de impedancia entre los mundos relacionales y orientados a objetos, lo que permite a los desarrolladores escribir aplicaciones que interactúan con datos almacenados en bases de datos relacionales con objetos .NET fuertemente tipados que representan el dominio de la aplicación, y eliminar la necesidad de una gran parte del código de "mecánica" de acceso de datos que normalmente deben escribir.

## <a name="should-i-use-ef6-or-ef-core"></a>¿Debo usar EF6 o EF Core?

EF Core es una versión más moderna, ligera y extensible de Entity Framework que tiene capacidades y ventajas muy similares a EF6.
EF Core es una reescritura completa y contiene muchas características nuevas que no están disponibles en EF6, aunque todavía carece de algunas de las funcionalidades más avanzadas de asignación de EF6.
Se recomienda usar EF Core en las aplicaciones nuevas siempre que el conjunto de características se ajuste a los requisitos.
En [Comparar EF Core y EF6](xref:efcore-and-ef6/index) se examina el proceso de elección más detalladamente.

## <a name="get-startedef6get-startedmd"></a>[Primeros pasos](~/ef6/get-started.md)

Agregue el paquete NuGet de EntityFramework al proyecto o instale Entity Framework Tools para Visual Studio. Luego vea vídeos, lea tutoriales y documentación avanzada que le ayuden a sacar el máximo partido de Entity Framework 6.

## <a name="past-entity-framework-versions"></a>Versiones anteriores de Entity Framework

Esta es la documentación de la versión más reciente de Entity Framework 6, aunque la mayor parte también se aplica a las versiones anteriores.
Vea [Novedades](~/ef6/what-is-new/index.md) y [Versiones anteriores](~/ef6/what-is-new/past-releases.md) para obtener una lista completa de las versiones de EF y las características incluidas.
