---
title: Xbox Live-Ablaufverfolgung Analyzer
description: Erfahren Sie, wie der Xbox Live-Ablaufverfolgung Analyzer verwenden, um die Dienstaufrufe von den Titel zu überprüfen.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Webdienstaufrufe, testen, Analyzer zu verfolgen
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623465"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live-Ablaufverfolgung Analyzer

Die Xbox Live Services-API können jetzt Titel Entwickler alle Dienstaufrufe zu erfassen und Analysieren von Offline klicken Sie dann für alle Verstöße Muster aufzurufen. -Dienst Aufruf Ablaufverfolgung kann aktiviert werden, mithilfe von neuen Funktionen in das Befehlszeilentool Xbtrace oder durch die protokollaktivierung für erweiterte Szenarien. Aufruf direkt aus dem Titel Code nachverfolgen aktivieren, wird ebenfalls unterstützt. Das offline-Analyse-Tool, den Xbox Live-Ablaufverfolgung Analyzer (XBLTraceAnalyzer.exe) finden Sie im Rahmen des Xbox Live-Tools-Pakets von [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>Sammeln Sie Protokolle und analysieren Sie die Dienstaufrufe

Die folgenden Schritte sind erforderlich, um die Protokolle zu erfassen, die enthalten die Aufzeichnung Ihrer Dienst-Aufrufe und mit Xbox Live-Trace-Analyzer analysieren.

1.  Erstellen Sie Ihre Titel mit der Version des Xbox Live Services-API, die in den Juli 2015 enthalten ist oder neuere Version von der Xbox (xdk Developer Kit-Version).
2.  Ändern Sie Ihren Titel zum Aktivieren der Ablaufverfolgung, wie unten beschrieben.
3.  Stellen Sie den Titel an.
4.  Starten Sie den Titel aus, und stellen Sie mindestens ein Aufruf von Xbox Live Services zum Initialisieren der Xbox Live Services-API.
5.  Ablaufverfolgung starten, an dem Punkt im Titel, dass Sie analysieren möchten.
6.  Beenden Sie die Ablaufverfolgung.
7.  Führen Sie das Xbox Live-Ablaufverfolgung Analyzer-Tool auf Ihrem Entwicklungs-PC, und anzuzeigen Sie die Ausgabe.

## <a name="starting-and-stopping-tracing"></a>Starten und beenden-Ablaufverfolgung

Es gibt drei Möglichkeiten zum Starten und Beenden der Ablaufverfolgung:

1.  Sie können eine Reihe von Xbox Live Services-APIs direkt aus dem Titel des aufrufen.
2.  Sie können die *Xbtrace* -Befehlszeilentools.
3.  Können Sie protokollaktivierung über die *Application Management (xbapp.exe)* -Befehlszeilentools.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>Starten und beenden-Ablaufverfolgung direkt aus Ihrem Titel

Um direkt aus dem Titel des Ablaufverfolgung zu starten, gehen Sie folgendermaßen vor:

1.  In der `Microsoft::Xbox::Services::Experimental` -Namespace festgelegt die `EnableServiceCallTracking` Eigenschaft der `ServiceCallTrackerSettings` Klasse auf "true".
2.  Rufen Sie `StartServiceCallTracking()` , Dienstaufrufe Ablaufverfolgung starten.
3.  Rufen Sie `StopServiceCallTracking()` um Dienstaufrufe Ablaufverfolgung zu beenden.
4.  Nachdem die Ablaufverfolgung beendet wird, kopieren die entstandenen Ablaufverfolgungsdatei aus das temporäre Laufwerk der Entwickler in der Konsole wieder auf Ihren PC mit *Kopieren von Dateien (xbcp.exe)* oder *Xbox eine Nachbarschaft* um Mithilfe von Xbox Live-Trace-Analyzer analysieren.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>Starten und Beenden der Ablaufverfolgung mit dem Befehlszeilentool xbtrace

Die einfachste und einfache Methode zum Ablaufverfolgung zu starten ist die Verwendung des Xbtrace-Befehlszeilentools mit dem Xboxliveservices Trace-Typ. Bei Verwendung von XbTrace wird die Ergebnisdatei der Ablaufverfolgung für Sie zurück zu Ihrem PC kopiert.

Starten und Beenden von ablaufverfolgungen mit Xbtrace verwendet protokollaktivierung. Vor der Verwendung von Xbtrace zum Starten und Beenden der Ablaufverfolgung, müssen Sie protokollaktivierung initialisieren, durch Aufrufen der `RegisterForProtcolActivation` Methode für die `ServiceCallTrackerSettings` Klasse.

Das folgende Beispiel zeigt, wie Sie zum Starten und Beenden von einer Xbox Live Services-Ablaufverfolgungs mithilfe von XbTrace:

    xbtrace start xboxliveservices
    xbtrace stop


Denken Sie daran, dass der Titel ausgeführt werden muss und protokollaktivierung muss initialisiert werden, bevor Sie starten und Beenden der Ablaufverfolgung mit Xbtrace können. Nachdem die Ablaufverfolgung beendet wird, wird von Xbtrace kopiert die Datei auf Ihrem Entwicklungs-PC und platziert sie in ein Verzeichnis, dessen Name "Xbtrace" und einen Zeitstempel enthält. Der Name dieses Verzeichnisses kann überschrieben werden, mithilfe von \[Etlfile\] Option aus, um Xbtrace.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>Starten und beenden-Ablaufverfolgung mithilfe von protokollaktivierung
----------------------------------------------------------
Ablaufverfolgung kann auch mithilfe der Funktionen des Protokoll-Aktivierung von "XbApp starten" gesteuert werden. Sie müssen wissen, dass der Titel des Titleid zum Starten und Beenden der Ablaufverfolgung über protokollaktivierung. Sie finden die Title-Id in die Manifestdatei des Titels. Ablaufverfolgung wird über URIs mit "ServiceCallTracking"-Parameter gesteuert. Die folgenden Beispiele zeigen, wie Sie zum Starten und Beenden der Ablaufverfolgung für einen Titel, deren Titel Id 12345678 ist:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

Bei Verwendung von protokollaktivierung wird die Ergebnisdatei der Ablaufverfolgung auf das temporäre Laufwerk der Entwickler in der Konsole gespeichert. Sie müssen die Datei an Ihrem PC mit Xbcp oder der Xbox eine Nachbarschaft zu kopieren. Die Datei wird nicht automatisch wieder auf den PC kopiert, wie bei Xbtrace verwenden.

Protokollaktivierung können Sie zusätzliche Trace-Parameter, z. B. eine Ausführlichkeit festgelegt. Vier Ebenen der Ausführlichkeit werden unterstützt: quiet, Diagnose, ausführliche und minimal. Das folgende Beispiel zeigt, wie Sie den Ausführlichkeitsgrad festlegen:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>Analysieren der Ablaufverfolgungsdatei

Nachdem die Datei wieder auf Ihren PC kopiert wurde, können das Xbox Live-Ablaufverfolgung Analyzer-Tool auf GNDP Sie um des Titels von Xbox Live Services zu analysieren. Finden Sie in der Dokumentation mit dem Xbox Live-Ablaufverfolgung Analyzer-Tool Game Developer Network eine Beschreibung der Vorgehensweise rufen Sie das Tool und die Ausgabe zu interpretieren. Sie können auch XBLTraceAnalyzer.exe mit der Möglichkeit, über die Befehlszeile ausführen –? oder -h, um Hilfe zur Befehlszeile anzuzeigen.
