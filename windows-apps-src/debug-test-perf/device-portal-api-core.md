---
ms.assetid: bfabd3d5-dd56-4917-9572-f3ba0de4f8c0
title: Referenz zu Kern-APIs des Device Portal
description: Hier erhalten Sie Informationen zu den Kern-REST-APIs für das Windows Device Portal, die Sie für den Zugriff auf die Daten und die programmatische Steuerung des Geräts verwenden können.
ms.custom: 19H1
ms.date: 04/19/2019
ms.topic: article
keywords: Windows 10, UWP, Geräteportal
ms.localizationpriority: medium
ms.openlocfilehash: 9e091cc7ad62f69b9e76541101555493609b8a06
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "75955309"
---
# <a name="device-portal-core-api-reference"></a>Referenz zu Kern-APIs des Device Portal

Alle Funktionen im Geräteportal basieren auf REST-APIs, die Entwickler direkt aufrufen können, um auf Ressourcen zuzugreifen und ihre Geräte programmgesteuert zu steuern.

## <a name="app-deployment"></a>App-Bereitstellung

### <a name="install-an-app"></a>Installieren einer App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine App installieren.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/app/packagemanager/package |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Paket   | (**erforderlich**) Der Dateiname des zu installierenden Pakets. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Die APPX- oder APPXBUNDLE-Datei sowie alle von der App benötigten Abhängigkeiten. 
- Das Zertifikat zum Signieren der App, wenn es sich um ein IoT- oder Windows-Desktop-Gerät handelt. Bei anderen Plattformen ist das Zertifikat nicht erforderlich. 

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="install-a-related-set"></a>Installieren einer verwandten Gruppe

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine [verwandte Gruppe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) installieren.

| Methode      | Anforderungs-URI |
| :------     | :------ |
| POST | /api/app/packagemanager/package |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Paket   | (**Erforderlich:** ) Die Dateinamen der zu installierenden Pakete. |

**Anforderungsheader**

- Keine

**Anforderungstext** 
- Füge Dateinamen optionaler Pakete „.opt“ hinzu, wenn du sie als Parameter angibst, z. B. „foo.appx.opt“ oder „bar.appxbundle.opt“. 
- Die APPX- oder APPXBUNDLE-Datei sowie alle von der App benötigten Abhängigkeiten. 
- Das Zertifikat zum Signieren der App, wenn es sich um ein IoT- oder Windows-Desktop-Gerät handelt. Bei anderen Plattformen ist das Zertifikat nicht erforderlich. 

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-an-app-in-a-loose-folder"></a>Registrieren einer App in einem losen Ordner

**Anforderung**

