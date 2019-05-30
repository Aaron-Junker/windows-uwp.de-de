---
title: Starten einer App auf einem Remotegerät
description: Erfahren Sie, wie Sie mithilfe von Project Rome eine App auf einem Remotegerät starten können.
ms.date: 02/12/2018
ms.topic: article
keywords: Windows 10, Uwp, verbundene Geräte "," Remotesystemen "," ROM "," Projekt "ROME"
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: ac4a5783250f3bd21cb8a3b96a579715830e687d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371715"
---
# <a name="launch-an-app-on-a-remote-device"></a>Starten einer App auf einem Remotegerät

In diesem Artikel wird das Starten einer Windows-App auf einem Remotegerät beschrieben.

Ab Windows 10, Version 1607, kann eine UWP-App eine UWP-App oder Windows-Desktopanwendung remote auf einem anderen Geräte starten, auf dem ebenfalls Windows 10, Version 1607 oder höher, ausgeführt wird. Voraussetzung ist, dass beide Geräte mit dem gleichen Microsoft-Konto (MSA) angemeldet sind. Dies ist der einfachste Anwendungsfall von Project Rome.

Das Feature für den Remotestart ermöglicht aufgabenorientierte Benutzeroberflächen; ein Benutzer kann eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät beenden. Wenn Benutzer beispielsweise auf dem Mobiltelefon in ihrem Auto Musik hören, könnten sie die Wiedergabefunktion anschließend an ihre Xbox One übertragen, wenn sie zu Hause ankommen. Durch Remotestarten können Apps kontextbezogene Daten an die Remote-App übergeben, die gestartet wird, damit diese den Vorgang an der Stelle fortsetzt, an der dieser unterbrochen wurde.

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

### <a name="add-the-remotesystem-capability"></a>Hinzufügen der Funktionen „remoteSystem“

Damit Ihre App eine App auf einem Remotegerät starten kann, müssen Sie Ihrem App-Paketmanifest die Funktion `remoteSystem` hinzufügen. Sie können den Paketmanifest-Designer verwenden, um die Funktion hinzuzufügen, indem Sie auf der Registerkarte **Funktionen** die Option **Remotesystem** auswählen. Sie können auch der Datei _Package.appxmanifest_ Ihres Projekts die folgende Zeile manuell hinzufügen.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Ermöglichen Sie die geräteübergreifende Freigabe

Darüber hinaus muss festgelegt werden, dass das Clientgerät die geräteübergreifende Freigabe zulässt. Diese Einstellung, die in zugegriffen wird **Einstellungen**: **System** > **freigegebene Umgebungen** > **Freigabe geräteübergreifend**, ist standardmäßig aktiviert. 

![Einstellungsseite geteilter Umgebungen](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Suchen eines Remotegeräts

Sie müssen zunächst das Gerät suchen, zu dem Sie eine Verbindung herstellen möchten. Eine detaillierte Anleitung dazu finden Sie unter [Ermitteln von Remotegeräten](discover-remote-devices.md). Wir verwenden hier eine einfache Methode, bei der die Funktionen zum Filtern nach Gerät oder nach Verbindungsart nicht verwendet werden. Wir erstellen ein Überwachungselement, das nach Remotesystemen sucht, und erstellen Handler für die Ereignisse, die ausgelöst werden, wenn Geräte erkannt oder entfernt werden. Auf diese Weise erhalten wir eine Sammlung von Remotegeräten.

Der Code in diesen Beispielen setzt voraus, dass Sie in Ihren Klassendateien über eine `using Windows.System.RemoteSystems`-Anweisung verfügen.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetBuildDeviceList)]

Als Erstes müssen Sie vor einem Remotestart die Funktion `RemoteSystem.RequestAccessAsync()` aufrufen. Überprüfen Sie den Rückgabewert, um sicherzustellen, dass Ihre App auf Remotegeräte zugreifen darf. Diese Überprüfung ist beispielsweise nicht erfolgreich, wenn Sie Ihrer App nicht die Funktion `remoteSystem` hinzugefügt haben.

Die Ereignishandler der Systemüberwachung werden aufgerufen, wenn ein Gerät, mit dem wir eine Verbindung herstellen können, erkannt wird oder nicht mehr verfügbar ist. Wir verwenden diese Ereignishandler, um eine fortlaufend aktualisierte Liste der Geräte zu führen, mit denen wir eine Verbindung herstellen können.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetEventHandlers)]


Wir verfolgen die Geräte nach Remotesystem-ID unter Verwendung eines **Wörterbuchs** nach. Es wird eine **ObservableCollection** verwendet, die eine Liste der enumerierbaren Geräte enthält. Eine **ObservableCollection** vereinfacht auch das Binden der Geräteliste an die UI, auch wenn wir dies in diesem Beispiel nicht ausführen.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetMembers)]

Fügen Sie Ihrem App-Startcode einen Aufruf an `BuildDeviceList()` hinzu, bevor Sie versuchen, eine Remote-App zu starten.

## <a name="launch-an-app-on-a-remote-device"></a>Starten einer App auf einem Remotegerät

Sie starten eine App remote, indem Sie das Gerät, mit dem Sie eine Verbindung herstellen möchten, an die [**RemoteLauncher.LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotelauncher.launchuriasync)-API übergeben. Für diese Methode gibt es drei Überladungen. Die einfachste Überladung, die in diesem Beispiel gezeigt wird, gibt den URI an, der die App auf dem Remotegerät aktiviert. In diesem Beispiel öffnet der URI die Karten-App auf dem Remotecomputer mit einer 3D-Ansicht der Space Needle.

Andere **RemoteLauncher.LaunchUriAsync**-Überladungen ermöglichen die Angabe von Optionen, wie den URI der Website, die angezeigt werden soll, wenn keine entsprechende App auf dem Remotegerät gestartet werden kann, und die Angabe einer optionalen Liste der Paketfamiliennamen, die zum Starten des URIs auf dem Remotegerät verwendet werden können. Sie können auch Daten in Form von Schlüssel-Wert-Paaren bereitstellen. Sie können beispielsweise Daten an die App übergeben, die Sie aktivieren, um einen Kontext für die Remote-App bereitzustellen, etwa den Namen des wiederzugebenden Titels oder die aktuelle Position der Wiedergabe bei der Übergabe von einem Gerät an ein anderes.

In Szenarien in der Praxis können Sie eine Benutzeroberfläche bereitstellen, um das Zielgerät auszuwählen. In diesem Beispiel verwenden wir zur Vereinfachung nur das erste Remotegerät in der Liste.

[!code-cs[Main](./code/RemoteLaunchScenario/MainPage.xaml.cs#SnippetRemoteUriLaunch)]

Das [**RemoteLaunchUriStatus**](https://docs.microsoft.com/uwp/api/windows.system.remotelaunchuristatus)-Objekt, das von **RemoteLauncher.LaunchUriAsync()** zurückgegeben wird, enthält Informationen dazu, ob der Remotestart erfolgreich war. Wenn bei dem Remotestart ein Fehler aufgetreten ist, wird ein Grund angegeben.

## <a name="related-topics"></a>Verwandte Themen

[Remote-Systemen-API-Referenz](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)  
[Verbundene apps und Geräten (Projekt "ROME") (Übersicht)](connected-apps-and-devices.md)  
[Entdecken von Remotegeräten](discover-remote-devices.md)  
Das [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) zeigt die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.
