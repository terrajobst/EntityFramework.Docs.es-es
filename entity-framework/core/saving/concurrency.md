---
title: Controlar la simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>Administrar la simultaneidad

Si una propiedad está configurada como un token de simultaneidad EF comprobará que ningún otro usuario modificó ese valor en la base de datos al guardar los cambios a ese registro.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) en GitHub.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Cómo funciona el control de simultaneidad en EF Core

Para obtener una descripción detallada de cómo funciona el control de simultaneidad en Entity Framework Core, vea [Tokens de simultaneidad](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Resolver conflictos de simultaneidad

Resolver un conflicto de simultaneidad implica el uso de un algoritmo para combinar los cambios pendientes del usuario actual con los cambios realizados en la base de datos. El método exacto varía en función de la aplicación, pero un enfoque común consiste en mostrar los valores para el usuario y hacer que decida los valores correctos que se almacenará en la base de datos.

**Hay tres conjuntos de valores disponibles para ayudar a resolver un conflicto de simultaneidad.**

* **Valores actuales** son los valores que la aplicación estaba intentando escribir en la base de datos.

* **Valores originales** son los valores que se recuperaron originalmente desde la base de datos antes de que se realizaron las modificaciones.

* **Valores de la base de datos** son los valores almacenados actualmente en la base de datos.

Para controlar un conflicto de simultaneidad, detectar un `DbUpdateConcurrencyException` durante `SaveChanges()`, use `DbUpdateConcurrencyException.Entries` para preparar un nuevo conjunto de cambios para las entidades afectadas y, a continuación, vuelva a intentar la `SaveChanges()` operación.

En el ejemplo siguiente, `Person.FirstName` y `Person.LastName` están configurados como token de simultaneidad. Hay un `// TODO:` comentario en la ubicación donde debe incluir lógica específica de aplicación para elegir el valor que se van a guardar en la base de datos.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
