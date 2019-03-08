---
title: Richtlinien für Hintergrundaufgaben
description: Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgaben
ms.localizationpriority: medium
ms.openlocfilehash: af8e45e13eb89185e346c3c8e8cd5303da399471
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658735"
---
# <a name="guidelines-for-background-tasks"></a>Richtlinien für Hintergrundaufgaben


Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt.

## <a name="background-task-guidance"></a>Ratschläge zu Hintergrundaufgaben

Beachten Sie beim Entwickeln Ihrer Hintergrundaufgabe und vor dem Veröffentlichen Ihrer App die folgenden Empfehlungen.

Wenn Sie eine Hintergrundaufgabe zur Medienwiedergabe im Hintergrund verwenden, finden Sie unter [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio) Informationen zu Verbesserungen in Windows 10, Version 1607, die dies erleichtern.

**In-Process und Out-of-Process-Hintergrundaufgaben:** Windows 10, Version 1607 eingeführt [in-Process-Hintergrundaufgaben](create-and-register-an-inproc-background-task.md) , sodass Sie Background-Code in demselben Prozess wie die Vordergrund-app ausführen. Beachten Sie bei der Entscheidung für Hintergrundaufgaben innerhalb oder außerhalb von Prozessen folgende Faktoren:

|Überlegung | Auswirkungen |
|--------------|--------|
|Flexibilität   | Wenn der Hintergrundprozess in einem anderen Prozess ausgeführt wird, hat ein Absturz im Hintergrundprozess keine Auswirkungen auf Ihre Vordergrund-Anwendung. Außerdem kann die Hintergrundaktivität beendet werden, sogar innerhalb Ihrer App, wenn Ausführungszeitlimits überschritten werden. Das Trennen der Hintergrundarbeit in eine Aufgabe separat von der Vordergrund-App ist möglicherweise die bessere Wahl, wenn Vordergrund- und Hintergrundprozess nicht miteinander kommunizieren müssen (ein wesentlicher Vorteil von Hintergrundaufgaben innerhalb von Prozessen ist nämlich der, dass keine Kommunikation zwischen Prozessen erforderlich ist). |
|Einfachheit    | Hintergrundaufgaben innerhalb von Prozessen erfordern keine prozessübergreifende Kommunikation und sind einfacher zu schreiben.  |
|Verfügbare Trigger | In-Process-Hintergrundaufgaben unterstützen nicht die folgenden Trigger: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) und **IoTStartupTask**. |
|VoIP | Hintergrundaufgaben innerhalb von Prozessen unterstützen nicht die Aktivierung einer VoIP-Hintergrundaufgabe in Ihrer Anwendung. |  

**Beschränkungen hinsichtlich der Anzahl von Instanzen der Trigger:** Es gibt Grenzen für einige Trigger wie viele Instanzen eine app registrieren kann. Eine App kann   [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) und [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396) nur einmal pro Instanz der App registrieren. Wenn eine App diese Grenze überschreitet, gibt die Registrierung eine Ausnahme aus.

**CPU-Kontingente:** Aufgaben im Hintergrund werden durch die Menge der Nutzung der wanduhrzeit beschränkt, die sie für Trigger vom Typ basiert, erhalten. Die meisten Trigger sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt. Einige können jedoch bis zu 10 Minuten ausgeführt werden, um rechenintensive Aufgaben abschließen zu können. Hintergrundaufgaben sollten klein bleiben, um den Akku zu schonen und ein besseres Benutzererlebnis für die Apps im Vordergrund zu ermöglichen. Die für Hintergrundaufgaben geltenden Ressourcenbeschränkungen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](support-your-app-with-background-tasks.md).

**Verwalten von Aufgaben im Hintergrund:** Ihre app sollte Abrufen einer Liste von registrierten Hintergrundaufgaben, registrieren Sie sich für den Fortschritt und Abschluss Handler und angemessene Behandlung der Ereignisse. Ihre Hintergrundaufgabenklassen müssen Fortschritt, Abbruch und Abschluss berichten. Weitere Informationen finden Sie unter [Behandeln einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md) und [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md).

