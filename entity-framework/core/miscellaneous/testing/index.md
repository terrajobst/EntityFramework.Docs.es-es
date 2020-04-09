---
title: Prueba de componentes mediante EF Core - EF Core
description: Diferentes métodos para probar aplicaciones que usan EF Core
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634249"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="42770-103">Pruebas de código que usa EF Core</span><span class="sxs-lookup"><span data-stu-id="42770-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="42770-104">Para probar el código que accede a una base de datos, es necesario:</span><span class="sxs-lookup"><span data-stu-id="42770-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="42770-105">Ejecutar consultas y actualizaciones en el mismo sistema de base de datos que se usa en producción.</span><span class="sxs-lookup"><span data-stu-id="42770-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="42770-106">Ejecutar consultas y actualizaciones en algún otro sistema de base de datos más fácil de administrar.</span><span class="sxs-lookup"><span data-stu-id="42770-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="42770-107">Usar dobles de prueba o algún otro mecanismo para evitar por completo el uso de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="42770-108">En este documento se describen las ventajas e inconvenientes de cada una de estas opciones y se muestra cómo usar EF Core con cada método.</span><span class="sxs-lookup"><span data-stu-id="42770-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="42770-109">Todos los proveedores de bases de datos no son iguales</span><span class="sxs-lookup"><span data-stu-id="42770-109">All database providers are not equal</span></span>

<span data-ttu-id="42770-110">Es muy importante entender que EF Core no está diseñado para extraer cada aspecto del sistema de base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="42770-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="42770-111">EF Core es un conjunto común de patrones y conceptos que se pueden usar con cualquier sistema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="42770-112">Así, los proveedores de bases de datos de EF Core basan el comportamiento y la funcionalidad específicos de base de datos en este marco común.</span><span class="sxs-lookup"><span data-stu-id="42770-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="42770-113">Esto permite a cada sistema de base de datos hacer lo que mejor se le da, a la vez que mantiene la homogeneidad, si fuera necesario, con otros sistemas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="42770-114">Básicamente, esto significa que al cambiar de proveedor de base de datos, cambia el comportamiento de EF Core y no se puede esperar que la aplicación funcione correctamente a menos que tenga en cuenta de forma explícita todas las diferencias de comportamiento.</span><span class="sxs-lookup"><span data-stu-id="42770-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="42770-115">Dicho esto, en muchos casos esto funciona, ya que hay un alto grado de homogeneidad entre bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="42770-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="42770-116">Esto es bueno y malo.</span><span class="sxs-lookup"><span data-stu-id="42770-116">This is good and bad.</span></span>
<span data-ttu-id="42770-117">Es bueno porque el cambio entre bases de datos puede ser relativamente fácil.</span><span class="sxs-lookup"><span data-stu-id="42770-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="42770-118">Es malo porque puede dar una falsa sensación de seguridad si la aplicación no se prueba por completo en el nuevo sistema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="42770-119">Enfoque 1: Sistema de base de datos de producción</span><span class="sxs-lookup"><span data-stu-id="42770-119">Approach 1: Production database system</span></span>

<span data-ttu-id="42770-120">Como se ha explicado en la sección anterior, la única manera de asegurarse de que se está probando lo que se ejecuta en producción es usar el mismo sistema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="42770-121">Por ejemplo, si la aplicación implementada usa SQL Azure, las pruebas también deben realizarse en SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="42770-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="42770-122">Pero que cada desarrollador ejecute pruebas en SQL Azure mientras trabaja activamente en el código es lento y costoso.</span><span class="sxs-lookup"><span data-stu-id="42770-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="42770-123">Esto muestra la principal contrapartida de estos métodos: ¿cuándo resulta adecuado desviarse del sistema de base de datos de producción para mejorar la eficacia de las pruebas?</span><span class="sxs-lookup"><span data-stu-id="42770-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="42770-124">Afortunadamente, en este caso, la respuesta es bastante fácil: use la instancia local de SQL Server para las pruebas de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="42770-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="42770-125">SQL Azure y SQL Server son muy similares, por lo que realizar las pruebas en SQL Server suele ser una contrapartida razonable.</span><span class="sxs-lookup"><span data-stu-id="42770-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="42770-126">Dicho esto, sigue siendo aconsejable ejecutar las pruebas en SQL Azure antes de pasar a producción.</span><span class="sxs-lookup"><span data-stu-id="42770-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="42770-127">LocalDb</span><span class="sxs-lookup"><span data-stu-id="42770-127">LocalDb</span></span> 

