---
title: Entdecken von Remotegeräten
description: Erfahren Sie, wie Sie Remote Geräte mithilfe von Project Rom aus Ihrer APP ermitteln.
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, verbundene Geräte, Remote Systeme, Rom, Project Rom
ms.localizationpriority: medium
ms.openlocfilehash: 01c13a30c8869643badc69c546b0a5212308956f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155924"
---
# <a name="discover-remote-devices"></a>Entdecken von Remotegeräten
Ihre APP kann das Drahtlos Netzwerk, Bluetooth und die cloudverbindung zum Ermitteln von Windows-Geräten verwenden, die mit dem gleichen Microsoft-Konto wie das ermittelende Gerät angemeldet sind. Auf den Remotegeräten muss keine spezielle Software installiert sein, damit sie erkennbar sind.

> [!NOTE]
> In diesem Handbuch wird davon ausgegangen, dass Ihnen bereits Zugriff auf das Remotesysteme-Feature gewährt wurde, indem Sie die Schritte in [Starten einer Remote-App](launch-a-remote-app.md) befolgt haben.

## <a name="filter-the-set-of-discoverable-devices"></a>Filtern der Gruppe von erkennbaren Geräten
Sie können den Satz der ermittelbaren Geräte eingrenzen, indem Sie einen [**remotesystemwatcher**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemWatcher) mit Filtern verwenden. Filter können den Ermittlungstyp (annähernd das lokale Netzwerk und die cloudverbindung), den Gerätetyp (Desktop, mobiles Gerät, Xbox, Hub und Holographic) und den Verfügbarkeitsstatus (den Status der Verfügbarkeit eines Geräts für die Verwendung von Remote System Features) erkennen.

Filter Objekte müssen vor oder während der Initialisierung des **remotesystemwatcher** -Objekts erstellt werden, da Sie als Parameter an ihren Konstruktor übergeben werden. Der folgende Code erstellt einen Filter von jedem verfügbaren Typ und fügt sie anschließend einer Liste hinzu.

> [!NOTE]
> Der Code in diesen Beispielen erfordert, dass Sie über eine- `using Windows.System.RemoteSystems` Anweisung in der Datei verfügen.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> Der Filter Wert "proximal" garantiert nicht den Grad der physischen Nähe. Verwenden Sie für Szenarien, die eine zuverlässige physische Nähe erfordern, den Wert [**remotesystemdiscoverytype. spatiallyproximal**](/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) in Ihrem Filter. Derzeit sind mit diesem Filter nur Geräte zulässig, die von Bluetooth erkannt werden. Da neue Ermittlungs Mechanismen und Protokolle unterstützt werden, die die physische Nähe gewährleisten, werden Sie hier ebenfalls eingeschlossen.  
Es gibt auch eine Eigenschaft in der [**Remotesystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) -Klasse, die angibt, ob ein ermitteltes Gerät tatsächlich innerhalb der physischen Nähe ist: [**Remotesystem. isavailablebyspatialnear**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity).

> [!NOTE]
> Wenn Sie beabsichtigen, Geräte über ein lokales Netzwerk zu ermitteln (festgelegt durch die Filter Auswahl des Suchtyps), muss Ihr Netzwerk ein "privates" oder "Domänen Profil" verwenden. Ihr Gerät erkennt andere Geräte nicht über ein öffentliches Netzwerk.

Sobald eine Liste mit [**IRemoteSystemFilter**](/uwp/api/Windows.System.RemoteSystems.IRemoteSystemFilter)-Objekten erstellt wurde, kann sie an den Konstruktor eines **RemoteSystemWatcher**übergeben werden.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

Wenn die [**Start**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.start)-Methode dieses Überwachungselements aufgerufen wird, wird das [**RemoteSystemAdded**](/uwp/api/windows.system.remotesystems.remotesystemwatcher.remotesystemadded)-Ereignis nur dann ausgelöst, wenn ein Gerät erkannt wird, das alle der folgenden Kriterien erfüllt:
* Es kann von einer proximalen Verbindung erkannt werden
* Es ist ein Desktop oder ein Telefon
* Es wird als verfügbar klassifiziert

