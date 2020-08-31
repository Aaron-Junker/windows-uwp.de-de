---
title: Richtlinien für Hintergrundaufgaben
description: Zeigen Sie ausführliche Anleitungen zum entwickeln und Ausführen von Prozess internen und Prozess internen Hintergrundaufgaben in Ihrer Anwendung an.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: bdcf398b448a3b0571b07063b9d4e70800259248
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094587"
---
# <a name="guidelines-for-background-tasks"></a>Richtlinien für Hintergrundaufgaben


Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt.

## <a name="background-task-guidance"></a>Ratschläge zu Hintergrundaufgaben

Beachten Sie beim Entwickeln Ihrer Hintergrundaufgabe und vor dem Veröffentlichen Ihrer App die folgenden Empfehlungen.

Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

**Hintergrundaufgaben innerhalb von Prozessen im Vergleich zu Hintergrundaufgaben außerhalb von Prozessen:** Mit Windows 10, Version 1607, wurden [Hintergrundaufgaben innerhalb von Prozessen](create-and-register-an-inproc-background-task.md) eingeführt. Dadurch kann Hintergrundcode im selben Prozess ausgeführt werden wie die Vordergrund-App. Beachten Sie bei der Entscheidung für Hintergrundaufgaben innerhalb oder außerhalb von Prozessen folgende Faktoren:

|Aspekt | Auswirkung |
|--------------|--------|
|Resilienz   | Wenn der Hintergrundprozess in einem anderen Prozess ausgeführt wird, hat ein Absturz im Hintergrundprozess keine Auswirkungen auf Ihre Vordergrund-Anwendung. Außerdem kann die Hintergrundaktivität beendet werden, sogar innerhalb Ihrer App, wenn Ausführungszeitlimits überschritten werden. Das Trennen der Hintergrundarbeit in eine Aufgabe separat von der Vordergrund-App ist möglicherweise die bessere Wahl, wenn Vordergrund- und Hintergrundprozess nicht miteinander kommunizieren müssen (ein wesentlicher Vorteil von Hintergrundaufgaben innerhalb von Prozessen ist nämlich der, dass keine Kommunikation zwischen Prozessen erforderlich ist). |
|Einfachheit    | Hintergrundaufgaben innerhalb von Prozessen erfordern keine prozessübergreifende Kommunikation und sind einfacher zu schreiben.  |
|Verfügbare Trigger | Hintergrundaufgaben innerhalb von Prozessen unterstützen nicht die folgenden Trigger: [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) und **IoTStartupTask** |
|VoIP | Hintergrundaufgaben innerhalb von Prozessen unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe in Ihrer Anwendung. |  

**Grenzwerte für die Anzahl der auslöserinstanzen:** Es gibt Grenzwerte für die Anzahl der Instanzen einiger Trigger, die eine APP registrieren kann. Eine APP kann nur "   [applicationtrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger)", " [mediaprocessingtrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) " und " [deviceusertrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) " einmal pro Instanz der APP registrieren. Wenn eine APP diesen Grenzwert überschreitet, löst die Registrierung eine Ausnahme aus.

**CPU-Kontingente:** Hintergrundaufgaben sind durch die Gesamtbetrachtungszeit eingeschränkt, die ihnen basierend auf dem Triggertyp zugeteilt wird. Die meisten Trigger sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt. Einige können jedoch bis zu 10 Minuten ausgeführt werden, um rechenintensive Aufgaben abschließen zu können. Hintergrundaufgaben sollten klein bleiben, um den Akku zu schonen und ein besseres Benutzererlebnis für die Apps im Vordergrund zu ermöglichen. Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

**Hintergrundaufgaben verwalten:** Ihre APP sollte eine Liste der registrierten Hintergrundaufgaben erhalten, sich für den Fortschritt und die Abschluss Handler registrieren und diese Ereignisse entsprechend behandeln. Ihre Hintergrundaufgabenklassen müssen Fortschritt, Abbruch und Abschluss berichten. Weitere Informationen finden Sie unter [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)und [Überwachen des Fortschritts und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md).

**Verwenden Sie " [backgroundtaskdeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)":** Wenn Ihre background-Task-Klasse asynchronen Code ausführt, stellen Sie sicher, dass Sie die-wartezahlen verwenden. Andernfalls kann die Hintergrundaufgabe vorzeitig beendet werden, wenn die [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) -Methode (oder die [onbackgroundaktivierte](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) Methode im Fall von in-Process-Hintergrundaufgaben) zurückgibt. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

Alternativ können Sie eine Verzögerung bewirken und asynchrone Methodenaufrufe mit **async/await** abschließen. Schließen Sie die Verzögerung nach den **await**-Methodenaufrufen.

**Aktualisieren des App-Manifests:** Deklarieren Sie für Hintergrundaufgaben außerhalb von Prozessen, die in einem separaten Prozess ausgeführt werden, im Anwendungsmanifest die einzelnen Hintergrundaufgaben gemeinsam mit dem verwendeten Triggertyp. Andernfalls kann Ihre App die Hintergrundaufgaben zur Laufzeit nicht registrieren.

Wenn Sie über mehrere Hintergrundaufgaben verfügen, sollten Sie berücksichtigen, ob Sie im selben Host Prozess ausgeführt werden oder in verschiedene Host Prozesse aufgeteilt werden sollen. Platzieren Sie Sie in separaten Host Prozessen, wenn Sie Bedenken haben, dass ein Fehler in einer Hintergrundaufgabe andere Hintergrundaufgaben verursachen könnte.  Verwenden Sie den **Ressourcengruppen** Eintrag im Manifest-Designer, um Hintergrundaufgaben in verschiedenen Host Prozessen zu gruppieren. 

