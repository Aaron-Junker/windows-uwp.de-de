---
author: PatrickFarley
title: Ermitteln von Remotegeräten
description: Erfahren Sie, wie Sie Remotegeräte über Ihre App mit Project Rome ermitteln können.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, verbundenen Geräten, remote-Systemen, "ROME" Projekt "ROME"
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "3665212"
---
# <a name="discover-remote-devices"></a>Ermitteln von Remotegeräten
Ihre App kann die WLAN-, Bluetooth- und Cloud-Verbindung nutzen, um Windows-Geräte zu ermitteln, die mit demselben Microsoft-Konto wie das ermittelnde Gerät angemeldet sind. Auf den Remotegeräten muss keine spezielle Software installiert sein, damit sie erkennbar sind.

> [!NOTE]
> In diesem Handbuch wird davon ausgegangen, dass Ihnen bereits Zugriff auf das Remotesysteme-Feature gewährt wurde, indem Sie die Schritte in [Starten einer Remote-App](launch-a-remote-app.md) befolgt haben.

## <a name="filter-the-set-of-discoverable-devices"></a>Filtern der Gruppe von erkennbaren Geräten
Die Gruppe der erkennbaren Geräte kann mithilfe von [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) mit Filtern eingegrenzt werden. Filter können den Ermittlungstyp (proximales gegenüber lokales Netzwerk gegenüber Cloud-Verbindung), Gerätetyp (Desktop, Mobilgerät, Xbox, Hub und Hologramm) und den Verfügbarkeitsstatus (den Status der Verfügbarkeit eines Geräts für die Verwendung von Remotesystem-Eigenschaften) erkennen.

Filter-Objekte müssen erstellt werden, bevor oder während das **RemoteSystemWatcher**-Objekt initialisiert wird, da sie als Parameter an den Konstruktor übergeben werden. Der folgende Code erstellt einen Filter von jedem verfügbaren Typ und fügt diese anschließend einer Liste hinzu.

> [!NOTE]
> Der Code in diesen Beispielen setzt voraus, dass Sie in Ihrer Datei über eine `using Windows.System.RemoteSystems`-Anweisung verfügen.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> Der Filterwert „proximal“ garantiert den Grad der physischen Näherung nicht. Verwenden Sie für Szenarien, für die eine zuverlässige physische Näherung erforderlich ist, den Wert [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) in Ihrem Filter. Derzeit erlaubt dieser Filter nur Geräte, die von Bluetooth erkannt werden können. Neue Erkennungsmethoden, die eine physische Näherung garantieren können, werden hier ebenfalls berücksichtigt, sobald diese unterstützt werden.  
Zudem ist in der [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)-Klasse eine Eigenschaft enthalten, die angibt, ob sich ein erkanntes Gerät tatsächlich physisch in der Nähe befindet: [**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Wenn Sie beabsichtigen, Geräte über ein lokales Netzwerk zu ermitteln (bestimmt durch die Auswahl des Filters „Ermittlungstyp“), muss Ihr Netzwerk das Profil „Privat“ oder „Domäne“ aufweisen. Ihr Gerät ermittelt andere Geräte nicht über ein „öffentliches“ Netzwerk.

Sobald eine Liste mit [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter)-Objekten erstellt wurde, kann diese an den Konstruktor eines **RemoteSystemWatcher**übergeben werden.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Wenn die [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start)-Methode dieses Überwachungselements aufgerufen wird, wird das [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded)-Ereignis nur dann ausgelöst, wenn ein Gerät erkannt wird, das alle der folgenden Kriterien erfüllt:
* Es kann von einer proximalen Verbindung erkannt werden
* Es ist ein Desktop oder ein Telefon
* Es wird als verfügbar klassifiziert

Ab diesem Punkt ist die Vorgehensweise zum Behandeln von Ereignissen, Abrufen von [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem)-Objekten und Herstellen einer Verbindung mit Remotegeräten die gleiche wie in [Starten einer Remote-App](launch-a-remote-app.md). Kurz gesagt: Die **RemoteSystem**-Objekte werden als Eigenschaften von [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs)-Objekten gespeichert, die mit jedem **RemoteSystemAdded**-Ereignis übergeben werden.

## <a name="discover-devices-by-address-input"></a>Ermitteln von Geräten durch Adresseingabe
Einige Geräte sind möglicherweise nicht mit einem Benutzer verknüpft oder durch eine Überprüfung erkennbar. Sie können jedoch trotzdem erreicht werden, wenn die ermittelnde App eine direkte Adresse verwendet. Die [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx)-Klasse wird verwendet, um die Adresse eines Remotegeräts darzustellen. Dies wird häufig in Form einer IP-Adresse gespeichert, jedoch sind auch verschiedene andere Formate zulässig (weitere Informationen finden Sie unter [**HostName-Konstruktor**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx).

Ein **RemoteSystem**-Objekt wird abgerufen, wenn ein gültiges **HostName**-Objekt bereitgestellt wird. Wenn die Adressdaten ungültig sind, wird ein `null`-Objektverweis zurückgegeben.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Abfragen einer Funktion auf einem Remotesystem

Auch wenn das Abfragen von Gerätefunktionen unabhängig von der Erkennungsfilterung ist, kann es dennoch wichtiger Teil des Erkennungsvorgangs sein. Mit der [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync)-Methode, können Sie erkannte Remotesysteme abfragen, zur Unterstützung bestimmter Funktionen wie der Konnektivität von Remotesitzungen oder dem Teilen räumlicher Instanzen (holografisch). Eine Liste der abfragbaren Funktionen finden Sie unter der [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities)-Klasse.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Erkennung von Cross-Benutzern

Entwickler können festlegen, dass _alle_ Geräte, die sich in der Nähe des Clientgeräts befinden, erkannt werden und nicht nur diejenigen Geräte, die auf denselben Benutzer registriert sind. Dies wird implementiert durch einen speziellen **IRemoteSystemFilter**, [**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter). Die Implementierung erfolgt auf die gleiche Weise, wie für andere Filtertypen:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Ist der [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind)-Wert auf **Anonym** eingestellt, erlaubt dieser die Erkennung aller proximalen Geräte, auch derjenigen, von nicht vertrauenswürdigen Benutzern.
* Der Wert **SameUser** filtert die Erkennung so, dass nur Geräte erkannt werden, die auf denselben Benutzer registriert sind wie das Client-Gerät. Hierbei handelt es sich um das standardmäßige Verhalten.

### <a name="checking-the-cross-user-sharing-settings"></a>Überprüfen der Einstellungen für Cross-Benutzer-Sharing

Zusätzlich dazu, dass der oben genannte Filter spezifisch in Ihrer Erkennungs-App enthalten ist, muss das Client-Gerät selbst so konfiguriert sein, dass geteilte Umgebungen von Geräten, die über andere Benutzer angemeldet sind, zugelassen werden. Dies ist eine Systemeinstellung, die über eine statische Methode in der **RemoteSystem**-Klasse abgefragt werden kann:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Um diese Einstellung zu ändern, muss der Benutzer die Anwendung **Einstellungen** öffnen. Im Menü **System** > **Geteilte Umgebungen** > **Auf Geräten freigeben** befindet sich ein Dropdown-Feld, in dem der Benutzer angeben kann, mit welchen Geräten sein System teilen kann.

![Einstellungsseite geteilter Umgebungen](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Verwandte Themen
* [Verbundene Apps und Geräte (Project Rome)](connected-apps-and-devices.md)
* [Starten einer Remote-App](launch-a-remote-app.md)
* [API-Referenz für Remotesysteme](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
