---
title: Lebenszyklus von Windows 10-UWP-Apps
description: In diesem Thema wird der Lebenszyklus einer Windows 10-UWP-App (Universelle Windows-Plattform) von ihrer Aktivierung bis zum Schließen beschrieben.
keywords: App-Lebenszyklus angehalten fortsetzen starten aktivieren
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 519ff7ee7bd581ce0f19c0e4297ad1ba34380068
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173034"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Lebenszyklus von Windows 10-UWP-Apps (Universelle Windows-Plattform)


In diesem Thema wird der Lebenszyklus einer UWP-App (Universelle Windows-Plattform) vom Starten bis zum Schließen beschrieben.

## <a name="a-little-history"></a>Etwas zur Vorgeschichte

Vor Windows 8 hatten Apps einen einfachen Lebenszyklus. Win32- und. NET-Apps werden entweder ausgeführt oder nicht. Wenn ein Benutzer eine App minimiert oder verlässt, wird sie weiterhin ausgeführt. Dies war in Ordnung, bis tragbare Geräte und die effiziente Energienutzung immer wichtiger wurden.

Mit Windows 8 wurde ein neues Anwendungsmodell mit UWP-apps eingeführt. Auf oberer Ebene wurde der neue Zustand „Angehalten“ eingeführt. Eine UWP-APP wird kurz nach dem minimieren oder wechseln zu einer anderen APP angehalten. Das bedeutet, dass die App-Threads angehalten werden und die App im Arbeitsspeicher verbleibt, es sei denn, das Betriebssystem muss Ressourcen zurückfordern. Wenn der Benutzer zur App zurückkehrt, kann sie schnell wieder in den Zustand „Ausgeführt“ versetzt werden.

Es gibt verschiedene Möglichkeiten für Apps, die im Hintergrund weiter ausgeführt werden müssen, z. B. [Hintergrundaufgaben](support-your-app-with-background-tasks.md) oder Apps mit [erweiterter Ausführung](/uwp/api/windows.applicationmodel.extendedexecution) und aktivitätsgestützter Ausführung. (Durch die **BackgroundMediaEnabled**-Funktion kann eine App beispielsweise weiterhin [Medien im Hintergrund wiedergeben](../audio-video-camera/background-audio.md).) Darüber hinaus können Hintergrundübertragungen selbst dann fortgesetzt werden, wenn die App angehalten oder sogar beendet wurde. Weitere Informationen finden Sie unter [So wird’s gemacht: Herunterladen einer Datei](/previous-versions/windows/apps/jj152726(v=win.10)).

Apps, die sich nicht im Vordergrund befinden, werden standardmäßig angehalten. Dies führt zu Energieeinsparungen und dazu, dass mehr Ressourcen für die derzeit im Vordergrund ausgeführte App zur Verfügung stehen.

Der Zustand „Angehalten“ stellt neue Anforderungen an Entwickler, da das Betriebssystem entscheiden kann, eine angehaltene App zu beenden, um Ressourcen freizugeben. Die beendete App wird weiterhin in der Taskleiste angezeigt. Wenn der Benutzer darauf klickt, muss die App ihren Zustand vor dem Beenden wiederherstellen, da dem Benutzer nicht bewusst ist, dass die App vom System geschlossen wurde. Er geht davon aus, dass die App im Hintergrund gewartet hat, während er andere Aufgaben erledigt hat, und erwartet, dass sie denselben Zustand wie zu dem Zeitpunkt aufweist, als er die App verlassen hat. In diesem Thema wird erläutert, wie Sie dies erreichen.

In Windows 10, Version 1607, werden zwei weitere Zustände im App-Modell eingeführt: die **Ausführung im Vordergrund** und die **Ausführung im Hintergrund**. In den folgenden Abschnitten werden auch diese neuen Zustände eingehender betrachtet.

## <a name="app-execution-state"></a>App-Ausführungszustand

In der folgenden Abbildung sind die möglichen Zustände im App-Modell dargestellt, die ab Windows 10, Version 1607, möglich sind. Sehen wir uns den typischen Lebenszyklus einer UWP-APP an.

![Zustandsdiagramm mit Übergängen zwischen App-Ausführungszuständen](images/updated-lifecycle.png)

Apps treten in den Zustand „Ausführung im Hintergrund“ ein, wenn sie gestartet oder aktiviert werden. Wenn die APP aufgrund eines Starts der Vordergrund-app in den Vordergrund verschoben werden muss, ruft die APP das [**leavingbackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground) -Ereignis ab.

