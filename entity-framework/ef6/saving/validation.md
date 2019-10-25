---
title: Validación-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812150"
---
# <a name="data-validation"></a><span data-ttu-id="df090-102">Validación de datos</span><span class="sxs-lookup"><span data-stu-id="df090-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="df090-103">**EF 4.1 en adelante solo** : las características, las API, etc. que se describen en esta página se introdujeron en Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="df090-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="df090-104">Si usa una versión anterior, parte o toda la información no se aplica.</span><span class="sxs-lookup"><span data-stu-id="df090-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="df090-105">El contenido de esta página se adapta a un artículo escrito originalmente por Julia Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="df090-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="df090-106">Entity Framework proporciona una gran variedad de características de validación que se pueden transmitir a través de una interfaz de usuario para la validación del lado cliente o se pueden usar para la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="df090-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="df090-107">Cuando se usa Code First, se pueden especificar validaciones mediante las configuraciones de anotación o de API fluida.</span><span class="sxs-lookup"><span data-stu-id="df090-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="df090-108">Las validaciones adicionales, y más complejas, se pueden especificar en el código y funcionarán si el modelo es el primero de Code, Model First o Database.</span><span class="sxs-lookup"><span data-stu-id="df090-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="df090-109">El modelo</span><span class="sxs-lookup"><span data-stu-id="df090-109">The model</span></span>

<span data-ttu-id="df090-110">Mostraré las validaciones con un par sencillo de clases: blog y post.</span><span class="sxs-lookup"><span data-stu-id="df090-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

## <a name="data-annotations"></a><span data-ttu-id="df090-111">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="df090-111">Data Annotations</span></span>

<span data-ttu-id="df090-112">Code First usa anotaciones del ensamblado de `System.ComponentModel.DataAnnotations` como un medio para configurar clases Code First.</span><span class="sxs-lookup"><span data-stu-id="df090-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="df090-113">Entre estas anotaciones se encuentran las que proporcionan reglas como `Required`, `MaxLength` y `MinLength`.</span><span class="sxs-lookup"><span data-stu-id="df090-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="df090-114">Varias aplicaciones cliente de .NET también reconocen estas anotaciones, por ejemplo, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="df090-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="df090-115">Puede lograr la validación del lado cliente y del lado servidor con estas anotaciones.</span><span class="sxs-lookup"><span data-stu-id="df090-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="df090-116">Por ejemplo, puede forzar que la propiedad título del blog sea una propiedad necesaria.</span><span class="sxs-lookup"><span data-stu-id="df090-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="df090-117">Sin ningún cambio de código o marcado adicional en la aplicación, una aplicación MVC existente realizará la validación del lado cliente, incluso generando dinámicamente un mensaje usando los nombres de la propiedad y de las anotaciones.</span><span class="sxs-lookup"><span data-stu-id="df090-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Figura 1](~/ef6/media/figure01.png)

<span data-ttu-id="df090-119">En el método posterior de esta vista de creación, Entity Framework se usa para guardar el nuevo blog en la base de datos, pero la validación del lado cliente de MVC se desencadena antes de que la aplicación llegue a ese código.</span><span class="sxs-lookup"><span data-stu-id="df090-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="df090-120">Sin embargo, la validación del lado cliente no es una prueba de viñetas.</span><span class="sxs-lookup"><span data-stu-id="df090-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="df090-121">Los usuarios pueden afectar a las características de su explorador o peor aún, un pirata informático podría usar algún truco para evitar las validaciones de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="df090-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="df090-122">Sin embargo, Entity Framework también reconocerá la anotación `Required` y la validará.</span><span class="sxs-lookup"><span data-stu-id="df090-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="df090-123">Una manera sencilla de probarlo es deshabilitar la característica de validación del lado cliente de MVC.</span><span class="sxs-lookup"><span data-stu-id="df090-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="df090-124">Puede hacerlo en el archivo Web. config de la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="df090-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="df090-125">La sección appSettings tiene una clave para ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="df090-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="df090-126">Si se establece esta clave en false, impedirá que la interfaz de usuario realice validaciones.</span><span class="sxs-lookup"><span data-stu-id="df090-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="df090-127">Incluso con la validación del lado cliente deshabilitada, obtendrá la misma respuesta en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="df090-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="df090-128">El mensaje de error "el campo título es obligatorio" se mostrará como antes.</span><span class="sxs-lookup"><span data-stu-id="df090-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="df090-129">A excepción de ahora, será el resultado de la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="df090-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="df090-130">Entity Framework realizará la validación en la anotación de `Required` (antes que los dos para crear un comando de `INSERT` para enviarlo a la base de datos) y devolverá el error a MVC, que mostrará el mensaje.</span><span class="sxs-lookup"><span data-stu-id="df090-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="df090-131">API fluida</span><span class="sxs-lookup"><span data-stu-id="df090-131">Fluent API</span></span>

