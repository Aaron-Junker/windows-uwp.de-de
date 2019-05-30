---
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Asynchrone Programmierung
description: In diesem Thema wird beschrieben, asynchrone Programmierung in die universelle Windows-Plattform (UWP) und seine Darstellung im C#, Microsoft Visual Basic .NET, C++ und JavaScript.
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, asynchron
ms.localizationpriority: medium
ms.openlocfilehash: 26378473803b8963c0ca85eb414bae798f9607e4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371577"
---
# <a name="asynchronous-programming"></a>Asynchrone Programmierung
In diesem Thema wird beschrieben, asynchrone Programmierung in die universelle Windows-Plattform (UWP) und seine Darstellung im C#, Microsoft Visual Basic .NET, C++ und JavaScript.

Mit asynchroner Programmierung können Sie die Reaktionsfähigkeit Ihrer App bei der Ausführung von zeitintensiven Vorgängen aufrechterhalten. Zum Beispiel muss eine App, die Inhalte aus dem Internet herunterlädt, eventuell mehrere Sekunden warten, bis die Inhalte übermittelt sind. Wenn Sie die Inhalte mit einer synchronen Methode für den UI-Thread abrufen, ist die App so lange blockiert, bis der Methodenaufruf beendet ist. In diesem Zeitraum reagiert die App nicht auf Benutzerinteraktionen, und da sie nicht zu antworten scheint, ist der Benutzer möglicherweise verärgert. Die asynchrone Programmierung eignet sich hier sehr viel besser, denn die App wird weiterhin ausgeführt und reagiert auch auf die UI, während ein anderer Vorgang noch abgeschlossen wird.

Für Methoden, deren Aufruf möglicherweise recht lange dauert, ist die asynchrone Programmierung in der Universellen Windows-Plattform das Standardverfahren. JavaScript, C#, Visual Basic- und C++ jedes sprachunterstützung für asynchrone Methoden bereitstellen.

## <a name="asynchronous-programming-in-the-uwp"></a>Asynchrone Programmierung auf der UWP
Viele UWP-Features, wie z. B. die [ **MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) APIs und [ **"storagefile"** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) -APIs werden als asynchrone APIs verfügbar gemacht. Gemäß der Konvention enden die Namen der asynchronen APIs "Async", um anzugeben, dass es sich bei ihrer Ausführung gehört wahrscheinlich durchgeführt werden, nachdem die Steuerung an den Aufrufer zurückgegeben hat.

Bei der Verwendung asynchroner APIs in einer UWP-App (Universelle Windows-Plattform) führt der Code einheitlich nicht blockierende Aufrufe aus. Bei Implementierung dieser asynchronen Muster in Ihren API-Definitionen können Aufrufer den Code auf vorhersagbare Weise nachvollziehen und verwenden.

Nachfolgend finden Sie eine Reihe häufiger Aufgaben, für die der Aufruf asynchroner UWP-APIs erforderlich ist.

-   Anzeigen von Meldungsdialogfeldern

-   Verwenden des Dateisystems (Anzeigen einer Dateiauswahl)

-   Senden und Empfangen von Daten an das/aus dem Internet

-   Verwenden von Sockets, Streams, Konnektivität

-   Verwenden von Terminen, Kontakten, Kalendern

-   Verwenden von Dateitypen (wie etwa Öffnen von PDF-Dateien oder Decodieren von Bild- oder Medienformaten)

-   Interagieren mit einem Gerät oder Dienst

Mit asynchronen UWP-Mustern können Sie möglicherweise die explizite Verwaltung von Threads gänzlich vermeiden. Jede Programmiersprache unterstützt das asynchrone Muster für die UWP auf spezifische Art und Weise:

| Programmiersprache | Asynchrone Darstellung           |
|----------------------|---------------------------------------|
| C#                   | **async**-Schlüsselwort, **await**-Operator |
| Visual Basic         | **Async**-Schlagwort, **Await**-Operator |
| C++/WinRT            | Coroutine und **Co_await** Operator  |
| C++/CX               | **task**-Klasse, **.then**-Methode      |
| JavaScript           | zugesagtes Objekt, **then**-Funktion     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Asynchrone Muster auf der UWP mit C# und Visual Basic
Ein typischer Code-Abschnitt in C# oder Visual Basic wird synchron ausgeführt. Das heißt, dass die Ausführung einer Zeile beendet wird, bevor die nächste Zeile ausgeführt wird. Es gab bereits Microsoft .NET-Programmierungsmodelle für eine asynchrone Ausführung, aber der entsprechende Code überbetont die Funktionsweise des asynchronen Codes als solcher gegenüber der Aufgabe, die mit dem Code ausgeführt werden soll. Die Compiler für UWP, .NET Framework, C# und Visual Basic verfügen über erweiterte Features, die die asynchrone Funktionsweise aus dem Code abstrahieren. Für .NET und die UWP können Sie somit asynchronen Code schreiben, der sich auf die Aufgaben und nicht auf die Ausführung als solche konzentriert. Ihr asynchroner Code wird sich nicht großartig von synchronem Code unterscheiden. Weitere Informationen finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Asynchrone Muster in UWP mit C++ / WinRT
Mit C++/WinRT, Verwendung von Coroutinen, und die **Co_await** Operator. Weitere Informationen und Codebeispiele, finden Sie unter [asynchrone Programmierung in C++ / WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Asynchrone Muster in UWP mit C++ / CX
In C++/CX basiert die asynchrone Programmierung auf der [**task-Klasse**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) und deren [**then-Methode**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class?view=vs-2017). Die Syntax ist ähnlich aufgebaut wie eine JavaScript-Zusage. Die **task-Klasse** und die zugehörigen Typen erlauben es außerdem, den Threadkontext abzubrechen und zu verwalten. Weitere Informationen finden Sie unter [asynchrone Programmierung in C++ / CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

Die [ **erstellen\_Async-Funktion** ](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) bietet Unterstützung für die Erstellung von asynchronen APIs, die von JavaScript oder einer anderen Sprache, die von der UWP unterstützt genutzt werden können. Weitere Informationen finden Sie unter [Erstellen von asynchronen Vorgängen in C++ / CX](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Asynchrone Muster in UWP mit JavaScript
In JavaScript basiert die asynchrone Programmierung auf dem vorgeschlagenen [Common JS Promises/A](https://wiki.commonjs.org/wiki/Promises/A)-Standard. Dabei werden von asynchronen Methoden zugesagte Objekte zurückgegeben. Zusagen werden sowohl auf der UWP als auch in der Windows-Bibliothek für JavaScript verwendet.

Ein zugesagtes Objekt entspricht einem Wert, der in der Zukunft erfüllt wird. Auf der UWP werden zugesagte Objekte über Factoryfunktionen abgerufen. Die Namen solcher Funktionen enden üblicherweise auf „Async“.

Asynchrone Funktionen können häufig genauso einfach wie konventionelle Funktionen aufgerufen werden. Anders ist nur, dass Sie die [**then**](https://docs.microsoft.com/previous-versions/windows/apps/br229728(v=win.10))- oder [**done**](https://docs.microsoft.com/previous-versions/windows/apps/hh701079(v=win.10))-Methode verwenden, um die Handler für Ergebnisse oder Fehler zuzuweisen und den Vorgang zu starten.

## <a name="related-topics"></a>Verwandte Themen
* [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Asynchrone Programmierung mit Async und Await (C#- und Visual Basic)](https://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Reversi Feature Beispielszenarien: asynchronem Code](https://docs.microsoft.com/previous-versions/windows/apps/jj712233(v=win.10))