Obwohl "gestartet" und "aktiviert" möglicherweise ähnliche Begriffe scheinen, verweisen Sie auf unterschiedliche Möglichkeiten, wie das Betriebssystem Ihre APP starten kann. Zunächst betrachten wir das Starten einer App.

## <a name="app-launch"></a>Starten einer App

Die [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)-Methode wird aufgerufen, wenn eine App gestartet wird. An die Methode wird ein [**LaunchActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)-Parameter übergeben, der u. a. die an die App übergebenen Argumente, den Bezeichner der Kachel, durch die die App gestartet wurde, und den vorherigen Zustand der App bereitstellt.

Den vorherigen Zustand Ihrer App können Sie von der [LaunchActivatedEventArgs.PreviousExecutionState](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate)-Eigenschaft abrufen, die eine [ApplicationExecutionState](/uwp/api/windows.applicationmodel.activation.applicationexecutionstate)-Enumeration zurückgibt. Im Folgenden sind die Werte und die Aktionen aufgeführt, die im jeweiligen Zustand ausgeführt werden sollten:

| ApplicationExecutionState | Erklärung | Auszuführende Aktion |
|-------|-------------|----------------|
| **NichtAktiv** | Eine App kann diesen Zustand aufweisen, wenn sie seit dem letzten Neustart oder der letzten Anmeldung durch den Benutzer nicht gestartet wurde. Sie kann sich auch in diesem Zustand befinden, wenn sie während der Ausführung abgestürzt ist oder weil sie zuvor vom Benutzer geschlossen wurde.| Initialisieren Sie die App, als würde sie erstmalig in der aktuellen Benutzersitzung ausgeführt. |
|**Angehalten** | Der Benutzer hat die App entweder minimiert oder verlassen und ist nicht innerhalb weniger Sekunden zurückgekehrt. | Beim Anhalten der App wurde ihr Zustand im Arbeitsspeicher beibehalten. Sie müssen lediglich Dateihandles oder andere Ressourcen erneut abrufen, die beim Anhalten der App freigegeben wurden. |
| **Beendet** | Die App wurde zuvor angehalten, dann aber irgendwann beendet, weil das System Arbeitsspeicher freigeben musste. | Stellen Sie den Zustand wieder her, in dem sich die App befand, als der Benutzer daraus gewechselt hat.|
|**ClosedByUser** | Der Benutzer hat die App im Tablet-Modus mit der Geste zum Schließen oder mit ALT+F4 geschlossen. Wenn der Benutzer die App schließt, wird sie zunächst angehalten und anschließend beendet. | Da die App im Wesentlichen dieselben Phasen durchlaufen hat, die zum Zustand „Terminated“ führen, behandeln Sie sie genauso wie eine App im Zustand „Terminated“.|
|**Wird ausgeführt** | Die App war bereits geöffnet, als der Benutzer versucht hat, sie erneut zu starten. | Nichts. Beachten Sie, dass keine weitere App-Instanz gestartet wird. Es wird einfach die bereits ausgeführte Instanz aktiviert. |

**Hinweis**  *Aktuelle Benutzersitzung* basiert auf Windows-Anmeldung. Solange sich der aktuelle Benutzer nicht abgemeldet oder Windows heruntergefahren oder neu gestartet hat, bleibt die aktuelle Benutzersitzung unabhängig von bestimmten Ereignissen – wie Sperrbildschirmauthentifizierung, Benutzerwechsel usw. – bestehen. 

Folgendes sollte unbedingt beachtet werden: Wenn das Gerät über genügend Ressourcen verfügt, startet das Betriebssystem häufig verwendete Apps, für die dieses Verhalten zulässig ist, vorab, um die Reaktionsfähigkeit zu verbessern. Eine vorab gestartete App wird Hintergrund gestartet und dann schnell angehalten. Bei Bedarf kann sie fortgesetzt werden, wodurch der Benutzer schneller darauf zugreifen kann als bei einem Start der App.

