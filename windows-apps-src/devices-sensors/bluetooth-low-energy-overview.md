---
title: Bluetooth Low Energy
description: Erfahren Sie mehr über die Bluetooth Low Energy (Le)-Spezifikation in UWP-apps, die Protokolle für die Ermittlung und Kommunikation zwischen energiesparenden Geräten definiert.
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Bluetooth, Bluetooth Le, Low Energy, GATT, GAP, Central, Peripherie, Client, Server, Watcher, Publisher
ms.localizationpriority: medium
ms.openlocfilehash: 566e8e26e1a219a07dd5e7b539d96878a79f9163
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159764"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (Le) ist eine Spezifikation, die Protokolle für die Ermittlung und Kommunikation zwischen energiesparenden Geräten definiert. Die Ermittlung von Geräten erfolgt über das Protokoll "generisches Zugriffs Profil" (GAP). Nach der Ermittlung erfolgt die Kommunikation zwischen Geräten über das GATT-Protokoll (Generic Attribute). Dieses Thema enthält eine kurze Übersicht über Bluetooth Le in UWP-apps. Weitere Informationen zu Bluetooth Le finden Sie in der [Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification/) -Version 4,0, in der Bluetooth Le eingeführt wurde. 

![Bluetooth-Le-Rollen](images/gatt-roles.png)

*GATT-und GAP-Rollen wurden in Windows 10, Version 1703, eingeführt.*

GATT-und GAP-Protokolle können in der UWP-App mithilfe der folgenden Namespaces implementiert werden.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>Zentral und Peripheriegeräte
Die zwei primären Rollen von Discovery werden als "zentral" und "Peripherie" bezeichnet. Im allgemeinen arbeitet Windows im zentralen Modus und stellt eine Verbindung mit verschiedenen Peripheriegeräten her. 

## <a name="attributes"></a>Attribute
Ein allgemeines Akronym, das Sie in den Windows Bluetooth-APIs sehen werden, ist das generische Attribut (GATT). Das GATT-Profil definiert die Struktur der Daten und Betriebsmodi, mit denen zwei Bluetooth-Le-Geräte kommunizieren. Das-Attribut ist der Haupt Baustein von GATT. Die Haupttypen von Attributen sind Dienste, Merkmale und Deskriptoren. Diese Attribute unterscheiden sich zwischen Clients und Servern, sodass Sie Ihre Interaktion in den entsprechenden Abschnitten ausführlicher erörtern sollten. 

![Typische Attribut Hierarchie in einem gemeinsamen Profil](images/gatt-service.png)

*Der herzpreis Dienst wird im GATT-Server-API-Formular ausgedrückt.*

## <a name="client-and-server"></a>Client und Server
Nachdem eine Verbindung hergestellt wurde, wird das Gerät, das die Daten enthält (in der Regel ein kleiner IOT-Sensor oder eine webauanlage), als Server bezeichnet. Das Gerät, das diese Daten zum Ausführen einer Funktion verwendet, wird als Client bezeichnet. Ein Windows-PC (Client) liest z. b. Daten aus einem Heartbeat-Monitor (Server), um zu verfolgen, ob ein Benutzer optimal arbeitet. Weitere Informationen finden Sie in den Themen zum [GATT-Client](gatt-client.md) und zum [GATT-Server](gatt-server.md) .

## <a name="watchers-and-publishers-beacons"></a>Watcher und Verleger (Beacons)
Zusätzlich zu den zentralen und peripheren Rollen gibt es Beobachter-und Sender Rollen. Sender werden häufig als Beacons bezeichnet und kommunizieren nicht über das GATT, da Sie den begrenzten Speicherplatz verwenden, der im Ankündigungs Paket für die Kommunikation bereitgestellt wird. Auf ähnliche Weise muss ein Beobachter keine Verbindung herstellen, um Daten zu empfangen, sondern nach nahe gelegenen Ankündigungen suchen. Verwenden Sie die [bluetoothleankünklmentwatcher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) -Klasse, um Windows so zu konfigurieren, dass Ankündigungen in der Nähe beobachtet werden. Verwenden Sie zum Übertragen von Signal Nutzlasten die [bluetoothlebegnasementpublisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) -Klasse. Weitere Informationen finden [Sie im Thema zur Ankündigung](ble-beacon.md) .

## <a name="see-also"></a>Weitere Informationen
- [Windows.Devices.Bluetooth.GenericAttributeProfile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)
- [Bluetooth Core-Spezifikation](https://www.bluetooth.com/specifications/bluetooth-core-specification)