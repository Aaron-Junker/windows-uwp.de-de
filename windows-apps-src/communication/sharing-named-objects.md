---
title: Freigeben von benannten Objekten
description: In diesem Thema wird erläutert, wie benannte Objekte zwischen universelle Windows-Plattform (UWP)-Anwendungen und Win32-Anwendungen gemeinsam genutzt werden.
ms.date: 04/06/2020
ms.topic: article
keywords: Windows 10, UWP
ms.openlocfilehash: 38d08e71c44945a7b22f124d15507c7889f8589d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175614"
---
# <a name="sharing-named-objects"></a>Freigeben von benannten Objekten

In diesem Thema wird erläutert, wie benannte Objekte zwischen universelle Windows-Plattform (UWP)-Anwendungen und Win32-Anwendungen gemeinsam genutzt werden.

## <a name="named-objects-in-packaged-applications"></a>Benannte Objekte in Paketanwendungen

[Benannte Objekte](/windows/win32/sync/object-names) bieten eine einfache Möglichkeit für Prozesse, Objekt Handles gemeinsam zu nutzen. Nachdem ein Prozess ein benanntes Objekt erstellt hat, können andere Prozesse den Namen verwenden, um die entsprechende Funktion aufzurufen, um ein Handle für das Objekt zu öffnen. Benannte Objekte werden häufig für die [Thread Synchronisierung](/windows/win32/sync/interprocess-synchronization) und [prozessübergreifende Kommunikation](./interprocess-communication.md)verwendet.

Standardmäßig können Paketanwendungen nur auf benannte Objekte zugreifen, die Sie erstellt haben. Um benannte Objekte mit App-Paketen gemeinsam zu nutzen, müssen beim Erstellen von Objekten Berechtigungen festgelegt werden, und Namen müssen qualifiziert werden, wenn Objekte geöffnet werden.

## <a name="creating-named-objects"></a>Erstellen benannter Objekte

Benannte Objekte werden mit einer entsprechenden `Create` API erstellt:

* [CreateEvent](/windows/win32/api/synchapi/nf-synchapi-createeventexw)
* [CreateFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-createfilemappingw)
* [CreateMutex.](/windows/win32/api/synchapi/nf-synchapi-createmutexexw)
* [CreateSemaphore](/windows/win32/api/synchapi/nf-synchapi-createsemaphoreexw)
* ["Kreatewaitabletimer"](/windows/win32/api/synchapi/nf-synchapi-createwaitabletimerexw)

Alle diese APIs verwenden einen `LPSECURITY_ATTRIBUTES` Parameter, der es dem Aufrufer ermöglicht, [Zugriffs Steuerungs Listen (ACLs)](/previous-versions/windows/desktop/legacy/aa379560(v=vs.85)) anzugeben, um zu steuern, welche Prozesse auf das Objekt zugreifen können. Um benannte Objekte für gepackte Anwendungen freigeben zu können, muss die Berechtigung innerhalb der ACLs erteilt werden, wenn die benannten Objekte erstellt werden.

Sicherheits-IDs (SIDs) stellen Identitäten in ACLs dar. Jede gepackte Anwendung hat eine eigene sid, die auf dem zugehörigen Paket Familiennamen basiert. Sie können die sid für eine gepackte Anwendung generieren, indem Sie den Paket Familiennamen an [deriveappcontainersidfromappcontainername](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)übergeben.

> [!NOTE]
> Der Paket Familienname befindet sich während der Entwicklungszeit über den Paket Manifest-Editor in Visual Studio, über das [Partner Center](../publish/view-app-identity-details.md) für Anwendungen, die über das Microsoft Store veröffentlicht werden, oder über den PowerShell [-Befehl Get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) für bereits installierte Anwendungen.

