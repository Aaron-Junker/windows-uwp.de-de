---
title: Prozessübergreifende Kommunikation (Interprocess Communication, IPC)
description: In diesem Thema werden verschiedene Möglichkeiten für die Durchführung der prozessübergreifenden Kommunikation (IPC) zwischen universelle Windows-Plattform Anwendungen (UWP) und Win32-Anwendungen erläutert.
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256380"
---
# <a name="interprocess-communication-ipc"></a>Prozessübergreifende Kommunikation (Interprocess Communication, IPC)

In diesem Thema werden verschiedene Möglichkeiten für die Durchführung der prozessübergreifenden Kommunikation (IPC) zwischen universelle Windows-Plattform Anwendungen (UWP) und Win32-Anwendungen erläutert.

## <a name="app-services"></a>App-Dienste

Mit App Services können Anwendungen Dienste verfügbar machen, die Eigenschaften Behälter von primitiven ([**valueset**](/uwp/api/Windows.Foundation.Collections.ValueSet)) im Hintergrund akzeptieren und zurückgeben. Rich-Objekte können bei der [Serialisierung](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)übermittelt werden.

App-Dienste können entweder [außerhalb des Prozesses](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) als Hintergrundaufgabe [oder innerhalb der](/windows/uwp/launch-resume/convert-app-service-in-process) Vordergrund Anwendung ausgeführt werden.

App Services werden am besten für die Freigabe kleiner Datenmengen verwendet, bei denen nahezu in Echtzeit Latenzzeiten nicht erforderlich sind.

## <a name="com"></a>COM

[Com](/windows/win32/com/component-object-model--com--portal) ist ein verteiltes objektorientiertes System zum Erstellen von binären Softwarekomponenten, die interagieren und kommunizieren können. Als Entwickler verwenden Sie com, um wiederverwendbare Softwarekomponenten und Automatisierungsebenen für eine Anwendung zu erstellen. COM-Komponenten können sich im Prozess befinden oder außerhalb des Prozesses befinden und über ein [Client-und Server](/windows/win32/com/com-clients-and-servers) Modell kommunizieren. Out-of-Process-COM-Server wurden lange als Mittel für die [Kommunikation zwischen Objekten](/windows/win32/com/inter-object-communication)verwendet.

Gepackte Anwendungen mit der Funktion [runfulltrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) können prozessübergreifende com-Server für IPC über das [Paket Manifest](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)registrieren. Dies wird als [Paket-com](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)bezeichnet.

## <a name="filesystem"></a>Dateisystem

### <a name="broadfilesystemaccess"></a>Broadfilesystemaccess

