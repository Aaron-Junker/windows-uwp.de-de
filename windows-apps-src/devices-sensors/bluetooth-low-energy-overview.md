---
title: Bluetooth Low Energy
description: Dieses Thema enthält eine kurze Übersicht über Bluetooth LE in UWP-Apps.
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, Bluetooth, Bluetooth LE, energiesparend, GATT, GAP, Central, Peripheral, Client, Server, Beobachter, Herausgeber
ms.localizationpriority: medium
ms.openlocfilehash: 1714a4c21852a7582325fc26f7e6a1f0f969126e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629035"
---
# <a name="bluetooth-low-energy"></a>Bluetooth Low Energy
Bluetooth Low Energy (LE) ist eine Spezifikation, die Protokolle für die Ermittlung von und die Kommunikation zwischen energieeffizienten Geräten definiert. Die Ermittlung von Geräten wird mit dem Generic Access Profile (GAP)-Protokoll durchgeführt. Nach der Ermittlung erfolgt die Kommunikation von Gerät zu Gerät über das Generic Attribute (GATT)-Protokoll. Dieses Thema enthält eine kurze Übersicht über Bluetooth LE in UWP-Apps. Weitere Informationen zu Bluetooth LE finden Sie in der [Bluetooth Core Specification](https://www.bluetooth.com/specifications/bluetooth-core-specification), Version 4.0, in der Bluetooth LE eingeführt wurde. 

![Bluetooth LE-Rollen](images/gatt-roles.png)

*GATT und eine GAP-Rollen wurden in Windows 10, Version 1703 eingeführt.*

GATT- und GAP-Protokolle können mithilfe der folgenden Namespaces in UWP-Apps implementiert werden.
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>Central und Peripheral
Die beiden primären Rollen für die Ermittlung heißen „Central“ und „Peripheral“. In der Regel wird Windows im Central-Modus ausgeführt und stellt eine Verbindung mit verschiedenen Peripheral-Geräten her. 

## <a name="attributes"></a>Attribute
Das allgemeine Akronym in den Windows-Bluetooth-APIs lautet GATT (Generic Attribute). Das GATT-Profil definiert die Struktur der Daten und Betriebsmodi, über die zwei Bluetooth LE-Geräte kommunizieren. Das Attribut ist der zentrale Baustein von GATT. Die wichtigsten Attributtypen sind Dienste, Merkmale und Deskriptoren. Diese Attribute verhalten sich in Clients und Servern unterschiedlich, daher es sinnvoller ist, ihre Interaktion in den entsprechenden Abschnitten zu behandeln. 

![Typische Attributhierarchie in einem allgemeinen Profil](images/gatt-service.png)

*Der Herz-Dienst wird in GATT-Server-API-Form ausgedrückt werden.*

## <a name="client-and-server"></a>Client und Server
Nachdem eine Verbindung hergestellt wurde, wird das Gerät (in der Regel ein kleiner IoT- oder tragbarer Sensor) mit den Daten als Server bezeichnet. Das Gerät, das diese Daten verwendet, um eine Funktion auszuführen, wird wie als Client bezeichnet. Ein Windows-PC (Client) liest z. B. Daten aus einem Pulsmessgerät (Server), um zu erfassen, ob ein Benutzer optimal trainiert. Weitere Informationen finden Sie in den Themen [GATT-Client](gatt-client.md) und [GATT-Server](gatt-server.md).

## <a name="watchers-and-publishers-beacons"></a>Beobachter und Herausgeber (Beacons)
Zusätzlich zu den Rollen „Central” und „Peripheral” gibt es die Rollen „Observer“ und „Broadcaster“. Broadcaster werden häufig als Beacons bezeichnet, sie kommunizieren nicht über GATT, da sie den im Ankündigungspaket für die Kommunikation vorgesehenen begrenzten Raum verwenden. Entsprechend muss ein Observer keine Verbindung zum Empfangen von Daten einrichten, er sucht nach Ankündigungen in der Nähe. Verwenden Sie zum Konfigurieren von Windows für die Beobachtung von Ankündigungen in der Nähe die [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)-Klasse. Um Beacon-Nutzlasten zu übertragen, verwenden Sie die [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)-Klasse. Weitere Informationen finden Sie im Thema [Ankündigungen](ble-beacon.md).

## <a name="see-also"></a>Weitere Informationen
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Bluetooth-Kern-Spezifikation](https://www.bluetooth.com/specifications/bluetooth-core-specification)