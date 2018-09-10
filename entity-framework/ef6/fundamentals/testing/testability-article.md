---
title: Capacidad de prueba y Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 2a2384c7868ae3cf6af4f915c06ae9fdb622634c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251328"
---
# <a name="testability-and-entity-framework-40"></a>Capacidad de prueba y Entity Framework 4.0
Scott Allen

Fecha de publicación: Mayo de 2010

## <a name="introduction"></a>Introducción

En este artículo se describe y se muestra cómo escribir código fácil de probar con el ADO.NET Entity Framework 4.0 y Visual Studio 2010. Este artículo no trata de centrarse en una metodología de pruebas específica, como el diseño controlado por pruebas (TDD) o el diseño controlado por comportamiento (BDD). En su lugar, este artículo se centrará en cómo escribir código que utiliza ADO.NET Entity Framework, pero que siga siendo fácil de aislar y probar de forma automática. Examinaremos los patrones de diseño comunes que facilitan las pruebas en los datos de escenarios de acceso y ver cómo se aplican esos patrones cuando se usa el marco de trabajo. También analizaremos las características específicas del marco para ver cómo estas características pueden trabajar en código comprobable.

## <a name="what-is-testable-code"></a>¿Qué es código fácil de probar?

La capacidad de comprobar una aplicación de software con pruebas unitarias automatizadas ofrece muchas ventajas deseables. Todo el mundo sabe que pruebas correctas reducen el número de defectos de software en una aplicación y el aumento de la calidad de la aplicación - pero con las pruebas unitarias en su lugar va más allá de simplemente detecta errores.

Permite que un equipo de desarrollo ahorrar tiempo y permanecen en el control del software que crean un conjunto de pruebas unitarias correctas. Un equipo puede realizar cambios en el código existente, refactorización, rediseño y reestructuración de software para cumplir los nuevos requisitos y agregar nuevos componentes en una aplicación todo sabiendo el conjunto de pruebas puede comprobar el comportamiento de la aplicación. Pruebas unitarias forman parte de un ciclo rápido de comentarios para facilitar el cambio y conservar el mantenimiento de software a medida que aumenta la complejidad.

Las pruebas unitarias viene con un precio, sin embargo. Un equipo tiene que invertir tiempo para crear y mantener pruebas unitarias. La cantidad de esfuerzo necesario para crear estas pruebas se relaciona directamente con el **testability** del software subyacente. ¿Es fácil probar el software? Un equipo de diseño de software con capacidad de prueba en mente creará pruebas eficaces con más rapidez que el equipo que trabaja con software un-testable.

Microsoft diseñó ADO.NET Entity Framework 4.0 (EF4) con capacidad de prueba en mente. Esto no significa que los desarrolladores van a escribir pruebas unitarias para el propio código de framework. En su lugar, los objetivos de capacidad de prueba de EF4 facilitan la creación fácil de probar código que se basa en el marco de trabajo. Antes de adentrarnos en los ejemplos específicos, merece la pena entender las cualidades de código fácil de probar.

### <a name="the-qualities-of-testable-code"></a>Las cualidades de código comprobable

Código que es muy fácil probar siempre mostrará al menos dos características. En primer lugar, puede probar código es fácil **observar**. Dado un conjunto de entradas, debe ser fácil de observar el resultado del código. Por ejemplo, el siguiente método de prueba es fácil porque el método devuelve directamente el resultado de un cálculo.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Un método de prueba es difícil, si el método escribe el valor calculado en un socket de red, una tabla de base de datos o un archivo similar al código siguiente. La prueba tiene que realizar trabajo adicional para recuperar el valor.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

En segundo lugar, es fácil de probar código **aislar**. Vamos a usar el pseudocódigo siguiente como ejemplo de código comprobable incorrecto.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

El método es fácil observar, podemos pasar una póliza de seguro y comprobar el valor devuelto coincide con un resultado esperado. Sin embargo, para probar el método necesitaremos tener una base de datos que se instala con el esquema correcto, y configurar el servidor SMTP en caso de que el método intenta enviar un correo electrónico.

Solo desea la prueba unitaria comprobar la lógica de cálculo dentro del método, pero la prueba puede producir un error porque el servidor de correo electrónico está desconectado o porque mover el servidor de base de datos. Ambos de estos errores están relacionadas con el comportamiento de la prueba que desea comprobar. El comportamiento es difícil aislar.

Los desarrolladores de software que esforzarse en escribir código fácil de probar con frecuencia se esfuerzan por mantener una separación de preocupaciones en el código que escriben. El método anterior debe centrarse en los cálculos de negocio y delegar los detalles de implementación de base de datos y el correo electrónico a otros componentes. Robert C. Martin llama a este principio de responsabilidad única. Un objeto debe encapsular una responsabilidad única y estrecha, como calcular el valor de una directiva. Todas las demás tareas de notificación y la base de datos deben ser la responsabilidad de algún otro objeto. El código escrito de este modo es más fácil aislar porque se centra en una sola tarea.

En. NET, tenemos las abstracciones que necesitamos seguir el principio de responsabilidad única y así conseguir aislamiento. Podemos usar definiciones de interfaz y forzar el código para usar la abstracción de interfaz en lugar de un tipo concreto. Más adelante en este artículo veremos cómo un método como el ejemplo erróneo presentado anteriormente puede trabajar con interfaces que *buscar* como se comunicará con la base de datos. Durante la prueba, sin embargo, se puede sustituir una implementación ficticia que no comunicarse con la base de datos, sino que contiene datos en memoria. Esta implementación ficticia aislará el código de problemas no relacionados en el código de acceso a datos o la configuración de la base de datos.

Hay otras ventajas derivadas del aislamiento. El cálculo de negocios en el último método solo tardará unos milisegundos para ejecutar, pero la propia prueba podría ejecutarse durante varios segundos como los saltos de código en torno a la red y se comunica a distintos servidores. Deben ejecutar las pruebas unitarias rápida para facilitar los cambios pequeños. Pruebas unitarias deben también ser repetibles y no producirá un error porque un componente no está relacionado con la prueba tiene un problema. Escribir código que sea fácil para observar y aislar significa que los desarrolladores tendrán mucho más fácil escribir pruebas para el código, dedicar menos tiempo esperando para que ejecutar las pruebas y más importante aún, dedican menos tiempo a la localización de errores que no existen.