Aufgrund des Vorabstarts kann die **OnLaunched()**-Methode der App eher vom System als vom Benutzer initiiert werden. Da der Vorabstart der App im Hintergrund erfolgt, müssen Sie in **OnLaunched()** u. U. unterschiedlich darauf reagieren. Wenn die App nach dem Starten beispielsweise Musik wiedergibt, weiß keiner, woher der Sound kommt, weil die App im Hintergrund vorab gestartet wurde. Auf den Vorabstart der App im Hintergrund folgt ein Aufruf von **Application.Suspending**. Wenn der Benutzer die App dann startet, wird sowohl das resuming-Ereignis als auch die **OnLaunched()**-Methode aufgerufen. Weitere Informationen dazu, wie Sie beim Vorabstartszenario vorgehen, finden Sie unter [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md). Es werden nur Apps vorab gestartet, für die dieser Vorgang zulässig ist.

Beim Start einer App zeigt Windows einen Begrüßungsbildschirm für die App an. Informationen zum Konfigurieren des Begrüßungs Bildschirms finden Sie unter [Hinzufügen eines Begrüßungs Bildschirms](/previous-versions/windows/apps/hh465331(v=win.10)).

Während der Begrüßungsbildschirm angezeigt wird, sollte die App Ereignishandler registrieren und eine angepasste Benutzeroberfläche einrichten, die sie für die Startseite benötigt. Diese im Konstruktor der Anwendung und in **OnLaunched()** ausgeführten Aufgaben sollten möglichst innerhalb weniger Sekunden abgeschlossen sein, damit das System nicht annimmt, dass Ihre App nicht mehr reagiert, und sie beendet. Wenn eine App Daten aus dem Netzwerk anfordern oder große Datenmengen von einem Datenträger abrufen muss, sollten diese Aktivitäten außerhalb des Startvorgangs erfolgen. Eine App kann ihre eigene angepasste Ladebenutzeroberfläche oder einen erweiterten Begrüßungsbildschirm verwenden, während sie auf den Abschluss von Vorgängen mit langer Ausführungsdauer wartet. Weitere Infos finden Sie unter [Längere Anzeige des Begrüßungsbildschirms](create-a-customized-splash-screen.md) und im [Beispiel für einen Begrüßungsbildschirm](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/SplashScreen).

Nach Abschluss des App-Starts wechselt die App in den Zustand **Ausgeführt**, und der Begrüßungsbildschirm sowie alle zugehörigen Ressourcen und Objekte werden ausgeblendet.

## <a name="app-activation"></a>Aktivieren einer App

Eine App kann nicht nur vom Benutzer gestartet, sondern auch vom System aktiviert werden. Eine App kann durch einen Vertrag wie den Freigabe-Vertrag aktiviert werden. Sie kann auch aktiviert werden, um ein benutzerdefiniertes URI-Protokoll oder eine Datei mit einer Erweiterung zu verarbeiten, für deren Verarbeitung die App registriert ist. Eine Liste der Methoden zum Aktivieren Ihrer App finden Sie unter [**ActivationKind**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind).

Die [**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application)-Klasse definiert Methoden, die überschrieben werden können, um die unterschiedlichen Möglichkeiten zum Aktivieren der App zu behandeln.
[**OnActivated**](/uwp/api/windows.ui.xaml.application.onactivated) kann alle möglichen Aktivierungstypen behandeln. Üblicherweise werden die meistverwendeten Aktivierungstypen jedoch mit spezifischen Methoden behandelt, während **OnActivated** als Fallbackmethode für weniger verbreitete Aktivierungstypen verwendet wird. Im Folgenden die zusätzlichen Methoden für spezifische Aktivierungen:

[**OnCachedFileUpdaterActivated**](/uwp/api/windows.ui.xaml.application.oncachedfileupdateractivated)  
[**OnFileActivated**](/uwp/api/windows.ui.xaml.application.onfileactivated)  
[**Onfileopenpickeraktivierte**](/uwp/api/windows.ui.xaml.application.onfileopenpickeractivated)  [**onfilesavepickeraktivierte**](/uwp/api/windows.ui.xaml.application.onfilesavepickeractivated)  
[**OnSearchActivated**](/uwp/api/windows.ui.xaml.application.onsearchactivated)  
[**OnShareTargetActivated**](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)

