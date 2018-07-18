---
title: Métodos abreviados de teclado del Diseñador de Framework Entity - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
caps.latest.revision: 3
ms.openlocfilehash: a4ed95647872949d9e34a6bb5d83d5d6119b0022
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122816"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Métodos abreviados de teclado del Diseñador de Entity Framework
Esta página proporciona una lista de accesos directos de teclado que están disponibles en las pantallas de distintos de Entity Framework Tools para Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Asistente de ADO.NET Entity Data Model

### <a name="step-one-choose-model-contents"></a>Paso 1: Elegir contenido de Model

![WizardOne](~/ef6/media/wizardone.png)

| Acceso directo  | Acción                                                     | Notas                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Mover a la siguiente pantalla                                        | No está disponible para todas las selecciones de contenido del modelo. |
| **ALT + f** | Finalizar el Asistente                                              | No está disponible para todas las selecciones de contenido del modelo. |
| **ALT + w** | Cambiar el foco a la "lo que el modelo contuviera?" . |                                                     |

### <a name="step-two-choose-your-connection"></a>Paso 2: Elegir la conexión

![WizardTwo](~/ef6/media/wizardtwo.png)

| Acceso directo  | Acción                                                     | Notas                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Mover a la siguiente pantalla                                        |                                                         |
| **ALT + p** | Mover a la pantalla anterior                                    |                                                         |
| **ALT + w** | Cambiar el foco a la "lo que el modelo contuviera?" . |                                                         |
| **Alt + c** | Abra la ventana "Propiedades de conexión"                    | Permite la definición de una nueva conexión de base de datos. |
| **ALT + e** | Excluir datos confidenciales de la cadena de conexión          |                                                         |
| **ALT +** | Incluir datos confidenciales en la cadena de conexión            |                                                         |
| **ALT + s** | Activar o desactivar la opción "Guardar configuración de conexión en el archivo App.Config" |                                                         |

### <a name="step-three-choose-your-version"></a>Paso 3: Elija su versión

![WizardThree](~/ef6/media/wizardthree.png)

| Acceso directo  | Acción                                             | Notas                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Mover a la siguiente pantalla                                |                                                                                       |
| **ALT + p** | Mover a la pantalla anterior                            |                                                                                       |
| **ALT + w** | Cambiar el foco a la selección de la versión de Entity Framework | Permite especificar una versión diferente de Entity Framework para su uso en el proyecto. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Paso cuatro: Elija la configuración y los objetos de base de datos

![WizardFour](~/ef6/media/wizardfour.png)

| Acceso directo  | Acción                                                                                    | Notas                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Finalizar el Asistente                                                                             |                                                                     |
| **ALT + p** | Mover a la pantalla anterior                                                                   |                                                                     |
| **ALT + w** | Cambiar el foco al panel de selección de objetos de base de datos                                            | Permite especificar objetos de base de datos que se van a invertir de ingeniería.    |
| **ALT + s** | Alternar el "poner en plural o en singular los nombres de objeto generado" opción                       |                                                                     |
| **ALT + k** | Activar o desactivar la opción "Incluir columnas de clave externa en el modelo"                              | No está disponible para todas las selecciones de contenido del modelo.                 |
| **ALT +** | Activar o desactivar la opción "Importar seleccionada procedimientos almacenados y funciones en el modelo de entidad" | No está disponible para todas las selecciones de contenido del modelo.                 |
| **ALT + m** | Cambia el foco al campo de texto de "Espacio de nombres del modelo"                                        | No está disponible para todas las selecciones de contenido del modelo.                 |
| **Espacio** | Alternar selección en el elemento                                                               | Si el elemento tiene elementos secundarios, todos los elementos secundarios se pueden activar o desactivar también |
| **Izquierda**  | Árbol de contraer secundarios                                                                       |                                                                     |
| **Derecha** | Expanda el árbol secundario                                                                         |                                                                     |
| **Arriba**    | Desplazarse al elemento anterior en el árbol                                                      |                                                                     |
| **Abajo**  | Vaya al siguiente elemento de árbol                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Superficie del Diseñador de EF

![DesignerSurface](~/ef6/media/designersurface.png)