Espero que puede apreciar las ventajas de las pruebas y comprender las calidades que exhibe el código fácil de probar. Que se van a resolver cómo escribir código que funciona con EF4 para guardar datos en una base de datos sin dejar de observable y fácil de aislar, pero primero se deberá limitar nuestro enfoque para tratar diseños apta para las pruebas de acceso a datos.

## <a name="design-patterns-for-data-persistence"></a>Patrones de diseño para la persistencia de datos

Ambos ejemplos incorrecta presentados anteriormente tenían demasiadas responsabilidades. El primer ejemplo incorrecto tenía que realizar un cálculo *y* escribir en un archivo. El segundo ejemplo incorrecto tuvo que leer datos de una base de datos *y* realizan un cálculo business *y* enviar correo electrónico. Mediante el diseño de los métodos más pequeños que separan las preocupaciones y delegación la responsabilidad a otros componentes hará grandes esfuerzos para escribir código fácil de probar. El objetivo es crear una funcionalidad creando las acciones de pequeñas escala y centradas de abstracciones.

Cuando se trata de persistencia de datos pequeño y centradas abstracciones que estamos buscando son tan comunes ha se han documentado como patrones de diseño. Libro de Martin Patterns of Enterprise Application Architecture fue el primer trabajo para describir estos patrones en la impresión. Antes le mostramos cómo estos ADO.NET Entity Framework implementa y funciona con estos patrones, le presentaremos una breve descripción de estos patrones en las secciones siguientes.

### <a name="the-repository-pattern"></a>El modelo de repositorio

Fowler dice un repositorio de "Media entre el dominio y los datos de las capas de asignación mediante una interfaz similar a colección para tener acceso a objetos de dominio". El objetivo del patrón de repositorio es aislar el código desde el comercializaba del acceso a datos y, como hemos visto anteriormente aislamiento es un rasgo necesario para la capacidad de prueba.

La clave para el aislamiento es cómo el repositorio expone objetos mediante una interfaz similar a colección. La lógica de que escritura para no usar el repositorio tiene idea de cómo el repositorio materializará los objetos que se solicite. El repositorio puede comunicarse con una base de datos, o simplemente podría devolver objetos de una colección en memoria. Todo el código que necesita saber es que el repositorio aparece para mantener la colección, y puede recuperar, agregar y eliminar objetos de la colección.

En las aplicaciones .NET existentes un repositorio concreto a menudo se hereda de una interfaz genérica similar al siguiente:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Haremos algunos cambios a la definición de interfaz cuando se proporciona una implementación de EF4, pero el concepto básico sigue siendo el mismo. Código puede usar un repositorio concreto que implementa esta interfaz para recuperar una entidad por su valor de clave principal para recuperar una colección de entidades basándose en la evaluación de un predicado, o simplemente recuperar todas las entidades disponibles. El código también puede agregar y quitar las entidades a través de la interfaz del repositorio.

Dado un objetos IRepository de empleado, el código puede realizar las siguientes operaciones.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Dado que el código está usando una interfaz (IRepository de empleado), podemos proporcionar el código con diferentes implementaciones de la interfaz. Una implementación puede ser una implementación respaldada por EF4 y persistencia de objetos en una base de datos de Microsoft SQL Server. Una implementación diferente (una que se usa durante las pruebas) podría estar respaldada por los objetos de una lista de empleados en memoria. La interfaz le ayudará a lograr el aislamiento en el código.

Tenga en cuenta la IRepository&lt;T&gt; interfaz no expone una operación de guardar. ¿Cómo se actualiza objetos existentes? Puede llegar a través de definiciones de IRepository que incluyen la operación de guardar, y las implementaciones de estos repositorios será preciso que conserve inmediatamente un objeto en la base de datos. Sin embargo, en muchas aplicaciones no queremos conservar los objetos individualmente. En su lugar, queremos cobren vida a objetos, quizás de diferentes repositorios, modificar estos objetos como parte de una actividad de negocio y, a continuación, se conservan todos los objetos como parte de una única operación atómica. Afortunadamente, hay un patrón para permitir que a este tipo de comportamiento.

### <a name="the-unit-of-work-pattern"></a>La unidad de patrón de trabajo

Fowler dice que una unidad de trabajo "mantendrá una lista de objetos afectados por una transacción empresarial y coordina la escritura fuera de los cambios y la resolución de problemas de simultaneidad". Es responsabilidad de la unidad de trabajo para realizar el seguimiento de cambios a los objetos se cobren vida desde un repositorio y conservan los cambios que hemos realizado en los objetos cuando decimos que la unidad de trabajo para confirmar los cambios. También es responsabilidad de la unidad de trabajo para aprovechar los nuevos objetos se ha agregado a todos los repositorios e inserción los objetos en una base de datos y también la eliminación de administrar.

Si alguna vez ha realizado cualquier trabajo con conjuntos de datos de ADO.NET, a continuación, ya estará familiarizado con la unidad de patrón de trabajo. Los conjuntos de datos de ADO.NET tenía la capacidad de realizar un seguimiento de nuestros actualizaciones, eliminaciones e inserción de objetos DataRow y podría (con la Ayuda de un TableAdapter) reconciliar todos los cambios a una base de datos. Sin embargo, los objetos de conjunto de datos modelo un subconjunto desconectado de la base de datos subyacente. La unidad de patrón de trabajo muestra el mismo comportamiento, pero funciona con objetos de negocios y los objetos de dominio que son conscientes de la base de datos y aísla de código de acceso a datos.

Una abstracción para modelar la unidad de trabajo en el código .NET podría parecerse a lo siguiente:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Mediante la exposición de las referencias del repositorio de la unidad de trabajo, que es posible garantizar que una sola unidad de trabajo objeto tiene la capacidad de realizar un seguimiento de todas las entidades materializadas durante una transacción empresarial. La implementación del método de confirmación para una unidad de trabajo real es donde ocurre toda la magia conciliar los cambios en memoria con la base de datos. 

Dada una referencia de IUnitOfWork, el código puede realizar cambios en objetos de negocios que se recuperan de uno o varios repositorios y guardar todos los cambios mediante la operación de confirmación atómica.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>El patrón de carga diferida

Fowler usa el nombre de carga diferida para describir "un objeto que no contiene todos los datos necesita, pero sabe cómo obtenerlo". Transparente carga diferida es una característica importante que al escribir código empresarial fácil de probar y trabajar con una base de datos relacional. Por ejemplo, considere el siguiente código.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

