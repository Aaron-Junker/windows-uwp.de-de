---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: Bewährte Methoden zum Verwenden des Threadpools
description: Informieren Sie sich über bewährte Methoden für die Arbeit mit dem Thread Pool, um asynchron in parallelen Threads arbeiten zu können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Thread, Thread Pool
ms.localizationpriority: medium
ms.openlocfilehash: 692dcae11c80e817d1d400e66e5f90b65df8b249
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161714"
---
# <a name="best-practices-for-using-the-thread-pool"></a>Bewährte Methoden zum Verwenden des Threadpools

In diesem Thema werden bewährte Methoden für die Verwendung des Threadpools beschrieben.

## <a name="dos"></a>Empfohlene Vorgehensweisen


-   Verwenden Sie den Threadpool, um parallele Vorgänge in Ihrer App auszuführen.

-   Verwenden Sie Arbeitselemente, um längere Aufgaben auszuführen, ohne den UI-Thread zu blockieren.

-   Erstellen Sie kurzlebige und unabhängige Arbeitselemente. Arbeitsaufgaben werden asynchron ausgeführt und können in beliebiger Reihenfolge von der Warteschlange an den Pool gesendet werden.

-   Verwenden Sie [**Windows.UI.Core.CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher), um Updates an den UI-Thread zu verteilen.

-   Verwenden Sie [**ThreadPoolTimer.CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) anstelle der **Sleep**-Funktion.

-   Verwenden Sie den Threadpool, anstatt ein eigenes Threadverwaltungssystem zu erstellen. Der Threadpool wird auf Betriebssystemebene ausgeführt, bietet erweiterte Funktionen und ist für die dynamische Skalierung je nach Geräteressourcen und Aktivitäten innerhalb des Prozesses und im gesamten System optimiert.

-   Stellen Sie in C++ sicher, dass Arbeitselementedelegate das Agile-Threadingmodell verwenden (C++-Delegate sind standardmäßig agil).

-   Verwenden Sie vorab zugeordnete Arbeitselemente, wenn Sie einen Fehler bei der Ressourcenzuweisung zum Zeitpunkt der Verwendung nicht tolerieren können.

## <a name="donts"></a>Vergabe


-   Erstellen Sie keine regelmäßigen Timer mit einem *period*-Wert von &lt;1 Millisekunde (einschließlich 0). Andernfalls verhält sich das Arbeitselement wie ein einmaliger Timer.

-   Senden Sie keine regelmäßigen Arbeitsaufgaben, deren Ausführung länger dauert als die im *period*-Parameter festgelegte Dauer.

-   Senden Sie keine Benutzeroberflächenaktualisierungen (mit Ausnahme von Popups und Benachrichtigungen) von eines Arbeitselement, die von einer Hintergrundaufgabe übermittelt wird. Verwenden Sie stattdessen Status- und Abschlusshandler für Hintergrundaufgaben, z. B. [**IBackgroundTaskInstance.Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress).

-   Beachten Sie bei der Verwendung von Arbeitsaufgabenhandlern mit dem **async**-Schlüsselwort, dass die Threadpool-Arbeitsaufgabe möglicherweise vor der Ausführung des gesamten Codes im Ereignishandler auf den Status „Abgeschlossen“ gesetzt wird. Code, der innerhalb des Handlers auf ein **await**-Schlüsselwort folgt, kann ausgeführt werden, nachdem die Arbeitsaufgabe auf den Status „Abgeschlossen“ gesetzt wurde.

-   Führen Sie ein vorab zugeordneten Arbeitselement nicht mehrmals aus, ohne sie erneut zu initialisieren. [Erstellen ein regelmäßiges Arbeitselement](create-a-periodic-work-item.md)

## <a name="related-topics"></a>Zugehörige Themen


* [Erstellen ein regelmäßiges Arbeitselement](create-a-periodic-work-item.md)
* [Senden ein Arbeitselement an den Threadpool](submit-a-work-item-to-the-thread-pool.md)
* [Timergesteuertes Übermitteln einer Arbeitsaufgabe](use-a-timer-to-submit-a-work-item.md)