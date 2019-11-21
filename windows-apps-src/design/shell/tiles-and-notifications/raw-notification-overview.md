---
Description: Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen.
title: Übersicht über unformatierte Benachrichtigungen
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b04e48163af47b7e753bc3bc050e44a947b122fc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259701"
---
# <a name="raw-notification-overview"></a>Übersicht über unformatierte Benachrichtigungen


Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen. Sie dienen ausschließlich zu Anweisungszwecken und enthalten keine UI-Komponente. Wie bei anderen Pushbenachrichtigungen übermittelt das Feature für den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS) unformatierte Benachrichtigungen von Ihrem Clouddienst an Ihre App.

Unformatierte Benachrichtigungen können für verschiedenste Zwecke verwendet werden, z. B. zum Auslösen einer Hintergrundaufgabe in der App, wenn der Benutzer der App die entsprechende Berechtigung erteilt hat. Durch die Verwendung von WNS für die Kommunikation mit der App entfällt der Verarbeitungsaufwand für das Erstellen dauerhafter Socketverbindungen, Senden von HTTP GET-Nachrichten und andere Verbindungen zwischen Dienst und App.

> [!IMPORTANT]
> Um die Funktionsweise von unformatierten Benachrichtigungen verstehen zu können, sollten Sie mit den in [Übersicht über den den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](windows-push-notification-services--wns--overview.md) erörterten Konzepten vertraut sein.

 

Wie bei Popup-, Kachel- und Signalpushbenachrichtigungen wird eine unformatierte Benachrichtigung vom Clouddienst Ihrer App über einen zugewiesenen Kanal-URI (Uniform Resource Identifier) an WNS gesendet. WNS wiederum übermittelt die Benachrichtigung an das Gerät und das Benutzerkonto, das diesem Kanal zugeordnet ist. Im Gegensatz zu anderen Pushbenachrichtigungen weisen unformatierte Benachrichtigungen kein bestimmtes Format auf. Der Inhalt der Nutzlast wird ausschließlich durch die App definiert.

Als Beispiel für eine App, die von der Verwendung unformatierter Benachrichtigungen profitieren könnte, nehmen wir hier eine theoretische Zusammenarbeits-App für Dokumente. Angenommen, zwei Benutzer bearbeiten gleichzeitig das gleiche Dokument. Der Clouddienst, der das freigegebene Dokument hostet, könnte jeden Benutzer mithilfe von unformatierten Benachrichtigungen informieren, wenn Änderungen vom anderen Benutzer vorgenommen werden. Die unformatierten Benachrichtigungen müssen dabei nicht die Änderungen am Dokument enthalten. Stattdessen weisen sie die App-Instanzen der beiden Benutzer an, eine Verbindung mit dem zentralen Speicherort herzustellen und die verfügbaren Änderungen zu synchronisieren. Durch die Verwendung von unformatierten Benachrichtigungen entfällt für die App und ihren Clouddienst der Mehraufwand, während der gesamten Zeit, in der das Dokument geöffnet ist, dauerhafte Verbindungen aufrecht zu erhalten.

## <a name="how-raw-notifications-work"></a>Funktionsweise von unformatierten Benachrichtigungen


Alle unformatierten Benachrichtigungen sind Pushbenachrichtigungen. Daher gilt die zum Senden und Empfangen von Pushbenachrichtigungen erforderliche Konfiguration auch für unformatierte Benachrichtigungen:

-   Sie benötigen einen gültigen WNS-Kanal zum Senden von unformatierten Benachrichtigungen. Weitere Informationen zum Anfordern eines Pushbenachrichtigungskanals finden Sie unter [Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10)).
-   Die **Internet**-Funktion muss im App-Manifest hinzugefügt werden. Im Manifest-Editor von Microsoft Visual Studio befindet sich diese Option auf der Registerkarte **Funktionen** unter **Internet (Client)** . Weitere Informationen finden Sie unter [**Funktionen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities).

Für den Textkörper der Benachrichtigung wird ein von der App definiertes Format verwendet. Der Client empfängt die Daten als eine mit Null beendete Zeichenfolge (**HSTRING**), die nur von der App verstanden werden muss.

