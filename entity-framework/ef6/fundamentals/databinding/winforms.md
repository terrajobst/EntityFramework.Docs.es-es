---
title: Enlace de datos con WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 071172810f7dac45f42aca0efa7f329bac31e9cd
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251198"
---
# <a name="databinding-with-winforms"></a>Enlace de datos con formularios Windows Forms
En este tutorial paso a paso muestra cómo enlazar tipos POCO a controles de Windows Forms (WinForms) en forma de "master-detail". La aplicación usa Entity Framework para rellenar los objetos con los datos de la base de datos, realizar un seguimiento de cambios y conservar los datos en la base de datos.

El modelo define dos tipos que participan en la relación de uno a varios: categoría (entidad de seguridad\\maestra) y el producto (dependientes\\detalle). A continuación, se usan las herramientas de Visual Studio para enlazar los tipos definidos en el modelo a los controles de WinForms. El marco de enlace de datos de formularios Windows Forms permite la navegación entre los objetos relacionados: selección de filas en la vista principal hace que la vista de detalle para actualizar con los datos secundarios correspondientes.

Las capturas de pantalla y los listados de código de este tutorial se toman de Visual Studio 2013, pero se puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.

## <a name="pre-requisites"></a>Requisitos previos

Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.

Si utiliza Visual Studio 2010, también debe instalar NuGet. Para obtener más información, consulte [instalar NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Crear la aplicación

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el panel izquierdo y **FormsApplication Windows** en el panel derecho
-   Escriba **WinFormswithEFSample** como el nombre
-   Seleccione **Aceptar**.

## <a name="install-the-entity-framework-nuget-package"></a>Instale el paquete NuGet de Entity Framework

-   En el Explorador de soluciones, haga doble clic en el **WinFormswithEFSample** proyecto
-   Seleccione **administrar paquetes NuGet...**
-   En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete
-   Haga clic en **instalar**  
    > [!NOTE]
    > Además del ensamblado de Entity Framework también se agrega una referencia a System.ComponentModel.DataAnnotations. Si el proyecto tiene una referencia a System.Data.Entity, a continuación, se eliminará cuando se instala el paquete de Entity Framework. El ensamblado System.Data.Entity ya no se usa para las aplicaciones de Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Implementación de IListSource para colecciones

Propiedades de la colección deben implementar la interfaz IListSource para habilitar el enlace de datos bidireccional con la ordenación cuando se usa Windows Forms. Para ello, vamos a ampliar ObservableCollection para agregar la funcionalidad de IListSource.

-   Agregar un **ObservableListSource** clase al proyecto:
    -   Haga doble clic en el nombre del proyecto
    -   Seleccione **Add -&gt; nuevo elemento**
    -   Seleccione **clase** y escriba **ObservableListSource** para el nombre de clase
-   Reemplace el código generado de forma predeterminada con el código siguiente:

*Esta clase permite datos bidireccional de enlace así como la ordenación. La clase se deriva de ObservableCollection&lt;T&gt; y agrega una implementación explícita de IListSource. El método GetList() de IListSource se implementa para devolver una implementación de IBindingList que permanece sincronizada con ObservableCollection. La implementación de IBindingList generada por ToBindingList admite la ordenación. El método de extensión ToBindingList se define en el ensamblado de Entity Framework.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definir un modelo

En este tutorial, puede decidió implementar un modelo con Code First o EF Designer. Realice una de las dos secciones siguientes.

### <a name="option-1-define-a-model-using-code-first"></a>Opción 1: Definir un modelo con Code First

En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First. Saltar a la siguiente sección (**opción 2: definir un modelo con Database First)** si prefiere usar Database First para invertir diseñar el modelo desde una base de datos mediante el diseñador EF

Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).

-   Agregue un nuevo **producto** clase al proyecto
-   Reemplace el código generado de forma predeterminada con el código siguiente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Agregar un **categoría** clase al proyecto.
-   Reemplace el código generado de forma predeterminada con el código siguiente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Además de definir las entidades, debe definir una clase que derive de **DbContext** y expone **DbSet&lt;TEntity&gt;**  propiedades. El **DbSet** propiedades que el contexto sepan qué tipos van a incluir en el modelo. El **DbContext** y **DbSet** tipos se definen en el ensamblado de Entity Framework.

Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.

