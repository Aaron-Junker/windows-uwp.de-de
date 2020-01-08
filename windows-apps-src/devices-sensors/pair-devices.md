---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Koppeln von Geräten
description: Einige Geräte müssen gekoppelt werden, bevor sie verwendet werden können. Der Windows.Devices.Enumeration-Namespace unterstützt drei verschiedene Verfahren zum Koppeln von Geräten
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 1dbf843d9a45cbf31e5ec5c1a538e6e5e2b53ee2
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684685"
---
# <a name="pair-devices"></a>Koppeln von Geräten



**Wichtige APIs**

- [**Windows. Devices. Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

Einige Geräte müssen gekoppelt werden, bevor sie verwendet werden können. Der [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-Namespace unterstützt drei verschiedene Verfahren zum Koppeln von Geräten:

-   Automatische Kopplung
-   Einfache Kopplung
-   Benutzerdefinierte Kopplung

**Tipp**  einige Geräte müssen nicht gekoppelt werden, um verwendet werden zu können. Dies wird im Abschnitt zur automatischen Kopplung behandelt.

 

## <a name="automatic-pairing"></a>Automatische Kopplung


Es kann vorkommen, dass sie in Ihrer Anwendung ein Gerät verwenden möchten, es dabei aber keine Rolle spielt, ob das Gerät gekoppelt ist. Sie möchten lediglich die Funktionen des Geräts nutzen. Wenn die App beispielsweise nur ein Bild einer Webcam erfassen soll, sind Sie meist nicht am Gerät selbst interessiert, sondern nur an der Erfassung des Bilds. Falls Geräte-APIs für das betreffende Gerät verfügbar sind, fällt das Szenario unter die automatische Kopplung.

Sie nutzen einfach die dem Gerät zugeordneten APIs, führen Aufrufe nach Bedarf durch und überlassen es dem System, sich um ggf. erforderliche Kopplungen zu kümmern. Einige Geräte müssen nicht gekoppelt werden, damit Sie Funktionen verwenden können. Falls das Gerät nicht gekoppelt werden muss, übernehmen die Geräte-APIs die Kopplung im Hintergrund, sodass Sie diese Funktionen nicht in Ihre App integrieren müssen. Ihre App verfügt hierbei über keinerlei Informationen darüber, ob ein Gerät gekoppelt ist oder sein muss, aber Sie können trotzdem auf das Gerät zugreifen und seine Funktionen nutzen.

## <a name="basic-pairing"></a>Einfache Kopplung


Bei einer einfachen Kopplung versucht Ihre Anwendung, das Geräte über die [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-APIs zu koppeln. In diesem Szenario wird der Kopplungsprozess von Windows durchgeführt und behandelt. Gegebenenfalls erforderliche Benutzerinteraktionen werden von Windows behandelt. Die einfache Kopplung wird verwendet, wenn Sie ein Gerät koppeln möchten und keine relevante Geräte-API für den Versuch einer automatischen Kopplung vorhanden ist. Sie möchten lediglich das Gerät nutzen und müssen es zuvor koppeln.

Zum Durchführen der einfachen Kopplung müssen Sie zuerst das [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt für das betreffende Gerät abrufen. Nach Erhalt dieses Objekts interagieren Sie mit der [**DeviceInformation.Pairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing)-Eigenschaft, bei der es sich um ein [**DeviceInformationPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing)-Objekt handelt. Rufen Sie einfach [**DeviceInformationPairing.PairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.pairasync) auf, um zu versuchen, die Kopplung durchzuführen. Sie müssen das Ergebnis abwarten (**await**), um der App Zeit für den Versuch zu geben, die Kopplungsaktion auszuführen. Das Ergebnis der Kopplungsaktion wird zurückgegeben, und falls keine Fehler zurückgegeben werden, wird das Gerät gekoppelt.

Bei Verwendung der einfachen Kopplung haben Sie außerdem Zugriff auf weitere Informationen zum Kopplungsstatus des Geräts. Beispielsweise kennen Sie den Kopplungsstatus ([**IsPaired**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) und wissen, ob das Gerät gekoppelt werden kann ([**CanPair**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Dies sind beides Eigenschaften des [**DeviceInformationPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing)-Objekts. Bei Verwendung der automatischen Kopplung haben Sie nur dann Zugriff auf diese Informationen, wenn Sie das relevante [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt abrufen.

## <a name="custom-pairing"></a>Benutzerdefinierte Kopplung


Bei der benutzerdefinierten Kopplung kann Ihre App in den Kopplungsprozess einbezogen werden. So kann die App die [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) angeben, die für den Kopplungsprozess unterstützt werden. Außerdem müssen Sie eine eigene Benutzeroberfläche für die bedarfsgerechte Interaktion mit dem Benutzer erstellen. Verwenden Sie die benutzerdefinierte Kopplung, wenn die App etwas mehr Einfluss auf den Ablauf des Kopplungsvorgangs haben soll oder wenn Sie Ihre eigene Benutzeroberfläche für die Kopplung anzeigen möchten.

Zum Implementieren der benutzerdefinierten Kopplung müssen Sie wie bei der einfachen Kopplung das [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekt für das betreffende Gerät abrufen. Wichtig ist aber die spezielle [**DeviceInformation.Pairing.Custom**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.custom)-Eigenschaft. Hierüber erhalten Sie ein [**DeviceInformationCustomPairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing)-Objekt. Alle [**DeviceInformationCustomPairing.PairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairasync)-Methoden erfordern die Einbindung eines [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds)-Parameters. Hiermit werden die Aktionen angegeben, die vom Benutzer durchgeführt werden müssen, um die Kopplung des Geräts zu erreichen. Weitere Informationen zu den unterschiedlichen Arten und den Aktionen, die Benutzer ausführen müssen, finden Sie auf der Seite mit der **DevicePairingKinds**-Referenz. Ebenso wie bei der einfachen Kopplung müssen Sie das Ergebnis abwarten (**await**), um der App Zeit für die Durchführung der Kopplungsaktion zu geben. Das Ergebnis der Kopplungsaktion wird zurückgegeben, und falls keine Fehler zurückgegeben werden, wird das Gerät gekoppelt.

Zum Unterstützen der benutzerdefinierten Kopplung müssen Sie einen Handler für das [**PairingRequested**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested)-Ereignis erstellen. Bei diesem Handler muss sichergestellt sein, dass alle unterschiedlichen [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds)-Aufzählungen abgedeckt sind, die bei einem Szenario mit benutzerdefinierter Kopplung verwendet werden. Welche Aktion jeweils die richtige ist, hängt von den **DevicePairingKinds**-Aufzählungen ab, die als Teil der Ereignisargumente bereitgestellt werden.

Es ist wichtig, darauf zu achten, dass die benutzerdefinierte Kopplung immer ein Vorgang auf Systemebene ist. Aus diesem Grund wird dem Benutzer bei Vorgängen auf dem Desktop oder einem Windows Phone immer ein Systemdialogfeld angezeigt, wenn die Kopplung durchgeführt werden soll. Dies ist der Fall, weil beide Plattformen über eine Benutzeroberfläche verfügen, für die die Zustimmung durch den Benutzer erforderlich ist. Da dieses Dialogfeld automatisch generiert wird, müssen Sie kein eigenes Dialogfeld erstellen, wenn Sie bei Vorgängen auf diesen Plattformen für [**DevicePairingKinds**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) die Option **ConfirmOnly** verwenden. Für die anderen **DevicePairingKinds**-Aufzählungen müssen Sie je nach **DevicePairingKinds**-Wert eine besondere Behandlung durchführen. Beispiele für die Behandlung der benutzerdefinierten Kopplung für unterschiedliche **DevicePairingKinds**-Werte finden Sie unter „Beispiel“.

Ab Windows 10, Version 1903, wird eine neue **devicepasirringkinds** unterstützt, **providepasswordcredential**. Dieser Wert bedeutet, dass die APP einen Benutzernamen und ein Kennwort vom Benutzer anfordern muss, um sich mit dem gekoppelten Gerät zu authentifizieren. Um diesen Fall zu behandeln, müssen Sie die Methode " [**Accept-withpasswordcredential**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_) " der Ereignis Argumente des Ereignis Handlers " **pageiingrequtry"** zum Akzeptieren der Kopplung aufruft. Übergeben Sie ein [**PasswordCredential**](https://docs.microsoft.com/uwp/api/windows.security.credentials.passwordcredential) -Objekt, das den Benutzernamen und das Kennwort als Parameter kapselt. Beachten Sie, dass sich der Benutzername und das Kennwort für das Remote Gerät von unterscheiden und häufig nicht mit den Anmelde Informationen für den lokal angemeldeten Benutzer identisch sind.

## <a name="unpairing"></a>Entkoppeln


Das Entkoppeln eines Geräts ist nur für die oben beschriebene einfache und benutzerdefinierte Kopplung relevant. Wenn Sie die automatische Kopplung verwenden, ist sich Ihre App des Kopplungsstatus des Geräts nicht bewusst, und es ist nicht erforderlich, eine Entkopplung durchzuführen. Falls Sie sich für die Entkopplung eines Geräts entscheiden, ist der Prozess für die Implementierung der einfachen und benutzerdefinierten Kopplung identisch. Dies liegt daran, dass keine zusätzlichen Informationen angegeben werden müssen oder mit dem Entkopplungsprozess interagiert werden muss.

Der erste Schritt beim Entkoppeln eines Geräts ist das Abrufen des [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekts für das Gerät, das entkoppelt werden soll. Anschließend müssen Sie die [**DeviceInformation.Pairing**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.pairing)-Eigenschaft abrufen und [**DeviceInformationPairing.UnpairAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationpairing.unpairasync) aufrufen. Wie bei der Kopplung warten Sie wieder das Ergebnis ab (**await**). Das Ergebnis der Entkopplungsaktion wird zurückgegeben, und falls keine Fehler zurückgegeben werden, wird das Gerät entkoppelt.

## <a name="sample"></a>Beispiel


Wenn Sie ein Beispiel zur Verwendung der [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-APIs herunterladen möchten, klicken Sie [hier](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

 

 
