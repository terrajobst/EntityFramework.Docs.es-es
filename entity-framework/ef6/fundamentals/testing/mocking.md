---
title: Las pruebas con un marco de simulación - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: abb915f2df5645b3838a659dafc59a2b499b4ee2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998218"
---
# <a name="testing-with-a-mocking-framework"></a>Las pruebas con un marco de simulación
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Al escribir pruebas para la aplicación suele ser deseable para evitar llegar a la base de datos.  Entity Framework le permite conseguir esto mediante la creación de un contexto – con el comportamiento definido por las pruebas, que hace uso de datos en memoria.  

## <a name="options-for-creating-test-doubles"></a>Opciones para crear los dobles de pruebas  

Existen dos enfoques diferentes que pueden usarse para crear una versión en memoria de su contexto.  

- **Crear sus propio dobles de pruebas** – este enfoque implica escribir su propia implementación en memoria de su contexto y DbSets. Esto le ofrece un gran control sobre cómo se comportan las clases, pero pueden implicar escribir y poseer una cantidad razonable de código.  
- **Usar un marco de simulación para crear los dobles de pruebas** : mediante un marco de simulación (como Moq) puede tener las implementaciones en memoria que contexto y los conjuntos creados dinámicamente en tiempo de ejecución para usted.  

En este artículo se tratará con el uso de un marco de simulación. Para crear sus propio dobles de pruebas, vea [las pruebas con su dobles de pruebas propio](writing-test-doubles.md).  

Para demostrar el uso de EF con un marco de simulación, vamos a usar Moq. La manera más fácil de obtener Moq es instalar el [Moq paquete de NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Las pruebas con las versiones anteriores a EF6  

El escenario mostrado en este artículo depende de algunos cambios que realizamos en DbSet en EF6. Para las pruebas con EF5 y una versión anterior, consulte [las pruebas con un contexto de imitar](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitaciones de EF en memoria de dobles de pruebas  

Dobles de pruebas en memoria pueden ser una buena forma de proporcionar cobertura de nivel de bits de la aplicación que usan EF de prueba unitaria. Sin embargo, al hacerlo se mediante LINQ to Objects para ejecutar consultas en datos en memoria. Esto puede provocar un comportamiento diferente que el proveedor LINQ de EF (LINQ to Entities) para traducir consultas en SQL que se ejecuta en la base de datos.  

Un ejemplo de esta diferencia está cargando datos relacionados. Si crea una serie de Blogs de publicaciones que cada relacionadas, a continuación, cuando se usan los datos en memoria siempre se cargarán esas entradas para cada Blog. Sin embargo, cuando se ejecuta en una base de datos solo se cargará los datos si usa el método Include.  

Por este motivo, se recomienda incluir siempre cierto nivel de pruebas to-end (además de las pruebas unitarias) para asegurarse de la aplicación funciona correctamente en una base de datos.  

## <a name="following-along-with-this-article"></a>Siguiendo con este artículo.  

Este artículo proporcionan listas de código completo que se pueden copiar en Visual Studio para seguir el tutorial si lo desea. Es más fácil crear un **proyecto de prueba unitaria** y será necesario al destino **.NET Framework 4.5** para completar las secciones que se usa async.  

## <a name="the-ef-model"></a>El modelo de EF  

El servicio, vamos a probar hace uso de EF modelo formado por el BloggingContext y las clases de Blog y Post. Este código puede haber sido generado por el Diseñador de EF o ser un modelo de Code First.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a>Propiedades DbSet virtual con EF Designer  

Tenga en cuenta que las propiedades DbSet en el contexto se marcan como virtuales. Esto permitirá que el marco de simulación derivar desde nuestro contexto e invalidar estas propiedades con una implementación simulada.  

Si utiliza Code First puede editar las clases directamente. Si está utilizando el Diseñador de EF, a continuación, deberá editar la plantilla T4 que genera el contexto. Abra el \<model_name\>. Archivo Context.tt que está anidado bajo el archivo edmx, busque el siguiente fragmento de código y agregue la palabra clave virtual tal como se muestra.  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a>Servicio va a probar  

Para mostrar la prueba de dobles de pruebas en memoria que vamos a escribir un par de pruebas para un BlogService. El servicio es capaz de crear nuevos blogs (AddBlog) y devolver todos los Blogs ordenados por nombre (GetAllBlogs). Además de GetAllBlogs, también hemos proporcionado un método que obtendrá de forma asincrónica todos los blogs, ordenados por nombre (GetAllBlogsAsync).  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a>Escenarios de prueba que no son de consulta  

Eso es todo lo que necesitamos hacer para comenzar a probar los métodos que no son de consulta. La siguiente prueba usa Moq para crear un contexto. A continuación, crea una clase DbSet\<Blog\> y conecta lo va a devolver desde la propiedad de Blogs del contexto. A continuación, el contexto se utiliza para crear un nuevo BlogService que, a continuación, se usa para crear un nuevo blog: mediante el método AddBlog. Por último, la prueba comprueba que el servicio agrega un nuevo Blog y llama a SaveChanges en el contexto.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a>Escenarios de consulta de prueba  

Para poder ejecutar consultas en nuestra prueba DbSet doble es necesario configurar una implementación de IQueryable. El primer paso es crear algunos datos en memoria: estamos usando una lista\<Blog\>. A continuación, creamos un contexto y DBSet\<Blog\> , a continuación, conectar la implementación de IQueryable de la clase DbSet: solo va a delegar para el proveedor LINQ to Objects que funciona con la lista\<T\>.  

A continuación, podemos crear un BlogService según nuestro dobles de pruebas y asegúrese de que los datos, obtenemos desde GetAllBlogs se ordenan por nombre.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a>Las pruebas con las consultas asincrónicas

Entity Framework 6 introdujo un conjunto de métodos de extensión que puede usarse para ejecutar una consulta de forma asincrónica. Algunos ejemplos de estos métodos son ToListAsync, FirstAsync, ForEachAsync, etcetera.  

Dado que las consultas de Entity Framework hacen uso de LINQ, los métodos de extensión se definen en IQueryable y IEnumerable. Sin embargo, ya que solo están diseñados para usarse con Entity Framework puede recibir el siguiente error si intenta usarlas en una consulta LINQ que no es una consulta de Entity Framework:

> El origen de IQueryable no implementa IDbAsyncEnumerable{0}. Solo los orígenes que implementan IDbAsyncEnumerable pueden usarse para las operaciones asincrónicas de Entity Framework. Para obtener más información, consulte [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).  

Mientras que los métodos asincrónicos se admiten solo cuando se ejecuta en una consulta EF, desea usarlos en la prueba unitaria al doble de un DbSet de prueba de ejecución frente a en memoria.  

Para poder usar los métodos asincrónicos se deberá crear un DbAsyncQueryProvider en memoria para procesar la consulta asincrónica. Si bien sería posible configurar un proveedor de consultas con Moq, es mucho más fácil crear una implementación de dobles de prueba en el código. El código para esta implementación es como sigue:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

Ahora que tenemos un proveedor de consultas async podemos escribir una prueba unitaria para nuestro nuevo método GetAllBlogsAsync.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
