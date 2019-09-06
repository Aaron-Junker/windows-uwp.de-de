---
title: Richtlinien für Hintergrundaufgaben
description: Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Hintergrundaufgabe
ms.localizationpriority: medium
ms.openlocfilehash: 5f7e64dca67f0327b6366cbca072e83d4bf4be60
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393565"
---
# <a name="guidelines-for-background-tasks"></a>Richtlinien für Hintergrundaufgaben


Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt.

## <a name="background-task-guidance"></a>Ratschläge zu Hintergrundaufgaben

Beachten Sie beim Entwickeln Ihrer Hintergrundaufgabe und vor dem Veröffentlichen Ihrer App die folgenden Empfehlungen.

Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

**Prozess interne und Out-of-Process-Hintergrundaufgaben:** Windows 10, Version 1607, führte Prozess interne [Hintergrundaufgaben](create-and-register-an-inproc-background-task.md) ein, mit denen Sie Hintergrund Code im selben Prozess wie Ihre Vordergrund-app ausführen können. Beachten Sie bei der Entscheidung für Hintergrundaufgaben innerhalb oder außerhalb von Prozessen folgende Faktoren:

|Überlegung | Auswirkungen |
|--------------|--------|
|Flexibilität   | Wenn der Hintergrundprozess in einem anderen Prozess ausgeführt wird, hat ein Absturz im Hintergrundprozess keine Auswirkungen auf Ihre Vordergrund-Anwendung. Außerdem kann die Hintergrundaktivität beendet werden, sogar innerhalb Ihrer App, wenn Ausführungszeitlimits überschritten werden. Das Trennen der Hintergrundarbeit in eine Aufgabe separat von der Vordergrund-App ist möglicherweise die bessere Wahl, wenn Vordergrund- und Hintergrundprozess nicht miteinander kommunizieren müssen (ein wesentlicher Vorteil von Hintergrundaufgaben innerhalb von Prozessen ist nämlich der, dass keine Kommunikation zwischen Prozessen erforderlich ist). |
|Einfachheit    | Hintergrundaufgaben innerhalb von Prozessen erfordern keine prozessübergreifende Kommunikation und sind einfacher zu schreiben.  |
|Verfügbare Trigger | Prozess interne Hintergrundaufgaben unterstützen die folgenden Trigger nicht: [Deviceusetrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [deviceservicingtrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) und **iotstartuptask**. |
|VoIP | Hintergrundaufgaben innerhalb von Prozessen unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe in Ihrer Anwendung. |  

**Grenzwerte für die Anzahl der auslöserinstanzen:** Es gibt Grenzwerte für die Anzahl der Instanzen einiger Trigger, die eine APP registrieren kann. Eine App kann   [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) und [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) nur einmal pro Instanz der App registrieren. Wenn eine App diese Grenze überschreitet, gibt die Registrierung eine Ausnahme aus.

**CPU-Kontingente:** Hintergrundaufgaben werden durch die Menge an Zeit, die auf dem auslösertyp basieren, auf die Zeitspanne beschränkt. Die meisten Trigger sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt. Einige können jedoch bis zu 10 Minuten ausgeführt werden, um rechenintensive Aufgaben abschließen zu können. Hintergrundaufgaben sollten klein bleiben, um den Akku zu schonen und ein besseres Benutzererlebnis für die Apps im Vordergrund zu ermöglichen. Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

**Hintergrundaufgaben verwalten:** Ihre APP sollte eine Liste der registrierten Hintergrundaufgaben erhalten, sich für den Fortschritt und die Abschluss Handler registrieren und diese Ereignisse entsprechend behandeln. Ihre Hintergrundaufgabenklassen müssen Fortschritt, Abbruch und Abschluss berichten. Weitere Informationen finden Sie unter [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md) und [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md).

**Verwenden Sie " [backgroundtaskdeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral)":** Wenn Ihre background-Task-Klasse asynchronen Code ausführt, stellen Sie sicher, dass Sie die-wartezahlen verwenden. Andernfalls kann die Hintergrundaufgabe vorzeitig beendet werden, wenn die [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) -Methode (oder die [onbackgroundaktivierte](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) Methode im Fall von in-Process-Hintergrundaufgaben) zurückgibt. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

Alternativ können Sie eine Verzögerung bewirken und asynchrone Methodenaufrufe mit **async/await** abschließen. Schließen Sie die Verzögerung nach den **await**-Methodenaufrufen.

**Aktualisieren Sie das App-Manifest:**  Für Hintergrundaufgaben, die außerhalb des Prozesses ausgeführt werden, deklarieren Sie jede Hintergrundaufgabe im Anwendungs Manifest zusammen mit dem Typ der Trigger, mit denen Sie verwendet wird. Andernfalls kann Ihre App die Hintergrundaufgaben zur Laufzeit nicht registrieren.

Bei mehreren Hintergrundaufgaben müssen Sie entscheiden, ob sie im selben Hostprozess oder in verschiedenen Hostprozessen ausgeführt werden sollten. Verwenden Sie separate Hostprozesse, wenn Sie befürchten, dass ein Fehler in einer Hintergrundaufgabe andere Hintergrundaufgaben beenden könnte.  Verwenden Sie den Eintrag **Ressourcengruppe** Manifest-Designer, um Hintergrundaufgaben in unterschiedliche Host-Prozesse einzuordnen. 

Zum Festlegen der **Ressourcengruppe** öffnen Sie den Package.appxmanifest-Designer, wählen **Deklarationen** und fügen eine **App Service**-Deklaration hinzu:

![Einstellungen für Ressourcengruppen](images/resourcegroup.png)

Weitere Informationen zu den Ressourcengruppen Einstellungen finden Sie in der [Referenz zum Anwendungsschema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) .

Hintergrundaufgaben, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden, müssen sich im Anwendungsmanifest nicht selbst deklarieren. Weitere Informationen zum Deklarieren von Hintergrundaufgaben außerhalb von Prozessen, die in einem separaten Prozess im Manifest ausgeführt werden, finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).