Wenn der Client offline ist, werden unformatierte Benachrichtigungen nur dann von WNS zwischengespeichert, wenn der [X-WNS-Cache-Policy](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))-Header in der Benachrichtigung enthalten ist. Es wird jedoch jeweils nur eine unformatierte Benachrichtigung zwischengespeichert und übermittelt, sobald das Gerät wieder online ist.

Auf dem Client gibt es nur drei Möglichkeiten für die Verarbeitung einer unformatierten Benachrichtigung: Sie wird durch ein Benachrichtigungsübermittlungsereignis an die aktive App übermittelt, an eine Hintergrundaufgabe gesendet oder verworfen. Ist der Client offline, wenn WNS versucht, eine unformatierte Benachrichtigung zu übermitteln, wird die Benachrichtigung daher verworfen.

## <a name="creating-a-raw-notification"></a>Erstellen einer unformatierten Benachrichtigung


Mit Ausnahme der folgenden Unterschiede ist das Senden einer unformatierten Benachrichtigung mit dem einer Kachel-, Popup- oder Signalpushbenachrichtigung identisch:

-   Der HTTP Content-Type-Header muss auf "application/octet-stream" festgelegt werden.
-   Der HTTP [X-WNS-Type](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))-Header muss auf "wns/raw" festgelegt werden.
-   Der Textkörper der Benachrichtigung kann eine beliebige Nutzlast kleiner 5 KB enthalten.

Unformatierte Benachrichtigungen sollen als Kurznachrichten verwendet werden, die eine Aktion der App auslösen (z. B. Herstellen einer direkten Verbindung mit dem Dienst, um eine größere Datenmenge zu synchronisieren, oder Durchführen einer lokalen Statusänderung auf Grundlage des Benachrichtigungsinhalts). Beachten Sie, dass die Übermittlung von WNS-Pushbenachrichtigungen nicht garantiert werden kann. In der App und dem Clouddienst muss daher die Möglichkeit berücksichtigt werden, dass die unformatierte Benachrichtigung den Client nicht erreicht, z. B. wenn dieser offline ist.

Weitere Informationen zum Senden von Pushbenachrichtigungen finden Sie unter [Schnellstart: Senden einer Pushbenachrichtigung](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10)).

## <a name="receiving-a-raw-notification"></a>Empfangen einer unformatierten Benachrichtigung


Es sind zwei Mechanismen verfügbar, über die Ihre App unformatierte Benachrichtigungen empfangen kann:

