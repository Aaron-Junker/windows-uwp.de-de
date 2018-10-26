---
author: drewbatgit
ms.assetid: 3848cd72-eccd-400e-93ff-13649cd81b6c
description: Dieser Artikel bietet Unterstützung für Apps mit dem Legacy-Hintergrundmedienmodell für die Wiedergabe und gibt Anleitungen für die Migration in das neue Modell.
title: Medienwiedergabe im Hintergrund (Legacy)
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 319343a06eeb49fc4ec0ca2fcd340f655654f718
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "5638162"
---
# <a name="legacy-background-media-playback"></a>Medienwiedergabe im Hintergrund (Legacy)


In diesem Artikel wird das ältere Modell zur Unterstützung von Hintergrundaudio für Ihre UWP-App beschrieben, das zwei Prozesse umfasst. Ab Windows 10, Version 1607, wird ein Modell für die Audiowiedergabe im Hintergrund verwendet, das nur einen Prozess umfasst und sehr viel einfacher zu implementieren ist. Weitere Informationen zu den aktuellen Empfehlungen für Hintergrundaudio finden Sie unter [Wiedergeben von Medien im Hintergrund](background-audio.md). Dieser Artikel bietet Hilfe bei der Unterstützung von Apps, die unter Verwendung des älteren Modells mit zwei Prozessen entwickelt wurden.

> [!NOTE]
> Ab Windows, Version 1703, **BackgroundMediaPlayer** ist veraltet und möglicherweise nicht in zukünftigen Versionen von Windows zur Verfügung.

## <a name="background-audio-architecture"></a>Architektur von Hintergrundaudio

Eine App für die Hintergrundwiedergabe besteht aus zwei Prozessen. Der erste Prozess ist die Haupt-App, die die App-UI und Clientlogik enthält und im Vordergrund ausgeführt wird. Der zweite Prozess ist die Wiedergabeaufgabe im Hintergrund, die [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) wie alle Hintergrundaufgaben von UWP-Apps implementiert. Die Hintergrundaufgabe enthält die Logik für die Audiowiedergabe und die Hintergrunddienste. Die Hintergrundaufgabe kommuniziert mit dem System über die Steuerelemente für den Systemmedientransport.

Das folgende Diagramm ist eine Übersicht des Systemdesigns.

![Architektur der Windows10-Hintergrundaudio](images/backround-audio-architecture-win10.png)
## <a name="mediaplayer"></a>Media Player

Der [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562)-Namespace enthält APIs, die zum Wiedergeben von Audio im Hintergrund verwendet werden. Es gibt eine einzige Instanz von [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) pro App, über die die Wiedergabe erfolgt. Ihre Hintergrundaudio-App ruft Methoden auf und legt Eigenschaften für die **MediaPlayer**-Klasse zum Festlegen des aktuellen Titels, zum Starten der Wiedergabe, zum Anhalten, zum schnellen Vor- und Zurückspulen usw. fest. Der Zugriff auf die Media Player-Objektinstanz erfolgt immer über die [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528)-Eigenschaft.

## <a name="mediaplayer-proxy-and-stub"></a>MediaPlayer-Proxy und -Stub

Wenn auf **BackgroundMediaPlayer.Current** über den Hintergrundprozess der App zugegriffen wird, wird die **MediaPlayer**-Instanz im Hintergrundaufgabenhost aktiviert und kann direkt bearbeitet werden.

Wenn auf **BackgroundMediaPlayer.Current** über die Vordergrund-App zugegriffen wird, ist die zurückgegebene **MediaPlayer**-Instanz tatsächlich ein Proxy, der mit einem Stub im Hintergrundprozess kommuniziert. Dieser Stub kommuniziert mit der tatsächlichen **MediaPlayer**-Instanz, die auch im Hintergrundprozess gehostet wird.