¿Cómo se rellena la colección de tarjetas de registro? Hay dos respuestas posibles. Una respuesta es que el repositorio de empleados, cuando se le pida para capturar a un empleado, emite una consulta para recuperar a tanto el empleado junto con información de tarjeta de tiempo asociada del empleado. En las bases de datos relacionales Esto normalmente requiere una consulta con una cláusula JOIN y puede dar lugar a recuperar más información que una aplicación necesita. ¿Qué ocurre si la aplicación nunca debe tener acceso a la propiedad de tarjetas de registro?

Una respuesta de la segunda consiste en cargar la propiedad de tarjetas de registro "a petición". La carga diferida es transparente para la lógica de negocios e implícita porque el código no llamar a API especiales para recuperar información de tarjeta de tiempo. El código se da por supuesto que la información de tarjeta de tiempo está presente cuando sea necesario. Hay algo de magia con la carga diferida que generalmente implica la intercepción en tiempo de ejecución de las invocaciones de método. El código de intercepción es responsable de comunicarse con la base de datos y recuperar información de tarjeta de tiempo y dejar la lógica de negocios puede la lógica de negocios. Esta magia de la carga diferida permite que el código de negocio aislar propio de operaciones de recuperación de datos y los resultados más código fácil de probar.

El inconveniente de una carga diferida es que cuando una aplicación *does* necesita la información de tarjeta de tiempo que el código ejecutará una consulta adicional. Esto supone un problema para muchas aplicaciones, pero para aplicaciones sensibles al rendimiento o las aplicaciones de bucle a través de un número de objetos employee y ejecutar una consulta para recuperar las tarjetas de tiempo durante cada iteración del bucle (un problema a menudo se denomina N + 1 problema de consulta), la carga diferida es un arrastre. En estos escenarios es posible que desee una aplicación cargar concienzudamente información de tarjeta de tiempo de la manera más eficaz posible.

Afortunadamente, veremos cómo EF4 admite ambos implícita carga diferida y diligente eficaz carga cuando se mueven a la siguiente sección e implementar estos patrones.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementación de patrones con Entity Framework

La buena noticia es que todos los patrones de diseño que se describe en la última sección son fáciles de implementar con EF4. Para demostrar que vamos a usar una aplicación sencilla de ASP.NET MVC para editar y mostrar los empleados y su información de tarjeta de tiempo asociada. Comenzaremos mediante el uso de la siguiente "objetos CLR" (poco). 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Estas definiciones de clase cambiará ligeramente como exploramos diferentes enfoques y las características de EF4, pero la intención es conservar estas clases ignoran la persistencia (PI) como sea posible. Un objeto PI desconoce *cómo*, o incluso *si*, el estado contiene se encuentra dentro de una base de datos. PI y poco va mano a mano con software comprobable. Los objetos con un enfoque POCO son menos restringida, más flexibles y fáciles de probar porque se pueden usar sin una base de datos está presente.

Con los objetos poco en su lugar que podemos crear un Entity Data Model (EDM) en Visual Studio (consulte la figura 1). No se utilizará el EDM para generar código para nuestras entidades. En su lugar, queremos usar las entidades que elaborar cariñosamente a mano. Solo se utilizará el EDM para generar el esquema de base de datos y proporcionar los metadatos de que ef4 necesita para asignar objetos a la base de datos.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Figura 1**

Nota: si desea desarrollar el modelo EDM en primer lugar, es posible limpiar, generar código POCO del EDM. Puede hacerlo con una extensión de Visual Studio 2010 ofrecida el equipo de programabilidad de datos. Para descargar la extensión, inicie el Administrador de extensiones en el menú Herramientas en Visual Studio y busque "POCO" (vea la ilustración 2) en la Galería de plantillas en línea. Hay varias plantillas POCO para EF. Para obtener más información sobre el uso de la plantilla, vea " [Tutorial: plantilla de POCO para Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Figura 2**

Desde este punto de partida de POCO exploraremos dos enfoques diferentes para código fácil de probar. El primer enfoque llamo el enfoque EF ya que aprovecha las abstracciones de la API de Entity Framework para implementar unidades de trabajo y repositorios. En el segundo enfoque crearemos nuestras propio abstracciones de repositorio personalizado y, a continuación, vea las ventajas y desventajas de cada enfoque. Comenzaremos explorando el enfoque EF.  

### <a name="an-ef-centric-implementation"></a>Una implementación centrados en EF

Tenga en cuenta la siguiente acción del controlador de un proyecto de ASP.NET MVC. Esta acción recupera un objeto Employee y devuelve un resultado para mostrar una vista detallada del empleado.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

¿Es fácil de probar el código? Hay al menos dos pruebas que necesitamos para comprobar el comportamiento de la acción. En primer lugar, nos gustaría comprobar que la acción devuelve la vista correcta: una prueba sencilla. ¿También queremos escribir una prueba para comprobar la acción recupera al empleado correcto y nos gustaría hacerlo sin ejecutar código para consultar la base de datos. Recuerde que queremos aislar el código sometido a prueba. Aislamiento garantizará que la prueba no producirá un error debido a un error en la configuración de la base de datos o el código de acceso a datos. Si se produce un error en la prueba, se sabrá que tenemos un error en la lógica del controlador y no en uno de los componentes del sistema de nivel inferior.

Para conseguir el aislamiento que necesitaremos algunas abstracciones como las interfaces que se presentan anteriormente para los repositorios y las unidades de trabajo. Recuerde que el modelo de repositorio está diseñado para mediar entre objetos de dominio y la capa de asignación de datos. En este escenario EF4 *es* la asignación de datos de capa y ya proporciona una abstracción de repositorio denominada IObjectSet&lt;T&gt; (desde el espacio de nombres System.Data.Objects). La definición de interfaz es similar a la siguiente.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt; cumple los requisitos para un repositorio porque se parece a una colección de objetos (a través de IEnumerable&lt;T&gt;) y proporciona métodos para agregar y quitar objetos de la colección simulada. Los métodos Attach y Detach exponen funcionalidades adicionales de la API de EF4. Para usar IObjectSet&lt;T&gt; como interfaz para repositorios se necesita una unidad de trabajo abstracción para enlazar juntos los repositorios.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Una implementación concreta de esta interfaz se comunicará con SQL Server y es fácil crear mediante la clase ObjectContext de EF4. La clase ObjectContext es la unidad real del trabajo en la API de EF4.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Poner un IObjectSet&lt;T&gt; cobren vida es tan sencillo como invocar el método CreateObjectSet del objeto ObjectContext. En segundo plano, el marco de trabajo usará los metadatos se proporciona en el EDM para generar una clase ObjectSet concreta&lt;T&gt;. Nos centraremos con devolver el IObjectSet&lt;T&gt; porque le ayudará a conservar la capacidad de prueba en el código de cliente de la interfaz.