-   Durch [Benachrichtigungsübermittlungsereignisse](#notification-delivery-events) während die App ausgeführt wird.
-   Durch [von der unformatierten Benachrichtigung ausgelöste Hintergrundaufgaben](#background-tasks-triggered-by-raw-notifications), wenn die App Hintergrundaufgaben ausführen kann.

Eine App kann beide Mechanismen zum Empfangen von unformatierten Benachrichtigungen verwenden. Bei einer App, die sowohl den Benachrichtigungsübermittlungs-Ereignishandler als auch von unformatierten Benachrichtigungen ausgelöste Hintergrundaufgaben implementiert, hat das Benachrichtigungsübermittlungsereignis Priorität, wenn die App ausgeführt wird.

-   Ist die App aktiv, hat das Benachrichtigungsübermittlungsereignis Vorrang vor der Hintergrundaufgabe, und die App erhält als Erstes die Gelegenheit, die Benachrichtigung zu verarbeiten.
-   Der Benachrichtigungsübermittlungs-Ereignishandler kann festlegen, dass die unformatierte Benachrichtigung bei der Beendigung des Handlers nicht an die Hintergrundaufgabe übergeben werden soll, indem er die [**PushNotificationReceivedEventArgs.Cancel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel)-Eigenschaft des Ereignisses auf **true** festlegt. Wenn die **Cancel**-Eigenschaft auf **false** oder nicht festgelegt ist (der Standardwert ist **false**), löst die unformatierte Benachrichtigung die Hintergrundaufgabe aus, nachdem der Benachrichtigungsübermittlungs-Ereignishandler beendet wurde.

### <a name="notification-delivery-events"></a>Benachrichtigungsübermittlungsereignisse

Ihre App kann über ein Benachrichtigungsübermittlungsereignis ([**PushNotificationReceived**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) unformatierte Benachrichtigungen erhalten, während die App verwendet wird. Wenn der Clouddienst eine unformatierte Benachrichtigung sendet, kann die aktive App die Benachrichtigung empfangen, indem sie das Benachrichtigungsübermittlungsereignis für den Kanal-URI behandelt.

Falls die App nicht aktiv ist und keine [Hintergrundaufgaben](#background-tasks-triggered-by-raw-notifications) verwendet, werden alle an sie gesendeten unformatierten Benachrichtigungen beim Empfang von WNS verworfen. Um zu verhindern, dass Ressourcen des Clouddiensts vergeudet werden, sollten Sie ggf. eine Logik implementieren, mit der der Dienst nachverfolgen kann, ob die App aktiv ist. Für diese Informationen kommen zwei Quellen in Frage: Eine App kann dem Dienst explizit mitteilen, dass sie für den Empfang von Benachrichtigungen bereit ist, und WNS kann dem Dienst mitteilen, wann er die Übermittlung beenden soll.

-   **Die App benachrichtigt den Clouddienst**: Die App kann eine Verbindung mit dem Dienst herstellen, um ihm mitzuteilen, dass sie im Vordergrund ausgeführt wird. Der Nachteil bei diesem Ansatz ist, dass die App u. U. sehr oft eine Verbindung mit dem Dienst herstellt. Der Vorteil ist jedoch, dass der Dienst immer weiß, wann die App für den Empfang eingehender unformatierter Benachrichtigungen bereit ist. Ein weiterer Vorteil: Wenn die App eine Verbindung mit dem Dienst herstellt, kann der Dienst unformatierte Benachrichtigungen an die spezifische Instanz der App senden, anstatt einen Broadcast zu senden.
-   **Der Clouddienst antwortet auf WNS-Antwortnachrichten**: Der App-Dienst kann anhand der von WNS zurückgegebenen Informationen [X-WNS-NotificationStatus](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)) und [X-WNS-DeviceConnectionStatus](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)) feststellen, wann keine unformatierten Benachrichtigungen mehr an die App gesendet werden sollen. Wenn Ihr Dienst eine Benachrichtigung als HTTP-POST an einen Kanal sendet, kann er als Antwort eine der folgenden Nachrichten erhalten:

    -   **X-WNS-NotificationStatus: dropped**: Gibt an, dass die Benachrichtigung nicht vom Client empfangen wurde. Die **dropped**-Antwort ist in der Regel darauf zurückzuführen, dass die App auf dem Gerät des Benutzers nicht mehr im Vordergrund ausgeführt wird.
    -   **X-WNS-DeviceConnectionStatus: disconnected** oder **X-WNS-DeviceConnectionStatus: tempconnected**: Gibt an, dass der Windows-Client nicht mehr mit WNS verbunden ist. Diese Nachricht von WNS kann nur empfangen werden, wenn Sie sie durch Festlegen des [X-WNS-RequestForStatus](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))-Headers in der HTTP-POST-Nachricht der Benachrichtigung anfordern.

    Der Clouddienst Ihrer App kann die Informationen in diesen Statusnachrichten verwenden, um Kommunikationsversuche durch unformatierte Benachrichtigungen zu beenden. Der Dienst kann das Senden von unformatierten Benachrichtigungen fortsetzen, wenn die App wieder im Vordergrund ausgeführt wird und eine Verbindung mit ihm herstellt.

    Sie sollten sich nicht auf [X-WNS-NotificationStatus](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)) verlassen, um festzustellen, ob die Benachrichtigung erfolgreich an den Client übermittelt wurde.

    Weitere Informationen finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)).

### <a name="background-tasks-triggered-by-raw-notifications"></a>Von unformatierten Benachrichtigungen ausgelöste Hintergrundaufgaben

> [!IMPORTANT]
> Vor der Verwendung von Hintergrundaufgaben für unformatierte Benachrichtigungen, muss einer App über [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) die Berechtigung zum Ausführen von Hintergrundaufgaben erteilt werden.

 

