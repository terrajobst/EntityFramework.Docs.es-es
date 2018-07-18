---
title: Capacidad de prueba y Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
caps.latest.revision: 3
ms.openlocfilehash: 61773f8a23ff54ddb78aeeb5656c669b12f91478
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122368"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="24122-102">Capacidad de prueba y Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="24122-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="24122-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="24122-103">Scott Allen</span></span>

<span data-ttu-id="24122-104">Fecha de publicación: Mayo de 2010</span><span class="sxs-lookup"><span data-stu-id="24122-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="24122-105">Introducción</span><span class="sxs-lookup"><span data-stu-id="24122-105">Introduction</span></span>

<span data-ttu-id="24122-106">En este artículo se describe y se muestra cómo escribir código fácil de probar con el ADO.NET Entity Framework 4.0 y Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="24122-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="24122-107">Este artículo no trata de centrarse en una metodología de pruebas específica, como el diseño controlado por pruebas (TDD) o el diseño controlado por comportamiento (BDD).</span><span class="sxs-lookup"><span data-stu-id="24122-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="24122-108">En su lugar, este artículo se centrará en cómo escribir código que utiliza ADO.NET Entity Framework, pero que siga siendo fácil de aislar y probar de forma automática.</span><span class="sxs-lookup"><span data-stu-id="24122-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="24122-109">Examinaremos los patrones de diseño comunes que facilitan las pruebas en los datos de escenarios de acceso y ver cómo se aplican esos patrones cuando se usa el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="24122-110">También analizaremos las características específicas del marco para ver cómo estas características pueden trabajar en código comprobable.</span><span class="sxs-lookup"><span data-stu-id="24122-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="24122-111">¿Qué es código fácil de probar?</span><span class="sxs-lookup"><span data-stu-id="24122-111">What Is Testable Code?</span></span>

<span data-ttu-id="24122-112">La capacidad de comprobar una aplicación de software con pruebas unitarias automatizadas ofrece muchas ventajas deseables.</span><span class="sxs-lookup"><span data-stu-id="24122-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="24122-113">Todo el mundo sabe que pruebas correctas reducen el número de defectos de software en una aplicación y el aumento de la calidad de la aplicación - pero con las pruebas unitarias en su lugar va más allá de simplemente detecta errores.</span><span class="sxs-lookup"><span data-stu-id="24122-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="24122-114">Permite que un equipo de desarrollo ahorrar tiempo y permanecen en el control del software que crean un conjunto de pruebas unitarias correctas.</span><span class="sxs-lookup"><span data-stu-id="24122-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="24122-115">Un equipo puede realizar cambios en el código existente, refactorización, rediseño y reestructuración de software para cumplir los nuevos requisitos y agregar nuevos componentes en una aplicación todo sabiendo el conjunto de pruebas puede comprobar el comportamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24122-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="24122-116">Pruebas unitarias forman parte de un ciclo rápido de comentarios para facilitar el cambio y conservar el mantenimiento de software a medida que aumenta la complejidad.</span><span class="sxs-lookup"><span data-stu-id="24122-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="24122-117">Las pruebas unitarias viene con un precio, sin embargo.</span><span class="sxs-lookup"><span data-stu-id="24122-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="24122-118">Un equipo tiene que invertir tiempo para crear y mantener pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="24122-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="24122-119">La cantidad de esfuerzo necesario para crear estas pruebas se relaciona directamente con el **testability** del software subyacente.</span><span class="sxs-lookup"><span data-stu-id="24122-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="24122-120">¿Es fácil probar el software?</span><span class="sxs-lookup"><span data-stu-id="24122-120">How easy is the software to test?</span></span> <span data-ttu-id="24122-121">Un equipo de diseño de software con capacidad de prueba en mente creará pruebas eficaces con más rapidez que el equipo que trabaja con software un-testable.</span><span class="sxs-lookup"><span data-stu-id="24122-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="24122-122">Microsoft diseñó ADO.NET Entity Framework 4.0 (EF4) con capacidad de prueba en mente.</span><span class="sxs-lookup"><span data-stu-id="24122-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="24122-123">Esto no significa que los desarrolladores van a escribir pruebas unitarias para el propio código de framework.</span><span class="sxs-lookup"><span data-stu-id="24122-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="24122-124">En su lugar, los objetivos de capacidad de prueba de EF4 facilitan la creación fácil de probar código que se basa en el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="24122-125">Antes de adentrarnos en los ejemplos específicos, merece la pena entender las cualidades de código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="24122-126">Las cualidades de código comprobable</span><span class="sxs-lookup"><span data-stu-id="24122-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="24122-127">Código que es muy fácil probar siempre mostrará al menos dos características.</span><span class="sxs-lookup"><span data-stu-id="24122-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="24122-128">En primer lugar, puede probar código es fácil **observar**.</span><span class="sxs-lookup"><span data-stu-id="24122-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="24122-129">Dado un conjunto de entradas, debe ser fácil de observar el resultado del código.</span><span class="sxs-lookup"><span data-stu-id="24122-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="24122-130">Por ejemplo, el siguiente método de prueba es fácil porque el método devuelve directamente el resultado de un cálculo.</span><span class="sxs-lookup"><span data-stu-id="24122-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="24122-131">Un método de prueba es difícil, si el método escribe el valor calculado en un socket de red, una tabla de base de datos o un archivo similar al código siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="24122-132">La prueba tiene que realizar trabajo adicional para recuperar el valor.</span><span class="sxs-lookup"><span data-stu-id="24122-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="24122-133">En segundo lugar, es fácil de probar código **aislar**.</span><span class="sxs-lookup"><span data-stu-id="24122-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="24122-134">Vamos a usar el pseudocódigo siguiente como ejemplo de código comprobable incorrecto.</span><span class="sxs-lookup"><span data-stu-id="24122-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="24122-135">El método es fácil observar, podemos pasar una póliza de seguro y comprobar el valor devuelto coincide con un resultado esperado.</span><span class="sxs-lookup"><span data-stu-id="24122-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="24122-136">Sin embargo, para probar el método necesitaremos tener una base de datos que se instala con el esquema correcto, y configurar el servidor SMTP en caso de que el método intenta enviar un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="24122-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="24122-137">Solo desea la prueba unitaria comprobar la lógica de cálculo dentro del método, pero la prueba puede producir un error porque el servidor de correo electrónico está desconectado o porque mover el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="24122-138">Ambos de estos errores están relacionadas con el comportamiento de la prueba que desea comprobar.</span><span class="sxs-lookup"><span data-stu-id="24122-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="24122-139">El comportamiento es difícil aislar.</span><span class="sxs-lookup"><span data-stu-id="24122-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="24122-140">Los desarrolladores de software que esforzarse en escribir código fácil de probar con frecuencia se esfuerzan por mantener una separación de preocupaciones en el código que escriben.</span><span class="sxs-lookup"><span data-stu-id="24122-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="24122-141">El método anterior debe centrarse en los cálculos de negocio y delegar los detalles de implementación de base de datos y el correo electrónico a otros componentes.</span><span class="sxs-lookup"><span data-stu-id="24122-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="24122-142">Robert C. Martin llama a este principio de responsabilidad única.</span><span class="sxs-lookup"><span data-stu-id="24122-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="24122-143">Un objeto debe encapsular una responsabilidad única y estrecha, como calcular el valor de una directiva.</span><span class="sxs-lookup"><span data-stu-id="24122-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="24122-144">Todas las demás tareas de notificación y la base de datos deben ser la responsabilidad de algún otro objeto.</span><span class="sxs-lookup"><span data-stu-id="24122-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="24122-145">El código escrito de este modo es más fácil aislar porque se centra en una sola tarea.</span><span class="sxs-lookup"><span data-stu-id="24122-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="24122-146">En. NET, tenemos las abstracciones que necesitamos seguir el principio de responsabilidad única y así conseguir aislamiento.</span><span class="sxs-lookup"><span data-stu-id="24122-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="24122-147">Podemos usar definiciones de interfaz y forzar el código para usar la abstracción de interfaz en lugar de un tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="24122-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="24122-148">Más adelante en este artículo veremos cómo un método como el ejemplo erróneo presentado anteriormente puede trabajar con interfaces que *buscar* como se comunicará con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="24122-149">Durante la prueba, sin embargo, se puede sustituir una implementación ficticia que no comunicarse con la base de datos, sino que contiene datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="24122-150">Esta implementación ficticia aislará el código de problemas no relacionados en el código de acceso a datos o la configuración de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="24122-151">Hay otras ventajas derivadas del aislamiento.</span><span class="sxs-lookup"><span data-stu-id="24122-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="24122-152">El cálculo de negocios en el último método solo tardará unos milisegundos para ejecutar, pero la propia prueba podría ejecutarse durante varios segundos como los saltos de código en torno a la red y se comunica a distintos servidores.</span><span class="sxs-lookup"><span data-stu-id="24122-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="24122-153">Deben ejecutar las pruebas unitarias rápida para facilitar los cambios pequeños.</span><span class="sxs-lookup"><span data-stu-id="24122-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="24122-154">Pruebas unitarias deben también ser repetibles y no producirá un error porque un componente no está relacionado con la prueba tiene un problema.</span><span class="sxs-lookup"><span data-stu-id="24122-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="24122-155">Escribir código que sea fácil para observar y aislar significa que los desarrolladores tendrán mucho más fácil escribir pruebas para el código, dedicar menos tiempo esperando para que ejecutar las pruebas y más importante aún, dedican menos tiempo a la localización de errores que no existen.</span><span class="sxs-lookup"><span data-stu-id="24122-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="24122-156">Espero que puede apreciar las ventajas de las pruebas y comprender las calidades que exhibe el código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="24122-157">Que se van a resolver cómo escribir código que funciona con EF4 para guardar datos en una base de datos sin dejar de observable y fácil de aislar, pero primero se deberá limitar nuestro enfoque para tratar diseños apta para las pruebas de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="24122-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="24122-158">Patrones de diseño para la persistencia de datos</span><span class="sxs-lookup"><span data-stu-id="24122-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="24122-159">Ambos ejemplos incorrecta presentados anteriormente tenían demasiadas responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="24122-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="24122-160">El primer ejemplo incorrecto tenía que realizar un cálculo *y* escribir en un archivo.</span><span class="sxs-lookup"><span data-stu-id="24122-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="24122-161">El segundo ejemplo incorrecto tuvo que leer datos de una base de datos *y* realizan un cálculo business *y* enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="24122-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="24122-162">Mediante el diseño de los métodos más pequeños que separan las preocupaciones y delegación la responsabilidad a otros componentes hará grandes esfuerzos para escribir código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="24122-163">El objetivo es crear una funcionalidad creando las acciones de pequeñas escala y centradas de abstracciones.</span><span class="sxs-lookup"><span data-stu-id="24122-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="24122-164">Cuando se trata de persistencia de datos pequeño y centradas abstracciones que estamos buscando son tan comunes ha se han documentado como patrones de diseño.</span><span class="sxs-lookup"><span data-stu-id="24122-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="24122-165">Libro de Martin Patterns of Enterprise Application Architecture fue el primer trabajo para describir estos patrones en la impresión.</span><span class="sxs-lookup"><span data-stu-id="24122-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="24122-166">Antes le mostramos cómo estos ADO.NET Entity Framework implementa y funciona con estos patrones, le presentaremos una breve descripción de estos patrones en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="24122-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="24122-167">El modelo de repositorio</span><span class="sxs-lookup"><span data-stu-id="24122-167">The Repository Pattern</span></span>