| Acceso directo                                                                                | Acción                      | Notas                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Espacio o escriba**                                                                         | Alternar selección            | Alterna la selección en el objeto con el foco.                                                                                                                                                                                         |
| **Esc**                                                                                 | Cancelar selección            | Cancela la selección actual.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Seleccionar todo                  | Selecciona todas las formas en la superficie de diseño.                                                                                                                                                                                       |
| **Flecha arriba**                                                                            | Subir                     | Mueve los seleccionados entidad superior de incremento de una cuadrícula. <br/> Si se encuentra en una lista, se desplaza hasta el campo secundario relacionado anterior.                                                                                                                            |
| **Flecha abajo**                                                                          | Bajar                   | Mueve los seleccionados entidad hacia abajo el incremento de una cuadrícula. <br/> Si se encuentra en una lista, se desplaza hasta el campo secundario relacionado siguiente.                                                                                                                              |
| **Flecha izquierda**                                                                          | Mover a la izquierda                   | Mueve los seleccionados incremento de una cuadrícula izquierdo de entidad. <br/> Si se encuentra en una lista, se desplaza hasta el campo secundario relacionado anterior.                                                                                                                          |
| **Flecha derecha**                                                                         | Mover a la derecha                  | Mueve los seleccionados incremento de la cuadrícula derecha una entidad. <br/> Si se encuentra en una lista, se desplaza hasta el campo secundario relacionado siguiente.                                                                                                                             |
| **Mayús + flecha izquierda**                                                                  | Forma de tamaño izquierdo             | Reduce el ancho de la entidad seleccionada en el incremento de una cuadrícula.                                                                                                                                                                     |
| **Mayús + flecha derecha**                                                                 | Forma de tamaño derecho            | Aumenta el ancho de la entidad seleccionada en el incremento de una cuadrícula.                                                                                                                                                                   |
| **Página principal**                                                                                | Primer elemento del mismo nivel                  | Mover el foco y la selección al primer objeto en la superficie de diseño en el mismo nivel.                                                                                                                                         |
| **Fin**                                                                                 | Último elemento del mismo nivel                   | Mover el foco y selección hasta el último objeto en la superficie de diseño en el mismo nivel.                                                                                                                                          |
| **CTRL + Inicio**                                                                         | Primer elemento del mismo nivel (foco)          | Igual que el primer elemento del mismo nivel, pero mover el foco en lugar de mover el foco y la selección.                                                                                                                                                          |
| **CTRL + fin**                                                                          | Último elemento del mismo nivel (foco)           | Igual que el último del mismo nivel, pero mueve el foco en lugar de mover el foco y la selección.                                                                                                                                                           |
| **Tabulación**                                                                                 | Siguiente elemento del mismo nivel                   | Mover el foco y la selección al siguiente objeto en la superficie de diseño en el mismo nivel.                                                                                                                                          |
| **Mayús+Tab**                                                                           | Anterior elemento del mismo nivel               | Mover el foco y la selección al objeto anterior en la superficie de diseño en el mismo nivel.                                                                                                                                      |
| **Ctrl + Alt + Tab**                                                                        | Siguiente elemento del mismo nivel (foco)           | Igual que del mismo nivel, pero mueve el foco en lugar de mover el foco y la selección.                                                                                                                                                           |
| **Alt + Ctrl + Mayús + Tab**                                                                  | Anterior elemento del mismo nivel (foco)       | Igual que el anterior del mismo nivel, pero mover el foco en lugar de mover el foco y la selección.                                                                                                                                                       |
| **&lt;**                                                                                | Ascendente                      | Se desplaza al siguiente objeto en la superficie de diseño en un nivel superior de la jerarquía. Si no hay ninguna forma por encima de esta forma en la jerarquía (es decir, el objeto se coloca directamente en la superficie de diseño), el diagrama está seleccionado. |
| **&gt;**                                                                                | Descienden                     | Se desplaza al siguiente objeto contenido en la superficie de diseño un nivel por debajo de ésta en la jerarquía. Si no hay ningún objeto contenido, esto es una operación inefectiva.                                                                              |
| **CTRL + &lt;**                                                                         | Ascendente (foco)              | Igual que ascend comando, pero sin seleccionar mover el foco.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Descienden (foco)             | Igual que descienden de comando, pero sin seleccionar mover el foco.                                                                                                                                                                         |
| **MAYÚS + fin**                                                                         | Siga a conectado         | Desde una entidad, se mueve a una entidad que esta entidad está conectada a.                                                                                                                                                               |
| **Supr**                                                                                 | Eliminar                      | Eliminar un objeto o el conector del diagrama.                                                                                                                                                                                     |
| **INS**                                                                                 | Insertar                      | Agrega una nueva propiedad a una entidad cuando se selecciona el encabezado de compartimiento "Propiedades escalares" o una propiedad en Sí.                                                                                                           |
| **Pág**                                                                               | Diagrama de desplazamiento de           | Desplaza la superficie de diseño, en incrementos iguales al 75% del alto de la superficie de diseño actualmente visible.                                                                                                                    |
| **PG hacia abajo**                                                                             | Diagrama de desplazamiento hacia abajo         | La superficie de diseño se desplaza hacia abajo.                                                                                                                                                                                                    |
| **MAYÚS + Pg hacia abajo**                                                                     | Diagrama de desplazamiento derecha        | Se desplaza a la superficie de diseño a la derecha.                                                                                                                                                                                            |
| **MAYÚS + Pág**                                                                       | Diagrama de desplazamiento izquierda         | Se desplaza a la superficie de diseño a la izquierda.                                                                                                                                                                                             |
| **F2**                                                                                  | Modo de edición             | Método abreviado de teclado estándar para entrar en modo de edición para un control de texto.                                                                                                                                                               |
| **MAYÚS + F10**                                                                         | Mostrar el menú contextual       | Método abreviado de teclado estándar para mostrar el menú contextual de un elemento seleccionado.                                                                                                                                                          |
| **Control + Mayús + Mouse el botón primario**  <br/> **Control + Mayús + rueda del mouse hacia delante**  | Zoom semántico en            | Para acercar el área de la vista de diagrama debajo el puntero del mouse.                                                                                                                                                                 |
| **Haga clic en Control + Mayús + Mouse** <br/> **Control + Mayús + rueda del mouse hacia atrás** | Alejar el Zoom semántico           | Aleja la vista del área de la vista de diagrama debajo el puntero del mouse. Volver a se centra en el diagrama de al alejar demasiado lejos del centro del diagrama actual.                                                                          |
| **Control + Mayús + '+'** <br/> **Control + rueda del mouse hacia delante**                        | Acercar                     | Acerca del centro de la vista de diagrama.                                                                                                                                                                                         |
| **Control + Mayús + '-'** <br/> **Control + rueda del mouse hacia atrás**                       | Alejar                    | Aleja desde el área de la vista de diagrama donde ha hecho clic. Volver a se centra en el diagrama de al alejar demasiado lejos del centro del diagrama actual.                                                                                            |
| **Control + Mayús + dibujar un rectángulo con el botón primario del mouse hacia abajo**                  | Área de zoom                   | Acercar centrado en el área que ha seleccionado. Mantenga presionadas las teclas de desplazamiento + Control, verá que el cursor cambia a una lupa, que le permite definir el área para acercar.                        |
| **Tecla de menú contextual +'m '**                                                              | Abra la ventana Detalles de asignación | Abre la ventana de detalles de la asignación para modificar las asignaciones para la entidad seleccionada                                                                                                                                                               |

