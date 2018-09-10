---
title: Enlace de datos con WPF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: e6df90db17d39d3aa91275800a6414fed40fb5db
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251159"
---
# <a name="databinding-with-wpf"></a>Enlace de datos con WPF
En este tutorial paso a paso muestra cómo enlazar tipos POCO a controles de WPF en un formulario de "master-detail". La aplicación utiliza la API de Entity Framework para rellenar los objetos con los datos de la base de datos, realizar un seguimiento de cambios y conservar los datos en la base de datos.

El modelo define dos tipos que participan en la relación de uno a varios: **categoría** (entidad de seguridad\\maestra) y **producto** (dependientes\\detalle). A continuación, se usan las herramientas de Visual Studio para enlazar los tipos definidos en el modelo a los controles WPF. El marco de enlace de datos WPF permite la navegación entre los objetos relacionados: selección de filas en la vista principal hace que la vista de detalle para actualizar con los datos secundarios correspondientes.

Las capturas de pantalla y los listados de código de este tutorial se toman de Visual Studio 2013, pero se puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Utilice la opción de 'Object' para la creación de orígenes de datos WPF

Con una versión anterior de Entity Framework se usa para recomienda el uso de la **base de datos** opción al crear un nuevo origen de datos basado en un modelo creado con EF Designer. Esto era porque el diseñador generaría un contexto que deriva de ObjectContext y clases de entidad que se derivan de EntityObject. Con la opción de base de datos le ayudarán a escribir el código mejor para interactuar con esta superficie de API.

Los diseñadores de EF para Visual Studio 2012 y Visual Studio 2013 generar un contexto que se deriva de DbContext, junto con clases de entidad POCO simple. Se recomienda usarlo en una plantilla de generación de código que use DbContext, tal como se describe más adelante en este tutorial con Visual Studio 2010.

Si usa la superficie de API de DbContext se debe usar el **objeto** opción al crear un nuevo origen de datos, como se muestra en este tutorial.

Si es necesario, puede [revertir a la generación de código en función de ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para los modelos creados con EF Designer.

## <a name="pre-requisites"></a>Requisitos previos

Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.

Si utiliza Visual Studio 2010, también debe instalar NuGet. Para obtener más información, consulte [instalar NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Crear la aplicación

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el panel izquierdo y **WPFApplication** en el panel derecho
-   Escriba **WPFwithEFSample** como el nombre
-   Seleccione **Aceptar**.

## <a name="install-the-entity-framework-nuget-package"></a>Instale el paquete NuGet de Entity Framework

-   En el Explorador de soluciones, haga doble clic en el **WinFormswithEFSample** proyecto
-   Seleccione **administrar paquetes NuGet...**
-   En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete
-   Haga clic en **instalar**  
    >[!NOTE]
    > Además del ensamblado de Entity Framework también se agrega una referencia a System.ComponentModel.DataAnnotations. Si el proyecto tiene una referencia a System.Data.Entity, a continuación, se eliminará cuando se instala el paquete de Entity Framework. El ensamblado System.Data.Entity ya no se usa para las aplicaciones de Entity Framework 6.

## <a name="define-a-model"></a>Definir un modelo

En este tutorial, puede decidió implementar un modelo con Code First o EF Designer. Realice una de las dos secciones siguientes.

### <a name="option-1-define-a-model-using-code-first"></a>Opción 1: Definir un modelo con Code First

En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First. Saltar a la siguiente sección (**opción 2: definir un modelo con Database First)** si prefiere usar Database First para invertir diseñar el modelo desde una base de datos mediante el diseñador EF

Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).

-   Agregue una nueva clase a la **WPFwithEFSample:**
    -   Haga doble clic en el nombre del proyecto
    -   Seleccione **agregar**, a continuación, **nuevo elemento**
    -   Seleccione **clase** y escriba **producto** para el nombre de clase
-   Reemplace el **producto** definición con el siguiente código de clase:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación. En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.

Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades. La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.

Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.

-   Agregue un nuevo **ProductContext** clase al proyecto con la siguiente definición:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Compile el proyecto.

### <a name="option-2-define-a-model-using-database-first"></a>Opción 2: Definir un modelo con Database First

Esta sección muestra cómo usar la primera base de datos para aplicar ingeniería inversa a su modelo de una base de datos con EF designer. Si ha completado la sección anterior (**opción 1: definir un modelo con Code First)**, omita esta sección e ir directamente a la **la carga diferida** sección.

#### <a name="create-an-existing-database"></a>Crear una base de datos existente

Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.

El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:

-   Si usa Visual Studio 2010 se creará una base de datos de SQL Express.
-   Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) base de datos.

Sigamos adelante y generar la base de datos.

-   **Vista -&gt; Explorador de servidores**
-   Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**
-   Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos

    ![Cambiar origen de datos](~/ef6/media/changedatasource.png)

