---
title: 'Xbox-Geräteportal: REST-API'
description: Weitere Informationen finden Sie in der Liste der Referenz Themen für die Xbox-Geräte Portal-Rest-API, die zur Remote Konfiguration und Verwaltung Ihrer Konsole verwendet wird.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: be8558daa39b126061caaa132fb134c8bdef15c1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172774"
---
# <a name="xbox-device-portal-rest-api"></a>Xbox-Geräteportal: REST-API

Dieser Abschnitt enthält Referenz Themen für die Xbox-Geräte Portal-Rest-API, die zur Remote Konfiguration und Verwaltung der-Konsole verwendet wird.

| URI        | BESCHREIBUNG |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| Registriert eine App, die in einem losen Ordner enthalten ist. |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| Lädt einen ganzen Ordner zur Konsole hoch. |
|[/ext/app/sshpins](uwp-sshpins-api.md)| Löschen Sie alle vertrauenswürdigen SSH-Pins Remote. Erfordert eine erneute Pin-Kopplung für die Visual Studio-UWP-Entwicklung. |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| Fordert Bereitstellungs Informationen für mindestens ein installiertes Paket an. |
|[/ext/fiddler](wdp-fiddler-api.md)| Aktivieren und Deaktivieren der Fiddler-Netzwerk Ablauf Verfolgung. |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| HTTP-Datenverkehr von der fokussierten App auf Xbox erhalten. |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| Hinzufügen, entfernen oder Aktualisieren von Netzwerk Anmelde Informationen. |
|[/ext/remoteinput](uwp-remoteinput-api.md)| Senden von Tastatur-, Maus-oder Controller Eingaben per Remote Zugriff an eine Xbox. |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| Holen Sie sich die Anzahl angefügter physischer Controller, oder schalten Sie alle physischen Controller aus. |
|[/ext/screenshot](wdp-media-capture-api.md)| Erfasst eine PNG-Darstellung des Bildschirms, der zurzeit auf der Konsole angezeigt wird. |
|[/ext/settings](wdp-xboxsettings-api.md)| Greift auf Xbox One-Entwicklereinstellungen zu. |
|[/ext/smb/developerfolder](wdp-smb-api.md)| Greift über den Datei-Explorer auf Ihrem Entwicklungscomputer auf den Entwicklerordner auf Ihrer Konsole zu. |
|[/ext/user](wdp-user-management.md)| Verwaltet Benutzer auf der Xbox One-Konsole. |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| Enthält Informationen über das Xbox One-Gerät. |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| Verwaltet Ihren Xbox Live-Sandkasten. |

## <a name="see-also"></a>Weitere Informationen

- [UWP auf Xbox One](index.md)
- [Windows-Geräteportal](../debug-test-perf/device-portal.md)