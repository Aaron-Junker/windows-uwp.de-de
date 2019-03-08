---
title: Übergeben von Arrays an eine Komponente für Windows-Runtime
description: Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beides, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst für die Eingabe oder für die Ausgabe vorgesehen sind.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 60c2e2221cd174ffd75a45d6fe8e2f66744d67a0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657875"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Übergeben von Arrays an eine Komponente für Windows-Runtime




Parameter in der UWP (Universal Windows-Plattform) sind entweder für die Eingabe oder für die Ausgabe, nie für beides, vorgesehen. Das bedeutet, dass der Inhalt eines Arrays, der an eine Methode übergeben wird, wie auch das Array selbst für die Eingabe oder für die Ausgabe vorgesehen sind. Wenn der Inhalt des Arrays für die Eingabe vorgesehen ist, liest die Methode aus dem Array, aber schreibt nicht in das Array. Wenn der Inhalt des Arrays für die Ausgabe vorgesehen ist, schreibt die Methode in das Array, aber liest nicht in daraus. Dies stellt ein Problem für Arrayparameter dar, da Arrays im .NET Framework Verweistypen sind, und der Inhalt eines Arrays änderbar ist, auch wenn der Array-Verweis per Wert übergeben wird (**ByVal** in Visual Basic). Für das [Windows-Runtime-Metadatenexport-Tool (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) müssen Sie die beabsichtigte Verwendung des Arrays angeben, falls dies nicht eindeutig aus dem Kontext ersichtlich ist. Fügen Sie dem Parameter zu diesem Zweck das Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” hinzu. Die Array-Nutzung wird wie folgt bestimmt:

-   Für den Rückgabewert oder einen Ausgabeparameter (einen **ByRef**-Parameter mit dem [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx)-Attribut in Visual Basic) ist das Array immer nur für die Ausgabe vorgesehen. Geben Sie das Attribut „ReadOnlyArrayAttribute” nicht an. Das „WriteOnlyArrayAttribute”-Attribut ist für Ausgabeparameter zulässig, aber redundant.

    > **Vorsicht**  der Visual Basic-Compiler wird die Ausgabe nur Regeln nicht erzwungen. Sie sollten nie aus Ausgabeparametern lesen, sie könnten **Nothing** enthalten. Weisen Sie immer ein neues Array zu.
 
-   Parameter mit dem Modifizierer **ref** (**ByRef** in Visual Basic) sind nicht zulässig. Winmdexp.exe erzeugt einen Fehler.
-   Für einen Parameter, der als Wert übergeben wird, müssen Sie angeben, ob der Arrayinhalt für die Eingabe oder die Ausgabe vorgesehen ist, indem Sie entweder das Attribut [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx) oder das Attribut [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx) angeben. Die Angabe von beiden Attributen ist ein Fehler.

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

Wir empfehlen, dass Sie sofort eine Kopie des Eingabearrays anlegen und die Kopie bearbeiten. Dadurch wird sichergestellt, dass die Methode sich identisch verhält, unabhängig davon, ob die Komponente von .NET Framework-Code aufgerufen wird oder nicht.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Verwenden von Komponenten aus verwaltetem und nicht verwaltetem Code


Parameter mit dem Attribut „ReadOnlyArrayAttribute” oder „WriteOnlyArrayAttribute” verhalten sich anders, je nachdem, ob der Aufrufer in systemeigenem oder verwaltetem Code geschrieben wurde. Wenn der Aufrufer systemeigener Code (JavaScript oder Visual C++-Komponentenerweiterungen) ist, wird der Arrayinhalt wie folgt behandelt:

-   ReadOnlyArrayAttribute: Das Array wird kopiert, wenn der Aufruf der Grenze der anwendungsbinärschnittstelle (ABI) hinaus überschreitet. Elemente werden bei Bedarf konvertiert. Daher sind versehentliche Änderungen, die die Methode an einem nur für die Eingabe bestimmten Array vornimmt, nicht für den Aufrufer sichtbar.
-   WriteOnlyArrayAttribute: Die aufgerufene Methode keine Annahmen über den Inhalt des ursprünglichen Arrays nicht möglich. Zum Beispiel könnte das Array, das die Methode erhält, möglicherweise nicht initialisiert werden oder Standardwerte enthalten. Von der Methode wird erwartet, die Werte aller Elemente im Array festzulegen.

Ist der Aufrufer in verwaltetem Code geschrieben, ist das ursprüngliche Array für die aufgerufene Methode genauso wie in einem Methodenaufruf in .NET Framework verfügbar. Arrayinhalt ist in .NET Framework-Code änderbar, sodass alle Änderungen, die die Methode an dem Array vornimmt, für den Aufrufer sichtbar sind. Dies ist wichtig, da es Komponententests für eine Komponente für Windows-Runtime betrifft. Wenn die Tests in verwaltetem Code geschrieben sind, erscheint der Inhalt eines Arrays während der Testphase als änderbar.

## <a name="related-topics"></a>Verwandte Themen

* [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx)
* [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx)
* [Erstellen von Windows-Runtime-Komponenten in c# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
