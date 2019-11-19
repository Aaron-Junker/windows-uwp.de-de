---
ms.assetid: ''
description: Zeigt, wie Sie die Übertragung von Spielen durch eine UWP-App verwalten.
title: Verwalten der Spielübertragung
ms.date: 09/27/2017
ms.topic: article
keywords: Windows 10, Spiel, Übertragung
ms.localizationpriority: medium
ms.openlocfilehash: 0936312b530ad4480371857851bd155b73e37a50
ms.sourcegitcommit: 49a34e957433966ac8d4822b5822f21087aa61c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/18/2019
ms.locfileid: "74153703"
---
# <a name="manage-game-broadcasting"></a>Verwalten der Spielübertragung
In diesem Artikel wird erläutert, wie Sie die Übertragung von Spielen durch eine UWP-App verwalten. Benutzer müssen die Übertragung von Spielen mithilfe der Systembenutzeroberfläche initiieren, die in Windows integriert ist. Aber ab Version 1709 von Windows 10 können Apps die Übertragungsoberfläche des Systems starten und Benachrichtigungen empfangen, wenn die Übertragung gestartet oder beendet wird.

## <a name="add-the-windows-desktop-extensions-for-the-uwp-to-your-app"></a>Hinzufügen der „Windows Desktop Extensions for the UWP” zu Ihrer App
Die APIs aus dem Namespace **[Windows.Media.AppBroadcasting](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting)** zur Verwaltung der App-Übertragung sind nicht im Universal API-Vertrag enthalten. Für den Zugriff auf die APIs müssen Sie Ihrer App mit den folgenden Schritten einen Verweis auf die „Windows Desktop Extensions for the UWP” hinzufügen.

1. Erweitern Sie im **Projektmappen-Explorer** Ihr UWP-Projekt, klicken Sie mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus. 
2. Erweitern Sie den Knoten **Universal Windows**, und wählen Sie **Erweiterungen** aus.
3. In der Liste der Erweiterungen aktivieren Sie das Kontrollkästchen neben dem Eintrag **Windows Desktop Extensions for the UWP**, der dem Zielbuild für Ihr Projekt entspricht. Die Übertragungsfeatures der App sind ab der Version 1709 verfügbar.
4. Klicken Sie auf **OK**.

## <a name="launch-the-system-ui-to-allow-the-user-to-initiate-broadcasting"></a>Starten Sie die Systembenutzeroberfläche, damit der Benutzer die Übertragung initiieren kann.
Es gibt verschiedene Gründe dafür, dass Ihre App momentan möglicherweise nicht übertragen kann. Beispielsweise könnte das aktuelle Gerät nicht den Hardwareanforderungen für Übertragungen entsprechen, oder eine andere App überträgt gerade Daten. Vor dem Starten der Systembenutzeroberfläche sollten Sie überprüfen, ob Ihre App momentan übertragen kann. Überprüfen Sie zunächst, ob die Übertragungs-APIs auf dem aktuellen Gerät verfügbar sind. Die APIs sind nur auf Geräten verfügbar, auf denen Version 1709 (oder höher) von Windows 10 ausgeführt wird. Statt die Version des Betriebssystems zu überprüfen, können Sie die Methode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** verwenden, um festzustellen, ob Version 1.0 von *Windows.Media.AppBroadcasting.AppBroadcastingContract* vorhanden ist. Wenn dieser Vertrag vorhanden ist, sind die Übertragungs-APIs auf dem Gerät verfügbar.

Als Nächstes rufen eine Instanz der Klasse **[AppBroadcastingUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui)** ab, indem Sie die Factorymethode **[GetDefault](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetDefault)** auf einem PC aufrufen, auf dem nur ein einzelner Benutzer angemeldet ist. Bei der Xbox, in der mehrere Benutzer angemeldet werden können, müssen Sie stattdessen **[getforuser](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.getforuser)** aufrufen. Rufen Sie dann **[GetStatus](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.GetStatus)** auf, um den Übertragungsstatus Ihrer App zu ermitteln.

Über die Eigenschaft **[CanStartBroadcast](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.CanStartBroadcast)** der Klasse **AppBroadcastingStatus** erfahren Sie, ob die App momentan eine Übertragung starten kann. Wenn sie das nicht kann, überprüfen Sie die Eigenschaft **[Details](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus.Details)** , um festzustellen, warum keine Übertragung möglich ist. Je nach Ursache sollten Sie dem Benutzer den Status oder Anweisungen zum Aktivieren der Übertragung anzeigen.

[!code-cpp[CanStartBroadcast](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetCanStartBroadcast)]

Veranlassen Sie, dass die Übertragungsbenutzeroberfläche der App vom System angezeigt wird, indem Sie **[ShowBroadcastUI](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingui.ShowBroadcastUI)** aufrufen.

> [!NOTE] 
> Die Methode **ShowBroadcastUI** ist eine Anforderung, die auch fehlschlagen kann. Der Erfolg ist vom aktuellen Zustand des Systems abhängig. Ihre App sollte nicht davon ausgehen, dass die Übertragung nach dem Aufrufen dieser Methode begonnen hat. Verwenden Sie das Ereignis **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** , um benachrichtigt zu werden, ob die Übertragung startet oder beendet wird.

[!code-cpp[LaunchBroadcastUI](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetLaunchBroadcastUI)]

## <a name="receive-notifications-when-broadcasting-starts-and-stops"></a>Empfangen von Benachrichtigungen beim Starten und Beenden von Übertragungen
Um Benachrichtigungen zu erhalten, wenn der Benutzer eine App-Übertragung über die Systembenutzeroberfläche startet oder beendet, initialisieren Sie eine Instanz der Klasse **[AppBroadcastingMonitor](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)** , und registrieren Sie einen Handler für das Ereignis **[IsCurrentAppBroadcastingChanged](https://docs.microsoft.com/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor.IsCurrentAppBroadcastingChanged)** . Wie im vorherigen Abschnitt erläutert, müssen Sie zu einem bestimmten Zeitpunkt die Methode **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** verwenden, um sicherzustellen, dass die Übertragungs-APIs auf dem Gerät vorhanden sind, bevor Sie versuchen, diese APIs zu verwenden. 

[!code-cpp[AppBroadcastingRegisterChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingRegisterChangedHandler)]

Im Handler für das Ereignis **IsCurrentAppBroadcastingChanged** können Sie die Benutzeroberfläche Ihrer App dem aktuellen Status der Übertragung entsprechend aktualisieren.

[!code-cpp[AppBroadcastingChangedHandler](./code/AppBroadcast/cpp/AppBroadcastExampleApp/App.cpp#SnippetAppBroadcastingChangedHandler)]

## <a name="related-topics"></a>Verwandte Themen

* [Gaming](index.md)

 

 