Paketanwendungen können IPC mithilfe des großen Dateisystems ausführen, indem Sie die eingeschränkte [broadfilesystemaccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations) -Funktion deklarieren. Diese Funktion gewährt [Windows. Storage](/uwp/api/Windows.Storage) -APIs und [xxxfromapp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32-APIs Zugriff auf das umfangreiche Dateisystem.

Standardmäßig ist IPC über das Dateisystem für gepackte Anwendungen auf die anderen in diesem Abschnitt beschriebenen Mechanismen beschränkt.

### <a name="publishercachefolder"></a>Publishercachefolder

Mit dem [Ordner "publishercachefolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder) " können Paketanwendungen Ordner in ihrem Manifest deklarieren, die von demselben Verleger gemeinsam mit anderen Paketen gemeinsam verwendet werden können.

Für den freigegebenen Speicherordner gelten die folgenden Anforderungen und Einschränkungen:

* Die Daten im freigegebenen Speicherordner werden nicht gesichert oder rostet.
* Der Benutzer kann den Inhalt des freigegebenen Speicher Ordners löschen.
* Sie können den freigegebenen Speicherordner nicht verwenden, um Daten von verschiedenen Verlegern für Anwendungen freizugeben.
* Sie können den freigegebenen Speicherordner nicht verwenden, um Daten für verschiedene Benutzer freizugeben.
* Der freigegebene Speicherordner verfügt nicht über die Versionsverwaltung.

Wenn Sie mehrere Anwendungen veröffentlichen und einen einfachen Mechanismus für die gemeinsame Nutzung von Daten suchen, ist publishercachefolder eine einfache Dateisystem basierte Option.

### <a name="sharedaccessstoragemanager"></a>Sharedaccessstoragemanager

[Sharedaccessstoragemanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) wird zusammen mit App Services, Protokoll Aktivierungen (z. b. launchuriforresultasync) usw. verwendet, um storagefiles über Token freizugeben.

## <a name="fulltrustprocesslauncher"></a>Fulltrustprocesslauncher

Mit der Funktion " [runfulltrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities) " können gepackte Anwendungen [voll vertrauenswürdige Prozesse](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher) innerhalb desselben Pakets starten.

Für Szenarien, in denen Paket Einschränkungen eine Belastung darstellen, oder wenn IPC-Optionen fehlen, könnte eine Anwendung einen voll vertrauenswürdigen Prozess als Proxy für die Schnittstelle mit dem System verwenden, und dann mit dem voll vertrauenswürdigen Prozess selbst über APP Services oder einen anderen gut unterstützten IPC-Mechanismus.

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[Launchuriforresultasync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results) wird für einen einfachen ([valueset](/uwp/api/Windows.Foundation.Collections.ValueSet)-) Datenaustausch mit anderen verpackten Anwendungen verwendet, die den [protocolforresults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results) -Aktivierungs Vertrag implementieren. Im Unterschied zu app-Diensten, die in der Regel im Hintergrund ausgeführt werden, wird die Zielanwendung im Vordergrund gestartet.

Dateien können freigegeben werden, indem [sharedstorageaccessmanager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) -Token über das valueset an die Anwendung übergeben werden.

## <a name="loopback"></a>Loopback

Loopback ist der Prozess der Kommunikation mit einem Netzwerkserver, der auf localhost lauscht (die Loopback Adresse).

Um die Sicherheit und die Netzwerk Isolation aufrechtzuerhalten, werden Loopback Verbindungen für IPC standardmäßig für gepackte Anwendungen blockiert. Sie können Loopback Verbindungen zwischen vertrauenswürdigen Paketen mithilfe von [Funktionen](/previous-versions/windows/apps/hh770532(v=win.10)) und [Manifest-Eigenschaften](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)aktivieren.

* Alle an Loopback Verbindungen beteiligten Paketanwendungen müssen die `privateNetworkClientServer` Funktion in Ihren [Paket Manifesten](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)deklarieren.
* Zwei gepackte Anwendungen können über Loopbacks kommunizieren, indem [loopbackaccessrules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) innerhalb ihrer Paket Manifeste deklariert werden.
    * Jede Anwendung muss die andere in den [loopbackaccessrules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)auflisten. Der Client deklariert eine "out"-Regel für den Server, und der Server deklariert "in"-Regeln für die unterstützten Clients.

> [!NOTE]
> Der Paket Familienname, der zum Identifizieren einer Anwendung in diesen Regeln erforderlich ist, kann während der Entwicklungszeit über den Paket Manifest-Editor in Visual Studio gefunden werden, über [Partner Center](/windows/uwp/publish/view-app-identity-details) für Anwendungen, die über das Microsoft Store veröffentlicht werden, oder über den PowerShell [-Befehl Get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) für bereits installierte Anwendungen.

Nicht gepackte Anwendungen und Dienste verfügen über keine Paket Identität und können daher nicht in [loopbackaccessrules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)deklariert werden. Sie können eine gepackte Anwendung so konfigurieren, dass eine Verbindung über Loopback mit nicht verpackten Anwendungen und Diensten über [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))hergestellt wird. Dies ist jedoch nur für querladen-oder Debugszenarien möglich, in denen Sie lokalen Zugriff auf den Computer haben und über Administratorrechte verfügen.

* Alle an Loopback Verbindungen beteiligten Paketanwendungen müssen die `privateNetworkClientServer` Funktion in Ihren [Paket Manifesten](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)deklarieren.
* Wenn eine gepackte Anwendung eine Verbindung mit einer nicht verpackten Anwendung oder einem Dienst herstellt, führen `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` Sie aus, um eine Loopback Ausnahme für die gepackte Anwendung hinzuzufügen.
* Wenn eine nicht gepackte Anwendung oder ein Dienst eine Verbindung mit einer verpackten Anwendung herstellt, führen Sie aus, `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` damit die gepackte Anwendung eingehende Loopback Verbindungen empfängt.
    * [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) muss fortlaufend ausgeführt werden, während die gepackte Anwendung auf Verbindungen lauscht.
    * Das `-is` Flag wurde in Windows 10, Version 1607, eingeführt (10,0; Build 14393).

> [!NOTE]
> Der Paket Familienname, der für das `-n` Flag von [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) erforderlich ist, kann während der Entwicklungszeit über den Paket Manifest-Editor in Visual Studio, über [Partner Center](/windows/uwp/publish/view-app-identity-details) für Anwendungen, die über die Microsoft Store veröffentlicht werden, oder über den PowerShell [-Befehl Get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) für bereits installierte Anwendungen gefunden werden.

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10)) ist auch für das [Debuggen von Netzwerk Isolations Problemen](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)nützlich.

