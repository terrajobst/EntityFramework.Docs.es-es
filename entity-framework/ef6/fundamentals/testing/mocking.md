---
title: Las pruebas con un marco de simulación - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
caps.latest.revision: 3
ms.openlocfilehash: 7529929a3ed3906e1201c0899f2fb8b959ec9ed2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122576"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="3fd00-102">Las pruebas con un marco de simulación</span><span class="sxs-lookup"><span data-stu-id="3fd00-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="3fd00-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3fd00-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="3fd00-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="3fd00-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="3fd00-105">Al escribir pruebas para la aplicación suele ser deseable para evitar llegar a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fd00-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="3fd00-106">Entity Framework le permite conseguir esto mediante la creación de un contexto – con el comportamiento definido por las pruebas, que hace uso de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="3fd00-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="3fd00-107">Opciones para crear los dobles de pruebas</span><span class="sxs-lookup"><span data-stu-id="3fd00-107">Options for creating test doubles</span></span>  

<span data-ttu-id="3fd00-108">Existen dos enfoques diferentes que pueden usarse para crear una versión en memoria de su contexto.</span><span class="sxs-lookup"><span data-stu-id="3fd00-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="3fd00-109">**Crear sus propio dobles de pruebas** – este enfoque implica escribir su propia implementación en memoria de su contexto y DbSets.</span><span class="sxs-lookup"><span data-stu-id="3fd00-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="3fd00-110">Esto le ofrece un gran control sobre cómo se comportan las clases, pero pueden implicar escribir y poseer una cantidad razonable de código.</span><span class="sxs-lookup"><span data-stu-id="3fd00-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="3fd00-111">**Usar un marco de simulación para crear los dobles de pruebas** : mediante un marco de simulación (como Moq) puede tener las implementaciones en memoria que contexto y los conjuntos creados dinámicamente en tiempo de ejecución para usted.</span><span class="sxs-lookup"><span data-stu-id="3fd00-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="3fd00-112">En este artículo se tratará con el uso de un marco de simulación.</span><span class="sxs-lookup"><span data-stu-id="3fd00-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="3fd00-113">Para crear sus propio dobles de pruebas, vea [las pruebas con su dobles de pruebas propio](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="3fd00-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="3fd00-114">Para demostrar el uso de EF con un marco de simulación, vamos a usar Moq.</span><span class="sxs-lookup"><span data-stu-id="3fd00-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="3fd00-115">La manera más fácil de obtener Moq es instalar el [Moq paquete de NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="3fd00-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="3fd00-116">Las pruebas con las versiones anteriores a EF6</span><span class="sxs-lookup"><span data-stu-id="3fd00-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="3fd00-117">El escenario mostrado en este artículo depende de algunos cambios que realizamos en DbSet en EF6.</span><span class="sxs-lookup"><span data-stu-id="3fd00-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="3fd00-118">Para las pruebas con EF5 y una versión anterior, consulte [las pruebas con un contexto de imitar](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="3fd00-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="3fd00-119">Limitaciones de EF en memoria de dobles de pruebas</span><span class="sxs-lookup"><span data-stu-id="3fd00-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="3fd00-120">Dobles de pruebas en memoria pueden ser una buena forma de proporcionar cobertura de nivel de bits de la aplicación que usan EF de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="3fd00-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="3fd00-121">Sin embargo, al hacerlo se mediante LINQ to Objects para ejecutar consultas en datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="3fd00-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="3fd00-122">Esto puede provocar un comportamiento diferente que el proveedor LINQ de EF (LINQ to Entities) para traducir consultas en SQL que se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fd00-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="3fd00-123">Un ejemplo de esta diferencia está cargando datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="3fd00-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="3fd00-124">Si crea una serie de Blogs de publicaciones que cada relacionadas, a continuación, cuando se usan los datos en memoria siempre se cargarán esas entradas para cada Blog.</span><span class="sxs-lookup"><span data-stu-id="3fd00-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="3fd00-125">Sin embargo, cuando se ejecuta en una base de datos solo se cargará los datos si usa el método Include.</span><span class="sxs-lookup"><span data-stu-id="3fd00-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="3fd00-126">Por este motivo, se recomienda incluir siempre cierto nivel de pruebas to-end (además de las pruebas unitarias) para asegurarse de la aplicación funciona correctamente en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fd00-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="3fd00-127">Siguiendo con este artículo.</span><span class="sxs-lookup"><span data-stu-id="3fd00-127">Following along with this article</span></span>  

<span data-ttu-id="3fd00-128">Este artículo proporcionan listas de código completo que se pueden copiar en Visual Studio para seguir el tutorial si lo desea.</span><span class="sxs-lookup"><span data-stu-id="3fd00-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="3fd00-129">Es más fácil crear un **proyecto de prueba unitaria** y será necesario al destino **.NET Framework 4.5** para completar las secciones que se usa async.</span><span class="sxs-lookup"><span data-stu-id="3fd00-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="3fd00-130">El modelo de EF</span><span class="sxs-lookup"><span data-stu-id="3fd00-130">The EF model</span></span>  

<span data-ttu-id="3fd00-131">El servicio, vamos a probar hace uso de EF modelo formado por el BloggingContext y las clases de Blog y Post.</span><span class="sxs-lookup"><span data-stu-id="3fd00-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="3fd00-132">Este código puede haber sido generado por el Diseñador de EF o ser un modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="3fd00-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="3fd00-133">Propiedades DbSet virtual con EF Designer</span><span class="sxs-lookup"><span data-stu-id="3fd00-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="3fd00-134">Tenga en cuenta que las propiedades DbSet en el contexto se marcan como virtuales.</span><span class="sxs-lookup"><span data-stu-id="3fd00-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="3fd00-135">Esto permitirá que el marco de simulación derivar desde nuestro contexto e invalidar estas propiedades con una implementación simulada.</span><span class="sxs-lookup"><span data-stu-id="3fd00-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="3fd00-136">Si utiliza Code First puede editar las clases directamente.</span><span class="sxs-lookup"><span data-stu-id="3fd00-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="3fd00-137">Si está utilizando el Diseñador de EF, a continuación, deberá editar la plantilla T4 que genera el contexto.</span><span class="sxs-lookup"><span data-stu-id="3fd00-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="3fd00-138">Abra el \<model_name\>. Archivo Context.tt que está anidado bajo el archivo edmx, busque el siguiente fragmento de código y agregue la palabra clave virtual tal como se muestra.</span><span class="sxs-lookup"><span data-stu-id="3fd00-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="3fd00-139">Servicio va a probar</span><span class="sxs-lookup"><span data-stu-id="3fd00-139">Service to be tested</span></span>  

<span data-ttu-id="3fd00-140">Para mostrar la prueba de dobles de pruebas en memoria que vamos a escribir un par de pruebas para un BlogService.</span><span class="sxs-lookup"><span data-stu-id="3fd00-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="3fd00-141">El servicio es capaz de crear nuevos blogs (AddBlog) y devolver todos los Blogs ordenados por nombre (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="3fd00-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="3fd00-142">Además de GetAllBlogs, también hemos proporcionado un método que obtendrá de forma asincrónica todos los blogs, ordenados por nombre (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="3fd00-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="3fd00-143">Escenarios de prueba que no son de consulta</span><span class="sxs-lookup"><span data-stu-id="3fd00-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="3fd00-144">Eso es todo lo que necesitamos hacer para comenzar a probar los métodos que no son de consulta.</span><span class="sxs-lookup"><span data-stu-id="3fd00-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="3fd00-145">La siguiente prueba usa Moq para crear un contexto.</span><span class="sxs-lookup"><span data-stu-id="3fd00-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="3fd00-146">A continuación, crea una clase DbSet\<Blog\> y conecta lo va a devolver desde la propiedad de Blogs del contexto.</span><span class="sxs-lookup"><span data-stu-id="3fd00-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="3fd00-147">A continuación, el contexto se utiliza para crear un nuevo BlogService que, a continuación, se usa para crear un nuevo blog: mediante el método AddBlog.</span><span class="sxs-lookup"><span data-stu-id="3fd00-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="3fd00-148">Por último, la prueba comprueba que el servicio agrega un nuevo Blog y llama a SaveChanges en el contexto.</span><span class="sxs-lookup"><span data-stu-id="3fd00-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="3fd00-149">Escenarios de consulta de prueba</span><span class="sxs-lookup"><span data-stu-id="3fd00-149">Testing query scenarios</span></span>  

<span data-ttu-id="3fd00-150">Para poder ejecutar consultas en nuestra prueba DbSet doble es necesario configurar una implementación de IQueryable.</span><span class="sxs-lookup"><span data-stu-id="3fd00-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="3fd00-151">El primer paso es crear algunos datos en memoria: estamos usando una lista\<Blog\>.</span><span class="sxs-lookup"><span data-stu-id="3fd00-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="3fd00-152">A continuación, creamos un contexto y DBSet\<Blog\> , a continuación, conectar la implementación de IQueryable de la clase DbSet: solo va a delegar para el proveedor LINQ to Objects que funciona con la lista\<T\>.</span><span class="sxs-lookup"><span data-stu-id="3fd00-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="3fd00-153">A continuación, podemos crear un BlogService según nuestro dobles de pruebas y asegúrese de que los datos, obtenemos desde GetAllBlogs se ordenan por nombre.</span><span class="sxs-lookup"><span data-stu-id="3fd00-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="3fd00-154">Las pruebas con las consultas asincrónicas</span><span class="sxs-lookup"><span data-stu-id="3fd00-154">Testing with async queries</span></span>

<span data-ttu-id="3fd00-155">Entity Framework 6 introdujo un conjunto de métodos de extensión que puede usarse para ejecutar una consulta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3fd00-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="3fd00-156">Algunos ejemplos de estos métodos son ToListAsync, FirstAsync, ForEachAsync, etcetera.</span><span class="sxs-lookup"><span data-stu-id="3fd00-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="3fd00-157">Dado que las consultas de Entity Framework hacen uso de LINQ, los métodos de extensión se definen en IQueryable y IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="3fd00-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="3fd00-158">Sin embargo, ya que solo están diseñados para usarse con Entity Framework puede recibir el siguiente error si intenta usarlas en una consulta LINQ que no es una consulta de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="3fd00-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="3fd00-159">El origen de IQueryable no implementa IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="3fd00-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="3fd00-160">Solo los orígenes que implementan IDbAsyncEnumerable pueden usarse para las operaciones asincrónicas de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3fd00-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="3fd00-161">Para obtener más información, consulte [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="3fd00-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="3fd00-162">Mientras que los métodos asincrónicos se admiten solo cuando se ejecuta en una consulta EF, desea usarlos en la prueba unitaria al doble de un DbSet de prueba de ejecución frente a en memoria.</span><span class="sxs-lookup"><span data-stu-id="3fd00-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="3fd00-163">Para poder usar los métodos asincrónicos se deberá crear un DbAsyncQueryProvider en memoria para procesar la consulta asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3fd00-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="3fd00-164">Si bien sería posible configurar un proveedor de consultas con Moq, es mucho más fácil crear una implementación de dobles de prueba en el código.</span><span class="sxs-lookup"><span data-stu-id="3fd00-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="3fd00-165">El código para esta implementación es como sigue:</span><span class="sxs-lookup"><span data-stu-id="3fd00-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="3fd00-166">Ahora que tenemos un proveedor de consultas async podemos escribir una prueba unitaria para nuestro nuevo método GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="3fd00-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
