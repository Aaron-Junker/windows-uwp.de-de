---
title: Geräteportal für Xbox – REST-API
description: API-Referenz für UWP auf Xbox One.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: d8fdcf01d7d1f72624d73acf2d10ce28dfb75e04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656995"
---
# <a name="xbox-device-portal-rest-api"></a>Geräteportal für Xbox – REST-API

Dieser Abschnitt enthält Referenzthemen für die REST-API für das Xbox-Geräteportal. Diese wird verwendet, um Ihre Konsole per Fernzugriff zu konfigurieren und zu verwalten.

| URI        | Beschreibung |
|------------|-------------|
|[/API/App/packagemanager/Register](wdp-loose-folder-register-api.md)| Registriert eine App, die in einem losen Ordner enthalten ist. |
|[/API/App/packagemanager/Upload](wdp-folder-upload.md)| Lädt einen ganzen Ordner zur Konsole hoch. |
|[/ext/App/sshpins](uwp-sshpins-api.md)| Löschen Sie alle vertrauenswürdigen SSH-PINs per Fernzugriff. Dies erfordert die erneute PIN-Kopplung für die UWP-Entwicklung in Visual Studio. |
|[/ext/App/deployinfo](uwp-deployinfo-api.md)| Fordert Bereitstellungsinformationen für ein oder mehrere installierte Pakete an. |
|[/ Ext/fiddler](wdp-fiddler-api.md)| Zum Aktivieren und Deaktivieren der Fiddler-Netzwerkablaufverfolgung |
|[/ext/httpmonitor/Sessions](wdp-httpMonitor-api.md)| Abrufen des HTTP-Datenverkehrs aus der fokussierten App auf der Xbox |
|[/ Ext/networkcredential](uwp-networkcredentials-api.md)| Hinzufügen, Entfernen oder Aktualisieren der Netzwerkanmeldeinformationen |
|[/ Ext/remoteinput](uwp-remoteinput-api.md)| Senden von Tastatur-, Maus- oder Controllereingaben auf einer Xbox per Fernzugriff |
|[/ext/remoteinput/Controllers](uwp-remoteinput-controllers-api.md)| Abrufen der Anzahl der angeschlossenen physischen Controller oder Deaktivieren aller physischen Controller |
|[bildschirmabbildung von/Ext /](wdp-media-capture-api.md)| Erfasst eine PNG-Darstellung des Bildschirms, der zurzeit auf der Konsole angezeigt wird. |
|[/ Ext/settings](wdp-xboxsettings-api.md)| Greift auf Xbox One-Entwicklereinstellungen zu. |
|[/ext/SMB/developerfolder](wdp-smb-api.md)| Greift über den Datei-Explorer auf Ihrem Entwicklungscomputer auf den Entwicklerordner auf Ihrer Konsole zu. |
|[/ Ext/Benutzer](wdp-user-management.md)| Verwaltet Benutzer auf der Xbox One-Konsole. |
|[/ext/Xbox/Info](wdp-xboxinfo-api.md)| Bietet Informationen zum Xbox One-Gerät |
|[/ext/xboxlive/Sandbox](wdp-sandbox-api.md)| Verwaltet Ihren Xbox Live-Sandkasten. |

## <a name="see-also"></a>Siehe auch

- [UWP auf Xbox One](index.md)
- [Windows-Geräteportal](../debug-test-perf/device-portal.md)