-   Conectarse a LocalDB o Express de SQL, según lo que se ha instalado y escriba **productos** como el nombre de la base de datos

    ![Agregar conexión LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Agregar conexión rápida](~/ef6/media/addconnectionexpress.png)

-   Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**

    ![Crear base de datos](~/ef6/media/createdatabase.png)

-   La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**
-   Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Modelo de ingeniería inversa

Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.

-   **Proyecto -&gt; Agregar nuevo elemento...**
-   Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**
-   Escriba **ProductModel** como el nombre y haga clic en **Aceptar**
-   Esto inicia el **Asistente para Entity Data Model**
-   Seleccione **generar desde la base de datos** y haga clic en **siguiente**

    ![Elegir contenido de Model](~/ef6/media/choosemodelcontents.png)

-   Seleccione la conexión a la base de datos que creó en la primera sección, escriba **ProductContext** como el nombre de la cadena de conexión y haga clic en **siguiente**

    ![Elegir la conexión](~/ef6/media/chooseyourconnection.png)

-   Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'

    ![Elija los objetos](~/ef6/media/chooseyourobjects.png)

Una vez completado el proceso de ingeniería inversa el nuevo modelo se agrega al proyecto y se abrirá para que pueda ver en el Diseñador de Entity Framework. También se agregó un archivo App.config al proyecto con los detalles de conexión para la base de datos.

#### <a name="additional-steps-in-visual-studio-2010"></a>Pasos adicionales en Visual Studio 2010

Si está trabajando en Visual Studio 2010, a continuación, deberá actualizar el diseñador EF para usar la generación de código de EF6.

-   Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**
-   Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**
-   Seleccione el **EF 6.x generador de DbContext para C\#,** escriba **ProductsModel** como el nombre y haga clic en Agregar

#### <a name="updating-code-generation-for-data-binding"></a>Actualizando la generación de código para el enlace de datos

EF genera código desde el modelo usando plantillas T4. Las plantillas se incluye con Visual Studio o se descarga desde la Galería de Visual Studio están diseñadas para su uso de propósito general. Esto significa que las entidades generadas a partir de estas plantillas tienen ICollection simple&lt;T&gt; propiedades. Sin embargo, al realizar datos enlace con WPF es deseable utilizar **ObservableCollection** para las propiedades de colección, por lo que WPF puede realizar un seguimiento de los cambios realizados en las colecciones. Para ello, vamos a modificar las plantillas para usar ObservableCollection.

