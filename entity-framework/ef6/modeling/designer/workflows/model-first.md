---
title: Model First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 3dd0eba29619f09995d7009dd29462c14bde98c4
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251146"
---
# <a name="model-first"></a><span data-ttu-id="8a00b-102">En primer lugar del modelo</span><span class="sxs-lookup"><span data-stu-id="8a00b-102">Model First</span></span>
<span data-ttu-id="8a00b-103">En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="8a00b-104">Modelo primero le permite crear un nuevo modelo con Entity Framework Designer y, a continuación, generar un esquema de base de datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="8a00b-105">El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="8a00b-106">Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="8a00b-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="8a00b-107">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="8a00b-107">Watch the video</span></span>
<span data-ttu-id="8a00b-108">En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="8a00b-109">Modelo primero le permite crear un nuevo modelo con Entity Framework Designer y, a continuación, generar un esquema de base de datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="8a00b-110">El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="8a00b-111">Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="8a00b-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="8a00b-112">**Presentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="8a00b-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="8a00b-113">**Vídeo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="8a00b-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="8a00b-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8a00b-114">Pre-Requisites</span></span>

<span data-ttu-id="8a00b-115">Necesitará tener Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="8a00b-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="8a00b-116">Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="8a00b-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="8a00b-117">1. Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="8a00b-117">1. Create the Application</span></span>

<span data-ttu-id="8a00b-118">Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Model First para tener acceso a datos:</span><span class="sxs-lookup"><span data-stu-id="8a00b-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="8a00b-119">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a00b-119">Open Visual Studio</span></span>
-   <span data-ttu-id="8a00b-120">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="8a00b-121">Seleccione **Windows** en el menú izquierdo y **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="8a00b-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="8a00b-122">Escriba **ModelFirstSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="8a00b-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="8a00b-123">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8a00b-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="8a00b-124">2. Crear modelo</span><span class="sxs-lookup"><span data-stu-id="8a00b-124">2. Create Model</span></span>

<span data-ttu-id="8a00b-125">Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="8a00b-126">**Proyecto -&gt; Agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="8a00b-127">Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="8a00b-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="8a00b-128">Escriba **BloggingModel** como el nombre y haga clic en **Aceptar**, esto inicia el Asistente para Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="8a00b-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="8a00b-129">Seleccione **modelo vacío** y haga clic en **finalizar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-129">Select **Empty Model** and click **Finish**</span></span>

    ![Crear un modelo vacío](~/ef6/media/createemptymodel.png)

