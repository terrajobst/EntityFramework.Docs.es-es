---
title: 'Creación de un modelo: EF6'
author: divega
ms.date: 2018-07-05
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4890228E-CEA1-4595-B8AD-CA81253F8767
caps.latest.revision: 3
ms.openlocfilehash: e1eb4077a1fd77c66bb3e992e1d25a5de4fcc64c
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913517"
---
# <a name="creating-a-model"></a>Creación de un modelo

Un modelo de EF almacena los detalles sobre cómo se asignan las propiedades y las clases de la aplicación a las columnas y las tablas de la base de datos. Hay dos formas principales de crear un modelo de EF:

- **Uso de Code First**: el desarrollador escribe código para especificar el modelo. EF genera los modelos y las asignaciones en tiempo de ejecución según las clases de entidad y la configuración de modelo adicional proporcionados por el desarrollador.

- **Uso de EF Designer**: el desarrollador dibuja cuadros y líneas para especificar el modelo con EF Designer. El modelo resultante se almacena como XML en un archivo con la extensión EDMX. Los objetos de dominio de la aplicación normalmente se generan de forma automática desde el modelo conceptual.

## <a name="ef-workflows"></a>Flujos de trabajo de EF

Estos dos enfoques pueden usarse para establecer como destino una base de datos existente o para crear una nueva base de datos, lo que da lugar a cuatro flujos de trabajo diferentes.
Descubra cuál es el más adecuado en su caso:  

|                                           | Solo quiero escribir código...                                                                                                                   | Quiero usar un diseñador...                                                                                                                        |
|:------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Voy a crear una nueva base de datos**          | [Use **Code First** para definir el modelo en el código y luego generar una base de datos.](~/ef6/modeling/code-first/workflows/new-database.md)           | [Use **Model First** para definir el modelo mediante cuadros y líneas y luego generar una base de datos.](~/ef6/modeling/designer/workflows/model-first.md)   |
| **Necesito acceder a una base de datos existente** | [Use **Code First** para crear un modelo basado en código que se asigne a una base de datos existente.](~/ef6/modeling/code-first/workflows/existing-database.md) | [Use **Database First** para crear un modelo de cuadros y líneas que se asigne a una base de datos existente.](~/ef6/modeling/designer/workflows/database-first.md) |

### <a name="watch-the-video-what-ef-workflow-should-i-use"></a>Vea el vídeo: What EF workflow should I use? (Qué flujo de trabajo de EF debo usar)

En este breve vídeo se explican las diferencias y cómo encontrar el adecuado para usted.

**Presentado por**: [Rowan Miller](http://romiller.com/)

![WhichWorkflow_Thumb](../media/whichworkflow-thumb.png) [WMV](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.wmv) | [MP4](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_mp4video_ChoseYourWorkflow.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/8/8F81F4CD-3678-4229-8D79-0C63FFA3C595/HDI_ITPro_Technet_winvideo_ChoseYourWorkflow.zip)

Si después de ver el vídeo todavía no se siente cómodo para decidir si quiere usar EF Designer o Code First, obtenga información sobre ambos.

## <a name="a-look-under-the-hood"></a>Aspectos técnicos

Independientemente de si usa Code First o EF Designer, un modelo de EF siempre tiene varios componentes:

- Los objetos de dominio de la aplicación o los propios tipos de entidad. Esto se suele denominar la capa de objeto

- Un modelo conceptual que consta de tipos de entidad específicos de dominio y relaciones, que se describen mediante [Entity Data Model](~/ef6/resources/glossary.md#entity-data-model). Se suele hacer referencia a esta capa con la letra "C", por _conceptual_.

- Un modelo de almacenamiento que representa las tablas, las columnas y las relaciones según se definen en la base de datos. Se suele hacer referencia a esta capa con la letra "S", por _storage_ (almacenamiento en inglés).  

- Una asignación entre el modelo conceptual y el esquema de base de datos. Esta asignación se suele conocer como asignación "C-S".

El motor de asignación de EF aprovecha la asignación "C-S" para transformar las operaciones en entidades (por ejemplo, crear, leer, actualizar y eliminar) en operaciones equivalentes en las tablas de la base de datos.

La asignación entre el modelo conceptual y los objetos de la aplicación se suele conocer como asignación "O-C". En comparación con la asignación "C-S", la asignación "O-C" es implícita y de uno a uno: las entidades, propiedades y relaciones definidas en el modelo conceptual tienen que coincidir con las formas y los tipos de los objetos .NET. A partir de EF4 y versiones posteriores, la capa de objetos puede componerse de objetos simples con propiedades sin dependencias en EF. Estos se conocen normalmente como objetos CLR estándar (POCO) y la asignación de tipos y propiedades se realiza según las convenciones de coincidencia de nombres. Anteriormente, en EF 3.5, había restricciones específicas para la capa de objeto, por ejemplo, las entidades tenían que derivar de la clase EntityObject e incluir atributos de EF para implementar la asignación "O-C".