Esta implementación concreta es útil en producción, pero es necesario centrarse en cómo vamos a usar nuestra abstracción IUnitOfWork para facilitar las pruebas.

### <a name="the-test-doubles"></a>Las dobles de pruebas

Para aislar la acción de controlador necesitaremos la capacidad para cambiar entre la unidad real del trabajo (respaldado por un ObjectContext) y una unidad de doble o "falso" de prueba de trabajo (realizando las operaciones en memoria). El enfoque común para realizar este tipo de cambio es para no permitir que el controlador de MVC a crear una instancia una unidad de trabajo, pero en su lugar, pase la unidad de trabajo en el controlador como un parámetro de constructor.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

El código anterior es un ejemplo de inserción de dependencias. No se permite el controlador crear su dependencia (la unidad de trabajo) pero insertar la dependencia en el controlador. En un proyecto MVC es habitual usar una fábrica de controladores personalizada en combinación con un contenedor de inversión de control (IoC) para automatizar la inserción de dependencias. Estos temas están fuera del ámbito de este artículo, pero puede leer más por las siguientes las referencias al final de este artículo.

Una unidad falsa de implementación de trabajo que podemos usar para realizar pruebas podría ser similar al siguiente.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Tenga en cuenta que la unidad de trabajo falsa expone una propiedad de confirmado. A veces resulta útil agregar características a una clase falsa que facilitar las pruebas. En este caso es fácil observar si el código confirma una unidad de trabajo mediante la comprobación de la propiedad confirmado.

También necesitaremos una falsa IObjectSet&lt;T&gt; para contener objetos Employee y tarjetas de registro en la memoria. Podemos proporcionar una única implementación de uso de genéricos.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Este doble de pruebas delega la mayor parte de su trabajo a un objeto HashSet subyacente&lt;T&gt; objeto. Tenga en cuenta que IObjectSet&lt;T&gt; requiere una restricción genérica aplicando T como una clase (un tipo de referencia) y también nos vemos obligados a implementar IQueryable&lt;T&gt;. Es fácil crear una colección en memoria aparecen como un elemento IQueryable&lt;T&gt; mediante el operador LINQ estándar AsQueryable.

### <a name="the-tests"></a>Las pruebas

Pruebas unitarias tradicionales usará una única clase de prueba que contenga todas las pruebas para todas las acciones en un único controlador MVC. Podemos escribir estas pruebas, o cualquier tipo de prueba unitaria, fakes utilizando la memoria que creamos. Sin embargo, en este artículo que se evitará la aplicación monolítica enfoque de la clase de prueba y nuestras pruebas para centrarse en una parte específica de la funcionalidad de grupo en su lugar.  Por ejemplo, "crear a nuevo empleado" podría ser la funcionalidad que va a probar, por lo que se usará una única clase de prueba para comprobar la acción de controlador único responsable de crear a un nuevo empleado.

Hay algún código de instalación común que necesitamos para todas estas clases de prueba específica. Por ejemplo, siempre se debe crear los repositorios de en memoria y falsa unidad de trabajo. También necesitamos una instancia del controlador de empleado con la unidad de trabajo insertado falsa. Compartiremos este código de configuración comunes entre las clases de prueba mediante el uso de una clase base.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

La "madre de objeto" se usa en la clase base es un patrón común para crear datos de prueba. Una madre de objeto contiene los métodos de generador para crear instancias de entidades de prueba para su uso a través de varios accesorios de prueba.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

Podemos usar el EmployeeControllerTestBase como clase base para un número de accesorios de prueba (consulte la figura 3). Cada accesorio de prueba a probar una acción de controlador específica. Por ejemplo, un accesorio de prueba se centrará en las pruebas de la acción Create utilizada durante una solicitud HTTP GET (para mostrar la vista para la creación de un empleado), y un accesorio diferentes se centrará en la acción Create utilizada en una solicitud HTTP POST (para aprovechar la información presentada por el usuario para crear a un empleado). Cada clase derivada solo es responsable de la configuración necesaria en su contexto específico y para proporcionar las aserciones necesarios para comprobar los resultados de su contexto de la prueba específica.

![EF test_03](~/ef6/media/eftest-03.png)

**Figura 3**

El estilo de prueba y la convención de nomenclatura presentado aquí no es necesario para el código comprobable: es solo un enfoque. Figura 4 muestra las pruebas que se ejecutan en el Resharper cerebro Jet probar el complemento de ejecutor para Visual Studio 2010.

![EF test_04](~/ef6/media/eftest-04.png)

**Figura 4**

Con una clase base para controlar el código del programa de instalación compartido, las pruebas unitarias para cada acción de controlador son pequeños y fáciles de escribir. Las pruebas se ejecutarán rápidamente (ya que se está realizando operaciones en memoria) y no deberían producir un error debido a la infraestructura no relacionada o preocupaciones medioambientales (ya que nos hemos aislado la unidad sometida a prueba).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

En estas pruebas, la clase base realiza la mayor parte del trabajo del programa de instalación. Recuerde que el constructor de clase base crea una instancia de la clase EmployeeController, una unidad de trabajo falsa y el repositorio en memoria. La clase de prueba se deriva de esta clase base y se centra en los detalles de las pruebas al método Create. En este caso los detalles se limitan a los pasos de "Organizar, actuar y afirmar" podrá ver en cualquier unidad de prueba de procedimiento:

-   Cree un objeto newEmployee para simular los datos entrantes.
-   Invocar la acción Create de la EmployeeController y pase el newEmployee.
-   Compruebe que la acción Create genera los resultados esperados (el empleado aparecerá en el repositorio).