**Verwendung [BackgroundTaskDeferral](https://msdn.microsoft.com/library/windows/apps/hh700499):** Wenn Ihr Hintergrund Aufgabenklasse asynchrone Code ausgeführt wird, sollten Sie unbedingt Verzögerungen zu verwenden. Andernfalls kann Ihre Hintergrundaufgabe vorzeitig beendet werden, wenn die [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode (oder die [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx)-Methode bei Hintergrundaufgaben innerhalb von Prozessen) abgeschlossen wird. Weitere Informationen finden Sie unter [Erstellen und Registrieren einer Hintergrundaufgabe außerhalb von Prozessen](create-and-register-a-background-task.md).

Alternativ können Sie eine Verzögerung bewirken und asynchrone Methodenaufrufe mit **async/await** abschließen. Schließen Sie die Verzögerung nach den **await**-Methodenaufrufen.

**Aktualisieren Sie das app-Manifest ein:**  Deklarieren Sie für Hintergrundaufgaben, die Out-of-Process ausgeführt, jeder Hintergrundaufgabe in das Anwendungsmanifest, zusammen mit dem Typ des Triggers, denen er mit verwendet wird. Andernfalls kann Ihre App die Hintergrundaufgaben zur Laufzeit nicht registrieren.

Bei mehreren Hintergrundaufgaben müssen Sie entscheiden, ob sie im selben Hostprozess oder in verschiedenen Hostprozessen ausgeführt werden sollten. Verwenden Sie separate Hostprozesse, wenn Sie befürchten, dass ein Fehler in einer Hintergrundaufgabe andere Hintergrundaufgaben beenden könnte.  Verwenden Sie den Eintrag **Ressourcengruppe** Manifest-Designer, um Hintergrundaufgaben in unterschiedliche Host-Prozesse einzuordnen. 

Zum Festlegen der **Ressourcengruppe** öffnen Sie den Package.appxmanifest-Designer, wählen **Deklarationen** und fügen eine **App Service**-Deklaration hinzu:

![Einstellungen für Ressourcengruppen](images/resourcegroup.png)

Finden Sie unter den [Schema Anwendungsverweis](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) für Weitere Informationen zu der Einstellung "Ressourcengruppe".

Hintergrundaufgaben, die im gleichen Prozess wie die Vordergrund-App ausgeführt werden, müssen sich im Anwendungsmanifest nicht selbst deklarieren. Weitere Informationen zum Deklarieren von Hintergrundaufgaben außerhalb von Prozessen, die in einem separaten Prozess im Manifest ausgeführt werden, finden Sie unter [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md).

**Bereiten Sie für app-Updates:** Wenn Ihre app aktualisiert werden, erstellen und Registrieren einer **ServicingComplete** Hintergrundaufgabe (finden Sie unter [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)) zum Aufheben der Registrierung Hintergrundaufgaben für die vorherige Version der app, und registrieren Sie die Aufgaben im Hintergrund für die neue Version. Dies ist auch ein geeigneter Zeitpunkt, um App-Updates durchzuführen, die möglicherweise außerhalb des Kontexts der Ausführung im Vordergrund erforderlich sind.

**Die Anforderung zum Ausführen der Aufgaben im Hintergrund:**

> **Wichtige**  ab Windows 10 können apps nicht mehr müssen die als Voraussetzung zum Ausführen von Aufgaben im Hintergrund auf dem Sperrbildschirm angezeigt werden.

UWP (Universelle Windows-Plattform)-Apps können alle unterstützten Aufgabentypen ausführen, ohne auf dem Sperrbildschirm angeheftet zu sein. Apps müssen jedoch vor dem Registrieren einer Hintergrundaufgabe [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) aufrufen. Diese Methode gibt [**BackgroundAccessStatus.DeniedByUser**](https://msdn.microsoft.com/library/windows/apps/hh700439) zurück, wenn der Benutzer Berechtigungen für Hintergrundaufgaben für Ihre App in den Geräteeinstellungen explizit verweigert hat. Weitere Informationen über die Auswahl des Benutzers über Hintergrundaktivitäten und den Stromsparmodus finden Sie unter [Optimieren von Hintergrundaktivitäten ](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Prüfliste für Hintergrundaufgaben

*Gilt für beide Aufgaben im in-Process und Out-of-Process-Hintergrund*

-   Verknüpfen Sie Ihre Hintergrundaufgabe mit dem korrekten Trigger.
-   Fügen Sie Bedingungen hinzu, die für das erfolgreiche Ausführen Ihrer Hintergrundaufgabe sorgen.
-   Behandeln Sie Status, Abschluss und Abbruch von Hintergrundaufgaben.
-   Registrieren Sie Ihre Hintergrundaufgaben während des App-Starts erneut. Dadurch wird sichergestellt, dass sie beim ersten Start der App registriert sind. Zudem können Sie mit dieser Methode feststellen, ob der Benutzer die Ausführung von Hintergrundaufgaben in Ihrer App deaktiviert hat (falls die Registrierung fehlschlägt)
-   Prüfen Sie Ihre App auf Fehler bei der Registrierung von Hintergrundaufgaben. Führen Sie die Registrierung der Hintergrundaufgabe ggf. mit anderen Parameterwerten erneut durch.
-   Für alle Gerätefamilien mit Ausnahme von Desktops können Hintergrundaufgaben beendet werden, wenn der Arbeitsspeicher des Geräts knapp wird. Wenn eine Ausnahme über wenig Arbeitsspeicher nicht angezeigt oder von der App nicht behandelt wird, wird die Hintergrundaufgabe ohne Warnung und ohne Auslösen des OnCanceled-Ereignisses beendet. Dadurch soll die Benutzerfreundlichkeit der App im Vordergrund sichergestellt werden. Entwerfen Sie die Hintergrundaufgabe so, dass dieses Szenario behandelt werden kann.

*Gilt nur für die Out-of-Process-Hintergrundaufgaben*

-   Erstellen Sie Ihre Hintergrundaufgabe in einer Komponente für Windows-Runtime.
-   Zeigen Sie keine UI außer Popup-, Kachel- und Signalupdates von der Hintergrundaufgabe an.
-   Fordern Sie in der [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx)-Methode Verzögerungen für jeden asynchronen Methodenaufruf an, und schließen Sie diese, wenn die Methode abgeschlossen wurde. Sie können auch eine Verzögerung mit **async/await** verwenden.
-   Verwenden Sie den dauerhaften Speicher, um Daten für die Hintergrundaufgabe und die App freizugeben.
-   Deklarieren Sie jede Hintergrundaufgabe im Anwendungsmanifest zusammen mit dem Triggertyp, mit dem sie verwendet wird. Überprüfen Sie die Korrektheit von Einstiegspunkten und Triggertypen.
-   Geben Sie im Manifest nur dann ein Executable-Element an, wenn Sie einen Trigger nutzen, der im selben Kontext wie die App ausgeführt werden soll (z. B. [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)).

*Gilt nur für prozessinterne Hintergrundaufgaben*

- Stellen Sie beim Abbrechen einer Aufgabe sicher, dass der `BackgroundActivated`-Ereignishandler vorhanden ist, bevor der Abbruch eintritt, oder der gesamte Prozess wird beendet.
-   Schreiben Sie Hintergrundaufgaben mit kurzer Laufzeit. Hintergrundaufgaben sind auf 30 Sekunden der Gesamtbetrachtungszeit beschränkt.
-   Verlassen Sie sich bei Hintergrundaufgaben nicht auf Benutzerinteraktionen.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Registrieren einer Hintergrundaufgabe innerhalb des Prozesses](create-and-register-an-inproc-background-task.md)
* [Erstellen Sie und registrieren Sie eine Out-of-Process-Hintergrundaufgabe](create-and-register-a-background-task.md)
* [Deklarieren Sie Hintergrundtasks im Manifest Anwendung](declare-background-tasks-in-the-application-manifest.md)
* [Wiedergeben von Medien im Hintergrund](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Eine abgebrochene Hintergrundaufgabe verarbeiten](handle-a-cancelled-background-task.md)
* [Überwachen von Aufgabenstatus Hintergrund und Abschluss](monitor-background-task-progress-and-completion.md)
* [Eine Hintergrundaufgabe registrieren](register-a-background-task.md)
* [Reagieren Sie auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)
* [Festlegen von Bedingungen für die Ausführung einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)
* [Live-Kachel aus einem Hintergrundtask aktualisiert](update-a-live-tile-from-a-background-task.md)
* [Verwenden Sie einen Wartungstrigger](use-a-maintenance-trigger.md)
* [Führen Sie eine Hintergrundaufgabe über einen timer](run-a-background-task-on-a-timer-.md)
* [Eine Hintergrundaufgabe Debuggen](debug-a-background-task.md)
* [Wie Sie auslösen, anhalten, fortsetzen und hintergrundereignissen in UWP-apps (beim debugging)](https://go.microsoft.com/fwlink/p/?linkid=254345)

 

 