-   Agregue un nuevo **ProductContext** clase al proyecto.
-   Reemplace el código generado de forma predeterminada con el código siguiente:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

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

EF genera código desde el modelo usando plantillas T4. Las plantillas se incluye con Visual Studio o se descarga desde la Galería de Visual Studio están diseñadas para su uso de propósito general. Esto significa que las entidades generadas a partir de estas plantillas tienen ICollection simple&lt;T&gt; propiedades. Sin embargo, al realizar el enlace de datos es recomendable tener las propiedades de colección que implementan IListSource. Por eso hemos creado la clase ObservableListSource anterior y ahora vamos a modificar las plantillas para hacer uso de esta clase.

-   Abra el **el Explorador de soluciones** y encontrar **ProductModel.edmx** archivo
-   Buscar el **ProductModel.tt** archivo que se anidará en el archivo ProductModel.edmx

    ![Plantilla de modelo de producto](~/ef6/media/productmodeltemplate.png)

-   Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio
-   Buscar y reemplazar las dos repeticiones de "**ICollection**"con"**ObservableListSource**". Estos se encuentran en aproximadamente líneas 296 y 484.
-   Buscar y reemplazar la primera aparición de "**HashSet**"con"**ObservableListSource**". Esta falta de disponibilidad se encuentra en línea aproximadamente 50. **No** reemplazar la segunda aparición de HashSet encuentra más adelante en el código.
-   Guarde el archivo ProductModel.tt. Esto debería producir el código para las entidades a generarse. Si el código no se pueden regenerar automáticamente, a continuación, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".

Si ahora abra el archivo Category.cs (que está anidado bajo ProductModel.tt), a continuación, debería ver que la colección de productos tiene el tipo **ObservableListSource&lt;producto&gt;**.

Compile el proyecto.

## <a name="lazy-loading"></a>Carga diferida

El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación. En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.

EF ofrece una opción de cargar entidades relacionadas de la base de datos automáticamente la primera vez que se tener acceso a la propiedad de navegación. Con este tipo de carga (denominada la carga diferida), tenga en cuenta que la primera vez que se tiene acceso a cada propiedad de navegación una consulta independiente se ejecutará en la base de datos si el contenido no está en el contexto.

Al utilizar tipos de entidad POCO, EF logra la carga diferida al crear instancias de tipos de proxy derivado en tiempo de ejecución y, a continuación, reemplazar propiedades virtuales en las clases para agregar el enlace de carga. Para obtener la carga diferida de objetos relacionados, debe declarar navegación captadores de propiedades como **pública** y **virtual** (**Overridable** en Visual Basic), y la clase no debe ser **sealed** (**NotOverridable** en Visual Basic). Cuando la base de datos utilizando las propiedades de navegación primera se realizan automáticamente virtuales para habilitar la carga diferida. En la sección Code First hemos optado por hacer que las propiedades de navegación virtual por la misma razón

## <a name="bind-object-to-controls"></a>Enlazar objetos a controles

Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación de WinForms.

-   En el menú principal, seleccione **proyecto -&gt; Agregar nuevo origen de datos...**
    (en Visual Studio 2010, deberá seleccionar **datos -&gt; Agregar nuevo origen de datos...** )
-   En la ventana de un tipo de origen de datos elija, seleccione **objeto** y haga clic en **siguiente**
-   En el cuadro de diálogo de objetos de datos, seleccione expandir la **WinFormswithEFSample** dos veces y seleccione **categoría** no es necesario para seleccionar el origen de datos del producto, porque se producirá a él a través del producto propiedad en el origen de datos de categoría.

    ![Origen de datos](~/ef6/media/datasource.png)

-   Haga clic en **Finalizar.** 
     *Si la ventana de orígenes de datos no aparece, seleccione *** Vista -&gt; otras Windows -&gt; orígenes de datos**