Lo que hemos creado nos permite probar cualquiera de las acciones de EmployeeController. Por ejemplo, cuando escribimos las pruebas para la acción Index del controlador de empleado se pueden heredar de la clase base de prueba para establecer la misma configuración básica para nuestras pruebas. Vuelva a la clase base creará una instancia de la EmployeeController, la unidad de trabajo falsa y el repositorio en memoria. Las pruebas para la acción del índice solo tiene que centrarse en invocar la acción del índice y probar las cualidades del modelo de la acción devuelve.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

Las pruebas que vamos a crear con fakes en memoria están orientadas a probar la *estado* del software. ¿Por ejemplo, al probar la acción de crear que desea inspeccionar el estado del repositorio cuando se ejecuta la acción create: el repositorio contiene al nuevo empleado?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Más adelante veremos las pruebas en función de interacción. Las pruebas en función de interacción, se le preguntará si el código sometido a prueba invoca los métodos adecuados en nuestros objetos y pasa los parámetros correctos. Por ahora, pasaremos la cubierta otro patrón de diseño de la carga diferida.

## <a name="eager-loading-and-lazy-loading"></a>La carga diligente y la carga diferida

En algún momento en el sitio web de ASP.NET MVC que queramos para mostrar información de un empleado e incluir al empleado de aplicación asociados a tarjetas de tiempo. Por ejemplo, podríamos tenemos una pantalla de resumen de la tarjeta de tiempo que muestra el nombre del empleado y el número total de tarjetas de tiempo en el sistema. Hay varios enfoques que podemos adoptar para implementar esta característica.

### <a name="projection"></a>Proyección

Es un enfoque sencillo para crear el resumen construir un modelo dedicado a la información que desea mostrar en la vista. En este escenario, el modelo podría ser similar al siguiente.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Tenga en cuenta que el EmployeeSummaryViewModel no es una entidad; en otras palabras no es algo que deseamos conservar en la base de datos. Sólo vamos a usar esta clase para distribuye aleatoriamente los datos en la vista de una manera fuertemente tipada. El modelo de vista es como un datos transferir el objeto (DTO) porque no contiene ningún comportamiento (no hay métodos): solo las propiedades. Las propiedades contendrá los datos que necesitamos pasar. Es fácil crear una instancia de este modelo de vista mediante el operador de proyección estándar de LINQ: el operador Select.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Hay dos características importantes para el código anterior. En primer lugar, el código es muy fácil probar porque está siendo fácil observar y aislar. El operador Select funciona igual de bien con nuestros fakes en memoria como lo hace en la unidad de trabajo real.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

La segunda característica importante es cómo el código permite EF4 generar una consulta única y eficiente para ensamblar la información del empleado y tarjeta de tiempo entre sí. Información de los empleados y la información de tarjeta de tiempo se han cargado en el mismo objeto sin usar ninguna API especial. El código simplemente expresa la información requiere el uso de operadores de LINQ estándar que funcionan con orígenes de datos en memoria, así como orígenes de datos remotos. Fue capaz de traducir los árboles de expresión generados por la consulta LINQ y C EF4\# compilador en una consulta de Transact-SQL única y eficiente.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Existen otras ocasiones cuando no queremos trabajar con un modelo de vista o un objeto DTO, pero con las entidades reales. Cuando se sabe que necesitamos un empleado *y* tarjetas de tiempo del empleado, concienzudamente, se puede cargar los datos relacionados de forma discreta y eficaz.

### <a name="explicit-eager-loading"></a>Carga diligente explícita

Cuando desea cargar concienzudamente información de entidad relacionada que necesitamos algún mecanismo para la lógica de negocios (o en este escenario, la lógica de acciones de controlador) para expresar su deseo de que el repositorio. La ObjectQuery EF4&lt;T&gt; clase define un método Include para especificar los objetos relacionados que se va a recuperar durante una consulta. Recuerde que ObjectContext EF4 expone las entidades a través de ObjectSet concreta&lt;T&gt; clase que hereda de ObjectQuery&lt;T&gt;.  Si usamos ObjectSet&lt;T&gt; referencias en la acción de controlador podríamos escribir el código siguiente para especifican una carga diligente de información de tarjeta de tiempo para cada empleado.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Sin embargo, puesto que estamos intentando mantener nuestro código comprobable nos estamos exponer no ObjectSet&lt;T&gt; desde fuera de la unidad de la clase de trabajo real. En su lugar, nos basamos en el IObjectSet&lt;T&gt; interfaz que es más fácil de imitar, pero IObjectSet&lt;T&gt; no define un método Include. La belleza de LINQ es que podemos crear nuestro propio operador Include.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Tenga en cuenta este operador de inclusión se define como un método de extensión de IQueryable&lt;T&gt; en lugar de IObjectSet&lt;T&gt;. Esto nos da la capacidad de utilizar el método con una gama más amplia de tipos posibles, incluidos IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;y ObjectSet&lt;T&gt;. En caso de que la secuencia subyacente no es un auténtico ObjectQuery EF4&lt;T&gt;, a continuación, no hay ningún daño que realiza y el operador de inclusión es una operación inefectiva. Si la secuencia subyacente *es* una ObjectQuery&lt;T&gt; (o derivado de ObjectQuery&lt;T&gt;), EF4 se vea nuestro requisito para los datos adicionales y formular SQL adecuado consulta.

Con este operador new en su lugar, podemos solicitar explícitamente una carga diligente de información de tarjeta de tiempo desde el repositorio.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Cuando se ejecuta en un ObjectContext con establecimiento real, el código produce la siguiente consulta única. La consulta recopila información suficiente de la base de datos en un solo viaje para materializar los objetos de empleados y rellenar totalmente su propiedad de tarjetas de registro.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

La buena noticia es que el código dentro del método de acción siga siendo fácil de probar por completo. No necesitamos proporcionar características adicionales para nuestros emulaciones admitir el operador de inclusión. Las malas noticias son que tuvimos que utilizar el operador de inclusión dentro del código que deseáramos mantener que ignoran la persistencia. Esto es un buen ejemplo del tipo de equilibrios, deberá evaluar al compilar código fácil de probar. Hay veces cuando se necesitan para permitir que la pérdida de inquietudes de persistencia fuera de la abstracción de repositorio para satisfacer los objetivos de rendimiento.

La alternativa a la carga diligente es la carga diferida. Medio de carga diferida hacemos *no* necesita nuestro código de negocio para anunciar explícitamente el requisito para los datos asociados. En su lugar, usamos nuestras entidades en la aplicación y si están datos adicionales necesitan Entity Framework cargará los datos a petición.

