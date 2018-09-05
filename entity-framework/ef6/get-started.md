---
title: 'Introducción a Entity Framework 6: EF6'
author: divega
ms.date: 2016-10-23
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
ms.openlocfilehash: 0087899d2fc65bc91ad36e35d5157e50b5c4bd2d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998089"
---
# <a name="get-started-with-entity-framework-6"></a>Introducción a Entity Framework 6

En esta guía se recopilan vínculos a artículos de documentación, tutoriales y vídeos seleccionados que le pueden ayudar a empezar rápidamente:

## <a name="get-entity-frameworkef6fundamentalsinstallmd"></a>[Obtener Entity Framework](~/ef6/fundamentals/install.md)
Aquí obtendrá información sobre cómo agregar Entity Framework a sus aplicaciones y, si quiere usar EF Designer, asegúrese de que lo instala en Visual Studio.

## <a name="creating-a-model-code-first-the-ef-designer-and-the-ef-workflowsef6modelingindexmd"></a>[Creación de un modelo: Code First, EF Designer y flujos de trabajo EF](~/ef6/modeling/index.md)
¿Prefiere especificar un código de escritura de modelos de EF o líneas y cuadros de dibujo?
¿Va a usar EF para asignar objetos a una base de datos existente o quiere que cree una base de datos específica para los objetos?
Aquí podrá obtener información sobre dos enfoques diferentes para usar EF6: EF Designer y Code First.
Asegúrese de seguir el debate y vea el vídeo sobre las diferencias.

## <a name="working-with-dbcontextef6fundamentalsworking-with-dbcontextmd"></a>[Trabajar con DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
DbContext es el primer tipo de EF y el más importante. Necesita conocerlo para saber cómo usarlo. Actúa como plataforma para consultas de bases de datos y mantiene un seguimiento de los cambios que hace a objetos, de modo que puedan persistir en la base de datos.

## <a name="ask-a-questionef6resourcesget-helpmd"></a>[Formular una pregunta](~/ef6/resources/get-help.md)
Descubra cómo obtener ayuda de expertos y contribuya con sus respuestas a la comunidad.

## <a name="contributehttpgithubcomaspnetentityframework6"></a>[Colaboracion](http://github.com/aspnet/EntityFramework6/)
Entity Framework 6 usa un modelo de desarrollo abierto. Descubra cómo puede ayudar a asegurarse de que EF sea aún mejor en nuestro repositorio de GitHub.

## <a name="index-of-walkthroughs"></a>Índice de tutoriales

- Code First
  - [Code First en un flujo de trabajo de base de datos existente](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First en un nuevo flujo de trabajo de base de datos](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Asignación de enumeraciones con Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Asignación de tipos espaciales con Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Creación de convenciones personalizadas e Code First](~/ef6/modeling/code-first/conventions/custom.md)
  - [Usar la configuración de Fluent de Code First con Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md)
  - [Code First Migrations in Team Environments](~/ef6/modeling/code-first/migrations/teams.md) (Migraciones de Code First en entornos de equipo)
  - [Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (Migraciones automáticas de Code First, ya no se recomienda)

- EF Designer
  - [Flujo de trabajo de Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Flujo de trabajo de Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Asignación de enumeraciones](~/ef6/modeling/designer/data-types/enums.md)
  - [Asignación de tipos espaciales](~/ef6/modeling/designer/data-types/spatial.md)
  - [Asignación de herencia de tabla por jerarquía](~/ef6/modeling/designer/inheritance/tph.md)
  - [Asignación de herencia de tabla por tipo](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Asignación de procedimientos almacenados para actualizaciones](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Asignación de procedimientos almacenados para consultas](~/ef6/modeling/designer/stored-procedures/query.md)
  - [División de entidades](~/ef6/modeling/designer/entity-splitting.md)
  - [División de tablas](~/ef6/modeling/designer/table-splitting.md)
  - [Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Definición de consultas, avanzado)
  - [Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Funciones con valores de tabla, avanzado)

- Aspectos básicos
  - [Async Query and Save](~/ef6/fundamentals/async.md) (Guardado y consultas asincrónicas)
  - [Databinding with WinForms](~/ef6/fundamentals/databinding/winforms.md) (Enlace de datos con WinForms)
  - [Databinding with WPF](~/ef6/fundamentals/databinding/wpf.md) (Enlace de datos con WPF)
  - [Escenarios desconectados con entidades de autoseguimiento](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (ya no se recomienda)
