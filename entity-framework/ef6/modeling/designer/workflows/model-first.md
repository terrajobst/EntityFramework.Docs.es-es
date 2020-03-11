---
title: Model First EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414960"
---
# <a name="model-first"></a><span data-ttu-id="3562c-102">Model First</span><span class="sxs-lookup"><span data-stu-id="3562c-102">Model First</span></span>
<span data-ttu-id="3562c-103">Este vídeo y el tutorial paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3562c-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="3562c-104">Model First permite crear un nuevo modelo mediante el Entity Framework Designer y, a continuación, generar un esquema de base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="3562c-105">El modelo se almacena en un archivo EDMX (extensión. EDMX) y se puede ver y editar en el Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="3562c-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="3562c-106">Las clases con las que interactúa en la aplicación se generan automáticamente a partir del archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="3562c-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3562c-107">Visualización del vídeo</span><span class="sxs-lookup"><span data-stu-id="3562c-107">Watch the video</span></span>
<span data-ttu-id="3562c-108">Este vídeo y el tutorial paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3562c-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="3562c-109">Model First permite crear un nuevo modelo mediante el Entity Framework Designer y, a continuación, generar un esquema de base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="3562c-110">El modelo se almacena en un archivo EDMX (extensión. EDMX) y se puede ver y editar en el Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="3562c-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="3562c-111">Las clases con las que interactúa en la aplicación se generan automáticamente a partir del archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="3562c-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="3562c-112">**Presentado por**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="3562c-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="3562c-113">**Vídeo**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="3562c-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3562c-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3562c-114">Pre-Requisites</span></span>