### <a name="lazy-loading"></a>Carga diferida

Es fácil imaginar un escenario donde no sabemos qué hay datos en una pieza de lógica de negocios. Podríamos sabemos que la lógica necesita un objeto employee, pero nos podemos crear bifurcaciones en diferentes rutas de ejecución donde algunas de esas rutas de acceso requieren información de tarjeta de tiempo del empleado y otras no. Escenarios como éste son perfectos para la carga diferida implícita porque mágicamente aparecen datos según sea necesario.

La carga diferida, también conocido como aplazada cargando, colocar algunos requisitos en nuestros objetos entidad. Poco con omisión de persistencia verdadero no tendría que enfrentarse a los requisitos de la capa de persistencia, pero es prácticamente imposible lograr la omisión de persistencia es true.  En su lugar, medimos la omisión de persistencia en grados relativos. Si se necesita heredar de una clase base orientada a servicios de persistencia o usar una colección especializada para lograr la carga diferida en poco sería lamentable. Afortunadamente, EF4 tiene una solución menos intrusiva.

### <a name="virtually-undetectable"></a>Prácticamente indetectables

Al usar objetos POCO, EF4 puede generar dinámicamente en tiempo de ejecución los servidores proxy para las entidades. Estos servidores proxy imperceptible encapsular los objetos materializados poco y proporcionan obtención servicios adicionales mediante la interceptación de cada propiedad y realizar trabajo adicional de la operación de establecimiento. Uno de estos servicios es la característica de carga diferida que estamos buscando. Otro servicio es un mecanismo que puede grabar cuando el programa cambia los valores de propiedad de una entidad de seguimiento de cambios eficaz. La lista de cambios se usa ObjectContext durante el método SaveChanges para conservar las entidades modificadas mediante comandos de actualización.

Para estos servidores proxy para que funcione, sin embargo, que necesitan una manera de enlazar propiedad get y operaciones set en una entidad y los servidores proxy de lograr este objetivo mediante la invalidación de los miembros virtuales. Por lo tanto, si queremos tener la carga diferida implícita y seguimiento de cambios eficaz se debe volver a nuestro definiciones de clase POCO y marcar las propiedades como virtual.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Aún podemos decir que la entidad de empleado es principalmente que ignoran la persistencia. El único requisito consiste en usar a los miembros virtuales, y esto no afecta a la capacidad de prueba del código. No tenemos que derivan de cualquier clase base especial, o incluso usar una colección especial dedicada a la carga diferida. Como se muestra el código, cualquier clase que implementa ICollection&lt;T&gt; está disponible para almacenar las entidades relacionadas.

También hay un cambio menor que necesitamos hacer dentro de nuestra unidad de trabajo. La carga diferida es *desactivar* de forma predeterminada cuando se trabaja directamente con un objeto ObjectContext. Hay una propiedad que podemos establecer en la propiedad ContextOptions para habilitar la carga diferida y podemos establecer esta propiedad dentro de nuestra unidad de trabajo real si desea habilitar la carga diferida en todas partes.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Implícita carga diferida habilitada, el código de aplicación puede usar a un empleado y el empleado asociadas tarjetas de tiempo sin dejar constancia del trabajo necesario para que EF que cargue los datos adicionales.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

La carga diferida hace más fácil de escribir el código de aplicación y con la magia de proxy el código siga siendo fácil de probar completamente. Fakes en memoria de la unidad de trabajo pueden precargar simplemente falsas entidades con asociados datos cuando sea necesario durante una prueba.

En este momento centraremos nuestra atención desde la creación de repositorios con IObjectSet&lt;T&gt; y examine las abstracciones para ocultar todos los signos de que el marco de persistencia.

## <a name="custom-repositories"></a>Repositorios personalizados

Cuando se presenta primero la unidad de trabajo del patrón de diseño en este artículo se proporcionan algunos ejemplos de código para el aspecto que es posible que la unidad de trabajo. Vamos a volver a presentar esta idea original con el empleado y el escenario de las tarjetas de tiempo empleado con que eso hemos trabajado.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

