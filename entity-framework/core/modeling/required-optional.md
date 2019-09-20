---
title: 'Propiedades obligatorias y opcionales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149148"
---
# <a name="required-and-optional-properties"></a>Propiedades obligatorias y opcionales

Una propiedad se considera opcional si es válida para que la contenga `null`. Si `null` no es un valor válido que se va a asignar a una propiedad, se considera que es una propiedad obligatoria.

## <a name="conventions"></a>Convenciones

Por Convención, una propiedad cuyo tipo .net pueda contener NULL se configurará como opcional`string`( `int?`, `byte[]`,, etc.). Las propiedades cuyo tipo CLR no puede contener valores NULL se configurarán `decimal`como `bool`Required (`int`,,, etc.).

> [!NOTE]  
> Una propiedad cuyo tipo .NET no puede contener valores NULL no se puede configurar como opcional. La propiedad siempre será considerada requerida por Entity Framework.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para indicar que una propiedad es obligatoria.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para indicar que se requiere una propiedad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

