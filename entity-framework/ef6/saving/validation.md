---
title: Validación - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 3aeb33763819544618c4a3068bb278c9b23409b6
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490653"
---
# <a name="data-validation"></a><span data-ttu-id="1988a-102">Validación de datos</span><span class="sxs-lookup"><span data-stu-id="1988a-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="1988a-103">**EF4.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="1988a-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="1988a-104">Si utiliza una versión anterior, algunos o todos de la información no se aplica</span><span class="sxs-lookup"><span data-stu-id="1988a-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="1988a-105">El contenido de esta página se ha adaptado del y el artículo originalmente escrito por Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="1988a-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="1988a-106">Entity Framework proporciona una gran variedad de características de validación que pueda proporcionar a través de una interfaz de usuario para la validación del lado cliente o usarse para la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="1988a-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="1988a-107">Cuando se usa el código en primer lugar, puede especificar las validaciones con anotación o configuraciones de la API fluidas.</span><span class="sxs-lookup"><span data-stu-id="1988a-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="1988a-108">Validaciones adicionales y más compleja, pueden especificarse en el código y funcionarán si el modelo de estado desde el código en primer lugar, en primer lugar el modelo o base de datos en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="1988a-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="1988a-109">El modelo</span><span class="sxs-lookup"><span data-stu-id="1988a-109">The model</span></span>

<span data-ttu-id="1988a-110">Voy a demostrar las validaciones con un simple par de clases: Blog y Post.</span><span class="sxs-lookup"><span data-stu-id="1988a-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a><span data-ttu-id="1988a-111">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="1988a-111">Data Annotations</span></span>

<span data-ttu-id="1988a-112">Código primero usa anotaciones desde el ensamblado System.ComponentModel.DataAnnotations como una manera de configurar primero las clases de código.</span><span class="sxs-lookup"><span data-stu-id="1988a-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="1988a-113">Entre estas anotaciones son aquellos que proporcionan reglas como necesario, MinLength y MaxLength.</span><span class="sxs-lookup"><span data-stu-id="1988a-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="1988a-114">Un número de aplicaciones de cliente de .NET también reconoce estas anotaciones, por ejemplo, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1988a-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="1988a-115">Puede lograr ambos lado y servidor de validación en el cliente con estas anotaciones.</span><span class="sxs-lookup"><span data-stu-id="1988a-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="1988a-116">Por ejemplo, puede forzar la propiedad Blog Title que es una propiedad necesaria.</span><span class="sxs-lookup"><span data-stu-id="1988a-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="1988a-117">Sin código adicional ni cambios de marcado en la aplicación, una aplicación existente de MVC realizará la validación del lado cliente, crear incluso dinámicamente un mensaje con los nombres de propiedad y anotación.</span><span class="sxs-lookup"><span data-stu-id="1988a-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Figura 1](~/ef6/media/figure01.png)

<span data-ttu-id="1988a-119">En la publicación back (método) de esta vista de creación, Entity Framework se usa para guardar el nuevo blog de la base de datos, pero se desencadene la validación del lado cliente de MVC antes de la aplicación llegue a ese código.</span><span class="sxs-lookup"><span data-stu-id="1988a-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="1988a-120">Validación del lado cliente pero no es infalible.</span><span class="sxs-lookup"><span data-stu-id="1988a-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="1988a-121">Los usuarios pueden afectar a las características de su explorador o lo que es peor aún, un pirata informático podría utilizar algunos trucos para evitar las validaciones de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="1988a-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="1988a-122">Pero Entity Framework también reconoce la anotación necesaria y validarlo.</span><span class="sxs-lookup"><span data-stu-id="1988a-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="1988a-123">Una manera sencilla de probar esto es deshabilitar la característica de validación del lado cliente de MVC.</span><span class="sxs-lookup"><span data-stu-id="1988a-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="1988a-124">Puede hacerlo en el archivo web.config de la aplicación de MVC.</span><span class="sxs-lookup"><span data-stu-id="1988a-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="1988a-125">La sección appSettings tiene una clave para ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="1988a-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="1988a-126">Si establece esta clave en false impedirá que la interfaz de usuario realizar validaciones.</span><span class="sxs-lookup"><span data-stu-id="1988a-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="1988a-127">Incluso con la validación del lado cliente deshabilitada, obtendrá la misma respuesta en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1988a-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="1988a-128">Como se mostrará el mensaje de error "el campo título es obligatorio".</span><span class="sxs-lookup"><span data-stu-id="1988a-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="1988a-129">Salvo que ahora será un resultado de validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="1988a-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="1988a-130">Entity Framework llevará a cabo la validación en la anotación necesaria (antes de que incluso lo molesta a la compilación y el comando de INSERCIÓN para enviar a la base de datos) y devolverá el error a MVC que se mostrará el mensaje.</span><span class="sxs-lookup"><span data-stu-id="1988a-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1988a-131">API fluida</span><span class="sxs-lookup"><span data-stu-id="1988a-131">Fluent API</span></span>