[Dieses Beispiel](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath#examples) veranschaulicht das grundlegende Muster, das für die ACL eines benannten Objekts erforderlich ist. Um benannte Objekte für gepackte Anwendungen freizugeben, erstellen Sie eine [EXPLICIT_ACCESS](/windows/win32/api/accctrl/ns-accctrl-explicit_access_w) Struktur für jede Anwendung:

* `grfAccessMode = GRANT_ACCESS`
* `grfAccessPermissions =` entsprechende Berechtigungen basierend auf dem Objekt und der vorgesehenen Verwendung
    * [Generische Zugriffsrechte](/windows/win32/secauthz/generic-access-rights)
    * [Sicherheits-und Zugriffsrechte für das Synchronisierungs Objekt](/windows/win32/sync/synchronization-object-security-and-access-rights)
    * [Sicherheit und Zugriffsrechte für die Datei Zuordnung](/windows/win32/memory/file-mapping-security-and-access-rights)
* `grfInheritance = NO_INHERITANCE`
* `Trustee.TrusteeForm = TRUSTEE_IS_SID`
* `Trustee.TrusteeType = TRUSTEE_IS_USER`
* `Trustee.ptstrName =`die von [deriveappcontainersidfromappcontainername](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) abgerufene sid.

Durch Auffüllen des- `LPSECURITY_ATTRIBUTES` Parameters in `Create` Aufrufen mit Regeln für App-Pakete `EXPLICIT_ACCESS` können Sie Zugriff auf diese Anwendungen gewähren, um das benannte Objekt zu öffnen.

> [!NOTE]
> Win32-Anwendungen können auf alle benannten Objekte zugreifen, die von Paketanwendungen erstellt werden, sofern Sie die Objektnamen beim [Öffnen](#opening-named-objects)qualifizieren. Ihnen muss kein Zugriff gewährt werden.

## <a name="opening-named-objects"></a>Öffnen von benannten Objekten

Benannte Objekte werden geöffnet, indem ein Name an eine entsprechende `Open` API übergeben wird:

* [OpenEvent](/windows/win32/api/synchapi/nf-synchapi-openeventw)
* [Fehler bei OpenFileMapping](/windows/win32/api/memoryapi/nf-memoryapi-openfilemappingw)
* [OpenMutex](/windows/win32/api/synchapi/nf-synchapi-openmutexw)
* [Opensemaphore](/windows/win32/api/synchapi/nf-synchapi-opensemaphorew)
* [Openwaitabletimer](/windows/win32/api/synchapi/nf-synchapi-openwaitabletimerw)

Benannte Objekte, die von einer verpackten Anwendung erstellt werden, werden innerhalb des Namespaces der Anwendung erstellt, auch bekannt als benannter Objekt Pfad. Beim Öffnen von benannten Objekten, die von einer verpackten Anwendung erstellt wurden, muss den Objektnamen der benannte Objekt Pfad der Anwendung vorangestellt werden.

[Getappcontainernamedobjectpath](/windows/win32/api/securityappcontainer/nf-securityappcontainer-getappcontainernamedobjectpath) gibt den benannten Objekt Pfad für eine Paket Anwendung basierend auf der SID zurück. Sie können die sid für eine gepackte Anwendung generieren, indem Sie den Paket Familiennamen an [deriveappcontainersidfromappcontainername](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername)übergeben.

> [!NOTE]
> Der Paket Familienname befindet sich während der Entwicklungszeit über den Paket Manifest-Editor in Visual Studio, über das [Partner Center](../publish/view-app-identity-details.md) für Anwendungen, die über das Microsoft Store veröffentlicht werden, oder über den PowerShell [-Befehl Get-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) für bereits installierte Anwendungen.

Verwenden Sie das folgende Format, wenn benannte Objekte geöffnet werden, die von einer Paket Anwendung erstellt wurden `<PATH>\<NAME>` :

* Ersetzen Sie dies `<PATH>` durch den benannten Objekt Pfad der Anwendung.
* Ersetzen Sie dies `<NAME>` durch den Objektnamen.

> [!NOTE]
> Das vorab Beheben von Objektnamen mit `<PATH>` ist nur erforderlich, wenn das Objekt von einer gepackten Anwendung erstellt wurde. Benannte Objekte, die von Win32-Anwendungen erstellt wurden, müssen nicht qualifiziert werden, obwohl der Zugriff immer noch gewährt werden muss, wenn die Objekte [erstellt](#creating-named-objects)werden.

## <a name="remarks"></a>Bemerkungen

Benannte Objekte in Paketanwendungen sind standardmäßig isoliert, um die Sicherheit zu gewährleisten und die Unterstützung für Anwendungslebenszyklus-Ereignisse wie anhalten und beenden sicherzustellen. Durch die gemeinsame Nutzung von benannten Objekten über mehrere Anwendungen hinweg werden strenge Bindungs-und Versions Einschränkungen eingeführt Aus diesen Gründen wird empfohlen, nur benannte Objekte zwischen Anwendungen desselben Herausgebers freizugeben.