La principal diferencia entre esta unidad de trabajo y la unidad de trabajo que hemos creado en la última sección es cómo esta unidad de trabajo no utiliza las abstracciones de EF4 framework (no hay ningún IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; funciona bien como una interfaz de repositorio, pero la API que expone no puede alinear perfectamente con las necesidades de la aplicación. En este enfoque próximo se representará mediante un IRepository personalizado de repositorios&lt;T&gt; abstracción.

Muchos desarrolladores que siguen el diseño controlado por pruebas, Diseño controlado por comportamiento y el diseño de las metodologías de dominio controlado por prefieren el IRepository&lt;T&gt; enfoque por varias razones. En primer lugar, el IRepository&lt;T&gt; interfaz representa una capa "para evitar daños". Como se describe de Eric Evans en su libro Domain Driven Design una capa anticorrupción mantiene el código de dominio fuera de la infraestructura de las API, como una API de persistencia. En segundo lugar, los desarrolladores pueden crear métodos en el repositorio que satisfacen las necesidades exactas de una aplicación (como detectado al escribir las pruebas). Por ejemplo, puede que con frecuencia sea necesario buscar una sola entidad con un valor de identificador, por lo que podemos agregar un método FindById a la interfaz del repositorio.  Nuestro IRepository&lt;T&gt; definición tendrá un aspecto similar al siguiente.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Tenga en cuenta colocaremos volver a usar un tipo IQueryable&lt;T&gt; interfaz para exponer las colecciones de entidades. IQueryable&lt;T&gt; permite que los árboles de expresión LINQ fluyen en el proveedor de EF4 y dar el proveedor de una vista holística de la consulta. Otra opción sería devolver IEnumerable&lt;T&gt;, lo que significa que el proveedor LINQ de EF4 sólo verán las expresiones generadas dentro del repositorio. Cualquier agrupación, ordenación y llevar a cabo fuera del repositorio de proyección no se crean en el comando SQL que se envían a la base de datos, que puede afectar negativamente al rendimiento. Por otro lado, un repositorio de devolver IEnumerable solo&lt;T&gt; resultados nunca lo sorprenderá con un nuevo comando SQL. Ambos enfoques funcionará, y ambos enfoques permanecen apta para las pruebas.

Resulta sencillo proporcionar una implementación única de la IRepository&lt;T&gt; interfaz con los genéricos y la API de ObjectContext de EF4.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

El IRepository&lt;T&gt; enfoque nos da algún control adicional sobre las consultas porque tiene un cliente invocar un método para llegar a una entidad. Dentro del método nos podríamos proporcionar comprobaciones adicionales y los operadores LINQ para exigir restricciones de la aplicación. Tenga en cuenta que la interfaz tiene dos restricciones en el parámetro de tipo genérico. La primera restricción es la su sabor desventajas de clase requerido por ObjectSet&lt;T&gt;, y la segunda restricción obliga a nuestras entidades para implementar IEntity: una abstracción que creó para la aplicación. La interfaz IEntity obliga a las entidades tengan una propiedad de Id. de lectura y, a continuación, podemos usar esta propiedad en el método FindById. IEntity se define con el código siguiente.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity podría considerarse una infracción de omisión de persistencia pequeña puesto que nuestras entidades son necesarios para implementar esta interfaz. Recuerde la omisión de persistencia es sobre compromisos, y para muchos la funcionalidad FindById compensarán el restricciones impuestas por la interfaz. La interfaz no influye en la capacidad de prueba.

Crear una instancia de un IRepository live&lt;T&gt; requiere un ObjectContext EF4, por lo que una unidad de implementación de trabajo concreta debe administrar la creación de instancias.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>Mediante el repositorio personalizado

Utilizando nuestro repositorio personalizado no es significativamente diferente de mediante el repositorio según IObjectSet&lt;T&gt;. En lugar de aplicar los operadores de LINQ directamente a una propiedad, primero necesitaremos uno invocar métodos del repositorio para tomar un tipo IQueryable&lt;T&gt; referencia.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Tenga en cuenta el operador de inclusión personalizado, implementado previamente funcionará sin cambios. Método FindById del repositorio quita duplicado lógica de acciones intenta recuperar una sola entidad.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

No hay ninguna diferencia significativa en las pruebas unitarias para los dos enfoques que hemos examinado. Podríamos proporcionar implementaciones falsas de IRepository&lt;T&gt; mediante la creación de clases concretas respaldadas por HashSet&lt;empleado&gt; : al igual que lo que hicimos en la última sección. Sin embargo, algunos desarrolladores prefieren usar los objetos ficticios y simular los marcos de objeto en lugar de generar emulaciones. Examinaremos usar simulaciones en nuestra implementación de prueba y analizar las diferencias entre los objetos ficticios y emulaciones en la sección siguiente.

### <a name="testing-with-mocks"></a>Las pruebas con objetos ficticios

Existen diferentes enfoques para crear una prueba de"double" de Martin Fowler lo que se llama. Una prueba doble (por ejemplo, un entorno de cinematográfico película doble) es un objeto que se va a compilar "stand" para el real, los objetos de producción durante las pruebas. Los repositorios en memoria, que hemos creado son dobles de pruebas para los repositorios que se comunican con SQL Server. Hemos visto cómo usar estos dobles de pruebas durante las pruebas unitarias para aislar el código y mantener las pruebas que se ejecutan rápidamente.

Las dobles de pruebas, que hemos creado tienen implementaciones reales, el trabajo. En segundo plano cada uno de ellos almacena una colección de objetos concretos, y agregará y quitar objetos de esta colección mientras se manipulan el repositorio durante una prueba. Algunos desarrolladores como compilar los dobles de pruebas de este modo: con el código real y las implementaciones de trabajo.  Estos valores Double de prueba son las que denominamos *fakes*. Tienen implementaciones de trabajo, pero no son lo suficientemente reales para su uso en producción. El repositorio falso en realidad no escribe en la base de datos. El servidor SMTP falso realmente no envía un mensaje de correo electrónico a través de la red.

### <a name="mocks-versus-fakes"></a>Simulacros frente a Fakes

Hay otro tipo de prueba de doble se conoce como un *simular*. Aunque fakes tienen implementaciones de trabajo, objetos ficticios vienen con ninguna implementación. Con la Ayuda de un marco de objeto ficticio construimos estos objetos ficticios en tiempo de ejecución y usarlas como dobles de pruebas. En esta sección vamos a usar el marco Moq de simulación de código abierto. Este es un ejemplo sencillo de usar Moq para crear dinámicamente una prueba doble para un repositorio de empleado.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Le pedimos Moq IRepository&lt;empleado&gt; implementación y se crea uno dinámicamente. Podemos obtener para el objeto que implementa IRepository&lt;empleado&gt; mediante el acceso a la propiedad del objeto de la simulación&lt;T&gt; objeto. Es este objeto interno para pasar en nuestros controladores, y no sabrán si se trata de un doble de pruebas o en el repositorio real. Tal como se podría invocar métodos en un objeto con una implementación real que podemos llamar a métodos en el objeto.

Debe se esté preguntando lo hará el repositorio ficticio cuando se invoca el método Add. Dado que no hay ninguna implementación detrás del objeto ficticio, agregar no hace nada. No hay ninguna colección concreta en segundo plano como la que teníamos con las emulaciones que escribimos, por lo que el empleado se descarta. ¿Qué sucede con el valor devuelto de FindById? En este caso el objeto ficticio hace lo único que puede hacer, que devuelve un valor predeterminado. Puesto que nos estamos devolviendo un tipo de referencia (un empleado), el valor devuelto es un valor null.

Simulacros podrían parecer inútil; Sin embargo, hay otras dos características de los simulacros que no hemos hablado. En primer lugar, el marco de trabajo Moq registra todas las llamadas realizadas en el objeto ficticio. Más adelante en el código podemos hacer Moq si nadie invoca el método Add, o si alguien ha llamado al método FindById. Veremos más adelante cómo podemos usar esta característica de grabación de "caja negra" en las pruebas.

La segunda característica excelente es cómo podemos usar Moq para programar un objeto ficticio con *expectativas*. Una expectativa indica el objeto ficticio cómo responder a ninguna interacción dada. Por ejemplo, podemos programar una expectativa en nuestra simulacro y decirle que devuelven un objeto employee cuando alguien llama a FindById. El marco de Moq usa una API de instalación y las expresiones lambda para programar estas expectativas.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

En este ejemplo, le pedimos Moq para generar dinámicamente un repositorio y, a continuación, se programa el repositorio con una expectativa. La expectativa indica el objeto ficticio para devolver un nuevo objeto de empleado con un valor de identificador de 5 cuando un usuario invoca el método FindById pasando un valor de 5. Esta prueba se supera, y no necesitamos crear una implementación completa a IRepository falsa&lt;T&gt;.

Vamos a volver a visitar las pruebas que hemos escrito anteriormente y rehacer para que usen los simulacros en lugar de fakes. Al igual que antes, vamos a usar una clase base para configurar las partes comunes de infraestructura que necesitamos para todas las pruebas del controlador.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

El código del programa de instalación principalmente permanece igual. En lugar de usar fakes, vamos a usar Moq para construir objetos ficticios. Organiza la clase base para que el simulacro unidad de trabajo devolver un repositorio ficticio cuando el código invoca la propiedad de los empleados. El resto de la configuración de simulacro llevará a cabo dentro de los accesorios de prueba dedicado a cada situación específica. Por ejemplo, el accesorio de prueba para la acción del índice configurará el repositorio ficticio para devolver una lista de empleados, cuando la acción invoca el método FindAll del repositorio ficticio.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Excepto las expectativas, un aspecto similares a las pruebas que teníamos antes de nuestras pruebas. Sin embargo, con la capacidad de grabación de un marco ficticio podemos enfocamos pruebas desde un ángulo diferente. Examinaremos esta nueva perspectiva en la sección siguiente.

### <a name="state-versus-interaction-testing"></a>Estado frente a las pruebas de interacción

Existen diferentes técnicas que puede usar para probar el software con objetos ficticios. Un enfoque es utilizar el estado de las pruebas, que es lo que hemos hecho en este artículo hasta el momento. Probar aserciones hace sobre el estado del software basado en Estados. En la última prueba se invoca un método de acción en el controlador y se realiza una aserción sobre el modelo que se debe compilar. Estos son algunos otros ejemplos de estado de prueba:

-   Compruebe que el repositorio contiene el nuevo objeto employee después de crear se ejecuta.
-   Compruebe que el modelo contiene una lista de todos los empleados, cuando se ejecuta el índice.
-   Compruebe que el repositorio no tiene a un empleado determinado cuando se ejecuta Delete.

Otro enfoque verá con objetos ficticios que consiste en comprobar *interacciones*. Mientras prueba aserciones hace sobre el estado de objetos basado en Estados, interacción en función probar aserciones hace acerca de cómo interactúan los objetos. Por ejemplo:

-   Compruebe que el controlador invoca el método Add del repositorio cuando se ejecuta Create.
-   Compruebe que el controlador invoca el método FindAll del repositorio cuando se ejecuta el índice.
-   Compruebe que el controlador invoca la unidad del método de confirmación del trabajo para guardar los cambios cuando se ejecuta la edición.

A menudo interacción pruebas requiere menos datos de prueba, porque no estamos buscando reiteradamente dentro de las colecciones y comprobar los recuentos. Por ejemplo, si conocemos el método FindById de un repositorio con el valor correcto - que invoca la acción de detalles, a continuación, la acción es probablemente comportan correctamente. Podemos comprobar este comportamiento sin tener que configurar los datos de prueba para devolver de FindById.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

El programa de instalación solo requerido en el accesorio de prueba anterior es la configuración proporcionada por la clase base. Cuando se invoca la acción de controlador, Moq registrará las interacciones con el repositorio ficticio. Mediante la API de comprobación de Moq, podemos hacer Moq si el controlador invocado FindById con el valor de identificador adecuado. Si el controlador no invocó el método, o invoca el método con un valor de parámetro inesperado, el método de comprobación generará una excepción y se producirá un error en la prueba.

Este es otro ejemplo para comprobar que la acción Create invoca confirmación en la unidad de trabajo actual.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Un peligro con las pruebas de interacción es la tendencia a través de especificar las interacciones. La capacidad del objeto ficticio para grabar y comprobar que cada interacción con el objeto ficticio no significa que la prueba debe intentar comprobar cada interacción. Algunos tipos de interacción son detalles de implementación y sólo debe comprobar las interacciones *requiere* para satisfacer la prueba actual.

La elección entre los objetos ficticios o fakes depende en gran medida el sistema que se está probando y su personal (o equipo) Preferencias. Los objetos ficticios pueden reducir drásticamente la cantidad de código que necesita para implementar los dobles de pruebas, pero no todo el mundo es cómodo las expectativas de programación y comprobando las interacciones.

## <a name="conclusions"></a>Conclusiones

En este artículo hemos mostrado varios enfoques para crear código fácil de probar al usar ADO.NET Entity Framework para la persistencia de datos. Podemos aprovechar abstracciones integradas como IObjectSet&lt;T&gt;, o crear nuestras propio abstracciones como IRepository&lt;T&gt;.  En ambos casos, la compatibilidad POCO en ADO.NET Entity Framework 4.0 permite que los consumidores de estas abstracciones permanezcan persistentes que ignoran la persistencia y muy fácil de probar. Características adicionales de EF4, como la carga diferida implícita permite el servicio de aplicación y empresarial de código para que funcione sin preocuparse por los detalles de un almacén de datos relacional. Por último, las abstracciones que creamos son fáciles de simulacro o falsa dentro de las pruebas unitarias, y podemos usar estos dobles de pruebas para lograr la ejecución rápida, altamente aislada y las pruebas fiables.

### <a name="additional-resources"></a>Recursos adicionales

-   Robert C. Martin, " [el principio de responsabilidad única](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler, [catálogo de patrones de](http://www.martinfowler.com/eaaCatalog/index.html) desde *patrones de arquitectura de aplicaciones de empresa*
-   Griffin Caprio, " [inserción de dependencias](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Blog de programación de datos, " [Tutorial: desarrollo con Entity Framework 4.0 controlado por pruebas](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Blog de programación de datos, " [patrones de uso de repositorio y unidad de trabajo con Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels, " [Introducción BDD](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen, " [Introducción a las especificaciones de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [BDD con MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans, " [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler, " [las simulaciones no son códigos auxiliares](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler, " [doble de pruebas](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, " [estado frente a las pruebas de interacción](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Biografía

Scott Allen es un miembro del personal técnico en Pluralsight y fundador de OdeToCode.com. De 15 años de desarrollo de software comercial, Scott ha trabajado en soluciones para todo, desde dispositivos incrustados de 8 bits a las aplicaciones web ASP.NET altamente escalables. Puede llegar a Scott en su blog en OdeToCode o en Twitter en [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).
