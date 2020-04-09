---
title: Enlace de datos con WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639141"
---
> [!IMPORTANT]
> **Este documento es válido para WPF en .NET Framework**
>
> En este documento se describe el enlace de datos para WPF en .NET Framework. Para los nuevos proyectos de .NET Core, se recomienda usar [EF Core](/ef/core) en lugar de Entity Framework 6. Se realiza un seguimiento de la documentación para el enlace de datos en EF Core en [el #778.](https://github.com/dotnet/EntityFramework.Docs/issues/778)

# <a name="databinding-with-wpf"></a>Databinding with WPF (Enlace de datos con WPF)
En este tutorial paso a paso se muestra cómo enlazar tipos POCO a controles WPFWPF en un formulario "maestro-detalle". La aplicación usa las API de Entity Framework para rellenar objetos con datos de la base de datos, realizar un seguimiento de los cambios y conservar los datos en la base de datos.

El modelo define dos tipos que participan en la\\relación uno a\\varios: **Categoría** (principal maestro) y **Producto** (detalle dependiente). A continuación, las herramientas de Visual Studio se usan para enlazar los tipos definidos en el modelo a los controles WPFWPF. El marco de enlace de datos WPFWPF permite la navegación entre objetos relacionados: la selección de filas en la vista maestra hace que la vista de detalle se actualice con los datos secundarios correspondientes.

Las capturas de pantalla y las listas de código de este tutorial se toman de Visual Studio 2013, pero puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Use la opción 'Objeto' para crear orígenes de datos WPF

Con la versión anterior de Entity Framework se usa el uso de la base de **datos** opción al crear un nuevo origen de datos basado en un modelo creado con el Diseñador de EF. Esto se debió a que el diseñador generaría un contexto derivado de ObjectContext y clases de entidad derivadas de EntityObject. El uso de la opción Base de datos le ayudaría a escribir el mejor código para interactuar con esta superficie de API.

Los diseñadores de EF para Visual Studio 2012 y Visual Studio 2013 generan un contexto que deriva de DbContext junto con clases de entidad POCO simples. Con Visual Studio 2010 se recomienda intercambiar a una plantilla de generación de código que usa DbContext como se describe más adelante en este tutorial.

Al usar la superficie de la API de DbContext, debe usar la opción **Object** al crear un nuevo origen de datos, como se muestra en este tutorial.

Si es necesario, puede volver a la generación de [código basada en ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para los modelos creados con EF Designer.

## <a name="pre-requisites"></a>Requisitos previos

Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.

Si usa Visual Studio 2010, también tiene que instalar NuGet. Para obtener más información, vea [Instalar NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Crear la aplicación

-   Abra Visual Studio.
-   **Archivo&gt; -&gt; Nuevo - Proyecto....**
-   Seleccione **Windows** en el panel izquierdo y **WPFApplication** en el panel derecho
-   Escriba **WPFwithEFSample** como nombre
-   Seleccione **Aceptar**

## <a name="install-the-entity-framework-nuget-package"></a>Instale el paquete NuGet de Entity Framework

-   En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **WinFormswithEFSample**
-   Seleccione **Administrar paquetes NuGet...**
-   En el cuadro de diálogo Administrar paquetes NuGet, seleccione la pestaña **Online** y elija el paquete **EntityFramework**
-   Haga clic en **Instalar**  
    >[!NOTE]
    > Además del ensamblado EntityFramework, también se agrega una referencia a System.ComponentModel.DataAnnotations. Si el proyecto tiene una referencia a System.Data.Entity, se quitará cuando se instale el paquete EntityFramework. El ensamblado System.Data.Entity ya no se utiliza para aplicaciones de Entity Framework 6.

## <a name="define-a-model"></a>Definir un modelo

En este tutorial puede optar por implementar un modelo mediante Code First o EF Designer. Complete una de las dos secciones siguientes.

### <a name="option-1-define-a-model-using-code-first"></a>Opción 1: Definir un modelo utilizando Code First

En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First. Vaya a la siguiente sección **(Opción 2: definir un modelo con Database First)** si prefiere usar Database First para realizar ingeniería inversa del modelo desde una base de datos mediante el diseñador de EF

Cuando se usa el desarrollo de Code First, normalmente se empieza escribiendo clases de .NET Framework que definen el modelo conceptual (dominio).

-   Agregue una nueva clase a **la WPFwithEFSample:**
    -   Haga clic con el botón derecho en el nombre del proyecto
    -   Seleccione **Agregar**y, a continuación, **Nuevo elemento**
    -   Seleccione **Clase** e introduzca **Producto** para el nombre de clase
-   Reemplace la definición de clase **Product** por el código siguiente:

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

El **Products** propiedad en el **Category** clase y **Category** propiedad en el **Product** clase son propiedades de navegación. En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.

Además de definir entidades, debe definir una clase que derive de&lt;DbContext y expone las propiedades de DbSet TEntity.&gt; Las propiedades&lt;DbSet TEntity&gt; permiten al contexto saber qué tipos desea incluir en el modelo.

Una instancia del tipo derivado DbContext administra los objetos de entidad durante el tiempo de ejecución, lo que incluye rellenar objetos con datos de una base de datos, realizar el seguimiento de cambios y conservar los datos en la base de datos.

-   Agregue una nueva clase **ProductContext** al proyecto con la siguiente definición:

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

### <a name="option-2-define-a-model-using-database-first"></a>Opción 2: Definir un modelo utilizando Database First

En esta sección se muestra cómo usar Database First para realizar ingeniería inversa del modelo desde una base de datos mediante el diseñador de EF. Si ha completado la sección anterior (**Opción 1: Definir un modelo mediante Code First),** omita esta sección y vaya directamente a la sección **Carga diferida.**

#### <a name="create-an-existing-database"></a>Crear una base de datos existente

Normalmente, cuando se dirige a una base de datos existente, ya se creará, pero para este tutorial necesitamos crear una base de datos para tener acceso.

El servidor de base de datos que se instala con Visual Studio es diferente en función de la versión de Visual Studio que haya instalado:

-   Si usa Visual Studio 2010, va a crear una base de datos de SQL Express.
-   Si usa Visual Studio 2012, va a crear una base de datos [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Vamos a seguir adelante y generar la base de datos.

-   **Ver&gt; - Explorador de servidores**
-   Haga clic con el botón derecho en Conexiones de **datos -&gt; Agregar conexión...**
-   Si no se ha conectado a una base de datos desde el Explorador de servidores antes de que deba seleccionar Microsoft SQL Server como origen de datos

    ![Cambiar origen de datos](~/ef6/media/changedatasource.png)

-   Conéctese a LocalDB o SQL Express, dependiendo del que haya instalado, e introduzca **Products** como nombre de la base de datos

    ![Agregar conexión LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Añadir conexión exprés](~/ef6/media/addconnectionexpress.png)

-   Seleccione **Aceptar** y se le preguntará si desea crear una nueva base de datos, seleccione **Sí**

    ![Creación de una base de datos](~/ef6/media/createdatabase.png)

-   La nueva base de datos aparecerá ahora en el Explorador de servidores, haga clic con el botón derecho en ella y seleccione **Nueva consulta**
-   Copie el siguiente SQL en la nueva consulta, haga clic con el botón derecho en la consulta y seleccione **Ejecutar**

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

Vamos a hacer uso de Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.

-   **Proyecto&gt; - Añadir nuevo elemento...**
-   Seleccione **Datos** en el menú de la izquierda y, a continuación, **ADO.NET Entity Data Model**
-   Escriba **ProductModel** como nombre y haga clic en **Aceptar**
-   Esto inicia el **Asistente para Entity Data Model**
-   Seleccione **Generar a partir de la base de datos** y haga clic en **Siguiente**

    ![Elegir contenido de Model](~/ef6/media/choosemodelcontents.png)

-   Seleccione la conexión a la base de datos que creó en la primera sección, escriba **ProductContext** como nombre de la cadena de conexión y haga clic en **Siguiente**

    ![Elija su conexión](~/ef6/media/chooseyourconnection.png)

-   Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'

    ![Elija sus objetos](~/ef6/media/chooseyourobjects.png)

Una vez completado el proceso de ingeniería inversa, el nuevo modelo se agrega al proyecto y se abre para que lo vea en Entity Framework Designer. También se ha agregado un archivo App.config al proyecto con los detalles de conexión de la base de datos.

#### <a name="additional-steps-in-visual-studio-2010"></a>Pasos adicionales en Visual Studio 2010

Si está trabajando en Visual Studio 2010, deberá actualizar el diseñador de EF para usar la generación de código EF6.

-   Haga clic con el botón derecho en un punto vacío del modelo en EF Designer y seleccione Agregar elemento de generación de **código...**
-   Seleccione **Plantillas en línea** en el menú de la izquierda y busque **DbContext**
-   Seleccione **EF 6.x DbContext\#Generator para C ,** escriba **ProductsModel** como nombre y haga clic en Agregar

#### <a name="updating-code-generation-for-data-binding"></a>Actualización de la generación de código para el enlace de datos

EF genera código a partir del modelo mediante plantillas T4. Las plantillas incluidas con Visual Studio o descargadas de la galería de Visual Studio están diseñadas para uso general. Esto significa que las entidades generadas a&lt;&gt; partir de estas plantillas tienen propiedades ICollection T simples. Sin embargo, al realizar el enlace de datos mediante WPFWPF es deseable usar **ObservableCollection** para las propiedades de colección para que WPFWPF pueda realizar un seguimiento de los cambios realizados en las colecciones. Para ello vamos a modificar las plantillas para utilizar ObservableCollection.

-   Abra el **Explorador de soluciones** y busque el archivo **ProductModel.edmx**
-   Busque el archivo **ProductModel.tt** que se anidará en el archivo ProductModel.edmx

    ![Plantilla de modelo de producto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio
-   Busque y reemplace las dos apariciones de "**ICollection"** por "**ObservableCollection**". Estos se encuentran aproximadamente en las líneas 296 y 484.
-   Busque y reemplace la primera aparición de "**HashSet**" por "**ObservableCollection**". Esta ocurrencia se encuentra aproximadamente en la línea 50. **No** reemplace la segunda aparición de HashSet que se encuentra más adelante en el código.
-   Busque y reemplace la única aparición de "**System.Collections.Generic**" por "**System.Collections.ObjectModel**". Se encuentra aproximadamente en la línea 424.
-   Guarde el archivo ProductModel.tt. Esto debería hacer que se regenere el código de las entidades. Si el código no se regenera automáticamente, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".

Si ahora abre el archivo de Category.cs (que está anidado en ProductModel.tt), debería ver que la colección Products tiene el tipo **ObservableCollection&lt;Product&gt;**.

Compile el proyecto.

## <a name="lazy-loading"></a>Carga diferida

El **Products** propiedad en el **Category** clase y **Category** propiedad en el **Product** clase son propiedades de navegación. En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.

EF le ofrece la opción de cargar entidades relacionadas desde la base de datos automáticamente la primera vez que tenga acceso a la propiedad de navegación. Con este tipo de carga (denominada carga diferida), tenga en cuenta que la primera vez que acceda a cada propiedad de navegación se ejecutará una consulta independiente en la base de datos si el contenido no está ya en el contexto.

Cuando se usan tipos de entidad POCO, EF logra la carga diferida mediante la creación de instancias de tipos de proxy derivados durante el tiempo de ejecución y, a continuación, invalidar las propiedades virtuales en las clases para agregar el enlace de carga. Para obtener la carga diferida de objetos relacionados, debe declarar los captadores de propiedades de navegación como **públicos** y **virtuales** **(Overridable** en Visual Basic) y la clase no debe estar **sellada** **(NotOverridable** en Visual Basic). Cuando se utilizan las propiedades de navegación de Database First se hacen virtuales automáticamente para habilitar la carga diferida. En la sección Code First elegimos hacer que las propiedades de navegación sean virtuales por la misma razón

## <a name="bind-object-to-controls"></a>Enlazar objeto a controles

Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación WPFWPF.

-   Haga doble clic en **MainWindow.xaml** en el Explorador de soluciones para abrir el formulario principal
-   En el menú principal, seleccione **Proyecto -&gt; Agregar nuevo origen de datos ...**
    (en Visual Studio 2010, debe seleccionar **Datos -&gt; Agregar nuevo origen**de datos... )
-   En la ventana Elegir un tipo de origen de datos, seleccione **Objeto** y haga clic en **Siguiente**
-   En el cuadro de diálogo Seleccionar los objetos de datos, desdoble el **ARCHIVO WPFwithEFSample** dos veces y seleccione **Categoría**  
    *No es necesario seleccionar el origen de datos **Del producto,** porque llegaremos a él a través de la propiedad **Producto**en el origen de datos **Categoría***  

    ![Seleccionar objetos de datos](~/ef6/media/selectdataobjects.png)

-   Haga clic en **Finalizar**.
-   La ventana Orígenes de datos se abre junto a la ventana MainWindow.xaml Si la ventana Orígenes de *datos no se muestra, seleccione Ver **-&gt; Otros orígenes de&gt; datos** * de Windows
-   Pulse el icono de pasador para que la ventana Orígenes de datos no se oculte automáticamente. Es posible que deba pulsar el botón de actualización si la ventana ya estaba visible.

    ![Orígenes de datos](~/ef6/media/datasources.png)

-   Seleccione el origen de datos **Categoría** y arrástrelo al formulario.

Lo siguiente ocurrió cuando arrastramos esta fuente:

-   El recurso **categoryViewSource** y el control **categoryDataGrid** se agregaron a XAML 
-   El DataContext propiedad en el elemento Grid primario se estableció en "'StaticResource **categoryViewSource** '".El **recurso categoryViewSource** sirve como origen\\de enlace para el elemento Grid primario externo. A continuación, los elementos Grid internos heredan el valor DataContext de la cuadrícula primaria (la propiedad ItemsSource de categoryDataGrid se establece en "-Binding-")

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

## <a name="adding-a-details-grid"></a>Adición de una cuadrícula de detalles

Ahora que tenemos una cuadrícula para mostrar Categorías vamos a agregar una cuadrícula de detalles para mostrar los Productos asociados.

-   Seleccione la propiedad **Products** en el origen de datos **Category** y arrástrela al formulario.
    -   El recurso **categoryProductsViewSource** y la cuadrícula **productDataGrid** se agregan a XAML
    -   La ruta de enlace para este recurso se establece en Products
    -   WPFWPF marco de enlace de datos garantiza que sólo los productos relacionados con la categoría seleccionada se muestran en **productDataGrid**
-   Desde el cuadro de herramientas, arrastre **Button** al formulario. Establezca la propiedad **Name** en **buttonSave** y la propiedad **Content** en **Save**.

El formulario debe ser similar a esto:

![Diseñador](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Agregar código que controla la interacción de datos

Es hora de agregar algunos controladores de eventos a la ventana principal.

-   En la ventana XAML, ** &lt;** haga clic en el elemento Window, esto selecciona la ventana principal
-   En la ventana **Propiedades** elija **Eventos** en la parte superior derecha y, a continuación, haga doble clic en el cuadro de texto situado a la derecha de la etiqueta **Cargado**

    ![Propiedades de la ventana principal](~/ef6/media/mainwindowproperties.png)

-   Agregue también el evento **Click** para el botón **Guardar** haciendo doble clic en el botón Guardar del diseñador. 

Esto le lleva al código subyacente para el formulario, ahora vamos a editar el código para usar el ProductContext para realizar el acceso a datos. Actualice el código de MainWindow como se muestra a continuación.

El código declara una instancia de ejecución prolongada de **ProductContext**. El objeto **ProductContext** se utiliza para consultar y guardar datos en la base de datos. A continuación, se llama a **Dispose()** en la instancia **de ProductContext** desde el método **OnClosing** invalidado.Los comentarios de código proporcionan detalles sobre lo que hace el código.

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

## <a name="test-the-wpf-application"></a>Pruebe la aplicación WPF

-   Compile y ejecute la aplicación. Si usó Code First, verá que se crea una base de datos **WPFwithEFSample.ProductContext** automáticamente.
-   Escriba un nombre de categoría en la cuadrícula superior y los nombres de productos en la cuadrícula inferior No escriba nada en las *columnas de ID, porque la clave principal la genera la base* de datos

    ![Ventana principal con nuevas categorías y productos](~/ef6/media/screen1.png)

-   Pulse el botón **Guardar** para guardar los datos en la base de datos

Después de la llamada a **SaveChanges()** de DbContext, los identificadores se rellenan con los valores generados por la base de datos. Porque llamamos **a Refresh()** después de **SaveChanges()** los controles **DataGrid** también se actualizan con los nuevos valores.

![Ventana principal con iDE rellenados](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información sobre el enlace de datos a colecciones mediante WPFWPF, vea [este tema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) en la documentación de WPFWPF.  