Die Ereignisdaten für diese Methoden enthalten dieselbe [**PreviousExecutionState**](/uwp/api/windows.applicationmodel.activation.iactivatedeventargs.previousexecutionstate)-Eigenschaft wie oben, an der Sie erkennen, in welchem Zustand sich die App vor der Aktivierung befunden hat. Um den Zustand zu interpretieren und die auszuführende Aktion zu ermitteln, gehen Sie auf dieselbe Weise vor, wie oben im Abschnitt [Starten einer App](#app-launch) beschrieben.

**Hinweis**   Wenn Sie sich mit dem Administrator Konto des Computers anmelden, können Sie keine UWP-Apps aktivieren.

## <a name="running-in-the-background"></a>Ausführung im Hintergrund ##

Ab Windows 10, Version 1607, können Apps Hintergrundaufgaben im selben Prozess wie die App selbst ausführen. Weitere Informationen dazu finden Sie unter [Hintergrundaktivität mit dem Einzelprozessmodell](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99). In diesem Artikel gehen wir nicht weiter auf die In-Process-Verarbeitung im Hintergrund ein. Sie beeinflusst den App-Lebenszyklus jedoch insofern, dass zwei neue Ereignisse implementiert wurden, die sich auf die Ausführung der App im Hintergrund beziehen. Die Ereignisse sind [**EnteredBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.enteredbackground) und [**LeavingBackground**](/uwp/api/windows.applicationmodel.core.coreapplication.leavingbackground).

Sie geben auch wieder, ob die Benutzeroberfläche Ihrer App für den Benutzer sichtbar ist.

Die Hintergrundausführung ist der Standardzustand, in dem eine Anwendung gestartet, aktiviert oder fortgesetzt wird. In diesem Zustand ist die Benutzeroberfläche Ihrer Anwendung noch nicht sichtbar.

## <a name="running-in-the-foreground"></a>Ausführung im Vordergrund ##

Bei der Ausführung im Vordergrund ist die Benutzeroberfläche Ihrer App sichtbar.

Das **LeavingBackground**-Ereignis wird ausgelöst, kurz bevor die Benutzeroberfläche der Anwendung sichtbar wird und der Zustand „Ausführung im Vordergrund“ eintritt. Es wird auch ausgelöst, wenn der Benutzer wieder zu Ihrer App wechselt.

Zuvor war die beste Stelle zum Laden von UI-Ressourcen der **Activated**- oder **Resuming**-Ereignishandler. Ab jetzt bietet **LeavingBackground** die beste Möglichkeit, zu überprüfen, ob die Benutzeroberfläche bereit ist.

Außerdem sollte bis zu diesem Zeitpunkt sichergestellt sein, dass visuelle Ressourcen einsatzbereit sind, da Sie hier letzte Hand anlegen können, bevor die Anwendung für den Benutzer sichtbar wird. Alle UI-Vorgänge in diesem Ereignishandler sollten schnell abgeschlossen sein, da sie die vom Benutzer empfundene Dauer bis zum Starten oder Fortsetzen der App beeinflussen. In **LeavingBackground** sollte sichergestellt werden, dass der erste UI-Frame einsatzbereit ist. Anschließend sollten Speicher- oder Netzwerkaufrufe mit langer Ausführungsdauer asynchron behandelt werden, damit der Ereignishandler zurückgegeben werden kann.

Sobald der Benutzer die App verlässt, wechselt sie wieder in den Zustand „Ausführung im Hintergrund“.

## <a name="reentering-the-background-state"></a>Zurückwechseln in den Hintergrundzustand

Das **EnteredBackground**-Ereignis gibt an, dass die App nicht mehr im Vordergrund angezeigt wird. Auf dem Desktop wird **EnteredBackground** ausgelöst, wenn Ihre App minimiert wird, und auf dem Handy, wenn der Benutzer zum Startbildschirm oder zu einer anderen App wechselt.

### <a name="reduce-your-apps-memory-usage"></a>Verringern des Speicherbedarfs Ihrer App

Da die App nicht mehr für den Benutzer sichtbar ist, bietet sich hier die Gelegenheit, das Rendern der Benutzeroberfläche und die Ausführung von Animationen zu beenden. Mit **LeavingBackground** können Sie diese Vorgänge erneut starten.

Falls Aufgaben im Hintergrund ausgeführt werden sollen, können Sie hier entsprechende Vorbereitungen treffen. Sie sollten [MemoryManager.AppMemoryUsageLevel](/uwp/api/windows.system.memorymanager.appmemoryusagelevel) überprüfen und den von der App genutzten Arbeitsspeicher ggf. reduzieren, wenn sie im Hintergrund ausgeführt wird. So verringert sich das Risiko, dass die App vom System beendet wird, um Ressourcen freizugeben.

Weitere Details finden Sie unter [Reduzieren der Speicherverwendung, wenn die App in den Hintergrundzustand eintritt](reduce-memory-usage.md).

### <a name="save-your-state"></a>Speichern des Zustands

Der Ereignishandler für die Aussetzung ist der beste Ort zum Speichern des App-Zustands. Wenn Sie jedoch im Hintergrund arbeiten (z. b. Audiowiedergabe, mit einer erweiterten Ausführungs Sitzung oder in-proc-Hintergrundaufgabe), empfiehlt es sich auch, die Daten asynchron aus dem **Enter** Prise-Ereignishandler zu speichern. Dies liegt daran, dass es möglich ist, dass Ihre APP beendet wird, während Sie im Hintergrund eine niedrigere Priorität hat. Und da die App in diesem Fall nicht den Zustand „Angehalten“ durchlaufen hat, gehen Ihre Daten verloren.

Wenn Sie Ihre Daten in Ihrem **enteredbackground** -Ereignishandler speichern, bevor die Hintergrund Aktivität beginnt, wird eine gute Benutzererfahrung gewährleistet, wenn der Benutzer die APP wieder in den Vordergrund bringt. Zum Speichern von Daten und Einstellungen können Sie die Anwendungsdaten-APIs verwenden. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen APP-Daten](../design/app-settings/store-and-retrieve-app-data.md).

