---
title: Übergeben von Arrays an eine Komponente für Windows-Runtime
description: Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beides, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst für die Eingabe oder für die Ausgabe vorgesehen sind.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f38538d1a77bdeb6a497eda5802f712f23002b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155184"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Übergeben von Arrays an eine Komponente für Windows-Runtime




Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beides, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst für die Eingabe oder für die Ausgabe vorgesehen sind. Wenn der Inhalt des Arrays für die Eingabe vorgesehen ist, liest die Methode aus dem Array, aber schreibt nicht in das Array. Wenn der Inhalt des Arrays für die Ausgabe vorgesehen ist, schreibt die Methode in das Array, aber liest nicht in daraus. Dies stellt ein Problem für Array Parameter dar, da Arrays in .net Verweis Typen sind und der Inhalt eines Arrays änderbar ist, auch wenn der Array Verweis als Wert (**ByVal** in Visual Basic) übermittelt wird. Für das [Windows-Runtime-Metadatenexport-Tool (Winmdexp.exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) müssen Sie die beabsichtigte Verwendung des Arrays angeben, falls dies nicht eindeutig aus dem Kontext ersichtlich ist. Fügen Sie dem Parameter zu diesem Zweck das Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” hinzu. Die Array-Nutzung wird wie folgt bestimmt:

-   Für den Rückgabewert oder einen Ausgabeparameter (einen **ByRef**-Parameter mit dem [OutAttribute](/dotnet/api/system.runtime.interopservices.outattribute)-Attribut in Visual Basic) ist das Array immer nur für die Ausgabe vorgesehen. Geben Sie das Attribut „ReadOnlyArrayAttribute” nicht an. Das „WriteOnlyArrayAttribute”-Attribut ist für Ausgabeparameter zulässig, aber redundant.

    > **Vorsicht**    Der Visual Basic-Compiler erzwingt keine reinen Ausgabe Regeln. Sie sollten nie aus Ausgabeparametern lesen, sie könnten **Nothing** enthalten. Weisen Sie immer ein neues Array zu.
 
-   Parameter mit dem Modifizierer **ref** (**ByRef** in Visual Basic) sind nicht zulässig. Winmdexp.exe erzeugt einen Fehler.
-   Für einen Parameter, der als Wert übergeben wird, müssen Sie angeben, ob der Arrayinhalt für die Eingabe oder die Ausgabe vorgesehen ist, indem Sie entweder das Attribut [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) oder das Attribut [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute) angeben. Die Angabe von beiden Attributen ist ein Fehler.

Wenn eine Methode ein Array für die Eingabe akzeptieren soll, ändern Sie den Arrayinhalt, und geben Sie das Array an den Aufrufer zurück; verwenden Sie einen schreibgeschützten Parameter für die Eingabe und einen lesegeschützten Parameter (oder den Rückgabewert) für die Ausgabe. Der folgende Code zeigt eine Möglichkeit, dieses Muster zu implementieren:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

Wir empfehlen, dass Sie sofort eine Kopie des Eingabearrays anlegen und die Kopie bearbeiten. Dadurch wird sichergestellt, dass die-Methode gleich verhält, unabhängig davon, ob die-Komponente von .NET-Code aufgerufen wird.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Verwenden von Komponenten aus verwaltetem und nicht verwaltetem Code


Parameter mit dem Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” verhalten sich anders, je nachdem, ob der Aufrufer in systemeigenem oder verwaltetem Code geschrieben wurde. Wenn der Aufrufer systemeigener Code (JavaScript oder Visual C++-Komponentenerweiterungen) ist, wird der Arrayinhalt wie folgt behandelt:

-   ReadOnlyArrayAttribute: Das Array wird kopiert, wenn der Aufruf die Grenze der binären Anwendungsschnittstelle (ABI) überschreitet. Elemente werden bei Bedarf konvertiert. Von der Methode fälschlicherweise an einem nur zur Eingabe bestimmten Array vorgenommene Änderungen sind daher für den Aufrufer nicht sichtbar.
-   WriteOnlyArrayAttribute: Die aufgerufene Methode kann keine Annahmen über den Inhalt des ursprünglichen Arrays treffen. Beispielsweise ist das Array, das die Methode empfängt, möglicherweise nicht oder mit Standardwerten initialisiert. Von der Methode wird das Festlegen der Werte aller Elemente im Array erwartet.

Wenn der Aufrufer verwalteter Code ist, steht das ursprüngliche Array der aufgerufenen Methode zur Verfügung, wie es bei jedem Methodenaufruf in .net der Fall wäre. Array Inhalte sind im .NET-Code änderbar, sodass alle Änderungen, die die Methode am Array vornimmt, für den Aufrufer sichtbar sind. Dies ist wichtig, da es Komponententests für eine Komponente für Windows-Runtime betrifft. Wenn die Tests in verwaltetem Code geschrieben werden, scheint der Inhalt eines Arrays während des Testens veränderlich zu sein.

## <a name="related-topics"></a>Zugehörige Themen

* [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)