## <a name="pipes"></a>Pipes

[Pipes](/windows/win32/ipc/pipes) ermöglichen die einfache Kommunikation zwischen einem Pipe-Server und einem oder mehreren Pipeclients.

[Anonyme Pipes](/windows/win32/ipc/anonymous-pipes) und [Named Pipes](/windows/win32/ipc/named-pipes) werden mit den folgenden Einschränkungen unterstützt:

* Benannte Pipes in verpackten Anwendungen werden standardmäßig nur zwischen Prozessen innerhalb desselben Pakets unterstützt, es sei denn, ein Prozess ist voll vertrauenswürdig.
* Named Pipes können über Pakete hinweg freigegeben werden, und zwar gemäß den Richtlinien für die [Freigabe von benannten Objekten](/windows/uwp/communication/sharing-named-objects)
* Benannte Pipes in verpackten Anwendungen müssen die Syntax `\\.\pipe\LOCAL\` für den Pipenamen verwenden.

## <a name="registry"></a>Registrierung

Die [Registrierungs](/windows/win32/sysinfo/registry-functions) Verwendung für IPC ist grundsätzlich nicht empfehlenswert, wird jedoch für vorhandenen Code unterstützt. Paketanwendungen können nur auf Registrierungsschlüssel zugreifen, auf die Sie Zugriff haben.

[Desktop Anwendungen, die als msix verpackt](/windows/msix/desktop/desktop-to-uwp-root) sind, nutzen die [Registrierungsvirtualisierung](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry) , sodass globale Registrierungs Schreibvorgänge in einer privaten Hive innerhalb des msix-Pakets enthalten sind. Dadurch wird die Kompatibilität des Quellcodes ermöglicht, während die Auswirkungen auf die globale Registrierung minimiert werden und für IPC zwischen Prozessen im gleichen Paket verwendet werden kann. Wenn Sie die Registrierung verwenden müssen, wird dieses Modell bevorzugt im Vergleich zur Bearbeitung der globalen Registrierung verwendet.

## <a name="rpc"></a>RPC

[RPC](/windows/win32/rpc/rpc-start-page) kann verwendet werden, um eine gepackte Anwendung mit einem Win32-RPC-Endpunkt zu verbinden, vorausgesetzt, die gepackte Anwendung verfügt über die korrekten Funktionen, um die ACLs auf dem RPC-Endpunkt abzugleichen.

Mithilfe von benutzerdefinierten Funktionen können OEMs und IHVs [beliebige Funktionen definieren](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability), [Ihre RPC-Endpunkte mit der ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)und [diese Funktionen dann autorisierten Client Anwendungen gewähren](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file). Eine vollständige Beispielanwendung finden Sie im Beispiel [customcapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability) .

RPC-Endpunkte können auch an bestimmte gepackte Anwendungen geleitet werden, um den Zugriff auf den Endpunkt nur auf diese Anwendungen zu beschränken, ohne dass der Verwaltungsaufwand benutzerdefinierter Funktionen erforderlich ist. Sie können die [deriveappcontainersidfromappcontainername](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) -API verwenden, um eine SID von einem Paket Familiennamen abzuleiten, und dann den RPC-Endpunkt mit der SID anzeigen, wie im Beispiel " [customcapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp) " gezeigt.

## <a name="shared-memory"></a>Shared Memory

Die [Datei Zuordnung](/windows/win32/memory/sharing-files-and-memory) kann verwendet werden, um eine Datei oder einen Arbeitsspeicher zwischen zwei oder mehr Prozessen mit den folgenden Einschränkungen freizugeben:

* Standardmäßig werden Dateizuordnungen in verpackten Anwendungen nur zwischen Prozessen innerhalb desselben Pakets unterstützt, es sei denn, ein Prozess ist voll vertrauenswürdig.
* Dateizuordnungen können über Pakete hinweg freigegeben werden, und zwar gemäß den Richtlinien für die [Freigabe von benannten Objekten](/windows/uwp/communication/sharing-named-objects)

Gemeinsam genutzter Arbeitsspeicher wird für die effiziente Freigabe und Bearbeitung großer Datenmengen empfohlen.