<span data-ttu-id="42770-128">Todos los principales sistemas de base de datos tienen alguna forma de "edición para desarrolladores" para las pruebas locales.</span><span class="sxs-lookup"><span data-stu-id="42770-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="42770-129">SQL Server también tiene una característica denominada [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="42770-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="42770-130">La principal ventaja de LocalDb es que inicia la instancia de base de datos a petición.</span><span class="sxs-lookup"><span data-stu-id="42770-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="42770-131">Esto evita que haya un servicio de base de datos ejecutándose en el equipo aunque no se estén ejecutando pruebas.</span><span class="sxs-lookup"><span data-stu-id="42770-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="42770-132">Pero LocalDb también plantea problemas:</span><span class="sxs-lookup"><span data-stu-id="42770-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="42770-133">No admite todo lo que [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="42770-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="42770-134">No está disponible en Linux.</span><span class="sxs-lookup"><span data-stu-id="42770-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="42770-135">Puede producir un retraso en la primera serie de pruebas cuando se inicia el servicio.</span><span class="sxs-lookup"><span data-stu-id="42770-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="42770-136">Personalmente, nunca me ha parecido un problema que haya un servicio de base de datos ejecutándose en el equipo de desarrollo y, en general, recomendaría usar Developer Edition.</span><span class="sxs-lookup"><span data-stu-id="42770-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="42770-137">Pero puede ser adecuado para algunas personas, especialmente en equipos de desarrollo menos potentes.</span><span class="sxs-lookup"><span data-stu-id="42770-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="42770-138">Enfoque 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="42770-138">Approach 2: SQLite</span></span>

