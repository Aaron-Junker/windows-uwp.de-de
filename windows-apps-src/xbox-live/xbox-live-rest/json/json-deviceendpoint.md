---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618875"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
DeviceEndpoint Objekt verfügt über den folgenden Spezifikationen.
 
| Mitglied| Typ| Beschreibung| 
| --- | --- | --- | 
| deviceName| string| Optional. Ein Anzeigename für das Gerät, falls zutreffend. Dieser Wert wird derzeit nicht verwendet.| 
| endpointUri| string| Erforderlich. Die URL, die die Clientplattform (Windows oder Windows Phone) aus der pushbenachrichtigungsdienst (WNS oder MPNS) abgerufen wurde.| 
| locale| string| Erforderlich. Die gewünschte Sprache von Benachrichtigungen an diesen Endpunkt gesendet werden soll. Kann es sich um eine Liste von durch Trennzeichen getrennten Werten nach Priorität sein. Beispiel: "de-DE, En-US, En".| 
| Plattform| string| Optional. Derzeit unterstützte Werte sind "WindowsPhone" und "Windows". Wenn nicht angegeben ist, wird er aus dem Token für Gerät abgeleitet.| 
| platformVersion| string| Optional. Das Format dieser Zeichenfolge ist speziell für jede Plattform. Dieser Wert wird derzeit nicht verwendet.| 
| systemId| GUID| Erforderlich. Eindeutiger Bezeichner für die "app-Instanz" (Geräte-oder benutzerbezogene Kombination). Best Practice-Implementierung für eine app zum Generieren eines zufälligen GUIDs bei Installation/erste Ausführung ist und weiterhin diesen Wert bei nachfolgenden Ausführungen der app zu verwenden.| 
| titleId| 32-Bit-Ganzzahl ohne Vorzeichen| Erforderlich. Die ID der Titel des Spiels den Aufruf an den Dienst ausgeben.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>Siehe auch
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>Parent 

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Verweis   