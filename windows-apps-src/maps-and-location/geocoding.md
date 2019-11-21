---
title: Durchführen der Geocodierung und umgekehrten Geocodierung
description: In dieser Anleitung erfahren Sie, wie Sie die-Methoden der maplocationfinder-Klasse im Windows. Services. Maps-Namespace durch Aufrufen der Methoden der maplocationfinder-Klasse in geografische Standorte (Geocodierung) konvertieren und geografische Orte in Straßenadressen (Reverse-Geocodierung) konvertieren.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, Geocodierung, Karte, Ort, Standort
ms.localizationpriority: medium
ms.openlocfilehash: 5d7e1dda355cf87a2c8e26c11327cfff32e9d0b5
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259367"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Durchführen der Geocodierung und umgekehrten Geocodierung

In dieser Anleitung erfahren Sie, wie Sie die-Methoden der maplocationfinder-Klasse im [**Windows. Services. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) -Namespace durch Aufrufen der Methoden der [**maplocationfinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse in geografische Standorte (Geocodierung) konvertieren und geografische Orte in Straßenadressen (Reverse-Geocodierung) konvertieren.

> [!TIP]
> Wenn Sie weitere Informationen zum Verwenden von Maps in Ihrer APP haben, laden Sie das [mapcontrol](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) -Beispiel aus dem [Windows Universal Samples](h https://github.com/Microsoft/Windows-universal-samples) -Repository auf GitHub herunter.

Die Klassen, die an Geocodierung und umgekehrter Geocodierung beteiligt sind, sind wie folgt organisiert.

-   Die [**maplocationfinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse enthält Methoden, die die Geocodierung ([**findlocationsasync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) und die Reverse-Geocodierung ([**findlocationsatasync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) verarbeiten.
-   Diese Methoden geben beide eine [**maplocationfinderresult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) -Instanz zurück.
-   Die Location [ **-Eigenschaft**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) von [**maplocationfinderresult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) macht eine Auflistung von [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekten verfügbar. 
-   [**Maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügen über eine [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) -Eigenschaft, die ein [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) -Objekt verfügbar macht, das eine Straße darstellt, und eine Point-Eigenschaft, die ein [**geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) -Objekt verfügbar [**macht**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , das einen geografischen Standort darstellt.

> [!IMPORTANT]
> Sie einen Zuordnungs-Authentifizierungsschlüssel angeben müssen, bevor Sie Kartendienste verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## <a name="get-a-location-geocode"></a>Abrufen eines Standorts (Geocode)

In diesem Abschnitt wird gezeigt, wie eine Straße oder ein Orts Name in einen geografischen Standort (Geocodierung) konvertiert wird.

1.  Aufrufen einer der über Ladungen der [**findlocationsasync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) -Methode der [**maplocationfinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse mit einem Namen oder einer Straße.
2.  Die [**findlocationsasync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) -Methode gibt ein [**maplocationfinderresult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) -Objekt zurück.
3.  Verwenden Sie die Location [ **-Eigenschaft von**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) [**maplocationfinderresult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) , um eine Collection [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügbar zu machen. Möglicherweise gibt es mehrere [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekte, da das System möglicherweise mehrere Orte findet, die der angegebenen Eingabe entsprechen.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Dieser Code zeigt die folgenden Ergebnisse im `tbOutputText`-Textfeld an:

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Abrufen einer Adresse (umgekehrte Geocodierung)

In diesem Abschnitt wird gezeigt, wie ein geografischer Standort in eine Adresse (reverse Geocoding) konvertiert wird.

1.  Rufen Sie die [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)-Methode der [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)-Klasse auf.
2.  Die [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)-Methode gibt ein [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult)-Objekt zurück, das eine Sammlung übereinstimmender [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)-Objekte enthält.
3.  Verwenden Sie die Location [ **-Eigenschaft von**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) [**maplocationfinderresult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) , um eine Collection [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügbar zu machen. Möglicherweise gibt es mehrere [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) -Objekte, da das System möglicherweise mehrere Orte findet, die der angegebenen Eingabe entsprechen.
4.  Greifen Sie über die [**Address**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) -Eigenschaft jedes [**maplocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation)auf [**mapaddress**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) -Objekte zu.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Dieser Code zeigt die folgenden Ergebnisse im `tbOutputText`-Textfeld an:

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Verwandte Themen

* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [Entwurfsrichtlinien für Karten](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Video: Nutzen von Karten und Speicherort über Telefon, Tablet und PC in Ihren Windows-apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [**Maplocationfinder** -Klasse](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**Findlocationsasync** -Methode](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**Findlocationsatasync** -Methode](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
