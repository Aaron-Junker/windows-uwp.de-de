---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: Aufrechterhalten der Reaktionsfähigkeit des UI-Threads
description: Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b9a129e8b780e85df2c38c50ab712641d3849a34
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "71339854"
---
# <a name="keep-the-ui-thread-responsive"></a>Aufrechterhalten der Reaktionsfähigkeit des UI-Threads


Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp. Das bedeutet für jede App etwas anderes. Für einige Apps bedeutet dies z. B. Folgendes: realistischere physische Effekte bereitstellen, Daten vom Datenträger oder aus dem Internet schneller laden, komplexe Szenen schnell darstellen, schnell zwischen Seiten navigieren, Anweisungen im Nu finden oder schnelles Verarbeiten von Daten. Unabhängig von der Art der Berechnung möchten Benutzer, dass die App auf ihre Eingabe reagiert. Es stört sie, wenn die App scheinbar nicht reagiert, während sie "denkt".

Ihre App ist ereignisgesteuert, was bedeutet, dass Ihr Code in Reaktion auf ein Ereignis eine Funktion erfüllt und anschließend befindet sich die App im Leerlauf und wartet auf das nächste Ereignis. Plattformcode für die Benutzeroberfläche (Layout, Eingabe, Auslösen von Ereignissen usw.) und Ihr App-Code für die Benutzeroberfläche werden alle im gleichen UI-Thread ausgeführt. Nur eine Anweisung kann jeweils für diesen Thread ausgeführt werden, wenn Ihr App-Code also zu lange für das Verarbeiten eines Ereignisses benötigt, kann das Framework das Layout nicht ausführen oder neue Ereignisse auslösen, die Interaktionen des Benutzers darstellen. Dies bedeutet, dass die Reaktionsfähigkeit Ihrer App direkt davon abhängt, ob der UI-Thread zum Durchführen von Arbeit verfügbar ist.

Sie müssen den UI-Thread verwenden, um fast alle Änderungen am UI-Thread vorzunehmen, einschließlich der Erstellung von UI-Typen und des Zugriffs auf ihre Member. Sie können die UI nicht aus einem Hintergrundthread aktualisieren, können jedoch mit [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) eine Nachricht posten, damit der Code dort ausgeführt wird.

> **Hinweis:**    Der einzige Unterschied besteht darin, dass es einen separaten Renderthread gibt, der UI-Änderungen ohne Auswirkungen auf die Eingabeverarbeitung oder auf das Basislayout anwenden kann. Viele Animationen und Übergänge, die sich nicht auf das Layout auswirken, können z. B. auf diesem Renderthread ausgeführt werden.

## <a name="delay-element-instantiation"></a>Verzögern der Element-Instanziierung

Einige der langsamsten Phasen in einer App können der Start und das Wechseln zwischen Ansichten sein. Machen Sie sich nicht mehr Arbeit als nötig, um die Benutzeroberfläche anzuzeigen, die dem Benutzer zuerst angezeigt wird. Erstellen Sie beispielsweise nicht die Benutzeroberfläche für die schrittweise offengelegte Benutzeroberfläche und den Inhalt von Popups.

-   Verwende [x:Load attribute](../xaml-platform/x-load-attribute.md) oder [x:DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute), um Elemente verzögert zu instanziieren.
-   Fügen Sie programmgesteuerte Elemente nach Bedarf in der Struktur ein.

[**CoreDispatcher.RunIdleAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync)-Warteschlangen eignen sich für die Verarbeitung des UI-Threads, wenn er nicht aktiv ist.

## <a name="use-asynchronous-apis"></a>Verwenden von asynchronen APIs

Zur Verbesserung der Reaktionsfähigkeit der App stellt die Plattform asynchrone Versionen vieler APIs bereit. Eine asynchrone API stellt sicher, dass Ihr aktiver Ausführungsthread nie für längere Zeit blockiert wird. Wenn Sie ein API-Element aus dem UI-Thread heraus aufrufen, verwenden Sie die asynchrone Version, sofern diese verfügbar ist. Weitere Informationen zum Programmieren mit **async**-Strukturen finden Sie unter [Asynchrone Programmierung](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps) oder unter [Aufrufen asynchroner APIs in C# oder Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).

## <a name="offload-work-to-background-threads"></a>Auslagern der Arbeit an Hintergrundthreads

Schreiben Sie Ereignishandler, die schnell zurückgegeben werden. In Fällen, in denen umfassende Arbeiten vorgenommen werden müssen, sollten Sie sie in einem Hintergrundthread planen und zurückgeben.

Mit dem Operator **await** in C#, dem Operator **Await** in Visual Basic oder mit Delegaten in C++ können Sie Arbeit auch asynchron planen. Dies garantiert jedoch nicht, dass die geplante Arbeit in einem Hintergrundthread ausgeführt wird. Viele der UWP-APIs planen für Sie die Arbeit im Hintergrundthread, wenn Sie Ihren App-Code jedoch nur mit **await** oder einem Delegaten aufrufen, führen Sie diesen Delegaten oder diese Methode in einem UI-Thread aus. Wenn Sie Ihren App-Code in einem Hintergrundthread ausführen möchten, müssen Sie dies explizit angeben. In C# und Visual Basic kannst du dies erreichen, indem du Code an [**Task.Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run) übergibst.

Denken Sie daran, dass nur aus dem UI-Thread auf UI-Elemente zugegriffen werden kann. Verwenden Sie den UI-Thread für den Zugriff auf UI-Elemente vor dem Starten der Hintergrundverarbeitung und/oder der Verwendung von [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) oder [**CoreDispatcher.RunIdleAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) im Hintergrundthread.

Ein Beispiel für Arbeit, die in einem Hintergrundthread ausgeführt werden kann, ist die Berechnung der Computer-KI in einem Spiel. Die Ausführung des Codes zur Berechnung der nächsten Aktion des Computers nimmt viel Zeit in Anspruch.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

In diesem Beispiel wird der `NextMove_Click`-Handler bei **await** zurückgegeben, damit der UI-Thread reaktionsfähig bleibt. Die Ausführung wird jedoch in diesem Handler fortgesetzt, nachdem `ComputeNextMove` abgeschlossen ist (in einem Hintergrundthread ausgeführt). Der restliche Code im Handler aktualisiert die UI mit den Ergebnissen.

> **Hinweis:**    Es gibt auch eine [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool)- und eine [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer)-API für UWP, die für ähnliche Szenarien verwendet werden können. Weitere Informationen finden Sie unter [Threading und asynchrone Programmierung](https://docs.microsoft.com/windows/uwp/threading-async/index).

## <a name="related-topics"></a>Verwandte Themen

* [Benutzerdefinierte Benutzerinteraktionen](https://docs.microsoft.com/windows/uwp/design/layout/index)
