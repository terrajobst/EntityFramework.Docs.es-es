---
title: "Administración de esquemas de base de datos - EF Core"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a>Administración de esquemas de base de datos
EF Core proporciona dos métodos principales para mantener sincronizados el esquema de la base de datos y el modelo de EF Core. Para elegir entre los dos, decida si es el modelo de EF Core o el esquema de la base de datos el origen verdadero.

Si quiere que el modelo de EF Core sea el origen verdadero, use [Migraciones][1]. Al realizar cambios en el modelo de EF Core, este método aplica de forma incremental los cambios de esquema correspondientes a la base de datos para que siga siendo compatible con el modelo de EF Core.

Use [Ingeniería inversa][2] si quiere que el esquema de la base de datos sea el origen verdadero. Este método permite aplicar la técnica de scaffolding a un elemento DbContext y a las clases de tipo de entidad mediante la aplicación de ingeniería inversa al esquema de la base de datos de un modelo de EF Core.

> [!NOTE]
> Las [API de creación y eliminación][3] también pueden crear el esquema de la base de datos a partir del modelo de EF Core. Pero son principalmente para pruebas, creación de prototipos y otros escenarios donde la eliminación de la base de datos es aceptable.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