-   Presione el icono de anclaje, por lo que la ventana de orígenes de datos no auto ocultar. Deberá presionar el botón de actualización si la ventana que ya estaba visible.

    ![Origen de datos 2](~/ef6/media/datasource2.png)

-   En el Explorador de soluciones, haga doble clic en el **Form1.cs** archivo para abrir el formulario principal en el diseñador.
-   Seleccione el **categoría** origen de datos y arrastrarlo en el formulario. De forma predeterminada, una DataGridView nuevo (**categoryDataGridView**) y controles de barra de herramientas de exploración se agregan al diseñador. Estos controles se enlazan a BindingSource (**categoryBindingSource**) y enlace Navigator (**categoryBindingNavigator**) los componentes que se crean así.
-   Editar las columnas en el **categoryDataGridView**. Queremos establecer la **CategoryId** columna como de solo lectura. El valor de la **CategoryId** propiedad es generada por la base de datos después de que guardemos los datos.
    -   Haga clic en el control DataGridView de formularios y seleccione Editar columnas...
    -   Seleccione la columna CategoryId y establece ReadOnly en True
    -   Haga clic en Aceptar
-   Seleccione los productos desde el origen de datos de categoría y arrastrarlo en el formulario. El productDataGridView y productBindingSource se agregan al formulario.
-   Editar las columnas en el productDataGridView. Queremos ocultar las columnas CategoryId y categoría y establezca el ProductId de sólo lectura. El valor para la propiedad ProductId es generado por la base de datos después de que guardemos los datos.
    -   Haga clic en el control DataGridView de formularios y seleccione **Editar columnas...** .
    -   Seleccione el **ProductId** columna y establezca **ReadOnly** a **True**.
    -   Seleccione el **CategoryId** columna y presione la **quitar** botón. Haga lo mismo con el **categoría** columna.
    -   Presione **Aceptar**.

    Hasta ahora, hemos asociado nuestros controles DataGridView de formularios componentes BindingSource en el diseñador. En la siguiente sección se agregará código para el código subyacente para establecer categoryBindingSource.DataSource a la colección de entidades que realiza el seguimiento DbContext actualmente. Cuando se arrastra y coloca los productos desde la categoría, los formularios Windows Forms encargó de la configuración de la propiedad productsBindingSource.DataSource a la propiedad categoryBindingSource y productsBindingSource.DataMember para productos. Debido a este enlace, se mostrará solo los productos que pertenecen a la categoría actualmente seleccionada en el productDataGridView.
-   Habilitar la **guardar** botón en la barra de herramientas de navegación, haga clic en el botón secundario del mouse y seleccione **habilitado**.

    ![Diseñador de formularios 1](~/ef6/media/form1-designer.png)

-   Agregue el controlador de eventos para la operación de guardar botón haciendo doble clic en el botón. Esto agregará el controlador de eventos y proporcionan el código subyacente para el formulario. El código para el **categoryBindingNavigatorSaveItem\_haga clic en** se agregará el controlador de eventos en la sección siguiente.

## <a name="add-the-code-that-handles-data-interaction"></a>Agregue el código que administra la interacción de datos

Ahora vamos a agregar el código para usar el ProductContext para tener acceso a datos. Como se muestra a continuación, actualice el código de la ventana del formulario principal.

El código declara una instancia de ejecución prolongada de ProductContext. El objeto ProductContext se usa para consultar y guardar los datos en la base de datos. El método Dispose() en la instancia de ProductContext, a continuación, se llama desde el método OnClosing invalidado. Los comentarios del código proporcionan detalles sobre lo que hace el código.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Probar la aplicación de Windows Forms

-   La aplicación y puede probar la funcionalidad de compilación y ejecución.

    ![Formulario 1 antes de guardar](~/ef6/media/form1beforesave.png)

-   Después de guardar las claves del almacén generado se muestran en la pantalla.

    ![Formulario 1 guardar después](~/ef6/media/form1aftersave.png)

-   Si utilizó Code First, también verá que una **WinFormswithEFSample.ProductContext** base de datos se crea automáticamente.

    ![Explorador de objetos de servidor](~/ef6/media/serverobjexplorer.png)
