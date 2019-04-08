---
title: Kommunizieren mit einem App-Remotedienst
description: Tauschen Sie Nachrichten mit einem App-Dienst aus, der auf einem Remotegerät mit Project Rome ausgeführt wird.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, verbundene Geräte, Remotesysteme, "ROME", Projekt "ROME", Hintergrundtasks, app service
ms.localizationpriority: medium
ms.openlocfilehash: ddadae05ca3243f9bbd6b53cbb98f234ac560acd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612935"
---
# <a name="communicate-with-a-remote-app-service"></a>Kommunizieren mit einem App-Remotedienst

Sie können nicht nur eine App auf einem Remotegerät mithilfe eines URI starten, sondern auch *App-Dienste* auf Remotegeräten ausführen und mit ihnen kommunizieren. Jedes Windows-basierte Gerät kann als Client- oder Hostgerät verwendet werden. Dies bietet Ihnen eine nahezu unbegrenzte Anzahl von Möglichkeiten zur Interaktion mit verbundenen Geräten, ohne eine App in den Vordergrund bringen zu müssen.

## <a name="set-up-the-app-service-on-the-host-device"></a>Einrichten des App-Diensts auf dem Hostgerät
Um einen App-Dienst auf einem Remotegerät ausführen zu können, muss bereits ein Anbieter dieses App-Diensts auf dem Hostgerät installiert sein. In dieser Anleitung wird die CSharp-Version des [Beispiels für den App-Dienst für die Generierung von Zufallszahlen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices) verwendet, das im [universellen Windows-Beispielrepository](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices) verfügbar ist. Anleitungen zum Schreiben eines eigenen App-Diensts finden Sie unter [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md).

Gleichgültig, ob Sie einen bereits erstellten App-Dienst verwenden oder einen eigenen schreiben, Sie müssen einige Änderungen vornehmen, um den Dienst mit Remotesystemen kompatibel zu machen. Wechseln Sie in Visual Studio zum Projekt des App-Dienstanbieters (im Beispiel „AppServicesProvider”genannt), und wählen Sie die Datei _Package.appxmanifest_ aus. Klicken Sie mit der rechten Maustaste, und wählen Sie **Code anzeigen** aus, um den gesamten Inhalt der Datei anzuzeigen. Erstellen Sie eine **Erweiterungen** -Element innerhalb der Hauptseite **Anwendung** Element (oder suchen sie nach, wenn sie bereits vorhanden ist). Erstellen Sie dann eine **Erweiterung** definieren das Projekt als app Service und seine übergeordneten Projekt zu verweisen.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Neben der **AppService** -Element, fügen die **SupportsRemoteSystems** Attribut:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Um Elemente zu verwenden, in diesem **uap3** -Namespace, müssen Sie Hinzufügen der Namespacedefinition am Anfang der manifest-Datei, wenn er nicht bereits vorhanden ist.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Anschließend erstellen Sie Ihr app Service-Anbieter-Projekt, und für die Host-Geräte bereitstellen.

## <a name="target-the-app-service-from-the-client-device"></a>Aufrufen des App-Diensts vom Clientgerät
Das Gerät, von dem der App-Remotedienst aufgerufen werden soll, muss über eine App mit Remotesystemfunktionen verfügen. Diese können der gleichen App hinzugefügt werden, die den App-Dienst auf dem Hostgerät bereitstellt (in diesem Fall installieren Sie gleiche App auf beiden Geräten), oder in einer völlig anderen App implementiert werden.

Die folgenden **using**-Anweisungen werden für den Code in diesem Abschnitt benötigt, damit er wie gezeigt ausgeführt wird:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Instanziieren Sie zunächst ein [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection)-Objekt, als ob Sie einen App-Dienst lokal aufrufen würden. Dieser Vorgang wird in [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md) ausführlicher behandelt. In diesem Beispiel ist der aufzurufende App-Dienst der Dienst für die Generierung von Zufallszahlen.

> [!NOTE]
> Es wird angenommen, dass ein [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)-Objekt bereits innerhalb des Codes geladen wurde, der die folgende Methode aufruft. Anleitungen dazu, wie Sie dies einrichten können, finden Sie unter [Starten einer Remote-App](launch-a-remote-app.md).

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Als Nächstes wird ein [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest)-Objekt für das vorgesehene Remotegerät erstellt. Es wird dann zum Öffnen der **AppServiceConnection** zu diesem Gerät verwendet. Beachten Sie, dass im folgenden Beispiel Fehlerbehandlung und Berichte erheblich vereinfacht dargestellt werden, um das Beispiel kurz zu halten.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

An diesem Punkt sollten Sie über eine offene Verbindung zu einem App-Dienst auf einem Remotecomputer verfügen.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Exchange-Dienst-spezifische Nachrichten über die Remoteverbindung

Hier können Sie Nachrichten in Form von [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset)-Objekten an den Dienst senden und von dem Dienst empfangen (weitere Informationen finden Sie unter [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md)). Der Dienst für die Generierung von Zufallszahlen verwendet zwei Ganzzahlen mit den Schlüsseln `"minvalue"` und `"maxvalue"` als Eingaben, wählt nach dem Zufallsprinzip innerhalb dieses Bereichs eine Ganzzahl aus und gibt diese an den aufrufenden Prozess mit dem Schlüssel `"Result"` zurück.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Sie haben jetzt eine Verbindung zu einem App-Dienst auf einem aufgerufenen Hostgerät hergestellt, einen Vorgang auf diesem Gerät ausgeführt und auf dem Clientgerät als Antwort Daten empfangen.

## <a name="related-topics"></a>Verwandte Themen

[Verbundene apps und Geräten (Projekt "ROME") (Übersicht)](connected-apps-and-devices.md)  
[Starten Sie eine remote-app](launch-a-remote-app.md)  
[Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md)  
[Remote-Systemen-API-Referenz](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