Nachdem Sie Ihre Daten gespeichert haben, können Sie bei einer Überschreitung der maximalen Speicherauslastung die Daten aus dem Speicher freigeben, da Sie sie später erneut laden können. Dadurch wird Speicher freigegeben, der von den für Hintergrundaktivitäten benötigten Ressourcen genutzt werden kann.

Beachten Sie Folgendes: Wenn in Ihrer App Hintergrundaktivitäten ausgeführt werden, kann sie vom Zustand „Ausführung im Hintergrund“ in den Zustand „Ausführung im Vordergrund“ wechseln, ohne jemals den Zustand „Angehalten“ zu erreichen.

### <a name="asynchronous-work-and-deferrals"></a>Asynchrone Vorgänge und Verzögerungen

Wenn Sie innerhalb Ihres Handlers einen asynchronen Aufruf ausführen, wird die Steuerung sofort von diesem asynchronen Aufruf zurückgegeben. Das bedeutet, dass die Ausführung dann vom Ereignishandler zurückgegeben werden kann und die App in den nächsten Zustand übergeht, obwohl der asynchrone Aufruf noch nicht abgeschlossen wurde. Verwenden Sie die [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral)-Methode für das an den Ereignishandler übergebene [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel)-Objekt, um das Anhalten zu verzögern, bis Sie für das zurückgegebene [**Windows.Foundation.Deferral**](/uwp/api/windows.foundation.deferral)-Objekt die [**Complete**](/uwp/api/windows.foundation.deferral.complete)-Methode aufgerufen haben.

Durch eine Verzögerung verlängert sich nicht die Zeit, die Ihr Code ausgeführt werden muss, bevor die App beendet wird. Dabei wird nur die Beendigung verzögert, bis die *Complete*-Methode der Verzögerung aufgerufen wird oder die Frist abläuft, *je nachdem, was zuerst eintritt*.

Falls Sie mehr Zeit zum Speichern des Zustands benötigen, sollten Sie nach Möglichkeiten suchen, den Zustand phasenweise zu speichern, bevor die App in den Hintergrundzustand eintritt, sodass weniger im **EnteredBackground**-Ereignishandler gespeichert werden muss. Alternativ können Sie auch [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) anfordern, um mehr Zeit zu erhalten. Da jedoch nicht sichergestellt ist, dass die Anforderung gewährt wird, sollten Sie Wege suchen, die zum Speichern des Zustands benötigte Zeit zu minimieren.

## <a name="app-suspend"></a>Anhalten einer App

Wenn der Benutzer eine App minimiert, wird von Windows einige Sekunden abgewartet, ob der Benutzer zur App zurückkehrt. Wenn er nicht innerhalb dieses Zeitfensters zurückkehrt und keine erweiterte Ausführung, Hintergrundaufgabe oder aktivitätsgestützte Ausführung aktiv ist, wird die App von Windows angehalten. Eine App wird auch angehalten, wenn der Sperrbildschirm angezeigt wird, es sei denn, eine erweiterte Ausführungssitzung usw. ist in der App aktiv.

