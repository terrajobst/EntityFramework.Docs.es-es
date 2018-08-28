---
title: Seleccionar versión de Entity Framework en tiempo de ejecución para los modelos EF diseñadores - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998247"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="82c67-102">Seleccionar versión de Entity Framework en tiempo de ejecución para los modelos de diseñador de EF</span><span class="sxs-lookup"><span data-stu-id="82c67-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="82c67-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="82c67-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="82c67-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="82c67-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="82c67-105">A partir de EF6 se agregó la siguiente pantalla para EF Designer para que pueda seleccionar la versión del tiempo de ejecución que desea especificar al crear un modelo.</span><span class="sxs-lookup"><span data-stu-id="82c67-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="82c67-106">Si la versión más reciente de Entity Framework ya no está instalada en el proyecto, aparecerá la pantalla.</span><span class="sxs-lookup"><span data-stu-id="82c67-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="82c67-107">Si ya está instalada la versión más reciente solo se usará de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="82c67-107">If the latest version is already installed it will just be used by default.</span></span>

![Pantalla](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="82c67-109">Destinatarios EF6.x</span><span class="sxs-lookup"><span data-stu-id="82c67-109">Targeting EF6.x</span></span>

<span data-ttu-id="82c67-110">Puede elegir EF6 desde la pantalla 'Elegir su versión' para agregar el tiempo de ejecución de EF6 a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="82c67-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="82c67-111">Una vez haya agregado EF6, deberá detener viendo esta pantalla en el proyecto actual.</span><span class="sxs-lookup"><span data-stu-id="82c67-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="82c67-112">EF6 se deshabilitará si ya tiene una versión anterior de EF instalado (ya que no puede dirigirse a varias versiones del tiempo de ejecución desde el mismo proyecto).</span><span class="sxs-lookup"><span data-stu-id="82c67-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="82c67-113">Si no está habilitada la opción de EF6 a continuación, siga estos pasos para actualizar el proyecto a EF6:</span><span class="sxs-lookup"><span data-stu-id="82c67-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="82c67-114">Haga doble clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="82c67-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="82c67-115">Seleccione **actualizaciones**</span><span class="sxs-lookup"><span data-stu-id="82c67-115">Select **Updates**</span></span>
3.  <span data-ttu-id="82c67-116">Seleccione **EntityFramework** (asegúrese de que va a actualizar a la versión que desee)</span><span class="sxs-lookup"><span data-stu-id="82c67-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="82c67-117">Haga clic en **Update**</span><span class="sxs-lookup"><span data-stu-id="82c67-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="82c67-118">Destinatarios EF5.x</span><span class="sxs-lookup"><span data-stu-id="82c67-118">Targeting EF5.x</span></span>

<span data-ttu-id="82c67-119">Puede elegir EF5 desde la pantalla 'Elegir su versión' para agregar el tiempo de ejecución de EF5 a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="82c67-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="82c67-120">Una vez haya agregado EF5, aún verá la pantalla con la opción de EF6 deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="82c67-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="82c67-121">Si tiene una versión EF4.x del runtime instalada ya verá que la versión de EF que aparecen en la pantalla, en lugar de EF5.</span><span class="sxs-lookup"><span data-stu-id="82c67-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="82c67-122">En esta situación puede actualizar a EF5 mediante los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="82c67-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="82c67-123">Seleccione **herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="82c67-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="82c67-124">Ejecute **Install-Package EntityFramework-versión 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="82c67-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="82c67-125">Destinatarios EF4.x</span><span class="sxs-lookup"><span data-stu-id="82c67-125">Targeting EF4.x</span></span>

<span data-ttu-id="82c67-126">Puede instalar el tiempo de ejecución EF4.x al proyecto mediante los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="82c67-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="82c67-127">Seleccione **herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="82c67-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="82c67-128">Ejecute **Install-Package EntityFramework-versión 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="82c67-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
