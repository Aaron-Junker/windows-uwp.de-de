---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: Bewährte Methoden zum Verwenden des Threadpools
description: In diesem Thema werden bewährte Methoden für die Verwendung des Threadpools beschrieben.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Thread, Threadpool
ms.localizationpriority: medium
ms.openlocfilehash: a498f685e7a810d19e2f1eb63ae112dd02587b84
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370694"
---
# <a name="best-practices-for-using-the-thread-pool"></a>Bewährte Methoden zum Verwenden des Threadpools

In diesem Thema werden bewährte Methoden für die Verwendung des Threadpools beschrieben.

## <a name="dos"></a>Empfohlene Vorgehensweisen


-   Verwenden Sie den Threadpool, um parallele Vorgänge in Ihrer App auszuführen.

-   Verwenden Sie Arbeitsaufgaben, um längere Aufgaben auszuführen, ohne den UI-Thread zu blockieren.

-   Erstellen Sie kurzlebige und unabhängige Arbeitsaufgaben. Arbeitsaufgaben werden asynchron ausgeführt und können in beliebiger Reihenfolge von der Warteschlange an den Pool gesendet werden.

-   Verwenden Sie [**Windows.UI.Core.CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher), um Updates an den UI-Thread zu verteilen.

-   Verwenden Sie [**ThreadPoolTimer.CreateTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) anstelle der **Sleep**-Funktion.

-   Verwenden Sie den Threadpool, anstatt ein eigenes Threadverwaltungssystem zu erstellen. Der Threadpool wird auf Betriebssystemebene ausgeführt, bietet erweiterte Funktionen und ist für die dynamische Skalierung je nach Geräteressourcen und Aktivitäten innerhalb des Prozesses und im gesamten System optimiert.

-   Stellen Sie in C++ sicher, dass Arbeitsaufgabendelegate das Agile-Threadingmodell verwenden (C++-Delegate sind standardmäßig agil).

-   Verwenden Sie vorab zugeordnete Arbeitsaufgaben, wenn Sie einen Fehler bei der Ressourcenzuweisung zum Zeitpunkt der Verwendung nicht tolerieren können.

## <a name="donts"></a>Nicht empfohlene Vorgehensweisen


-   Erstellen Sie keine regelmäßigen Timer mit einem *period*-Wert von &lt;1 Millisekunde (einschließlich 0). Andernfalls verhält sich die Arbeitsaufgabe wie ein einmaliger Timer.

-   Senden Sie keine regelmäßigen Arbeitsaufgaben, deren Ausführung länger dauert als die im *period*-Parameter festgelegte Dauer.

-   Senden Sie keine Benutzeroberflächenaktualisierungen (mit Ausnahme von Popups und Benachrichtigungen) von einer Arbeitsaufgabe, die von einer Hintergrundaufgabe übermittelt wird. Verwenden Sie stattdessen Status- und Abschlusshandler für Hintergrundaufgaben, z. B. [**IBackgroundTaskInstance.Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress).

-   Beachten Sie bei der Verwendung von Arbeitsaufgabenhandlern mit dem **async**-Schlüsselwort, dass die Threadpool-Arbeitsaufgabe möglicherweise vor der Ausführung des gesamten Codes im Ereignishandler auf den Status „Abgeschlossen“ gesetzt wird. Code, der innerhalb des Handlers auf ein **await**-Schlüsselwort folgt, kann ausgeführt werden, nachdem die Arbeitsaufgabe auf den Status „Abgeschlossen“ gesetzt wurde.

-   Führen Sie eine vorab zugeordnete Arbeitsaufgabe nicht mehrmals aus, ohne sie erneut zu initialisieren. [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md)

## <a name="related-topics"></a>Verwandte Themen


* [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md)
* [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)