Wenn eine App angehalten wird, ruft sie das [**Application.Suspending**](/uwp/api/windows.ui.xaml.application.suspending)-Ereignis auf. Die UWP-Projektvorlagen in Visual Studio enthalten für dieses Ereignis den Handler mit dem Namen **OnSuspending** in **„App.Xaml.cs“**. Vor Windows 10, Version 1607, hätten Sie den Code zum Speichern des Zustands hier eingefügt. Mittlerweile wird empfohlen, den Zustand beim Eintreten in den Hintergrundzustand zu speichern, wie oben beschrieben.

Sie sollten exklusive Ressourcen und Dateihandles freigeben, damit andere Apps darauf zugreifen können, während Ihre App angehalten wird. Beispiele für exklusive Ressourcen sind Kameras, E/A-Geräte, externe Geräte und Netzwerkressourcen. Durch die explizite Freigabe exklusiver Ressourcen und Dateihandles kann sichergestellt werden, dass andere Apps darauf zugreifen können, während Ihre App angehalten ist. Wenn die App fortgesetzt wird, sollte sie ihre exklusiven Ressourcen und Dateihandles erneut abrufen.

### <a name="be-aware-of-the-deadline"></a>Achten Sie auf die Ablauffrist

Um zu gewährleisten, dass Geräte schnell und flexibel reagieren, ist die Zeit zum Ausführen von Code im suspending-Ereignishandler begrenzt. Sie wird als Frist bezeichnet, ist für jedes Gerät unterschiedlich und kann mithilfe einer Eigenschaft des [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation)-Objekts ermittelt werden.

Hier verhält es sich genauso wie beim **EnteredBackground**-Ereignishandler: Wenn Sie innerhalb Ihres Handlers einen asynchronen Aufruf ausführen, wird die Steuerung sofort von diesem asynchronen Aufruf zurückgegeben. Das bedeutet, dass die Ausführung dann vom Ereignishandler zurückgegeben werden kann und die App in den Zustand „Angehalten“ übergeht, obwohl der asynchrone Aufruf noch nicht abgeschlossen wurde. Verwenden Sie die [**GetDeferral**](/uwp/api/windows.applicationmodel.suspendingoperation.getdeferral)-Methode für das [**SuspendingOperation**](/uwp/api/Windows.ApplicationModel.SuspendingOperation)-Objekt (verfügbar über die Ereignisargumente), um den Wechsel in den Zustand „Angehalten“ zu verzögern, bis Sie die [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete)-Methode für das zurückgegebene [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral)-Objekt aufgerufen haben.

Falls Sie mehr Zeit benötigen, können Sie auch [ExtendedExecutionSession](/archive/msdn-magazine/2015/windows-10-special-issue/app-lifecycle-keep-apps-alive-with-background-tasks-and-extended-execution) anfordern. Da jedoch nicht sichergestellt ist, dass die Anforderung gewährt wird, sollten Sie Wege suchen, die im **Suspended**-Ereignishandler benötigte Zeit zu minimieren.

### <a name="app-terminate"></a>Beenden einer App

Das System versucht, die App und die zugehörigen Daten im Arbeitsspeicher beizubehalten, während sie angehalten ist. Wenn das System aber nicht über die notwendigen Ressourcen verfügt, um die App im Arbeitsspeicher beizubehalten, wird die App beendet. Apps erhalten keine Benachrichtigung, dass sie beendet werden. App-Daten können also nur im **OnSuspension**-Ereignishandler oder asynchron im **EnteredBackground**-Handler gespeichert werden.

Wenn Ihre App feststellt, dass sie nach dem Beenden aktiviert wurde, sollte sie die gespeicherten Anwendungsdaten laden, damit die App denselben Zustand wie vor dem Beenden aufweist. Wenn der Benutzer wieder zu einer angehaltenen App wechselt, die beendet wurde, sollte die App ihre Anwendungsdaten mithilfe der [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)-Methode wiederherstellen. Das System benachrichtigt eine App nicht, wenn sie beendet wird. Bevor Ihre App angehalten wird, muss sie die Anwendungsdaten speichern und exklusive Ressourcen und Dateihandles freigeben und diese wiederherstellen, wenn sie nach der Beendigung erneut aktiviert wird.

