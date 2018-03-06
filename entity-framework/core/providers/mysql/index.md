---
title: Proveedor de base de datos MySQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a>Proveedor de base de datos MySQL para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con MySQL. El proveedor se mantiene como parte del [proyecto MySQL](http://dev.mysql.com).

> [!WARNING]  
> Este proveedor es una versión preliminar.

> [!NOTE]  
> Este proveedor no se mantiene como parte del proyecto Entity Framework Core. Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.

## <a name="install"></a>Instalar

Instale el [paquete NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Primeros pasos

Vea [Starting with MySQL EF Core provider and Connector/Net 7.0.4 (Empezar a trabajar con el proveedor MySQL para EF Core y Connector/Net 7.0.4)](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* MySQL

## <a name="supported-platforms"></a>Plataformas compatibles

* .NET Framework (4.5.1 y superiores)

* Núcleo de .NET

No olvide consultar la documentación de MySQL para ver información de compatibilidad de versiones [aquí](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) y [aquí](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html).
