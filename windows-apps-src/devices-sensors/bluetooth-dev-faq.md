---
title: Bluetooth-Entwickler – Häufig gestellte Fragen
description: Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu den UWP-Bluetooth-APIs.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 4cc1bafb90b20083d55a622873dea7be5efbf5b7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633485"
---
# <a name="bluetooth-developer-faq"></a>Bluetooth-Entwickler – Häufig gestellte Fragen

Dieser Artikel bietet Antworten auf häufig gestellte Fragen zu UWP-Bluetooth-APIs.

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>Welche APIs soll ich verwenden? Bluetooth Classic (RFCOMM) oder Bluetooth Low Energy (GATT)?
Im Internet finden sich verschiedene Diskussionen zu diesem allgemeinen Thema, deshalb bezieht sich diese Antwort nur auf die Unterschiede in Bezug auf Windows. Hier einige allgemeine Richtlinien:

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>Bluetooth LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

Verwenden Sie für die Kommunikation mit einem Gerät, das die Bluetooth Low Energy unterstützt, die GATT-APIs. Wenn Ihr Anwendungsfall nur eine sporadische Kommunikation, wenig Bandbreite oder geringen Energieverbrauch erfordert, lautet die Antwort Bluetooth Low Energy. Der Haupt-Namespace, der diese Funktion enthält, ist [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile). 

**Wenn keine Verwendung von Bluetooth LE**
- In Szenarien mit hoher Bandbreite und sehr häufiger Kommunikation. Wenn ständig großen Mengen an Daten synchronisiert werden müssen, sollten Sie erwägen, Bluetooth Classic oder vielleicht sogar WLAN zu verwenden. 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>Bluetooth Classic (Windows.Devices.Bluetooth.Rfcomm)

Mit den RFCOMM-APIs erhalten Entwickler einen Socket, um eine bidirektionale Kommunikation mit seriellem Anschluss auszuführen. Nachdem ein Socket verfügbar ist, entsprechen die Methoden zum Schreiben in und Lesen aus diesem Socket ziemlich genau den Standardverfahren. Eine entsprechende Implementierung finden Sie im [Rfcomm Chat-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat). 

**Wenn keine Verwendung von Bluetooth Rfcomm** 
- Für Benachrichtigungen. Das Bluetooth GATT-Protokoll verfügt über einen speziellen Befehl dafür und ermöglicht einen deutlich geringeren Stromverbrauch und schnellere Reaktionszeiten. 
- Beim Überprüfen auf Näherungs- oder Anwesenheitserkennung. Verwenden dazu die [Ankündigungs-APIs](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement), und stellen Sie eine Verbindung über Bluetooth LE her. 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>Warum reagiert mein Bluetooth LE-Gerät nach einem Trennvorgang nicht mehr?

Der häufigste Grund besteht darin, dass die Kopplungsinformationen auf dem Remotegerät verloren gegangen sind. Viele ältere Bluetooth-Geräte erfordern keine Authentifizierung. Zum Schutz des Benutzers erfordern alle über die Einstellungs-App ausgeführten Kopplungsversuche eine Authentifizierung. Einige Geräte sind dafür nicht ausgelegt. 

Ab Windows 10, Version 1511, haben Entwickler Kontrolle über den Kopplungsvorgang. Im [Beispiel für die Geräteenumeration und -kopplung](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) werden verschiedene Aspekte beim Zuordnen neuer Geräte erörtert.

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

Bei Bluetooth RFCOMM (Classic)-Geräten ist dies nicht erforderlich. Ab Windows 10, Version 1607, können Sie einfach Geräte in der Nähe suchen und eine Verbindung herstellen. Das aktualisierte [RFCOMM Chat-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat) veranschaulicht diese Funktionalität. 

**(14393 und ältere Versionen)** Da dieses Feature für Bluetooth Low Energy (GATT-Client) nicht verfügbar ist, müssen diese Geräte weiterhin über die Einstellungsseite oder mithilfe der [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx)-APIs gekoppelt werden, damit Sie darauf zugreifen können.

**(15030 und höhere Versionen)** Das Koppeln von Bluetooth-Geräten ist nicht mehr erforderlich. Verwenden Sie die neuen asynchronen APIs wie GetGattServicesAsync und GetCharacteristicsAsync, um den aktuellen Zustand des Remotegeräts abzufragen. Ausführlichere Informationen finden Sie in den [Client-Dokumentationen](gatt-client.md). 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>Wann sollte ich vor der Kommunikation ein Gerät koppeln?
In der Regel sollten Sie das Gerät koppeln (entweder, indem Sie den Benutzer zur Einstellungsseite leiten oder mit der Device-Enumeration und den Kopplungs-APIs), wenn Sie eine vertrauenswürdige, langfristige Verbindung mit einem Gerät benötigen. Wenn Sie Informationen zu lesen lediglich, deaktiviert das Gerät, das ist öffentlich verfügbar gemacht (Temperatursensor oder Signalframes), und eine Verbindung herstellen oder Ankündigungen warten, ohne dass alle anstrengungen, die mit dem Gerät. Dies verhindert langfristig Interoperabilitätsprobleme, da die Kopplung von vielen Geräten nicht unterstützt wird. 

## <a name="do-all-windows-devices-support-peripheral-role"></a>Unterstützen alle Windows-Geräte die Rolle „Peripheral“?

Nein – Hierbei handelt es sich um eine hardwareabhängige Funktion. Es steht aber eine Methode (BluetoothAdapter.IsPeripheralRoleSupported) für die Abfrage zur Verfügung, ob diese Rolle unterstützt wird oder nicht.  Zu den derzeit unterstützten Geräten zählen Windows Phone auf 8992+ und RPi3 (Windows IoT). 

## <a name="can-i-access-these-apis-from-win32"></a>Kann ich auf diese APIs aus Win32 zugreifen?

Ja, alle diese APIs sollten funktionieren. In diesem Blog wird ausführlich beschrieben, wie [Windows-APIs aus Desktop-Anwendungen](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) aufgerufen werden. 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>Ist diese Funktion im *-Hier SKU einfügen-* vorhanden?

**Bluetooth LE**: Ja, alle Funktionen ist in der OneCore und auf die letzte Geräten mit einem funktionierenden Bluetooth LE-Stapel verfügbar sein sollen. 
> Einschränkung: Peripherie-Rolle ist abhängig von der Hardware und einigen Windows Server-Editionen unterstützen keine Bluetooth. 

**Bluetooth (klassisch) für die BR/EDR**: Im großen und sehr ähnliche Ebene Profil-Unterstützung verfügen, jedoch einige Variationen vorhanden. Siehe die Dokumentation zu [RFCOMM](send-or-receive-files-with-rfcomm.md) und diese Dokumentation zu unterstützten Profilen für [PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles) und [Telefon](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles).