Sowohl der Vordergrund- als auch der Hintergrundprozess können auf den Großteil der Eigenschaften der **MediaPlayer**-Instanz zugreifen, mit Ausnahme von [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010) und [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635), auf die nur über den Hintergrundprozess zugegriffen werden kann. Die Vordergrund-App und der Hintergrundprozess können beide Benachrichtigungen über medienspezifische Ereignisse wie [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/dn652609), [**MediaEnded**](https://msdn.microsoft.com/library/windows/apps/dn652603) und [**MediaFailed**](https://msdn.microsoft.com/library/windows/apps/dn652606) empfangen.

## <a name="playback-lists"></a>Wiedergabelisten

Ein häufiges Szenario für Hintergrundaudioanwendungen besteht darin, mehrere Elemente in einer Zeile wiederzugeben. Dies kann am einfachsten im Hintergrundprozess mithilfe eines [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955)-Objekts erreicht werden, das als Quelle für den **MediaPlayer** festgelegt werden kann, indem es der [**MediaPlayer.Source**](https://msdn.microsoft.com/library/windows/apps/dn987010)-Eigenschaft zugewiesen wird.

Es ist nicht möglich, über den Vordergrundprozess, der im Hintergrundprozess festgelegt wurde, auf eine **MediaPlaybackList** zuzugreifen.

## <a name="system-media-transport-controls"></a>Steuerelemente für den Systemmedientransport

Ein Benutzer kann beispielsweise mithilfe von Bluetooth-Geräten, SmartGlass und die Steuerelemente für den Systemmedientransport die Audiowiedergabe ohne direkte Verwendung der Benutzeroberfläche der App steuern. Die Hintergrundaufgabe verwendet die [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677)-Klasse, um diese vom Benutzer initiierten Systemereignisse zu abonnieren.

Zum Abrufen einer **SystemMediaTransportControls**-Instanz aus dem Hintergrundprozess verwenden Sie die [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635)-Eigenschaft. Vordergrund-Apps rufen eine Instanz der Klasse mithilfe von [**SystemMediaTransportControls.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708) auf. Die zurückgegebene Instanz ist jedoch eine Vordergrundinstanz, die nicht mit der Hintergrundaufgabe im Zusammenhang steht.

## <a name="sending-messages-between-tasks"></a>Senden von Nachrichten zwischen Aufgaben

Unter Umständen möchten Sie, dass die beiden Prozesse einer Hintergrund-App miteinander kommunizieren. Beispielsweise kann es sein, dass die Hintergrundaufgabe die Vordergrundaufgabe benachrichtigen soll, wenn die Wiedergabe eines neuen Titels gestartet wird. Außerdem soll dann der Songtitel an die Vordergrundaufgabe gesendet werden, damit er auf dem Bildschirm angezeigt wird.

Über einen einfachen Kommunikationsmechanismus werden Ereignisse sowohl im Vordergrundprozess als auch im Hintergrundprozess ausgelöst. Die Methoden [**SendMessageToForeground**](https://msdn.microsoft.com/library/windows/apps/dn652533) und [**SendMessageToBackground**](https://msdn.microsoft.com/library/windows/apps/dn652532) rufen jeweils Ereignisse in dem entsprechenden Prozess auf. Nachrichten können durch Abonnieren der Ereignisse [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530) und [**MessageReceivedFromForeground**](https://msdn.microsoft.com/library/windows/apps/dn652531) empfangen werden.

Daten können als Argument an die Methoden zum Senden einer Nachricht übergeben werden, die dann an die „MessageReceived“-Ereignishandler übergeben werden. Übergeben Sie Daten mit der [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/dn636131)-Klasse. Bei dieser Klasse handelt es sich um ein Wörterbuch, das eine Zeichenfolge als Schlüssel und andere Werttypen als Werte enthält. Sie können einfache Werttypen übergeben, z.B. ganze Zahlen, Zeichenfolgen und booleschen Werte.

## <a name="background-task-life-cycle"></a>Lebenszyklus von Hintergrundaufgaben

Die Lebensdauer einer Hintergrundaufgabe ist eng mit dem aktuellen Wiedergabestatus Ihrer App verknüpft. Wenn der Benutzer die Audiowiedergabe beispielsweise anhält, kann das System die App je nach den Umständen beenden oder abbrechen. Das System kann die Hintergrundaufgabe nach einer gewissen Zeit ohne Audiowiedergabe automatisch beenden.

Die [**IBackgroundTask.Run**](https://msdn.microsoft.com/library/windows/apps/br224811)-Methode wird gestartet, wenn die App das erste Mal auf [**BackgroundMediaPlayer.Current**](https://msdn.microsoft.com/library/windows/apps/dn652528) im Code zugreift, der in der Vordergrund-App ausgeführt wird, oder wenn Sie einen Handler für das [**MessageReceivedFromBackground**](https://msdn.microsoft.com/library/windows/apps/dn652530)-Ereignis registrieren, je nachdem, welcher Fall zuerst eintritt. Es wird empfohlen, dass Sie sich für den „MessageReceived“-Ereignishandler registrieren, bevor Sie **BackgroundMediaPlayer.Current** das erste Mal aufrufen, damit die Vordergrund-App keine Nachrichten versäumt, die vom Hintergrundprozess gesendet werden.

Damit die Hintergrundaufgabe aktiv bleibt, muss die App eine [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499) aus der **Run**-Methode anfordern und [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504) aufrufen, wenn die Aufgabeninstanz das Ereignis [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) oder [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) empfängt. Verwenden Sie in der **Run**-Methode keine Schleife oder Warten, da dabei Ressourcen verwendet werden. Dies könnte dazu führen, dass die Hintergrundaufgabe der App vom System beendet wird.

Die Hintergrundaufgabe ruft das **Completed**-Ereignis ab, wenn die **Run**-Methode abgeschlossen ist und keine Verzögerung angefordert wurde. In manchen Fällen kann, nachdem die App das **Canceled**-Ereignis abgerufen hat, das **Completed**-Ereignis folgen. Die Aufgabe kann möglicherweise ein **Canceled**-Ereignis empfangen, während **Run** ausgeführt wird, Sie sollten daher sicherstellen, dass diese potenzielle Übereinstimmung verwaltet wird.

Eine Hintergrundaufgabe kann in den folgenden Situationen abgebrochen werden:

-   Eine neue App mit Audiowiedergabefunktionen wird in Systemen gestartet, die die Unterrichtlinie für Exklusivität erzwingen. Weitere Informationen finden Sie im Abschnitt [Systemrichtlinien für die Lebensdauer einer Audiohintergrundaufgabe](#system-policies-for-background-audio-task-lifetime) weiter unten.

-   Eine Hintergrundaufgabe wurde gestartet, aber es wird noch keine Musik wiedergegeben. Anschließend wird die Vordergrund-App angehalten.

-   Andere Medienunterbrechungen, z. B. eingehende Telefonanrufe oder VoIP-Anrufe.

Situationen, in denen die Hintergrundaufgabe ohne vorherige Ankündigung beendet werden kann:

-   Ein VoIP-Anruf geht ein, und es ist nicht genügend Speicherplatz auf dem System vorhanden, um die Hintergrundaufgabe aktiv zu halten.

-   Es wird gegen eine Ressourcenrichtlinie verstoßen.

-   Eine Aufgabe wird nicht korrekt abgebrochen oder abgeschlossen.

## <a name="system-policies-for-background-audio-task-lifetime"></a>Systemrichtlinien für die Lebensdauer einer Audiohintergrundaufgabe

Anhand der folgenden Richtlinien können Sie feststellen, wie das System die Lebensdauer von Audiohintergrundaufgaben verwaltet.

### <a name="exclusivity"></a>Exklusivität

Wenn diese Unterrichtlinie aktiviert ist, wird die Anzahl von Audiohintergrundaufgaben zu einem bestimmten Zeitpunkt auf maximal 1 beschränkt. Sie ist für Mobile- und andere Nicht-Desktop-SKUs aktiviert.

### <a name="inactivity-timeout"></a>Zeitüberschreitung nach Inaktivität

Aufgrund von Ressourcenbeschränkungen kann das System die Hintergrundaufgabe nach einer gewissen Zeit der Inaktivität beenden.

Eine Hintergrundaufgabe gilt als „inaktiv“, wenn beide der folgenden Bedingungen erfüllt sind:

-   Die Vordergrund-App ist nicht sichtbar (sie ist angehalten oder beendet).

-   Der Hintergrund-Media Player befindet sich nicht im Wiedergabezustand.

Wenn beide der folgenden Bedingungen erfüllt sind, startet die Richtlinie für das Hintergrundmediensystem einen Zeitgeber. Wenn sich keine der beiden Bedingungen geändert hat, wenn der Zeitgeber abläuft, beendet die Richtlinie für das Hintergrundmediensystem die Hintergrundaufgabe.

### <a name="shared-lifetime"></a>Freigegebene Lebensdauer

Wenn diese Unterrichtlinie aktiviert ist, wird erzwungen, dass die Hintergrundaufgabe von der Lebensdauer der Vordergrundaufgabe abhängig ist. Wenn die Vordergrundaufgabe entweder durch den Benutzer oder vom System beendet wird, wird auch die Hintergrundaufgabe beendet.

Beachten Sie jedoch, dass dies nicht bedeutet, dass der Vordergrund vom Hintergrund abhängig ist. Wenn die Hintergrundaufgabe beendet wird, wird dadurch nicht erzwungen, dass auch die Vordergrundaufgabe beendet wird.

In der folgenden Tabelle ist aufgeführt, welche Richtlinie auf welchen Gerätetypen erzwungen werden.

| Unterrichtlinie             | Desktop  | Mobilgerät   | Sonstiges    |
|------------------------|----------|----------|----------|
| **Exklusivität**        | Deaktiviert | Aktiviert  | Aktiviert  |
| **Zeitüberschreitung nach Inaktivität** | Deaktiviert | Aktiviert  | Deaktiviert |
| **Freigegebene Lebensdauer**    | Aktiviert  | Deaktiviert | Deaktiviert |


 

 