<span data-ttu-id="8a00b-131">Se abre el Diseñador de Entity Framework con un modelo en blanco.</span><span class="sxs-lookup"><span data-stu-id="8a00b-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="8a00b-132">Ahora podemos comenzar a agregar entidades, propiedades y asociaciones en el modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="8a00b-133">Haga doble clic en la superficie de diseño y seleccione **propiedades**</span><span class="sxs-lookup"><span data-stu-id="8a00b-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="8a00b-134">En el cambio de la ventana de propiedades el **Entity Container Name** a **BloggingContext**
    *es el nombre del contexto derivado que se generarán para usted, el contexto Representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos*</span><span class="sxs-lookup"><span data-stu-id="8a00b-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="8a00b-135">Haga doble clic en la superficie de diseño y seleccione **Add New -&gt; entidad...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="8a00b-136">Escriba **Blog** como el nombre de entidad y **BlogId** como el nombre de clave y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Agregar entidad Blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="8a00b-138">Haga doble clic en la nueva entidad en la superficie de diseño y seleccione **Add New -&gt; propiedad escalar**, escriba **nombre** como el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="8a00b-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="8a00b-139">Repita este proceso para agregar un **Url** propiedad.</span><span class="sxs-lookup"><span data-stu-id="8a00b-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="8a00b-140">Haga doble clic en el **Url** propiedad en la superficie de diseño y seleccione **propiedades**, en el cambio de la ventana de propiedades el **Nullable** si se establece en **True** 
     *Esto nos permite guardar un Blog en la base de datos sin asignarle una dirección Url*</span><span class="sxs-lookup"><span data-stu-id="8a00b-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="8a00b-141">Mediante las técnicas que acaba de aprender, agregue un **Post** entidad con un **PostId** propiedad clave</span><span class="sxs-lookup"><span data-stu-id="8a00b-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="8a00b-142">Agregar **título** y **contenido** las propiedades escalares del **Post** entidad</span><span class="sxs-lookup"><span data-stu-id="8a00b-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="8a00b-143">Ahora que tenemos un par de entidades, es momento de agregar una asociación (o una relación) entre ellos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="8a00b-144">Haga doble clic en la superficie de diseño y seleccione **Add New -&gt; asociación...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="8a00b-145">Hacer que apunte a un extremo de la relación **Blog** con una multiplicidad de **una** y el otro punto final para **Post** con una multiplicidad de **muchos** 
     *Esto significa que un Blog tiene todas las entradas y una entrada pertenezca a un Blog*</span><span class="sxs-lookup"><span data-stu-id="8a00b-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="8a00b-146">Asegúrese del **agregar propiedades de clave externa a la entidad 'Post'** está activada y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Agregar asociación MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="8a00b-148">Ahora tenemos un modelo simple que podemos usar para leer y escribir datos y generar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="8a00b-150">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="8a00b-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="8a00b-151">Si está trabajando en Visual Studio 2010, hay algunos pasos adicionales que debe seguir para actualizar a la versión más reciente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="8a00b-152">Actualizar es importante porque proporciona acceso a una superficie de API mejorada, que es mucho más fácil de usar, así como las correcciones de errores más recientes.</span><span class="sxs-lookup"><span data-stu-id="8a00b-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="8a00b-153">En primer lugar, necesitamos obtener la versión más reciente de Entity Framework en NuGet.</span><span class="sxs-lookup"><span data-stu-id="8a00b-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="8a00b-154">\*\*Proyecto –&gt; administrar paquetes NuGet... \*\* 
     \*Si no tiene el \**administrar paquetes NuGet... \*\* opción, debe instalar la [versión más reciente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="8a00b-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="8a00b-155">Seleccione el **Online** ficha</span><span class="sxs-lookup"><span data-stu-id="8a00b-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="8a00b-156">Seleccione el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="8a00b-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="8a00b-157">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-157">Click **Install**</span></span>

<span data-ttu-id="8a00b-158">A continuación, se debe intercambiar nuestro modelo para generar código que hace uso de la API de DbContext, que se introdujo en versiones posteriores de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8a00b-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="8a00b-159">Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="8a00b-160">Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="8a00b-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="8a00b-161">Seleccione el EF **5.x generador de DbContext para C\#**, escriba **BloggingModel** como el nombre y haga clic en **agregar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Plantilla de DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="8a00b-163">3. Generar la base de datos</span><span class="sxs-lookup"><span data-stu-id="8a00b-163">3. Generating the Database</span></span>

<span data-ttu-id="8a00b-164">Dado que nuestro modelo, Entity Framework puede calcular un esquema de base de datos que nos permitirá almacenar y recuperar datos mediante el modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="8a00b-165">El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:</span><span class="sxs-lookup"><span data-stu-id="8a00b-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="8a00b-166">Si usa Visual Studio 2010 se creará una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="8a00b-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="8a00b-167">Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="8a00b-168">Sigamos adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="8a00b-169">Haga doble clic en la superficie de diseño y seleccione **generar base de datos de modelo...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="8a00b-170">Haga clic en **nueva conexión...**</span><span class="sxs-lookup"><span data-stu-id="8a00b-170">Click **New Connection…**</span></span> <span data-ttu-id="8a00b-171">y especifique LocalDB o Express de SQL, según la versión de Visual Studio que está utilizando, escriba **ModelFirst.Blogging** como el nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Conexión de LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexión de SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="8a00b-174">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="8a00b-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="8a00b-175">Seleccione **siguiente** y Entity Framework Designer calculará una secuencia de comandos para crear el esquema de base de datos</span><span class="sxs-lookup"><span data-stu-id="8a00b-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="8a00b-176">Una vez que se muestra la secuencia de comandos, haga clic en **finalizar** y se agregará al proyecto y abrir la secuencia de comandos</span><span class="sxs-lookup"><span data-stu-id="8a00b-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="8a00b-177">Haga doble clic en el script y seleccione **Execute**, se le pedirá que especifique la base de datos para conectarse a, especifique LocalDB o Express de SQL Server, según la versión de Visual Studio que está utilizando</span><span class="sxs-lookup"><span data-stu-id="8a00b-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="8a00b-178">4. Leer y escribir datos</span><span class="sxs-lookup"><span data-stu-id="8a00b-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="8a00b-179">Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="8a00b-180">Las clases que vamos a usar para acceder a los datos se generan automáticamente para usted según el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="8a00b-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="8a00b-181">*Esta captura de pantalla es de Visual Studio 2012, si usa Visual Studio 2010 el BloggingModel.tt y BloggingModel.Context.tt archivos estarán directamente en el proyecto en lugar de anidan en el archivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="8a00b-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Clases generadas](~/ef6/media/generatedclasses.png)

<span data-ttu-id="8a00b-183">Como se muestra a continuación, implemente el método Main en Program.cs.</span><span class="sxs-lookup"><span data-stu-id="8a00b-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="8a00b-184">Este código crea una nueva instancia de nuestro contexto y, a continuación, usa para insertar un nuevo Blog.</span><span class="sxs-lookup"><span data-stu-id="8a00b-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="8a00b-185">A continuación, usa una consulta LINQ para recuperar todos los Blogs de la base de datos ordenado alfabéticamente por título.</span><span class="sxs-lookup"><span data-stu-id="8a00b-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="8a00b-186">Ahora puede ejecutar la aplicación y probarlo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="8a00b-187">5. Trabajar con los cambios del modelo</span><span class="sxs-lookup"><span data-stu-id="8a00b-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="8a00b-188">Ahora es momento de realizar algunos cambios en nuestro modelo, cuando se realizan estos cambios que también es necesario para actualizar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="8a00b-189">Comenzaremos por agregar una nueva entidad de usuario a nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="8a00b-190">Agregue un nuevo **usuario** nombre de la entidad con **Username** como el nombre de clave y **cadena** como el tipo de propiedad para la clave</span><span class="sxs-lookup"><span data-stu-id="8a00b-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Agregar entidad de usuario](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="8a00b-192">Haga doble clic en el **Username** propiedad en la superficie de diseño y seleccione **propiedades**, cambio de las propiedades en la ventana la **MaxLength** si se establece en \*\*50 \*\* 
     *Esto restringe los datos que se pueden almacenar en el nombre de usuario y 50 caracteres*</span><span class="sxs-lookup"><span data-stu-id="8a00b-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="8a00b-193">Agregar un **DisplayName** propiedad escalar a la **usuario** entidad</span><span class="sxs-lookup"><span data-stu-id="8a00b-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="8a00b-194">Ahora tenemos un modelo actualizado y estamos preparados actualizar la base de datos para dar cabida a nuestro nuevo tipo de entidad de usuario.</span><span class="sxs-lookup"><span data-stu-id="8a00b-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="8a00b-195">Haga doble clic en la superficie de diseño y seleccione **generar base de datos de modelo...** , Entity Framework calculará una secuencia de comandos para volver a crear un esquema basado en el modelo actualizado.</span><span class="sxs-lookup"><span data-stu-id="8a00b-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="8a00b-196">Haga clic en **finalizar**</span><span class="sxs-lookup"><span data-stu-id="8a00b-196">Click **Finish**</span></span>
-   <span data-ttu-id="8a00b-197">Puede recibir advertencias sobre la sobrescritura de la secuencia de comandos DDL existente y las partes de asignación y almacenamiento del modelo, haga clic en **Sí** tanto estas advertencias</span><span class="sxs-lookup"><span data-stu-id="8a00b-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="8a00b-198">Se abre el script actualizado de SQL para crear la base de datos de</span><span class="sxs-lookup"><span data-stu-id="8a00b-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="8a00b-199">*El script generado quitará todas las tablas existentes y, a continuación, volver a crear el esquema desde el principio. Esto puede funcionar para el desarrollo local, pero no es un viable para insertar los cambios en una base de datos que ya se ha implementado. Si necesita publicar los cambios en una base de datos que ya se ha implementado, deberá modificar la secuencia de comandos o usar una herramienta de comparación de esquemas para calcular un script de migración.*</span><span class="sxs-lookup"><span data-stu-id="8a00b-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="8a00b-200">Haga doble clic en el script y seleccione **Execute**, se le pedirá que especifique la base de datos para conectarse a, especifique LocalDB o Express de SQL Server, según la versión de Visual Studio que está utilizando</span><span class="sxs-lookup"><span data-stu-id="8a00b-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="8a00b-201">Resumen</span><span class="sxs-lookup"><span data-stu-id="8a00b-201">Summary</span></span>

<span data-ttu-id="8a00b-202">En este tutorial que analizamos el desarrollo de Model First, que nos permitió crear un modelo en el Diseñador de EF y, a continuación, generar una base de datos de ese modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="8a00b-203">A continuación, se usa el modelo para leer y escribir algunos datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8a00b-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="8a00b-204">Por último, se actualizó el modelo y, a continuación, volver a crear el esquema de base de datos para que coincida con el modelo.</span><span class="sxs-lookup"><span data-stu-id="8a00b-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