<span data-ttu-id="24122-168">Fowler dice un repositorio de "Media entre el dominio y los datos de las capas de asignación mediante una interfaz similar a colección para tener acceso a objetos de dominio".</span><span class="sxs-lookup"><span data-stu-id="24122-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="24122-169">El objetivo del patrón de repositorio es aislar el código desde el comercializaba del acceso a datos y, como hemos visto anteriormente aislamiento es un rasgo necesario para la capacidad de prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="24122-170">La clave para el aislamiento es cómo el repositorio expone objetos mediante una interfaz similar a colección.</span><span class="sxs-lookup"><span data-stu-id="24122-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="24122-171">La lógica de que escritura para no usar el repositorio tiene idea de cómo el repositorio materializará los objetos que se solicite.</span><span class="sxs-lookup"><span data-stu-id="24122-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="24122-172">El repositorio puede comunicarse con una base de datos, o simplemente podría devolver objetos de una colección en memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="24122-173">Todo el código que necesita saber es que el repositorio aparece para mantener la colección, y puede recuperar, agregar y eliminar objetos de la colección.</span><span class="sxs-lookup"><span data-stu-id="24122-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="24122-174">En las aplicaciones .NET existentes un repositorio concreto a menudo se hereda de una interfaz genérica similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="24122-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="24122-175">Haremos algunos cambios a la definición de interfaz cuando se proporciona una implementación de EF4, pero el concepto básico sigue siendo el mismo.</span><span class="sxs-lookup"><span data-stu-id="24122-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="24122-176">Código puede usar un repositorio concreto que implementa esta interfaz para recuperar una entidad por su valor de clave principal para recuperar una colección de entidades basándose en la evaluación de un predicado, o simplemente recuperar todas las entidades disponibles.</span><span class="sxs-lookup"><span data-stu-id="24122-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="24122-177">El código también puede agregar y quitar las entidades a través de la interfaz del repositorio.</span><span class="sxs-lookup"><span data-stu-id="24122-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="24122-178">Dado un objetos IRepository de empleado, el código puede realizar las siguientes operaciones.</span><span class="sxs-lookup"><span data-stu-id="24122-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="24122-179">Dado que el código está usando una interfaz (IRepository de empleado), podemos proporcionar el código con diferentes implementaciones de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="24122-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="24122-180">Una implementación puede ser una implementación respaldada por EF4 y persistencia de objetos en una base de datos de Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="24122-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="24122-181">Una implementación diferente (una que se usa durante las pruebas) podría estar respaldada por los objetos de una lista de empleados en memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="24122-182">La interfaz le ayudará a lograr el aislamiento en el código.</span><span class="sxs-lookup"><span data-stu-id="24122-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="24122-183">Tenga en cuenta la IRepository&lt;T&gt; interfaz no expone una operación de guardar.</span><span class="sxs-lookup"><span data-stu-id="24122-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="24122-184">¿Cómo se actualiza objetos existentes?</span><span class="sxs-lookup"><span data-stu-id="24122-184">How do we update existing objects?</span></span> <span data-ttu-id="24122-185">Puede llegar a través de definiciones de IRepository que incluyen la operación de guardar, y las implementaciones de estos repositorios será preciso que conserve inmediatamente un objeto en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="24122-186">Sin embargo, en muchas aplicaciones no queremos conservar los objetos individualmente.</span><span class="sxs-lookup"><span data-stu-id="24122-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="24122-187">En su lugar, queremos cobren vida a objetos, quizás de diferentes repositorios, modificar estos objetos como parte de una actividad de negocio y, a continuación, se conservan todos los objetos como parte de una única operación atómica.</span><span class="sxs-lookup"><span data-stu-id="24122-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="24122-188">Afortunadamente, hay un patrón para permitir que a este tipo de comportamiento.</span><span class="sxs-lookup"><span data-stu-id="24122-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="24122-189">La unidad de patrón de trabajo</span><span class="sxs-lookup"><span data-stu-id="24122-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="24122-190">Fowler dice que una unidad de trabajo "mantendrá una lista de objetos afectados por una transacción empresarial y coordina la escritura fuera de los cambios y la resolución de problemas de simultaneidad".</span><span class="sxs-lookup"><span data-stu-id="24122-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="24122-191">Es responsabilidad de la unidad de trabajo para realizar el seguimiento de cambios a los objetos se cobren vida desde un repositorio y conservan los cambios que hemos realizado en los objetos cuando decimos que la unidad de trabajo para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="24122-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="24122-192">También es responsabilidad de la unidad de trabajo para aprovechar los nuevos objetos se ha agregado a todos los repositorios e inserción los objetos en una base de datos y también la eliminación de administrar.</span><span class="sxs-lookup"><span data-stu-id="24122-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="24122-193">Si alguna vez ha realizado cualquier trabajo con conjuntos de datos de ADO.NET, a continuación, ya estará familiarizado con la unidad de patrón de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="24122-194">Los conjuntos de datos de ADO.NET tenía la capacidad de realizar un seguimiento de nuestros actualizaciones, eliminaciones e inserción de objetos DataRow y podría (con la Ayuda de un TableAdapter) reconciliar todos los cambios a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="24122-195">Sin embargo, los objetos de conjunto de datos modelo un subconjunto desconectado de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="24122-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="24122-196">La unidad de patrón de trabajo muestra el mismo comportamiento, pero funciona con objetos de negocios y los objetos de dominio que son conscientes de la base de datos y aísla de código de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="24122-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="24122-197">Una abstracción para modelar la unidad de trabajo en el código .NET podría parecerse a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24122-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="24122-198">Mediante la exposición de las referencias del repositorio de la unidad de trabajo, que es posible garantizar que una sola unidad de trabajo objeto tiene la capacidad de realizar un seguimiento de todas las entidades materializadas durante una transacción empresarial.</span><span class="sxs-lookup"><span data-stu-id="24122-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="24122-199">La implementación del método de confirmación para una unidad de trabajo real es donde ocurre toda la magia conciliar los cambios en memoria con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="24122-200">Dada una referencia de IUnitOfWork, el código puede realizar cambios en objetos de negocios que se recuperan de uno o varios repositorios y guardar todos los cambios mediante la operación de confirmación atómica.</span><span class="sxs-lookup"><span data-stu-id="24122-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="24122-201">El patrón de carga diferida</span><span class="sxs-lookup"><span data-stu-id="24122-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="24122-202">Fowler usa el nombre de carga diferida para describir "un objeto que no contiene todos los datos necesita, pero sabe cómo obtenerlo".</span><span class="sxs-lookup"><span data-stu-id="24122-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="24122-203">Transparente carga diferida es una característica importante que al escribir código empresarial fácil de probar y trabajar con una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="24122-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="24122-204">Por ejemplo, considere el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="24122-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="24122-205">¿Cómo se rellena la colección de tarjetas de registro?</span><span class="sxs-lookup"><span data-stu-id="24122-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="24122-206">Hay dos respuestas posibles.</span><span class="sxs-lookup"><span data-stu-id="24122-206">There are two possible answers.</span></span> <span data-ttu-id="24122-207">Una respuesta es que el repositorio de empleados, cuando se le pida para capturar a un empleado, emite una consulta para recuperar a tanto el empleado junto con información de tarjeta de tiempo asociada del empleado.</span><span class="sxs-lookup"><span data-stu-id="24122-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="24122-208">En las bases de datos relacionales Esto normalmente requiere una consulta con una cláusula JOIN y puede dar lugar a recuperar más información que una aplicación necesita.</span><span class="sxs-lookup"><span data-stu-id="24122-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="24122-209">¿Qué ocurre si la aplicación nunca debe tener acceso a la propiedad de tarjetas de registro?</span><span class="sxs-lookup"><span data-stu-id="24122-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="24122-210">Una respuesta de la segunda consiste en cargar la propiedad de tarjetas de registro "a petición".</span><span class="sxs-lookup"><span data-stu-id="24122-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="24122-211">La carga diferida es transparente para la lógica de negocios e implícita porque el código no llamar a API especiales para recuperar información de tarjeta de tiempo.</span><span class="sxs-lookup"><span data-stu-id="24122-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="24122-212">El código se da por supuesto que la información de tarjeta de tiempo está presente cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="24122-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="24122-213">Hay algo de magia con la carga diferida que generalmente implica la intercepción en tiempo de ejecución de las invocaciones de método.</span><span class="sxs-lookup"><span data-stu-id="24122-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="24122-214">El código de intercepción es responsable de comunicarse con la base de datos y recuperar información de tarjeta de tiempo y dejar la lógica de negocios puede la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="24122-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="24122-215">Esta magia de la carga diferida permite que el código de negocio aislar propio de operaciones de recuperación de datos y los resultados más código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="24122-216">El inconveniente de una carga diferida es que cuando una aplicación *does* necesita la información de tarjeta de tiempo que el código ejecutará una consulta adicional.</span><span class="sxs-lookup"><span data-stu-id="24122-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="24122-217">Esto supone un problema para muchas aplicaciones, pero para aplicaciones sensibles al rendimiento o las aplicaciones de bucle a través de un número de objetos employee y ejecutar una consulta para recuperar las tarjetas de tiempo durante cada iteración del bucle (un problema a menudo se denomina N + 1 problema de consulta), la carga diferida es un arrastre.</span><span class="sxs-lookup"><span data-stu-id="24122-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="24122-218">En estos escenarios es posible que desee una aplicación cargar concienzudamente información de tarjeta de tiempo de la manera más eficaz posible.</span><span class="sxs-lookup"><span data-stu-id="24122-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="24122-219">Afortunadamente, veremos cómo EF4 admite ambos implícita carga diferida y diligente eficaz carga cuando se mueven a la siguiente sección e implementar estos patrones.</span><span class="sxs-lookup"><span data-stu-id="24122-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="24122-220">Implementación de patrones con Entity Framework</span><span class="sxs-lookup"><span data-stu-id="24122-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="24122-221">La buena noticia es que todos los patrones de diseño que se describe en la última sección son fáciles de implementar con EF4.</span><span class="sxs-lookup"><span data-stu-id="24122-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="24122-222">Para demostrar que vamos a usar una aplicación sencilla de ASP.NET MVC para editar y mostrar los empleados y su información de tarjeta de tiempo asociada.</span><span class="sxs-lookup"><span data-stu-id="24122-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="24122-223">Comenzaremos mediante el uso de la siguiente "objetos CLR" (poco).</span><span class="sxs-lookup"><span data-stu-id="24122-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="24122-224">Estas definiciones de clase cambiará ligeramente como exploramos diferentes enfoques y las características de EF4, pero la intención es conservar estas clases ignoran la persistencia (PI) como sea posible.</span><span class="sxs-lookup"><span data-stu-id="24122-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="24122-225">Un objeto PI desconoce *cómo*, o incluso *si*, el estado contiene se encuentra dentro de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="24122-226">PI y poco va mano a mano con software comprobable.</span><span class="sxs-lookup"><span data-stu-id="24122-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="24122-227">Los objetos con un enfoque POCO son menos restringida, más flexibles y fáciles de probar porque se pueden usar sin una base de datos está presente.</span><span class="sxs-lookup"><span data-stu-id="24122-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="24122-228">Con los objetos poco en su lugar que podemos crear un Entity Data Model (EDM) en Visual Studio (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="24122-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="24122-229">No se utilizará el EDM para generar código para nuestras entidades.</span><span class="sxs-lookup"><span data-stu-id="24122-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="24122-230">En su lugar, queremos usar las entidades que elaborar cariñosamente a mano.</span><span class="sxs-lookup"><span data-stu-id="24122-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="24122-231">Solo se utilizará el EDM para generar el esquema de base de datos y proporcionar los metadatos de que ef4 necesita para asignar objetos a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![eftest_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="24122-233">**Figura 1**</span><span class="sxs-lookup"><span data-stu-id="24122-233">**Figure 1**</span></span>

<span data-ttu-id="24122-234">Nota: si desea desarrollar el modelo EDM en primer lugar, es posible limpiar, generar código POCO del EDM.</span><span class="sxs-lookup"><span data-stu-id="24122-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="24122-235">Puede hacerlo con una extensión de Visual Studio 2010 ofrecida el equipo de programabilidad de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="24122-236">Para descargar la extensión, inicie el Administrador de extensiones en el menú Herramientas en Visual Studio y busque "POCO" (vea la ilustración 2) en la Galería de plantillas en línea.</span><span class="sxs-lookup"><span data-stu-id="24122-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="24122-237">Hay varias plantillas POCO para EF.</span><span class="sxs-lookup"><span data-stu-id="24122-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="24122-238">Para obtener más información sobre el uso de la plantilla, vea " [Tutorial: plantilla de POCO para Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="24122-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![eftest_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="24122-240">**Figura 2**</span><span class="sxs-lookup"><span data-stu-id="24122-240">**Figure 2**</span></span>

<span data-ttu-id="24122-241">Desde este punto de partida de POCO exploraremos dos enfoques diferentes para código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="24122-242">El primer enfoque llamo el enfoque EF ya que aprovecha las abstracciones de la API de Entity Framework para implementar unidades de trabajo y repositorios.</span><span class="sxs-lookup"><span data-stu-id="24122-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="24122-243">En el segundo enfoque crearemos nuestras propio abstracciones de repositorio personalizado y, a continuación, vea las ventajas y desventajas de cada enfoque.</span><span class="sxs-lookup"><span data-stu-id="24122-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="24122-244">Comenzaremos explorando el enfoque EF.</span><span class="sxs-lookup"><span data-stu-id="24122-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="24122-245">Una implementación centrados en EF</span><span class="sxs-lookup"><span data-stu-id="24122-245">An EF Centric Implementation</span></span>

<span data-ttu-id="24122-246">Tenga en cuenta la siguiente acción del controlador de un proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="24122-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="24122-247">Esta acción recupera un objeto Employee y devuelve un resultado para mostrar una vista detallada del empleado.</span><span class="sxs-lookup"><span data-stu-id="24122-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="24122-248">¿Es fácil de probar el código?</span><span class="sxs-lookup"><span data-stu-id="24122-248">Is the code testable?</span></span> <span data-ttu-id="24122-249">Hay al menos dos pruebas que necesitamos para comprobar el comportamiento de la acción.</span><span class="sxs-lookup"><span data-stu-id="24122-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="24122-250">En primer lugar, nos gustaría comprobar que la acción devuelve la vista correcta: una prueba sencilla.</span><span class="sxs-lookup"><span data-stu-id="24122-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="24122-251">¿También queremos escribir una prueba para comprobar la acción recupera al empleado correcto y nos gustaría hacerlo sin ejecutar código para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="24122-252">Recuerde que queremos aislar el código sometido a prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="24122-253">Aislamiento garantizará que la prueba no producirá un error debido a un error en la configuración de la base de datos o el código de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="24122-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="24122-254">Si se produce un error en la prueba, se sabrá que tenemos un error en la lógica del controlador y no en uno de los componentes del sistema de nivel inferior.</span><span class="sxs-lookup"><span data-stu-id="24122-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="24122-255">Para conseguir el aislamiento que necesitaremos algunas abstracciones como las interfaces que se presentan anteriormente para los repositorios y las unidades de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="24122-256">Recuerde que el modelo de repositorio está diseñado para mediar entre objetos de dominio y la capa de asignación de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="24122-257">En este escenario EF4 *es* la asignación de datos de capa y ya proporciona una abstracción de repositorio denominada IObjectSet&lt;T&gt; (desde el espacio de nombres System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="24122-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="24122-258">La definición de interfaz es similar a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="24122-259">IObjectSet&lt;T&gt; cumple los requisitos para un repositorio porque se parece a una colección de objetos (a través de IEnumerable&lt;T&gt;) y proporciona métodos para agregar y quitar objetos de la colección simulada.</span><span class="sxs-lookup"><span data-stu-id="24122-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="24122-260">Los métodos Attach y Detach exponen funcionalidades adicionales de la API de EF4.</span><span class="sxs-lookup"><span data-stu-id="24122-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="24122-261">Para usar IObjectSet&lt;T&gt; como interfaz para repositorios se necesita una unidad de trabajo abstracción para enlazar juntos los repositorios.</span><span class="sxs-lookup"><span data-stu-id="24122-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="24122-262">Una implementación concreta de esta interfaz se comunicará con SQL Server y es fácil crear mediante la clase ObjectContext de EF4.</span><span class="sxs-lookup"><span data-stu-id="24122-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="24122-263">La clase ObjectContext es la unidad real del trabajo en la API de EF4.</span><span class="sxs-lookup"><span data-stu-id="24122-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="24122-264">Poner un IObjectSet&lt;T&gt; cobren vida es tan sencillo como invocar el método CreateObjectSet del objeto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="24122-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="24122-265">En segundo plano, el marco de trabajo usará los metadatos se proporciona en el EDM para generar una clase ObjectSet concreta&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="24122-266">Nos centraremos con devolver el IObjectSet&lt;T&gt; porque le ayudará a conservar la capacidad de prueba en el código de cliente de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="24122-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="24122-267">Esta implementación concreta es útil en producción, pero es necesario centrarse en cómo vamos a usar nuestra abstracción IUnitOfWork para facilitar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="24122-268">Las dobles de pruebas</span><span class="sxs-lookup"><span data-stu-id="24122-268">The Test Doubles</span></span>

<span data-ttu-id="24122-269">Para aislar la acción de controlador necesitaremos la capacidad para cambiar entre la unidad real del trabajo (respaldado por un ObjectContext) y una unidad de doble o "falso" de prueba de trabajo (realizando las operaciones en memoria).</span><span class="sxs-lookup"><span data-stu-id="24122-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="24122-270">El enfoque común para realizar este tipo de cambio es para no permitir que el controlador de MVC a crear una instancia una unidad de trabajo, pero en su lugar, pase la unidad de trabajo en el controlador como un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="24122-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="24122-271">El código anterior es un ejemplo de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="24122-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="24122-272">No se permite el controlador crear su dependencia (la unidad de trabajo) pero insertar la dependencia en el controlador.</span><span class="sxs-lookup"><span data-stu-id="24122-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="24122-273">En un proyecto MVC es habitual usar una fábrica de controladores personalizada en combinación con un contenedor de inversión de control (IoC) para automatizar la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="24122-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="24122-274">Estos temas están fuera del ámbito de este artículo, pero puede leer más por las siguientes las referencias al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="24122-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="24122-275">Una unidad falsa de implementación de trabajo que podemos usar para realizar pruebas podría ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="24122-276">Tenga en cuenta que la unidad de trabajo falsa expone una propiedad de confirmado.</span><span class="sxs-lookup"><span data-stu-id="24122-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="24122-277">A veces resulta útil agregar características a una clase falsa que facilitar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="24122-278">En este caso es fácil observar si el código confirma una unidad de trabajo mediante la comprobación de la propiedad confirmado.</span><span class="sxs-lookup"><span data-stu-id="24122-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="24122-279">También necesitaremos una falsa IObjectSet&lt;T&gt; para contener objetos Employee y tarjetas de registro en la memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="24122-280">Podemos proporcionar una única implementación de uso de genéricos.</span><span class="sxs-lookup"><span data-stu-id="24122-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="24122-281">Este doble de pruebas delega la mayor parte de su trabajo a un objeto HashSet subyacente&lt;T&gt; objeto.</span><span class="sxs-lookup"><span data-stu-id="24122-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="24122-282">Tenga en cuenta que IObjectSet&lt;T&gt; requiere una restricción genérica aplicando T como una clase (un tipo de referencia) y también nos vemos obligados a implementar IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="24122-283">Es fácil crear una colección en memoria aparecen como un elemento IQueryable&lt;T&gt; mediante el operador LINQ estándar AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="24122-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="24122-284">Las pruebas</span><span class="sxs-lookup"><span data-stu-id="24122-284">The Tests</span></span>

<span data-ttu-id="24122-285">Pruebas unitarias tradicionales usará una única clase de prueba que contenga todas las pruebas para todas las acciones en un único controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="24122-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="24122-286">Podemos escribir estas pruebas, o cualquier tipo de prueba unitaria, fakes utilizando la memoria que creamos.</span><span class="sxs-lookup"><span data-stu-id="24122-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="24122-287">Sin embargo, en este artículo que se evitará la aplicación monolítica enfoque de la clase de prueba y nuestras pruebas para centrarse en una parte específica de la funcionalidad de grupo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="24122-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="24122-288">Por ejemplo, "crear a nuevo empleado" podría ser la funcionalidad que va a probar, por lo que se usará una única clase de prueba para comprobar la acción de controlador único responsable de crear a un nuevo empleado.</span><span class="sxs-lookup"><span data-stu-id="24122-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="24122-289">Hay algún código de instalación común que necesitamos para todas estas clases de prueba específica.</span><span class="sxs-lookup"><span data-stu-id="24122-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="24122-290">Por ejemplo, siempre se debe crear los repositorios de en memoria y falsa unidad de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="24122-291">También necesitamos una instancia del controlador de empleado con la unidad de trabajo insertado falsa.</span><span class="sxs-lookup"><span data-stu-id="24122-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="24122-292">Compartiremos este código de configuración comunes entre las clases de prueba mediante el uso de una clase base.</span><span class="sxs-lookup"><span data-stu-id="24122-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="24122-293">La "madre de objeto" se usa en la clase base es un patrón común para crear datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="24122-294">Una madre de objeto contiene los métodos de generador para crear instancias de entidades de prueba para su uso a través de varios accesorios de prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="24122-295">Podemos usar el EmployeeControllerTestBase como clase base para un número de accesorios de prueba (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="24122-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="24122-296">Cada accesorio de prueba a probar una acción de controlador específica.</span><span class="sxs-lookup"><span data-stu-id="24122-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="24122-297">Por ejemplo, un accesorio de prueba se centrará en las pruebas de la acción Create utilizada durante una solicitud HTTP GET (para mostrar la vista para la creación de un empleado), y un accesorio diferentes se centrará en la acción Create utilizada en una solicitud HTTP POST (para aprovechar la información presentada por el usuario para crear a un empleado).</span><span class="sxs-lookup"><span data-stu-id="24122-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="24122-298">Cada clase derivada solo es responsable de la configuración necesaria en su contexto específico y para proporcionar las aserciones necesarios para comprobar los resultados de su contexto de la prueba específica.</span><span class="sxs-lookup"><span data-stu-id="24122-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![eftest_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="24122-300">**Figura 3**</span><span class="sxs-lookup"><span data-stu-id="24122-300">**Figure 3**</span></span>

<span data-ttu-id="24122-301">El estilo de prueba y la convención de nomenclatura presentado aquí no es necesario para el código comprobable: es solo un enfoque.</span><span class="sxs-lookup"><span data-stu-id="24122-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="24122-302">Figura 4 muestra las pruebas que se ejecutan en el Resharper cerebro Jet probar el complemento de ejecutor para Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="24122-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![eftest_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="24122-304">**Figura 4**</span><span class="sxs-lookup"><span data-stu-id="24122-304">**Figure 4**</span></span>

<span data-ttu-id="24122-305">Con una clase base para controlar el código del programa de instalación compartido, las pruebas unitarias para cada acción de controlador son pequeños y fáciles de escribir.</span><span class="sxs-lookup"><span data-stu-id="24122-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="24122-306">Las pruebas se ejecutarán rápidamente (ya que se está realizando operaciones en memoria) y no deberían producir un error debido a la infraestructura no relacionada o preocupaciones medioambientales (ya que nos hemos aislado la unidad sometida a prueba).</span><span class="sxs-lookup"><span data-stu-id="24122-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="24122-307">En estas pruebas, la clase base realiza la mayor parte del trabajo del programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="24122-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="24122-308">Recuerde que el constructor de clase base crea una instancia de la clase EmployeeController, una unidad de trabajo falsa y el repositorio en memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="24122-309">La clase de prueba se deriva de esta clase base y se centra en los detalles de las pruebas al método Create.</span><span class="sxs-lookup"><span data-stu-id="24122-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="24122-310">En este caso los detalles se limitan a los pasos de "Organizar, actuar y afirmar" podrá ver en cualquier unidad de prueba de procedimiento:</span><span class="sxs-lookup"><span data-stu-id="24122-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="24122-311">Cree un objeto newEmployee para simular los datos entrantes.</span><span class="sxs-lookup"><span data-stu-id="24122-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="24122-312">Invocar la acción Create de la EmployeeController y pase el newEmployee.</span><span class="sxs-lookup"><span data-stu-id="24122-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="24122-313">Compruebe que la acción Create genera los resultados esperados (el empleado aparecerá en el repositorio).</span><span class="sxs-lookup"><span data-stu-id="24122-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="24122-314">Lo que hemos creado nos permite probar cualquiera de las acciones de EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="24122-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="24122-315">Por ejemplo, cuando escribimos las pruebas para la acción Index del controlador de empleado se pueden heredar de la clase base de prueba para establecer la misma configuración básica para nuestras pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="24122-316">Vuelva a la clase base creará una instancia de la EmployeeController, la unidad de trabajo falsa y el repositorio en memoria.</span><span class="sxs-lookup"><span data-stu-id="24122-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="24122-317">Las pruebas para la acción del índice solo tiene que centrarse en invocar la acción del índice y probar las cualidades del modelo de la acción devuelve.</span><span class="sxs-lookup"><span data-stu-id="24122-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="24122-318">Las pruebas que vamos a crear con fakes en memoria están orientadas a probar la *estado* del software.</span><span class="sxs-lookup"><span data-stu-id="24122-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="24122-319">¿Por ejemplo, al probar la acción de crear que desea inspeccionar el estado del repositorio cuando se ejecuta la acción create: el repositorio contiene al nuevo empleado?</span><span class="sxs-lookup"><span data-stu-id="24122-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="24122-320">Más adelante veremos las pruebas en función de interacción.</span><span class="sxs-lookup"><span data-stu-id="24122-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="24122-321">Las pruebas en función de interacción, se le preguntará si el código sometido a prueba invoca los métodos adecuados en nuestros objetos y pasa los parámetros correctos.</span><span class="sxs-lookup"><span data-stu-id="24122-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="24122-322">Por ahora, pasaremos la cubierta otro patrón de diseño de la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="24122-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="24122-323">La carga diligente y la carga diferida</span><span class="sxs-lookup"><span data-stu-id="24122-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="24122-324">En algún momento en el sitio web de ASP.NET MVC que queramos para mostrar información de un empleado e incluir al empleado de aplicación asociados a tarjetas de tiempo.</span><span class="sxs-lookup"><span data-stu-id="24122-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="24122-325">Por ejemplo, podríamos tenemos una pantalla de resumen de la tarjeta de tiempo que muestra el nombre del empleado y el número total de tarjetas de tiempo en el sistema.</span><span class="sxs-lookup"><span data-stu-id="24122-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="24122-326">Hay varios enfoques que podemos adoptar para implementar esta característica.</span><span class="sxs-lookup"><span data-stu-id="24122-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="24122-327">Proyección</span><span class="sxs-lookup"><span data-stu-id="24122-327">Projection</span></span>

<span data-ttu-id="24122-328">Es un enfoque sencillo para crear el resumen construir un modelo dedicado a la información que desea mostrar en la vista.</span><span class="sxs-lookup"><span data-stu-id="24122-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="24122-329">En este escenario, el modelo podría ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="24122-330">Tenga en cuenta que el EmployeeSummaryViewModel no es una entidad; en otras palabras no es algo que deseamos conservar en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="24122-331">Sólo vamos a usar esta clase para distribuye aleatoriamente los datos en la vista de una manera fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="24122-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="24122-332">El modelo de vista es como un datos transferir el objeto (DTO) porque no contiene ningún comportamiento (no hay métodos): solo las propiedades.</span><span class="sxs-lookup"><span data-stu-id="24122-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="24122-333">Las propiedades contendrá los datos que necesitamos pasar.</span><span class="sxs-lookup"><span data-stu-id="24122-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="24122-334">Es fácil crear una instancia de este modelo de vista mediante el operador de proyección estándar de LINQ: el operador Select.</span><span class="sxs-lookup"><span data-stu-id="24122-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="24122-335">Hay dos características importantes para el código anterior.</span><span class="sxs-lookup"><span data-stu-id="24122-335">There are two notable features to the above code.</span></span> <span data-ttu-id="24122-336">En primer lugar, el código es muy fácil probar porque está siendo fácil observar y aislar.</span><span class="sxs-lookup"><span data-stu-id="24122-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="24122-337">El operador Select funciona igual de bien con nuestros fakes en memoria como lo hace en la unidad de trabajo real.</span><span class="sxs-lookup"><span data-stu-id="24122-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="24122-338">La segunda característica importante es cómo el código permite EF4 generar una consulta única y eficiente para ensamblar la información del empleado y tarjeta de tiempo entre sí.</span><span class="sxs-lookup"><span data-stu-id="24122-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="24122-339">Información de los empleados y la información de tarjeta de tiempo se han cargado en el mismo objeto sin usar ninguna API especial.</span><span class="sxs-lookup"><span data-stu-id="24122-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="24122-340">El código simplemente expresa la información requiere el uso de operadores de LINQ estándar que funcionan con orígenes de datos en memoria, así como orígenes de datos remotos.</span><span class="sxs-lookup"><span data-stu-id="24122-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="24122-341">Fue capaz de traducir los árboles de expresión generados por la consulta LINQ y C EF4\# compilador en una consulta de Transact-SQL única y eficiente.</span><span class="sxs-lookup"><span data-stu-id="24122-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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

<span data-ttu-id="24122-342">Existen otras ocasiones cuando no queremos trabajar con un modelo de vista o un objeto DTO, pero con las entidades reales.</span><span class="sxs-lookup"><span data-stu-id="24122-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="24122-343">Cuando se sabe que necesitamos un empleado *y* tarjetas de tiempo del empleado, concienzudamente, se puede cargar los datos relacionados de forma discreta y eficaz.</span><span class="sxs-lookup"><span data-stu-id="24122-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="24122-344">Carga diligente explícita</span><span class="sxs-lookup"><span data-stu-id="24122-344">Explicit Eager Loading</span></span>

<span data-ttu-id="24122-345">Cuando desea cargar concienzudamente información de entidad relacionada que necesitamos algún mecanismo para la lógica de negocios (o en este escenario, la lógica de acciones de controlador) para expresar su deseo de que el repositorio.</span><span class="sxs-lookup"><span data-stu-id="24122-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="24122-346">La ObjectQuery EF4&lt;T&gt; clase define un método Include para especificar los objetos relacionados que se va a recuperar durante una consulta.</span><span class="sxs-lookup"><span data-stu-id="24122-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="24122-347">Recuerde que ObjectContext EF4 expone las entidades a través de ObjectSet concreta&lt;T&gt; clase que hereda de ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="24122-348">Si usamos ObjectSet&lt;T&gt; referencias en la acción de controlador podríamos escribir el código siguiente para especifican una carga diligente de información de tarjeta de tiempo para cada empleado.</span><span class="sxs-lookup"><span data-stu-id="24122-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="24122-349">Sin embargo, puesto que estamos intentando mantener nuestro código comprobable nos estamos exponer no ObjectSet&lt;T&gt; desde fuera de la unidad de la clase de trabajo real.</span><span class="sxs-lookup"><span data-stu-id="24122-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="24122-350">En su lugar, nos basamos en el IObjectSet&lt;T&gt; interfaz que es más fácil de imitar, pero IObjectSet&lt;T&gt; no define un método Include.</span><span class="sxs-lookup"><span data-stu-id="24122-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="24122-351">La belleza de LINQ es que podemos crear nuestro propio operador Include.</span><span class="sxs-lookup"><span data-stu-id="24122-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="24122-352">Tenga en cuenta este operador de inclusión se define como un método de extensión de IQueryable&lt;T&gt; en lugar de IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="24122-353">Esto nos da la capacidad de utilizar el método con una gama más amplia de tipos posibles, incluidos IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;y ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="24122-354">En caso de que la secuencia subyacente no es un auténtico ObjectQuery EF4&lt;T&gt;, a continuación, no hay ningún daño que realiza y el operador de inclusión es una operación inefectiva.</span><span class="sxs-lookup"><span data-stu-id="24122-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="24122-355">Si la secuencia subyacente *es* una ObjectQuery&lt;T&gt; (o derivado de ObjectQuery&lt;T&gt;), EF4 se vea nuestro requisito para los datos adicionales y formular SQL adecuado consulta.</span><span class="sxs-lookup"><span data-stu-id="24122-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="24122-356">Con este operador new en su lugar, podemos solicitar explícitamente una carga diligente de información de tarjeta de tiempo desde el repositorio.</span><span class="sxs-lookup"><span data-stu-id="24122-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="24122-357">Cuando se ejecuta en un ObjectContext con establecimiento real, el código produce la siguiente consulta única.</span><span class="sxs-lookup"><span data-stu-id="24122-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="24122-358">La consulta recopila información suficiente de la base de datos en un solo viaje para materializar los objetos de empleados y rellenar totalmente su propiedad de tarjetas de registro.</span><span class="sxs-lookup"><span data-stu-id="24122-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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

<span data-ttu-id="24122-359">La buena noticia es que el código dentro del método de acción siga siendo fácil de probar por completo.</span><span class="sxs-lookup"><span data-stu-id="24122-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="24122-360">No necesitamos proporcionar características adicionales para nuestros emulaciones admitir el operador de inclusión.</span><span class="sxs-lookup"><span data-stu-id="24122-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="24122-361">Las malas noticias son que tuvimos que utilizar el operador de inclusión dentro del código que deseáramos mantener que ignoran la persistencia.</span><span class="sxs-lookup"><span data-stu-id="24122-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="24122-362">Esto es un buen ejemplo del tipo de equilibrios, deberá evaluar al compilar código fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="24122-363">Hay veces cuando se necesitan para permitir que la pérdida de inquietudes de persistencia fuera de la abstracción de repositorio para satisfacer los objetivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="24122-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="24122-364">La alternativa a la carga diligente es la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="24122-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="24122-365">Medio de carga diferida hacemos *no* necesita nuestro código de negocio para anunciar explícitamente el requisito para los datos asociados.</span><span class="sxs-lookup"><span data-stu-id="24122-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="24122-366">En su lugar, usamos nuestras entidades en la aplicación y si están datos adicionales necesitan Entity Framework cargará los datos a petición.</span><span class="sxs-lookup"><span data-stu-id="24122-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="24122-367">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="24122-367">Lazy Loading</span></span>

<span data-ttu-id="24122-368">Es fácil imaginar un escenario donde no sabemos qué hay datos en una pieza de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="24122-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="24122-369">Podríamos sabemos que la lógica necesita un objeto employee, pero nos podemos crear bifurcaciones en diferentes rutas de ejecución donde algunas de esas rutas de acceso requieren información de tarjeta de tiempo del empleado y otras no.</span><span class="sxs-lookup"><span data-stu-id="24122-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="24122-370">Escenarios como éste son perfectos para la carga diferida implícita porque mágicamente aparecen datos según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="24122-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="24122-371">La carga diferida, también conocido como aplazada cargando, colocar algunos requisitos en nuestros objetos entidad.</span><span class="sxs-lookup"><span data-stu-id="24122-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="24122-372">Poco con omisión de persistencia verdadero no tendría que enfrentarse a los requisitos de la capa de persistencia, pero es prácticamente imposible lograr la omisión de persistencia es true.</span><span class="sxs-lookup"><span data-stu-id="24122-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="24122-373">En su lugar, medimos la omisión de persistencia en grados relativos.</span><span class="sxs-lookup"><span data-stu-id="24122-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="24122-374">Si se necesita heredar de una clase base orientada a servicios de persistencia o usar una colección especializada para lograr la carga diferida en poco sería lamentable.</span><span class="sxs-lookup"><span data-stu-id="24122-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="24122-375">Afortunadamente, EF4 tiene una solución menos intrusiva.</span><span class="sxs-lookup"><span data-stu-id="24122-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="24122-376">Prácticamente indetectables</span><span class="sxs-lookup"><span data-stu-id="24122-376">Virtually Undetectable</span></span>

<span data-ttu-id="24122-377">Al usar objetos POCO, EF4 puede generar dinámicamente en tiempo de ejecución los servidores proxy para las entidades.</span><span class="sxs-lookup"><span data-stu-id="24122-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="24122-378">Estos servidores proxy imperceptible encapsular los objetos materializados poco y proporcionan obtención servicios adicionales mediante la interceptación de cada propiedad y realizar trabajo adicional de la operación de establecimiento.</span><span class="sxs-lookup"><span data-stu-id="24122-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="24122-379">Uno de estos servicios es la característica de carga diferida que estamos buscando.</span><span class="sxs-lookup"><span data-stu-id="24122-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="24122-380">Otro servicio es un mecanismo que puede grabar cuando el programa cambia los valores de propiedad de una entidad de seguimiento de cambios eficaz.</span><span class="sxs-lookup"><span data-stu-id="24122-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="24122-381">La lista de cambios se usa ObjectContext durante el método SaveChanges para conservar las entidades modificadas mediante comandos de actualización.</span><span class="sxs-lookup"><span data-stu-id="24122-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="24122-382">Para estos servidores proxy para que funcione, sin embargo, que necesitan una manera de enlazar propiedad get y operaciones set en una entidad y los servidores proxy de lograr este objetivo mediante la invalidación de los miembros virtuales.</span><span class="sxs-lookup"><span data-stu-id="24122-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="24122-383">Por lo tanto, si queremos tener la carga diferida implícita y seguimiento de cambios eficaz se debe volver a nuestro definiciones de clase POCO y marcar las propiedades como virtual.</span><span class="sxs-lookup"><span data-stu-id="24122-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="24122-384">Aún podemos decir que la entidad de empleado es principalmente que ignoran la persistencia.</span><span class="sxs-lookup"><span data-stu-id="24122-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="24122-385">El único requisito consiste en usar a los miembros virtuales, y esto no afecta a la capacidad de prueba del código.</span><span class="sxs-lookup"><span data-stu-id="24122-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="24122-386">No tenemos que derivan de cualquier clase base especial, o incluso usar una colección especial dedicada a la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="24122-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="24122-387">Como se muestra el código, cualquier clase que implementa ICollection&lt;T&gt; está disponible para almacenar las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="24122-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="24122-388">También hay un cambio menor que necesitamos hacer dentro de nuestra unidad de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="24122-389">La carga diferida es *desactivar* de forma predeterminada cuando se trabaja directamente con un objeto ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="24122-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="24122-390">Hay una propiedad que podemos establecer en la propiedad ContextOptions para habilitar la carga diferida y podemos establecer esta propiedad dentro de nuestra unidad de trabajo real si desea habilitar la carga diferida en todas partes.</span><span class="sxs-lookup"><span data-stu-id="24122-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="24122-391">Implícita carga diferida habilitada, el código de aplicación puede usar a un empleado y el empleado asociadas tarjetas de tiempo sin dejar constancia del trabajo necesario para que EF que cargue los datos adicionales.</span><span class="sxs-lookup"><span data-stu-id="24122-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="24122-392">La carga diferida hace más fácil de escribir el código de aplicación y con la magia de proxy el código siga siendo fácil de probar completamente.</span><span class="sxs-lookup"><span data-stu-id="24122-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="24122-393">Fakes en memoria de la unidad de trabajo pueden precargar simplemente falsas entidades con asociados datos cuando sea necesario durante una prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="24122-394">En este momento centraremos nuestra atención desde la creación de repositorios con IObjectSet&lt;T&gt; y examine las abstracciones para ocultar todos los signos de que el marco de persistencia.</span><span class="sxs-lookup"><span data-stu-id="24122-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="24122-395">Repositorios personalizados</span><span class="sxs-lookup"><span data-stu-id="24122-395">Custom Repositories</span></span>

<span data-ttu-id="24122-396">Cuando se presenta primero la unidad de trabajo del patrón de diseño en este artículo se proporcionan algunos ejemplos de código para el aspecto que es posible que la unidad de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="24122-397">Vamos a volver a presentar esta idea original con el empleado y el escenario de las tarjetas de tiempo empleado con que eso hemos trabajado.</span><span class="sxs-lookup"><span data-stu-id="24122-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="24122-398">La principal diferencia entre esta unidad de trabajo y la unidad de trabajo que hemos creado en la última sección es cómo esta unidad de trabajo no utiliza las abstracciones de EF4 framework (no hay ningún IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="24122-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="24122-399">IObjectSet&lt;T&gt; funciona bien como una interfaz de repositorio, pero la API que expone no puede alinear perfectamente con las necesidades de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24122-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="24122-400">En este enfoque próximo se representará mediante un IRepository personalizado de repositorios&lt;T&gt; abstracción.</span><span class="sxs-lookup"><span data-stu-id="24122-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="24122-401">Muchos desarrolladores que siguen el diseño controlado por pruebas, Diseño controlado por comportamiento y el diseño de las metodologías de dominio controlado por prefieren el IRepository&lt;T&gt; enfoque por varias razones.</span><span class="sxs-lookup"><span data-stu-id="24122-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="24122-402">En primer lugar, el IRepository&lt;T&gt; interfaz representa una capa "para evitar daños".</span><span class="sxs-lookup"><span data-stu-id="24122-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="24122-403">Como se describe de Eric Evans en su libro Domain Driven Design una capa anticorrupción mantiene el código de dominio fuera de la infraestructura de las API, como una API de persistencia.</span><span class="sxs-lookup"><span data-stu-id="24122-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="24122-404">En segundo lugar, los desarrolladores pueden crear métodos en el repositorio que satisfacen las necesidades exactas de una aplicación (como detectado al escribir las pruebas).</span><span class="sxs-lookup"><span data-stu-id="24122-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="24122-405">Por ejemplo, puede que con frecuencia sea necesario buscar una sola entidad con un valor de identificador, por lo que podemos agregar un método FindById a la interfaz del repositorio.</span><span class="sxs-lookup"><span data-stu-id="24122-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="24122-406">Nuestro IRepository&lt;T&gt; definición tendrá un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="24122-407">Tenga en cuenta colocaremos volver a usar un tipo IQueryable&lt;T&gt; interfaz para exponer las colecciones de entidades.</span><span class="sxs-lookup"><span data-stu-id="24122-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="24122-408">IQueryable&lt;T&gt; permite que los árboles de expresión LINQ fluyen en el proveedor de EF4 y dar el proveedor de una vista holística de la consulta.</span><span class="sxs-lookup"><span data-stu-id="24122-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="24122-409">Otra opción sería devolver IEnumerable&lt;T&gt;, lo que significa que el proveedor LINQ de EF4 sólo verán las expresiones generadas dentro del repositorio.</span><span class="sxs-lookup"><span data-stu-id="24122-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="24122-410">Cualquier agrupación, ordenación y llevar a cabo fuera del repositorio de proyección no se crean en el comando SQL que se envían a la base de datos, que puede afectar negativamente al rendimiento.</span><span class="sxs-lookup"><span data-stu-id="24122-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="24122-411">Por otro lado, un repositorio de devolver IEnumerable solo&lt;T&gt; resultados nunca lo sorprenderá con un nuevo comando SQL.</span><span class="sxs-lookup"><span data-stu-id="24122-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="24122-412">Ambos enfoques funcionará, y ambos enfoques permanecen apta para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="24122-413">Resulta sencillo proporcionar una implementación única de la IRepository&lt;T&gt; interfaz con los genéricos y la API de ObjectContext de EF4.</span><span class="sxs-lookup"><span data-stu-id="24122-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="24122-414">El IRepository&lt;T&gt; enfoque nos da algún control adicional sobre las consultas porque tiene un cliente invocar un método para llegar a una entidad.</span><span class="sxs-lookup"><span data-stu-id="24122-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="24122-415">Dentro del método nos podríamos proporcionar comprobaciones adicionales y los operadores LINQ para exigir restricciones de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24122-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="24122-416">Tenga en cuenta que la interfaz tiene dos restricciones en el parámetro de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="24122-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="24122-417">La primera restricción es la su sabor desventajas de clase requerido por ObjectSet&lt;T&gt;, y la segunda restricción obliga a nuestras entidades para implementar IEntity: una abstracción que creó para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24122-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="24122-418">La interfaz IEntity obliga a las entidades tengan una propiedad de Id. de lectura y, a continuación, podemos usar esta propiedad en el método FindById.</span><span class="sxs-lookup"><span data-stu-id="24122-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="24122-419">IEntity se define con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="24122-420">IEntity podría considerarse una infracción de omisión de persistencia pequeña puesto que nuestras entidades son necesarios para implementar esta interfaz.</span><span class="sxs-lookup"><span data-stu-id="24122-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="24122-421">Recuerde la omisión de persistencia es sobre compromisos, y para muchos la funcionalidad FindById compensarán el restricciones impuestas por la interfaz.</span><span class="sxs-lookup"><span data-stu-id="24122-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="24122-422">La interfaz no influye en la capacidad de prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="24122-423">Crear una instancia de un IRepository live&lt;T&gt; requiere un ObjectContext EF4, por lo que una unidad de implementación de trabajo concreta debe administrar la creación de instancias.</span><span class="sxs-lookup"><span data-stu-id="24122-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="24122-424">Mediante el repositorio personalizado</span><span class="sxs-lookup"><span data-stu-id="24122-424">Using the Custom Repository</span></span>

<span data-ttu-id="24122-425">Utilizando nuestro repositorio personalizado no es significativamente diferente de mediante el repositorio según IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="24122-426">En lugar de aplicar los operadores de LINQ directamente a una propiedad, primero necesitaremos uno invocar métodos del repositorio para tomar un tipo IQueryable&lt;T&gt; referencia.</span><span class="sxs-lookup"><span data-stu-id="24122-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="24122-427">Tenga en cuenta el operador de inclusión personalizado, implementado previamente funcionará sin cambios.</span><span class="sxs-lookup"><span data-stu-id="24122-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="24122-428">Método FindById del repositorio quita duplicado lógica de acciones intenta recuperar una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="24122-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="24122-429">No hay ninguna diferencia significativa en las pruebas unitarias para los dos enfoques que hemos examinado.</span><span class="sxs-lookup"><span data-stu-id="24122-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="24122-430">Podríamos proporcionar implementaciones falsas de IRepository&lt;T&gt; mediante la creación de clases concretas respaldadas por HashSet&lt;empleado&gt; : al igual que lo que hicimos en la última sección.</span><span class="sxs-lookup"><span data-stu-id="24122-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="24122-431">Sin embargo, algunos desarrolladores prefieren usar los objetos ficticios y simular los marcos de objeto en lugar de generar emulaciones.</span><span class="sxs-lookup"><span data-stu-id="24122-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="24122-432">Examinaremos usar simulaciones en nuestra implementación de prueba y analizar las diferencias entre los objetos ficticios y emulaciones en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="24122-433">Las pruebas con objetos ficticios</span><span class="sxs-lookup"><span data-stu-id="24122-433">Testing with Mocks</span></span>

<span data-ttu-id="24122-434">Existen diferentes enfoques para crear una prueba de"double" de Martin Fowler lo que se llama.</span><span class="sxs-lookup"><span data-stu-id="24122-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="24122-435">Una prueba doble (por ejemplo, un entorno de cinematográfico película doble) es un objeto que se va a compilar "stand" para el real, los objetos de producción durante las pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="24122-436">Los repositorios en memoria, que hemos creado son dobles de pruebas para los repositorios que se comunican con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="24122-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="24122-437">Hemos visto cómo usar estos dobles de pruebas durante las pruebas unitarias para aislar el código y mantener las pruebas que se ejecutan rápidamente.</span><span class="sxs-lookup"><span data-stu-id="24122-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="24122-438">Las dobles de pruebas, que hemos creado tienen implementaciones reales, el trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="24122-439">En segundo plano cada uno de ellos almacena una colección de objetos concretos, y agregará y quitar objetos de esta colección mientras se manipulan el repositorio durante una prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="24122-440">Algunos desarrolladores como compilar los dobles de pruebas de este modo: con el código real y las implementaciones de trabajo.</span><span class="sxs-lookup"><span data-stu-id="24122-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="24122-441">Estos valores Double de prueba son las que denominamos *fakes*.</span><span class="sxs-lookup"><span data-stu-id="24122-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="24122-442">Tienen implementaciones de trabajo, pero no son lo suficientemente reales para su uso en producción.</span><span class="sxs-lookup"><span data-stu-id="24122-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="24122-443">El repositorio falso en realidad no escribe en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="24122-444">El servidor SMTP falso realmente no envía un mensaje de correo electrónico a través de la red.</span><span class="sxs-lookup"><span data-stu-id="24122-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="24122-445">Simulacros frente a Fakes</span><span class="sxs-lookup"><span data-stu-id="24122-445">Mocks versus Fakes</span></span>

<span data-ttu-id="24122-446">Hay otro tipo de prueba de doble se conoce como un *simular*.</span><span class="sxs-lookup"><span data-stu-id="24122-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="24122-447">Aunque fakes tienen implementaciones de trabajo, objetos ficticios vienen con ninguna implementación.</span><span class="sxs-lookup"><span data-stu-id="24122-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="24122-448">Con la Ayuda de un marco de objeto ficticio construimos estos objetos ficticios en tiempo de ejecución y usarlas como dobles de pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="24122-449">En esta sección vamos a usar el marco Moq de simulación de código abierto.</span><span class="sxs-lookup"><span data-stu-id="24122-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="24122-450">Este es un ejemplo sencillo de usar Moq para crear dinámicamente una prueba doble para un repositorio de empleado.</span><span class="sxs-lookup"><span data-stu-id="24122-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="24122-451">Le pedimos Moq IRepository&lt;empleado&gt; implementación y se crea uno dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="24122-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="24122-452">Podemos obtener para el objeto que implementa IRepository&lt;empleado&gt; mediante el acceso a la propiedad del objeto de la simulación&lt;T&gt; objeto.</span><span class="sxs-lookup"><span data-stu-id="24122-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="24122-453">Es este objeto interno para pasar en nuestros controladores, y no sabrán si se trata de un doble de pruebas o en el repositorio real.</span><span class="sxs-lookup"><span data-stu-id="24122-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="24122-454">Tal como se podría invocar métodos en un objeto con una implementación real que podemos llamar a métodos en el objeto.</span><span class="sxs-lookup"><span data-stu-id="24122-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="24122-455">Debe se esté preguntando lo hará el repositorio ficticio cuando se invoca el método Add.</span><span class="sxs-lookup"><span data-stu-id="24122-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="24122-456">Dado que no hay ninguna implementación detrás del objeto ficticio, agregar no hace nada.</span><span class="sxs-lookup"><span data-stu-id="24122-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="24122-457">No hay ninguna colección concreta en segundo plano como la que teníamos con las emulaciones que escribimos, por lo que el empleado se descarta.</span><span class="sxs-lookup"><span data-stu-id="24122-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="24122-458">¿Qué sucede con el valor devuelto de FindById?</span><span class="sxs-lookup"><span data-stu-id="24122-458">What about the return value of FindById?</span></span> <span data-ttu-id="24122-459">En este caso el objeto ficticio hace lo único que puede hacer, que devuelve un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="24122-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="24122-460">Puesto que nos estamos devolviendo un tipo de referencia (un empleado), el valor devuelto es un valor null.</span><span class="sxs-lookup"><span data-stu-id="24122-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="24122-461">Simulacros podrían parecer inútil; Sin embargo, hay otras dos características de los simulacros que no hemos hablado.</span><span class="sxs-lookup"><span data-stu-id="24122-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="24122-462">En primer lugar, el marco de trabajo Moq registra todas las llamadas realizadas en el objeto ficticio.</span><span class="sxs-lookup"><span data-stu-id="24122-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="24122-463">Más adelante en el código podemos hacer Moq si nadie invoca el método Add, o si alguien ha llamado al método FindById.</span><span class="sxs-lookup"><span data-stu-id="24122-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="24122-464">Veremos más adelante cómo podemos usar esta característica de grabación de "caja negra" en las pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="24122-465">La segunda característica excelente es cómo podemos usar Moq para programar un objeto ficticio con *expectativas*.</span><span class="sxs-lookup"><span data-stu-id="24122-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="24122-466">Una expectativa indica el objeto ficticio cómo responder a ninguna interacción dada.</span><span class="sxs-lookup"><span data-stu-id="24122-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="24122-467">Por ejemplo, podemos programar una expectativa en nuestra simulacro y decirle que devuelven un objeto employee cuando alguien llama a FindById.</span><span class="sxs-lookup"><span data-stu-id="24122-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="24122-468">El marco de Moq usa una API de instalación y las expresiones lambda para programar estas expectativas.</span><span class="sxs-lookup"><span data-stu-id="24122-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="24122-469">En este ejemplo, le pedimos Moq para generar dinámicamente un repositorio y, a continuación, se programa el repositorio con una expectativa.</span><span class="sxs-lookup"><span data-stu-id="24122-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="24122-470">La expectativa indica el objeto ficticio para devolver un nuevo objeto de empleado con un valor de identificador de 5 cuando un usuario invoca el método FindById pasando un valor de 5.</span><span class="sxs-lookup"><span data-stu-id="24122-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="24122-471">Esta prueba se supera, y no necesitamos crear una implementación completa a IRepository falsa&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="24122-472">Vamos a volver a visitar las pruebas que hemos escrito anteriormente y rehacer para que usen los simulacros en lugar de fakes.</span><span class="sxs-lookup"><span data-stu-id="24122-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="24122-473">Al igual que antes, vamos a usar una clase base para configurar las partes comunes de infraestructura que necesitamos para todas las pruebas del controlador.</span><span class="sxs-lookup"><span data-stu-id="24122-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="24122-474">El código del programa de instalación principalmente permanece igual.</span><span class="sxs-lookup"><span data-stu-id="24122-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="24122-475">En lugar de usar fakes, vamos a usar Moq para construir objetos ficticios.</span><span class="sxs-lookup"><span data-stu-id="24122-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="24122-476">Organiza la clase base para que el simulacro unidad de trabajo devolver un repositorio ficticio cuando el código invoca la propiedad de los empleados.</span><span class="sxs-lookup"><span data-stu-id="24122-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="24122-477">El resto de la configuración de simulacro llevará a cabo dentro de los accesorios de prueba dedicado a cada situación específica.</span><span class="sxs-lookup"><span data-stu-id="24122-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="24122-478">Por ejemplo, el accesorio de prueba para la acción del índice configurará el repositorio ficticio para devolver una lista de empleados, cuando la acción invoca el método FindAll del repositorio ficticio.</span><span class="sxs-lookup"><span data-stu-id="24122-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="24122-479">Excepto las expectativas, un aspecto similares a las pruebas que teníamos antes de nuestras pruebas.</span><span class="sxs-lookup"><span data-stu-id="24122-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="24122-480">Sin embargo, con la capacidad de grabación de un marco ficticio podemos enfocamos pruebas desde un ángulo diferente.</span><span class="sxs-lookup"><span data-stu-id="24122-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="24122-481">Examinaremos esta nueva perspectiva en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="24122-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="24122-482">Estado frente a las pruebas de interacción</span><span class="sxs-lookup"><span data-stu-id="24122-482">State versus Interaction Testing</span></span>

<span data-ttu-id="24122-483">Existen diferentes técnicas que puede usar para probar el software con objetos ficticios.</span><span class="sxs-lookup"><span data-stu-id="24122-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="24122-484">Un enfoque es utilizar el estado de las pruebas, que es lo que hemos hecho en este artículo hasta el momento.</span><span class="sxs-lookup"><span data-stu-id="24122-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="24122-485">Probar aserciones hace sobre el estado del software basado en Estados.</span><span class="sxs-lookup"><span data-stu-id="24122-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="24122-486">En la última prueba se invoca un método de acción en el controlador y se realiza una aserción sobre el modelo que se debe compilar.</span><span class="sxs-lookup"><span data-stu-id="24122-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="24122-487">Estos son algunos otros ejemplos de estado de prueba:</span><span class="sxs-lookup"><span data-stu-id="24122-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="24122-488">Compruebe que el repositorio contiene el nuevo objeto employee después de crear se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="24122-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="24122-489">Compruebe que el modelo contiene una lista de todos los empleados, cuando se ejecuta el índice.</span><span class="sxs-lookup"><span data-stu-id="24122-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="24122-490">Compruebe que el repositorio no tiene a un empleado determinado cuando se ejecuta Delete.</span><span class="sxs-lookup"><span data-stu-id="24122-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="24122-491">Otro enfoque verá con objetos ficticios que consiste en comprobar *interacciones*.</span><span class="sxs-lookup"><span data-stu-id="24122-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="24122-492">Mientras prueba aserciones hace sobre el estado de objetos basado en Estados, interacción en función probar aserciones hace acerca de cómo interactúan los objetos.</span><span class="sxs-lookup"><span data-stu-id="24122-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="24122-493">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="24122-493">For example:</span></span>

-   <span data-ttu-id="24122-494">Compruebe que el controlador invoca el método Add del repositorio cuando se ejecuta Create.</span><span class="sxs-lookup"><span data-stu-id="24122-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="24122-495">Compruebe que el controlador invoca el método FindAll del repositorio cuando se ejecuta el índice.</span><span class="sxs-lookup"><span data-stu-id="24122-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="24122-496">Compruebe que el controlador invoca la unidad del método de confirmación del trabajo para guardar los cambios cuando se ejecuta la edición.</span><span class="sxs-lookup"><span data-stu-id="24122-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="24122-497">A menudo interacción pruebas requiere menos datos de prueba, porque no estamos buscando reiteradamente dentro de las colecciones y comprobar los recuentos.</span><span class="sxs-lookup"><span data-stu-id="24122-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="24122-498">Por ejemplo, si conocemos el método FindById de un repositorio con el valor correcto - que invoca la acción de detalles, a continuación, la acción es probablemente comportan correctamente.</span><span class="sxs-lookup"><span data-stu-id="24122-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="24122-499">Podemos comprobar este comportamiento sin tener que configurar los datos de prueba para devolver de FindById.</span><span class="sxs-lookup"><span data-stu-id="24122-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="24122-500">El programa de instalación solo requerido en el accesorio de prueba anterior es la configuración proporcionada por la clase base.</span><span class="sxs-lookup"><span data-stu-id="24122-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="24122-501">Cuando se invoca la acción de controlador, Moq registrará las interacciones con el repositorio ficticio.</span><span class="sxs-lookup"><span data-stu-id="24122-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="24122-502">Mediante la API de comprobación de Moq, podemos hacer Moq si el controlador invocado FindById con el valor de identificador adecuado.</span><span class="sxs-lookup"><span data-stu-id="24122-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="24122-503">Si el controlador no invocó el método, o invoca el método con un valor de parámetro inesperado, el método de comprobación generará una excepción y se producirá un error en la prueba.</span><span class="sxs-lookup"><span data-stu-id="24122-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="24122-504">Este es otro ejemplo para comprobar que la acción Create invoca confirmación en la unidad de trabajo actual.</span><span class="sxs-lookup"><span data-stu-id="24122-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="24122-505">Un peligro con las pruebas de interacción es la tendencia a través de especificar las interacciones.</span><span class="sxs-lookup"><span data-stu-id="24122-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="24122-506">La capacidad del objeto ficticio para grabar y comprobar que cada interacción con el objeto ficticio no significa que la prueba debe intentar comprobar cada interacción.</span><span class="sxs-lookup"><span data-stu-id="24122-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="24122-507">Algunos tipos de interacción son detalles de implementación y sólo debe comprobar las interacciones *requiere* para satisfacer la prueba actual.</span><span class="sxs-lookup"><span data-stu-id="24122-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="24122-508">La elección entre los objetos ficticios o fakes depende en gran medida el sistema que se está probando y su personal (o equipo) Preferencias.</span><span class="sxs-lookup"><span data-stu-id="24122-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="24122-509">Los objetos ficticios pueden reducir drásticamente la cantidad de código que necesita para implementar los dobles de pruebas, pero no todo el mundo es cómodo las expectativas de programación y comprobando las interacciones.</span><span class="sxs-lookup"><span data-stu-id="24122-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="24122-510">Conclusiones</span><span class="sxs-lookup"><span data-stu-id="24122-510">Conclusions</span></span>

<span data-ttu-id="24122-511">En este artículo hemos mostrado varios enfoques para crear código fácil de probar al usar ADO.NET Entity Framework para la persistencia de datos.</span><span class="sxs-lookup"><span data-stu-id="24122-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="24122-512">Podemos aprovechar abstracciones integradas como IObjectSet&lt;T&gt;, o crear nuestras propio abstracciones como IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="24122-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="24122-513">En ambos casos, la compatibilidad POCO en ADO.NET Entity Framework 4.0 permite que los consumidores de estas abstracciones permanezcan persistentes que ignoran la persistencia y muy fácil de probar.</span><span class="sxs-lookup"><span data-stu-id="24122-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="24122-514">Características adicionales de EF4, como la carga diferida implícita permite el servicio de aplicación y empresarial de código para que funcione sin preocuparse por los detalles de un almacén de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="24122-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="24122-515">Por último, las abstracciones que creamos son fáciles de simulacro o falsa dentro de las pruebas unitarias, y podemos usar estos dobles de pruebas para lograr la ejecución rápida, altamente aislada y las pruebas fiables.</span><span class="sxs-lookup"><span data-stu-id="24122-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="24122-516">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="24122-516">Additional Resources</span></span>

-   <span data-ttu-id="24122-517">Robert C. Martin, " [el principio de responsabilidad única](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="24122-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="24122-518">Martin Fowler, [catálogo de patrones de](http://www.martinfowler.com/eaaCatalog/index.html) desde *patrones de arquitectura de aplicaciones de empresa*</span><span class="sxs-lookup"><span data-stu-id="24122-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="24122-519">Griffin Caprio, " [inserción de dependencias](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="24122-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="24122-520">Blog de programación de datos, " [Tutorial: desarrollo con Entity Framework 4.0 controlado por pruebas](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="24122-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="24122-521">Blog de programación de datos, " [patrones de uso de repositorio y unidad de trabajo con Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="24122-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="24122-522">Dave Astels, " [Introducción BDD](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="24122-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="24122-523">Aaron Jensen, " [Introducción a las especificaciones de máquina](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="24122-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="24122-524">Eric Lee, " [BDD con MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="24122-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="24122-525">Eric Evans, " [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="24122-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="24122-526">Martin Fowler, " [las simulaciones no son códigos auxiliares](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="24122-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="24122-527">Martin Fowler, " [doble de pruebas](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="24122-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="24122-528">Jeremy Miller, " [estado frente a las pruebas de interacción](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="24122-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   <span data-ttu-id="24122-529">Moq, \<http://code.google.com/p/moq/></span><span class="sxs-lookup"><span data-stu-id="24122-529">Moq, \<http://code.google.com/p/moq/></span></span>

### <a name="biography"></a><span data-ttu-id="24122-530">Biografía</span><span class="sxs-lookup"><span data-stu-id="24122-530">Biography</span></span>

<span data-ttu-id="24122-531">Scott Allen es un miembro del personal técnico en Pluralsight y fundador de OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="24122-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="24122-532">De 15 años de desarrollo de software comercial, Scott ha trabajado en soluciones para todo, desde dispositivos incrustados de 8 bits a las aplicaciones web ASP.NET altamente escalables.</span><span class="sxs-lookup"><span data-stu-id="24122-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="24122-533">Puede llegar a Scott en su blog en OdeToCode o en Twitter en \< http://twitter.com/OdeToCode>.</span><span class="sxs-lookup"><span data-stu-id="24122-533">You can reach Scott on his blog at OdeToCode, or on Twitter at \<http://twitter.com/OdeToCode>.</span></span>
