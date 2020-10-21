---
title: Durchführen der Geocodierung und umgekehrten Geocodierung
description: In dieser Anleitung erfahren Sie, wie Sie die-Methoden der maplocationfinder-Klasse im Windows. Services. Maps-Namespace durch Aufrufen der Methoden der maplocationfinder-Klasse in geografische Standorte (Geocodierung) konvertieren und geografische Orte in Straßenadressen (Reverse-Geocodierung) konvertieren.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, Geocodierung, map, Location
ms.localizationpriority: medium
ms.openlocfilehash: 992a9902081f0655885383ef90ea02ed1e79f13a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297709"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Durchführen der Geocodierung und umgekehrten Geocodierung

> [!NOTE]
> [**Mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) -und Map-Dienste erfordern einen Zuordnungs Authentifizierungsschlüssel, der als [**mapservicetoken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)bezeichnet wird. Weitere Informationen zum Abrufen und Festlegen eines Kartenauthentifizierungsschlüssels finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

In dieser Anleitung erfahren Sie, wie Sie die-Methoden der maplocationfinder-Klasse im [**Windows. Services. Maps**](/uwp/api/Windows.Services.Maps) -Namespace durch Aufrufen der Methoden der [**maplocationfinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse in geografische Standorte (Geocodierung) konvertieren und geografische Orte in Straßenadressen (Reverse-Geocodierung) konvertieren.

> [!TIP]
> Wenn Sie weitere Informationen zum Verwenden von Maps in Ihrer APP haben, laden Sie das [mapcontrol](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) -Beispiel aus dem [Windows Universal Samples](hhttps://github.com/Microsoft/Windows-universal-samples) -Repository auf GitHub herunter.

Die Klassen, die an Geocodierung und umgekehrter Geocodierung beteiligt sind, sind wie folgt organisiert.

-   Die [**maplocationfinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse enthält Methoden, die die Geocodierung ([**findlocationsasync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) und die Reverse-Geocodierung ([**findlocationsatasync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) verarbeiten.
-   Diese Methoden geben beide eine [**maplocationfinderresult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) -Instanz zurück.
-   Die Location [**-Eigenschaft**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) von [**maplocationfinderresult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) macht eine Auflistung von [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekten verfügbar. 
-   [**Maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügen über eine [**Address**](/uwp/api/windows.services.maps.maplocation.address) -Eigenschaft, die ein [**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress) -Objekt verfügbar macht, das eine Straße darstellt, und eine Point-Eigenschaft, die ein [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint) -Objekt verfügbar [**macht**](/uwp/api/windows.services.maps.maplocation.point) , das einen geografischen Standort darstellt.

> [!IMPORTANT]
> Sie müssen einen Maps-Authentifizierungsschlüssel angeben, bevor Sie map-Dienste verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## <a name="get-a-location-geocode"></a>Abrufen eines Standorts (Geocode)

In diesem Abschnitt wird gezeigt, wie eine Straße oder ein Orts Name in einen geografischen Standort (Geocodierung) konvertiert wird.

1.  Aufrufen einer der über Ladungen der [**findlocationsasync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) -Methode der [**maplocationfinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) -Klasse mit einem Namen oder einer Straße.
2.  Die [**findlocationsasync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) -Methode gibt ein [**maplocationfinderresult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) -Objekt zurück.
3.  Verwenden Sie die Location [**-Eigenschaft von**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) [**maplocationfinderresult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) , um eine Collection [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügbar zu machen. Möglicherweise gibt es mehrere [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekte, da das System möglicherweise mehrere Orte findet, die der angegebenen Eingabe entsprechen.

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

1.  Rufen Sie die [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)-Methode der [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)-Klasse auf.
2.  Die [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)-Methode gibt ein [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)-Objekt zurück, das eine Sammlung übereinstimmender [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)-Objekte enthält.
3.  Verwenden Sie die Location [**-Eigenschaft von**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) [**maplocationfinderresult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) , um eine Collection [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekte verfügbar zu machen. Möglicherweise gibt es mehrere [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation) -Objekte, da das System möglicherweise mehrere Orte findet, die der angegebenen Eingabe entsprechen.
4.  Greifen Sie über die [**Address**](/uwp/api/windows.services.maps.maplocation.address) -Eigenschaft jedes [**maplocation**](/uwp/api/Windows.Services.Maps.MapLocation)auf [**mapaddress**](/uwp/api/Windows.Services.Maps.MapAddress) -Objekte zu.

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

## <a name="related-topics"></a>Zugehörige Themen

* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [Entwurfsrichtlinien für Karten](./display-maps.md)
* [Video: Nutzen von Karten und Speicherort über Telefon, Tablet und PC in Ihren Windows-apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [**Maplocationfinder** -Klasse](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**Findlocationsasync** -Methode](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**Findlocationsatasync** -Methode](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
