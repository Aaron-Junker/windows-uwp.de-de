---
ms.assetid: ''
description: Erfahren Sie, wie Sie Game Broadcasting für eine universelle Windows-Plattform-app (UWP) mithilfe der Windows-Systembenutzer Oberfläche und der Windows-Desktop Erweiterungen für UWP verwalten.
title: Verwalten der Spielübertragung
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, Game, Broadcasting
ms.localizationpriority: medium
ms.openlocfilehash: 87c35bb4612ad970f01853b2ace46b44b1882781
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362493"
---
# <a name="manage-game-broadcasting"></a>Verwalten der Spielübertragung
In diesem Artikel erfahren Sie, wie Sie Game Broadcasting für eine UWP-App verwalten. Benutzer müssen die Übertragung mithilfe der in Windows integrierten Systembenutzer Oberfläche initiieren, aber ab Windows 10, Version 1709, können Apps die Benutzeroberfläche für das System Broadcast starten und Benachrichtigungen empfangen, wenn die Übertragung gestartet und beendet wird.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Hinzufügen der Windows-Desktop Erweiterungen für die UWP zu Ihrer APP
Die APIs zum Verwalten von App-Broadcast, die im **[Windows. Media. appbroadcasting](/uwp/api/windows.media.appbroadcasting)** -Namespace enthalten sind, sind nicht im Universal API-Vertrag enthalten. Um auf die APIs zuzugreifen, müssen Sie mit den folgenden Schritten einen Verweis auf die Windows-Desktop Erweiterungen für die UWP zu Ihrer APP hinzufügen.

1. Erweitern Sie in Visual Studio in **Projektmappen-Explorer**das UWP-Projekt, klicken Sie mit der rechten Maustaste auf **Verweise** , und wählen Sie dann **Verweis hinzufügen...** aus. 
2. Erweitern Sie den Knoten **Universal Windows** , und wählen Sie **Erweiterungen**aus.
3. Aktivieren Sie in der Liste der Erweiterungen das Kontrollkästchen neben den **Windows-Desktop Erweiterungen für den UWP-** Eintrag, der mit dem Zielbuild für Ihr Projekt übereinstimmt. Für die APP-Broadcast Features muss die Version 1709 oder höher sein.
4. Klicken Sie auf **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Starten Sie die Systembenutzer Oberfläche, um dem Benutzer das Initiieren von Broadcast zu gestatten
Es gibt mehrere Gründe, warum Ihre APP derzeit nicht übertragen werden kann, z. b., wenn das aktuelle Gerät nicht die Hardwareanforderungen für die Übertragung erfüllt oder wenn derzeit eine andere APP sendet. Vor dem Starten der Systembenutzer Oberfläche können Sie überprüfen, ob Ihre APP derzeit übertragen werden kann. Überprüfen Sie zunächst, ob die Broadcast-APIs auf dem aktuellen Gerät verfügbar sind. Die APIs sind nicht auf Geräten verfügbar, auf denen eine ältere Betriebssystemversion als Windows 10, Version 1709, ausgeführt wird. Anstatt eine bestimmte Betriebssystemversion zu überprüfen, verwenden Sie die Methode **[apiinformation. isapikontratpresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** , um die *Windows. Media. appbroadcasting. appbroadcastingcontract* -Version 1,0 abzufragen. Wenn dieser Vertrag vorhanden ist, sind die Broadcast-APIs auf dem Gerät verfügbar.

Rufen Sie als nächstes eine Instanz der **[AppBroadcastingUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** -Klasse ab, indem Sie die Factorymethode " **[GetDefault](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** " auf dem PC aufrufen, wobei ein einzelner Benutzer gleichzeitig angemeldet ist. Bei der Xbox, in der mehrere Benutzer angemeldet werden können, müssen Sie stattdessen **[getforuser](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** aufrufen. Anschließend können Sie **[GetStatus](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** aufrufen, um den Broadcast Status Ihrer APP zu erhalten.

Die **[canstartbroadcast](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** -Eigenschaft der **appbroadcastingstatus** -Klasse gibt Aufschluss darüber, ob die APP zurzeit mit der Übertragung beginnen kann. Wenn dies nicht der Wert ist, können Sie die Eigenschaft **[Details](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** überprüfen, um zu ermitteln, warum Broadcasting nicht verfügbar ist. Abhängig von der Ursache können Sie den Status des Benutzers anzeigen oder Anweisungen zum Aktivieren der Übertragung anzeigen.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetCanStartBroadcast":::

Fordern Sie an, dass die Benutzeroberfläche der APP-Übertragung vom System durch Aufrufen von **[ShowBroadcastUI](/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** angezeigt wird.

> [!NOTE] 
> Die **ShowBroadcastUI** -Methode stellt eine Anforderung dar, die je nach aktuellem Status des Systems möglicherweise nicht erfolgreich ist. Ihre APP sollte nicht davon ausgehen, dass die Übertragung begonnen hat, nachdem diese Methode aufgerufen wurde. Verwenden Sie das **[iscurrentappbroadcastingchanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** -Ereignis, um benachrichtigt zu werden, wenn die Übertragung startet oder beendet wird.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetLaunchBroadcastUI":::

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Empfangen von Benachrichtigungen beim Starten und Beenden von Sendungen
Registrieren Sie sich für den Empfang von Benachrichtigungen, wenn der Benutzer die Benutzeroberfläche des Systems zum Starten oder Beenden der Übertragung Ihrer APP verwendet, indem eine Instanz der **[appbroadcastingmonitor](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** -Klasse initialisiert und ein Handler für das  **[iscurrentappbroadcastingchanged](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** -Ereignis registriert wird. Wie bereits im vorherigen Abschnitt erläutert, sollten Sie sicherstellen, **[dass die Broadcast](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** -APIs auf dem Gerät vorhanden sind, bevor Sie versuchen, Sie zu verwenden. 

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingRegisterChangedHandler":::

Im Handler für das **iscurrentappbroadcastingchanged** -Ereignis möchten Sie möglicherweise die Benutzeroberfläche Ihrer APP aktualisieren, um den aktuellen Übertragungs Status widerzuspiegeln.

:::code language="cpp" source="~/../snippets-windows/windows-uwp/gaming/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp" id="SnippetAppBroadcastingChangedHandler":::

## <a name="related-topics"></a>Verwandte Themen

* [Spiele](index.md)

 

 
