---
author: normesta
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: Asynchrone Programmierung
description: Dieses Thema beschreibt die asynchrone Programmierung in die universelle Windows-Plattform (UWP) und ihre Darstellung in c#, Microsoft Visual Basic, C++ und JavaScript.
ms.author: normesta
ms.date: 05/14/2018
ms.topic: article
keywords: Windows10, UWP, asynchron
ms.localizationpriority: medium
ms.openlocfilehash: 04d91fc7166812f53e8b2238b1a47c8aeb9c425f
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459128"
---
# <a name="asynchronous-programming"></a>Asynchrone Programmierung
Dieses Thema beschreibt die asynchrone Programmierung in die universelle Windows-Plattform (UWP) und ihre Darstellung in c#, Microsoft Visual Basic, C++ und JavaScript.

Mit asynchroner Programmierung können Sie die Reaktionsfähigkeit Ihrer App bei der Ausführung von zeitintensiven Vorgängen aufrechterhalten. Zum Beispiel muss eine App, die Inhalte aus dem Internet herunterlädt, eventuell mehrere Sekunden warten, bis die Inhalte übermittelt sind. Wenn Sie die Inhalte mit einer synchronen Methode für den UI-Thread abrufen, ist die App so lange blockiert, bis der Methodenaufruf beendet ist. In diesem Zeitraum reagiert die App nicht auf Benutzerinteraktionen, und da sie nicht zu antworten scheint, ist der Benutzer möglicherweise verärgert. Die asynchrone Programmierung eignet sich hier sehr viel besser, denn die App wird weiterhin ausgeführt und reagiert auch auf die UI, während ein anderer Vorgang noch abgeschlossen wird.

Für Methoden, deren Aufruf möglicherweise recht lange dauert, ist die asynchrone Programmierung in der Universellen Windows-Plattform das Standardverfahren. JavaScript, c#, Visual Basic und C++ jedes bieten sprachunterstützung für asynchrone Methoden.

## <a name="asynchronous-programming-in-the-uwp"></a>Asynchrone Programmierung auf der UWP
Viele UWP-Features, z. B. die [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124) -APIs und [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) -APIs, werden als asynchrone APIs verfügbar gemacht. Die Namen asynchroner APIs enden üblicherweise mit "Async", um anzugeben, dass ein Teil der Ausführung ist wahrscheinlich durchgeführt werden, nachdem die Steuerung an den Aufrufer zurückgegeben wurde.

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
| C#                   | **async**-Schlagwort, **await**-Operator |
| Visual Basic         | **Async**-Schlagwort, **Await**-Operator |
| C++/WinRT            | Coroutine und **Co_await** operator  |
| C++/CX               | **task**-Klasse, **.then**-Methode      |
| JavaScript           | zugesagtes Objekt, **then**-Funktion     |

## <a name="asynchronous-patterns-in-uwp-using-c-and-visual-basic"></a>Asynchrone Muster auf der UWP mit C# und Visual Basic
Ein typischer Code-Abschnitt in C# oder Visual Basic wird synchron ausgeführt. Das heißt, dass die Ausführung einer Zeile beendet wird, bevor die nächste Zeile ausgeführt wird. Es gab bereits Microsoft .NET-Programmierungsmodelle für eine asynchrone Ausführung, aber der entsprechende Code überbetont die Funktionsweise des asynchronen Codes als solcher gegenüber der Aufgabe, die mit dem Code ausgeführt werden soll. Die Compiler für UWP, .NET Framework, C# und Visual Basic verfügen über erweiterte Features, die die asynchrone Funktionsweise aus dem Code abstrahieren. Für .NET und die UWP können Sie somit asynchronen Code schreiben, der sich auf die Aufgaben und nicht auf die Ausführung als solche konzentriert. Ihr asynchroner Code wird sich nicht großartig von synchronem Code unterscheiden. Weitere Informationen finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md).

## <a name="asynchronous-patterns-in-uwp-with-cwinrt"></a>Asynchrone Muster in UWP mit C++ / WinRT
Mit C++ / WinRT Coroutinen und dem Operator **Co_await** Sie verwenden. Weitere Informationen und Codebeispiele finden Sie unter [asynchrone Programmierung in C++ / WinRT](../cpp-and-winrt-apis/concurrency.md).

## <a name="asynchronous-patterns-in-uwp-with-ccx"></a>Asynchrone Muster in UWP mit C++ / CX
In C++/CX basiert die asynchrone Programmierung auf der [**task-Klasse**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750113.aspx) und deren [**then-Methode**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750044.aspx). Die Syntax ist ähnlich aufgebaut wie eine JavaScript-Zusage. Die **task-Klasse** und die zugehörigen Typen erlauben es außerdem, den Threadkontext abzubrechen und zu verwalten. Weitere Informationen finden Sie unter [asynchrone Programmierung in C++ / CX](asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

Die [**create\_async function**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750102.aspx) unterstützt die Erstellung asynchroner APIs, die über JavaScript oder eine andere Sprache mit Unterstützung für UWP verwendet werden können. Weitere Informationen finden Sie unter [Erstellen asynchroner Vorgänge in C++ / CX](https://msdn.microsoft.com/library/windows/apps/xaml/hh750082.aspx).

## <a name="asynchronous-patterns-in-uwp-using-javascript"></a>Asynchrone Muster in UWP mit JavaScript
In JavaScript basiert die asynchrone Programmierung auf dem vorgeschlagenen [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A)-Standard. Dabei werden von asynchronen Methoden zugesagte Objekte zurückgegeben. Zusagen werden sowohl auf der UWP als auch in der Windows-Bibliothek für JavaScript verwendet.

Ein zugesagtes Objekt entspricht einem Wert, der in der Zukunft erfüllt wird. Auf der UWP werden zugesagte Objekte über Factoryfunktionen abgerufen. Die Namen solcher Funktionen enden üblicherweise auf „Async“.

Asynchrone Funktionen können häufig genauso einfach wie konventionelle Funktionen aufgerufen werden. Anders ist nur, dass Sie die [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728)- oder [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079)-Methode verwenden, um die Handler für Ergebnisse oder Fehler zuzuweisen und den Vorgang zu starten.

## <a name="related-topics"></a>Verwandte Themen
* [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [Asynchrone Programmierung mit Async und Await (C# und Visual Basic)](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Szenarien für Reversi-Beispielfeatures: Asynchroner Code](https://msdn.microsoft.com/library/windows/apps/xaml/jj712233.aspx#async)
