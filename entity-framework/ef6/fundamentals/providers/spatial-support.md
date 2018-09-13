---
title: 'Compatibilidad con el proveedor de tipos espaciales: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489757"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="5fc6e-102">Proveedor de compatibilidad para tipos espaciales</span><span class="sxs-lookup"><span data-stu-id="5fc6e-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="5fc6e-103">Entity Framework admite trabajar con datos espaciales a través de las clases de DbGeography o DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="5fc6e-104">Estas clases se basan en la funcionalidad específica de la base de datos que ofrece el proveedor de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="5fc6e-105">No todos los proveedores admiten datos espaciales y aquellos que lo hacen pueden tener requisitos previos adicionales, como la instalación de los ensamblados de tipo espacial.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="5fc6e-106">Para obtener más información sobre la compatibilidad con proveedor de tipos espaciales se proporciona a continuación.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="5fc6e-107">Información adicional sobre cómo usar tipos espaciales en una aplicación puede encontrarse en dos tutoriales, uno para Code First y otro para Database First o Model First:</span><span class="sxs-lookup"><span data-stu-id="5fc6e-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="5fc6e-108">Tipos de datos espaciales en el código en primer lugar</span><span class="sxs-lookup"><span data-stu-id="5fc6e-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="5fc6e-109">Tipos de datos espaciales en el Diseñador de EF</span><span class="sxs-lookup"><span data-stu-id="5fc6e-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="5fc6e-110">Versiones EF que admiten tipos espaciales</span><span class="sxs-lookup"><span data-stu-id="5fc6e-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="5fc6e-111">Compatibilidad con tipos espaciales se introdujo en EF5.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="5fc6e-112">Sin embargo, en EF5 tipos espaciales solo se admiten cuando la aplicación tiene como destino y se ejecuta en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="5fc6e-113">A partir de EF6 admiten los tipos espaciales para las aplicaciones destinadas a .NET 4 y .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="5fc6e-114">Proveedores EF que admiten tipos espaciales</span><span class="sxs-lookup"><span data-stu-id="5fc6e-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="5fc6e-115">EF5</span><span class="sxs-lookup"><span data-stu-id="5fc6e-115">EF5</span></span>  

<span data-ttu-id="5fc6e-116">Los proveedores de Entity Framework para EF5 que somos conscientes de que son tipos espaciales de soporte técnico:</span><span class="sxs-lookup"><span data-stu-id="5fc6e-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5fc6e-117">Proveedor de Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5fc6e-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5fc6e-118">Este proveedor se incluye como parte de EF5.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="5fc6e-119">Este proveedor depende de algunas bibliotecas adicionales de bajo nivel que quizá deba instalarse: consulte a continuación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5fc6e-120">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="5fc6e-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5fc6e-121">Se trata de un proveedor de terceros de Devart.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5fc6e-122">Si necesita un proveedor de EF5 que admite tipos espaciales, a continuación, por favor, ponga en contacto con y estaremos encantados de agregarlo a esta lista.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="5fc6e-123">EF6</span><span class="sxs-lookup"><span data-stu-id="5fc6e-123">EF6</span></span>  

<span data-ttu-id="5fc6e-124">Los proveedores de Entity Framework para EF6 que somos conscientes de que son tipos espaciales de soporte técnico:</span><span class="sxs-lookup"><span data-stu-id="5fc6e-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5fc6e-125">Proveedor de Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5fc6e-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5fc6e-126">Este proveedor se incluye como parte de EF6.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="5fc6e-127">Este proveedor depende de algunas bibliotecas adicionales de bajo nivel que quizá deba instalarse: consulte a continuación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5fc6e-128">Devart dotConnect para Oracle</span><span class="sxs-lookup"><span data-stu-id="5fc6e-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5fc6e-129">Se trata de un proveedor de terceros de Devart.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5fc6e-130">Si necesita un proveedor de EF6 que admite tipos espaciales, a continuación, por favor, ponga en contacto con y estaremos encantados de agregarlo a esta lista.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="5fc6e-131">Requisitos previos para tipos espaciales con Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="5fc6e-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="5fc6e-132">Compatibilidad espacial de SQL Server depende de los tipos específicos de SQL Server de bajo nivel, SqlGeography y SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="5fc6e-133">Estos tipos residen en el ensamblado Microsoft.SqlServer.Types.dll, y no se distribuye este ensamblado como parte de EF o como parte de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="5fc6e-134">Cuando se instala Visual Studio a menudo también instalará una versión de SQL Server, y esto incluye la instalación de Microsoft.SqlServer.Types.dll.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="5fc6e-135">Si SQL Server no está instalado en el equipo donde desea usar tipos espaciales o tipos espaciales se excluyeron de la instalación de SQL Server, deberá instalarlos manualmente.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="5fc6e-136">Los tipos se pueden instalar mediante `SQLSysClrTypes.msi`, que forma parte del Feature Pack de Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="5fc6e-137">Tipos espaciales son SQL Server específica de la versión, por lo que recomendamos [busque "Feature Pack de SQL Server"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) de Microsoft Download Center, a continuación, seleccione y descargue la opción que corresponda a la versión de SQL Server que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="5fc6e-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