-   Abra el **el Explorador de soluciones** y encontrar **ProductModel.edmx** archivo
-   Buscar el **ProductModel.tt** archivo que se anidará en el archivo ProductModel.edmx

    ![Plantilla de modelo de producto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio
-   Buscar y reemplazar las dos repeticiones de "**ICollection**"con"**ObservableCollection**". Estos se encuentran aproximadamente en las líneas 296 y 484.
-   Buscar y reemplazar la primera aparición de "**HashSet**"con"**ObservableCollection**". Esta falta de disponibilidad se encuentra aproximadamente en línea 50. **No** reemplazar la segunda aparición de HashSet encuentra más adelante en el código.
-   Buscar y reemplazar la aparición única de "**System.Collections.Generic**"con"**System.Collections.ObjectModel**". Esto se encuentra aproximadamente en línea 424.
-   Guarde el archivo ProductModel.tt. Esto debería producir el código para las entidades a generarse. Si el código no se pueden regenerar automáticamente, a continuación, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".

Si ahora abra el archivo Category.cs (que está anidado bajo ProductModel.tt), a continuación, debería ver que la colección de productos tiene el tipo **ObservableCollection&lt;producto&gt;**.

Compile el proyecto.

## <a name="lazy-loading"></a>Carga diferida

El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación. En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.

EF ofrece una opción de cargar entidades relacionadas de la base de datos automáticamente la primera vez que se tener acceso a la propiedad de navegación. Con este tipo de carga (denominada la carga diferida), tenga en cuenta que la primera vez que se tiene acceso a cada propiedad de navegación una consulta independiente se ejecutará en la base de datos si el contenido no está en el contexto.

Al utilizar tipos de entidad POCO, EF logra la carga diferida al crear instancias de tipos de proxy derivado en tiempo de ejecución y, a continuación, reemplazar propiedades virtuales en las clases para agregar el enlace de carga. Para obtener la carga diferida de objetos relacionados, debe declarar navegación captadores de propiedades como **pública** y **virtual** (**Overridable** en Visual Basic), y la clase no debe ser **sealed** (**NotOverridable** en Visual Basic). Cuando la base de datos utilizando las propiedades de navegación primera se realizan automáticamente virtuales para habilitar la carga diferida. En la sección Code First hemos optado por hacer que las propiedades de navegación virtual por la misma razón

## <a name="bind-object-to-controls"></a>Enlazar objetos a controles

Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación de WPF.

-   Haga doble clic en **MainWindow.xaml** en el Explorador de soluciones para abrir el formulario principal
-   En el menú principal, seleccione **proyecto -&gt; Agregar nuevo origen de datos...**
    (en Visual Studio 2010, deberá seleccionar **datos -&gt; Agregar nuevo origen de datos...** )
-   En la elegir un Typewindow del origen de datos, seleccione **objeto** y haga clic en **siguiente**
-   En el cuadro de diálogo de objetos de datos, seleccione expandir la **WPFwithEFSample** dos veces y seleccione **categoría**  
    *No es necesario para seleccionar la **producto** del origen de datos, ya que obtenemos a él a través de la **producto**de propiedad en el **categoría** origen de datos*  

    ![Seleccionar objetos de datos](~/ef6/media/selectdataobjects.png)

-   Haga clic en **Finalizar.**
-   Se abre la ventana de orígenes de datos junto a la ventana de MainWindow.xaml *si la ventana de orígenes de datos no aparece, seleccione **vista -&gt; otras Windows -&gt; orígenes de datos***
-   Presione el icono de anclaje, por lo que la ventana de orígenes de datos no auto ocultar. Deberá presionar el botón de actualización si la ventana que ya estaba visible.

    ![Orígenes de datos](~/ef6/media/datasources.png)

-   Seleccione el ** categoría ** origen de datos y arrastrarlo en el formulario.

El siguiente, se produjo cuando se arrastra este origen:

-   El **categoryViewSource** recursos y la ** categoryDataGrid ** se agregó a XAML. Para obtener más información sobre DataViewSources, consulte http://bea.stollnitz.com/blog/?p=387.
-   La propiedad DataContext en el elemento de cuadrícula principal se estableció en "{StaticResource **categoryViewSource** }".  El **categoryViewSource** recursos actúa como un origen de enlace de la actividad\\elemento de cuadrícula principal. Los elementos internos de la cuadrícula, a continuación, heredan el valor de DataContext de la cuadrícula (la propiedad ItemsSource del categoryDataGrid se establece en "{Binding}") primario. 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Agregar una cuadrícula de detalles

Ahora que tenemos una cuadrícula para mostrar las categorías vamos a agregar una cuadrícula de detalles para mostrar los productos asociados.

-   Seleccione el ** productos ** propiedad en la ** categoría ** origen de datos y arrastrarlo en el formulario.
    -   El **categoryProductsViewSource** recursos y **productDataGrid** cuadrícula se agregan a XAML
    -   La ruta de acceso de enlace para este recurso está establecido en productos
    -   Marco de enlace de datos WPF garantiza que solo productos relacionados con la categoría seleccionada se mostrarán en **productDataGrid**
-   En el cuadro de herramientas, arrastre **botón** sesión en el formulario. Establecer el **nombre** propiedad **buttonSave** y el **contenido** propiedad **guardar**.

El formato debe ser similar al siguiente:

![Diseñador](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Agregue código que administra la interacción de datos

Es el momento de agregar algunos controladores de eventos a la ventana principal.

-   En la ventana XAML, haga clic en el  **&lt;ventana** elemento, este comando selecciona la ventana principal
-   En el **propiedades** ventana Elegir **eventos** en la esquina superior derecha, a continuación, haga doble clic en el cuadro de texto a la derecha de la **Loaded** etiqueta

    ![Propiedades de la ventana principal](~/ef6/media/mainwindowproperties.png)

-   Agregue también el **haga clic en** eventos para el **guardar** botón haciendo doble clic en el botón Guardar en el diseñador. 

Aparecerá el código subyacente del formulario, ahora se modificará el código para usar el ProductContext para tener acceso a datos. Actualice el código para MainWindow tal y como se muestra a continuación.

El código declara una instancia de ejecución prolongada de **ProductContext**. El **ProductContext** objeto se usa para consultar y guardar datos en la base de datos. El **Dispose**() en el **ProductContext** , a continuación, se denomina instancia de invalidado **OnClosing** método. Los comentarios del código proporcionan detalles sobre lo que hace el código.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Probar la aplicación de WPF

-   Compile y ejecute la aplicación. Si utilizó Code First, verá que una **WPFwithEFSample.ProductContext** base de datos se crea automáticamente.
-   Escriba un nombre de categoría en los nombres de producto y de cuadrícula principales en la cuadrícula inferior *no escribir nada en las columnas de identificador, ya que la clave principal se genera la base de datos*

    ![Ventana principal con los productos y las nuevas categorías](~/ef6/media/screen1.png)

-   Presione el **guardar** botón para guardar los datos en la base de datos

Después de llamar a la clase DbContext **SaveChanges**(), los identificadores se rellenan con los valores de la base de datos generada. Dado que se llama **actualizar**() después de **SaveChanges**() la **DataGrid** controles se actualizan con los nuevos valores.

![Ventana principal con los identificadores de rellenado](~/ef6/media/screen2.png)