<span data-ttu-id="df090-132">Puede usar la API fluida de Code First en lugar de anotaciones para obtener el mismo cliente & la validación del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="df090-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="df090-133">En lugar de usar `Required`, lo mostraré con una validación de MaxLength.</span><span class="sxs-lookup"><span data-stu-id="df090-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="df090-134">Las configuraciones de la API fluida se aplican como Code First está compilando el modelo a partir de las clases.</span><span class="sxs-lookup"><span data-stu-id="df090-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="df090-135">Puede insertar las configuraciones invalidando el método OnModelCreating de la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="df090-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="df090-136">A continuación se muestra una configuración que especifica que la propiedad BloggerName no puede tener más de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="df090-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="df090-137">Los errores de validación que se produzcan en función de las configuraciones de la API fluida no llegarán automáticamente a la interfaz de usuario, pero puede capturarlos en el código y, a continuación, responder a ellos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="df090-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="df090-138">Este es un código de error de control de excepciones en la clase BlogController de la aplicación que captura ese error de validación cuando Entity Framework intenta guardar un blog con un BloggerName que supere el máximo de 10 caracteres.</span><span class="sxs-lookup"><span data-stu-id="df090-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="df090-139">La validación no se vuelve a pasar automáticamente a la vista, que es el motivo por el que se usa el código adicional que usa `ModelState.AddModelError`.</span><span class="sxs-lookup"><span data-stu-id="df090-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="df090-140">Esto garantiza que los detalles del error lo convierten en la vista que utilizará el `ValidationMessageFor` HtmlHelper para mostrar el error.</span><span class="sxs-lookup"><span data-stu-id="df090-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="df090-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="df090-141">IValidatableObject</span></span>

<span data-ttu-id="df090-142">`IValidatableObject` es una interfaz que reside en `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="df090-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="df090-143">Aunque no forma parte de la API de Entity Framework, todavía puede aprovecharla para la validación del lado servidor en las clases de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="df090-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="df090-144">`IValidatableObject` proporciona un método de `Validate` que Entity Framework llamará durante SaveChanges o se puede llamar a sí mismo en cualquier momento en el que desee validar las clases.</span><span class="sxs-lookup"><span data-stu-id="df090-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="df090-145">Las configuraciones como `Required` y `MaxLength` realizan la validación en un único campo.</span><span class="sxs-lookup"><span data-stu-id="df090-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="df090-146">En el método `Validate` puede tener una lógica aún más compleja, por ejemplo, comparando dos campos.</span><span class="sxs-lookup"><span data-stu-id="df090-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="df090-147">En el ejemplo siguiente, se ha extendido la clase `Blog` para implementar `IValidatableObject` y, a continuación, proporcionar una regla que el `Title` y el `BloggerName` no coincidan.</span><span class="sxs-lookup"><span data-stu-id="df090-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="df090-148">El constructor `ValidationResult` toma un `string` que representa el mensaje de error y una matriz de `string`s que representan los nombres de miembro que están asociados a la validación.</span><span class="sxs-lookup"><span data-stu-id="df090-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="df090-149">Dado que esta validación comprueba el `Title` y el `BloggerName`, se devuelven ambos nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="df090-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="df090-150">A diferencia de la validación proporcionada por la API fluida, el resultado de la validación lo reconocerá en la vista y el controlador de excepción que se usó anteriormente para agregar el error a `ModelState` no es necesario.</span><span class="sxs-lookup"><span data-stu-id="df090-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="df090-151">Dado que he establecido ambos nombres de propiedad en el `ValidationResult`, MVC HtmlHelpers muestra el mensaje de error de ambas propiedades.</span><span class="sxs-lookup"><span data-stu-id="df090-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Ilustración 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="df090-153">DbContext. ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="df090-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="df090-154">`DbContext` tiene un método reemplazable denominado `ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="df090-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="df090-155">Cuando llame a `SaveChanges`, Entity Framework llamará a este método para cada entidad de su caché cuyo estado no sea `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="df090-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="df090-156">Puede colocar la lógica de validación directamente aquí o incluso usar este método para llamar a, por ejemplo, el método `Blog.Validate` agregado en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="df090-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="df090-157">A continuación se muestra un ejemplo de una invalidación `ValidateEntity` que valida las nuevas `Post`para asegurarse de que el título de la publicación no se ha usado ya.</span><span class="sxs-lookup"><span data-stu-id="df090-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="df090-158">Primero se comprueba si la entidad es una publicación y se agrega su estado.</span><span class="sxs-lookup"><span data-stu-id="df090-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="df090-159">Si ese es el caso, busca en la base de datos para ver si ya existe una publicación con el mismo título.</span><span class="sxs-lookup"><span data-stu-id="df090-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="df090-160">Si ya hay una publicación existente, se crea un nuevo `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="df090-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="df090-161">`DbEntityValidationResult` aloja un `DbEntityEntry` y un `ICollection<DbValidationErrors>` para una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="df090-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="df090-162">Al principio de este método, se crea una instancia de `DbEntityValidationResult` y, a continuación, se agregan los errores detectados a su colección de `ValidationErrors`.</span><span class="sxs-lookup"><span data-stu-id="df090-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="df090-163">Desencadenar explícitamente la validación</span><span class="sxs-lookup"><span data-stu-id="df090-163">Explicitly triggering validation</span></span>

<span data-ttu-id="df090-164">Una llamada a `SaveChanges` desencadena todas las validaciones que se describen en este artículo.</span><span class="sxs-lookup"><span data-stu-id="df090-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="df090-165">Pero no es necesario depender de `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="df090-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="df090-166">Puede que prefiera validar cualquier otro lugar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="df090-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="df090-167">`DbContext.GetValidationErrors` desencadenará todas las validaciones, las definidas por las anotaciones o la API fluida, la validación creada en `IValidatableObject` (por ejemplo, `Blog.Validate`) y las validaciones realizadas en el método de `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="df090-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="df090-168">El código siguiente llamará `GetValidationErrors` en la instancia actual de un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="df090-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="df090-169">`ValidationErrors` se agrupan por tipo de entidad en `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="df090-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="df090-170">El código recorre en iteración los `DbEntityValidationResult`s devueltos por el método y, a continuación, a través de cada `DbValidationError` dentro de.</span><span class="sxs-lookup"><span data-stu-id="df090-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="df090-171">Otras consideraciones al usar la validación</span><span class="sxs-lookup"><span data-stu-id="df090-171">Other considerations when using validation</span></span>