Mithilfe des folgenden Anforderungsformats können Sie eine App in einem losen Ordner registrieren.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/app/packagemanager/networkapp |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    }
}
```

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="register-a-related-set-in-loose-file-folders"></a>Registrieren einer verwandten Gruppe in losen Dateiordnern

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine [verwandte Gruppe](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/) in losen Ordnern registrieren.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/app/packagemanager/networkapp |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

```json
{
    "mainpackage" :
    {
        "networkshare" : "\\some\share\path",
        "username" : "optional_username",
        "password" : "optional_password"
    },
    "optionalpackages" :
    [
        {
            "networkshare" : "\\some\share\path2",
            "username" : "optional_username2",
            "password" : "optional_password2"
        },
        ...
    ]
}
```

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-app-installation-status"></a>Abrufen des App-Installationsstatus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status einer derzeit ausgeführten App-Installation abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/app/packagemanager/state |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | Das Ergebnis der letzten Bereitstellung |
| 204 | Die Installation läuft |
| 404 | Keine Installationsaktion wurde gefunden |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="uninstall-an-app"></a>Deinstallieren einer App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine App deinstallieren.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/app/packagemanager/package |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Paket   | (**Erforderlich**) PackageFullName (von GET /api/app/packagemanager/packages) der Ziel-App |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-installed-apps"></a>Abrufen installierter Apps

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste der auf dem System installierten Apps abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/app/packagemanager/packages |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält eine Liste der installierten Pakete mit zugehörigen Details. Die Vorlage für diese Antwort lautet wie folgt.
```json
{"InstalledPackages": [
    {
        "Name": string,
        "PackageFamilyName": string,
        "PackageFullName": string,
        "PackageOrigin": int, (https://msdn.microsoft.com/library/windows/desktop/dn313167(v=vs.85).aspx)
        "PackageRelativeId": string,
        "Publisher": string,
        "Version": {
            "Build": int,
            "Major": int,
            "Minor": int,
            "Revision": int
     },
     "RegisteredUsers": [
     {
        "UserDisplayName": string,
        "UserSID": string
     },...
     ]
    },...
]}
```
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="bluetooth"></a>Bluetooth

<hr>

### <a name="get-the-bluetooth-radios-on-the-machine"></a>Abrufen der Bluetooth-Geräte auf dem Computer

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine Liste der auf dem Computer installierten Bluetooth-Geräte abrufen. Dies kann auch mit denselben JSON-Daten zu einer WebSocket-Verbindung aktualisiert werden.
 
| Methode        | Anforderungs-URI |
| :------          | :------ |
| GET           | /api/bt/getradios |
| GET/WebSocket | /api/bt/getradios |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält ein JSON-Array von Bluetooth-Geräten, die mit dem Gerät verbunden sind.
```json
{"BluetoothRadios" : [
    {
        "BluetoothAddress" : int64,
        "DisplayName" : string,
        "HasUnknownUsbDevice" : boolean,
        "HasProblem" : boolean,
        "ID" : string,
        "ProblemCode" : int,
        "State" : string
    },...
]}
```
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | Beschreibung |
| :------             | :------ |
| 200              | OK |
| 4XX              | Fehlercodes |
| 5XX              | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="turn-the-bluetooth-radio-on-or-off"></a>Ein-/Ausschalten des Bluetooth-Geräts

**Anforderung**

Schaltet ein bestimmtes Bluetooth-Gerät ein oder aus.
 
| Methode | Anforderungs-URI |
| :------   | :------ |
| POST   | /api/bt/setradio |

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| ID            | (**Erforderlich:** ) Die Geräte-ID für das Bluetooth-Gerät; muss Base64-codiert sein. |
| Bundesland/Kanton         | (**Erforderlich:** ) Dies kann `"On"` oder `"Off"` sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | Beschreibung |
| :------             | :------ |
| 200              | OK |
| 4XX              | Fehlercodes |
| 5XX              | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

---
### <a name="get-a-list-of-paired-bluetooth-devices"></a>Abrufen einer Liste mit gekoppelten Bluetooth-Geräten

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine Liste der derzeit gekoppelten Bluetooth-Geräte abrufen. Dies kann mit denselben JSON-Daten zu einer WebSocket-Verbindung aktualisiert werden. Die Geräteliste kann sich während der Lebensdauer der WebSocket-Verbindung ändern. Bei jedem Update wird eine vollständige Liste der Geräte über die WebSocket-Verbindung gesendet.

| Methode        | Anforderungs-URI       |
| :---          | :---              |
| GET           | /api/bt/getpaired |
| GET/WebSocket | /api/bt/getpaired |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält ein JSON-Array von Bluetooth-Geräten, die derzeit gekoppelt sind.
```json
{"PairedDevices": [
    {
        "Name" : string,
        "ID" : string,
        "AudioConnectionStatus" : string
    },...
]}
```
Das Feld *AudioConnectionStatus* ist vorhanden, wenn das Gerät auf diesem System für Audiodaten verwendet werden kann. (Dies kann durch Richtlinien und optionale Komponenten beeinflusst werden.) Der *AudioConnectionStatus* ist entweder „Verbunden“ oder „Getrennt“.

---
### <a name="get-a-list-of-available-bluetooth-devices"></a>Abrufen einer Liste mit verfügbaren Bluetooth-Geräten

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine Liste der für die Kopplung verfügbaren Bluetooth-Geräte abrufen. Dies kann mit denselben JSON-Daten zu einer WebSocket-Verbindung aktualisiert werden. Die Geräteliste kann sich während der Lebensdauer der WebSocket-Verbindung ändern. Bei jedem Update wird eine vollständige Liste der Geräte über die WebSocket-Verbindung gesendet.

| Methode        | Anforderungs-URI          |
| :---          | :---                 |
| GET           | /api/bt/getavailable |
| GET/WebSocket | /api/bt/getavailable |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält ein JSON-Array von Bluetooth-Geräten, die derzeit für die Kopplung verfügbar sind.
```json
{"AvailableDevices": [
    {
        "Name" : string,
        "ID" : string
    },...
]}
```

---
### <a name="connect-a-bluetooth-device"></a>Verbinden eines Bluetooth-Geräts

**Anforderung**

Stellt eine Verbindung mit dem Gerät her, sofern das Gerät für Audiodaten auf diesem System verwendet werden kann. (Dies kann durch Richtlinien und optionale Komponenten beeinflusst werden.)

| Methode       | Anforderungs-URI           |
| :---         | :---                  |
| POST         | /api/bt/connectdevice |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :---          | :--- |
| ID            | (**Erforderlich:** ) Die Zuordnungsendpunkt-ID für das Bluetooth-Gerät; muss Base64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | Beschreibung |
| :---             | :--- |
| 200              | OK |
| 4XX              | Fehlercodes |
| 5XX              | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT


---
### <a name="disconnect-a-bluetooth-device"></a>Trennen eines Bluetooth-Geräts

**Anforderung**

Trennt die Verbindung des Geräts, sofern das Gerät für Audiodaten auf diesem System verwendet werden kann. (Dies kann durch Richtlinien und optionale Komponenten beeinflusst werden.)

| Methode       | Anforderungs-URI              |
| :---         | :---                     |
| POST         | /api/bt/disconnectdevice |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :---          | :--- |
| ID            | (**Erforderlich:** ) Die Zuordnungsendpunkt-ID für das Bluetooth-Gerät; muss Base64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode | Beschreibung |
| :---             | :--- |
| 200              | OK |
| 4XX              | Fehlercodes |
| 5XX              | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

---
## <a name="device-manager"></a>Geräte-Manager
<hr>

### <a name="get-the-installed-devices-on-the-machine"></a>Abrufen der auf dem Computer installierten Geräte

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste der auf dem Computer installierten Geräte abrufen.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/devicemanager/devices |

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält ein JSON-Array von Geräten, die mit dem Gerät verbunden sind.
```json
{"DeviceList": [
    {
        "Class": string,
        "Description": string,
        "ID": string,
        "Manufacturer": string,
        "ParentID": string,
        "ProblemCode": int,
        "StatusCode": int
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* IoT

<hr>

### <a name="get-data-on-connected-usb-deviceshubs"></a>Abrufen von Daten von verbundenen USB-Geräten/Hubs

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du eine Liste von USB-Deskriptoren für verbundene USB-Geräte und Hubs abrufen.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /ext/devices/usbdevices |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort ist ein JSON-Objekt, das die DeviceID für das USB-Gerät zusammen mit den USB-Deskriptoren und Portinformationen für Hubs enthält.
```json
{
    "DeviceList": [
        {
        "ID": string,
        "ParentID": string, // Will equal an "ID" within the list, or be blank
        "Description": string, // optional
        "Manufacturer": string, // optional
        "ProblemCode": int, // optional
        "StatusCode": int // optional
        },
        ...
    ]
}
```

**Beispiel für Rückgabedaten**
```json
{
    "DeviceList": [{
        "ID": "System",
        "ParentID": ""
    }, {
        "Class": "USB",
        "Description": "Texas Instruments USB 3.0 xHCI Host Controller",
        "ID": "PCI\\VEN_104C&DEV_8241&SUBSYS_1589103C&REV_02\\4&37085792&0&00E7",
        "Manufacturer": "Texas Instruments",
        "ParentID": "System",
        "ProblemCode": 0,
        "StatusCode": 25174026
    }, {
        "Class": "USB",
        "Description": "USB Composite Device",
        "DeviceDriverKey": "{36fc9e60-c465-11cf-8056-444553540000}\\0016",
        "ID": "USB\\VID_045E&PID_00DB\\8&2994096B&0&1",
        "Manufacturer": "(Standard USB Host Controller)",
        "ParentID": "USB\\VID_0557&PID_8021\\7&2E9A8711&0&4",
        "ProblemCode": 0,
        "StatusCode": 25182218
    }]
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

## <a name="dump-collection"></a>Absturzabbildsammlung

<hr>

### <a name="get-the-list-of-all-crash-dumps-for-apps"></a>Abrufen der Liste alle Absturzabbilder für Apps

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste aller verfügbaren Absturzabbilder für jede quergeladene App abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/dumps |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält eine Liste der Absturzabbilder für jede quergeladene Anwendung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="get-the-crash-dump-collection-settings-for-an-app"></a>Abrufen der Absturzabbildsammlungs-Einstellungen für eine App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Absturzabbildsammlungs-Einstellungen für eine quergeladene App abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashcontrol |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort weist das folgende Format auf.
```json
{"CrashDumpEnabled": bool}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="delete-a-crash-dump-for-a-sideloaded-app"></a>Löschen eines Absturzabbilds für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Absturzabbild einer quergeladenen App löschen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashdump |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App. |
| fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die gelöscht werden soll. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="disable-crash-dumps-for-a-sideloaded-app"></a>Deaktivieren der Absturzabbilder für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie Absturzabbilder für eine quergeladene App deaktivieren.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/debug/dump/usermode/crashcontrol |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="download-the-crash-dump-for-a-sideloaded-app"></a>Herunterladen des Absturzabbilds für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Absturzabbild einer quergeladenen App herunterladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/crashdump |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App. |
| fileName   | (**erforderlich**) Der Name der Absturzabbilddatei, die Sie herunterladen möchten. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält eine Absturzabbilddatei. Sie können die Absturzabbilddatei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="enable-crash-dumps-for-a-sideloaded-app"></a>Aktivieren der Absturzabbilder für eine quergeladene App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie Absturzabbilder für eine quergeladene App aktivieren.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/debug/dump/usermode/crashcontrol |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| packageFullname   | (**erforderlich**) Der vollständige Name des Pakets für die quergeladene App. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 

**Verfügbare Gerätefamilien**

* Windows Mobile (im Windows-Insider-Programm)
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="get-the-list-of-bugcheck-files"></a>Abrufen der Liste der Fehlerüberprüfungsdateien

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der Fehlerüberprüfungs-Minidumpdateien abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dumplist |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält eine Liste der Minidumpdateinamen und die Größen dieser Dateien. Diese Liste wird das folgende Format aufweisen. 
```json
{"DumpFiles": [
    {
        "FileName": string,
        "FileSize": int
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="download-a-bugcheck-dump-file"></a>Herunterladen einer Fehlerüberprüfungs-Speicherabbilddatei

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Fehlerüberprüfungs-Speicherabbilddatei herunterladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/dump |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Dateiname   | (**erforderlich**) Der Dateiname der Speicherabbilddatei. Sie finden diesen mithilfe der API zum Abrufen der Speicherabbildliste. |


**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Speicherabbilddatei. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-the-bugcheck-crash-control-settings"></a>Abrufen der CrashControl-Fehlerüberprüfungseinstellungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die CrashControl-Fehlerüberprüfungseinstellungen abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/kernel/crashcontrol |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die CrashControl-Einstellungen. Weitere Informationen zu [CrashControl](https://technet.microsoft.com/library/cc951703.aspx) finden Sie im Artikel. Die Vorlage für die Antwort lautet wie folgt.
```json
{
    "autoreboot": bool (0 or 1),
    "dumptype": int (0 to 4),
    "maxdumpcount": int,
    "overwrite": bool (0 or 1)
}
```

**Abbildtypen**

0: Deaktiviert

1: Vollständiges Speicherabbild (erfasst den gesamten verwendeten Arbeitsspeicher)

2: Kernelspeicherabbild (ignoriert den Benutzermodusspeicher)

3: Eingeschränkter Kernelminidump

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-a-live-kernel-dump"></a>Abrufen eines Live-Kernelspeicherabbilds

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein Live-Kernelspeicherabbild abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/livekernel |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält das vollständige Kernelmodus-Speicherabbild. Sie können diese Datei mithilfe von WinDbg untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-a-dump-from-a-live-user-process"></a>Abrufen eines Speicherabbilds von einem Livebenutzerprozess

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das Speicherabbild von einem Livebenutzerprozess abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/debug/dump/usermode/live |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| pid   | (**erforderlich**) Die eindeutige Prozess-ID für den betreffenden Prozess. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Prozesssicherung. Sie können diese Datei mit WinDbg oder Visual Studio untersuchen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="set-the-bugcheck-crash-control-settings"></a>Festlegen der CrashControl-Fehlerüberprüfungseinstellungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Einstellungen zum Sammeln von Fehlerüberprüfungsdaten festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/debug/dump/kernel/crashcontrol |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| autoreboot   | (**optional**) True oder False. Dieser Parameter gibt an, ob das System nach einem Fehler oder Einfrieren automatisch neu gestartet wird. |
| dumptype   | (**optional**) Der Speicherabbildtyp. Die unterstützten Werte finden Sie unter [CrashDumpType-Enumeration](https://docs.microsoft.com/previous-versions/azure/reference/dn802457(v=azure.100)).|
| maxdumpcount   | (**optional**) Die maximale Anzahl der zu speichernden Speicherabbilder. |
| overwrite   | (**optional**) True oder False. Dieser Parameter gibt an, ob alte Speicherabbilder überschrieben werden, wenn der durch *maxdumpcount* angegebene Höchstwert für die Anzahl von Speicherabbildern erreicht wurde. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

## <a name="etw"></a>ETW

<hr>

### <a name="create-a-realtime-etw-session-over-a-websocket"></a>Erstellen einer Echtzeit-ETW-Sitzung über ein Websocket

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Echtzeit-ETW-Sitzung erstellen. Dies erfolgt über ein Websocket.  ETW-Ereignisse werden auf dem Server zusammengefasst und einmal pro Sekunde an den Client gesendet. 
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET/WebSocket | /api/etw/session/realtime |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die ETW-Ereignisse von den aktivierten Anbietern.  ETW-WebSocket-Befehle finden Sie unten. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

### <a name="etw-websocket-commands"></a>ETW-WebSocket-Befehle
Diese Befehle werden vom Client an den Server gesendet.

| Befehl | Beschreibung |
| :----- | :----- |
| Anbieter *{guid}* aktivieren *{level}* | Den durch *{guid}* (ohne Klammern) markierten Anbieter auf der angegebenen Ebene aktivieren. *{level}* ist ein **int** von 1 (am wenigsten detailliert) bis 5 (ausführlich). |
| Anbieter *{guid}* deaktivieren | Den durch *{guid}* (ohne Klammern) markierten Anbieter deaktivieren. |

Diese Antworten werden vom Server an den Client gesendet. Diese werden als Text gesendet, und erhalten Sie das folgende Format durch eine JSON-Analyse.
```json
{
    "Events":[
        {
            "Timestamp": int,
            "ProviderName": string,
            "ID": int, 
            "TaskName": string,
            "Keyword": int,
            "Level": int,
            payload objects...
        },...
    ],
    "Frequency": int
}
```

Nutzlast-Objekte sind zusätzliche Schlüssel-Wert-Paare (Zeichenkette:Zeichenkette), die im ursprünglichen ETW-Ereignis bereitgestellt werden.

Beispiel:
```json
{
    "ID" : 42, 
    "Keyword" : 9223372036854775824, 
    "Level" : 4, 
    "Message" : "UDPv4: 412 bytes transmitted from 10.81.128.148:510 to 132.215.243.34:510. ",
    "PID" : "1218", 
    "ProviderName" : "Microsoft-Windows-Kernel-Network", 
    "TaskName" : "KERNEL_NETWORK_TASK_UDPIP", 
    "Timestamp" : 131039401761757686, 
    "connid" : "0", 
    "daddr" : "132.245.243.34", 
    "dport" : "500", 
    "saddr" : "10.82.128.118", 
    "seqnum" : "0", 
    "size" : "412", 
    "sport" : "500"
}
```

<hr>

### <a name="enumerate-the-registered-etw-providers"></a>Auflisten der registrierten ETW-Anbieter

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die registrierten Anbieter auflisten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/etw/providers |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Liste der ETW-Anbieter. Diese Liste enthält den Anzeigenamen und die GUID für jeden Anbieter im folgenden Format.
```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
|  200 | OK | 

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="enumerate-the-custom-etw-providers-exposed-by-the-platform"></a>Auflisten der benutzerdefinierten ETW-Anbieter, die von der Plattform verfügbar gemacht werden.

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die registrierten Anbieter auflisten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/etw/customproviders |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

200 OK. Die Antwort enthält die Liste der ETW-Anbieter. Diese Liste enthält den Anzeigenamen und die GUID für jeden Anbieter.

```json
{"Providers": [
    {
        "GUID": string, (GUID)
        "Name": string
    },...
]}
```

**Statuscode**

- Standardstatuscodes

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

## <a name="location"></a>Speicherort

<hr>

### <a name="get-location-override-mode"></a>Abrufen des Überschreibungsmodus für den Standort

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du den Überschreibungsstatus für den Gerätestandortstapel abrufen. Damit dieser Aufruf erfolgreich ausgeführt werden kann, muss der Entwicklermodus aktiv sein.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /ext/location/override |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält den Überschreibungsstatus des Geräts im folgenden Format. 

```json
{"Override" : bool}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
|  200 | OK | 
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

### <a name="set-location-override-mode"></a>Festlegen des Überschreibungsmodus für den Standort

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du den Überschreibungsstatus für den Gerätestandortstapel festlegen. Bei Aktivierung ermöglicht der Standortstapel das Einfügen einer Position. Damit dieser Aufruf erfolgreich ausgeführt werden kann, muss der Entwicklermodus aktiv sein.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

```json
{"Override" : bool}
```

**Antwort**

Die Antwort enthält den Überschreibungsstatus, auf den das Gerät festgelegt wurde, im folgenden Format. 

```json
{"Override" : bool}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

### <a name="get-the-injected-position"></a>Abrufen der eingefügten Position

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du den eingefügten (gespooften) Gerätestandort abrufen. Es muss ein eingefügter Standort festgelegt sein, andernfalls wird ein Fehler ausgelöst.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /ext/location/position |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die aktuell eingefügten Breiten- und Längengradwerte im folgenden Format. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

|  HTTP-Statuscode      | Beschreibung | 
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

### <a name="set-the-injected-position"></a>Festlegen der eingefügten Position

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du den eingefügten (gespooften) Gerätestandort festlegen. Der Überschreibungsmodus für den Standort muss zunächst auf dem Gerät aktiviert werden, und der festgelegte Standort muss gültig sein, andernfalls wird ein Fehler ausgelöst.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| PUT | /ext/location/override |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Antwort**

Die Antwort enthält den festgelegten Standort im folgenden Format. 

```json
{
    "Latitude" : double,
    "Longitude" : double
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="os-information"></a>Betriebssysteminformationen

<hr>

### <a name="get-the-machine-name"></a>Abrufen des Computernamens

**Anforderung**

Sie können den Namen eines Computers durch Verwendung des folgenden Anforderungsformats abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/os/machinename |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält den Namen des Computers im folgenden Format. 

```json
{"ComputerName": string}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-operating-system-information"></a>Abrufen der Betriebssysteminformationen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Betriebssysteminformationen für einen Computer abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/os/info |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Betriebssysteminformationen im folgenden Format.

```json
{
    "ComputerName": string,
    "OsEdition": string,
    "OsEditionId": int,
    "OsVersion": string,
    "Platform": string
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="get-the-device-family"></a>Abrufen der Gerätefamilie 

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Gerätefamilie (Xbox, Smartphone, Desktop usw.) abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/os/devicefamily |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Gerätefamilie (SKU – Desktop, Xbox, usw.).

```json
{
   "DeviceType" : string
}
```

„DeviceType“ sieht wie folgt aus: „Windows.Xbox“, „Windows.Desktop“ usw. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-the-machine-name"></a>Festlegen des Computernamens

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Namen eines Computers festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/os/machinename |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Name | (**erforderlich**) Der neue Name für den Computer. Dieser sollte base64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="user-information"></a>Benutzerinformationen

<hr>

### <a name="get-the-active-user"></a>Abrufen des aktiven Benutzers

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du den Namen des aktiven Gerätebenutzers abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/users/activeuser |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält Benutzerinformationen im folgenden Format. 

Bei Erfolg: 
```json
{
    "UserDisplayName" : string, 
    "UserSID" : string
}
```
Bei Misserfolg:
```json
{
    "Code" : int, 
    "CodeText" : string, 
    "Reason" : string, 
    "Success" : bool
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

## <a name="performance-data"></a>Leistungsdaten

<hr>

### <a name="get-the-list-of-running-processes"></a>Abrufen der Liste der ausgeführten Prozesse

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der derzeit ausgeführten Prozesse abrufen.  Dies kann auch zu einer WebSocket-Verbindung aktualisiert werden, wobei einmal pro Sekunde die gleichen JSON-Daten per Push an den Client gesendet werden. 
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/resourcemanager/processes |
| GET/WebSocket | /api/resourcemanager/processes |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält eine Liste der Prozesse mit Details für jeden Prozess. Die Informationen sind im JSON-Format und haben die folgende Vorlage.
```json
{"Processes": [
    {
        "CPUUsage": float,
        "ImageName": string,
        "PageFileUsage": long,
        "PrivateWorkingSet": long,
        "ProcessId": int,
        "SessionId": int,
        "UserName": string,
        "VirtualSize": long,
        "WorkingSetSize": long
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="get-the-system-performance-statistics"></a>Abrufen der Leistungsstatistik des Systems

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Leistungsstatistik des Systems abrufen. Diese Informationen umfassen z. B. Lese- und Schreibzyklen sowie die Menge des genutzten Arbeitsspeichers.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/resourcemanager/systemperf |
| GET/WebSocket | /api/resourcemanager/systemperf |

Dies kann auch auf eine WebSocket-Verbindung aktualisiert werden.  Sie stellt unten einmal pro Sekunde die gleichen JSON-Daten bereit. 

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Leistungsstatistik für das System, z. B. CPU- und GPU-Nutzung, Speicherzugriff und Netzwerkzugriff. Diese Informationen sind im JSON-Format und haben die folgende Vorlage.
```json
{
    "AvailablePages": int,
    "CommitLimit": int,
    "CommittedPages": int,
    "CpuLoad": int,
    "IOOtherSpeed": int,
    "IOReadSpeed": int,
    "IOWriteSpeed": int,
    "NonPagedPoolPages": int,
    "PageSize": int,
    "PagedPoolPages": int,
    "TotalInstalledInKb": int,
    "TotalPages": int,
    "GPUData": 
    {
        "AvailableAdapters": [{ (One per detected adapter)
            "DedicatedMemory": int,
            "DedicatedMemoryUsed": int,
            "Description": string,
            "SystemMemory": int,
            "SystemMemoryUsed": int,
            "EnginesUtilization": [ float,... (One per detected engine)]
        },...
    ]},
    "NetworkingData": {
        "NetworkInBytes": int,
        "NetworkOutBytes": int
    }
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="power"></a>Leistung

<hr>

### <a name="get-the-current-battery-state"></a>Abrufen des aktuellen Akkustatus

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den aktuellen Akkustatus abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/battery |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Informationen zum aktuellen Akkustatus werden im folgenden Format zurückgegeben.
```json
{
    "AcOnline": int (0 | 1),
    "BatteryPresent": int (0 | 1),
    "Charging": int (0 | 1),
    "DefaultAlert1": int,
    "DefaultAlert2": int,
    "EstimatedTime": int,
    "MaximumCapacity": int,
    "RemainingCapacity": int
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="get-the-active-power-scheme"></a>Abrufen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/activecfg |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Das aktive Energieschema hat das folgende Format.
```json
{"ActivePowerScheme": string (guid of scheme)}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-the-sub-value-for-a-power-scheme"></a>Abrufen des Unterwerts für ein Energieschema

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Unterwert für ein Energieschema abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/cfg/ *<power scheme path>* |

Optionen:
- SCHEME_CURRENT

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

Eine vollständige Liste der verfügbaren Energiezustände ist auf einzelne Anwendungen bezogen und enthält die Einstellungen zur Kennzeichnung verschiedener Energiezustände wie niedriger und kritischer Ladezustand. 

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-the-power-state-of-the-system"></a>Abrufen des Energiestatus des Systems

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Energiestatus des Systems überprüfen. So können Sie überprüfen, ob es sich im Energiesparmodus befindet.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/state |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Informationen zum Energiezustand haben die folgende Vorlage.
```json
{"LowPowerState" : false, "LowPowerStateAvailable" : true }
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="set-the-active-power-scheme"></a>Festlegen des aktiven Energieschemas

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie das aktive Energieschema festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/power/activecfg |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| scheme | (**erforderlich**) Die GUID des Schemas, das Sie als das aktive Energieschema für das System festlegen möchten. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="set-the-sub-value-for-a-power-scheme"></a>Festlegen des Unterwerts für ein Energieschema

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Unterwert für ein Energieschema festlegen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/power/cfg/ *<power scheme path>* |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| valueAC | (**erforderlich**) Der für den Netzbetrieb zu verwendende Wert. |
| valueDC | (**erforderlich**) Der für den Akkubetrieb zu verwendende Wert. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-a-sleep-study-report"></a>Abrufen eines Berichts zur Ruhezustandsuntersuchung

**Anforderung**

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/report |

Mit dem folgenden Anforderungsformat können Sie einen Bericht zur Ruhezustandsuntersuchung abrufen.

**URI-Parameter**
| URI-Parameter | Beschreibung |
| :------          | :------ |
| Dateiname | (**erforderlich**) Der vollständige Name für die Datei, die Sie herunterladen möchten. Dieser Wert sollte hex64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort ist eine Datei mit der Ruhezustandsuntersuchung. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="enumerate-the-available-sleep-study-reports"></a>Auflisten der verfügbaren Berichte zu Ruhezustandsuntersuchungen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die verfügbaren Berichte zu Ruhezustandsuntersuchungen auflisten
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/reports |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Liste der verfügbaren Berichte hat die folgende Vorlage.

```json
{"Reports": [
    {
        "FileName": string
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

### <a name="get-the-sleep-study-transform"></a>Abrufen der Transformation der Ruhezustandsuntersuchung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Transformation der Ruhezustandsuntersuchung abrufen. Diese Transformation ist eine XSLT-Datei, die den Bericht zur Ruhezustandsuntersuchung in ein lesbares XML-Format konvertiert.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/power/sleepstudy/transform |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die Transformation der Ruhezustandsuntersuchung.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* IoT

<hr>

## <a name="remote-control"></a>Remotesteuerung

<hr>

### <a name="restart-the-target-computer"></a>Neustarten des Zielcomputers

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Zielcomputer neu starten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/control/restart |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="shut-down-the-target-computer"></a>Herunterfahren des Zielcomputers

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Zielcomputer herunterfahren.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/control/shutdown |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="task-manager"></a>Task-Manager

<hr>

### <a name="start-a-modern-app"></a>Starten einer Modern App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Modern App starten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/taskmanager/app |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| appid   | (**erforderlich**) Die PRAID für die App, die gestartet werden soll. Dieser Wert sollte hex64-codiert sein. |
| Paket   | (**erforderlich**) Der vollständige Name für das App-Paket, das Sie starten möchten. Dieser Wert sollte hex64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="stop-a-modern-app"></a>Beenden einer Modern App

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Modern App beenden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/taskmanager/app |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :---          | :--- |
| Paket   | (**erforderlich**) Der vollständige Name des App-Pakets, das Sie beenden möchten. Dieser Wert sollte hex64-codiert sein. |
| forcestop   | (**optional**) Der Wert **yes** gibt an, dass das Beenden sämtlicher Prozesse erzwungen werden soll. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="kill-process-by-pid"></a>Beenden eines Prozesses anhand der PID

**Anforderung**

Mit dem folgenden Anforderungsformat kannst du einen Prozess beenden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/taskmanager/process |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| pid   | (**Erforderlich:** ) Die eindeutige Prozess-ID des zu beendenden Prozesses. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

## <a name="networking"></a>Netzwerk

<hr>

### <a name="get-the-current-ip-configuration"></a>Abrufen der aktuellen IP-Konfiguration

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die aktuelle IP-Konfiguration abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/networking/ipconfig |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Antwort enthält die IP-Konfiguration in der folgenden Vorlage.

```json
{"Adapters": [
    {
        "Description": string,
        "HardwareAddress": string,
        "Index": int,
        "Name": string,
        "Type": string,
        "DHCP": {
            "LeaseExpires": int, (timestamp)
            "LeaseObtained": int, (timestamp)
            "Address": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "WINS": {(WINS is optional)
            "Primary": {
                "IpAddress": string,
                "Mask": string
            },
            "Secondary": {
                "IpAddress": string,
                "Mask": string
            }
        },
        "Gateways": [{ (always 1+)
            "IpAddress": "10.82.128.1",
            "Mask": "255.255.255.255"
            },...
        ],
        "IpAddresses": [{ (always 1+)
            "IpAddress": "10.82.128.148",
            "Mask": "255.255.255.0"
            },...
        ]
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="set-a-static-ip-address-ipv4-configuration"></a>Festlegen einer statischen IP-Adresse (IPv4-Konfiguration)

**Anforderung**

Legt die IPv4-Konfiguration mit statischer IP-Adresse und DNS fest. Wenn keine statische IP-Adresse angegeben wurde, wird DHCP aktiviert. Wenn eine statische IP-Adresse angegeben wird, muss auch DNS angegeben werden.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| PUT | /api/networking/ipv4config |


**URI-Parameter**

| URI-Parameter | Beschreibung |
| :---          | :--- |
| AdapterName | (**Erforderlich:** ) Die GUID der Netzwerkschnittstelle. |
| IPAddress | Die festzulegende statische IP-Adresse. |
| SubnetMask | (**Erforderlich**, wenn *IPAddress* nicht NULL ist:) Die statische Subnetzmaske. |
| DefaultGateway | (**Erforderlich**, wenn *IPAddress* nicht NULL ist:) Das statische Standardgateway. |
| PrimaryDNS | (**Erforderlich**, wenn *IPAddress* nicht NULL ist:) Das statische primäre DNS, das festgelegt werden soll. |
| SecondayDNS | (**Erforderlich**, wenn *PrimaryDNS* nicht NULL ist:) Das statische sekundäre DNS, das festgelegt werden soll. |

Aus Gründen der Übersichtlichkeit solltest du zum Festlegen einer Schnittstelle auf DHCP nur den `AdapterName` der Verbindung serialisieren:

```json
{
    "AdapterName":"{82F86C1B-2BAE-41E3-B08D-786CA44FEED7}"
}
```

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-network-interfaces"></a>Auflisten der Drahtlos-Netzwerkschnittstellen

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Drahtlos-Netzwerkschnittstellen auflisten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wifi/interfaces |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Eine Liste der verfügbaren Drahtlosschnittstellen mit Details im folgenden Format.

```json 
{"Interfaces": [{
    "Description": string,
    "GUID": string (guid with curly brackets),
    "Index": int,
    "ProfilesList": [
        {
            "GroupPolicyProfile": bool,
            "Name": string, (Network currently connected to)
            "PerUserProfile": bool
        },...
    ]
    }
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="enumerate-wireless-networks"></a>Auflisten von Drahtlosnetzwerken

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Liste der Drahtlosnetzwerke an der angegebenen Schnittstelle auflisten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wifi/networks |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Schnittstelle   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Suchen nach Drahtlosnetzwerken verwendet werden soll, ohne Klammern. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Liste der an der angegebenen *interface* gefundenen Drahtlosnetzwerke. Diese enthält Details für die Netzwerke im folgenden Format.

```json
{"AvailableNetworks": [
    {
        "AlreadyConnected": bool,
        "AuthenticationAlgorithm": string, (WPA2, etc)
        "Channel": int,
        "CipherAlgorithm": string, (for example, AES)
        "Connectable": int, (0 | 1)
        "InfrastructureType": string,
        "ProfileAvailable": bool,
        "ProfileName": string,
        "SSID": string,
        "SecurityEnabled": int, (0 | 1)
        "SignalQuality": int,
        "BSSID": [int,...],
        "PhysicalTypes": [string,...]
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="connect-and-disconnect-to-a-wi-fi-network"></a>Herstellen der Verbindung mit einem WLAN-Netzwerk und Trennen der Verbindung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Verbindung mit einem WLAN-Netzwerk herstellen oder die Verbindung trennen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/wifi/network |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Schnittstelle   | (**erforderlich**) Die GUID für die Netzwerkschnittstelle, die zum Herstellen der Verbindung mit dem Netzwerk verwendet werden soll. |
| op   | (**erforderlich**) Gibt die durchzuführende Aktion an. Mögliche Werte sind „connect“ und „disconnect“.|
| ssid   | (**erforderlich, wenn *op* == connect**) Die SSID des Netzwerks, mit dem die Verbindung hergestellt werden soll. |
| key   | (**Erforderlich, wenn *op* == connect und Netzwerk Authentifizierung erfordert**:) Der gemeinsam verwendete Schlüssel. |
| createprofile | (**erforderlich**) Erstellen Sie ein Profil für das Netzwerk auf dem Gerät.  Dadurch stellt das Gerät künftig automatisch eine Verbindung zum Netzwerk her. Dies kann **ja** oder **nein** sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-a-wi-fi-profile"></a>Löschen eines WLAN-Profils

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein Profil löschen, das einem Netzwerk an einer bestimmten Schnittstelle zugeordnet ist.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/wifi/profile |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Schnittstelle   | (**erforderlich**) Die GUID der Netzwerkschnittstelle, die dem zu löschenden Profil zugeordnet ist. |
| Profil   | (**erforderlich**) Der Name des zu löschenden Profils. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

## <a name="windows-error-reporting-wer"></a>Windows-Fehlerberichterstattung (WER)

<hr>

### <a name="download-a-windows-error-reporting-wer-file"></a>Herunterladen einer WER-Datei (Windows Error Reporting)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WER-Datei herunterladen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wer/report/file |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Benutzer   | (**erforderlich**) Der dem Bericht zugeordnete Benutzername. |
| Typ   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten. |
| Name   | (**erforderlich**) Der Name des Berichts. Dieser sollte base64-codiert sein. |
| Datei   | (**erforderlich**) Der Name der Datei des Berichts, die heruntergeladen werden soll. Dieser sollte base64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Antwort enthält die angeforderte Datei. 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="enumerate-files-in-a-windows-error-reporting-wer-report"></a>Auflisten von Dateien in einem Bericht zur Windows-Fehlerberichterstattung (WER)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die Dateien in einem WER-Bericht auflisten.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wer/report/files |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Benutzer   | (**erforderlich**) Der dem Bericht zugeordnete Benutzer. |
| Typ   | (**erforderlich**) Der Typ des Berichts. Dieser kann **queried** oder **archived** lauten. |
| Name   | (**erforderlich**) Der Name des Berichts. Dieser sollte base64-codiert sein. |

**Anforderungsheader**

- Keine

**Anforderungstext**

```json
{"Files": [
    {
        "Name": string, (Filename, not base64 encoded)
        "Size": int (bytes)
    },...
]}
```

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="list-the-windows-error-reporting-wer-reports"></a>Auflisten der Berichte zur Windows-Fehlerberichterstattung (WER)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie die WER-Berichte abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wer/reports |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die WER-Berichte in folgendem Format.

```json
{"WerReports": [
    {
        "User": string,
        "Reports": [
            {
                "CreationTime": int,
                "Name": string, (not base64 encoded)
                "Type": string ("Queue" or "Archive")
            },
    },...
]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Desktop
* HoloLens
* IoT

<hr>

## <a name="windows-performance-recorder-wpr"></a>Windows Performance Recorder (WPR) 

<hr>

### <a name="start-tracing-with-a-custom-profile"></a>Starten der Ablaufverfolgung mit einem benutzerdefinierten Profil

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie ein WPR-Profil hochladen und die Ablaufverfolgung mit diesem Profil starten.  Es kann immer nur eine Spur ausgeführt werden. Das Profil bleibt nicht auf dem Gerät. 
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/wpr/customtrace |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Ein multipart-konformer HTTP-Text, der das benutzerdefinierte WPR-Profil enthält.

**Antwort**

Der Status der WPR-Sitzung im folgenden Format.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="start-a-boot-performance-tracing-session"></a>Starten einer Startleistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Start-Ablaufverfolgungssitzung starten. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/wpr/boottrace |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Profil   | (**erforderlich**) Dieser Parameter ist beim Starten erforderlich. Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Beim Start gibt diese API den Status der WPR-Sitzung im folgenden Format zurück.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="stop-a-boot-performance-tracing-session"></a>Beenden einer Startleistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Start-Ablaufverfolgungssitzung beenden. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wpr/boottrace |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

-  Keine.  **Hinweis:** Hierbei handelt es sich um einen zeitintensiven Vorgang.  Er wird wieder verfügbar, wenn der ETL-Schreibvorgang auf der Festplatte abgeschlossen ist.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="start-a-performance-tracing-session"></a>Starten einer Leistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Ablaufverfolgungssitzung starten. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.  Es kann immer nur eine Spur ausgeführt werden. 
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/wpr/trace |


**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Profil   | (**erforderlich**) Der Name des Profils, das eine Leistungs-Ablaufverfolgungssitzung starten soll. Die möglichen Profile werden in „perfprofiles/profiles.json“ gespeichert. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Beim Start gibt diese API den Status der WPR-Sitzung im folgenden Format zurück.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="stop-a-performance-tracing-session"></a>Beenden einer Leistungs-Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine WPR-Ablaufverfolgungssitzung beenden. Diese wird auch als Leistungs-Ablaufverfolgungssitzung bezeichnet.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wpr/trace |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine.  **Hinweis:** Hierbei handelt es sich um einen zeitintensiven Vorgang.  Er wird wieder verfügbar, wenn der ETL-Schreibvorgang auf der Festplatte abgeschlossen ist.  

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="retrieve-the-status-of-a-tracing-session"></a>Abrufen des Status einer Ablaufverfolgungssitzung

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie den Status der aktuellen WPR-Sitzung abrufen.
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wpr/status |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Der Status der WPR-Ablaufverfolgungssitzung im folgenden Format.

```json
{
    "SessionType": string, (Running or Idle) 
    "State": string (normal or boot)
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="list-completed-tracing-sessions-etls"></a>Aufführen abgeschlossener Ablaufverfolgungssitzungen (ETLs)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Liste mit ETL-Ablaufverfolgungen für das Gerät abrufen. 

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wpr/tracefiles |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

Die Liste mit den abgeschlossenen Ablaufverfolgungssitzungen wird im folgenden Format bereitgestellt:

```json
{"Items": [{
    "CurrentDir": string (filepath),
    "DateCreated": int (File CreationTime),
    "FileSize": int (bytes),
    "Id": string (filename),
    "Name": string (filename),
    "SubPath": string (filepath),
    "Type": int
}]}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="download-a-tracing-session-etl"></a>Herunterladen einer Ablaufverfolgungssitzung (ETL)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Ablaufverfolgungsdatei (Systemstart- oder Benutzermodus-Ablaufverfolgung) herunterladen. 

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/wpr/tracefile |


**URI-Parameter**

Im Anforderungs-URI kann der folgende zusätzliche Parameter angegeben werden:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Dateiname   | (**erforderlich**) Der Name der herunterzuladenden ETL-Ablaufverfolgung.  Diese befinden sich unter „/api/wpr/tracefiles“. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

### <a name="delete-a-tracing-session-etl"></a>Löschen einer Ablaufverfolgungssitzung (ETL)

**Anforderung**

Mit dem folgenden Anforderungsformat können Sie eine Ablaufverfolgungsdatei (Systemstart- oder Benutzermodus-Ablaufverfolgung) löschen. 

| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/wpr/tracefile |


**URI-Parameter**

Im Anforderungs-URI kann der folgende zusätzliche Parameter angegeben werden:

| URI-Parameter | Beschreibung |
| :------          | :------ |
| Dateiname   | (**erforderlich**) Der Name der zu löschenden ETL-Ablaufverfolgung.  Diese befinden sich unter „/api/wpr/tracefiles“. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Gibt die ETL-Ablaufverfolgungsdatei zurück.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* IoT

<hr>

## <a name="dns-sd-tags"></a>DNS-SD-Tags 

<hr>

### <a name="view-tags"></a>Anzeigen von Tags

**Anforderung**

Anzeigen der derzeit für das Gerät angewendeten Tags.  Diese werden über DNS-SD-TXT-Datensätze im T-Schlüssel angekündigt.  
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/dns-sd/tags |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort** Die derzeit angewendeten Tags im folgenden Format. 
```json
 {
    "tags": [
        "tag1", 
        "tag2", 
        ...
     ]
}
```

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 5XX | Serverfehler |


**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tags"></a>Löschen von Tags

**Anforderung**

Löschen aller Tags derzeit von DNS-SD angekündigten Tags.   
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tags |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**
 - Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 5XX | Serverfehler |


**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

<hr>

### <a name="delete-tag"></a>Löschen eines Tags

**Anforderung**

Löschen eines derzeit von DNS-SD angekündigten Tags.   
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/dns-sd/tag |


**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| tagValue | (**Erforderlich**) Das zu entfernende Tag. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**
 - Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |


**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT
 
<hr>

### <a name="add-a-tag"></a>Hinzufügen eines Tags

**Anforderung**

Hinzufügen eines Tags zur DNS-SD-Ankündigung.   
 
| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/dns-sd/tag |


**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| tagValue | (**Erforderlich**) Das hinzuzufügende Tag. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**
 - Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 401 | Überlauf des Tagbereichs.  Tritt auf, wenn das vorgeschlagene Tag zu lang für den resultierenden DNS-SD-Dienstdatensatz ist. |


**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* Xbox
* HoloLens
* IoT

## <a name="app-file-explorer"></a>App-Datei-Explorer

<hr>

### <a name="get-known-folders"></a>Abrufen bekannter Ordner

**Anforderung**

Abrufen einer Liste zugänglicher Ordner der obersten Ebene.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/filesystem/apps/knownfolders |


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort** Die verfügbaren Ordner im folgenden Format: 
```json
 {"KnownFolders": [
    "folder0",
    "folder1",...
]}
```
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | Bereitstellungsanforderung akzeptiert und wird verarbeitet |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |


**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT

<hr>

### <a name="get-files"></a>Abrufen von Dateien

**Anforderung**

Abrufen einer Liste von Dateien in einem Ordner.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/filesystem/apps/files |


**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| knownfolderid | (**Erforderlich**) Das Verzeichnis auf oberster Ebene, das die Liste der Dateien enthalten soll. Verwenden Sie **LocalAppData** für den Zugriff auf quergeladene Apps. |
| packagefullname | (**Erforderlich, wenn *knownfolderid* == LocalAppData:** ) Der vollständige Name des gewünschten Pakets der App. |
| Pfad | (**Optional**) Das Unterverzeichnis in dem oben angegebenen Ordner oder Paket. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort** Die verfügbaren Ordner im folgenden Format: 
```json
{"Items": [
    {
        "CurrentDir": string (folder under the requested known folder),
        "DateCreated": int,
        "FileSize": int (bytes),
        "Id": string,
        "Name": string,
        "SubPath": string (present if this item is a folder, this is the name of the folder),
        "Type": int
    },...
]}
```
**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT

<hr>

### <a name="download-a-file"></a>Herunterladen einer Datei

**Anforderung**

Abrufen einer Datei aus einem bekannten Ordner oder aus „appLocalData“

| Methode      | Anforderungs-URI |
| :------     | :----- |
| GET | /api/filesystem/apps/file |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| knownfolderid | (**Erforderlich**) Das Verzeichnis auf oberster Ebene, in das Sie Dateien herunterladen möchten. Verwenden Sie **LocalAppData** für den Zugriff auf quergeladene Apps. |
| Dateiname | (**Erforderlich**) Der Name der Datei, die heruntergeladen wird. |
| packagefullname | (**Erforderlich, wenn *knownfolderid* == LocalAppData:** ) Der vollständige Name des gewünschten Pakets. |
| Pfad | (**Optional**) Das Unterverzeichnis in dem oben angegebenen Ordner oder Paket. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Die angeforderte Datei, sofern vorhanden

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | Die angeforderte Datei |
| 404 | Datei nicht gefunden |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT

<hr>

### <a name="rename-a-file"></a>Umbenennen einer Datei

**Anforderung**

Umbenennen einer Datei in einem Ordner.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/filesystem/apps/rename |


**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| knownfolderid | (**Erforderlich**) Das übergeordnete Verzeichnis, in dem sich die Datei befindet. Verwenden Sie **LocalAppData** für den Zugriff auf quergeladene Apps. |
| Dateiname | (**Erforderlich**) Der ursprüngliche Name der umzubenennenden Datei. |
| newfilename | (**erforderlich**) Der neue Name der Datei.|
| packagefullname | (**Erforderlich, wenn *knownfolderid* == LocalAppData:** ) Der vollständige Name des gewünschten Pakets der App. |
| Pfad | (**Optional**) Das Unterverzeichnis in dem oben angegebenen Ordner oder Paket. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |. Datei umbenannt
| 404 | Datei nicht gefunden |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT

<hr>

### <a name="delete-a-file"></a>Löschen einer Datei

**Anforderung**

Löschen einer Datei in einem Ordner.

| Methode      | Anforderungs-URI |
| :------     | :----- |
| DELETE | /api/filesystem/apps/file |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| knownfolderid | (**Erforderlich**) Das Verzeichnis auf oberster Ebene, in dem Sie Dateien löschen möchten. Verwenden Sie **LocalAppData** für den Zugriff auf quergeladene Apps. |
| Dateiname | (**erforderlich**) Der Name der zu löschenden Datei. |
| packagefullname | (**Erforderlich, wenn *knownfolderid* == LocalAppData:** ) Der vollständige Name des gewünschten Pakets der App. |
| Pfad | (**Optional**) Das Unterverzeichnis in dem oben angegebenen Ordner oder Paket. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

- Keine 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |. Die Datei wird gelöscht. |
| 404 | Datei nicht gefunden |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT

<hr>

### <a name="upload-a-file"></a>Hochladen einer Datei

**Anforderung**

Hochladen einer Datei in einen Ordner.  Dadurch wird eine vorhandene Datei mit demselben Namen überschrieben, es werden jedoch keine neuen Ordner erstellt. 

| Methode      | Anforderungs-URI |
| :------     | :----- |
| POST | /api/filesystem/apps/file |

**URI-Parameter**

| URI-Parameter | Beschreibung |
| :------     | :----- |
| knownfolderid | (**Erforderlich**) Das Verzeichnis auf oberster Ebene, in das Sie Dateien hochladen möchten. Verwenden Sie **LocalAppData** für den Zugriff auf quergeladene Apps. |
| packagefullname | (**Erforderlich, wenn *knownfolderid* == LocalAppData:** ) Der vollständige Name des gewünschten Pakets der App. |
| Pfad | (**Optional**) Das Unterverzeichnis in dem oben angegebenen Ordner oder Paket. |

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

| HTTP-Statuscode      | Beschreibung |
| :------     | :----- |
| 200 | OK |. Die Datei wird hochgeladen |
| 4XX | Fehlercodes |
| 5XX | Fehlercodes |

**Verfügbare Gerätefamilien**

* Windows Mobile
* Windows Desktop
* HoloLens
* Xbox
* IoT
