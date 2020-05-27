---
ms.assetid: 23001DA5-C099-4C02-ACE9-3597F06ECBF4
title: IDs der AEP-Dienstklasse
description: Die AEP-Dienste (Zuordnungsendpunkt) bieten einen Programmierungsvertrag für Dienste, die von einem Gerät über ein bestimmtes Protokoll unterstützt werden. Für mehrere dieser Dienste sind Bezeichner festgelegt, die verwendet werden sollen, wenn auf sie verwiesen wird.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0af0e8b17578c2ef3cd3fb0a466446b06f8e4a00
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854736"
---
# <a name="aep-service-class-ids"></a>IDs der AEP-Dienstklasse

Die AEP-Dienste (Zuordnungsendpunkt) bieten einen Programmierungsvertrag für Dienste, die von einem Gerät über ein bestimmtes Protokoll unterstützt werden. Für mehrere dieser Dienste sind Bezeichner festgelegt, die verwendet werden sollen, wenn auf sie verwiesen wird. Diese Verträge werden durch die **System.Devices.AepService.ServiceClassId**-Eigenschaft gekennzeichnet. In diesem Abschnitt sind einige bekannte IDs der AEP-Dienstklasse aufgeführt. Die Klassen-ID des AEP-Diensts gilt auch für Protokolle mit benutzerdefinierten Klassen-IDs.

App-Entwickler sollten auf Basis der Klassen-IDs erweiterte Abfragesyntaxfilter (Advanced Query Syntax, AQS) verwenden, um ihre Abfragen auf die zu verwendenden AEP-Dienste zu beschränken. Dadurch werden sowohl die Abfrageergebnisse auf die betreffenden Dienste beschränkt als auch Leistung, Akkulaufzeit und Dienstqualität für das Gerät erheblich verbessert. Eine Anwendung kann z. B. diese Dienstklassen-IDs verwenden, um ein Gerät für die Miracast-Synchronisierung oder als DLNA-DMR (Digital Media-Renderer) zu verwenden. Weitere Informationen zur Interaktion zwischen Geräten und Diensten finden Sie unter [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind).

> **Wichtige APIs**
>
> - [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)

## <a name="bluetooth-and-bluetooth-le-services"></a>Bluetooth- und Bluetooth LE-Dienste

Bluetooth-Dienste zählen zu einem von zwei Protokollen, dem Bluetooth- oder Bluetooth LE-Protokoll. Bezeichner für diese Protokolle:

- Bluetooth-Protokoll-ID: {e0cbf06c-cd8b-4647-bb8a-263b43f 0F 974}
- Bluetooth-Le-Protokoll-ID: {bb7bb05e-5972-42b5-94fc-76eaa7084d49}

Das Bluetooth-Protokoll unterstützt verschiedene Dienste, die alle dasselbe grundlegende Format aufweisen. Die ersten vier Ziffern der GUID variieren je nach Dienst, aber alle Bluetooth-GUIDs enden mit **0000-0000-1000-8000-00805F9B34FB**. Der RFCOMM-Dienst weist den Vorspann 0x0003 auf, wodurch sich die vollständige ID **00030000-0000-1000-8000-00805F9B34FB** ergeben würde. Die folgende Tabelle enthält einige allgemeine Bluetooth-Dienste.

| Dienstname                         | GUID                                     |
|--------------------------------------|------------------------------------------|
| RFCOMM                               | **00030000-0000-1000-8000-00805F9B34FB** |
| GATT: Warnungsbenachrichtigungsdienst    | **18110000-0000-1000-8000-00805F9B34FB** |
| GATT: E/A für Automatisierung                 | **18150000-0000-1000-8000-00805F9B34FB** |
| GATT: Akkudienst               | **180F0000-0000-1000-8000-00805F9B34FB** |
| GATT: Blutdruck                | **18100000-0000-1000-8000-00805F9B34FB** |
| GATT: Körperbau              | **181B0000-0000-1000-8000-00805F9B34FB** |
| GATT: Verbindungsmanagement               | **181E0000-0000-1000-8000-00805F9B34FB** |
| GATT: Fortlaufende Zuckerspiegelüberwachung | **181F0000-0000-1000-8000-00805F9B34FB** |
| GATT: Aktueller Zeitdienst          | **18050000-0000-1000-8000-00805F9B34FB** |
| GATT: Radfahrleistung                 | **18180000-0000-1000-8000-00805F9B34FB** |
| GATT: Radfahrgeschwindigkeit und -frequenz     | **18160000-0000-1000-8000-00805F9B34FB** |
| GATT: Geräteinformationen            | **180A0000-0000-1000-8000-00805F9B34FB** |
| GATT: Umgebungsabtastung         | **181A0000-0000-1000-8000-00805F9B34FB** |
| GATT: Generischer Zugriff                | **18000000-0000-1000-8000-00805F9B34FB** |
| GATT: Generisches Attribut             | **18010000-0000-1000-8000-00805F9B34FB** |
| GATT: Blutzucker                       | **18080000-0000-1000-8000-00805F9B34FB** |
| GATT: Indikator für den Gesundheitszustand            | **18090000-0000-1000-8000-00805F9B34FB** |
| GATT: Herzfrequenz                    | **180D0000-0000-1000-8000-00805F9B34FB** |
| GATT: Menschliches Schnittstellengerät        | **18120000-0000-1000-8000-00805F9B34FB** |
| GATT: Sofortige Warnung               | **18020000-0000-1000-8000-00805F9B34FB** |
| GATT: Positionierung in Gebäuden            | **18210000-0000-1000-8000-00805F9B34FB** |
| GATT: Internetprotokollunterstützung     | **18200000-0000-1000-8000-00805F9B34FB** |
| GATT: Verbindungsausfall                     | **18030000-0000-1000-8000-00805F9B34FB** |
| GATT: Position und Navigation       | **18190000-0000-1000-8000-00805F9B34FB** |
| GATT: Dienst für nächsten Sommerzeitwechsel       | **18070000-0000-1000-8000-00805F9B34FB** |
| GATT: Dienst für Warnstatus des Mobiltelefons    | **180E0000-0000-1000-8000-00805F9B34FB** |
| GATT: Pulsoximeter                | **18220000-0000-1000-8000-00805F9B34FB** |
| GATT: Referenzzeit-Aktualisierungsdienst | **18060000-0000-1000-8000-00805F9B34FB** |
| GATT: Laufgeschwindigkeit und -frequenz     | **18140000-0000-1000-8000-00805F9B34FB** |
| GATT: Abtastparameter               | **18130000-0000-1000-8000-00805F9B34FB** |
| GATT: Tx-Leistung                      | **18040000-0000-1000-8000-00805F9B34FB** |
| GATT: Benutzerdaten                     | **181C0000-0000-1000-8000-00805F9B34FB** |
| GATT: Gewichtsskala                  | **181D0000-0000-1000-8000-00805F9B34FB** |

