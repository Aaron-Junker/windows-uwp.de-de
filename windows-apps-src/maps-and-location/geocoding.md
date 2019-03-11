---
title: Durchführen der Geocodierung und umgekehrten Geocodierung
description: Dieses Handbuch wird das Konvertieren von Adressen in geografischen Standorten ("Geocoding") und zum Konvertieren von geografischer Standorten befinden, Anschriften (reverse-Geocoding) durch Aufrufen der Methoden der Klasse MapLocationFinder im Windows.Services.Maps-Namespace veranschaulicht.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP, Geocodierung, Karte, Ort, Standort
ms.localizationpriority: medium
ms.openlocfilehash: a30ca89242b15866019fffc6972bdae7086f3f7e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637625"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Durchführen der Geocodierung und umgekehrten Geocodierung

Dieser Anleitung erfahren Sie, wie Adressen in geografische Standorte ("Geocoding") konvertiert, und geografische Standorte in Straßennamen (reverse-Geocoding) zu konvertieren, durch Aufrufen der Methoden der der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) -Klasse in der [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) Namespace.

> [!TIP]
> Weitere Informationen zum Verwenden von Zuordnungen in Ihre app Herunterladen der [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) Beispiel der [universelle Windows-Repository Beispiele](h https://github.com/Microsoft/Windows-universal-samples) auf GitHub.

Die Klassen, die geocodierung und reverse-Geocoding beteiligt sind wie folgt organisiert.

-   Die [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) -Klasse enthält Methoden, die geocodierung verarbeiten ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) und reverse-geokodierung ([ **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Diese Methoden zurück, eine [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) Instanz.
-   Die [ **Speicherorte** ](https://msdn.microsoft.com/library/windows/apps/dn627552) Eigenschaft der [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) stellt eine Sammlung von [  **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte. 
-   [**Kartenregion**](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte haben beide eine [**Adresse**](https://msdn.microsoft.com/library/windows/apps/dn636929) -Eigenschaft, die macht einer [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) Objekt, das eine Straße und Hausnummer, darstellt und einen [**Punkt**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) -Eigenschaft, die macht einer [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) Objekt der einen geografischen Standort darstellt.

> [!IMPORTANT]
> Sie müssen einen Authentifizierungsschlüssel Zuordnungen angeben, bevor Sie die Zuordnung von Diensten verwenden können. Weitere Informationen finden Sie unter [Anfordern eines Kartenauthentifizierungsschlüssels](authentication-key.md).

## <a name="get-a-location-geocode"></a>Abrufen eines Standorts (Geocode)

In diesem Abschnitt zeigt, wie eine Adresse oder eine Bezeichnung an einen geografischen Standort ("Geocoding") konvertiert wird.

1.  Rufen Sie eine der Überladungen der der [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) Methode der [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) Klasse mit einem direkten oder Straße Adresse.
2.  Die [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) Methode gibt eine [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) Objekt.
3.  Verwenden der [ **Speicherorte** ](https://msdn.microsoft.com/library/windows/apps/dn627552) Eigenschaft der [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) , eine Sammlung verfügbar zu machen [  **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte. Es gibt möglicherweise mehrere [ **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte, da das System möglicherweise auf mehrere Standorte, die mit der angegebenen Eingabe entsprechen.

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

In diesem Abschnitt zeigt, wie Sie einen geografischen Standort in eine Adresse (reverse-Geocoding) zu konvertieren.

1.  Rufen Sie die [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)-Methode der [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)-Klasse auf.
2.  Die [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)-Methode gibt ein [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551)-Objekt zurück, das eine Sammlung übereinstimmender [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549)-Objekte enthält.
3.  Verwenden der [ **Speicherorte** ](https://msdn.microsoft.com/library/windows/apps/dn627552) Eigenschaft der [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) , eine Sammlung verfügbar zu machen [  **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte. Es gibt möglicherweise mehrere [ **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549) Objekte, da das System möglicherweise auf mehrere Standorte, die mit der angegebenen Eingabe entsprechen.
4.  Zugriff [ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533) Objekte über die [ **Adresse** ](https://msdn.microsoft.com/library/windows/apps/dn636929) Eigenschaft der einzelnen [ **Kartenregion** ](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Beispiel für die app von UWP-Datenverkehr](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Entwurfsrichtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Video: Verwenden von Karten und Position auf verschiedenen Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Maps-Entwicklercenter](https://www.bingmapsportal.com/)
* [**MapLocationFinder** class](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync** Methode](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync** Methode](https://msdn.microsoft.com/library/windows/apps/dn636928)