<span data-ttu-id="3562c-115">Para completar este tutorial, deberá tener instalados Visual Studio 2010 o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3562c-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="3562c-116">Si usa Visual Studio 2010, también debe tener instalado [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="3562c-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="3562c-117">1. crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="3562c-117">1. Create the Application</span></span>

<span data-ttu-id="3562c-118">Para simplificar las cosas, vamos a crear una aplicación de consola básica que use el Model First para realizar el acceso a los datos:</span><span class="sxs-lookup"><span data-stu-id="3562c-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="3562c-119">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3562c-119">Open Visual Studio</span></span>
-   <span data-ttu-id="3562c-120">**Archivo-&gt; nuevo proyecto de&gt;...**</span><span class="sxs-lookup"><span data-stu-id="3562c-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="3562c-121">Seleccionar **ventanas** en el menú izquierdo y en la **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="3562c-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="3562c-122">Escriba **ModelFirstSample** como nombre</span><span class="sxs-lookup"><span data-stu-id="3562c-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="3562c-123">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3562c-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="3562c-124">2. crear modelo</span><span class="sxs-lookup"><span data-stu-id="3562c-124">2. Create Model</span></span>

<span data-ttu-id="3562c-125">Vamos a hacer uso de Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="3562c-126">**Proyecto-&gt; agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="3562c-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="3562c-127">Seleccione **datos** en el menú de la izquierda y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="3562c-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="3562c-128">Escriba **BloggingModel** como nombre y haga clic en **Aceptar**. Esto iniciará el Asistente para Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="3562c-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="3562c-129">Seleccione **modelo vacío** y haga clic en **Finalizar** .</span><span class="sxs-lookup"><span data-stu-id="3562c-129">Select **Empty Model** and click **Finish**</span></span>

    ![Crear modelo vacío](~/ef6/media/createemptymodel.png)

<span data-ttu-id="3562c-131">El Entity Framework Designer se abre con un modelo en blanco.</span><span class="sxs-lookup"><span data-stu-id="3562c-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="3562c-132">Ahora podemos empezar a agregar entidades, propiedades y asociaciones al modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="3562c-133">Haga clic con el botón derecho en la superficie de diseño y seleccione **propiedades** .</span><span class="sxs-lookup"><span data-stu-id="3562c-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="3562c-134">En el ventana Propiedades cambie el **nombre del contenedor de entidades** a **BloggingContext**
    *este es el nombre del contexto derivado que se generará automáticamente, el contexto representa una sesión con la base de datos, lo que nos permite consultar y guardar datos* .</span><span class="sxs-lookup"><span data-stu-id="3562c-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="3562c-135">Haga clic con el botón derecho en la superficie de diseño y seleccione **Agregar nueva&gt; entidad..** .</span><span class="sxs-lookup"><span data-stu-id="3562c-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="3562c-136">Escriba **blog** como el nombre de la entidad y **BlogId** como nombre de la clave y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="3562c-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Agregar entidad de blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="3562c-138">Haga clic con el botón derecho en la nueva entidad en la superficie de diseño y seleccione **Agregar nueva-&gt; propiedad escalar** **, escriba el nombre de** la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3562c-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="3562c-139">Repita este proceso para agregar una propiedad de **dirección URL** .</span><span class="sxs-lookup"><span data-stu-id="3562c-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="3562c-140">Haga clic con el botón derecho en la propiedad **dirección URL** en la superficie de diseño y seleccione **propiedades**, en el ventana Propiedades cambie la configuración que **acepta valores NULL** a **true**
    *esto nos permite guardar un blog en la base de datos sin asignarle una dirección URL* .</span><span class="sxs-lookup"><span data-stu-id="3562c-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="3562c-141">Con las técnicas que acaba de aprender, agregue una entidad **post** con una propiedad de clave **PostId**</span><span class="sxs-lookup"><span data-stu-id="3562c-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="3562c-142">Agregar propiedades escalares de **título** y de **contenido** a la entidad **post**</span><span class="sxs-lookup"><span data-stu-id="3562c-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="3562c-143">Ahora que tenemos un par de entidades, es el momento de agregar una asociación (o relación) entre ellas.</span><span class="sxs-lookup"><span data-stu-id="3562c-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="3562c-144">Haga clic con el botón derecho en la superficie de diseño y seleccione **Agregar nueva&gt; asociación..** .</span><span class="sxs-lookup"><span data-stu-id="3562c-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="3562c-145">Haga que un extremo del punto de relación se envíe a un **blog** con una multiplicidad de **uno** y el otro extremo para **publicar** con una multiplicidad de **muchos**
    *Esto significa que un blog tiene muchas publicaciones y una publicación pertenece a un blog* .</span><span class="sxs-lookup"><span data-stu-id="3562c-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="3562c-146">Asegúrese de que el cuadro de la **entidad agregar propiedades de clave externa al "post"** está activado y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="3562c-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Agregar Asociación MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="3562c-148">Ahora tenemos un modelo simple en el que se puede generar una base de datos y se usa para leer y escribir datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="3562c-150">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="3562c-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="3562c-151">Si está trabajando en Visual Studio 2010, hay algunos pasos adicionales que debe seguir para actualizar a la versión más reciente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3562c-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="3562c-152">La actualización es importante porque le proporciona acceso a una superficie de API mejorada, que es mucho más fácil de usar, así como las correcciones de errores más recientes.</span><span class="sxs-lookup"><span data-stu-id="3562c-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="3562c-153">En primer lugar, necesitamos obtener la versión más reciente de Entity Framework de NuGet.</span><span class="sxs-lookup"><span data-stu-id="3562c-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="3562c-154">**Proyecto:&gt; administrar paquetes NuGet...** 
    *si no tiene la opción **administrar paquetes Nuget...** , debe instalar la [versión más reciente de Nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*</span><span class="sxs-lookup"><span data-stu-id="3562c-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="3562c-155">Seleccione la pestaña **en línea**</span><span class="sxs-lookup"><span data-stu-id="3562c-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="3562c-156">Seleccione el paquete **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3562c-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="3562c-157">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="3562c-157">Click **Install**</span></span>

<span data-ttu-id="3562c-158">A continuación, necesitamos cambiar nuestro modelo para generar código que haga uso de la API de DbContext, que se presentó en versiones posteriores de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3562c-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="3562c-159">Haga clic con el botón derecho en una zona vacía del modelo en el diseñador de EF y seleccione **Agregar elemento de generación de código..** .</span><span class="sxs-lookup"><span data-stu-id="3562c-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="3562c-160">Seleccione **plantillas en línea** en el menú de la izquierda y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="3562c-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="3562c-161">Seleccione el **generador de DbContext de EF 5. x para C\#** , escriba **BloggingModel** como nombre y haga clic en **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="3562c-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Plantilla DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="3562c-163">3. generar la base de datos</span><span class="sxs-lookup"><span data-stu-id="3562c-163">3. Generating the Database</span></span>

<span data-ttu-id="3562c-164">Dado nuestro modelo, Entity Framework puede calcular un esquema de base de datos que nos permitirá almacenar y recuperar datos mediante el modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="3562c-165">El servidor de base de datos que se instala con Visual Studio es diferente en función de la versión de Visual Studio que haya instalado:</span><span class="sxs-lookup"><span data-stu-id="3562c-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="3562c-166">Si usa Visual Studio 2010, va a crear una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="3562c-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="3562c-167">Si usa Visual Studio 2012, va a crear una base de datos de [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="3562c-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="3562c-168">Vamos a generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="3562c-169">Haga clic con el botón secundario en la superficie de diseño y seleccione **generar base de datos del modelo..** .</span><span class="sxs-lookup"><span data-stu-id="3562c-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="3562c-170">Haga clic en **nueva conexión..** .</span><span class="sxs-lookup"><span data-stu-id="3562c-170">Click **New Connection…**</span></span> <span data-ttu-id="3562c-171">y especifique LocalDB o SQL Express, en función de la versión de Visual Studio que use, escriba **ModelFirst. blog** como nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Conexión de LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexión de SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="3562c-174">Seleccione **Aceptar** y se le preguntará si desea crear una nueva base de datos, seleccione **sí** .</span><span class="sxs-lookup"><span data-stu-id="3562c-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="3562c-175">Seleccione **siguiente** y el Entity Framework Designer calculará un script para crear el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="3562c-176">Una vez que se muestre el script, haga clic en **Finalizar** y el script se agregará al proyecto y se abrirá.</span><span class="sxs-lookup"><span data-stu-id="3562c-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="3562c-177">Haga clic con el botón derecho en el script y seleccione **Ejecutar**; se le pedirá que especifique la base de datos a la que se va a conectar, especifique LocalDB o SQL Server Express, en función de la versión de Visual Studio que use.</span><span class="sxs-lookup"><span data-stu-id="3562c-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="3562c-178">4. leer & escribir datos</span><span class="sxs-lookup"><span data-stu-id="3562c-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="3562c-179">Ahora que tenemos un modelo, es el momento de usarlo para tener acceso a algunos datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="3562c-180">Las clases que se van a usar para obtener acceso a los datos se generan automáticamente basándose en el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="3562c-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="3562c-181">*Esta captura de pantalla es de Visual Studio 2012, si usa Visual Studio 2010, los archivos BloggingModel.tt y BloggingModel.Context.tt estarán directamente en el proyecto en lugar de anidados en el archivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="3562c-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Clases generadas](~/ef6/media/generatedclasses.png)

<span data-ttu-id="3562c-183">Implemente el método Main en Program.cs como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3562c-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="3562c-184">Este código crea una nueva instancia de nuestro contexto y, a continuación, la usa para insertar un nuevo blog.</span><span class="sxs-lookup"><span data-stu-id="3562c-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="3562c-185">A continuación, usa una consulta LINQ para recuperar todos los blogs de la base de datos ordenados alfabéticamente por título.</span><span class="sxs-lookup"><span data-stu-id="3562c-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="3562c-186">Ahora puede ejecutar la aplicación y probarla.</span><span class="sxs-lookup"><span data-stu-id="3562c-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="3562c-187">5. tratar con cambios en el modelo</span><span class="sxs-lookup"><span data-stu-id="3562c-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="3562c-188">Ahora es el momento de realizar algunos cambios en nuestro modelo, cuando realizamos estos cambios también necesitamos actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="3562c-189">Comenzaremos agregando una nueva entidad de usuario a nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="3562c-190">Agregue un nuevo nombre de entidad de **usuario** **con el** nombre de usuario como el nombre de clave y la **cadena** como el tipo de propiedad de la clave.</span><span class="sxs-lookup"><span data-stu-id="3562c-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Agregar entidad de usuario](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="3562c-192">Haga clic con el botón derecho en la propiedad **username** en la superficie de diseño y seleccione **propiedades**, en el ventana Propiedades cambie la configuración de **MaxLength** a **50**
    *esto restringe los datos que se pueden almacenar en el nombre de usuario a 50 caracteres* .</span><span class="sxs-lookup"><span data-stu-id="3562c-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="3562c-193">Agregar una propiedad escalar **displayName** a la entidad **User**</span><span class="sxs-lookup"><span data-stu-id="3562c-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="3562c-194">Ahora tenemos un modelo actualizado y estamos preparados para actualizar la base de datos para dar cabida a nuestro nuevo tipo de entidad de usuario.</span><span class="sxs-lookup"><span data-stu-id="3562c-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="3562c-195">Haga clic con el botón secundario en la superficie de diseño y seleccione **generar base de datos a partir del modelo...** , Entity Framework calculará un script para volver a crear un esquema basado en el modelo actualizado.</span><span class="sxs-lookup"><span data-stu-id="3562c-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="3562c-196">Haga clic en **Finish** (Finalizar).</span><span class="sxs-lookup"><span data-stu-id="3562c-196">Click **Finish**</span></span>
-   <span data-ttu-id="3562c-197">Puede recibir advertencias sobre cómo sobrescribir el script DDL existente y las partes de asignación y almacenamiento del modelo; haga clic en **sí** para ambas advertencias.</span><span class="sxs-lookup"><span data-stu-id="3562c-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="3562c-198">El script SQL actualizado para crear la base de datos se abre automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3562c-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="3562c-199">*El script que se genera quitará todas las tablas existentes y, a continuación, volverá a crear el esquema desde el principio. Esto puede funcionar para el desarrollo local, pero no es viable para insertar cambios en una base de datos que ya se ha implementado. Si necesita publicar cambios en una base de datos que ya se ha implementado, deberá editar el script o usar una herramienta de comparación de esquemas para calcular un script de migración.*</span><span class="sxs-lookup"><span data-stu-id="3562c-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="3562c-200">Haga clic con el botón derecho en el script y seleccione **Ejecutar**; se le pedirá que especifique la base de datos a la que se va a conectar, especifique LocalDB o SQL Server Express, en función de la versión de Visual Studio que use.</span><span class="sxs-lookup"><span data-stu-id="3562c-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="3562c-201">Resumen</span><span class="sxs-lookup"><span data-stu-id="3562c-201">Summary</span></span>

<span data-ttu-id="3562c-202">En este tutorial, hemos visto Model First desarrollo, que nos permitió crear un modelo en EF Designer y, a continuación, generar una base de datos a partir de ese modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="3562c-203">Después usamos el modelo para leer y escribir algunos datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3562c-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="3562c-204">Por último, hemos actualizado el modelo y, a continuación, hemos recreado el esquema de la base de datos para que coincida con el modelo.</span><span class="sxs-lookup"><span data-stu-id="3562c-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