Eine ausführlichere Auflistung der verfügbaren Bluetooth-Dienste finden Sie in der [GATT-Dienst Spezifikation](https://www.bluetooth.com/specifications/gatt/services/). Sie können auch die [**GattServiceUuids**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile.GattServiceUuids)-API verwenden, um einige allgemeine GATT-Dienste abzurufen.

## <a name="custom-bluetooth-le-services"></a>Benutzerdefinierte Bluetooth LE-Dienste

Benutzerdefinierte Bluetooth LE-Dienste verwenden die folgende Protokollkennung: {bb7bb05e-5972-42b5-94fc76eaa7084d49}

Benutzerdefinierte Profile werden mit ihren eigenen definierten GUIDs definiert. Diese benutzerdefinierte GUID sollte für **System.Devices.AepService.ServiceClassId** verwendet werden.

## <a name="upnp-services"></a>UPnP-Dienste

UPnP-Dienste verwenden den folgenden Protokoll Bezeichner: {0e2614-12s0-46e6-91ba-428607ccef64}

In der Regel wird der Name aller UPnP-Dienste mithilfe des in RFC 4122 definierten Algorithmus mit einem Hash in eine GUID verwandelt. In der folgenden Tabelle sind einige in Windows definierte allgemeine UPnP-Dienste aufgeführt.

| Dienstname                       | GUID                                      |
|------------------------------------|-------------------------------------------|
| Ziel-Editor für Dimensionsverarbeitung                 | **ba36014c-b51f-51cc-bf71-1ad779ced3c6**  |
| AV-Transport                       | **deeacb78-707a-52df-b1c6-6f945e7e25bf**  |
| Renderingkontrolle                  | **cc7fe721-a3c7-5a14-8c49-4419dc895513**  |
| Ebene-3-Weiterleitung                 | **97d477fa-o403-577b-a714-b29a9007797f**  |
| Konfiguration der allgemeinen WAN-Schnittstelle | **e4c1c624-c3c4-5104-b72e-ac425d9d157c**  |
| WAP-IP-Verbindung                  | **e4ac1c23-b5ac-5c27-8814-6bd837d8832c**  |
| WFA-WLAN-Konfiguration             | **23d5f 7dB-747F -5099-8 ddfd0c3c688**  |
| Drucker, erweitert                   | **fb9074da-3d9f-5384-922e-9978ae51ef0c**  |
| Drucker, grundlegend                      | **5d2a7252-d45c-5158-87a4-05212da327e1**  |
| Medienempfängerregistrierungsstelle           | **0b4a2add-d725-5198-b2ba-852b8bf 8d183**  |
| Inhaltsverzeichnis                  | **89e701dd-0597-5279-a31c-235991d0db1c**  |
| Wählen                               | **085dfa4a-3948-53c7-a0d7-16d8ec26b29b**  |

## <a name="wsd-services"></a>WSD-Dienste

Die WSD-Dienste verwenden den folgenden Protokoll Bezeichner: {782232aa-a2s9-4993-971b-aedc551346b0}

In der Regel wird der Name aller WSD-Dienste mithilfe des in RFC 4122 definierten Algorithmus mit einem Hash in eine GUID verwandelt. In der folgenden Tabelle sind einige in Windows definierte allgemeine WSD-Dienste aufgeführt.

| Dienstname | GUID                                     |
|--------------|------------------------------------------|
| Drucker      | **65 dca7bd-2611-583e-9a12-ad90f 47749cf** |
| Scanner      | **56ec8b9e-0237-5cae-aa3b-d322dd2e6c1e** |

## <a name="aqs-sample"></a>AQS-Beispiel

Diese AQS filtert nach allen UPnP-**AssociationEndpointService**-Objekten, die DIAL unterstützen. In diesem Fall wird [**DeviceInformationKind**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationKind) auf **AsssociationEndpointService** festgelegt.

``` syntax
System.Devices.AepService.ProtocolId:="{0e261de4-12f0-46e6-91ba-428607ccef64}" AND
System.Devices.AepService.ServiceClassId:="{085DFA4A-3948-53C7-A0D7-16D8EC26B29B}"
```
