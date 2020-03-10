---
title: 'Creación y configuración de un modelo: EF Core'
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412780"
---
# <a name="creating-and-configuring-a-model"></a>Creación y configuración de un modelo

Entity Framework usa un conjunto de convenciones para compilar un modelo basado en la forma de las clases de entidad. Puede especificar una configuración adicional para complementar o reemplazar lo que se ha detectado por convención.

Este artículo trata de la configuración que se puede aplicar a un modelo para cualquier almacén de datos y que se puede aplicar al elegir como destino cualquier base de datos relacional. Los proveedores también pueden habilitar la configuración específica de un almacén de datos determinado. Para obtener documentación sobre la configuración específica del proveedor, vea la sección  [Proveedores de bases de datos](../providers/index.md) .

> [!TIP]  
> Puede ver un  [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples)  de este artículo en GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Uso de la API fluida para configurar un modelo

Puede reemplazar el método  `OnModelCreating`  del contexto derivado y usar  `ModelBuilder API` para configurar el modelo. Este es el método más eficaz de configuración y permite especificar la configuración sin modificar las clases de entidad. La configuración de API fluida tiene la prioridad más alta y reemplaza las anotaciones de datos y las convenciones.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Uso de anotaciones de datos para configurar un modelo

También puede aplicar atributos (conocidos como anotaciones de datos) a las clases y las propiedades. Las anotaciones de datos reemplazarán a las convenciones, pero la configuración de la API fluida también las reemplazará.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