**Hinweis zum Debuggen mit Visual Studio:** Visual Studio hindert Windows daran, eine APP zu sperren, die an den Debugger angefügt ist. Dies hat den Zweck, dem Benutzer das Anzeigen der Debugging-Benutzeroberfläche von Visual Studio zu ermöglichen, während die App ausgeführt wird. Beim Debuggen einer App können Sie mit Visual Studio ein Anhalteereignis an die App senden. Stellen Sie sicher, dass die Symbolleiste **Debugspeicherort** angezeigt wird, und klicken Sie dann auf das Symbol **Anhalten**.

## <a name="app-resume"></a>Fortsetzen einer App

Eine angehaltene App wird fortgesetzt, wenn der Benutzer zu ihr wechselt oder es sich um die aktive App handelt und der Energiesparmodus für das Gerät beendet wird.

Wenn eine App aus dem Zustand **Angehalten** fortgesetzt wird, wechselt sie in den Zustand **Ausführung im Hintergrund**, und das System stellt die App so wieder her, wie sie verlassen wurde. So entsteht für den Benutzer der Eindruck, die App wäre im Hintergrund weiter ausgeführt worden. Im Arbeitsspeicher gespeicherte App-Daten gehen nicht verloren. Daher müssen die meisten Apps den Zustand nicht wiederherstellen, wenn sie fortgesetzt werden. Sie sollten allerdings alle Datei- oder Gerätehandles erneut abrufen, die sie beim Anhalten freigegeben haben, und jeden Zustand wiederherstellen, der beim Anhalten der App explizit freigegeben wurde.

Eine App kann für Stunden oder Tage angehalten werden. Wenn Inhalte oder Netzwerkverbindungen der App inzwischen veraltet sind, sollten diese beim Fortsetzen der App aktualisiert werden. Wenn eine App einen Ereignishandler für das [**Application.Resuming**](/uwp/api/windows.ui.xaml.application.resuming)-Ereignis registriert hat, wird dieser beim Fortsetzen der App aus dem Zustand **Angehalten** aufgerufen. Sie können die App-Inhalte und -Daten in diesem Ereignishandler aktualisieren.

Wenn eine angehaltene App für die Teilnahme an einem App-Vertrag oder einer Erweiterung aktiviert wird, empfängt sie zunächst das **Resuming**-Ereignis und dann das **Activated**-Ereignis.

Wenn die angehaltene App beendet wurde, gibt es kein **Resuming**-Ereignis, und stattdessen wird **OnLaunched()** mit einem **ApplicationExecutionState** von **Terminated** aufgerufen. Da Sie den Zustand beim Anhalten der App gespeichert haben, können Sie ihn während **OnLaunched()** wiederherstellen. So hat der Benutzer den Eindruck, dass die App denselben Zustand aufweist wie zu dem Zeitpunkt, als er sie verlassen hat.

Angehaltene Apps empfangen keine Netzwerkereignisse, für deren Empfang sie registriert sind. Diese Netzwerkereignisse werden nicht in die Warteschlange gestellt, sondern einfach verpasst. Apps sollten beim Fortsetzen daher den Netzwerkstatus prüfen.

**Hinweis**    Da das [**resuming**](/uwp/api/windows.ui.xaml.application.resuming) -Ereignis nicht vom UI-Thread ausgelöst wird, muss ein Verteiler verwendet werden, wenn der Code in Ihrem Resume-Handler mit ihrer Benutzeroberfläche kommuniziert. Ein Codebeispiel zur Vorgehensweise finden Sie unter [Aktualisieren des UI-Threads von einem Hintergrundthread](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md).

Allgemeine Richtlinien finden Sie unter [Richtlinien für das Anhalten und Fortsetzen von Apps](./index.md).

## <a name="app-close"></a>Schließen einer App

Im Allgemeinen müssen Benutzer Apps nicht schließen, sondern können deren Verwaltung Windows überlassen. Benutzer können Apps jedoch mit der Geste zum Schließen, durch Drücken von ALT+F4 oder mithilfe der Aufgabenumschaltfunktion in Windows Phone schließen.

Es gibt kein Ereignis zum Angeben, dass der Benutzer die App geschlossen hat. Wenn eine App durch den Benutzer geschlossen wird, wird sie zuerst angehalten, damit Sie ihren Zustand speichern können. In Windows 8.1 und höher wird eine App nach dem Schließen durch den Benutzer vom Bildschirm und aus der Umschaltliste entfernt, aber nicht explizit beendet.

