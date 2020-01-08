---
title: Bluetooth-Entwickler – Häufig gestellte Fragen
description: Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu den UWP-Bluetooth-APIs.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 584d327fc4882db6d3bf8d0cfd2a84b13023c6f4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684842"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth-Entwickler – Häufig gestellte Fragen

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu UWP-Bluetooth-APIs.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Welche APIs soll ich verwenden? Bluetooth Classic (RFCOMM) oder Bluetooth Low Energy (GATT)?
Im Internet finden sich verschiedene Diskussionen zu diesem allgemeinen Thema, deshalb bezieht sich diese Antwort nur auf die Unterschiede in Bezug auf Windows. Hier einige allgemeine Richtlinien:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Verwenden Sie für die Kommunikation mit einem Gerät, das die Bluetooth Low Energy unterstützt, die GATT-APIs. Wenn Ihr Anwendungsfall selten, eine geringe Bandbreite oder eine geringe Leistung erfordert, ist Bluetooth Low Energy die Antwort. Der Haupt-Namespace, der diese Funktion enthält, ist [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Verwendung von Bluetooth Le nicht**
- In Szenarien mit hoher Bandbreite und sehr häufiger Kommunikation. Wenn ständig großen Mengen an Daten synchronisiert werden müssen, sollten Sie erwägen, Bluetooth Classic oder vielleicht sogar WLAN zu verwenden. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Die RFCOMM-APIs geben Entwicklern einen Socket für die Ausführung bidirektionaler serieller Port-Kommunikation. Wenn Sie einen Socket haben, sind die Methoden zum Schreiben und lesen Recht Standard. Eine entsprechende Implementierung finden Sie im [Rfcomm Chat-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Verwendung von Bluetooth RFCOMM nicht** 
- Für Benachrichtigungen. Das Bluetooth GATT-Protokoll verfügt über einen speziellen Befehl dafür und ermöglicht einen deutlich geringeren Stromverbrauch und schnellere Reaktionszeiten. 
- Beim Überprüfen auf Näherungs- oder Anwesenheitserkennung. Verwenden dazu die [Ankündigungs-APIs](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement), und stellen Sie eine Verbindung über Bluetooth LE her. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Warum reagiert mein Bluetooth LE-Gerät nach einem Trennvorgang nicht mehr?

Der häufigste Grund hierfür ist, dass das Remote Gerät die Paarinformationen verloren hat. Für eine große Anzahl älterer Bluetooth-Geräte ist keine Authentifizierung erforderlich. Um den Benutzer zu schützen, erfordern alle paarweise aus der Einstellungs Anwendung ausgeführten Kopplung-Transaktionen eine Authentifizierung, und einige Geräte wurden in diesem Fall nicht entworfen. 

Ab Windows 10 Release 1511 haben Entwickler die Kontrolle über den Kopplung-Handshake. Im [Beispiel für die Geräteenumeration und -kopplung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) werden verschiedene Aspekte beim Zuordnen neuer Geräte erörtert.

In diesem Beispiel wird die Kopplung mit einem Gerät ohne Verschlüsselung initiiert. Dies funktioniert allerdings nur, wenn das Remotegerät keine Verschlüsselung oder Authentifizierung erfordert.

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>Muss ich Bluetooth-Geräte vor der Verwendung koppeln?

Sie müssen Geräte nicht koppeln, bevor Sie Sie verwenden, wenn Sie Bluetooth RFCOMM (klassisch) nutzen. Ab Windows 10, Version 1607, können Sie einfach Geräte in der Nähe suchen und eine Verbindung herstellen. Das aktualisierte [RFCOMM Chat-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) veranschaulicht diese Funktionalität. 

**(14393 und ältere Versionen)** Da dieses Feature für Bluetooth Low Energy (GATT-Client) nicht verfügbar ist, müssen diese Geräte weiterhin über die Einstellungsseite oder mithilfe der [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)-APIs gekoppelt werden, damit Sie darauf zugreifen können.

**(15030 und höhere Versionen)** Das Koppeln von Bluetooth-Geräten ist nicht mehr erforderlich. Verwenden Sie die neuen asynchronen APIs wie GetGattServicesAsync und GetCharacteristicsAsync, um den aktuellen Zustand des Remotegeräts abzufragen. Ausführlichere Informationen finden Sie in den [Client-Dokumentationen](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Wann sollte ich vor der Kommunikation ein Gerät koppeln?
Im Allgemeinen müssen Sie, wenn Sie eine vertrauenswürdige, langfristige Bindung mit einem Gerät benötigen, mit dem Gerät koppeln, indem Sie den Benutzer entweder an die Seite "Einstellungen" weiterleiten oder die APIs für die Geräte Aufzählung und-Kopplung verwenden. Wenn Sie lediglich Informationen aus dem Gerät lesen müssen, das öffentlich verfügbar gemacht wird (ein Temperatursensor oder ein Leuchtsignal), dann können Sie die Ankündigungen verbinden oder darauf warten, ohne sich mit dem Gerät zu koppeln. Dadurch werden Interoperabilitätsprobleme langfristig vermieden, da eine große Anzahl von Geräten keine Kopplung unterstützt. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Unterstützen alle Windows-Geräte die Rolle „Peripheral“?

Nein Dabei handelt es sich um ein Hardware abhängiges Feature, aber es wird eine-Methode bereitgestellt, bluetoothadapter. isperipheralrolesupported, um abzufragen, ob Sie unterstützt wird.  Zu den derzeit unterstützten Geräten zählen Windows Phone auf 8992+ und RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Kann ich auf diese APIs aus Win32 zugreifen?

Ja, alle diese APIs sollten funktionieren. In diesem Blog wird ausführlich beschrieben, wie [Windows-APIs aus Desktop-Anwendungen](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) aufgerufen werden. 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Ist diese Funktion im *-Hier SKU einfügen-* vorhanden?

**Bluetooth LE**: Ja, alle Funktionen sind in OneCore vorhanden und sollten auf den neuesten Geräten mit einem funktionierenden Bluetooth LE-Stapel verfügbar sein. 
> Einschränkungen: die Peripherie Rolle ist Hardware abhängig, und einige Windows Server-Editionen unterstützen Bluetooth nicht. 

**Bluetooth BR/EDR (klassisch)** : einige Variationen sind vorhanden, aber meistens verfügen Sie über eine sehr ähnliche Profil Ebenen-Unterstützung. Weitere Informationen finden Sie in der Dokumentation zu [RFCOMM](send-or-receive-files-with-rfcomm.md) und den folgenden unterstützten Profil Dokumenten für [PC](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles) und [Telefon](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles) .