**Vorbereiten der APP-Updates:** Wenn Ihre APP aktualisiert wird, erstellen und registrieren Sie eine **servicingcomplete** -Hintergrundaufgabe (siehe [systemtriggertype](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)), um die Registrierung der Hintergrundaufgaben für die vorherige Version der APP aufzuheben, und registrieren Sie die Hintergrundaufgaben für die neue Version. Dies ist auch ein geeigneter Zeitpunkt, um App-Updates durchzuführen, die möglicherweise außerhalb des Kontexts der Ausführung im Vordergrund erforderlich sind.

**Anforderung zum Ausführen von Hintergrundaufgaben:**

> **Wichtig ab Windows**10 müssen apps nicht mehr auf dem Sperrbildschirm als Voraussetzung für die Durchführung von Hintergrundaufgaben angezeigt werden.  

UWP (Universelle Windows-Plattform)-Apps können alle unterstützten Aufgabentypen ausführen, ohne auf dem Sperrbildschirm angeheftet zu sein. Apps müssen jedoch vor dem Registrieren einer Hintergrundaufgabe [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) aufrufen. Diese Methode gibt [**BackgroundAccessStatus.DeniedByUser**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) zurück, wenn der Benutzer Berechtigungen für Hintergrundaufgaben für Ihre App in den Geräteeinstellungen explizit verweigert hat. Weitere Informationen über die Auswahl des Benutzers über Hintergrundaktivitäten und den Stromsparmodus finden Sie unter [Optimieren von Hintergrundaktivitäten ](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Prüfliste für Hintergrundaufgaben

*Gilt für Prozess interne und prozessübergreifende Hintergrundaufgaben*

-   Verknüpfen Sie Ihre Hintergrundaufgabe mit dem korrekten Trigger.
-   Fügen Sie Bedingungen hinzu, die für das erfolgreiche Ausführen Ihrer Hintergrundaufgabe sorgen.
-   Behandeln Sie Status, Abschluss und Abbruch von Hintergrundaufgaben.
-   Registrieren Sie Ihre Hintergrundaufgaben während des App-Starts erneut. Dadurch wird sichergestellt, dass sie beim ersten Start der App registriert sind. Zudem können Sie mit dieser Methode feststellen, ob der Benutzer die Ausführung von Hintergrundaufgaben in Ihrer App deaktiviert hat (falls die Registrierung fehlschlägt)
-   Prüfen Sie Ihre App auf Fehler bei der Registrierung von Hintergrundaufgaben. Führen Sie die Registrierung der Hintergrundaufgabe ggf. mit anderen Parameterwerten erneut durch.
-   Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt werden kann.

*Gilt nur für Out-of-Process-Hintergrundaufgaben*

-   Erstellen Sie Ihre Hintergrundaufgabe in einer Windows-Runtime Komponente.
-   Zeigen Sie keine UI außer Popup-, Kachel- und Signalupdates von der Hintergrundaufgabe an.
-   Fordern Sie in der [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)-Methode Verzögerungen für jeden asynchronen Methodenaufruf an, und schließen Sie diese, wenn die Methode abgeschlossen wurde. Sie können auch eine Verzögerung mit **async/await** verwenden.
-   Verwenden Sie den dauerhaften Speicher, um Daten für die Hintergrundaufgabe und die App freizugeben.
-   Deklarieren Sie jede Hintergrundaufgabe im Anwendungsmanifest zusammen mit dem Triggertyp, mit dem sie verwendet wird. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen.
-   Geben Sie im Manifest nur dann ein Executable-Element an, wenn Sie einen Trigger nutzen, der im selben Kontext wie die App ausgeführt werden soll (z. B. [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)).

*Gilt nur für Prozess interne Hintergrundaufgaben*

- Stellen Sie beim Abbrechen einer Aufgabe sicher, dass der `BackgroundActivated`-Ereignishandler vorhanden ist, bevor der Abbruch eintritt, oder der gesamte Prozess wird beendet.
-   Schreiben Sie Hintergrundaufgaben mit kurzer Laufzeit. Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.
-   Verlassen Sie sich bei Hintergrundaufgaben nicht auf Benutzerinteraktionen.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md)
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
* [Gewusst wie: Starten von Suspend-, Resume-und Background-Ereignissen in UWP-Apps (beim Debuggen)](https://go.microsoft.com/fwlink/p/?linkid=254345)

 

 
