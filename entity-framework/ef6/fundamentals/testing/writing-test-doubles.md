---
title: Las pruebas con sus propio dobles de pruebas - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997222"
---
# <a name="testing-with-your-own-test-doubles"></a>Las pruebas con sus propio dobles de pruebas
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Al escribir pruebas para la aplicación suele ser deseable para evitar llegar a la base de datos.  Entity Framework le permite conseguir esto mediante la creación de un contexto – con el comportamiento definido por las pruebas, que hace uso de datos en memoria.  

## <a name="options-for-creating-test-doubles"></a>Opciones para crear los dobles de pruebas  

Existen dos enfoques diferentes que pueden usarse para crear una versión en memoria de su contexto.  

- **Crear sus propio dobles de pruebas** – este enfoque implica escribir su propia implementación en memoria de su contexto y DbSets. Esto le ofrece un gran control sobre cómo se comportan las clases, pero pueden implicar escribir y poseer una cantidad razonable de código.  
- **Usar un marco de simulación para crear los dobles de pruebas** : mediante un marco de simulación (como Moq) puede tener las implementaciones en memoria que contexto y los conjuntos creados dinámicamente en tiempo de ejecución para usted.  

En este artículo tratará la creación de su propia prueba doble. Para obtener información sobre el uso de un marco de simulación vea [las pruebas con un marco de simulación](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Las pruebas con las versiones anteriores a EF6  

El código mostrado en este artículo es compatible con EF6. Para las pruebas con EF5 y una versión anterior, consulte [las pruebas con un contexto de imitar](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Limitaciones de EF en memoria de dobles de pruebas  

Dobles de pruebas en memoria pueden ser una buena forma de proporcionar cobertura de nivel de bits de la aplicación que usan EF de prueba unitaria. Sin embargo, al hacerlo se mediante LINQ to Objects para ejecutar consultas en datos en memoria. Esto puede provocar un comportamiento diferente que el proveedor LINQ de EF (LINQ to Entities) para traducir consultas en SQL que se ejecuta en la base de datos.  

Un ejemplo de esta diferencia está cargando datos relacionados. Si crea una serie de Blogs de publicaciones que cada relacionadas, a continuación, cuando se usan los datos en memoria siempre se cargarán esas entradas para cada Blog. Sin embargo, cuando se ejecuta en una base de datos solo se cargará los datos si usa el método Include.  

Por este motivo, se recomienda incluir siempre cierto nivel de pruebas to-end (además de las pruebas unitarias) para asegurarse de la aplicación funciona correctamente en una base de datos.  

## <a name="following-along-with-this-article"></a>Siguiendo con este artículo.  

Este artículo proporcionan listas de código completo que se pueden copiar en Visual Studio para seguir el tutorial si lo desea. Es más fácil crear un **proyecto de prueba unitaria** y será necesario al destino **.NET Framework 4.5** para completar las secciones que se usa async.  

## <a name="creating-a-context-interface"></a>Creación de una interfaz de contexto  

Vamos a ver en las pruebas de un servicio que hace uso de EF modelo. Para poder reemplazar el contexto de EF con una versión en memoria para las pruebas, definiremos una interfaz que nuestro contexto EF (y su doble de memoria) seguirán imeplement.  

Vamos a probar el servicio de consultar y modificar datos mediante las propiedades DbSet de nuestro contexto y también llamar a SaveChanges para insertar los cambios en la base de datos. Por lo que hemos incluido a estos miembros en la interfaz.  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a>El modelo de EF  

El servicio, vamos a probar hace uso de EF modelo formado por el BloggingContext y las clases de Blog y Post. Este código puede haber sido generado por el Diseñador de EF o ser un modelo de Code First.  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementa la interfaz de contexto con EF Designer  

Tenga en cuenta que nuestro contexto implementa la interfaz IBloggingContext.  

Si utiliza Code First, a continuación, puede editar su contexto directamente para implementar la interfaz. Si está utilizando el Diseñador de EF, a continuación, deberá editar la plantilla T4 que genera el contexto. Abra el \<model_name\>. Archivo Context.tt que está anidado bajo el archivo edmx, busque el siguiente fragmento de código y agregue en la interfaz tal como se muestra.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
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
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
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

<a name="creating-the-in-memory-test-doubles"/> ## Duplica crear la prueba en memoria  

Ahora que tenemos el modelo de EF real y el servicio que puede usarlo, es momento de crear la prueba en memoria dobles que podemos usar para realizar pruebas. Hemos creado una prueba TestContext dobles para nuestro contexto. Llegamos al elegir el comportamiento que queremos para admitir las pruebas de dobles de pruebas se va a ejecutar. En este ejemplo nos estamos captura solo el número de veces que se llama a SaveChanges, pero puede incluir la lógica que se necesita para comprobar el escenario que se está probando.  

También hemos creado un TestDbSet que proporciona una implementación en memoria de DbSet. Hemos proporcionado una implementación completa para todos los métodos en DbSet (excepto para buscar), pero solo tiene que implementar a los miembros que se va a usar el escenario de prueba.  

TestDbSet hace uso de otras clases de infraestructura que hemos incluido para asegurarse de que se pueden procesar las consultas asincrónicas.  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

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

### <a name="implementing-find"></a>Implementación de búsqueda  

El método Find es difícil de implementar de forma genérica. Si necesita probar código que hace uso del método Find es más fácil crear una prueba de DbSet para cada uno de los tipos de entidad que necesitan compatibilidad con buscar. A continuación, puede escribir lógica para buscar ese tipo de entidad, como se muestra a continuación.  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a>Escritura de pruebas  

Eso es todo lo que necesitamos hacer para comenzar a probar. La siguiente prueba crea un TestContext y, a continuación, un servicio basado en este contexto. El servicio, a continuación, se usa para crear un nuevo blog: mediante el método AddBlog. Por último, la prueba comprueba que el servicio agrega un nuevo Blog a la propiedad de Blogs del contexto y llama a SaveChanges en el contexto.  

Esto es solo un ejemplo de los tipos de cosas que puede probar con un doble de pruebas en memoria y se puede ajustar la lógica de las dobles de pruebas y la comprobación para satisfacer sus necesidades.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

Este es otro ejemplo de una prueba - en este momento uno que realiza una consulta. La prueba se inicia mediante la creación de un contexto de prueba con algunos datos en su propiedad de Blog: tenga en cuenta que los datos no están en orden alfabético. A continuación, podemos crear un BlogService según el contexto de prueba y asegúrese de que los datos, obtenemos desde GetAllBlogs se ordenan por nombre.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

Por último, vamos a escribir una prueba más que usa el método asincrónico para asegurarse de que se incluyen en la infraestructura de async [TestDbSet](#creating-the-in-memory-test-doubles) está trabajando.  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
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
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
