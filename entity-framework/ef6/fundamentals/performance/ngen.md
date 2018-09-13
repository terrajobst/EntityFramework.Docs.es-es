---
title: Mejorar el rendimiento de inicio con NGen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 2e9a93c7741046da25b890fed084a6b280bf5643
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489965"
---
# <a name="improving-startup-performance-with-ngen"></a>Mejorar el rendimiento de inicio con NGen
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

.NET Framework admite la generación de imágenes nativas para las aplicaciones administradas y las bibliotecas como una manera de ayudar a las aplicaciones de inicio más rápido y también en algunos casos usan menos memoria. Las imágenes nativas se crean mediante la traducción de los ensamblados de código administrado en archivos que contienen instrucciones máquina nativas antes de ejecuta la aplicación, lo cual elimina la necesidad de generar las instrucciones nativas en el compilador JIT (Just-In-Time) de .NET tiempo de ejecución de la aplicación.  

Antes de la versión 6, bibliotecas del runtime EF core formaban parte de .NET Framework y las imágenes nativas generadas automáticamente para ellos. Comenzando con la versión 6 se ha combinado el tiempo de ejecución EF todo en el paquete EntityFramework NuGet. Las imágenes nativas hasta ahora se generaron mediante la herramienta de línea de comandos de NGen.exe para obtener resultados similares.  

Observaciones empíricas muestran que las imágenes nativas de los ensamblados en tiempo de ejecución EF pueden cortar entre 1 y 3 segundos de tiempo de inicio de la aplicación.  

## <a name="how-to-use-ngenexe"></a>Uso de NGen.exe  

La función de la herramienta NGen.exe más básica consiste en "install" (es decir, para crear y guardar en disco) las imágenes nativas para un ensamblado y todas sus dependencias directas. Le mostramos cómo hacerlo:  

1. Abra una ventana del símbolo del sistema como administrador  
2. Cambie el directorio de trabajo actual a la ubicación de los ensamblados que desea generar imágenes nativas para:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. Dependiendo de su sistema operativo y configuración de la aplicación deba genera imágenes nativas para la arquitectura de 32 bits, la arquitectura de 64 bits o ambos.  

    Para 32 bits que ejecute:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    Para ejecutar de 64 bits:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Generación de imágenes nativas para la arquitectura incorrecta es un error muy común. En caso de duda, simplemente puede generar imágenes nativas para todas las arquitecturas que se aplican al sistema operativo instalado en la máquina.  

NGen.exe también admite otras funciones como la desinstalación y mostrar las imágenes nativas instaladas, puesta en cola la generación de varias imágenes, etcetera. Para obtener más detalles de uso de leer el [NGen.exe documentación](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>Cuándo utilizar NGen.exe  

Cuando se trata de decidir qué ensamblados para generar imágenes nativas en una aplicación basada en EF versión 6 o posterior, debe considerar las siguientes opciones:  

- **El ensamblado principal de tiempo de ejecución EF, EntityFramework.dll**: una aplicación típica de EF en función ejecuta una cantidad significativa de código de este ensamblado en el inicio o en el primer acceso a la base de datos. Por lo tanto, crear imágenes nativas de este ensamblado generará las mayores ganancias en el rendimiento de inicio.  
- **La aplicación usado cualquier ensamblado del proveedor EF**: tiempo de inicio también puede beneficiarse ligeramente para generar imágenes nativas de estos. Por ejemplo, si la aplicación utiliza el proveedor EF para SQL Server que desee generar una imagen nativa para EntityFramework.SqlServer.dll.  
- **Ensamblados de la aplicación y otras dependencias**: el [NGen.exe documentación](https://msdn.microsoft.com/library/6t9t5wcf.aspx) cubre criterios generales para elegir qué ensamblados para generar imágenes nativas para y el impacto de las imágenes nativas en la seguridad Opciones avanzadas como "enlace fuerte," escenarios como el uso de imágenes nativas de depuración y generación de perfiles de escenarios, etcetera.  

> [!TIP]
> Asegúrese de medir el impacto del uso de las imágenes nativas en el rendimiento de inicio y el rendimiento general de la aplicación con cuidado y compárelos con los requisitos reales. Mientras que las imágenes nativas con carácter general le ayudará a mejorar el inicio, el rendimiento y reducir el uso de memoria en algunos casos, no todos los escenarios se beneficiarán igualmente. Por ejemplo, en la ejecución de estado estable (es decir, una vez que se han invocado todos los métodos de la aplicación está usando al menos una vez) código generado por el compilador JIT puede en realidad producen un rendimiento ligeramente mejor que las imágenes nativas.  

## <a name="using-ngenexe-in-a-development-machine"></a>Uso de NGen.exe en una máquina de desarrollo  

Durante el desarrollo, el compilador JIT de .NET compilador ofrecerá el mejor equilibrio general para el código que cambia con frecuencia. Generación de imágenes nativas para dependencias compiladas como los ensamblados en tiempo de ejecución EF puede ayudar a acelerar el desarrollo y pruebas si recortar unos pocos segundos al principio de cada ejecución.  

Un buen lugar para encontrar los ensamblados en tiempo de ejecución EF es la ubicación del paquete NuGet para la solución. Por ejemplo, para una aplicación mediante EF 6.0.2 con SQL Server y destinadas a .NET 4.5 o superior puede escribir lo siguiente en una ventana del símbolo del sistema (no olvide abrirlo como administrador):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Esto aprovecha el hecho de que la instalación de las imágenes nativas para el proveedor EF para SQL Server también de forma predeterminada instalará las imágenes nativas para el ensamblado en tiempo de ejecución EF principal. Esto funciona porque NGen.exe puede detectar que EntityFramework.dll es una dependencia directa del ensamblado EntityFramework.SqlServer.dll ubicado en el mismo directorio.  

## <a name="creating-native-images-during-setup"></a>Crear imágenes nativas durante la instalación  

El WiX Toolkit admite puesta en cola la generación de imágenes nativas para los ensamblados administrados durante la instalación, como se explica en este [guía](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Otra alternativa consiste en crear una tarea de instalación personalizada que ejecute el comando de NGen.exe.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Comprobando que las imágenes nativas se utilizan para EF  

Puede comprobar que una aplicación específica utiliza un ensamblado nativo mediante la búsqueda de los ensamblados cargados que tienen la extensión ". ni.dll"o". ni.exe". Por ejemplo, una imagen nativa para el ensamblado de tiempo de ejecución principal de EF se llamará EntityFramework.ni.dll. Una manera fácil de inspeccionar los ensamblados .NET cargados de un proceso es usar [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Otras cosas a tener en cuenta  

**Creación de una imagen nativa de un ensamblado no debe confundirse con el registro del ensamblado en el [GAC (caché Global de ensamblados)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe permite la creación de imágenes de los ensamblados que no están en la GAC y, de hecho, varias aplicaciones que utilizan una versión concreta de EF pueden compartir la misma imagen nativa. Aunque Windows 8 puede crear automáticamente las imágenes nativas para los ensamblados ubicados en la GAC, el tiempo de ejecución EF está optimizado para implementarse junto con la aplicación y no se recomienda registrarlo en la GAC esto tiene un impacto negativo en la resolución de ensamblado y las aplicaciones, entre otros aspectos de mantenimiento.  