Ab diesem Punkt ist die Vorgehensweise zum Behandeln von Ereignissen, Abrufen von [**RemoteSystem**](/uwp/api/Windows.System.RemoteSystems.RemoteSystem)-Objekten und Herstellen einer Verbindung mit Remotegeräten die gleiche wie in [Starten einer Remote-App](launch-a-remote-app.md). Kurz gesagt werden die **Remotesystem** -Objekte als Eigenschaften von [**remotesystemaddebuargs**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) -Objekten gespeichert, die mit jedem **remotesystemadded** -Ereignis übermittelt werden.

## <a name="discover-devices-by-address-input"></a>Ermitteln von Geräten durch Adresseingabe
Einige Geräte sind möglicherweise nicht mit einem Benutzer verknüpft oder durch eine Überprüfung erkennbar. Sie können jedoch trotzdem erreicht werden, wenn die ermittelnde App eine direkte Adresse verwendet. Die [**HostName**](/uwp/api/windows.networking.hostname)-Klasse wird verwendet, um die Adresse eines Remotegeräts darzustellen. Dies wird häufig in Form einer IP-Adresse gespeichert, jedoch sind auch verschiedene andere Formate zulässig (weitere Informationen finden Sie unter [**HostName-Konstruktor**](/uwp/api/windows.networking.hostname.-ctor).

Ein **RemoteSystem**-Objekt wird abgerufen, wenn ein gültiges **HostName**-Objekt bereitgestellt wird. Wenn die Adressdaten ungültig sind, wird ein `null`-Objektverweis zurückgegeben.

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>Abfragen einer Funktion auf einem Remote System

Obwohl die Ermittlung von Ermittlungs Filtern getrennt ist, kann das Abfragen von Gerätefunktionen ein wichtiger Bestandteil des Ermittlungs Prozesses sein. Mithilfe der [**Remotesystem. getcapabilitysupportedasync**](/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) -Methode können Sie erkannte Remote Systeme Abfragen, um bestimmte Funktionen zu unterstützen, z. b. Remote Sitzungs Konnektivität oder die Freigabe räumlicher Entitäten (Holographic). Eine Liste der abfragbaren Funktionen finden Sie unter der [**knownremotesystemfunktionalitäten**](/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) -Klasse.

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>Benutzer übergreifende Ermittlung

Entwickler können die Ermittlung _aller_ Geräte in der Nähe des Client Geräts angeben, nicht nur Geräte, die für denselben Benutzer registriert sind. Dies wird über einen speziellen **iremotesystemfilter**, [**remotesystemauthorizationkindfilter**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter), implementiert. Es ist wie die anderen Filtertypen implementiert:

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* Ein [**remotesystemauthorizationkind**](/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) -Wert von **Anonymous** ermöglicht die Ermittlung aller nahen Geräte, auch derjenigen von nicht vertrauenswürdigen Benutzern.
* Ein Wert von **sameuser** filtert die Ermittlung auf Geräte, die für denselben Benutzer wie das Client Gerät registriert sind. Dies ist das Standardverhalten.

### <a name="checking-the-cross-user-sharing-settings"></a>Überprüfen der Benutzer übergreifenden Freigabe Einstellungen

Zusätzlich zu dem oben beschriebenen Filter, der in ihrer Ermittlungs-App angegeben wird, muss auch das Client Gerät selbst so konfiguriert werden, dass es freigegebene Benutzeroberflächen von Geräten zulässt, die mit anderen Benutzern angemeldet sind. Dies ist eine Systemeinstellung, die mit einer statischen Methode in der **Remotesystem** -Klasse abgefragt werden kann:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

Um diese Einstellung zu ändern, muss der Benutzer die app " **Einstellungen** " öffnen. Im Menü für die gemeinsame Nutzung von **System**Freigaben  >  **Shared experiences**  >  **über Geräte** gibt es eine Dropdown Liste, in der der Benutzer angeben kann, mit welchen Geräten das System gemeinsam genutzt werden kann.

![Seite mit Einstellungen für freigegebene Erfahrungen](images/shared-experiences-settings.png)

## <a name="related-topics"></a>Zugehörige Themen
* [Verbundene Apps und Geräte (Projekt „Rome”)](connected-apps-and-devices.md)
* [Starten einer Remote-App](launch-a-remote-app.md)
* [API-Referenz für Remotesysteme](/uwp/api/Windows.System.RemoteSystems)
* [Beispiel für Remotesysteme](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)