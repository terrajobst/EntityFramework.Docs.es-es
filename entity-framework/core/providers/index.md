---
title: Proveedores de bases de datos - EF Core
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 6f058698f78c787fc6c313486874b0af2183f97a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949321"
---
# <a name="database-providers"></a>Proveedores de bases de datos

Entity Framework Core puede tener acceso a muchas bases de datos diferentes a través de bibliotecas de complementos denominadas proveedores de bases de datos.

## <a name="current-providers"></a>Proveedores actuales
> [!IMPORTANT]  
> Los proveedores de EF Core se componen de una serie de orígenes. No todos los proveedores se mantienen como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore). Al considerar un proveedor, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos. Además, asegúrese de revisar la documentación de cada proveedor para obtener información detallada de compatibilidad de versiones.

| Paquete NuGet                                                                                                        | Motores de base de datos compatibles | Mantenedor o proveedor                                                           | Notas o requisitos           | Vínculos útiles                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:-------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | De SQL Server 2008 en adelante    | [Proyecto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Documentación](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | De SQLite 3.7 en adelante         | [Proyecto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Documentación](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Base de datos en memoria de EF Core | [Proyecto EF Core](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Solo para pruebas               | [Documentación](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Equipo de desarrollo de Npgsql](https://github.com/npgsql)                          |                                | [Documentación](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Proyecto Pomelo Foundation](https://github.com/PomeloFoundation)              |                                | [Archivo Léame](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | Servidor MyCAT               | [Proyecto Pomelo Foundation](https://github.com/PomeloFoundation)              | Versión preliminar, hasta EF Core 1.1 | [Archivo Léame](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3,5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [Proyecto MySQL](http://dev.mysql.com) (Oracle)                                | Versión preliminar                    | [Documentación](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 y 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | De EF Core 2.0 en adelante            | [Documentación](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 y 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | De EF Core 2.0 en adelante            | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versión de Windows                | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versión de Linux                  | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Versión de macOS                  | [blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | De Oracle 9.2.0.4 en adelante     | [DevArt](https://www.devart.com/)                                             | Pagado                           | [Documentación](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | De PostgreSQL 8.0 en adelante     | [DevArt](https://www.devart.com/)                                             | Pagado                           | [Documentación](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | De SQLite 3 en adelante           | [DevArt](https://www.devart.com/)                                             | Pagado                           | [Documentación](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | De MySQL 5 en adelante            | [DevArt](https://www.devart.com/)                                             | Pagado                           | [Documentación](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Archivos de Microsoft Access     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, .NET Framework    | [Archivo Léame](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Proveedores futuros

### <a name="cosmos-db"></a>Cosmos DB

Hemos desarrollado un proveedor de EF Core para la API de DocumentDB de Cosmos DB. Será el primer proveedor de bases de datos completas y orientadas a documentos que hayamos elaborado y lo aprendido en este ejercicio se traducirá en mejoras en el diseño de la versión posterior a 2.1. El plan actual es publicar una versión preliminar del proveedor en el período de tiempo de la versión 2.1.

### <a name="oracle"></a>Oracle
El equipo de Oracle .NET ha anunciado que tienen previsto publicar un proveedor propio para EF Core 2.0 aproximadamente en el tercer trimestre de 2018. Vea su [declaración de intenciones para .NET Core y Entity Framework Core](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) para obtener más información.
Dirija las preguntas sobre este proveedor, incluida la escala de tiempo de la versión, al [sitio de la comunidad de Oracle](https://community.oracle.com/).

Mientras tanto, el equipo de EF ha generado un [proveedor de EF Core de ejemplo para las bases de datos de Oracle](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). La finalidad del proyecto no es generar un proveedor de EF Core propiedad de Microsoft, sino ayudarnos a identificar vacíos en la funcionalidad base y relacional de EF Core que hemos de abordar para mejorar la compatibilidad con Oracle e iniciar el desarrollo de otros proveedores de Oracle para EF Core con Oracle o con terceros.

Se tendrán en cuenta las contribuciones que mejoren la implementación de ejemplo. También se agradecerá y apoyará cualquier esfuerzo de la comunidad para crear un proveedor de Oracle de código abierto para EF Core, tomando el ejemplo como punto de partida.

## <a name="adding-a-database-provider-to-your-application"></a>Agregar un proveedor de bases de datos a la aplicación

La mayoría de los proveedores de bases de datos para EF Core se distribuyen como paquetes NuGet. Esto significa que pueden instalarse con la herramienta `dotnet` de la línea de comandos:

``` console
dotnet add package provider_package_name
```

O en Visual Studio, con la Consola del administrador de paquetes de NuGet:

``` powershell
install-package provider_package_name
```

Una vez instalado, se configurará el proveedor en su `DbContext`, ya sea en el método `OnConfiguring` o el método `AddDbContext` si se usa un contenedor de inserción de dependencias. Por ejemplo, la línea siguiente configura el proveedor de SQL Server con la cadena de conexión pasada:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Los proveedores de bases de datos pueden ampliar EF Core para habilitar una funcionalidad única para bases de datos específicas. Algunos conceptos son comunes a la mayoría de las bases de datos y se incluyen en los componentes principales de EF Core. Estos conceptos incluyen la expresión de consultas en LINQ, las transacciones y el seguimiento de cambios en objetos una vez cargados desde la base de datos. Algunos conceptos son específicos de un proveedor determinado. Por ejemplo, el proveedor de SQL Server permite [configurar tablas optimizadas para memoria](xref:core/providers/sql-server/memory-optimized-tables) (una característica específica de SQL Server). Otros conceptos son específicos de una clase de proveedores. Por ejemplo, los proveedores de EF Core para bases de datos relacionales se basan en la biblioteca común `Microsoft.EntityFrameworkCore.Relational`, que proporciona API para configurar asignaciones de tabla y columna, restricciones de clave externa, etc. Los proveedores normalmente se distribuyen como paquetes NuGet.

> [!IMPORTANT]  
> Cuando se publica una nueva versión de revisión de EF Core, suele incluir actualizaciones del paquete `Microsoft.EntityFrameworkCore.Relational`. Cuando se agrega un proveedor de bases de datos relacional, este paquete se convierte en una dependencia transitiva de la aplicación. Pero muchos proveedores se publican independientemente de EF Core y es posible que no puedan actualizarse para que se basen en la versión de revisión más reciente de ese paquete. A fin de asegurarse de que obtendrá todas las correcciones de errores, se recomienda agregar la versión de revisión de `Microsoft.EntityFrameworkCore.Relational` como dependencia directa de la aplicación.
