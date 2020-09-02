---
title: Starten einer App auf einem Remotegerät
description: Erfahren Sie, wie Sie eine UWP-APP oder eine Windows-Desktop Anwendung Remote auf einem anderen Gerät starten, sodass ein Benutzer eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät beenden kann.
ms.date: 02/12/2018
ms.topic: article
keywords: Windows 10, UWP, verbundene Geräte, Remote Systeme, Rom, Project Rom
ms.assetid: 54f6a33d-a3b5-4169-8664-653dbab09175
ms.localizationpriority: medium
ms.openlocfilehash: 4163106c5439ec8881c1b5042f63fb7abf4fd668
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362503"
---
# <a name="launch-an-app-on-a-remote-device"></a>Starten einer App auf einem Remotegerät

In diesem Artikel wird das Starten einer Windows-App auf einem Remotegerät beschrieben.

Ab Windows 10, Version 1607, kann eine UWP-App eine UWP-App oder Windows-Desktopanwendung remote auf einem anderen Geräte starten, auf dem ebenfalls Windows 10, Version 1607 oder höher, ausgeführt wird. Voraussetzung ist, dass beide Geräte mit dem gleichen Microsoft-Konto (MSA) angemeldet sind. Dies ist der einfachste Anwendungsfall von Project Rom.

Das Remote Start Feature ermöglicht aufgabenorientierte Benutzeroberflächen. ein Benutzer kann eine Aufgabe auf einem Gerät starten und auf einem anderen Gerät Fertigstellen. Wenn der Benutzer z. b. Musik an seinem Telefon in seinem Auto abhört, könnte er die Wiedergabe Funktionalität an die Xbox 1 Hand geben, wenn er zu Hause kommt. Remote Start ermöglicht apps das Übergeben von Kontext Daten an die Remote-app, die gestartet wird, um den Speicherort der Aufgabe aufzurufen.

## <a name="preliminary-setup"></a>Vorläufige Einrichtung

### <a name="add-the-remotesystem-capability"></a>Hinzufügen der Funktionen „remoteSystem“

Damit Ihre App eine App auf einem Remotegerät starten kann, müssen Sie Ihrem App-Paketmanifest die Funktion `remoteSystem` hinzufügen. Sie können den Paket Manifest-Designer verwenden, um ihn hinzuzufügen, indem Sie auf der Registerkarte " **Funktionen** " **Remote System** auswählen, oder Sie können die folgende Zeile manuell der Datei " _Package. appxmanifest_ " des Projekts hinzufügen.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-device-sharing"></a>Aktivieren der Geräte übergreifenden Freigabe

Außerdem muss das Client Gerät so festgelegt werden, dass es die Geräte übergreifende Freigabe zulässt. Diese Einstellung ist standardmäßig aktiviert und wird in den **Einstellungen**: **System**  >  **Shared Erlebnisse**  >  **auf Geräten**gemeinsam genutzt. 

![Seite mit Einstellungen für freigegebene Erfahrungen](images/shared-experiences-settings.png)

## <a name="find-a-remote-device"></a>Suchen eines Remotegeräts

Sie müssen zunächst das Gerät suchen, zu dem Sie eine Verbindung herstellen möchten. Eine detaillierte Anleitung dazu finden Sie unter [Ermitteln von Remotegeräten](discover-remote-devices.md). Wir verwenden hier eine einfache Methode, bei der die Funktionen zum Filtern nach Gerät oder nach Verbindungsart nicht verwendet werden. Wir erstellen ein Überwachungselement, das nach Remotesystemen sucht, und erstellen Handler für die Ereignisse, die ausgelöst werden, wenn Geräte erkannt oder entfernt werden. Auf diese Weise erhalten wir eine Sammlung von Remotegeräten.

Der Code in diesen Beispielen erfordert, dass Sie über eine- `using Windows.System.RemoteSystems` Anweisung in der Klassendatei (en) verfügen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetBuildDeviceList":::