Um die **Ressourcengruppe**festzulegen, öffnen Sie den Package. appxmanifest-Designer, wählen Sie **Deklarationen**aus, und fügen Sie eine **App Service** Deklaration hinzu:

![Ressourcengruppen Einstellung](images/resourcegroup.png)

Weitere Informationen zu den Ressourcengruppen Einstellungen finden Sie in der [Referenz zum Anwendungsschema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) .

Hintergrundaufgaben, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden, müssen sich im Anwendungsmanifest nicht selbst deklarieren. Weitere Informationen zum Deklarieren von Hintergrundaufgaben außerhalb von Prozessen, die in einem separaten Prozess im Manifest ausgeführt werden, finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).

**Vorbereiten von App-Updates:** Wenn Ihre App aktualisiert werden soll, erstellen und registrieren Sie eine **ServicingComplete**-Hintergrundaufgabe (siehe [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)), um die Registrierung von Hintergrundaufgaben für die vorherige Version der App aufzuheben und die Hintergrundaufgaben für die neue Version zu registrieren. Dies ist auch ein geeigneter Zeitpunkt, um App-Updates durchzuführen, die möglicherweise außerhalb des Kontexts der Ausführung im Vordergrund erforderlich sind.

**Anfordern der Ausführung von Hintergrundaufgaben:**

> **Wichtig**    Ab Windows 10 müssen sich apps nicht mehr auf dem Sperrbildschirm befinden als Voraussetzung für die Durchführung von Hintergrundaufgaben.

UWP (Universelle Windows-Plattform)-Apps können alle unterstützten Aufgabentypen ausführen, ohne auf dem Sperrbildschirm angeheftet zu sein. Apps müssen jedoch [**getaccessstate**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) aufrufen und überprüfen, dass die Ausführung der APP im Hintergrund nicht verweigert wird. Stellen Sie sicher, dass [**getaccessstatus**] nicht eine der abgelehnten [**backgroundaccessstatus**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) -Aufstände zurückgibt. Diese Methode gibt z. b. zurück ( https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) Wenn der Benutzer die Hintergrundaufgaben Berechtigungen für Ihre APP in den Geräteeinstellungen explizit verweigert hat.

Wenn die Ausführung Ihrer APP im Hintergrund verweigert wird, sollte Ihre APP [**requestaccessasync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) aufrufen und sicherstellen, dass die Antwort nicht verweigert wird, bevor Hintergrundaufgaben registriert werden.

Weitere Informationen zur Benutzer Auswahl im Zusammenhang mit Hintergrund Aktivitäten und Batterie Schoner finden Sie unter [Optimieren von Hintergrund Aktivitäten](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Prüfliste für Hintergrundaufgaben

*Gilt sowohl für Hintergrundaufgaben innerhalb als auch außerhalb von Prozessen*

-   Verknüpfen Sie Ihre Hintergrundaufgabe mit dem korrekten Trigger.
-   Fügen Sie Bedingungen hinzu, die für das erfolgreiche Ausführen Ihrer Hintergrundaufgabe sorgen.
-   Behandeln Sie Status, Abschluss und Abbruch von Hintergrundaufgaben.
-   Registrieren Sie Ihre Hintergrundaufgaben während des App-Starts erneut. Dadurch wird sichergestellt, dass sie beim ersten Start der App registriert sind. Zudem können Sie mit dieser Methode feststellen, ob der Benutzer die Ausführung von Hintergrundaufgaben in Ihrer App deaktiviert hat (falls die Registrierung fehlschlägt)
-   Prüfen Sie Ihre App auf Fehler bei der Registrierung von Hintergrundaufgaben. Führen Sie die Registrierung der Hintergrundaufgabe ggf. mit anderen Parameterwerten erneut durch.
-   Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt werden kann.

*Gilt nur für Hintergrundaufgaben außerhalb von Prozessen*

-   Erstellen Sie Ihre Hintergrundaufgabe in einer Windows-Runtime Komponente.
-   Zeigen Sie keine UI außer Popup-, Kachel- und Signalupdates von der Hintergrundaufgabe an.
-   Fordern Sie in der [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode Verzögerungen für jeden asynchronen Methodenaufruf an, und schließen Sie diese, wenn die Methode abgeschlossen wurde. Oder verwenden Sie eine Verzögerung mit " **Async"/"warten**".
-   Verwenden Sie den dauerhaften Speicher, um Daten für die Hintergrundaufgabe und die App freizugeben.
-   Deklarieren Sie jede Hintergrundaufgabe im Anwendungsmanifest zusammen mit dem Triggertyp, mit dem sie verwendet wird. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen.
-   Geben Sie kein ausführbares Element im Manifest an, es sei denn, Sie verwenden einen-Fehler, der im gleichen Kontext wie die app ausgeführt werden soll (z. b. [**controlchannel-**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)Fehler).

*Gilt nur für Hintergrundaufgaben innerhalb von Prozessen*

- Stellen Sie beim Abbrechen einer Aufgabe sicher, dass der `BackgroundActivated`-Ereignishandler vorhanden ist, bevor der Abbruch eintritt, oder der gesamte Prozess wird beendet.
-   Schreiben Sie Hintergrundaufgaben mit kurzer Laufzeit. Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.
-   Verlassen Sie sich bei Hintergrundaufgaben nicht auf Benutzerinteraktionen.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und registrieren Sie eine Prozess interne Hintergrundaufgabe](create-and-register-an-inproc-background-task.md).
* [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md)
* [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md)
* [Wiedergeben von Medien im Hintergrund](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)
* [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)
* [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)
* [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)
* [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)
* [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)
* [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

 

 