<span data-ttu-id="42770-139">EF Core prueba el proveedor de SQL Server principalmente mediante su ejecución en una instancia local de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42770-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="42770-140">Estas pruebas ejecutan decenas de miles de consultas en un par de minutos en un equipo rápido.</span><span class="sxs-lookup"><span data-stu-id="42770-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="42770-141">Esto muestra que el uso del sistema de base de datos real puede ser una solución eficiente.</span><span class="sxs-lookup"><span data-stu-id="42770-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="42770-142">Es un mito que el uso de una base de datos más ligera sea la única manera de ejecutar pruebas rápidamente.</span><span class="sxs-lookup"><span data-stu-id="42770-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="42770-143">Dicho esto, ¿qué ocurre si, por cualquier motivo, no se pueden ejecutar pruebas en algo cercano al sistema de base de datos de producción?</span><span class="sxs-lookup"><span data-stu-id="42770-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="42770-144">La siguiente mejor opción es usar algo con funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="42770-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="42770-145">Esto suele significar otra base de datos relacional, para lo que [SQLite](https://sqlite.org/index.html) es la opción obvia.</span><span class="sxs-lookup"><span data-stu-id="42770-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="42770-146">SQLite es una buena opción porque:</span><span class="sxs-lookup"><span data-stu-id="42770-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="42770-147">Se ejecuta en proceso con la aplicación y, por tanto, tiene poca sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="42770-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="42770-148">Usa archivos simples creados automáticamente para bases de datos, por lo que no requiere administración de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="42770-149">Tiene un modo en memoria que evita incluso la creación de archivos.</span><span class="sxs-lookup"><span data-stu-id="42770-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="42770-150">Pero recuerde que:</span><span class="sxs-lookup"><span data-stu-id="42770-150">However, remember that:</span></span>
* <span data-ttu-id="42770-151">SQLite inevitablemente no admite todo lo que el sistema de base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="42770-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="42770-152">SQLite se comporta de forma diferente al sistema de base de datos de producción para algunas consultas.</span><span class="sxs-lookup"><span data-stu-id="42770-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="42770-153">Por lo tanto, si usa SQLite para algunas pruebas, asegúrese de probar también en el sistema de base de datos real.</span><span class="sxs-lookup"><span data-stu-id="42770-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="42770-154">Vea [Pruebas con SQLite](xref:core/miscellaneous/testing/sqlite) para obtener instrucciones específicas de EF Core.</span><span class="sxs-lookup"><span data-stu-id="42770-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="42770-155">Método 3: Base de datos en memoria de EF Core</span><span class="sxs-lookup"><span data-stu-id="42770-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="42770-156">EF Core incluye una base de datos en memoria que se usa para las pruebas internas del propio EF Core.</span><span class="sxs-lookup"><span data-stu-id="42770-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="42770-157">Esta base de datos en general **no es adecuada como sustituto para probar las aplicaciones que usan EF Core**.</span><span class="sxs-lookup"><span data-stu-id="42770-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="42770-158">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="42770-158">Specifically:</span></span>
* <span data-ttu-id="42770-159">No es una base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="42770-159">It is not a relational database</span></span>
* <span data-ttu-id="42770-160">No admite transacciones</span><span class="sxs-lookup"><span data-stu-id="42770-160">It doesn't support transactions</span></span>
* <span data-ttu-id="42770-161">No está optimizada para el rendimiento</span><span class="sxs-lookup"><span data-stu-id="42770-161">It is not optimized for performance</span></span>

<span data-ttu-id="42770-162">Nada de esto es muy importante a la hora de probar elementos internos de EF Core, ya que se usa específicamente donde la base de datos es irrelevante para la prueba.</span><span class="sxs-lookup"><span data-stu-id="42770-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="42770-163">Por otro lado, estos aspectos tienden a ser muy importantes al probar una aplicación que usa EF Core.</span><span class="sxs-lookup"><span data-stu-id="42770-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="42770-164">Pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="42770-164">Unit testing</span></span>

<span data-ttu-id="42770-165">Imagine que va a probar una parte de la lógica de negocio que pueda necesitar usar algunos datos de una base de datos, pero que no supone probar propiamente las interacciones de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="42770-166">Una opción es usar un [doble de prueba](https://en.wikipedia.org/wiki/Test_double) como simulacro o imitación.</span><span class="sxs-lookup"><span data-stu-id="42770-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="42770-167">Los dobles de prueba se usan para las pruebas internas de EF Core.</span><span class="sxs-lookup"><span data-stu-id="42770-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="42770-168">Pero nunca se intentan simular DbContext o IQueryable.</span><span class="sxs-lookup"><span data-stu-id="42770-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="42770-169">Hacerlo es difícil, engorroso y delicado.</span><span class="sxs-lookup"><span data-stu-id="42770-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="42770-170">**No lo haga.**</span><span class="sxs-lookup"><span data-stu-id="42770-170">**Don't do it.**</span></span>

<span data-ttu-id="42770-171">En su lugar, se usa la base de datos en memoria siempre que se realizan pruebas unitarias de algo que usa DbContext.</span><span class="sxs-lookup"><span data-stu-id="42770-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="42770-172">En este caso, el uso de la base de datos en memoria es adecuado porque la prueba no depende del comportamiento de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="42770-173">Pero no lo haga para probar consultas o actualizaciones reales de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="42770-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="42770-174">Vea [Pruebas con InMemory](xref:core/miscellaneous/testing/in-memory) para obtener instrucciones específicas de EF Core sobre el uso de la base de datos en memoria para las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="42770-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