<span data-ttu-id="1988a-132">Puede usar código la API fluida del primero en lugar de las anotaciones de obtener el mismo cliente lado & servidor de validación de lado.</span><span class="sxs-lookup"><span data-stu-id="1988a-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="1988a-133">En lugar de uso necesario, le mostraré este con una longitud máxima validación.</span><span class="sxs-lookup"><span data-stu-id="1988a-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="1988a-134">Se aplican configuraciones de la API Fluent como código en primer lugar es generar el modelo de las clases.</span><span class="sxs-lookup"><span data-stu-id="1988a-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="1988a-135">Puede insertar las configuraciones invalidando el método OnModelCreating de la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="1988a-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="1988a-136">Aquí es una configuración que especifica que la propiedad BloggerName no puede tener más de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="1988a-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

<span data-ttu-id="1988a-137">Errores de validación que se produce según las configuraciones de la API Fluent no automáticamente alcance la interfaz de usuario, pero puede capturará en el código y responder a ella en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="1988a-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="1988a-138">Alguna excepción aquí es código de error en la clase de aplicación BlogController que captura ese error de validación cuando Entity Framework intenta guardar un blog con un BloggerName que supera el máximo de 10 caracteres de control.</span><span class="sxs-lookup"><span data-stu-id="1988a-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

<span data-ttu-id="1988a-139">Automáticamente se no pasan la validación en la vista que es el motivo por el código adicional que utiliza ModelState.AddModelError se está usando.</span><span class="sxs-lookup"><span data-stu-id="1988a-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="1988a-140">Esto garantiza que los detalles del error facilitan a la vista que, a continuación, usará el ValidationMessageFor Htmlhelper para mostrar el error.</span><span class="sxs-lookup"><span data-stu-id="1988a-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="1988a-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="1988a-141">IValidatableObject</span></span>

<span data-ttu-id="1988a-142">IValidatableObject es una interfaz que se encuentra en System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="1988a-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="1988a-143">Aunque no es parte de la API de Entity Framework, puede seguir aprovechando lo para la validación del lado servidor en las clases de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1988a-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="1988a-144">IValidatableObject proporciona un método Validate que llamará a Entity Framework durante SaveChanges, o bien puede llamar a usted mismo siempre que desee para validar las clases.</span><span class="sxs-lookup"><span data-stu-id="1988a-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="1988a-145">Configuraciones necesarias, como y MaxLength realizar validación en un único campo.</span><span class="sxs-lookup"><span data-stu-id="1988a-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="1988a-146">En el método Validate puede tener incluso una lógica más compleja, por ejemplo, comparar dos campos.</span><span class="sxs-lookup"><span data-stu-id="1988a-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="1988a-147">En el ejemplo siguiente, la clase de Blog se amplió para implementar IValidatableObject y, a continuación, proporcionar una regla que no pueden coincidir con el título y BloggerName.</span><span class="sxs-lookup"><span data-stu-id="1988a-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

<span data-ttu-id="1988a-148">El constructor ValidationResult toma una cadena que representa el mensaje de error y una matriz de cadenas que representan los nombres de miembros que están asociados con la validación.</span><span class="sxs-lookup"><span data-stu-id="1988a-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="1988a-149">Puesto que esta validación comprueba que el título y el BloggerName, se devuelven ambos nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="1988a-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="1988a-150">A diferencia de la validación proporcionada por la API Fluent, este resultado de la validación se reconocerá la vista y el controlador de excepciones que usé anteriormente para agregar el error en la clase ModelState no es necesario.</span><span class="sxs-lookup"><span data-stu-id="1988a-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="1988a-151">Dado que ambos nombres de propiedad establece en ValidationResult, el HtmlHelpers MVC muestra el mensaje de error para ambas propiedades de.</span><span class="sxs-lookup"><span data-stu-id="1988a-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Figura 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="1988a-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="1988a-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="1988a-154">DbContext tiene un método reemplazable denominado ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="1988a-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="1988a-155">Cuando se llama a SaveChanges, Entity Framework llamará a este método para cada entidad en su caché cuyo estado no se modifica.</span><span class="sxs-lookup"><span data-stu-id="1988a-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="1988a-156">Puede colocar lógica de validación directamente en aquí o, incluso, use este método para llamar a, por ejemplo, el método Blog.Validate agregado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="1988a-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="1988a-157">Este es un ejemplo de una invalidación ValidateEntity que valida las entradas de nuevo para asegurarse de que el título de la publicación no se haya usado ya.</span><span class="sxs-lookup"><span data-stu-id="1988a-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="1988a-158">En primer lugar comprueba si la entidad es una entrada de blog y que su estado se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="1988a-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="1988a-159">Si es así, busca en la base de datos para ver si ya hay una publicación con el mismo título.</span><span class="sxs-lookup"><span data-stu-id="1988a-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="1988a-160">Si hay ya una publicación existente, se crea un nuevo DbEntityValidationResult.</span><span class="sxs-lookup"><span data-stu-id="1988a-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="1988a-161">DbEntityValidationResult aloja un DbEntityEntry y un ICollection de DbValidationErrors para una entidad única.</span><span class="sxs-lookup"><span data-stu-id="1988a-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="1988a-162">Al principio de este método, se crea una instancia de un DbEntityValidationResult y, a continuación, se agregan los errores detectados en su colección ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="1988a-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="1988a-163">Activar explícitamente la validación</span><span class="sxs-lookup"><span data-stu-id="1988a-163">Explicitly triggering validation</span></span>

