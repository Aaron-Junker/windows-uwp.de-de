---
title: Übersicht über Karten und Position
description: In diesem Abschnitt wird erläutert, wie Sie in Ihrer App Karten anzeigen, Kartendienste verwenden, die Position suchen und einen Geofence einrichten. Außerdem erfahren Sie in diesem Abschnitt, wie die Windows-Karten-App mit einer bestimmten Karte, Route oder detaillierten Wegbeschreibung gestartet wird.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Karte, Position, Kartendienste
ms.localizationpriority: medium
ms.openlocfilehash: 3482370719a658f303964204661f1fb5d69ae5b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582658"
---
# <a name="maps-and-location-overview"></a>Übersicht über Karten und Position




In diesem Abschnitt wird erläutert, wie Sie in Ihrer App Karten anzeigen, Kartendienste verwenden, die Position suchen und einen Geofence einrichten. Außerdem erfahren Sie in diesem Abschnitt, wie die Windows-Karten-App mit einer bestimmten Karte, Route oder detaillierten Wegbeschreibung gestartet wird.

> [!TIP]
> Um mehr über das Verwenden von Karten und Positionen in Ihrer App zu erfahren, laden Sie die folgenden Beispiele aus dem [Repository „Windows-universal-samples“](https://go.microsoft.com/fwlink/p/?LinkId=619979) (Beispiele für die universelle Windows-Plattform) auf GitHub herunter:
-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [UWP-Geolocation-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=533278)

 

## <a name="display-maps"></a>Anzeigen von Karten


Mit APIs aus dem [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751)-Namespace kann Ihre App Karten mit 2D-, 3D- oder Streetside-Ansichten anzeigen. Sie können interessante Orte (POI) auf der Karte mit Ortsmarken, Bildern, Formen oder XAML-UI-Elementen markieren. Außerdem können Sie nebeneinander angeordnete Bilder überlagern oder die Kartenbilder komplett ersetzen.

| Thema | Beschreibung |
|-------|-------------|
| [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md) | Ihre App muss authentifiziert werden, bevor sie [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) und Kartendienste im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Artikel wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel aus dem [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen. |
| [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md) | Sie können mit der [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse anpassbare Karten in Ihrer App anzeigen. In diesem Thema werden auch 3D-Luftbilder und Streetside-Ansichten behandelt. |
| [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md) | Hinzufügen interessanter Orte (POI) mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen auf einer Karte. |
| [Überlagern von nebeneinander angeordneten Bildern in einer Karte](overlay-tiled-images.md) | Überlagern Sie Bilder von Drittanbietern oder benutzerdefinierte nebeneinander angeordnete Bilder in einer Karte mithilfe von Kachelquellen. Verwenden Sie Kachelquellen, um spezielle Infos wie Wetterdaten, Einwohnerzahlen oder seismische Daten zu überlagern oder die Standardkarte vollständig zu ersetzen. |



## <a name="access-map-services"></a>Zugreifen auf Kartendienste

Fügen Sie Ihrer App mithilfe der APIs aus dem [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace Routen, Wegbeschreibungen und Geocodierungsfunktionen hinzu.

| Thema | Beschreibung |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md) | Ihre App muss authentifiziert werden, bevor sie [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) und Kartendienste im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Artikel wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel aus dem [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen. |
| [Anzeigen von interessanten Orten (POI) auf einer Karte](display-poi.md) | Hinzufügen interessanter Orte (POI) mit Ortsmarken, Bildern, Formen und XAML-UI-Elementen auf einer Karte. |
| [Anzeigen von Routen und Wegbeschreibungen](routes-and-directions.md) | Fordern Sie Routen und Wegbeschreibungen an, und zeigen Sie sie in Ihrer App an. |
| [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md) | Sie konvertieren Adressen in geografische Standorte (Geocodierung) und geografische Standorte in Adressen (umgekehrte Geocodierung), indem Sie die Methoden der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace aufrufen. |
| [Suchen und Herunterladen von Kartenpaketen für die Offlineverwendung](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| In der Vergangenheit musste Ihre App Benutzer an die Einstellungs-App weiterleiten, damit sie Offlinekarten herunterladen konnten. Nun können Sie Klassen im Namespace [Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps) nutzen, um heruntergeladene Pakete in einem bestimmten Bereich (basierend auf einer [Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)- oder [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox)-Klasse) zu finden. <br> Sie können auch den Downloadstatus eines Kartenpakets überprüfen sowie einen Download starten, ohne dass der Benutzer die App verlassen muss. <br> Beispiele dazu finden Sie im Referenzinhalt und im [UWP-Kartenbeispiel](https://go.microsoft.com/fwlink/p/?LinkId=619977).

## <a name="get-the-users-location"></a>Abrufen des Benutzerstandorts

Mit APIs aus dem [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603)-Namespace kann Ihre App die aktuelle Position des Benutzers abrufen und Sie über Positionsänderungen benachrichtigen. Diese API-Member werden auch häufig in Parametern der Karten-APIs verwendet. Mit APIs aus dem [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744)-Namespace wird Ihre App benachrichtigt, wenn der Benutzer einen Geofence (einen vordefinierten geografischen Bereich) betritt oder verlässt.

| Thema | Beschreibung |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md) | Ihre App muss authentifiziert werden, bevor sie [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) und Kartendienste im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Artikel wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel aus dem [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen. |
| [Entwurfsrichtlinien für Apps mit Positionsbestimmung](guidelines-and-checklist-for-detecting-location.md) | Leistungsrichtlinien für Apps, für die der Zugriff auf den Standort eines Benutzers erforderlich ist. |
| [Abrufen der Position eines Benutzers](get-location.md) | Erhalten Sie Zugriff auf die Position eines Benutzers, und rufen Sie diese anschließend ab. | 
| [Richtlinien für die Verwendung der Funktion zur Nachverfolgung besuchter Standorte](guidelines-for-visits.md) | Hier erfahren Sie, wie Sie die leistungsstarke Funktion für die Nachverfolgung besuchter Standorte (Visits Tracking) für eine praktischere Standortnachverfolgung verwenden können. |
| [Entwurfsanleitung für Geofencing](guidelines-for-geofencing.md) | Leistungsrichtlinien für Apps, die das Geofencing-Feature verwenden |
| [Einrichten von Geofence-Bereichen](set-up-a-geofence.md) | Richten Sie einen Geofence-Bereich in Ihrer App ein, und erfahren Sie, wie Sie Benachrichtigungen im Vordergrund und Hintergrund behandeln. |

## <a name="launch-the-windows-maps-app"></a>Starten der Windows-Karten-App

Ihre App kann die Windows-Karten-App starten, wie hier veranschaulicht, um bestimmte Karten und detaillierte Wegbeschreibungen anzuzeigen. Anstatt die Kartenfunktionen direkt in Ihrer eigenen App bereitzustellen, können Sie sie auch über die Windows-Karten-App verfügbar machen. Weitere Informationen finden Sie unter [Starten der Windows-Karten-App](https://msdn.microsoft.com/library/windows/apps/mt228341).

![Ein Beispiel der Windows-Karten-App.](images/mapnyc.png)

## <a name="related-topics"></a>Verwandte Themen

* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [UWP-Geolocation-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=533278)
* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Abrufen der aktuellen Position](get-location.md)
* [Entwurfsrichtlinien für Apps mit Positionsbestimmung](guidelines-and-checklist-for-detecting-location.md)
* [Entwurfsrichtlinien für Karten](controls-map.md)
* [Entwurfsrichtlinien für Apps mit Berücksichtigung von Datenschutz](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