Die Hintergrundaufgabe muss mit einem [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) registriert sein. Ist die Aufgabe nicht registriert, wird sie beim Empfang einer unformatierten Benachrichtigung nicht ausgeführt.

Eine von einer unformatierten Benachrichtigung ausgelöste Hintergrundaufgabe ermöglicht es dem Clouddienst Ihrer App, eine Verbindung mit der App herzustellen, auch wenn die App nicht ausgeführt wird (sie kann die Ausführung jedoch auslösen). Dies ist möglich, ohne dass die App eine dauerhafte Verbindung aufrecht erhalten muss. Unformatierte Benachrichtigungen sind der einzige Benachrichtigungstyp, der Hintergrundaufgaben auslösen kann. Doch obwohl Popup-, Kachel- und Signalpushbenachrichtigungen keine Hintergrundaufgaben auslösen können, können durch unformatierte Benachrichtigungen ausgelöste Hintergrundaufgaben Kacheln aktualisieren und Popupbenachrichtigungen über lokale API-Aufrufe aufrufen.

Zur Veranschaulichung der Funktionsweise von Hintergrundaufgaben, die durch unformatierte Benachrichtigungen ausgelöst werden, nehmen wir als Beispiel eine App zum Lesen von E-Books. Als Erstes kauft ein Benutzer (möglicherweise auf einem anderen Gerät) online ein Buch. Als Reaktion kann der Clouddienst der App eine unformatierte Benachrichtigung, deren Nutzlast besagt, dass das Buch gekauft wurde und von der App heruntergeladen werden soll, an jedes Gerät des Benutzers senden. Die App stellt dann eine direkte Verbindung mit dem Clouddienst der App her, um einen Hintergrunddownload des neuen Buchs zu starten, damit das Buch später, wenn der Benutzer die App startet, schon da ist und gelesen werden kann.

Um eine Hintergrundaufgabe mit einer unformatierten Benachrichtigung auszulösen, muss Ihre App:

1.  die Berechtigung zum Ausführen von Hintergrundaufgaben (die der Benutzer jederzeit widerrufen kann) über [**BackgroundExecutionManager.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_) anfordern.
2.  die Hintergrundaufgabe implementieren. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](../../../launch-resume/support-your-app-with-background-tasks.md).

Die Hintergrundaufgabe wird dann jedes Mal als Reaktion auf den [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) aufgerufen, wenn eine unformatierte Benachrichtigung für die App empfangen wird. Die Hintergrundaufgabe interpretiert die App-spezifische Nutzlast der unformatierten Benachrichtigung und führt die entsprechenden Aktionen aus.

Für jede App kann jeweils nur eine Hintergrundaufgabe ausgeführt werden. Wird eine Hintergrundaufgabe für eine App ausgelöst, für die bereits eine Hintergrundaufgabe ausgeführt wird, muss die erste Hintergrundaufgabe abgeschlossen werden, bevor die neue Aufgabe ausgeführt wird.

## <a name="other-resources"></a>Weitere Ressourcen


Weitere Informationen finden Sie unter Herunterladen des Beispiels für unformatierte [Benachrichtigungen](https://code.msdn.microsoft.com/windowsapps/Raw-notifications-sample-3bc28c5d) für Windows 8.1 und das [Beispiel für Pushbenachrichtigungen und periodische Benachrichtigungen](https://code.msdn.microsoft.com/windowsapps/push-and-periodic-de225603) für Windows 8.1 und wieder verwenden Ihres Quellcodes in Ihrer Windows 10-app.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für unformatierte Benachrichtigungen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-raw-notification-overview)
* [Schnellstart: Erstellen und Registrieren einer unformatierten Benachrichtigungs Hintergrundaufgabe](https://docs.microsoft.com/previous-versions/windows/apps/jj676800(v=win.10))
* [Schnellstart: Abfangen von Pushbenachrichtigungen für laufende Apps](https://docs.microsoft.com/previous-versions/windows/apps/jj709908(v=win.10))
* [**Rawnotification**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**Backgroundexecutionmanager. requestaccessasync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 