<span data-ttu-id="1988a-164">Una llamada a SaveChanges desencadena todas las validaciones que se tratan en este artículo.</span><span class="sxs-lookup"><span data-stu-id="1988a-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="1988a-165">Pero no debe depender de SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1988a-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="1988a-166">Prefiere validar en otro lugar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1988a-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="1988a-167">Se desencadenará DbContext.GetValidationErrors todas las validaciones, los definidos por las anotaciones o la API Fluent, la validación que se creó en IValidatableObject (por ejemplo, Blog.Validate) y las validaciones efectuadas en el DbContext.ValidateEntity método.</span><span class="sxs-lookup"><span data-stu-id="1988a-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="1988a-168">El código siguiente llamará GetValidationErrors en la instancia actual de una clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="1988a-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="1988a-169">ValidationErrors se agrupan por tipo de entidad en DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="1988a-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="1988a-170">El código recorre en iteración en primer lugar a través de la DbValidationResults devuelto por el método y, a continuación, cada ValidationError dentro.</span><span class="sxs-lookup"><span data-stu-id="1988a-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="1988a-171">Otras consideraciones al utilizar la validación</span><span class="sxs-lookup"><span data-stu-id="1988a-171">Other considerations when using validation</span></span>

<span data-ttu-id="1988a-172">Estos son algunos otros puntos a tener en cuenta al utilizar la validación de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="1988a-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="1988a-173">La carga diferida está deshabilitada durante la validación.</span><span class="sxs-lookup"><span data-stu-id="1988a-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="1988a-174">EF validará las anotaciones de datos en las propiedades no asignados (propiedades que no están asignadas a una columna de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="1988a-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="1988a-175">Después de que los cambios se detectan durante SaveChanges se realiza la validación.</span><span class="sxs-lookup"><span data-stu-id="1988a-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="1988a-176">Si realiza cambios durante la validación es su responsabilidad para notificar el seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="1988a-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="1988a-177">DbUnexpectedValidationException se produce si se producen errores durante la validación.</span><span class="sxs-lookup"><span data-stu-id="1988a-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="1988a-178">Las facetas que Entity Framework se incluye en el modelo (longitud máxima, necesaria, etc.) producirá la validación, incluso si no hay anotaciones de datos en las clases o utilizado EF Designer para crear el modelo.</span><span class="sxs-lookup"><span data-stu-id="1988a-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="1988a-179">Reglas de prioridad:</span><span class="sxs-lookup"><span data-stu-id="1988a-179">Precedence rules:</span></span>
    -   <span data-ttu-id="1988a-180">Llamadas a la API Fluent anular las anotaciones de datos correspondiente</span><span class="sxs-lookup"><span data-stu-id="1988a-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="1988a-181">Orden de ejecución:</span><span class="sxs-lookup"><span data-stu-id="1988a-181">Execution order:</span></span>
    -   <span data-ttu-id="1988a-182">Validación de propiedades se produce antes de la validación de tipo</span><span class="sxs-lookup"><span data-stu-id="1988a-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="1988a-183">Validación de tipo solo se produce si la propiedad validación es correcta</span><span class="sxs-lookup"><span data-stu-id="1988a-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="1988a-184">Si una propiedad es compleja también incluirá su validación:</span><span class="sxs-lookup"><span data-stu-id="1988a-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="1988a-185">Validación de nivel de propiedad en las propiedades de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="1988a-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="1988a-186">Validación de nivel de tipo en el tipo complejo, incluida la validación de IValidatableObject en el tipo complejo</span><span class="sxs-lookup"><span data-stu-id="1988a-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="1988a-187">Resumen</span><span class="sxs-lookup"><span data-stu-id="1988a-187">Summary</span></span>

<span data-ttu-id="1988a-188">La API de validación en Entity Framework se reproduce perfectamente con validación del lado cliente en MVC, pero no tiene que depender de validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="1988a-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="1988a-189">Entity Framework se encargará de la validación en el servidor de DataAnnotations o configuraciones que se haya aplicado con la API Fluent de code first.</span><span class="sxs-lookup"><span data-stu-id="1988a-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="1988a-190">También ha visto un número de puntos de extensibilidad para personalizar el comportamiento si usa la interfaz IValidatableObject o pulse en el método DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="1988a-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="1988a-191">Y estos últimos dos medias de validación están disponibles a través de la clase DbContext, independientemente de si usa el flujo de trabajo de Code First, Model First o primera base de datos para describir el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="1988a-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