<span data-ttu-id="df090-172">Estos son algunos aspectos que se deben tener en cuenta al usar la validación de Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="df090-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="df090-173">La carga diferida está deshabilitada durante la validación</span><span class="sxs-lookup"><span data-stu-id="df090-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="df090-174">EF validará las anotaciones de datos en propiedades no asignadas (propiedades que no están asignadas a una columna en la base de datos)</span><span class="sxs-lookup"><span data-stu-id="df090-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="df090-175">La validación se realiza después de que se detecten los cambios durante la `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="df090-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="df090-176">Si realiza cambios durante la validación, es su responsabilidad notificar al seguimiento de cambios</span><span class="sxs-lookup"><span data-stu-id="df090-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="df090-177">se produce `DbUnexpectedValidationException` si se producen errores durante la validación</span><span class="sxs-lookup"><span data-stu-id="df090-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="df090-178">Las caras que Entity Framework incluye en el modelo (longitud máxima, requerida, etc.) provocarán la validación, incluso si no hay ninguna anotación de datos en las clases o si se usó el diseñador de EF para crear el modelo.</span><span class="sxs-lookup"><span data-stu-id="df090-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="df090-179">Reglas de prioridad:</span><span class="sxs-lookup"><span data-stu-id="df090-179">Precedence rules:</span></span>
  - <span data-ttu-id="df090-180">Las llamadas a la API fluida invalidan las anotaciones de datos correspondientes</span><span class="sxs-lookup"><span data-stu-id="df090-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="df090-181">Orden de ejecución:</span><span class="sxs-lookup"><span data-stu-id="df090-181">Execution order:</span></span>
  - <span data-ttu-id="df090-182">La validación de la propiedad tiene lugar antes de la validación de tipos</span><span class="sxs-lookup"><span data-stu-id="df090-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="df090-183">La validación de tipos solo se produce si la validación de la propiedad se realiza correctamente</span><span class="sxs-lookup"><span data-stu-id="df090-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="df090-184">Si una propiedad es compleja, su validación también incluirá:</span><span class="sxs-lookup"><span data-stu-id="df090-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="df090-185">Validación de nivel de propiedad en las propiedades de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="df090-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="df090-186">Validación del nivel de tipo en el tipo complejo, incluida la validación de `IValidatableObject` en el tipo complejo</span><span class="sxs-lookup"><span data-stu-id="df090-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="df090-187">Resumen</span><span class="sxs-lookup"><span data-stu-id="df090-187">Summary</span></span>

<span data-ttu-id="df090-188">La API de validación en Entity Framework se reproduce muy bien con la validación del lado cliente en MVC, pero no tiene que depender de la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="df090-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="df090-189">Entity Framework se encargará de la validación en el lado del servidor para las anotaciones o configuraciones que haya aplicado con la API fluida de Code First.</span><span class="sxs-lookup"><span data-stu-id="df090-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="df090-190">También ha visto una serie de puntos de extensibilidad para personalizar el comportamiento si usa la interfaz `IValidatableObject` o si va al método `DbContext.ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="df090-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="df090-191">Y estos dos últimos métodos de validación están disponibles a través del `DbContext`, independientemente de que use el flujo de trabajo Code First, Model First o Database First para describir el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="df090-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
