---
title: Glosario de Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: cbd61838afc23dfb37cee7c624c65476c5270099
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122735"
---
# <a name="entity-framework-glossary"></a>Glosario de Entity Framework
## <a name="code-first"></a>Code First
Creación de un modelo de Entity Framework mediante código. Puede tener como destino el modelo y base de datos existente o una base de datos.

## <a name="context"></a>Contexto
Una clase que representa una sesión con la base de datos, lo que le permite consultar y guardar los datos. Un contexto se deriva de la clase DbContext o ObjectContext.

## <a name="convention-code-first"></a>Convención (Code First)
Una regla que Entity Framework usa para inferir la forma del modelo desde las clases.

## <a name="database-first"></a>En primer lugar la base de datos
Creación de un modelo de Entity Framework mediante el Diseñador de EF, destinado a una base de datos existente.

## <a name="eager-loading"></a>Carga diligente
Modelo de carga de datos relacionados en una consulta para un tipo de entidad también carga las entidades relacionadas como parte de la consulta.

## <a name="ef-designer"></a>Diseñador de EF
Un diseñador visual de Visual Studio que le permite crear un modelo de Entity Framework con cuadros y líneas.

## <a name="entity"></a>Entity
Clase u objeto que representa datos de aplicación como clientes, productos y pedidos.

## <a name="entity-data-model"></a>Entity Data Model
Un modelo que describe las entidades y las relaciones entre ellos. EF usa el EDM para describir el modelo conceptual en el que los programas de desarrollador. EDM se basa en el modelo de entidad relación introducido por la recuperación ante desastres. Peter Chen. El EDM se desarrolló originalmente con el objetivo principal de convertirse en el modelo de datos comunes a través de un conjunto de tecnologías de desarrollador y servidor de Microsoft. EDM también se usa como parte del Protocolo OData.

## <a name="explicit-loading"></a>Carga explícita
Modelo de carga de datos relacionados que se cargan los objetos relacionados mediante una llamada a una API.

## <a name="fluent-api"></a>API fluida
Una API que puede usarse para configurar un modelo de Code First.

## <a name="foreign-key-association"></a>Asociación de clave externa
Una asociación entre entidades que se incluye una propiedad que representa la clave externa en la clase de la entidad dependiente. Por ejemplo, el producto contiene una propiedad de Id. de categoría.

## <a name="identifying-relationship"></a>Relación de identificación
Relación donde la clave principal de la entidad de seguridad también forma parte de la clave principal de la entidad dependiente. En este tipo de relación, la entidad dependiente no puede existir sin la entidad de seguridad.

## <a name="independent-association"></a>Asociación independiente
Una asociación entre entidades que no hay ninguna propiedad que representa la clave externa de la clase de la entidad dependiente. Por ejemplo, una clase de producto contiene una relación con categoría pero ninguna propiedad CategoryId. Entity Framework realiza un seguimiento del estado de la asociación con independencia del estado de las entidades en los extremos de asociación de dos.

## <a name="lazy-loading"></a>Carga diferida
Modelo de carga de datos relacionados, donde los objetos relacionados se cargan automáticamente cuando se tiene acceso a una propiedad de navegación.

## <a name="model-first"></a>En primer lugar del modelo
Creación de un modelo de Entity Framework mediante el Diseñador de EF, que, a continuación, sirve para crear una nueva base de datos.

## <a name="navigation-property"></a>Propiedad de navegación
Una propiedad de una entidad que hace referencia a otra entidad. Por ejemplo, el producto contiene una propiedad de navegación de la categoría y categoría contiene una propiedad de navegación de productos.

## <a name="poco"></a>POCO
Acrónimo de objetos CLR antiguos sin formato. Una clase de usuario simple que no tiene ninguna dependencia con cualquier marco de trabajo. En el contexto de EF, un una clase de entidad que no se deriva de EntityObject, implementa las interfaces o lleva a cabo los atributos definidos en EF. Estas clases de entidad que se desconectan entre el marco de persistencia también se dice que "que ignoran la persistencia".  

## <a name="relationship-inverse"></a>Inverso de relación
El extremo opuesto de una relación, por ejemplo, el producto. Categoría y categoría. Producto.

## <a name="self-tracking-entity"></a>Entidad con seguimiento propio
Una entidad creada desde una plantilla de generación de código que ayuda a con el desarrollo de N niveles.

## <a name="table-per-concrete-type-tpc"></a>Tipo de tabla por concreta (TPC)
Un método de asignación de la herencia donde cada tipo no abstracto en la jerarquía se asigna a una tabla en la base de datos independiente.

## <a name="table-per-hierarchy-tph"></a>Tabla por jerarquía (TPH)
Un método de asignación de la herencia donde todos los tipos de la jerarquía se asignan a la misma tabla en la base de datos. Está asociado el discriminador columnas se usan para identificar qué tipo de cada fila.

## <a name="table-per-type-tpt"></a>Tabla por tipo (TPT)
Un método de asignación de la herencia, donde las propiedades comunes de todos los tipos de la jerarquía se asignan a la misma tabla en la base de datos, pero las propiedades únicas para cada tipo se asignan a una tabla independiente.

## <a name="type-discovery"></a>Detección de tipos
El proceso de identificación de los tipos que deben formar parte de un modelo de Entity Framework.