## <a name="mapping-details-window"></a>Detalles de la asignación (ventana)

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| Acceso directo                  | Acción         | Notas                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Tabulación**                   | Cambie el contexto | Alterna entre el área de ventana principal y la barra de herramientas a la izquierda                                                                     |
| **Teclas de dirección**            | Navegación     | Subir y bajar por filas, o derecha e izquierda en las columnas en el área de ventana principal. Moverse entre los botones de la barra de herramientas de la izquierda. |
| **Entrar** <br/> **Espacio** | Seleccionar         | Selecciona un botón en la barra de herramientas de la izquierda.                                                                                          |
| **Alt + flecha abajo**      | Apertura de la lista      | Una lista desplegable si se selecciona una celda que tiene una lista desplegable.                                                                     |
| **Entrar**                 | Lista, seleccione    | Selecciona un elemento en una lista desplegable.                                                                                               |
| **Esc**                   | Cerrar lista     | Cierra una lista desplegable.                                                                                                              |

## <a name="visual-studio-navigation"></a>Navegación de Visual Studio

Entity Framework también proporciona una serie de acciones que puede tener métodos abreviados de teclado asignados (ningún acceso directo se asigna de forma predeterminada). Para crear estos accesos directos personalizados, haga clic en el menú Herramientas, a continuación, en Opciones.  En el entorno, elija el teclado.  Desplácese hacia abajo en la lista en la parte central hasta que pueda seleccionar el comando deseado, especifique el método abreviado en el cuadro de texto "Presione las teclas de método abreviado" y haga clic en asignar. Los métodos abreviados de posibles son los siguientes:

| Acceso directo                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