**Verhalten durch geschlossene Benutzer:**    Wenn Ihre APP etwas anderes tun muss, wenn Sie vom Benutzer geschlossen wird, als wenn Sie von Windows geschlossen wird, können Sie den Aktivierungs Ereignishandler verwenden, um zu bestimmen, ob die APP vom Benutzer oder von Windows beendet wurde. Beschreibungen zu den Status **ClosedByUser** und **Terminated** finden Sie in der Referenz für die [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)-Enumeration.

Wir raten dazu, dass Apps sich selbst nur dann programmgesteuert schließen sollten, wenn dies absolut erforderlich ist. Wenn eine App beispielsweise einen Arbeitsspeicherverlust erkennt, kann sie sich selbst schließen, um die Sicherheit der persönlichen Daten des Benutzers zu wahren.

## <a name="app-crash"></a>App-Absturz

Der Systemabsturz ist dafür vorgesehen, dass die Benutzer ihre vorherigen Aktivitäten so schnell wie möglich wieder aufnehmen können. Sie sollten kein Warnungsdialogfeld oder eine andere Benachrichtigung bereitstellen, da dies für den Benutzer eine Verzögerung verursacht.

Wenn die App abstürzt, nicht mehr reagiert oder eine Ausnahme generiert, wird über die [Feedback- und Diagnoseeinstellungen](https://support.microsoft.com/help/4468236/diagnostics-feedback-and-privacy-in-windows-10-microsoft-privacy) des Benutzers ein Problembericht an Microsoft gesendet. Microsoft stellt Ihnen einige der Fehlerdaten im Problembericht bereit, mit denen Sie Ihre App verbessern können. Sie können diese Daten auf der Seite „Qualität“ Ihrer App im Dashboard einsehen.

Wenn der Benutzer eine App nach einem Absturz aktiviert, empfängt ihr Ereignishandler einen [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)-Wert **NotRunning**, und es sollten einfach die ursprüngliche Benutzeroberfläche und die ursprünglichen Daten der App angezeigt werden. Verwenden Sie nach einem Absturz nicht routinemäßig die APP-Daten, die Sie für die wieder **Aufnahme** mit angehalten hätten **, da diese** Daten beschädigt werden könnten. Weitere Informationen finden Sie unter [Richtlinien für App Suspend und Resume](./index.md)

## <a name="app-removal"></a>App-Deinstallation

Löscht ein Benutzer eine App, wird die App mit allen zugehörigen lokalen Daten entfernt. Das Entfernen von Apps hat keine Folgen für die Benutzerdaten, die an allgemeinen Speicherorten, z. B. in den Dokument- oder Bildbibliotheken, gespeichert wurden.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>App-Lebenszyklus und Visual Studio-Projektvorlagen

Der grundlegende Code, der für den Lebenszyklus der App relevant ist, wird in den Visual Studio-Projektvorlagen bereitgestellt. Die einfache App behandelt die Startaktivierung, stellt einen Speicherort zum Wiederherstellen Ihrer App-Daten bereit und zeigt die primäre Benutzeroberfläche an, bevor Sie eigenen Code hinzugefügt haben. Weitere Informationen finden Sie unter [c#-, VB-und C++-Projektvorlagen für-apps](/previous-versions/windows/apps/hh768232(v=win.10)).

## <a name="key-application-lifecycle-apis"></a>Wichtige APIs für den Anwendungslebenszyklus

-   [**Windows.ApplicationModel**](/uwp/api/Windows.ApplicationModel)-Namespace
-   [**Windows. applicationmodel. Activation**](/uwp/api/Windows.ApplicationModel.Activation) -Namespace
-   [**Windows.ApplicationModel.Core**](/uwp/api/Windows.ApplicationModel.Core)-Namespace
-   [**Windows.UI.Xaml.Application**](/uwp/api/Windows.UI.Xaml.Application)-Klasse (XAML)
-   [**Windows.UI.Xaml.Window**](/uwp/api/Windows.UI.Xaml.Window)-Klasse (XAML)

## <a name="related-topics"></a>Zugehörige Themen

* [**ApplicationExecutionState**](/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)
* [Richtlinien für das Anhalten und Fortsetzen von Apps](./index.md)
* [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md)
* [Behandeln der App-Aktivierung](activate-an-app.md)
* [Behandeln des Anhaltens von Apps](suspend-an-app.md)
* [Behandeln der App-Fortsetzung](resume-an-app.md)
* [Hintergrund Aktivität mit dem Modell mit einem einzelnen Prozess](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [Wiedergeben von Medien im Hintergrund](../audio-video-camera/background-audio.md)

 

 