Als Erstes müssen Sie vor einem Remotestart die Funktion `RemoteSystem.RequestAccessAsync()` aufrufen. Überprüfen Sie den Rückgabewert, um sicherzustellen, dass Ihre App auf Remotegeräte zugreifen darf. Diese Überprüfung ist beispielsweise nicht erfolgreich, wenn Sie Ihrer App nicht die Funktion `remoteSystem` hinzugefügt haben.

Die Ereignishandler der Systemüberwachung werden aufgerufen, wenn ein Gerät, mit dem wir eine Verbindung herstellen können, erkannt wird oder nicht mehr verfügbar ist. Wir verwenden diese Ereignishandler, um eine fortlaufend aktualisierte Liste der Geräte zu führen, mit denen wir eine Verbindung herstellen können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetEventHandlers":::


Wir verfolgen die Geräte nach Remotesystem-ID unter Verwendung eines **Wörterbuchs** nach. Eine **ObservableCollection** dient zum Speichern der Liste der Geräte, die aufgelistet werden können. Mit **ObservableCollection** ist es auch einfach, die Liste der Geräte an die Benutzeroberfläche zu binden, obwohl dies in diesem Beispiel nicht der Fall ist.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetMembers":::

Fügen Sie Ihrem App-Startcode einen Aufruf an `BuildDeviceList()` hinzu, bevor Sie versuchen, eine Remote-App zu starten.

## <a name="launch-an-app-on-a-remote-device"></a>Starten einer App auf einem Remotegerät

Sie starten eine App remote, indem Sie das Gerät, mit dem Sie eine Verbindung herstellen möchten, an die [**RemoteLauncher.LaunchUriAsync**](/uwp/api/windows.system.remotelauncher.launchuriasync)-API übergeben. Für diese Methode gibt es drei Überladungen. Die einfachste Überladung, die in diesem Beispiel gezeigt wird, gibt den URI an, der die App auf dem Remotegerät aktiviert. In diesem Beispiel öffnet der URI die Karten-App auf dem Remotecomputer mit einer 3D-Ansicht der Space Needle.

Andere **RemoteLauncher.LaunchUriAsync**-Überladungen ermöglichen die Angabe von Optionen, wie den URI der Website, die angezeigt werden soll, wenn keine entsprechende App auf dem Remotegerät gestartet werden kann, und die Angabe einer optionalen Liste der Paketfamiliennamen, die zum Starten des URIs auf dem Remotegerät verwendet werden können. Sie können auch Daten in Form von Schlüssel-Wert-Paaren bereitstellen. Sie können beispielsweise Daten an die App übergeben, die Sie aktivieren, um einen Kontext für die Remote-App bereitzustellen, etwa den Namen des wiederzugebenden Titels oder die aktuelle Position der Wiedergabe bei der Übergabe von einem Gerät an ein anderes.

In Szenarien in der Praxis können Sie eine Benutzeroberfläche bereitstellen, um das Zielgerät auszuwählen. In diesem Beispiel verwenden wir zur Vereinfachung nur das erste Remotegerät in der Liste.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteLaunchScenario/cs/MainPage.xaml.cs" id="SnippetRemoteUriLaunch":::

Das [**RemoteLaunchUriStatus**](/uwp/api/windows.system.remotelaunchuristatus)-Objekt, das von **RemoteLauncher.LaunchUriAsync()** zurückgegeben wird, enthält Informationen dazu, ob der Remotestart erfolgreich war. Wenn bei dem Remotestart ein Fehler aufgetreten ist, wird ein Grund angegeben.

## <a name="related-topics"></a>Verwandte Themen

[API-Referenz für Remotesysteme](/uwp/api/Windows.System.RemoteSystems)  
[Übersicht über verbundene apps und Geräte (Project Rom)](connected-apps-and-devices.md)  
[Entdecken von Remotegeräten](discover-remote-devices.md)  
Das [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems) zeigt die Vorgehensweise zum Erkennen eines Remotesystems, Starten einer App auf einem Remotesystem und Verwenden von App-Diensten zum Senden von Nachrichten zwischen Apps, die auf zwei Systemen ausgeführt werden.
